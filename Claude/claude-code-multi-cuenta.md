
Guía para tener 2+ cuentas de Claude Code activas al mismo tiempo y alternar entre ellas sin cerrar sesión.

## 1. Crear carpetas de configuración separadas

```
mkdir ~/.claude-accAl
mkdir ~/.claude-accJen
```

Cada carpeta guarda las credenciales de una cuenta distinta, aisladas entre sí. Cambia los nombres según las cuentas que uses.

## 2. Verificar tu shell

```
echo $SHELL
```

- Si dice `/bin/zsh` → edita `~/.zshrc`
- Si dice `/bin/bash` → edita `~/.bash_profile` o `~/.bashrc`

## 3. Crear el archivo de shell si no existe

```
touch ~/.zshrc
```

## 4. Editar el archivo y agregar los alias

Abrir con nano, TextEdit o VS Code:

```
nano ~/.zshrc
# o
open -e ~/.zshrc
# o
code ~/.zshrc
```

Agregar (una línea por alias, o separadas con `;`):

```
alias claude1='CLAUDE_CONFIG_DIR=~/.claude-accJen claude'
alias claude2='CLAUDE_CONFIG_DIR=~/.claude-accAl claude'
```

Con nano: `Ctrl+O` para guardar, Enter para confirmar, `Ctrl+X` para salir.

## 5. Recargar el shell

```
source ~/.zshrc
```

⚠️ Esto solo aplica a la terminal donde lo corres. Una terminal nueva ya lo carga automático al abrir.

## 6. Iniciar sesión en cada cuenta

Correr el alias correspondiente (primera vez pide login por navegador):

```
claude1   # cuenta Jen
claude2   # cuenta Al
```

## 7. Alternar cuentas

Simplemente correr `claude1` o `claude2` según cuál cuenta quieras usar — en la misma terminal o en pestañas distintas. Cada una mantiene su sesión y límite de uso independiente, sin `/logout`.

## 8. Verificar cuenta activa (opcional)

Dentro de una sesión de Claude Code:

```
/usage
/login
```

## Troubleshooting

**`zsh: command not found: claudeX`**
1. `cat ~/.zshrc | grep claudeX` → confirma que el alias esté guardado
2. `echo $SHELL` → confirma que estás editando el archivo correcto
3. `source ~/.zshrc` → recarga el archivo en la sesión actual
