# Automatización de Respaldos MySQL con mysqldump y Cron

## Descripción

Esta práctica tiene como objetivo automatizar la generación de respaldos de una base de datos MySQL mediante el uso de `mysqldump` y el programador de tareas `cron` en un entorno Ubuntu (WSL). Se configuraron distintos esquemas de respaldo: cada minuto, en fechas específicas y con retención limitada de archivos.

---

## Requisitos previos

- Windows con Ubuntu (WSL) instalado
- Docker Desktop instalado y en ejecución
- DBeaver instalado
- Contenedor Docker con MySQL corriendo, nombrado `sql`

---

## Paso 0 — Iniciar el contenedor MySQL

Desde la terminal de Ubuntu (WSL):

```bash
docker start sql
```

Verificar que el contenedor esté activo:

```bash
docker ps
```

Debe aparecer el contenedor `sql` con estado `Up` y el puerto `3306:3306` expuesto.

> Docker Desktop debe estar abierto en Windows para que los comandos `docker` funcionen desde WSL.

---

## Paso 1 — Verificar que Cron esté activo

```bash
systemctl status cron
```

Si el servicio aparece como `inactive (dead)`, iniciarlo con:

```bash
sudo systemctl start cron
```

---

## Paso 2 — Verificar la ruta de mysqldump

```bash
which mysqldump
```

Resultado esperado:

```
/usr/bin/mysqldump
```

---

## Configuraciones de respaldo

### 1. Respaldo automático cada minuto

Ir a la carpeta donde se guardarán los respaldos:

```bash
cd /mnt/c/Users/Edgar/Documents/servidor/
```

Abrir el crontab para editar:

```bash
crontab -e
```

Agregar la siguiente línea (sin `#` al inicio):

```bash
* * * * * /usr/bin/mysqldump -h 127.0.0.1 -u TU_USUARIO -pTU_CONTRASEÑA servidor > /mnt/c/Users/Edgar/Documents/servidor/respaldo_$(date +\%F_\%H-\%M).sql && ls /mnt/c/Users/Edgar/Documents/servidor/
```

El `&& ls` al final muestra los archivos en la terminal cada vez que se genera un respaldo.

Verificar que la tarea fue guardada:

```bash
crontab -l
```

**Resultado esperado:** cada minuto aparece un nuevo archivo:

```
respaldo_2026-03-25_19-39.sql
respaldo_2026-03-25_19-40.sql
respaldo_2026-03-25_19-41.sql
```

---

### 2. Respaldo en fechas y horarios específicos

La sintaxis de cron es:

```
* * * * * comando
│ │ │ │ │
│ │ │ │ └── Día de la semana (0=domingo, 6=sábado)
│ │ │ └──── Mes (1-12)
│ │ └────── Día del mes (1-31)
│ └──────── Hora (0-23)
└────────── Minuto (0-59)
```

**Todos los días a las 11:00 PM:**

```bash
0 23 * * * /usr/bin/mysqldump -h 127.0.0.1 -u TU_USUARIO -pTU_CONTRASEÑA servidor > /mnt/c/Users/Edgar/Documents/servidor/respaldo_$(date +\%F_\%H-\%M).sql
```

**Cada lunes a las 8:00 AM:**

```bash
0 8 * * 1 /usr/bin/mysqldump -h 127.0.0.1 -u TU_USUARIO -pTU_CONTRASEÑA servidor > /mnt/c/Users/Edgar/Documents/servidor/respaldo_$(date +\%F_\%H-\%M).sql
```

**El día 1 de cada mes a medianoche:**

```bash
0 0 1 * * /usr/bin/mysqldump -h 127.0.0.1 -u TU_USUARIO -pTU_CONTRASEÑA servidor > /mnt/c/Users/Edgar/Documents/servidor/respaldo_$(date +\%F_\%H-\%M).sql
```

---

### 3. Retención limitada de respaldos (últimos 5)

Para conservar únicamente los 5 respaldos más recientes y eliminar los anteriores automáticamente:

```bash
* * * * * /usr/bin/mysqldump -h 127.0.0.1 -u TU_USUARIO -pTU_CONTRASEÑA servidor > /mnt/c/Users/Edgar/Documents/servidor/respaldo_$(date +\%F_\%H-\%M).sql && ls -t /mnt/c/Users/Edgar/Documents/servidor/respaldo_*.sql | tail -n +6 | xargs rm -f
```

**Cómo funciona cada parte:**

| Parte | Función |
|---|---|
| `mysqldump ... > respaldo_fecha.sql` | Genera el nuevo archivo de respaldo |
| `&&` | Continúa solo si el respaldo fue exitoso |
| `ls -t ... respaldo_*.sql` | Lista respaldos del más reciente al más antiguo |
| `tail -n +6` | Selecciona todos excepto los 5 más recientes |
| `xargs rm -f` | Elimina los seleccionados |

> Para conservar N respaldos, usa `tail -n +(N+1)`. Ejemplo: para 10 respaldos usa `tail -n +11`.

---

## Resumen de comandos

```bash
# Iniciar contenedor
docker start sql

# Verificar e iniciar cron
systemctl status cron
sudo systemctl start cron

# Verificar ruta de mysqldump
which mysqldump

# Ir a la carpeta de respaldos
cd /mnt/c/Users/Edgar/Documents/servidor/

# Ver archivos generados
ls

# Editar crontab
crontab -e

# Ver tareas configuradas
crontab -l
```

---

Autora: Ximena Lozada Naranjo  
Materia: Computación en la Nube (361)  
Institución: Universidad Autónoma de Baja California

*Documentación generada el 25 de marzo de 2026*
