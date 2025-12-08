# Creación del repositorio  
Documentación completa del proceso de creación, configuración y despliegue del repositorio para la **Tarea Unidad 0 – RA5**.

Autor: **Lucas Fernández Alonso**

---

## Índice
1. [Instalación de GitHub CLI (gh)](#instalación-de-github-cli-gh)  
2. [Autenticación en GitHub CLI](#autenticación-en-github-cli)  
3. [Configuración de Git y creación del directorio](#configuración-de-git-y-creación-del-directorio)  
4. [Creación de la estructura del proyecto](#creación-de-la-estructura-del-proyecto)  
5. [Inicialización y subida al repositorio remoto](#inicialización-y-subida-al-repositorio-remoto)  
6. [Creación del README.md](#creación-del-readmemd)  
7. [Verificación en GitHub](#verificación-en-github)  

---

## 1. Instalación de GitHub CLI (gh)

GitHub CLI permite gestionar repositorios desde la terminal.

```bash
sudo apt update
sudo apt install gh
```

![instalar gh](img/repositorio%20(1).png)

> Nota: `gh` es especialmente útil para automatizar flujos de trabajo sin necesidad de usar interfaz web.

---

## 2. Autenticación en GitHub CLI

Ejecutamos:

```bash
gh auth login
```

Se debe seleccionar:

- GitHub.com  
- Método de autenticación: **SSH**  
- Autorización mediante **token personal** obtenido desde GitHub.

![autenticar gh](img/repositorio%20(2).png)
![token gh](img/repositorio%20(3).png)

Verificamos el login:

```bash
gh auth status
```

![comprobar login](img/repositorio%20(4).png)

---

## 3. Configuración de Git y creación del directorio

Creamos el directorio y configuramos Git para usar `main` como rama principal.

```bash
mkdir PPS-Unidad0-Tarea-LucasFernandezAlonso
cd PPS-Unidad0-Tarea-LucasFernandezAlonso

git config --global init.defaultBranch main
git init
```

![init repo](img/repositorio%20(5).png)

---

## 4. Creación de la estructura del proyecto

Creamos directorios y archivos necesarios en un solo comando:

```bash
mkdir -p calculator docs .github/workflows && \
touch calculator/__init__.py \
      calculator/gui.py \
      docs/index.md \
      docs/git.md \
      docs/gitActions.md \
      docs/gitPages.md \
      docs/docker.md \
      docs/conclusiones.md \
      mkdocs.yml \
      requirements.txt \
      .github/workflows/CreacionDocumentacion.yml
```

![estructura del proyecto](img/repositorio%20(6).png)

---

## 5. Inicialización y subida al repositorio remoto

Añadimos y guardamos los cambios localmente:

```bash
git add .
git commit -am "Inicializando repositorio"
git branch -M main
```

Creamos el repositorio remoto desde la terminal usando `gh`:

```bash
gh repo create Lurax99/PPS-Unidad0-Tarea-LucasFernandezAlonso \
    --public --source=. --remote=origin --push
```

![crear repo GH](img/repositorio%20(7).png)

---

## 6. Creación del README.md

Creamos un README inicial con la información básica del proyecto:

```bash
echo -e "# Tarea Unidad 0 - RA5. Tarea para entregar Unidad0\nLucas Fernández Alonso" > README.md

git add .
git commit -am "Agregado README"
git push origin main
```

![agregar readme](img/repositorio%20(8).png)

---

## 7. Verificación en GitHub

Comprobamos que:

- El repositorio está creado correctamente.
- La estructura es correcta.
- El README aparece en el repositorio principal.

![Comprobación final](img/repositorio%20(9).png)

---

# Conclusión

El repositorio quedó completamente configurado con:

- Git inicializado y rama `main`.
- Autenticación mediante GitHub CLI.
- Estructura del proyecto generada.
- Integración remota mediante `gh repo create`.
- Documentación inicial en `README.md`.
