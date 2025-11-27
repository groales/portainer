# Instalación avanzada y TLS

Esta guía cubre pasos adicionales para desplegar Portainer con mejores prácticas, incluyendo uso de `.env`, TLS y consideraciones de seguridad.

## Requisitos
- Docker y Docker Compose instalados
- Archivo `.env` creado a partir de `stack.env.example`
- Puertos disponibles: `9443` (HTTPS), `8000` (opcional)

## Variables en `.env`
Ejemplo de variables útiles en `.env`:
```
PORTAINER_IMAGE=portainer/portainer-ce:latest
HTTPS_PORT=9443
EDGE_PORT=8000
DATA_VOLUME=data
```
Asegúrate de que `docker-compose.yaml` las usa correctamente.

## Arranque del stack
```powershell
cd C:\Users\gustavo.roales.ICT-IBERIA\repos\infra-portainer
docke compose up -d
```

## Certificados TLS
- Si usas un proxy (Traefik, Nginx), gestiona TLS en el proxy y expón Portainer internamente.
- Si expones Portainer directamente, configura certificados válidos o asigna reverse proxy delante.

## Seguridad
- Limita el acceso por red (firewall/VPN).
- No compartas `.env` ni secretos.
- Haz backup periódico del volumen `data`.

## Verificación
```powershell
docker ps --filter name=portainer
```
Accede: `https://<HOST>:9443`

---
¿Quieres que añada configuración de Traefik con reglas y certificados (Let's Encrypt)?