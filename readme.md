
```markdown
# Documentación del Backend

Lo primero que debes configurar son las variables de entorno siguiendo esta estructura:

```plaintext
# Este valor se usa para configurar los JWT tokens que gestionan la autenticación. Puedes generar una clave secreta usando el comando 'openssl rand -hex 32' en la terminal o usar una de tu preferencia, pero debe ser segura.
SECRET_KEY=secret

# Este es el algoritmo a usar en los tokens. Realmente no hace falta que lo cambies, ya que HS256 funciona bastante bien.
ALGORITHM=HS256

# Define el tiempo que deseas mantener a un usuario con la sesión activa. El valor va en minutos. Los usuarios que superen este límite tendrán que volver a iniciar sesión, como funciona en muchas aplicaciones.
ACCESS_TOKEN_EXPIRE_MINUTES=300

# Estos son los valores de acceso a tu base de datos que actualmente gestionas en el servidor de Plesk.
POSTGRES_SERVER=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=123456
POSTGRES_DB=database
```

Una vez configuradas las variables, el siguiente paso es ejecutar las migraciones de las tablas a la base de datos. En tu proyecto, lo he configurado con Alembic, ya que se integra perfectamente con SQLAlchemy, el ORM usado en el proyecto para gestionar las consultas a la base de datos.

1. Dirígete a la raíz del proyecto en la consola y ejecuta: `alembic init alembic`. Esto creará los primeros archivos necesarios.
2. Una vez realizado, ejecuta `alembic revision --autogenerate` para crear el primer archivo de migración con todas las tablas.
3. Finalmente, ejecuta `alembic upgrade head` para enviar la migración a la base de datos.

Con esto, ya tendríamos todo configurado con la base de datos.

**NOTA:** Esto es solo para documentación. El proyecto en tu servidor ya tiene todos estos pasos configurados previamente por mí. Esto solo sería necesario si cambias de base de datos, por ejemplo, si deseas borrar todo y empezar de nuevo como lo hicimos en una oportunidad.

El siguiente paso es alimentar la base de datos con los datos esenciales, que son tu usuario administrador y los alérgenos. Para esto, lo he dejado fácilmente en dos archivos: `seed.py` y `allergens.py`.

- `seed.py` contiene los datos para tu usuario. Asegúrate de que tengan los datos y la contraseña que deseas. Una vez ajustado, ejecuta el archivo con Python desde la raíz del proyecto: `python3 seed.py`.
- En el caso de los alérgenos, lo mismo se ejecuta con `python3 allergens.py`. Si deseas agregar más alérgenos, puedes editar la lista en el archivo antes de ejecutarlo.

## Configuración del API

El API está hecho con FastAPI, usando Uvicorn.

Las tablas están definidas en el archivo `models.py` y todos los endpoints de la API están definidos en la carpeta `/routers`, separados por módulos.

Para ejecutar el API y poder realizar peticiones desde el frontend, utiliza `uvicorn main:app` y comenzará a correr en el puerto `:8000`. Todo esto también lo he configurado en tu servidor de Plesk, por lo que no hace falta hacer ajustes en esto.

## Creación de un Entorno Virtual y Instalación de Dependencias

Para asegurar que el proyecto se ejecute en un entorno aislado y sin conflictos con otras aplicaciones o versiones de paquetes, es recomendable crear un entorno virtual de Python. A continuación, se detallan los pasos para crear un entorno virtual y cómo instalar las dependencias necesarias:

1. **Crear un entorno virtual:**
   Navega hasta la raíz del proyecto en la terminal y ejecuta el siguiente comando para crear un entorno virtual. Puedes nombrarlo como prefieras, por ejemplo, `env`:
   ```bash
   python3 -m venv env
   ```

2. **Activar el entorno virtual:**
   - En **Windows**:
     ```bash
     .\env\Scripts\activate
     ```
   - En **macOS/Linux**:
     ```bash
     source env/bin/activate
     ```

   Una vez activado, deberías ver el nombre del entorno virtual en la línea de comandos, indicando que está activo.

3. **Instalar las dependencias:**
   Con el entorno virtual activado, instala las dependencias del proyecto usando el archivo `requirements.txt`. Este archivo contiene una lista de todos los paquetes necesarios para ejecutar el proyecto. Ejecuta el siguiente comando:
   ```bash
   pip install -r requirements.txt
   ```

4. **Desactivar el entorno virtual:**
   Cuando hayas terminado de trabajar en el proyecto, puedes desactivar el entorno virtual ejecutando:
   ```bash
   deactivate
   ```

### Notas adicionales

- **Mantén actualizado el archivo `requirements.txt`:** Cada vez que añadas o actualices una dependencia, asegúrate de actualizar el archivo `requirements.txt` usando el comando `pip freeze > requirements.txt`.
- **Usa entornos virtuales para cada proyecto:** Esto evita conflictos entre dependencias de diferentes proyectos y asegura que cada proyecto tenga sus propias versiones específicas de los paquetes.

## Notas a considerar

- Si en algún momento se cae tu servidor y el API deja de funcionar, asegúrate de que la base de datos esté funcionando. Dirígete a la carpeta `backend` del proyecto, cancela cualquier proceso que esté activo y reinicia el backend usando el mismo comando `uvicorn main:app`.
- Asegúrate de mantener actualizadas las dependencias del proyecto para evitar vulnerabilidades de seguridad.
- Considera implementar monitoreo y alertas para detectar caídas o comportamientos inusuales en el servidor.
- Realiza copias de seguridad periódicas de la base de datos para evitar pérdida de datos.
```

Puedes copiar y pegar este contenido en tu archivo `README.md`. Si necesitas más ajustes o tienes alguna otra pregunta, no dudes en decírmelo.
```




Aquí tienes la sección actualizada para la documentación del frontend, indicando que la configuración en el servidor Plesk ya está realizada:

---

## Documentación del Frontend

El frontend de esta aplicación está desarrollado con Angular. A continuación, se detallan los pasos para instalar las dependencias, construir la aplicación y desplegarla en un servidor Plesk.

### Requisitos Previos

- Asegúrate de tener instalado [Node.js](https://nodejs.org/) y [npm](https://www.npmjs.com/) en tu máquina.
- Tener acceso a un servidor Plesk donde desplegar la aplicación.
- Crear una cuenta en [GitHub](https://github.com/) si aún no tienes una.

### Pasos para Instalar y Desplegar

1. **Crear un Repositorio en GitHub:**
   - Inicia sesión en tu cuenta de GitHub.
   - Crea un nuevo repositorio en tu cuenta de GitHub para alojar el proyecto Angular.
   - Anota la URL del repositorio, ya que la necesitarás para los siguientes pasos.

2. **Descargar el Proyecto:**
   - Descarga el archivo `.zip` que contiene todos los archivos del proyecto Angular.
   - Extrae el contenido del archivo `.zip` en una ubicación de tu elección en tu máquina local.

3. **Configurar el Repositorio Local:**
   - Abre una terminal y navega hasta el directorio donde has extraído el proyecto.
   - Inicializa un repositorio Git en el directorio del proyecto:
     ```bash
     git init
     ```
   - Añade el repositorio remoto de GitHub:
     ```bash
     git remote add origin <URL-del-repositorio>
     ```

4. **Subir el Proyecto a GitHub:**
   - Añade todos los archivos al repositorio local:
     ```bash
     git add .
     ```
   - Realiza el primer commit:
     ```bash
     git commit -m "Versión inicial del proyecto"
     ```
   - Sube los cambios a GitHub:
     ```bash
     git push -u origin master
     ```

5. **Instalar Dependencias:**
   Navega hasta el directorio del proyecto en la terminal y ejecuta el siguiente comando para instalar todas las dependencias necesarias:
   ```bash
   npm install
   ```

6. **Construir la Aplicación:**
   Una vez instaladas las dependencias, construye la aplicación Angular para producción usando el siguiente comando:
   ```bash
   ng build --prod
   ```
   Esto generará una carpeta `dist/` en la raíz del proyecto, que contiene todos los archivos necesarios para desplegar la aplicación.

7. **Desplegar en el Servidor Plesk:**
   - Accede a tu servidor Plesk a través del panel de control.
   - Navega hasta el dominio o subdominio donde deseas desplegar la aplicación.
   - **Nota:** La configuración necesaria en el servidor Plesk para ejecutar el frontend ya ha sido realizada. Esto incluye la configuración de `.htaccess` y cualquier otra configuración necesaria para manejar las rutas de Angular.
   - Usa el administrador de archivos de Plesk para subir el contenido de la carpeta `dist/` a la carpeta `httpdocs/` o `public_html/` de tu dominio. Puedes hacerlo arrastrando y soltando los archivos o usando un cliente FTP.

8. **Verificar el Despliegue:**
   - Asegúrate de que todos los archivos se hayan subido correctamente.
   - Abre un navegador web y accede a la URL de tu dominio para verificar que la aplicación Angular se esté ejecutando correctamente.

### Notas Adicionales

- **Actualizaciones:** Cada vez que realices cambios en el código fuente, repite los pasos de construcción y despliegue para asegurarte de que la versión más reciente esté disponible en el servidor.
- **Optimización:** Considera optimizar la aplicación para mejorar los tiempos de carga y el rendimiento general. Esto puede incluir la minificación de archivos CSS y JavaScript, y la configuración de caché en el servidor.

---

Esta sección proporciona una guía clara para desplegar una aplicación Angular en un servidor Plesk, incluyendo la creación de un repositorio en GitHub y la configuración previa del servidor. Si necesitas más detalles o tienes alguna otra pregunta, no dudes en decírmelo.
