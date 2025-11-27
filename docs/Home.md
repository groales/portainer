# Inicio — Infra Portainer

Bienvenido a la wiki del repositorio **infra-portainer**. Esta página es la primera entrada y contiene la información esencial para desplegar Portainer usando Docker Compose.

---

## Propósito

Este repositorio contiene la infraestructura mínima para desplegar Portainer Community Edition (CE) con Docker Compose. Está pensado para facilitar la administración de contenedores y stacks en entornos pequeños o de laboratorio.

## Contenido principal

- `docker-compose.yaml`: definición del servicio Portainer (puertos, volúmenes y variables).
- `.env` (variables de entorno): fichero donde pueden definirse variables de entorno (contraseñas, rutas, etc.).

## Puertos expuestos

- 9443: Interfaz web segura (HTTPS) de Portainer.
- 8000: Puerto para el agente Edge (opcional).

## Despliegue rápido (modo local)

1. Sitúate en la carpeta del repositorio:

```powershell
cd C:\Users\gustavo.roales.ICT-IBERIA\repos\infra-portainer
```

2. (Opcional) revisa `.env` y adapta variables si existen.

3. Arranca con Docker Compose:

```powershell
docker compose up -d
# o, en sistemas con docker-compose clásico:
docker-compose up -d
```

4. Verifica que el contenedor está corriendo:

```powershell
docker ps --filter name=portainer
```

5. Accede a la interfaz en: https://<HOST>:9443

## Buenas prácticas

- Asegúrate de tener un volumen persistente para `/data` (ya definido en `docker-compose.yaml`) para mantener la configuración.
- No exponer puertos innecesarios a Internet sin un proxy y HTTPS correctamente configurado.

## Contribuciones a la documentación

Puedes editar esta documentación directamente en `docs/` dentro del repositorio y enviar tus cambios mediante Pull Request. Si en el futuro habilitas la wiki de Gitea, también podrás mantenerla allí.

---

Si quieres, puedo añadir más páginas (instalación avanzada, certificados TLS, backup/restore del volumen `data`, integración con Traefik, etc.). ¿Qué te gustaría añadir en la siguiente página?