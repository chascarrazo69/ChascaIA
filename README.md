# ChascaIA — Proyecto Final DAW

> **Descripción breve:** ChascaIA es un asistente de ciberseguridad con inteligencia artificial que ayuda a cualquier persona o empresa a resolver dudas y problemas sencillos de seguridad informática (contraseñas, correos sospechosos, redes wifi, virus...). Desarrollado **sin frameworks**, con **HTML5, CSS3 y JavaScript** en el cliente y **Python** en el servidor, desplegado con **Docker** detrás de **Nginx** con certificado SSL.

* **Autores:** Mario Andújar Gómez, David Fernández Hernández
* **Dominio:** `https://fernandez-andujar-dario.proyecto-daw.iesabdera.local`
* **Gestión del proyecto:** [Enlace al tablero de GitHub Projects](https://github.com/users/chascarrazo69/projects/1)

---

## 1. Alcance Funcional

* **Problema que resuelve:** Mucha gente no sabe si un correo es fraude, si su contraseña es segura o cómo proteger su wifi. ChascaIA responde estas dudas en lenguaje sencillo.
* **Público objetivo:** Usuarios particulares sin conocimientos técnicos y pequeñas empresas.
* **CRUD completo sobre "Consultas de seguridad":**
  * **Crear:** el usuario escribe su problema y la IA genera una respuesta.
  * **Ver:** historial de consultas realizadas.
  * **Editar:** cambiar el título o la categoría de una consulta.
  * **Borrar:** eliminar consultas del historial.
  * Validación de datos en cliente (JavaScript) y en servidor (Python).
* **3 roles de usuario:**
  * **Administrador:** uso ilimitado de la IA, gestión de usuarios y roles, panel de control y gestión de todas las consultas.
  * **Usuario registrado:** uso de la IA con límite diario, gestión de su perfil y de su propio historial de consultas.
  * **Invitado (sin registro):** uso muy limitado de la IA, sin historial guardado.
* **API externa:** **Groq API** (plan gratuito) para generar las respuestas de la IA, con **Pollinations.ai** como respaldo si Groq no está disponible. Respuestas en formato JSON con control de errores.

---

## 2. Stack Tecnológico

* **Frontend:** **HTML5** semántico, **CSS3** (diseño adaptable con *media queries*) y **JavaScript** (peticiones con `fetch`), sin frameworks.
* **Backend:** **Python** sin frameworks (API REST con el módulo estándar `http.server`, conexión a MySQL con `mysql-connector-python` y llamadas a la IA con `urllib`).
* **Base de datos:** **MySQL** (tablas de usuarios, roles y consultas).
* **Proxy inverso + SSL:** **Nginx** con certificado SSL bajo el dominio particularizado.
* **Despliegue:** **Docker** (`Dockerfile` + `docker-compose.yml` con volúmenes y archivo `.env`).
* **Control de versiones y gestión:** **Git + GitHub** (ramas, Pull Requests, GitHub Projects, Issues y Milestones).

---

## 5. Recursos y Logística por Fase

| Fase | Recursos (Hardware y Software) | Logística |
| :--- | :--- | :--- |
| **1. Planificación** | Ordenadores del equipo, navegador, editor de texto | Repositorio en GitHub, tablero en GitHub Projects, Issues y Milestones por fase |
| **2. Análisis y diseño** | Draw.io, MySQL Workbench, Figma | Carpeta `/docs` con diagrama E-R, wireframes y esquema de Docker |
| **3. Implementación** | VS Code, HTML5, CSS3, JavaScript, Python, MySQL, Docker, OpenSSL | Ramas Git, Pull Requests vinculados a Issues, contenedores Docker y `.env.example` |
| **4. Pruebas y validaciones** | Postman, DevTools del navegador, Lighthouse | Dominio local en el archivo `hosts` y gestión de errores en GitHub Issues |
| **5. Documentación final** | Editor Markdown, herramienta de capturas | Redacción de `manual_tecnico.md`, `manual_usuario.md` y registro de prompts de IA |

**Recursos externos gratuitos:** Groq API (plan gratuito) como IA principal y Pollinations.ai como respaldo.

---

## 6. Arquitectura y Despliegue con Docker

La aplicación se divide en 4 contenedores definidos en `docker-compose.yml`:

| Contenedor | Función |
| :--- | :--- |
| `proxy-nginx` | Proxy inverso con certificado SSL (puerto 443, HTTPS) |
| `frontend-web` | Interfaz web en HTML5, CSS3 y JavaScript |
| `backend-python` | API REST en Python y conexión con Groq API |
| `database-mysql` | Base de datos MySQL con volumen persistente |

**Pasos para levantar el proyecto:**

1. Clonar el repositorio y copiar `.env.example` a `.env`, rellenando la contraseña de MySQL, la clave secreta y la API Key de Groq.
2. Añadir al archivo `hosts` del sistema la línea:
   `127.0.0.1 fernandez-andujar-dario.proyecto-daw.iesabdera.local`
3. Ejecutar `docker compose up -d --build`.
4. Abrir en el navegador `https://fernandez-andujar-dario.proyecto-daw.iesabdera.local`.

---

## 7. Control de Versiones, Gestión en GitHub y Documentación

* **Ramas:** `main` (versión estable), `develop` (integración), `feature/*` (nuevas funcionalidades) y `fix/*` (correcciones). Los cambios se unen mediante Pull Requests vinculados a GitHub Issues.
* **Gestión:** tablero Kanban en **GitHub Projects**, tareas y errores en **GitHub Issues** y un **Milestone** por cada fase.
* **Documentación del proyecto:**
  * [README.md](./README.md) — Presentación, alcance, fases, cronograma y recursos.
  * [ESTUDIO_VIABILIDAD.md](./ESTUDIO_VIABILIDAD.md) — Justificación técnica, temporal, económica y legal.
  * [ANALISIS_RIESGOS.md](./ANALISIS_RIESGOS.md) — Riesgos, valoración P×I, prevención y contingencia.
  * *(Fase 5)*: `manual_tecnico.md` y `manual_usuario.md`.
* **Uso de IA:** si se usan herramientas de IA durante el desarrollo, se justificará su uso y se adjuntarán los prompts exactos.
