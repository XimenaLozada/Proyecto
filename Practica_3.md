# Importación de Datos CSV a MySQL con DBeaver

## Introducción

En esta práctica se realizó la importación de un archivo CSV hacia una base de datos MySQL utilizando DBeaver como herramienta de gestión. El archivo de datos corresponde al DENUE (Directorio Estadístico Nacional de Unidades Económicas), publicado por el INEGI. El objetivo fue cargar esta información en una tabla para posteriormente realizar consultas SQL sobre ella.

---

## Herramientas utilizadas

- DBeaver
- MySQL
- Archivo CSV (DENUE - INEGI)
- GitHub

---

## Procedimiento

### 1. Conectarse a la base de datos en DBeaver

1. Abrir DBeaver.
2. Establecer conexión con el servidor MySQL.
3. Seleccionar la base de datos destino donde se importarán los datos.

---

### 2. Iniciar la importación

1. Hacer clic derecho sobre la base de datos en el panel izquierdo.
2. Seleccionar la opción **Import Data**.
3. Elegir el formato **CSV → Import from CSV file(s)**.
4. Presionar **Next**.

---

### 3. Seleccionar el archivo

1. Presionar **Browse** o **Add File**.
2. Localizar el archivo `denue.csv` en el equipo.
3. Seleccionarlo y presionar **Next**.

---

### 4. Crear la tabla destino

1. Seleccionar la opción **Create new table**.
2. Asignar el nombre `denue` a la nueva tabla.
3. Revisar que DBeaver haya detectado correctamente las columnas del archivo.

---

### 5. Finalizar la importación

En la sección **Data load settings** se puede mantener la configuración predeterminada. Presionar **Finish** para ejecutar la carga de datos.

---

## Verificación

Para confirmar que los datos se importaron correctamente:

```sql
SELECT * FROM denue;
```

---

## Errores frecuentes y cómo resolverlos

### Error: Data too long for column

```
SQL Error [1406]: Data too long for column 'nom_estab'
```

**Causa:** El texto del archivo supera el tamaño máximo definido para esa columna.

**Solución:** Ampliar el tamaño del campo con:

```sql
ALTER TABLE denue
MODIFY nom_estab VARCHAR(255);
```

También se puede usar el tipo `TEXT` para columnas con contenido muy largo.

---

### Error: Error occurred during batch insert

**Causa:** Algunas filas contienen datos incompatibles con el tipo de dato definido en la tabla.

**Solución:** Presionar **Skip** para omitir las filas con error, o revisar y corregir el archivo CSV antes de reimportar.

---

### Error: Table doesn't exist

```
Table 'database.denue' doesn't exist
```

**Causa:** Se intenta insertar datos en una tabla que no existe.

**Solución:** Crear la tabla manualmente antes de importar:

```sql
CREATE TABLE denue (
    id INT,
    nom_estab VARCHAR(255),
    nombre_act VARCHAR(255)
);
```

---

### Error: Formato de fecha incorrecto

**Causa:** El archivo CSV contiene fechas en formato `DD/MM/YYYY`, pero MySQL requiere `YYYY-MM-DD`.

**Solución:** Convertir las fechas en el archivo CSV al formato correcto antes de importar.

---

## Conclusión

La importación de archivos CSV con DBeaver es una forma eficiente de cargar grandes volúmenes de datos en una base de datos MySQL sin necesidad de escribir sentencias `INSERT` manualmente. Sin embargo, es indispensable revisar previamente los tipos y tamaños de las columnas para evitar errores durante el proceso. Esta habilidad es especialmente útil cuando se trabaja con datos abiertos o reportes exportados desde otros sistemas.

---

Autora: Ximena Lozada Naranjo  
Materia: Computación en la Nube (361)  
Institución: Universidad Autónoma de Baja California
