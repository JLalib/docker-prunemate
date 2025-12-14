# PruneMate – Contenedor Docker

Este repositorio contiene la configuración necesaria para desplegar **PruneMate**, una herramienta web ligera para la **autolimpieza y mantenimiento de Docker**, utilizando **Docker Compose**.

PruneMate permite gestionar y ejecutar tareas de limpieza (`docker system prune`) de forma visual y controlada, ayudando a liberar espacio en disco eliminando contenedores, imágenes, redes y volúmenes no utilizados.

```
⚠️ DISCLAIMER: PruneMate uses Docker's native prune commands to delete unused resources. This means it removes containers, images, networks, and volumes that Docker considers "unused" - be careful with volumes as they may contain important data.
Ensure you understand what will be pruned before enabling automated schedules. The author is not responsible for any data loss or system issues. Use at your own risk."
```
---

## 🚀 Características

- Imagen: `anoniemerd/prunemate:latest`
- Interfaz web simple e intuitiva
- Limpieza manual y programada de recursos Docker
- Acceso directo al motor Docker mediante socket
- Configuración persistente
- Logs persistentes
- Reinicio automático (`unless-stopped`)

---

## 📁 Estructura de archivos

```
.
├── compose.yml
├── logs/
└── config/
```

- `logs/` → almacena los logs generados por PruneMate  
- `config/` → contiene la configuración persistente de la aplicación  

> ⚠️ **Importante:** PruneMate requiere acceso al socket Docker del host (`/var/run/docker.sock`).

---

## 🐳 docker-compose.yml

```yaml
services:
  prunemate:
    image: anoniemerd/prunemate:latest
    container_name: prunemate
    ports:
      - "8300:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./logs:/var/log
      - ./config:/config
    environment:
      - PRUNEMATE_TZ=Europe/Madrid
      - PRUNEMATE_TIME_24H=true
    restart: unless-stopped
```

---

## 🔧 Variables de entorno

| Variable | Descripción |
|---------|-------------|
| `PRUNEMATE_TZ` | Zona horaria del contenedor |
| `PRUNEMATE_TIME_24H` | Habilita formato horario 24h |

---

## 🌐 Acceso a la interfaz web

```
http://TU-IP:8300
```

---

## ▶️ Puesta en marcha

```bash
mkdir logs config
docker compose up -d
```

---

## 🛑 Detener el contenedor

```bash
docker compose down
```

---

## 🔄 Actualizar PruneMate

```bash
docker compose pull
docker compose up -d
```

---

## ⚠️ Consideraciones de seguridad

- El acceso al **socket Docker** otorga control total sobre el host.
- No expongas PruneMate a Internet sin protección adicional.
- Recomendado únicamente para entornos controlados o servidores personales.

