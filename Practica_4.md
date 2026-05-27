# Configuración de Streamlit con UV en un Contenedor Docker

## Descripción

Esta práctica consistió en preparar un entorno de desarrollo Python dentro de un contenedor Docker basado en Ubuntu, e instalar y ejecutar una aplicación web usando Streamlit. Se utilizó UV como gestor moderno de dependencias, se clonó un proyecto de demostración desde GitHub y se resolvió un problema de configuración de puertos en Docker.

---

## Entorno de Trabajo

- Docker Desktop
- Contenedor basado en Ubuntu
- Python 3
- Streamlit
- UV (gestor de paquetes y entornos virtuales)
- Git
- Puerto de la aplicación: 8501

---

## Procedimiento

### 1. Actualización del sistema

Al iniciar el contenedor se ejecutaron los comandos de actualización:

```bash
apt update
apt upgrade -y
```

`apt update` descarga la lista actualizada de paquetes disponibles desde los repositorios. `apt upgrade -y` instala las versiones más recientes de los paquetes ya existentes en el sistema, aceptando automáticamente las confirmaciones.

---

### 2. Instalación de dependencias

Se instalaron las herramientas necesarias para trabajar con Python y Git:

```bash
apt install -y git curl python3 python3-venv python3-pip
```

Cada herramienta cumple una función específica: `git` permite clonar repositorios, `curl` se usa para descargar archivos desde internet, `python3` es el intérprete de Python, `python3-venv` permite crear entornos virtuales aislados y `python3-pip` es el gestor de paquetes estándar de Python.

Para verificar la instalación de Python se usó:

```bash
python3 --version
```

La versión instalada fue **Python 3.13.3**.

---

### 3. Instalación de UV

UV es una herramienta moderna que reemplaza a `pip` y `venv` con mayor velocidad. Se instaló con:

```bash
curl -Ls https://astral.sh/uv/install.sh | sh
```

El script se descargó e instaló automáticamente en `/root/.local/bin`. Para que la terminal reconociera el nuevo comando se ejecutó:

```bash
source $HOME/.bashrc
```

Se verificó la instalación con:

```bash
uv --version
```

Versión instalada: **UV 0.10.9**.

---

### 4. Clonación del proyecto

Se descargó el proyecto de demostración de Streamlit desde GitHub:

```bash
git clone https://github.com/streamlit/demo-seattle-weather.git
```

Se creó la carpeta `demo-seattle-weather` con todos los archivos del proyecto. Se accedió a ella con:

```bash
cd demo-seattle-weather
```

---

### 5. Creación del entorno virtual

Se creó un entorno virtual con UV para aislar las dependencias del proyecto:

```bash
uv venv
```

Se activó el entorno con:

```bash
source .venv/bin/activate
```

Cuando el entorno está activo, el nombre del proyecto aparece al inicio del prompt de la terminal. Se instalaron las dependencias del proyecto automáticamente con:

```bash
uv sync
```

Este comando leyó los archivos de configuración del proyecto e instaló todas las librerías necesarias, incluyendo Streamlit, pandas y numpy.

---

### 6. Ejecución de la aplicación

Se inició la aplicación con:

```bash
streamlit run streamlit_app.py --server.address 0.0.0.0 --server.port 8501
```

La opción `--server.address 0.0.0.0` es necesaria para que el servidor escuche en todas las interfaces de red del contenedor y sea accesible desde el exterior. `--server.port 8501` define el puerto de la aplicación.

La aplicación quedó disponible en:

```
http://localhost:8501
```

---

## Problema detectado

El contenedor fue creado originalmente a partir de la imagen `ubuntu/nginx`, diseñada para el servidor web Nginx. El mapeo de puertos se configuró incorrectamente como `8501:80`, lo que generó un conflicto porque Streamlit escucha en el puerto 8501 del contenedor, no en el 80.

---

## Solución

Se eliminó el contenedor y se creó uno nuevo con el mapeo correcto:

```
-p 8501:8501
```

El formato correcto en Docker es `puerto_host:puerto_contenedor`. Con esta configuración, el puerto 8501 del contenedor quedó enlazado al puerto 8501 del equipo host, permitiendo acceder correctamente a la aplicación.

---

## Aprendizajes

- Administración de paquetes con `apt` en Linux
- Instalación de herramientas de desarrollo Python
- Uso del gestor UV para entornos virtuales
- Clonación de proyectos con Git
- Ejecución de aplicaciones web interactivas con Streamlit
- Configuración correcta de puertos en Docker
- Diagnóstico y solución de conflictos en mapeo de puertos

---

## Conclusión

Esta práctica demostró el proceso completo para poner en funcionamiento una aplicación Python en un entorno Docker. El uso de UV como gestor de dependencias simplifica notablemente la configuración del entorno en comparación con las herramientas tradicionales. Además, comprender correctamente el mapeo de puertos en Docker es esencial para que los servicios sean accesibles desde fuera del contenedor.

---

Autora: Ximena Lozada Naranjo  
Materia: Computación en la Nube (361)  
Institución: Universidad Autónoma de Baja California
