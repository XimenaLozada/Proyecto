# Creación y Administración de una Base de Datos MySQL con DBeaver

## Descripción

Esta práctica tuvo como objetivo aprender a crear y gestionar una base de datos relacional utilizando MySQL como sistema gestor y DBeaver como interfaz gráfica de administración. Se trabajaron operaciones fundamentales del lenguaje SQL: creación de base de datos, definición de tabla, inserción de registros y consulta de datos.

---

## Entorno de Trabajo

- DBeaver (interfaz de administración de bases de datos)
- MySQL (sistema gestor de base de datos)
- Conexión local al servidor MySQL
- Editor SQL integrado en DBeaver

La base de datos creada durante la práctica fue nombrada: **servidor**

---

## Procedimiento

### 1. Creación de la base de datos

Se abrió DBeaver y se estableció conexión con el servidor MySQL local. Una vez conectado, se creó la base de datos con el siguiente comando:

```sql
CREATE DATABASE servidor;
```

Para comenzar a trabajar dentro de ella se ejecutó:

```sql
USE servidor;
```

---

### 2. Diseño y creación de la tabla

Se creó la tabla `estudiantes` con tres columnas. El campo `id` actúa como llave primaria con incremento automático, `nombre` almacena el nombre del estudiante como texto y `fecha_nacimiento` guarda la fecha en formato DATE.

```sql
CREATE TABLE estudiantes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    fecha_nacimiento DATE
);
```

---

### 3. Inserción de registros

Se insertó un registro de prueba. Es importante señalar que MySQL requiere el formato `YYYY-MM-DD` para los campos de tipo DATE.

```sql
INSERT INTO estudiantes (nombre, fecha_nacimiento)
VALUES ('Pako', '1981-05-21');
```

---

### 4. Consulta de los datos

Para verificar que el registro fue guardado correctamente se ejecutó:

```sql
SELECT * FROM estudiantes;
```

La consulta devuelve todos los registros de la tabla, incluyendo el `id` generado automáticamente, el nombre y la fecha de nacimiento.

---

## Aprendizajes

- Creación de bases de datos y tablas en MySQL
- Tipos de datos más comunes: `INT`, `VARCHAR`, `DATE`
- Inserción de registros con `INSERT INTO`
- Consulta de datos con `SELECT`
- Identificación y corrección de errores de sintaxis SQL
- Uso de DBeaver como herramienta de administración visual

---

## Conclusión

A través de esta práctica se comprendió el ciclo básico de trabajo con una base de datos relacional: crear la estructura, poblarla con datos y consultarla. El manejo de bases de datos es una habilidad esencial en el desarrollo de sistemas, ya que prácticamente cualquier aplicación requiere algún mecanismo de persistencia de información.

---

Autora: Ximena Lozada Naranjo  
Materia: Computación en la Nube (361)  
Institución: Universidad Autónoma de Baja California
