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