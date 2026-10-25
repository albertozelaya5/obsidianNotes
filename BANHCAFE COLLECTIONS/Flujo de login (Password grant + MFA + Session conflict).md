## El flujo en una frase

Es **un login con tres finales posibles**: o entra directo, o te pide el
código MFA, o te dice que ya hay una sesión activa en otro dispositivo.
Los tres caminos, si salen bien, terminan entregando lo mismo:
`accessToken` + `refreshToken`.

---
Entonces el flujo es
1. Inicia sesion => sale bien ok
2. si sale mal => puede tener otra sesion iniciada (opcion1) => se decide si mantener la sesion o cambiarla por la actual => si se cambia se llama al endpoint de session conflict grant, si todo sale bien me envia al output final
3. si sale mal => puede requerir el token (opcion 2) => se abre modal para ponerlo con el mfaTicket y mfaCode => sale bien ok => me devolvera accesstoken y refreshToken (output final)
4. si sale mal => me envia las mismas respuestas que el primer endpoint
5. 
## Las piezas

### 1. Start
Lo que le metés al flujo: `userName`, `password`, `client_id`, `baseUrl`.

### 2. Password grant
El primer intento de login (POST con usuario y contraseña). Pasa una de dos:

- **Éxito** → el backend ya te da los tokens, salta directo al Output.
- **Fail** → no siempre es un error "de verdad"; puede ser que el backend
  necesite un paso extra. Eso lo decide el siguiente nodo.

### 3. grant result (el repartidor)
Mira la respuesta HTTP del paso anterior (`http.status` y `body.error`) y
decide a dónde mandarte:

| Condición                                  | Significado                                          | A dónde va               |
| ------------------------------------------ | --------------------------------------------------- | ------------------------ |
| `status 403` + `error: 'mfa_required'`     | Contraseña correcta, pero falta el segundo factor   | **Mfa grant**            |
| `status 403` + `error: 'active_session_exists'` | El usuario ya tiene sesión abierta en otro lado | **Session conflict grant** |
| Default                                    | Éxito normal, o un error real                        | Sigue derecho            |

### 4. Mfa grant
Segundo intento, ahora mandando:

- `mfaTicket`: ticket temporal que el backend devolvió en el paso 2
  (`body.mfaTicket`). Amarra "este código MFA pertenece a este intento de login".
- `mfaCode`: el código de 6 dígitos que escribe el usuario.

Si el código es correcto → tokens → Output.

### 5. Session conflict grant
El camino de "ya hay un dispositivo usándolo". Manda `conflictTicket`
(`body.conflictTicketId`, otro ticket temporal del paso 2). Llamar a este
endpoint es básicamente decir *"sí, cerrá la otra sesión y dame la mía"*.
Si responde bien → tokens → Output.

### 6. Output
Normaliza la salida: agarra `accessToken` y `refreshToken` del body,
vengan del camino que vengan.

---

## Cómo se ve para tu frontend

```
login(user, pass)
   ├─ 200 OK ............................ guardás tokens, listo
   ├─ 403 mfa_required ................. mostrás input de código →
   │                                     reenviás con mfaTicket + mfaCode
   └─ 403 active_session_exists ........ mostrás modal "ya hay sesión activa,
                                         ¿cerrarla?" → llamás con conflictTicket
```

La gracia del diseño: **el error 403 no es "fallaste", es "necesito un dato
más"**. Cada rama te devuelve un *ticket* que tenés que reenviar en la
segunda llamada.

---

## Detalle para confirmar con backend

En **Mfa grant**, tanto `mfaTicket` como `mfaCode` aparecen mapeados a
`body.mfaTicket`. El `mfaCode` debería venir de lo que **teclea el usuario**,
no del body de la respuesta anterior. Puede ser solo cómo quedó dibujado el
diagrama, pero vale la pena preguntarle.