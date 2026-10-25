```ts

// Banhcafe OAuth 2.0 Client — React Native / Expo
// Compatible with Banhcafe.Microservices.ExternalSecurity OpenIddict server.
//
// Usage:
//   const client = new OAuthClient({ baseUrl: "https://..." });
//   const session = await client.login({ username, password, deviceId });

const GRANT_TYPES = {
  PASSWORD: "password",
  MFA: "urn:banhcafe:mfa",
  SESSION: "urn:banhcafe:session",
  SESSION_CONFLICT_RESOLUTION: "urn:banhcafe:resolve-session-conflict",
  REFRESH: "urn:banhcafe:refresh",
} as const;

// --- Types ---

export interface OAuthConfig {
  baseUrl: string;
  clientId?: string;
  storage?: TokenStorage;
}

export interface TokenStorage {
  getAccessToken(): Promise<string | null>;
  getRefreshToken(): Promise<string | null>;
  setTokens(accessToken: string, refreshToken: string): Promise<void>;
  clearTokens(): Promise<void>;
}

export interface TokenResponse {
  access_token: string;
  token_type: string;
  expires_in: number;
  refresh_token?: string;
  restricted?: boolean;
  required_action?: string | null;
  device_id?: string;
  mfa_ticket?: string;
  target?: string;
  recipient?: string;
  action?: string;
  conflict_ticket?: string;
  existing_device_id?: string;
  existing_session_created_at?: string;
  error?: string;
  error_description?: string;
}

export interface LoginParams {
  username: string;
  password: string;
  deviceId?: string;
}

export interface BiometricParams {
  username: string;
  password: string;
  deviceId: string;
}

export interface MfaParams {
  mfaTicket: string;
  mfaCode: string;
  deviceId?: string;
}

export interface ResolveConflictParams {
  conflictTicket: string;
}

export interface OAuthSession {
  accessToken: string;
  refreshToken: string | null;
  restricted: boolean;
  requiredAction: string | null;
  deviceId?: string;

  // Intermediate step holders — null unless the flow is blocked
  mfaTicket?: string;
  mfaTarget?: string;
  mfaRecipient?: string;
  conflictTicket?: string;
  existingDeviceId?: string;
  existingSessionCreatedAt?: string;
}

// --- Default in-memory storage ---

function defaultStorage(): TokenStorage {
  let access: string | null = null;
  let refresh: string | null = null;
  return {
    async getAccessToken() {
      return access;
    },
    async getRefreshToken() {
      return refresh;
    },
    async setTokens(a: string, r: string) {
      access = a;
      refresh = r;
    },
    async clearTokens() {
      access = null;
      refresh = null;
    },
  };
}

// --- Client ---

export class OAuthError extends Error {
  constructor(
    public readonly code: string,
    message: string,
    public readonly details?: Record<string, unknown>,
  ) {
    super(message);
    this.name = "OAuthError";
  }
}

export class OAuthClient {
  private readonly baseUrl: string;
  private readonly clientId: string;
  private readonly storage: TokenStorage;

  constructor(config: OAuthConfig) {
    this.baseUrl = config.baseUrl.replace(/\/+$/, "");
    this.clientId = config.clientId ?? "web-cobranzas";
    this.storage = config.storage ?? defaultStorage();
  }

  // --------------------------------------------------
  // High-level convenience flows
  // --------------------------------------------------

  /** Password grant with automatic forward-chaining through MFA and
   *  session-conflict resolution. Returns a resolved session or throws
   *  when the chain hits an error the caller must handle (e.g. wrong code). */
  async login(params: LoginParams): Promise<OAuthSession> {
    let session = await this.authenticate(params);

    while (session.mfaTicket) {
      // The caller must provide the code — we cannot guess it
      throw new OAuthError(
        "mfa_required",
        "MFA code is required. Call resolveMfa() with the code the user received.",
        { mfaTicket: session.mfaTicket, target: session.mfaTarget, recipient: session.mfaRecipient },
      );
    }

    if (session.conflictTicket) {
      session = await this.resolveConflict({ conflictTicket: session.conflictTicket });
    }

    return session;
  }

  /** Biometric grant — convenience wrapper. */
  async loginBiometric(params: BiometricParams): Promise<OAuthSession> {
    return this.tokenExchange({
      grantType: GRANT_TYPES.BIOMETRIC,
      username: params.username,
      password: params.password,
      deviceId: params.deviceId,
    });
  }

  /** Exchange an SSO cookie for tokens. The cookie is sent automatically
   *  by the browser in a WebView — this method assumes the cookie is
   *  already present in the HTTP stack. */
  async exchangeSession(): Promise<OAuthSession> {
    return this.tokenExchange({ grantType: GRANT_TYPES.SESSION });
  }

  // --------------------------------------------------
  // Step-by-step grants
  // --------------------------------------------------

  /** Step 1 — password grant.
   *  Returns a session that may contain mfa_ticket or conflict_ticket
   *  if the flow is blocked. */
  async authenticate(params: LoginParams): Promise<OAuthSession> {
    return this.tokenExchange({
      grantType: GRANT_TYPES.PASSWORD,
      username: params.username,
      password: params.password,
      deviceId: params.deviceId, // ESTO NO
    });
  }

  /** Step 2a — resolve an MFA challenge.
   *  Call this after receiving an mfa_ticket from authenticate(). */
  async resolveMfa(params: MfaParams): Promise<OAuthSession> {
    return this.tokenExchange({
      grantType: GRANT_TYPES.MFA,
      mfaTicket: params.mfaTicket,
      mfaCode: params.mfaCode,
      deviceId: params.deviceId,
    });
  }

  /** Step 2b — resolve a session conflict.
   *  Call this after receiving a conflict_ticket from authenticate()
   *  or resolveMfa(). */
  async resolveConflict(params: ResolveConflictParams): Promise<OAuthSession> {
    return this.tokenExchange({
      grantType: GRANT_TYPES.SESSION_CONFLICT_RESOLUTION,
      conflictTicket: params.conflictTicket,
    });
  }

  /** Rotate the stored refresh token and return a fresh session. */
  async refresh(): Promise<OAuthSession> {
    const refreshToken = await this.storage.getRefreshToken();
    if (!refreshToken) throw new OAuthError("no_refresh_token", "No refresh token available.");

    return this.tokenExchange({ grantType: GRANT_TYPES.REFRESH, refreshToken });
  }

  /** Revoke the current session (logout). */
  async logout(): Promise<void> {
    const accessToken = await this.storage.getAccessToken();
    if (accessToken) {
      try {
        await fetch(`${this.baseUrl}/admin/connect/logout`, {
          method: "POST",
          headers: {
            Authorization: `Bearer ${accessToken}`,
            "Content-Type": "application/json",
          },
        });
      } catch {
        // Swallow — we clear local tokens regardless
      }
    }
    await this.storage.clearTokens();
  }

  /** Return the stored access token, refreshing it automatically if
   *  a refresh token is available and the current token is missing. */
  async getValidAccessToken(): Promise<string | null> {
    const access = await this.storage.getAccessToken();
    if (access) return access;

    const refresh = await this.storage.getRefreshToken();
    if (!refresh) return null;

    const session = await this.refresh();
    return session.accessToken;
  }

  /** Build an Authorization header value. */
  async authHeader(): Promise<string> {
    const token = await this.getValidAccessToken();
    return token ? `Bearer ${token}` : "";
  }

  // --------------------------------------------------
  // Internal — raw token exchange
  // --------------------------------------------------

  private async tokenExchange(params: TokenExchangeParams): Promise<OAuthSession> {
    const body = new URLSearchParams();
    body.append("grant_type", params.grantType);
    body.append("client_id", this.clientId);

    if (params.username) body.append("username", params.username);
    if (params.password) body.append("password", params.password);
    if (params.deviceId) body.append("device_id", params.deviceId);
    if (params.mfaTicket) body.append("mfa_ticket", params.mfaTicket);
    if (params.mfaCode) body.append("mfa_code", params.mfaCode);
    if (params.conflictTicket) body.append("conflict_ticket", params.conflictTicket);
    if (params.refreshToken) body.append("refresh_token", params.refreshToken);

    const res = await fetch(`${this.baseUrl}/admin/connect/token`, {
      method: "POST",
      headers: { "Content-Type": "application/x-www-form-urlencoded" },
      body: body.toString(),
    });

    const json: TokenResponse = await res.json();

    if (json.error) {
      throw new OAuthError(json.error, json.error_description ?? json.error, json as never);
    }

    const session: OAuthSession = {
      accessToken: json.access_token,
      refreshToken: json.refresh_token ?? null,
      restricted: json.restricted ?? false,
      requiredAction: json.required_action ?? null,
      deviceId: json.device_id,
      mfaTicket: json.mfa_ticket,
      mfaTarget: json.target,
      mfaRecipient: json.recipient,
      conflictTicket: json.conflict_ticket,
      existingDeviceId: json.existing_device_id,
      existingSessionCreatedAt: json.existing_session_created_at,
    };

    // Store tokens only when the session is fully resolved
    if (session.accessToken && !session.mfaTicket && !session.conflictTicket) {
      await this.storage.setTokens(session.accessToken, session.refreshToken ?? "");
    }

    return session;
  }
}

// --------------------------------------------------
// Internal types
// --------------------------------------------------

interface TokenExchangeParams {
  grantType: string;
  username?: string;
  password?: string;
  deviceId?: string;
  mfaTicket?: string;
  mfaCode?: string;
  conflictTicket?: string;
  refreshToken?: string;
} 
 
```