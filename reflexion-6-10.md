1. 
git log muestra el historial de commits en una rama, mostrando detalles como autor, fecha y mensaje. git reflog registra todas las acciones, incluyendo resets y checkouts, con timestamps de cambios locales.

2.
Tras un git reset --hard accidental que borra commits, git reflog permite recuperar el estado anterior revisando el historial de HEAD y recreando la rama perdida desde un punto específico.

3.
Aunque git reflog ayuda a recuperar estados, sus entradas caducan después de 90 días y no protegen contra pérdida de datos en el repositorio local.