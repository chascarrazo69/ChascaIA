# Estudio de Viabilidad — ChascaIA

* **Proyecto:** ChascaIA — Asistente de ciberseguridad con inteligencia artificial
* **Autores:** Mario Andújar Gómez, David Fernández Hernández
* **Dominio:** `fernandez-andujar-dario.proyecto-daw.iesabdera.local`

---

## 1. Objetivo y Dictamen

Este estudio comprueba, antes de empezar a programar, si ChascaIA se puede realizar con los conocimientos, el tiempo y los recursos disponibles. Se analiza desde cuatro puntos de vista: **técnico**, **operativo y temporal**, **económico** y **legal y de seguridad**.

* **Dictamen final:** **PROYECTO VIABLE**. Todas las tecnologías elegidas son gratuitas, compatibles entre sí y adecuadas al alcance del proyecto, que puede completarse en las 5 fases previstas en el cronograma con un coste de **0,00 €**.

---

## 2. Viabilidad Técnica

Con autorización del profesor, el proyecto se desarrolla **sin frameworks**, usando directamente los lenguajes base. El equipo ya trabaja con ellos en los módulos del ciclo, lo que permite entender y controlar todo el código sin depender de herramientas que habría que aprender desde cero.

| Capa | Tecnología elegida | Alternativa descartada | Por qué se elige |
| :--- | :--- | :--- | :--- |
| **Frontend** | **HTML5 + CSS3 + JavaScript** (sin frameworks) | Angular + Bootstrap | Son los lenguajes que el equipo ya domina. HTML5 semántico estructura las páginas, CSS3 con *media queries* adapta la web a móvil, tablet y ordenador, y JavaScript valida los formularios y se comunica con el backend mediante `fetch`. Se evita la curva de aprendizaje de Angular y la web es más ligera. |
| **Backend** | **Python** (sin frameworks) | Flask / Django / FastAPI | Python incluye en su biblioteca estándar lo necesario para crear una API REST sencilla (`http.server`, `json`, `urllib`, `hashlib`). Solo se añade el conector `mysql-connector-python` para la base de datos. Así el equipo programa y entiende cada parte: rutas, sesiones, roles y llamadas a la IA. |
| **Base de datos** | **MySQL** | Guardar datos en ficheros | Base de datos relacional conocida y fiable. Las relaciones entre usuarios, roles y consultas se representan con claves foráneas, y los índices agilizan la carga del historial. |
| **Despliegue** | **Docker + Docker Compose** | Instalar todo a mano en el ordenador | Cada parte (web, backend, base de datos y proxy) va en su propio contenedor. Así el proyecto funciona igual en el ordenador de cualquier miembro del equipo y se arranca con un solo comando. |
| **Proxy inverso + SSL** | **Nginx + certificado SSL** | Apache / abrir los puertos directamente | Nginx es ligero y fácil de configurar como único punto de entrada por HTTPS bajo el dominio particularizado. Sirve los archivos HTML, CSS y JS, reenvía las peticiones `/api` al backend de Python, oculta los puertos internos y permite comprimir con Gzip. |
| **API externa (IA)** | **Groq API (plan gratuito)** + **Pollinations.ai** de respaldo | Entrenar una IA propia / APIs de pago | Groq ofrece modelos de lenguaje con un plan gratuito permanente sin tarjeta de crédito, con respuestas muy rápidas en JSON. Si se supera el límite, la API devuelve un error en lugar de cobrar. Pollinations.ai no requiere registro y sirve de respaldo si Groq falla. |
| **Gestión del proyecto** | **GitHub Projects + Issues + Milestones** | Hojas de cálculo | Reúne en un solo sitio el código, las tareas, los errores y los hitos de cada fase. |

---

## 3. Viabilidad Operativa y Temporal

* **Encaje en las 5 fases:** el trabajo se reparte según el cronograma del proyecto, dedicando la mayor parte del tiempo a la Fase 3 (Implementación).
* **El alcance encaja en el tiempo:** el proyecto tiene un único CRUD principal (Consultas de seguridad), 3 roles y una sola API externa. Es un alcance acotado que cabe en la fase de implementación.
* **Sin frameworks:** el equipo no tiene que aprender herramientas nuevas, pero sí programar a mano partes que un framework daría hechas (rutas, sesiones y control de roles). Por eso se empieza por lo básico (registro, login y consultas) antes que por las funciones opcionales.
* **Optimización:** en la Fase 4 se dedica tiempo a SEO básico con HTML5 semántico, carga diferida de imágenes (`loading="lazy"`), compresión Gzip en Nginx y caché.
* **Seguimiento:** el avance se controla cada semana con el tablero de GitHub Projects, los Issues (tareas y errores) y un Milestone por fase.
* **Uso por los usuarios:** la interfaz tipo chat es sencilla y está pensada para personas sin conocimientos técnicos, desde cualquier dispositivo.

---

## 4. Viabilidad Económica y de Recursos

El proyecto tiene un **coste total de 0,00 €** porque solo usa software libre y servicios gratuitos:

| Recurso | Herramienta | Modalidad | Coste |
| :--- | :--- | :--- | :---: |
| Contenedores y despliegue | Docker Engine + Docker Compose | Software libre | 0,00 € |
| Desarrollo | HTML5, CSS3, JavaScript, Python, VS Code | Software libre / estándares abiertos | 0,00 € |
| Base de datos | MySQL Community + `mysql-connector-python` | Software libre | 0,00 € |
| Servidor web y SSL | Nginx + OpenSSL (certificado local) | Software libre | 0,00 € |
| Repositorio y gestión | GitHub (Repositorios, Projects, Issues, Milestones) | Plan gratuito | 0,00 € |
| Inteligencia artificial | Groq API | Plan gratuito permanente | 0,00 € |
| IA de respaldo | Pollinations.ai | Gratuito, sin registro | 0,00 € |
| Hardware | Ordenadores propios del equipo | Ya disponibles | 0,00 € |
| **TOTAL** | | | **0,00 €** |

---

## 5. Viabilidad Legal, Seguridad y Uso de IA

* **Protección de datos (RGPD / LOPDGDD):** solo se guardan los datos imprescindibles (nombre de usuario, correo y contraseña). Las contraseñas nunca se guardan tal cual: se transforman con un *hash* seguro (`hashlib` con PBKDF2). El usuario puede borrar sus consultas en cualquier momento.
* **Datos enviados a la IA:** a la API externa solo se envía el texto de la consulta, nunca el nombre ni el correo del usuario. La web avisa de que no se deben escribir contraseñas reales ni datos personales en las preguntas.
* **Conexión segura:** todo el tráfico va cifrado por HTTPS gracias al certificado SSL de Nginx.
* **Seguridad programada a mano:** al no usar framework, el equipo aplica directamente las medidas básicas: consultas a MySQL con parámetros (contra inyección SQL), escapar el texto antes de mostrarlo en la web (contra XSS) y comprobar el rol del usuario en cada ruta del backend.
* **Claves secretas:** la contraseña de MySQL y la API Key de Groq se guardan en el archivo `.env`, que está en `.gitignore` y nunca se sube a GitHub. Solo se sube `.env.example` sin datos reales.
* **Uso responsable de la IA:** ChascaIA solo da consejos de seguridad **defensivos** (proteger, prevenir, detectar). Se le indica que rechace peticiones para atacar o acceder a sistemas ajenos. Las respuestas se muestran como **orientativas** y no sustituyen a un profesional.
* **Licencias:** todas las herramientas tienen licencias libres compatibles con un proyecto académico, y se respetan las condiciones de uso de Groq y Pollinations.ai.
* **Transparencia:** si se usan herramientas de IA durante el desarrollo, se justificará su uso y se adjuntarán los prompts exactos en la documentación final.
