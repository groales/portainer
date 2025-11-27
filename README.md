# Infraestructura: Portainer

Este repositorio contiene el despliegue de **Portainer CE** para la gestión centralizada de contenedores Docker.

## 🧩 Componentes

- `docker-compose.yml` → definición del stack
- `stack.env.example` → plantilla de configuración (no contiene secretos)
- `scripts/backup.sh` → script básico para backups
- `scripts/restore.sh` → restauración
- `.gitignore` → evita subir volúmenes y secretos

## 🚀 Despliegue

1. Crear `.env` desde el ejemplo:

```bash
cp stack.env.example .env
```

## 📚 Documentación

- Guía inicial: [`docs/Home.md`](docs/Home.md)
- Próximas páginas sugeridas:
	- `docs/Instalacion.md` — instalación avanzada y TLS
	- `docs/Backup.md` — backup/restore del volumen `data`
	- `docs/Traefik.md` — integración con proxy reverso