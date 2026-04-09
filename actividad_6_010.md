
# 🔍 Actividad 6.10 – `git log` y `git reflog`: leer y recuperar historia

> Has aprendido a crear, modificar y deshacer commits.
> En esta actividad vas a **aprender a inspeccionar el historial con `git log`** y a usar **`git reflog` como red de seguridad para recuperar estados “perdidos”**.

---

## 🎯 Objetivos de la actividad

Al finalizar esta actividad serás capaz de:

- Usar `git log` con distintas opciones (`--oneline`, `--graph`, `--decorate`, filtros).
- Crear un alias simple para ver un log “bonito”.
- Explicar qué es `git reflog` y cuándo usarlo.
- Recuperar un commit o rama “perdida” usando `git reflog`.

---

## 🧠 Conceptos clave: `log` vs `reflog`

- `git log` muestra el **historial de commits alcanzables** desde las referencias actuales (ramas, etiquetas).
- `git reflog` muestra un registro de todos los **movimientos de HEAD y referencias** (cambios de rama, resets, merges, etc.), incluso hacia commits que ya no están en ninguna rama visible.

Por eso:

- `git log` se usa para **navegar el historial normal**.
- `git reflog` se usa para **rescatar commits “perdidos”** (por ejemplo, tras un reset duro o borrar una rama por error).

---

## 🧩 Parte 1 – Preparar un repositorio con varias ramas y merges

### Reto 1.1 – Crear un historial con algo de “ramificación”

1. Crea una carpeta para esta actividad:

   ```bash
   mkdir actividad-6-10-log-reflog
   cd actividad-6-10-log-reflog
   ```

2. Inicializa el repositorio:

   ```bash
   git init
   ```

3. Crea `historia.md`:

   ```markdown
   # Historia del proyecto

   Commit inicial.
   ```

4. Añade y haz commit:

   ```bash
   git add historia.md
   git commit -m "Commit inicial"
   ```

5. Crea una rama `feature-a`:

   ```bash
   git checkout -b feature-a
   echo "Funcionalidad A: en progreso." >> historia.md
   git add historia.md
   git commit -m "Añade funcionalidad A"
   ```

6. Vuelve a `main` y crea otra rama `feature-b`:

   ```bash
   git checkout main
   git checkout -b feature-b
   echo "Funcionalidad B: en progreso." >> historia.md
   git add historia.md
   git commit -m "Añade funcionalidad B"
   ```

7. Vuelve a `main`, añade un cambio propio y haz merge de `feature-a`:

   ```bash
   git checkout main
   echo "Cambio en main antes del merge." >> historia.md
   git add historia.md
   git commit -m "Actualiza main antes de integrar A"
   git merge feature-a
   ```

Ahora tienes varios commits y ramas, suficiente para ver `git log --graph`.

### Checkpoint 1 – Evidencia

- Crea `cp1-log-basico.txt` con la salida de:

  ```bash
  git log --oneline --graph --decorate --all
  ```

  (o una variación similar).

---

## 🧩 Parte 2 – Explorar el historial con `git log`

### Reto 2.1 – Diferentes vistas de `git log`

1. Ejecuta y observa:

   ```bash
   git log
   git log --oneline
   git log --oneline --graph
   git log --oneline --graph --decorate --all
   ```

   - `--oneline` condensa cada commit en una línea.
   - `--graph` dibuja la estructura de ramas.
   - `--decorate` muestra nombres de ramas/etiquetas.
   - `--all` incluye todas las ramas.

2. Filtra commits por autor (sustituye por tu nombre si hace falta):

   ```bash
   git log --oneline --author="tu_nombre"
   ```

3. Filtra por texto en el mensaje:

   ```bash
   git log --oneline --grep="Funcionalidad"
   ```

### Reto 2.2 – Crear un alias de log “bonito”

1. Configura un alias global, por ejemplo:

   ```bash
   git config --global alias.lg "log --oneline --graph --decorate --all"
   ```

   (Puedes ajustar el formato si lo deseas).

2. Ejecuta:

   ```bash
   git lg
   ```

### Checkpoint 2 – Evidencia

- Crea `cp2-log-y-alias.txt` con:
  - Algunas salidas de `git log` que hayas probado (`--oneline`, `--graph`, filtros).
  - El contenido de tu alias (`git config --global alias.lg`).
  - Una frase explicando cuál de las vistas de log te resulta más útil y por qué.

---

## 🧩 Parte 3 – Introducir `git reflog`: tu “caja negra”

### Reto 3.1 – Ver el reflog

1. Ejecuta:

   ```bash
   git reflog
   ```

   Verás una lista de entradas como `HEAD@{0}`, `HEAD@{1}`, etc., con acciones recientes (checkout, commit, merge).

2. Observa qué acciones aparecen (commits, cambios de rama, merges).

### Checkpoint 3 – Evidencia

- Crea `cp3-reflog-inicial.txt` con:
  - Algunas líneas representativas de `git reflog`.
  - Una explicación breve de qué diferencia hay entre `git log` y `git reflog`.

---

## 🧩 Parte 4 – “Perder” una rama y recuperarla con `reflog`

### Reto 4.1 – Borrar una rama y simular un “susto”

1. Asegúrate de que `feature-b` tiene al menos un commit exclusivo (ya lo tiene).

2. Desde `main`, borra la rama `feature-b`:

   ```bash
   git branch -D feature-b
   ```

   (la `-D` fuerza el borrado).

3. Ejecuta:

   ```bash
   git branch
   ```

   Verás que `feature-b` ya no aparece.

### Reto 4.2 – Recuperar el trabajo de `feature-b` con `git reflog`

Aunque la rama se ha borrado, los commits siguen en la base de datos de Git hasta que se limpien, y puedes localizarlos con `reflog`.

1. Ejecuta:

   ```bash
   git reflog
   ```

2. Busca una entrada de reflog que corresponda a cuando estabas en `feature-b` (o al commit que quieres recuperar).

   - Puedes anotar el hash del commit o usar la notación `HEAD@{n}` si estaba asociado a HEAD.

3. Crea una nueva rama apuntando a ese commit:

   ```bash
   git branch feature-b-recuperada <hash_del_commit>
   ```

   o, si usas la notación de reflog:

   ```bash
   git branch feature-b-recuperada HEAD@{n}
   ```



4. Comprueba:

   ```bash
   git branch
   git log --oneline feature-b-recuperada
   ```

### Checkpoint 4 – Evidencia

- Crea `cp4-recuperar-rama.txt` con:
  - El fragmento de `git reflog` que usaste para localizar el commit.
  - Los comandos para recrear la rama (`git branch feature-b-recuperada ...`).
  - Una explicación de por qué `reflog` es útil cuando borras ramas por error.

---

## 🧩 Parte 5 – Recuperar un commit tras un reset duro

### Reto 5.1 – Hacer un `reset --hard` y volver atrás con `reflog`

> ⚠️ Haz esto solo en este repositorio de práctica.

1. Asegúrate de estar en `main` y de que tienes varios commits.

2. Mira el historial:

   ```bash
   git log --oneline
   ```

3. Elige un commit más antiguo (no el último) y haz un reset duro hacia él, por ejemplo:

   ```bash
   git reset --hard <hash_antiguo>
   ```

   Ahora los commits posteriores **desaparecen de `git log`** en `main`.

4. Comprueba:

   ```bash
   git log --oneline
   ```

5. Usa `git reflog` para encontrar la referencia al estado anterior (donde HEAD estaba en el último commit antes del reset).

6. Cuando veas la entrada deseada (por hash o `HEAD@{n}`), restaura la rama a ese punto:

   ```bash
   git reset --hard <hash_a_recuperar>
   ```

   o:

   ```bash
   git reset --hard HEAD@{n}
   ```

7. Comprueba de nuevo:

   ```bash
   git log --oneline
   ```

   Verás que los commits “perdidos” han vuelto.

### Checkpoint 5 – Evidencia

- Crea `cp5-reflog-reset.txt` con:
  - El comando de `git reset --hard` que usaste para “perder” commits.
  - El fragmento de `git reflog` que te permitió encontrar el estado anterior.
  - El comando de `git reset --hard ...` que utilizaste para recuperarlo.

---

## 🧩 Parte 6 – Reflexión final: buenas prácticas con `log` y `reflog`

Crea `reflexion-6-10.md` y responde:

1. Explica con tus palabras la diferencia entre `git log` y `git reflog` y qué tipo de información ofrece cada uno.
2. Describe una situación real en la que `git reflog` podría “salvarte la vida” (por ejemplo, tras un `reset --hard` o borrar una rama por error).
3. ¿Por qué, a pesar de existir `git reflog`, sigue siendo importante tener cuidado con comandos destructivos como `reset --hard`?

---

## 📦 Entrega esperada en el repositorio

Tu repositorio de la Actividad 6.10 debe incluir:

- `historia.md` (versión final).
- `cp1-log-basico.txt`
- `cp2-log-y-alias.txt`
- `cp3-reflog-inicial.txt`
- `cp4-recuperar-rama.txt`
- `cp5-reflog-reset.txt`
- `reflexion-6-10.md`

---

## 🏁 Evaluación sugerida

- 4 pts – Uso correcto de `git log` con opciones y alias, y de `git reflog` para localizar estados previos.
- 3 pts – Recuperación exitosa de rama y commits “perdidos” usando reflog.
- 3 pts – Calidad de la reflexión en `reflexion-6-10.md` (comprensión del papel de `log` y `reflog` en la seguridad del trabajo).

**Puntuación máxima: 10 puntos.**

---