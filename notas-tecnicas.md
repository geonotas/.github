# Notas técnicas

## Creación y configuración de la organización

### Creación de la organización `geonotas`
En la interfaz web de [GitHub](https://github.com/), debe crearse la organización `geonotas` (`https://github.com/geonotas`).

También debe crearse el directorio correspondiente en la computadora local.

```shell
# Creación de un directorio para la organización geonotas
mkdir geonotas
cd geonotas
```

### Creación del repositorio `.github`
En la organización `geonotas`, debe crearse el repositorio `.github` (`https://github.com/geonotas/.github`), para contener el archivo `profile/README.md` y otros documentos.

También debe crearse el directorio del repositorio local.

```shell
# Creación de un directorio para el repositorio .github
mkdir -p .github/profile
cd .github
```

Luego, se crea el archivo `profile/README.md`.

```shell
# Creación de profile/README.md
echo "# GeoNotas" > profile/README.md
```

Posteriormente, se inicializa el repositorio local y se sincroniza con el repositorio remoto.

```shell
# Inicialización del repositorio local y sincronización con el repositorio remoto
git init
git add .
git commit -m "Commit inicial"
git branch -M main
git remote add origin git@github.com:geonotas/.github
GIT_SSH_COMMAND='ssh -i ~/.ssh/mfvargas' git push -u origin main
```

## Creación de un ambiente Conda
Se crea un ambiente [Conda](https://conda.io/) para instalar y administrar el software que se utiliza.


```shell
# Actualización de Conda
conda update -n base -c conda-forge -y conda

# Creación del ambiente e instalación de paquetes
conda create -y -n geonotas
conda activate geonotas
conda config --env --add channels conda-forge
conda config --env --set channel_priority strict
conda install -c conda-forge -y mamba
mamba install -c conda-forge -y \
  quarto \
  gdal libgdal-arrow-parquet

# Una vez finalizada la instalación, el ambiente debe desactivarse y activarse nuevamente
conda deactivate
conda activate geonotas
```

En caso de ser necesario, el ambiente puede borrarse con los siguientes comandos.

```shell
# Borrado del ambiente Conda
conda deactivate
conda env remove -n geonotas
```

### Instalación de Conda/Miniconda
En caso de ser necesario, se recomienda instalar Conda mediante [Miniconda](https://docs.conda.io/projects/miniconda/).

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
```

Se incluye también el procedimiento para la desinstalación.

```shell
# Desinstalación de Miniconda
conda activate
conda init --reverse --all
rm -rf ~/miniconda3

# Hay que salir y entrar de la terminal
```

## Creación del repositorio `geonotas.github.io`
En la organización `geonotas`, debe crearse el repositorio `geonotas.github.io` (`https://github.com/geonotas/geonotas.github.io`), para contener el código fuente del sitio web en https://geonotas.github.io/.

El repositorio local se crea mediante el sistema de publicación técnica y científica [Quarto](https://quarto.org/), como un documento de tipo [libro (*book*)](https://quarto.org/docs/books/).

```shell
# Creación del sitio web de geonotas
quarto create-project geonotas.github.io \
  --title "geonotas" \
  --type book

cd geonotas.github.io
```

Se borran y renombran algunos de los archivos generados automáticamente por Quarto.
```shell
# Borrado y renombramiento de archivos
rm intro.qmd
rm summary.qmd
mv references.bib referencias.bib
```

Y se genera contenido para otros archivos.
```shell
# index.qmd
echo "# Prefacio {.unnumbered}"  > index.qmd
echo "Este sitio ..."           >> index.qmd

# referencias.qmd
echo "# Referencias {.unnumbered}"  > referencias.qmd
echo "::: {#refs}"                 >> referencias.qmd
echo ":::"                         >> referencias.qmd

# _quarto.yml
echo "project:"                     > _quarto.yml
echo "  type: book"                >> _quarto.yml
echo "  output-dir: docs"          >> _quarto.yml
echo ""                            >> _quarto.yml
echo "book:"                       >> _quarto.yml
echo "  title: geonotas"           >> _quarto.yml
echo "  chapters:"                 >> _quarto.yml
echo "    - index.qmd"             >> _quarto.yml
echo "    - referencias.qmd"       >> _quarto.yml
echo ""                            >> _quarto.yml
echo "format:"                     >> _quarto.yml
echo "  html:"                     >> _quarto.yml
echo "    lang: es"                >> _quarto.yml
echo "    theme: cosmo"            >> _quarto.yml
echo "  pdf:"                      >> _quarto.yml
echo "    documentclass: scrreprt" >> _quarto.yml

# .nojekyll (evita procesamiento adicional de Jekyll en GH Pages)
echo "" > .nojekyll

# .gitignore
echo "/.quarto/" > .gitignore
```

Vista previa del sitio
```shell
# Vista previa
quarto preview
```

Generación del sitio
```shell
# Generación
quarto render
```
