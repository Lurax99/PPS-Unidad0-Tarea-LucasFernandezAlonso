# Despliegue de la documentación con Docker Compose

Documentación completa del proceso de **instalación de Docker**, configuración de **Docker Compose** y despliegue local de la documentación generada con MkDocs para la **Tarea Unidad 0 – RA5**.

El objetivo es servir la documentación estática utilizando un contenedor **Nginx**, facilitando un despliegue reproducible y estandarizado.

**Autor:** Lucas Fernández Alonso

---

## Índice

1. [Instalación de Docker](#1-instalación-de-docker)  
2. [Añadir el usuario al grupo Docker](#2-añadir-el-usuario-al-grupo-docker)  
3. [Acceso al directorio del proyecto](#3-acceso-al-directorio-del-proyecto)  
4. [Comprobación de ramas remotas](#4-comprobación-de-ramas-remotas)  
5. [Uso de la rama gh-pages](#5-uso-de-la-rama-gh-pages)  
6. [Creación del archivo docker-compose.yml](#6-creación-del-archivo-docker-composeyml)  
7. [Arranque del contenedor](#7-arranque-del-contenedor)  
8. [Verificación del contenedor en ejecución](#8-verificación-del-contenedor-en-ejecución)  
9. [Acceso desde el navegador](#9-acceso-desde-el-navegador)  
10. [Inspección del contenedor](#10-inspección-del-contenedor)  
11. [Conclusión](#11-conclusión)

---

## 1. Instalación de Docker

Actualizamos los repositorios e instalamos Docker desde los paquetes oficiales:

```bash
sudo apt update
sudo apt install -y docker.io
docker version
```

![instalacion docker](img/docker%20(1).png)

> **Notas:**  
> - La opción `-y` acepta automáticamente todas las confirmaciones.  
> - El comando `docker version` permite verificar que Docker se ha instalado correctamente.

---

## 2. Añadir el usuario al grupo Docker

Para evitar usar `sudo` en cada comando Docker, añadimos el usuario actual al grupo `docker`:

```bash
sudo usermod -aG docker $USER
```

![grupo docker](img/docker%20(2).png)

> **Nota:** Es necesario cerrar sesión y volver a entrar para que el cambio tenga efecto.

---

## 3. Acceso al directorio del proyecto

Nos desplazamos al directorio raíz del repositorio:

```bash
cd PPS-Unidad0-Tarea-LucasFernandezAlonso/
```

---

## 4. Comprobación de ramas remotas

Actualizamos la información del repositorio remoto y listamos las ramas disponibles:

```bash
git fetch --all
git branch -r
```

---

## 5. Uso de la rama `gh-pages`

Traemos la rama remota `gh-pages` a local y verificamos que estamos en la rama correcta:

```bash
git checkout -t origin/gh-pages
git status
```

![rama gh-pages](img/docker%20(3).png)

---

## 6. Creación del archivo docker-compose.yml

Creamos el archivo de configuración de Docker Compose:

```bash
nano docker-compose.yml
```

![crear compose](img/docker%20(4).png)

Contenido del archivo `docker-compose.yml`:

```yaml
services:
  nginx-docs:
    image: nginx:alpine
    container_name: PPS-Unidad0-Tarea-LucasFernandezAlonso
    ports:
      - "8085:80"
    volumes:
      - ./:/usr/share/nginx/html:ro
    restart: always
```

> **Notas:**
> - Se omite la clave `version` para usar la versión más reciente compatible de Docker Compose.
> - Se utiliza un **bind mount** desde la carpeta actual (`./`) hacia el directorio web de Nginx.
> - El volumen se monta en **modo solo lectura (`:ro`)** para mayor seguridad.

---

## 7. Arranque del contenedor

Levantamos el servicio en segundo plano:

```bash
docker-compose up -d
```

![contenedor levantado](img/docker%20(5).png)

---

## 8. Verificación del contenedor en ejecución

Comprobamos que el contenedor está activo:

```bash
docker ps
```

![docker ps](img/docker%20(6).png)

---

## 9. Acceso desde el navegador

Accedemos a la documentación desde el navegador web:

```
http://localhost:8085
```

![documentacion local](img/docker%20(7).png)

---

## 10. Inspección del contenedor

Obtenemos información detallada del contenedor:

```bash
docker inspect PPS-Unidad0-Tarea-LucasFernandezAlonso
```

![docker inspect](img/docker%20(8).png)

Archivo completo generado:  
[Ver archivo JSON de docker inspect](inspect.json)

---

## 11. Conclusión

El uso de **Docker Compose** para servir la documentación presenta claras ventajas:

- **Declarativo y limpio:** toda la configuración se centraliza en un único archivo.
- **Reproducible:** cualquier usuario puede levantar el servicio con `docker-compose up`.
- **Centralización de la configuración:**
  - Nombre del contenedor
  - Puertos expuestos
  - Volúmenes (bind mount)
  - Imagen utilizada
- **Estándar actual** para el despliegue de servicios Docker.

Esta solución permite servir la documentación de forma local, rápida y segura, manteniendo coherencia con entornos profesionales.