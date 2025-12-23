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
