# Curso de Git

## Comando init

Comando para inicializar un proyecto vació de Git, este crea la carpeta **.git/** donde se encontrara toda la configuración del repositorio.

```bash
git init  
```

## Comando config

es el comando que te permite ver y establecer las opciones de configuración que controlan cómo funciona Git.

### Niveles de Configuración

Git maneja tres niveles de configuración:

- **Sistema (--system)**: Aplica a todos los usuarios del sistema
- **Global (--global):** Aplica a todos tus repositorios como usuario
- **Local (--local):** Aplica solo al repositorio actual (es el predeterminado)

### Configuraciones Esenciales

#### Identidad (obligatorio para hacer commits

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

#### Editor predeterminado

```bash
git config --global core.editor "code --wait"  ## VS Code
git config --global core.editor "vim"          ## Vim
```

### Comandos Útiles

#### Ver toda tu configuración

```bash
git config --list
git config --list --show-origin  ## muestra de dónde viene cada config
```

#### Ver la configuración por entorno

```bash
git config --global -e
git config --local -e
```

#### Ver un valor específico

```bash
git config user.name
git config --global user.email
```

#### Eliminar una configuración

```bash
git config --global --unset user.name
```

### Otras Configuraciones Útiles

#### Alias para comandos frecuentes:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
## Ahora puedes usar: git st, git co, git br
```

#### Colores en la terminal:

```bash
git config --global color.ui auto
```

#### Configurar saltos de línea:

```bash
git config --global core.autocrlf true   ## Windows
git config --global core.autocrlf input  ## Mac/Linux
```

#### Rama predeterminada:

```bash
git config --global init.defaultBranch main

```

## Comando add

git add agrega archivos al staging area (área de preparación), es decir, los marca para incluirlos en el próximo commit.

### Usos Comunes

#### Agregar un archivo específico:

```bash
git add archivo.txt
git add src/index.js
```

#### Agregar todos los archivos modificados:

```bash
git add .              # Agrega todo desde el directorio actual
git add -A             # Agrega todo el repositorio
git add --all          # Lo mismo que -A
```

#### Agregar solo archivos modificados (no nuevos):

```bash
git add -u
```

#### Agregar múltiples archivos:

```bash
git add archivo1.txt archivo2.js carpeta/
```

#### Agregar de forma interactiva (elegir qué cambios incluir):

```bash
git add -p    # Te pregunta por cada cambio
```

## Comando commit

git commit guarda los cambios que tienes en el staging area (los que agregaste con git add) en el historial de tu repositorio.

### Usos Más Comunes

#### Commit con mensaje en una línea:

```bash
git commit -m "Mensaje descriptivo del cambio"
```

#### Commit con mensaje multilínea:

```bash
git commit
# Se abre tu editor para escribir un mensaje más detallado
```

#### Commit saltándose el staging (add + commit):

```bash
git commit -am "Mensaje"
# Solo funciona con archivos ya rastreados, no con archivos nuevos
```

#### Modificar el último commit:

```bash
git commit --amend -m "Nuevo mensaje"
# Útil si olvidaste algo o quieres cambiar el mensaje
```

## Comando status

git status es uno de los comandos más útiles de Git. Te muestra el estado actual de tu repositorio.

### Qué Te Muestra

#### Información que obtienes:

- Archivos modificados
- Archivos nuevos (sin rastrear)
- Archivos en staging (listos para commit)
- Rama actual
- Si estás adelantado o atrasado respecto al remoto

#### Ejemplo de Salida

```bash
git status
```

**Podrías ver algo como:**
```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   archivo1.txt
        new file:   archivo2.js

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes)
        modified:   archivo3.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        nuevo-archivo.html

```

#### Versión Corta

Para una salida más compacta:

```bash
git status -s
# o
git status --short

```

**Salida tipo:**
```
M  archivo1.txt       # Modificado y en staging
M  archivo3.css       # Modificado pero NO en staging
A  archivo2.js        # Agregado (nuevo en staging)
?? nuevo-archivo.html # Sin rastrear

```

#### Estados de los Archivos

- Sin rastrear (Untracked): Archivos nuevos que Git no conoce
- Modificado (Modified): Archivos que cambiaron pero no están en staging
- En staging (Staged): Archivos listos para el próximo commit
- Sin cambios (Unmodified): Archivos que no han cambiado

## Comando checkout

git checkout es un comando muy versátil que se usa principalmente para cambiar entre ramas o restaurar archivos.

### Usos Principales

#### Cambiar de Rama

Cambiar a una rama existente:

```bash
git checkout nombre-rama
git checkout main
git checkout desarrollo
```

Crear y cambiar a una nueva rama:

```bash
git checkout -b nueva-rama
# Equivale a:
# git branch nueva-rama
# git checkout nueva-rama
```

#### Restaurar Archivos

Descartar cambios en un archivo (volver a la última versión commiteada):

```bash
git checkout -- archivo.txt
git checkout archivo.txt
```

Restaurar todos los archivos modificados:

```bash
git checkout -- .
```

Recuperar un archivo de otro commit:

```bash
git checkout abc1234 archivo.txt    # Por hash del commit
git checkout HEAD~2 archivo.txt     # 2 commits atrás
```

#### Ver un Commit Específico (Modo Detached HEAD)

```bash
git checkout abc1234    # Ver el estado del repositorio en ese commit
```

⚠️ Advertencia: Esto te pone en "detached HEAD state". Para volver:

```bash
git checkout main    # O el nombre de tu rama
```

## Comando reset

git reset es un comando poderoso que te permite mover el puntero HEAD y deshacer commits o cambios en el staging area.

### Los Tres Modos de Reset

- **-soft (Suave)** Mueve HEAD pero mantiene los cambios en staging:

```bash
git reset --soft HEAD~1
# Deshace el último commit pero los archivos quedan en staging
# Útil para rehacer un commit
```

- **-mixed (Mixto - predeterminado)** Mueve HEAD y quita del staging, pero mantiene los cambios en archivos:

```bash
git reset HEAD~1
git reset --mixed HEAD~1    # Mismo efecto
# Deshace el commit y quita del staging
# Los cambios siguen en tus archivos
```

- **-hard (Duro)** Mueve HEAD y elimina todos los cambios:

```bash
git reset --hard HEAD~1
# ⚠️ PELIGROSO: Elimina el commit Y los cambios
# No se puede recuperar fácilmente
```

## Comando alias
Los alias de Git te permiten crear comandos cortos para operaciones que usas frecuentemente. Se configuran con git config.

### Crear Alias

#### Alias Simples
```bash
# Sintaxis básica
git config --global alias.nombre-corto 'comando-completo'

# Ejemplos comunes
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```
Ahora puedes usar:
```bash
git st        # En vez de git status
git co main   # En vez de git checkout main
git br        # En vez de git branch
git ci -m "mensaje"  # En vez de git commit -m "mensaje"
```

#### Alias Útiles y Populares
Comandos con Opciones
```bash
# Log bonito de una línea
git config --global alias.lg "log --oneline --graph --decorate --all"

# Status corto
git config --global alias.s "status -s"

# Último commit
git config --global alias.last "log -1 HEAD"

# Diferencias staged
git config --global alias.staged "diff --staged"

# Ver ramas ordenadas por fecha
git config --global alias.branches "branch -a --sort=-committerdate"

# Deshacer último commit (soft)
git config --global alias.undo "reset --soft HEAD~1"

# Listar alias
git config --global alias.aliases "config --get-regexp alias"
```

#### Alias Avanzados
```bash
# Ver quién modificó cada línea
git config --global alias.who "blame -w -C -C -C"

# Log con gráfico colorido
git config --global alias.tree "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Commits de hoy
git config --global alias.today "log --since=midnight --author='Tu Nombre' --oneline"

# Agregar y commitear en un paso
git config --global alias.ac "!git add -A && git commit -m"

# Ver cambios de un archivo
git config --global alias.filelog "log -u"
```

#### Alias con Comandos de Shell
Usa ! para ejecutar comandos de shell:
```bash
# Contar commits
git config --global alias.count "!git log --oneline | wc -l"

# Limpiar ramas locales ya mergeadas
git config --global alias.cleanup "!git branch --merged | grep -v '\\*\\|main\\|master' | xargs -n 1 git branch -d"

# Abrir repositorio en GitHub
git config --global alias.open "!open $(git remote get-url origin)"
```

#### Ver y Gestionar Alias

Ver Todos los Alias
```bash
git config --global --get-regexp alias
# O si creaste el alias "aliases":
git aliases
```

Eliminar un Alias
```bash
git config --global --unset alias.nombre-corto
```

Editar Directamente el Archivo de Config
```bash
git config --global --edit
```

## Comando diff

git diff muestra las diferencias entre archivos, commits, ramas y más. Es esencial para revisar cambios antes de commitear.

### Usos Principales

#### Ver Cambios No Staged

```bash
# Ver todos los cambios en archivos modificados (no en staging)
git diff

# Ver cambios en un archivo específico
git diff archivo.txt
```

#### Ver Cambios en Staging

```bash
# Ver cambios que ya agregaste con git add
git diff --staged
# o
git diff --cached
```

#### Comparar Commits

```bash
# Diferencia entre dos commits
git diff commit1 commit2
git diff abc1234 def5678

# Diferencia con el commit anterior
git diff HEAD~1 HEAD

# Ver cambios de los últimos 3 commits
git diff HEAD~3
```

#### Comparar Ramas

```bash
# Diferencias entre ramas
git diff main feature-branch

# Ver qué hay en una rama que no está en otra
git diff main..feature-branch

# Diferencias desde el ancestro común
git diff main...feature-branch
```

### Opciones Útiles

#### Mostrar Solo Nombres de Archivos
```bash
# Lista de archivos modificados
git diff --name-only

# Con estado (Modificado, Agregado, Eliminado)
git diff --name-status
```

#### Estadísticas de Cambios
```bash
# Resumen numérico de cambios
git diff --stat

# Resumen más corto
git diff --shortstat
```

#### Ignorar Espacios en Blanco
```bash
# Ignorar cambios de espacios
git diff -w
# o
git diff --ignore-all-space

# Ignorar espacios al final de línea
git diff --ignore-space-at-eol
```

#### Colores y Formato
```bash
# Con más contexto (líneas alrededor)
git diff -U10    # 10 líneas de contexto

# Formato de palabras (word diff)
git diff --word-diff

# Color siempre activado
git diff --color-words
```

### Casos de Uso Prácticos

#### Antes de Commitear
```bash
# 1. Ver qué cambió
git diff

# 2. Agregar al staging
git add archivo.txt

# 3. Revisar lo que vas a commitear
git diff --staged

# 4. Hacer commit
git commit -m "Mensaje"
```

#### Revisar un Archivo Específico
```bash
# Cambios no staged en un archivo
git diff ruta/al/archivo.js

# Cambios staged en un archivo
git diff --staged ruta/al/archivo.js

# Cambios en un archivo entre dos commits
git diff abc1234 def5678 archivo.txt
```

#### Comparar con Remoto
```bash
# Ver diferencias con la rama remota
git diff origin/main

# Antes de hacer pull
git fetch
git diff HEAD origin/main
```

#### Ver Cambios de un Commit Específico
```bash
# Ver qué cambió en un commit
git diff commit^!
# o
git show commit
```

#### Interpretando la Salida
```bash
diff --git a/archivo.txt b/archivo.txt
index 1234567..abcdefg 100644
--- a/archivo.txt          # Versión anterior
+++ b/archivo.txt          # Versión nueva
@@ -1,4 +1,5 @@            # Líneas afectadas
 Línea sin cambios
-Línea eliminada           # - = eliminada
+Línea agregada            # + = agregada
+Otra línea nueva
 Línea sin cambios
```

### Herramientas Visuales

#### Configurar Herramienta Externa

```bash
# Configurar VS Code como difftool
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'

# Usar
git difftool
```

#### Otros Difftool Populares

```bash
# Meld
git config --global diff.tool meld
```

#### Usar cualquier difftool configurado

```bash
git difftool archivo.txt
```
#### Alias Útiles para Diff

```bash
# Ver solo archivos cambiados
git config --global alias.changed "diff --name-only"

# Diff con estadísticas
git config --global alias.ds "diff --stat"

# Diff de lo staged
git config --global alias.dc "diff --cached"

# Diff con palabra por palabra
git config --global alias.dw "diff --word-diff"
```

### Ejemplos Completos

#### Revisar cambios antes de push:
```bash
# Ver qué commits vas a subir
git log origin/main..HEAD

# Ver las diferencias con el remoto
git diff origin/main

# Ver estadísticas
git diff --stat origin/main
```

#### Comparar dos ramas:
```bash
# ¿Qué hay en feature que no está en main?
git diff main..feature

# Solo nombres de archivos
git diff --name-only main feature

# Estadísticas
git diff --stat main feature
```

#### Ver cambios en un rango de tiempo:
```bash
# Cambios desde ayer
git diff '@{yesterday}'

# Cambios desde hace 1 semana
git diff '@{1 week ago}'
```

## Comando reflog

`git reflog` es como una **caja negra** o **red de seguridad** de Git. Registra **cada movimiento de HEAD** en tu repositorio local, incluso los que no aparecen en `git log`.

### ¿Qué Registra?

Reflog guarda:

- Commits
- Resets
- Checkouts
- Merges
- Rebases
- Amendos
- ¡Prácticamente TODO lo que hagas!

### Ver el Reflog

```bash
# Ver el historial completo de HEAD
git reflog

# Equivalente a:
git reflog show HEAD
```

**Salida típica:**
```
abc1234 HEAD@{0}: commit: Agrega nueva funcionalidad
def5678 HEAD@{1}: reset: moving to HEAD~1
9abcdef HEAD@{2}: commit: Commit que luego reseteé
1234567 HEAD@{3}: checkout: moving from main to feature
8901234 HEAD@{4}: commit: Actualiza README
```

### Entendiendo la Salida
```
hash    HEAD@{n}: acción: descripción
│       │         │        │
│       │         │        └─ Qué pasó
│       │         └─────────── Tipo de operación
│       └──────────────────── Índice temporal (0 = más reciente)
└──────────────────────────── Hash del commit
```

### Casos de Uso Principales

#### **Recuperar un Commit Perdido**

```bash
# Hiciste un reset --hard por error
git reset --hard HEAD~3
# ¡Oh no! Perdiste 3 commits

# Ver qué tenías antes
git reflog

# Recuperar (usa el hash antes del reset)
git reset --hard HEAD@{1}
# o
git reset --hard abc1234
```

#### **Recuperar una Rama Eliminada**

```bash
# Eliminaste una rama por error
git branch -D feature-importante
# ¡Ups!

# Ver el reflog
git reflog

# Buscar el último commit de esa rama
# Recrear la rama en ese punto
git checkout -b feature-importante abc1234
```

#### **Deshacer un Rebase**

```bash
# Hiciste un rebase que salió mal
git rebase main
# Todo está roto

# Ver el estado antes del rebase
git reflog

# Volver al estado antes del rebase
git reset --hard HEAD@{1}
```

#### **Recuperar Cambios Después de Amend**

```bash
# Hiciste amend y perdiste el commit anterior
git commit --amend

# Ver el commit original
git reflog

# Crear una rama con el commit viejo
git branch commit-viejo HEAD@{1}
```

### Reflog de Ramas Específicas

```bash
# Ver reflog de una rama específica
git reflog show main
git reflog show feature-branch

# Ver reflog con más detalles
git reflog show --all
```

### Opciones Útiles

```bash
# Ver con fechas
git reflog show --date=iso

# Limitar número de entradas
git reflog -10

# Ver con más contexto
git reflog show --pretty=fuller

# Solo de los últimos N días
git reflog --since="2 days ago"
```

### Ejemplo Práctico Completo

**Escenario:** Hiciste varios cambios y te arrepentiste

```bash
# Estado inicial
git log --oneline
# abc1234 Commit 3
# def5678 Commit 2
# 9abcdef Commit 1

# Hiciste reset hard
git reset --hard HEAD~2
# Solo queda: 9abcdef Commit 1

# ¡Oh no! Quiero recuperar todo
git reflog
# abc1234 HEAD@{0}: reset: moving to HEAD~2
# def5678 HEAD@{1}: commit: Commit 2
# 9abcdef HEAD@{2}: commit: Commit 1

# Recuperar
git reset --hard HEAD@{1}
# o
git reset --hard abc1234

# ¡Todo de vuelta!
git log --oneline
# abc1234 Commit 3
# def5678 Commit 2
# 9abcdef Commit 1
```

### Limitaciones Importantes

⚠️ **Reflog es LOCAL**

- Solo existe en tu máquina
- No se comparte con `push`/`pull`
- Si clonas un repo, no verás el reflog anterior

⚠️ **Tiene Expiración**

- Por defecto guarda 90 días
- Los commits inalcanzables se eliminan después de 30 días
- Puedes cambiar esto:

```bash
# Cambiar tiempo de expiración
git config --global gc.reflogExpire 180  # 180 días
git config --global gc.reflogExpireUnreachable 60  # 60 días
```

⚠️ **No Todo Se Guarda**

- Solo cambios commitados
- Archivos sin commitear no están en reflog

### Comandos Relacionados

```bash
# Limpiar reflog manualmente (¡cuidado!)
git reflog expire --expire=now --all
git gc --prune=now

# Ver objetos huérfanos
git fsck --lost-found
```

### Alias Útiles

```bash
# Reflog más legible
git config --global alias.rl "reflog --pretty=format:'%C(yellow)%h%C(reset) - %C(cyan)%gd%C(reset) %C(green)(%ar)%C(reset) %s %C(bold blue)<%an>%C(reset)'"

# Uso:
git rl
```

### Tips de Recuperación

1. **Siempre mira reflog primero** cuando pierdas algo
2. **Actúa rápido** - tienes 30-90 días según config
3. **Busca el hash correcto** - usa `HEAD@{n}` o el hash directo
4. **Crea una rama de respaldo** antes de intentar recuperar:

```bash
   git branch backup-antes-recuperar
```

### Ejemplo Real: "Arruiné Todo"

```bash
# 1. ¡Pánico! Hice algo mal
git reflog

# 2. Identificar el estado bueno (antes de arruinarlo)
# Busca la línea antes de tu error

# 3. Ver qué había en ese punto
git show HEAD@{5}

# 4. Si es lo correcto, recuperar
git reset --hard HEAD@{5}

# 5. Verificar
git log
git status
```

Reflog es tu **salvavidas en Git**. Casi cualquier cosa que hagas mal se puede recuperar si actúas a tiempo.

## Comando mv

`git mv` es el comando de Git para **mover o renombrar archivos** manteniendo el historial. Es equivalente a mover/renombrar y hacer `git add` en un solo paso.

### Uso Básico

#### Renombrar un Archivo

```bash
# Cambiar nombre de archivo
git mv archivo-viejo.txt archivo-nuevo.txt

# Ejemplo real
git mv index.html home.html
git mv script.js main.js
```

#### Mover un Archivo

```bash
# Mover a un directorio
git mv archivo.txt carpeta/

# Mover y renombrar al mismo tiempo
git mv archivo.txt carpeta/nuevo-nombre.txt

# Ejemplo real
git mv README.md docs/README.md
```

#### Mover Múltiples Archivos

```bash
# Mover varios archivos a una carpeta
git mv archivo1.txt archivo2.txt archivo3.txt carpeta/
```

## Comando rm

`git rm` elimina archivos del repositorio de Git **y** del sistema de archivos. Es la forma correcta de eliminar archivos en Git.

### Uso Básico

#### Eliminar un Archivo

```bash
# Eliminar archivo del repositorio y del disco
git rm archivo.txt

# Eliminar múltiples archivos
git rm archivo1.txt archivo2.js archivo3.css
```

#### Eliminar con Patrones

```bash
# Eliminar todos los .log
git rm *.log

# Eliminar todos los archivos en una carpeta
git rm carpeta/*

# Eliminar archivos que coincidan con patrón
git rm test_*.js
```

### Opciones Importantes

#### -cached (Eliminar Solo de Git, No del Disco)

```bash
# Mantener el archivo localmente pero quitarlo de Git
git rm --cached archivo.txt

# Útil para archivos que olvidaste agregar a .gitignore
git rm --cached secretos.env
echo "secretos.env" >> .gitignore
```

**Caso de uso común:**

```bash
# Subiste node_modules por error
git rm -r --cached node_modules/
echo "node_modules/" >> .gitignore
git commit -m "Elimina node_modules del repositorio"
```

## Comando branch

`git branch` es el comando para **crear, listar, renombrar y eliminar ramas** en Git. Las ramas te permiten trabajar en diferentes versiones de tu proyecto simultáneamente.

### Listar Ramas

#### Ver Ramas Locales

```bash
# Listar todas las ramas locales
git branch

# Salida (el * indica la rama actual):
# * main
#   feature-login
#   bugfix-header
```

#### Ver Todas las Ramas (Locales y Remotas)

```bash
# Ver ramas locales y remotas
git branch -a

# Solo ramas remotas
git branch -r
```

#### Con Más Información

```bash
# Ver último commit de cada rama
git branch -v

# Ver ramas y su relación con remotas
git branch -vv

# Ver ramas mergeadas a la actual
git branch --merged

# Ver ramas NO mergeadas a la actual
git branch --no-merged
```

### Crear Ramas

#### Crear Nueva Rama

```bash
# Crear rama (sin cambiar a ella)
git branch nombre-rama

# Ejemplo
git branch feature-nueva
```

#### Crear y Cambiar a la Rama

```bash
# Crear y cambiar en un paso
git checkout -b nombre-rama

# O con el comando moderno
git switch -c nombre-rama

# Ejemplo
git checkout -b feature-login
```

#### Crear Rama desde un Commit Específico

```bash
# Crear rama desde un commit
git branch nombre-rama abc1234

# Crear desde un tag
git branch hotfix v1.2.3

# Crear desde otra rama
git branch nueva-rama main
```

### Cambiar de Rama

```bash
# Método tradicional
git checkout nombre-rama

# Método moderno (Git 2.23+)
git switch nombre-rama

# Ejemplos
git checkout main
git switch feature-login
```

### Renombrar Ramas

#### Renombrar Rama Actual

```bash
# Renombrar la rama en la que estás
git branch -m nuevo-nombre

# Ejemplo
git branch -m feature-auth
```

#### Renombrar Otra Rama

```bash
# Renombrar sin estar en ella
git branch -m nombre-viejo nombre-nuevo

# Ejemplo
git branch -m feature-old feature-new
```

#### Forzar Renombrado (Sobrescribir)

```bash
# Si ya existe una rama con ese nombre
git branch -M nuevo-nombre
```

### Eliminar Ramas

#### Eliminar Rama Local (Seguro)

```bash
# Eliminar solo si está mergeada
git branch -d nombre-rama

# Ejemplo
git branch -d feature-completada
```

#### Eliminar Rama Local (Forzado)

```bash
# Eliminar aunque no esté mergeada (pierdes cambios)
git branch -D nombre-rama

# Ejemplo
git branch -D feature-cancelada
```

#### Eliminar Rama Remota

```bash
# Eliminar rama en el servidor
git push origin --delete nombre-rama

# O sintaxis alternativa
git push origin :nombre-rama

# Ejemplo
git push origin --delete feature-vieja
```

### Opciones Útiles

#### Ordenar Ramas

```bash
# Por fecha de último commit (más recientes primero)
git branch --sort=-committerdate

# Por fecha (más antiguas primero)
git branch --sort=committerdate

# Alfabéticamente
git branch --sort=refname
```

#### Filtrar Ramas

```bash
# Ramas que contienen un commit específico
git branch --contains abc1234

# Ramas que NO contienen un commit
git branch --no-contains abc1234

# Ramas con patrón
git branch --list "feature-*"
```

#### Formateo Personalizado

```bash
# Formato personalizado
git branch --format='%(refname:short) %(committerdate:relative)'

# Con colores
git branch --format='%(color:green)%(refname:short)%(color:reset) - %(contents:subject)'
```