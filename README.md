# Infraestructura: Portainer

Este repositorio contiene el despliegue de **Portainer CE** para la gestión centralizada de contenedores Docker.

## 🧩 Componentes

- `docker-compose.yml` → definición del stack
- `stack.env.example` → plantilla de configuración (no contiene secretos)
- `scripts/backup.sh` → script básico para backups
- `scripts/restore.sh` → restauración
- `.gitignore` → evita subir volúmenes y secretos

## 🚀 Despliegue

1. Crear `stack.env` desde el ejemplo:

```bash
cp stack.env.example stack.env