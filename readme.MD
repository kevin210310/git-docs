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