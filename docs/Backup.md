# Backup y Restore del volumen `data`

Portainer guarda su estado en el volumen `data`. Aquí tienes comandos para realizar copias de seguridad y restaurarlas.

## Backup
```powershell
# Crear backup comprimido del volumen 'data'
docker run --rm -v data:/data -v "$PWD:/backup" alpine sh -c "tar czf /backup/portainer-data-$(date +%Y%m%d).tar.gz -C /data ."
```
El archivo se guardará en el directorio actual con timestamp.

## Restore
```powershell
# Restaurar backup al volumen 'data'
docker run --rm -v data:/data -v "$PWD:/backup" alpine sh -c "rm -rf /data/* && tar xzf /backup/portainer-data-YYYYMMDD.tar.gz -C /data"
```
Sustituye `YYYYMMDD` por la fecha del backup.

## Comprobación
```powershell
docker ps --filter name=portainer
```
Si el contenedor estaba corriendo, reinícialo tras restaurar:
```powershell
docker restart portainer
```

---
¿Quieres que convierta estos comandos en scripts (`scripts/backup.sh`, `scripts/restore.sh`) con validaciones?