# Despliegue de Página Web Estática en Docker usando Ubuntu y Nginx

## Descripción

En esta práctica se llevó a cabo el despliegue de un sitio web HTML dentro de un contenedor Docker. Se utilizó Ubuntu como sistema operativo base y Nginx como servidor web. Las actividades incluyeron la actualización del sistema operativo, la navegación por el sistema de archivos de Linux, la descarga de un repositorio desde GitHub y la solución de un error relacionado con rutas de archivos estáticos.

---

## Entorno de Trabajo

El entorno configurado para el desarrollo de esta práctica fue:

- Docker Desktop
- Imagen base: Ubuntu
- Servidor web: Nginx
- Control de versiones: Git
- Puerto configurado: 8080

El directorio raíz donde Nginx sirve los archivos es:

```
/var/www/html
```

---

## Procedimiento

### 1. Actualización del sistema operativo

Se ejecutaron los siguientes comandos para dejar el sistema en su versión más reciente:

- `apt update` — actualiza el índice de paquetes disponibles
- `apt upgrade` — muestra los paquetes pendientes de actualización
- `apt upgrade -y` — aplica las actualizaciones sin pedir confirmación manual

Al finalizar, el sistema indicó que no había paquetes pendientes de actualizar.

---

### 2. Navegación al directorio del servidor

Para ubicarse en la carpeta donde Nginx publica los archivos web se ejecutó:

```bash
cd /var/www/html
```

---

### 3. Revisión del contenido del directorio

Se usó el comando `ls` para listar los archivos existentes. Los archivos presentes inicialmente eran:

- `index.html`
- `logo.png`

---

### 4. Descarga del repositorio desde GitHub

Se clonó el proyecto con el siguiente comando:

```bash
git clone https://github.com/edgarlopez75-blip/practica2.git
```

El repositorio se descargó dentro del directorio `/var/www/html`, generando la subcarpeta `practica2`. La estructura del directorio quedó así:

```
/var/www/html/
├── index.html
├── logo.png
└── practica2/
```

---

## Acceso desde el navegador

Para visualizar la aplicación se accedió a la siguiente URL:

```
http://127.0.0.1:8080/practica2/index.html
```

- `127.0.0.1` → dirección de loopback (localhost)
- `8080` → puerto mapeado en Docker
- `practica2` → carpeta del proyecto clonado
- `index.html` → archivo principal de la aplicación

---

## Problema detectado

Al cargar la página, el logo no se mostraba. El archivo `logo.png` se encontraba en `/var/www/html/` mientras que `index.html` estaba dentro de `/var/www/html/practica2/`. Como el navegador busca los recursos relativos a la ubicación del HTML, no podía encontrar la imagen.

---

## Solución

Se trasladó la imagen al directorio correcto con el siguiente comando:

```bash
mv /var/www/html/logo.png /var/www/html/practica2/
```

Después de este cambio la imagen se visualizó correctamente.

---

## Aprendizajes

- Uso de comandos básicos de Linux en terminal
- Gestión de directorios y archivos en entornos Ubuntu
- Instalación y actualización de paquetes con `apt`
- Clonación de repositorios con Git
- Comprensión de rutas relativas en HTML
- Despliegue de aplicaciones estáticas en Docker con Nginx
- Diagnóstico y corrección de errores con recursos no encontrados

---

## Conclusión

Esta práctica permitió entender el flujo completo para desplegar una aplicación web estática en un entorno contenerizado. Además de las tareas técnicas, se desarrolló la capacidad de identificar y resolver errores relacionados con la organización de archivos, un aspecto fundamental en el desarrollo y despliegue de aplicaciones web.

---

Autora: Ximena Lozada Naranjo  
Materia: Computación en la Nube (361)  
Institución: Universidad Autónoma de Baja California
