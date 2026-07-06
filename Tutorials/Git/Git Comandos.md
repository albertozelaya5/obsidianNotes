
- **`git fetch`**: Descarga los cambios nuevos desde el servidor (Azure, GitHub, etc.) a tu base de datos local, pero **no toca tu código** todavía.
    
- **`git merge`**: Toma los cambios que ya están descargados en tu máquina y los **fusiona** con la rama en la que estás parado actualmente.

-  `git merge origin/master` => Trae los cambios remotos y los fusiona en el historial mediante un commit union
		Luego se pone `shift o`, un mensaje y luego `esc` y `:x`

- git rebase master => Es para mover el historial de esa rama master atrás, y mover los cambios de mi rama local hasta después de los cambios de master
  
- `git revert HEAD` => Hace exactamente lo opuesto al commit que se quiere borrar