1. Hacer documento de word Aurora, y documento de Excel para permisos (Ver carpeta ejemplo docs Aurora que mande en teams el 20 de julio a las 8:57am)
2. Hacer pr de rama principal => pre production
3. Hacer pr de pre production => master
4. Hacer `npm run build`, desde la rama master (no es necesario cambiar las `.env`, ya que existe `.env.production`)
5. mover contenido de la carpeta `dist` a `/Volumes/Aplicaciones BHC/00 DESARROLLO/Aurora` (para ello poner command k y conectarse a `smb://172.19.20.42`)
6. copiar contenido de `Production`, y moverlo a `backup`
7. Eliminar contenido de la carpeta `Production`
8. pegar contenido de la carpeta `dist` a `Production`
9. Probar con Khaterine y luego con Ana Maribel, Lenin o David

