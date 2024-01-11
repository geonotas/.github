# Notas técnicas
Este documento contiene algunas notas técnicas relacionadas con el desarrollo del sitio [https://geonotas.github.io/](https://geonotas.github.io/).

Todos los componentes del sitio están almacenados en la organización [`geonotas`](https://github.com/geonotas) de [GitHub](https://github.com), incluyendo:

- El repositorio [`geonotas/.github`](https://github.com/geonotas/.github). Contiene el archivo [README.md](https://github.com/geonotas/.github/blob/main/profile/README.md), con información general de la organización, y otros documentos.
- El repositorio [`geonotas/geonotas.github.io`](https://github.com/geonotas/geonotas.github.io). Contiene el código fuente del sitio [https://geonotas.github.io/](https://geonotas.github.io/), el cual se publica mediante [GitHub Pages](https://pages.github.com/). Este sitio se desarrolló con el sistema de publicación técnica y científica [Quarto](https://quarto.org/), como un documento de tipo [libro (*book*)](https://quarto.org/docs/books/).

El desarrollo se realiza en un [ambiente](https://conda.io/projects/conda/en/latest/user-guide/concepts/environments.html) del administrador de paquetes [Conda](https://docs.conda.io), el cual se instaló mediante [Miniconda](https://docs.conda.io/projects/miniconda).

## Contenido
- [Creación de un ambiente Conda](#creaci%C3%B3n-de-un-ambiente-conda)
- [Creación de la organización `geonotas`](#creaci%C3%B3n-de-la-organizaci%C3%B3n-geonotas)
- [Creación del repositorio `geonotas/.github`](#creaci%C3%B3n-del-repositorio-geonotasgithub)
- [Creación del repositorio `geonotas.github.io`](#creaci%C3%B3n-del-repositorio-geonotasgeonotasgithubio)

## Creación de un ambiente Conda
1. Crear un ambiente Conda.
```shell
# Actualización de Conda
conda update -n base -c conda-forge -y conda

# Creación del ambiente
conda create -y -n geonotas
conda activate geonotas
conda config --env --add channels conda-forge
conda config --env --set channel_priority strict

# Instalación de paquetes
conda install -c conda-forge -y mamba
mamba install -c conda-forge -y \
  quarto \
  gdal libgdal-arrow-parquet

# Una vez finalizada la instalación, el ambiente debe desactivarse y activarse nuevamente
conda deactivate
conda activate geonotas
```

**Borrado del ambiente**  
En caso de ser necesario, el ambiente puede borrarse con los siguientes comandos.
```shell
# Borrado del ambiente Conda
conda deactivate
conda env remove -n geonotas
```

**Instalación y desinstalación de Conda/Miniconda**  
En caso de ser necesario instalar Conda, se recomienda utilizar el instalador [Miniconda](https://docs.conda.io/projects/miniconda/).
```shell
# Instalación de Miniconda
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
  -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh

# Inicialización
~/miniconda3/bin/conda init bash

# Hay que salir y entrar de la terminal
exit
```

Se detalla también el procedimiento para la desinstalación de Conda.
```shell
# Desinstalación de Miniconda
conda activate
conda init --reverse --all
rm -rf ~/miniconda3

# Hay que salir y entrar de la terminal
exit
```

## Creación de la organización `geonotas`
1. Crear la organización GitHub `geonotas` (`https://github.com/geonotas`) mediante el comando **Create new organization** de la interfaz web de GitHub.

2. Crear el directorio correspondiente en la computadora local.
```shell
# Creación de un directorio para la organización geonotas
mkdir geonotas
cd geonotas
```

## Creación del repositorio `geonotas/.github`
1. Crear el repositorio Git remoto `geonotas/.github` (`https://github.com/geonotas/.github`) mediante el comando **Create new repository** de la interfaz web de GitHub.

2. Crear el directorio del repositorio local.
```shell
# Creación de un directorio para el repositorio .github
mkdir -p .github/profile
cd .github
```

3. Crear el archivo `profile/README.md`.
```shell
# Creación de profile/README.md
echo "# GeoNotas"                                         > profile/README.md
echo "Ejemplos de procesamiento de datos geoespaciales." >> profile/README.md
```

4. Inicializar el repositorio local y sincronizarlo con el repositorio remoto.
```shell
# Inicialización del repositorio local y sincronización con el repositorio remoto
git init
git add .
git commit -m "Commit inicial"
git branch -M main
git remote add origin git@github.com:geonotas/.github
GIT_SSH_COMMAND='ssh -i ~/.ssh/mfvargas' git push -u origin main
```

## Creación del repositorio `geonotas/geonotas.github.io`
1. Crear el repositorio Git `geonotas/geonotas.github.io` (`https://github.com/geonotas/geonotas.github.io`) mediante el comando **Create new repository** de la interfaz web de GitHub.

2. Crear el directorio del repositorio local mediante el sistema de publicación técnica y científica [Quarto](https://quarto.org/), como un documento de tipo [libro (*book*)](https://quarto.org/docs/books/). El ambiente Conda debe estar activado (con `conda activate geonotas`).
```shell
# Creación del sitio web de geonotas
quarto create-project geonotas.github.io \
  --title GeoNotas \
  --type book

cd geonotas.github.io
```

3. Borrar y renombrar algunos de los archivos generados automáticamente por Quarto.
```shell
# Borrado de archivos innecesarios
rm intro.qmd
rm summary.qmd
rm references.bib
rm references.qmd
```

4. Generar contenido inicial para otros archivos.
```shell
# index.qmd
echo "# Introducción {.unnumbered}" > index.qmd
echo "Este libro presenta ejemplos de procesamiento de datos geoespaciales " \
     "mediante lenguajes de programación y bibliotecas de software." \
     >> index.qmd

# _quarto.yml
echo "project:"                     > _quarto.yml
echo "  type: book"                >> _quarto.yml
echo "  output-dir: docs"          >> _quarto.yml
echo ""                            >> _quarto.yml
echo "book:"                       >> _quarto.yml
echo "  title: GeoNotas"           >> _quarto.yml
echo "  subtitle: Ejemplos de procesamiento de datos espaciales"  >> _quarto.yml
echo "  reader-mode: true"         >> _quarto.yml
echo "  cover-image: cover.png"    >> _quarto.yml
echo "  favicon: cover.png"        >> _quarto.yml
echo "  site-url: https://geonotas.github.io"                     >> _quarto.yml
echo "  repo-url: https://github.com/geonotas/geonotas.github.io" >> _quarto.yml
echo ""                            >> _quarto.yml
echo "  chapters:"                 >> _quarto.yml
echo "    - index.qmd"             >> _quarto.yml
echo ""                            >> _quarto.yml
echo "format:"                     >> _quarto.yml
echo "  html:"                     >> _quarto.yml
echo "    lang: es"                >> _quarto.yml
echo "    theme: cosmo"            >> _quarto.yml
echo "    code-link: true"         >> _quarto.yml

# .nojekyll (evita procesamiento adicional de Jekyll en GH Pages)
touch .nojekyll

# .gitignore
echo "/.quarto/"  > .gitignore
```

5. Generar vista previa del documento Quarto.
```shell
# Vista previa del sitio
quarto preview
```

6. Generar el documento Quarto.
```shell
# Generación del sitio
quarto render
```

7. Inicializar el repositorio local y sincronizarlo con el repositorio remoto.
```shell
# Inicialización del repositorio local y sincronización con el repositorio remoto
git init
git add .
git commit -m "Commit inicial"
git branch -M main
git remote add origin git@github.com:geonotas/geonotas.github.io
GIT_SSH_COMMAND='ssh -i ~/.ssh/mfvargas' git push -u origin main
```

8. Para publicar el sitio web en GitHub Pages, en la sección **Settings - Pages** de la interfaz web de `geonotas/geonotas.github.io`, debe especificarse `main` como la rama (*branch*) de origen y `/docs` como el directorio del sitio.