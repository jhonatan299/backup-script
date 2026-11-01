# Comandos para el laboratorio (tareas 2 a 5)

Ejecútalos en la terminal del laboratorio, dentro de `/home/project`, después de pegar el contenido de `backup.sh`.

## Tarea 2: permisos de ejecución → `backup-permissions`

```bash
chmod u+x backup.sh
ls -l backup.sh | tee backup-permissions
```

## Tarea 3: archivo de respaldo → `backup-file-check`

```bash
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-LX0117EN-SkillsNetwork/labs/Final%20Project/important-documents.zip
unzip -DDo important-documents.zip
touch important-documents/*
./backup.sh important-documents .
ls -l backup-*.tar.gz | tee backup-file-check
```

## Tarea 4: copiar a `/usr/local/bin` → `backup-script-copy`

```bash
sudo cp backup.sh /usr/local/bin/
ls -l /usr/local/bin/backup.sh | tee backup-script-copy
```

## Tarea 5: cron cada 24 horas → `crontab-schedule`

Primero, prueba con un cron que corre cada minuto:

```bash
crontab -e
```

Agrega esta línea, guarda y cierra el editor:

```
*/1 * * * * /usr/local/bin/backup.sh /home/project/important-documents /home/project
```

```bash
sudo service cron start
```

Espera un par de minutos y revisa que aparezcan respaldos nuevos:

```bash
ls -l /home/project/backup-*.tar.gz
```

Si aparecen, detén cron para que no siga creando uno por minuto:

```bash
sudo service cron stop
```

Ahora deja el horario definitivo. Abre `crontab -e` otra vez, borra la línea `*/1` y pon esta:

```
0 0 * * * /usr/local/bin/backup.sh /home/project/important-documents /home/project
```

Después:

```bash
sudo service cron start
crontab -l | tee crontab-schedule
```

Para la entrega, copia el contenido de cada archivo con `cat backup-permissions`, `cat backup-file-check` y los demás.
