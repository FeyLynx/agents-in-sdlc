---
title: "Ejercicio 2: Añade funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Un desarrollador normalmente localiza todos los archivos necesarios, realiza los cambios, ejecuta las pruebas, depura los errores, averigua qué archivo falta, vuelve a cambiar el código... La lista continúa.

Para eso sirve el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos rápidos de comentarios e interacción. El agente en la nube actúa como un compañero de programación y trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot explora el proyecto, identifica los archivos pertinentes y coordina cambios.
- el modo agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos con el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que las personas filtren por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para editores. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas mediante el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter, mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API estándar del DOM.

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace localhost en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en marcha, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo necesario.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. En el desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Al crearlo utilizamos Claude Sonnet 4.5 y por eso aparece en las instrucciones. Esperamos que las sugerencias de código sean relativamente coherentes y ofrezcan una buena experiencia. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden diferir de los ejemplos. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog enviando esta indicación:

   ```plaintext
   Muéstrame el backlog de tareas de mi repositorio de GitHub. Ayúdame a priorizarlas según cuáles serán más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las tareas según su utilidad para las personas que utilizan el sitio.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, pruebas y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivos, entre ellos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en archivos manejables e incluir ejemplos y plantillas.

## Implementa los filtros

Para completar el filtrado, habrá que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas. Añadamos la funcionalidad.

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que el modelo seleccionado sigue siendo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para incluir filtros por editor y categoría según los requisitos de la incidencia relacionada del backlog de GitHub. Ya existe una función auxiliar para editores del ejercicio anterior; consérvala y mejórala si hace falta al completar el trabajo restante en la capa de datos, la interfaz y las pruebas. Comprueba que se superan todas las pruebas antes de terminar. El servidor ya está en marcha, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar las funciones de la capa de datos, la interfaz y las pruebas, y después empezar a modificar los archivos y ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar varios minutos, según la complejidad de la tarea y del código. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que fallen algunas pruebas durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. Hemos visto cómo:

- el modo agente de GitHub Copilot implementa nuevas funciones en la interfaz y la capa de datos.
- el modo agente explora el proyecto, identifica los archivos pertinentes y coordina cambios.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos con el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Con lo que has aprendido en este ejercicio, pide a Copilot que añada paginación al sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación y si quieres que el usuario pueda seleccionar el tamaño de página o prefieres fijarlo.
- redacta una indicación que proporcione a Copilot las directrices necesarias para implementar la solución deseada.
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Ese es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
