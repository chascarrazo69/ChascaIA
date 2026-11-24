# Análisis de Riesgos — ChascaIA

* **Proyecto:** ChascaIA — Asistente de ciberseguridad con inteligencia artificial
* **Autores:** Mario Andújar Gómez, David Fernández Hernández

---

## 1. Metodología y Escala de Valoración (P × I)

Cada riesgo se valora multiplicando su **Probabilidad** (lo fácil que es que ocurra) por su **Impacto** (el daño que causaría si ocurre).

| Valor | Probabilidad (P) | Impacto (I) |
| :---: | :--- | :--- |
| 1 | Muy baja | Insignificante |
| 2 | Baja | Menor |
| 3 | Media | Moderado |
| 4 | Alta | Grave |
| 5 | Muy alta | Catastrófico |

**Nivel del riesgo = P × I**

* 🟢 **Bajo (1 – 4):** se acepta y se vigila.
* 🟡 **Medio (5 – 9):** requiere medidas de prevención durante el desarrollo.
* 🟠 **Alto (10 – 15):** prioritario; necesita prevención temprana y un plan de contingencia preparado.
* 🔴 **Crítico (16 – 25):** bloqueante; hay que resolverlo antes de pasar de fase.

---

## 2. Registro de Riesgos, Prevención y Contingencia

| ID | Área | Fase | Riesgo | P | I | Nivel | Prevención (antes) | Contingencia (si ocurre) | Responsable |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- | :--- |
| **R-01** | Arquitectura/Docker/Proxy | 3. Implementación | Fallos de conexión entre Nginx + SSL y los contenedores de MySQL, backend y frontend. | 3 | 4 | 🟠 **12 (Alto)** | Diseñar en la Fase 2 la red de Docker y usar los nombres de los servicios en `docker-compose.yml`. Probar pronto el dominio en el archivo `hosts`. | Revisar los registros con `docker compose logs`, comprobar la configuración de `proxy_pass` y el certificado SSL. | Equipo |
| **R-02** | Arquitectura/Docker/Proxy | 3. Implementación | Pérdida de los datos de MySQL al reconstruir o borrar los contenedores. | 2 | 4 | 🟡 **8 (Medio)** | Usar un volumen persistente para MySQL y guardar en el repositorio un script SQL con la estructura y datos de prueba. | Restaurar la base de datos desde el script SQL del repositorio. | Equipo |
| **R-03** | Desarrollo/API/Roles | 3. Implementación | La API de Groq no responde, cambia su formato o se supera el límite gratuito (error 429). | 3 | 4 | 🟠 **12 (Alto)** | Centralizar las llamadas a la IA en una única función de Python con tiempo máximo de espera y límite de consultas por rol. | Cambiar automáticamente a Pollinations.ai como respaldo y mostrar un aviso amable al usuario sin romper la web. | Equipo |
| **R-04** | Desarrollo/API/Roles | 3. Implementación | La IA da respuestas incorrectas o peligrosas sobre seguridad. | 4 | 4 | 🔴 **16 (Crítico)** | Dar a la IA instrucciones fijas para responder solo con consejos defensivos y sencillos, y mostrar un aviso de que las respuestas son orientativas. | Permitir al usuario reportar una respuesta, que el administrador la revise y ajustar las instrucciones de la IA. | Equipo |
| **R-05** | Recursos/Seguridad/IA | 3. Implementación | Uso malintencionado: usuarios que piden ayuda para atacar o entrar en sistemas ajenos. | 3 | 5 | 🟠 **15 (Alto)** | Indicar a la IA que rechace estas peticiones, aplicar límites de uso por rol y mostrar unas condiciones de uso. | El administrador revisa las consultas sospechosas desde el panel y bloquea la cuenta. | Administrador |
| **R-06** | Desarrollo/API/Roles | 4. Pruebas y validaciones | Un usuario accede a funciones de otro rol o un invitado supera su límite de consultas. | 3 | 4 | 🟠 **12 (Alto)** | Ocultar en JavaScript las opciones que no correspondan al rol y, sobre todo, comprobar el rol y el límite en cada ruta del backend de Python (el cliente no es de fiar). | Hacer pruebas con los 3 tipos de usuario en la Fase 4 y corregir los fallos mediante Pull Request. | Equipo |
| **R-07** | Recursos/Seguridad/IA | 3. Implementación | Subir por error el archivo `.env` con la API Key de Groq o la contraseña de MySQL a GitHub. | 2 | 5 | 🟠 **10 (Alto)** | Añadir `.env` al `.gitignore` desde el primer commit y subir solo `.env.example` sin datos reales. | Anular y generar al momento una nueva API Key y contraseña, y limpiar el historial del repositorio. | Equipo |
| **R-08** | Recursos/Seguridad/IA | 3. Implementación | Los usuarios escriben datos personales o contraseñas reales en sus consultas, que se envían a la API externa. | 3 | 4 | 🟠 **12 (Alto)** | Avisar en el chat de que no se escriban datos personales y no enviar nunca el nombre ni el correo del usuario a la IA. | El usuario puede borrar la consulta y el administrador puede eliminar consultas con datos sensibles. | Equipo |
| **R-09** | Planificación/GitHub | 3. Implementación | Retrasos y fallos de seguridad (inyección SQL, XSS, sesiones mal gestionadas) por programar sin framework partes que normalmente vienen hechas. | 4 | 3 | 🟠 **12 (Alto)** | Empezar por lo básico (login y consultas), usar siempre consultas con parámetros en MySQL, escapar el texto antes de mostrarlo y repartir tareas en GitHub Issues. | Reducir funciones opcionales (como el panel de estadísticas) y priorizar el CRUD, los roles y la API. | Equipo |
| **R-10** | Planificación/GitHub | 5. Documentación final | Falta de tiempo para redactar `manual_tecnico.md`, `manual_usuario.md` y el registro de prompts de IA. | 3 | 3 | 🟡 **9 (Medio)** | Controlar cada fase con GitHub Milestones y anotar los prompts de IA desde la Fase 1. | Dejar de añadir funciones al acabar la Fase 3 y escribir los manuales durante las pruebas. | Equipo |

---

## 3. Matriz de Riesgos 5×5 (Probabilidad × Impacto)

| Probabilidad \ Impacto | 1 (Insignificante) | 2 (Menor) | 3 (Moderado) | 4 (Grave) | 5 (Catastrófico) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **5 (Muy alta)** | 🟡 5 | 🟠 10 | 🟠 15 | 🔴 20 | 🔴 25 |
| **4 (Alta)** | 🟢 4 | 🟡 8 | 🟠 **12** *(R-09)* | 🔴 **16** *(R-04)* | 🔴 20 |
| **3 (Media)** | 🟢 3 | 🟡 6 | 🟡 **9** *(R-10)* | 🟠 **12** *(R-01, R-03, R-06, R-08)* | 🟠 **15** *(R-05)* |
| **2 (Baja)** | 🟢 2 | 🟢 4 | 🟡 6 | 🟡 **8** *(R-02)* | 🟠 **10** *(R-07)* |
| **1 (Muy baja)** | 🟢 1 | 🟢 2 | 🟢 3 | 🟢 4 | 🟡 5 |

**Prioridad:** el riesgo **R-04** es crítico y debe tratarse antes de terminar la Fase 3. Después van los altos: R-05, R-01, R-03, R-06, R-08, R-09 y R-07.

---

## 4. Seguimiento Continuo

Los riesgos se revisan al cerrar cada una de las **5 fases**:

1. Se comprueba si algún riesgo ha cambiado de probabilidad o impacto y se actualiza esta tabla.
2. Si un riesgo ocurre, se abre un **GitHub Issue** con la etiqueta `riesgo`, vinculado al **Milestone** de la fase.
3. La solución se hace en una rama `fix/*` y se cierra mediante **Pull Request**.
4. Los riesgos nuevos que aparezcan se añaden a este documento con el siguiente código (R-11, R-12...).
