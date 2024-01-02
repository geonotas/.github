# Notas técnicas

## Creación del directorio principal
```shell
mkdir geonotas
cd geonotas
```

## Creación del directorio del repositorio .github
```shell
mkdir -p .github/profile
cd .github
```

Creación de README.md
```shell
echo "# GeoNotas" > profile/README.md
```

En GitHub:
1. Crear la organización "geonotas" (https://github.com/geonotas)
2. En geonotas, crear el repositorio ".github" (https://github.com/geonotas/.github)

Creación del repositorio Git
```shell
git init
git add .
git commit -m "Commit inicial"
git branch -M main
git remote add origin git@github.com:geonotas/.github
GIT_SSH_COMMAND='ssh -i ~/.ssh/mfvargas' git push -u origin main
```
