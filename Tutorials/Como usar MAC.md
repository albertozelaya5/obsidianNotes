- Command es windows
- commmand k para entrar a una carpeta compartida
- command m para minimizar
- `shift command 4` => tomar captura de pantalla
- `shift command 3`=> tomar captura pantalla completa
- `shift command 5` => grabar pantalla
- `ctrl command space` => tecla de emojis
- `command +` => aumentar zoom de una pagina
- `command k` => abrir carpeta compartida en finder

## Homebrew
usamos esta herramienta para instalar, actualizar y borrar aplicaciones mediante la terminal, como en linux

Para ver los archivos ocultos en el finder, le damos `Cmd + Shift + . (punto)`

## Atajos utiles en MAC
Aquí tienes los comandos más útiles y eficientes para Mac enfocados en desarrollo y sistema, organizados por categorías:

### 🌟 Atajos Esenciales en Finder

- `Cmd + Shift + . `→ Muestra/oculta archivos ocultos (como los `.env`).
    
- `Cmd + Shift + G `→ Abre la barra para ir directamente a cualquier ruta o carpeta compartida.
    
### 📂 Gestión de Archivos y Carpetas (Terminal)

- `ls -la` → Lista todos los archivos, incluyendo ocultos, con permisos y tamaños.
    
- `pwd` → Muestra la ruta exacta de la carpeta donde estás parado.
    
- `cp -R carpeta1 carpeta2` → Copia una carpeta completa (con todo su contenido).
    
- `rm -rf nombre` → Borra un archivo o carpeta de forma permanente y forzada. _(Usar con cuidado)_.
    

### 🌐 Red y Conectividad

- `ipconfig getifaddr en0` → Muestra tu IP local (Wi-Fi).
    
- `curl ifconfig.me` → Muestra tu IP pública de internet.
    
- `ping 8.8.8.8` → Verifica si tienes salida a internet (ping a Google).
    
- `sudo lsof -i :3000` → Encuentra qué proceso tiene trabado el puerto `3000` (muy común en frontend).
    
- kill -9 â†’ Mata a la fuerza el proceso que trababa el puerto (usas el ID que te dio el comando anterior).
    

### 💻 Sistema y Rendimiento

- `top -o cpu` → Muestra los procesos que más CPU están consumiendo en tiempo real.
    
- `killall Dock` → Reinicia la interfaz del Dock y Launchpad (arregla apps que no aparecen).
    
- `killall Finder` → Reinicia el Finder si se queda congelado.
    
- `pmset -g assertions` → Muestra qué app o proceso está impidiendo que tu Mac entre en suspensión.
    

### 🍺 Mantenimiento con Homebrew

- `brew update` → Actualiza la lista de paquetes de Homebrew.
    
- `brew upgrade` → Actualiza todas tus herramientas y aplicaciones instaladas a su última versión.
    
- `brew cleanup` → Borra instaladores viejos y caché, liberando espacio en disco.

## Aerospace
