# Normalización de la Base de Datos DENUE en Excel

> **Materia:** Computación en la Nube (361)  
> **Maestro:** Guillermo Alejandro Chávez Sánchez  
> **Herramienta:** Microsoft Excel  
> **Fuente de datos:** INEGI — DENUE (Directorio Estadístico Nacional de Unidades Económicas)  
> **Integrante:** Ximena Lozada Naranjo

---

## Descripción general

Se aplicó un proceso de normalización relacional al conjunto de datos del DENUE, reduciendo la tabla original de 26 columnas a una estructura más limpia y organizada. El resultado es un archivo Excel con 8 hojas, donde se separan los datos en tablas especializadas (catálogos) y se documentan las decisiones técnicas tomadas.

---

## Estructura del archivo Excel

El archivo `DENUE_Normalizado.xlsx` está compuesto por las siguientes hojas:

| Hoja | Contenido |
|------|-----------|
| `Establecimientos` | Tabla principal con 678 registros (reducida de 26 a 11 columnas) |
| `CAT_Actividades` | Catálogo de actividades económicas SCIAN con agrupación por sector |
| `CAT_Entidades` | Catálogo de entidades federativas |
| `CAT_Municipios` | Catálogo de municipios con referencia a entidad |
| `CAT_Localidades` | Catálogo de localidades con jerarquía completa |
| `CAT_PersonalOcupado` | Rangos de personal con estimado numérico y clasificación de tamaño |
| `Tablas_Especificas` | Resúmenes analíticos por municipio, actividad y tamaño |
| `Justificacion` | Documentación técnica de las decisiones de normalización |

---

## Diccionarios de datos

### `Establecimientos` — Tabla principal

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `ID` | Numérico | Identificador único del establecimiento |
| `Nombre Establecimiento` | Texto | Nombre comercial |
| `Razón Social` | Texto | Razón social legal (puede ser nulo) |
| `Clave Actividad` | Numérico | Código SCIAN → referencia `CAT_Actividades` |
| `Rango Personal Ocupado` | Texto | Categoría → referencia `CAT_PersonalOcupado` |
| `Dirección` | Texto | Dirección consolidada en un solo campo |
| `Código Postal` | Numérico | CP del establecimiento |
| `ID Localidad` | Numérico | Referencia → `CAT_Localidades` |
| `ID Municipio` | Numérico | Referencia → `CAT_Municipios` |
| `ID Entidad` | Numérico | Referencia → `CAT_Entidades` |

---

### `CAT_Actividades`

| Campo | Descripción |
|-------|-------------|
| `Código Actividad` | Código SCIAN de 6 dígitos |
| `Nombre Actividad` | Descripción de la actividad económica |
| `Sector (2 dígitos)` | Agrupación por sector productivo |

---

### `CAT_Entidades`

| Campo | Descripción |
|-------|-------------|
| `Clave Entidad` | Clave numérica |
| `Nombre Entidad` | Nombre de la entidad federativa |

---

### `CAT_Municipios`

| Campo | Descripción |
|-------|-------------|
| `Clave Municipio` | Clave numérica del municipio |
| `Nombre Municipio` | Nombre del municipio |
| `Clave Entidad` | Referencia a `CAT_Entidades` |

---

### `CAT_Localidades`

| Campo | Descripción |
|-------|-------------|
| `Clave Localidad` | Clave numérica |
| `Nombre Localidad` | Nombre de la localidad |
| `Clave Municipio` | Referencia a `CAT_Municipios` |
| `Clave Entidad` | Referencia a `CAT_Entidades` |

---

### `CAT_PersonalOcupado`

| Campo | Descripción |
|-------|-------------|
| `Rango` | Rango textual (ej. "0 a 5 personas") |
| `Estimado Personas` | Valor numérico representativo |
| `Clasificación Tamaño` | Micro / Pequeña / Mediana / Grande |

---

## Tablas de análisis (`Tablas_Especificas`)

### A) Por municipio
Muestra la concentración geográfica de unidades económicas: municipio, entidad, número de establecimientos, porcentaje del total y personal estimado.

### B) Por actividad económica
Identifica las actividades dominantes: código y nombre SCIAN, conteo de establecimientos y participación porcentual.

### C) Por tamaño de empresa
Refleja la estructura del tejido empresarial: clasificación por tamaño, número de establecimientos y participación porcentual.

---

## Justificación de la normalización

| # | Acción | Justificación técnica |
|---|--------|-----------------------|
| 1 | Reducción de 26 a 11 columnas en `Establecimientos` | La tabla original tenía alta redundancia en campos de vialidad. Consolidar la dirección en un campo único elimina ruido y facilita la lectura |
| 2 | Catálogo de actividades SCIAN | El nombre de la actividad depende solo del código. Separarlo evita repetir el mismo texto en cientos de filas (**3FN**) |
| 3 | Catálogo de entidades | La entidad depende únicamente de su clave. Normalizar reduce 678 repeticiones a los registros únicos necesarios |
| 4 | Catálogo de municipios | El municipio depende de su clave compuesta. Aislar esta dependencia cumple la **2FN** y conserva la jerarquía geográfica |
| 5 | Catálogo de localidades | La localidad depende de su clave completa. Separarla elimina dependencias transitivas de la tabla principal |
| 6 | Catálogo de personal ocupado | Estandariza la clasificación por tamaño según criterios INEGI/SE y agrega un valor numérico para cálculos estadísticos |
| 7 | Tablas de análisis | Permiten obtener KPIs de forma inmediata sin manipular el dato crudo |
| 8 | Tratamiento de nulos | Los nulos en `raz_social`, `numero_int` y `cod_postal` se conservan como vacíos, ya que son valores válidos y esperados en el DENUE |

---

## Enfoque de diseño

La normalización se orientó a facilitar el trabajo del analista:

- **Menos ruido:** de 26 columnas a 11 campos semánticamente claros
- **Sin redundancia:** los catálogos permiten actualizar un dato en un solo lugar
- **Jerarquía geográfica explícita:** Entidad → Municipio → Localidad como cadena de llaves foráneas
- **KPIs listos:** tablas de resumen disponibles sin transformaciones adicionales
- **Integridad de nulos:** los valores faltantes están documentados como válidos, no corregidos artificialmente

---

## Estructura del repositorio

```
DENUE-Normalizacion/
├── README.md
└── DENUE_Normalizado.xlsx
```

---

## Fuentes

- INEGI — DENUE: https://www.inegi.org.mx/app/mapa/denue/
- SCIAN: https://www.inegi.org.mx/app/scian/
