# Notas técnicas

## Creación de la organización `geonotas`
En la interfaz web de [GitHub](https://github.com), debe crearse la organización `geonotas` (`https://github.com/geonotas`).

También debe crearse el directorio correspondiente en la computadora local.
```shell
# Creación de un directorio para la organización geonotas
mkdir geonotas
cd geonotas
```

## Creación del repositorio `.github`
En la organización `geonotas`, debe crearse el repositorio `.github` (`https://github.com/geonotas/.github`).

También debe crearse el directorio del repositorio local.
```shell
# Creación de un directorio para el repositorio .github
mkdir -p .github/profile
cd .github
```

Creación del archivo `profile/README.md`
```shell
# Creación de profile/README.md
echo "# GeoNotas" > profile/README.md
```

Sincronización del repositorio local con el repositorio remoto
```shell
# Sincronización del repositorio local con el repositorio remoto
git init
git add .
git commit -m "Commit inicial"
git branch -M main
git remote add origin git@github.com:geonotas/.github
GIT_SSH_COMMAND='ssh -i ~/.ssh/mfvargas' git push -u origin main
```
