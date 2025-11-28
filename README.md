# Infraestructura: Portainer

# Portainer CE — Despliegue con Docker Compose (Linux)

Este repositorio proporciona una forma sencilla de desplegar **Portainer Community Edition (CE)** usando Docker Compose en Linux, siguiendo las recomendaciones oficiales:
https://docs.portainer.io/start/install-ce/server/docker/linux#docker-compose

## Requisitos
- Docker y Docker Compose instalados en el host.
- Usuario con permisos para acceder a `/var/run/docker.sock`.
- Conectividad a los puertos necesarios.

## Qué incluye este stack
- Servicio `portainer` con la imagen `portainer/portainer-ce:lts`.
- Montaje del socket Docker: `/var/run/docker.sock:/var/run/docker.sock`.
- Volumen persistente `portainer_data` montado en `/data` para conservar la configuración.
- Puertos:
	- `9443:9443` (UI HTTPS de Portainer)
- Red `portainer_network` (por defecto).

## Archivo Compose
El `docker-compose.yaml` actual contiene:

```yaml
services:
	portainer:
		container_name: portainer
		image: portainer/portainer-ce:lts
		restart: always
		volumes:
			- /var/run/docker.sock:/var/run/docker.sock
			- portainer_data:/data
		ports:
			- 9443:9443


volumes:
	portainer_data:
		name: portainer_data

networks:
	default:
		name: portainer_network
```

## Pasos de despliegue (PowerShell en Windows o bash equivalente en Linux)
1) Clonar el repositorio (si no lo tienes):
```powershell
cd C:\Users\gustavo.roales.ICT-IBERIA\repos
git clone https://git.ictiberia.com/ICT-IT/portainer
cd portainer
```

2) (Opcional) Crear y ajustar `.env` si lo usas para variables adicionales.

3) Levantar Portainer con Docker Compose:
```powershell
docker compose up -d
```

4) Verificar que Portainer está en ejecución:
```powershell
docker ps --filter name=portainer
```

5) Acceder a la interfaz web:
- URL: `https://<HOST>:9443`
- Al primer acceso, Portainer te pedirá crear el usuario administrador.

## Notas y buenas prácticas
- Mantén el volumen `portainer_data` para persistir configuración y datos.
- Para exponer Portainer a Internet, usa un proxy inverso (Traefik/Nginx) con TLS y reglas de acceso.
- Limita el acceso a la UI mediante firewall/VPN.

### Edge Agents (opcional)
Este stack no expone el puerto `8000` por defecto. Si necesitas gestionar agentes remotos (Edge), añade de nuevo el puerto en la sección `ports`:

```yaml
		ports:
			- 9443:9443
			- 8000:8000
```

## Documentación adicional
- Guía inicial: [`docs/Home.md`](docs/Home.md)
- Instalación avanzada y TLS: [`docs/Instalacion.md`](docs/Instalacion.md)
- Backup/Restore del volumen `data`: [`docs/Backup.md`](docs/Backup.md)
 - Proxy inverso y TLS con Traefik: [`docs/Traefik.md`](docs/Traefik.md)

## Solución de problemas
- Puerto ocupado (`9443/8000`): comprueba procesos con `netstat` o `Get-NetTCPConnection` y ajusta puertos en `docker-compose.yaml`.
- Permisos sobre `/var/run/docker.sock`: ejecuta con un usuario que pertenezca al grupo `docker` (Linux) o valida que Docker Desktop esté activo (Windows WSL2).
- Certificados/TLS: si accedes por `https://` y hay advertencias, usa un proxy inverso con certificados válidos o configura confiables en el cliente.
- Contenedor no arranca: revisa logs con `docker logs portainer` y valida que el volumen `portainer_data` exista.

## Personalización rápida
- Eliminar Edge Agents: borra la línea `- 8000:8000` en `ports:`.
- Cambiar nombre de red/volumen: ajusta `name: portainer_network` y `name: portainer_data` en las secciones `networks` y `volumes`.
- Variables mediante `.env`: añade valores personalizados y referencia desde el compose si lo necesitas.
	- `docs/Traefik.md` — integración con proxy reverso