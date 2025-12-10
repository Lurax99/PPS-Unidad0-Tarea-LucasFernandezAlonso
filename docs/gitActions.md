# Documentación del despliegue de MkDocs

Documentación completa del proceso de **configuración y despliegue de la documentación del proyecto** para la **Tarea Unidad 0 – RA5**.  
Incluye la creación y edición de `mkdocs.yml`, la configuración de GitHub Actions y la publicación automática en GitHub Pages.

**Autor:** Lucas Fernández Alonso

---

## Índice

1. [Editar mkdocs.yml](#1-editar-mkdocsyml)  
2. [Configurar la pipeline (GitHub Actions)](#2-configuración-de-la-pipeline-github-actions)  
3. [Commit y push de los cambios](#3-hacemos-commit-y-push-de-los-cambios)  
4. [Verificación del workflow en GitHub](#4-comprobamos-que-se-ha-creado-el-workflow-en-github)  
5. [Verificación final y rama `gh-pages`](#5-verificación-final)

---

## 1. Editar mkdocs.yml

Abrimos el archivo para editarlo:

```bash
nano mkdocs.yml
```

> **Nota:** Como MkDocs usa `docs/` por defecto como directorio de documentación, **no es necesario configurar `docs_dir`**.

![editar mkdocs](img/gitactions%20(1).png)

Ejemplo de configuración final:

```yaml
site_name: PPS Unidad 0 - Tarea RA5
site_description: Documentación del proyecto
site_author: Lucas Fernández Alonso

nav:
  - Inicio: index.md
  - Git: git.md
  - GitHub Actions: gitActions.md
  - GitHub Pages: gitPages.md
  - Docker: docker.md
  - Conclusiones: conclusiones.md

theme:
  name: mkdocs

plugins:
  - search
```

![configuracion yml](img/gitactions%20(2).png)

---

## 2. Configuración de la pipeline (GitHub Actions)

Abrimos el archivo de workflow:

```bash
nano .github/workflows/deploy_docs.yml
```

![config workflow](img/gitactions%20(3).png)

Contenido final del workflow:

```yaml
name: Deploy MkDocs documentation

on:
  push:
    branches:
      - main

permissions:
  contents: write
  pages: write
  id-token: write

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install MkDocs
        run: |
          pip install mkdocs

      - name: Deploy to GitHub Pages
        run: |
          mkdocs gh-deploy --force
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 3. Hacemos commit y push de los cambios

```bash
git add .
git commit -m "configuración de MkDocs y pipeline CI/CD de documentación"
git push origin main
```

![commit push](img/gitactions%20(4).png)

---

## 4. Verificación del workflow en GitHub

Comprobamos que el workflow se ha creado y ejecutado correctamente:

![comprobamos](img/gitactions%20(5).png)

> **Nota:** Pueden aparecer errores en otros workflows si se crearon archivos `.yml` vacíos previamente.

---

## 5. Verificación final y rama `gh-pages`

Todos los pasos se han realizado correctamente:

![pasos](img/gitactions%20(6).png)

Se ha creado correctamente la rama `gh-pages`:

![rama gh-pages](img/gitactions%20(7).png)

# Conclusión

La documentación del proyecto quedó completamente configurada y desplegada con éxito. Los puntos clave son:

- Archivo `mkdocs.yml` creado y configurado correctamente.
- Workflow de GitHub Actions (`.github/workflows/deploy_docs.yml`) configurado para desplegar automáticamente la documentación en cada push a `main`.
- Dependencias instaladas correctamente (MkDocs).
- La rama `gh-pages` creada y publicada en GitHub Pages.
- Todos los pasos verificados y documentados, asegurando que la documentación online refleja la versión más reciente del proyecto.
