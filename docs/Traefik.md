# Traefik — Proxy inverso y TLS para Portainer

Esta guía muestra cómo exponer Portainer detrás de **Traefik** con HTTPS automático (Let's Encrypt) y reglas limpias.

## Requisitos
- Traefik corriendo en el mismo host (como servicio en otro `docker-compose` o stack).
- Resolución DNS del dominio hacia tu host.

## Etiquetas (labels) de ejemplo para Portainer
Añade estas etiquetas al servicio `portainer` cuando uses Traefik (en el mismo `compose` o en un overlay). Nota: si mantienes `ports: 9443:9443`, la UI seguirá accesible directamente; con Traefik se recomienda eliminar la exposición directa y publicar solo vía proxy.

```yaml
services:
  portainer:
    image: portainer/portainer-ce:lts
    container_name: portainer
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
    # Quita los puertos directos si usas Traefik
    # ports:
    #   - 9443:9443
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.portainer.rule=Host(`portainer.tudominio.com`)"
      - "traefik.http.routers.portainer.entryPoints=websecure"
      - "traefik.http.routers.portainer.tls=true"
      - "traefik.http.services.portainer.loadbalancer.server.port=9443"
      - "traefik.http.routers.portainer.tls.certresolver=myresolver"
```

## Configuración Traefik (resumen)
Traefik necesita:
- `entryPoints` `web` (80) y `websecure` (443).
- `certResolver` configurado (por ejemplo, `myresolver` con ACME/Let's Encrypt).

Ejemplo mínimo de `traefik.yml` (referencia):
```yaml
entryPoints:
  web:
    address: ":80"
  websecure:
    address: ":443"

certificatesResolvers:
  myresolver:
    acme:
      email: tu-email@ejemplo.com
      storage: /letsencrypt/acme.json
      httpChallenge:
        entryPoint: web
```

Volúmenes para Traefik (contenedor):
```yaml
- /var/run/docker.sock:/var/run/docker.sock:ro
- ./letsencrypt:/letsencrypt
```

## Pasos
1. Configura y levanta Traefik con `web` y `websecure`, y un `certResolver`.
2. Aplica las labels a Portainer y evita exponer puertos directos.
3. Apunta el DNS `portainer.tudominio.com` al host.
4. Accede por: `https://portainer.tudominio.com`

## Notas
- Si Portainer corre en 9443 con certificado propio, especifica `loadbalancer.server.port=9443` (como en el ejemplo).
- Para publicar en HTTP interno puedes cambiar a puerto 9000 (HTTP) y ajustar labels acorde (`server.port=9000`).
- Revisa logs de Traefik y Portainer si hay errores de TLS o reglas.