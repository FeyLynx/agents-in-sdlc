---
title: "Lección 2: Añade las valoraciones con estrellas, una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador redirigido y combínalo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y has probado una conversación, es el momento de hacer tu primer cambio en el proyecto. Será un cambio pequeño: los juegos ya tienen una valoración con estrellas en sus datos, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot en una rama de funcionalidad.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para confirmarlo en un navegador redirigido.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama que tengas seleccionados en el terminal, en lugar de crear un árbol de trabajo específico. Las conversaciones guardadas te permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen formando parte del estado normal de Git en el disco.

En una conversación verás tres elementos: tus indicaciones y las respuestas del agente, la actividad de las herramientas que utiliza el agente para explorar y editar archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Vamos a iniciar una nueva conversación para implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si todavía no está abierto, inicia Copilot con el comando siguiente:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa la siguiente indicación para solicitar el cambio:

   ```plaintext
   Muestra `starRating` sobre 5 en cada tarjeta de juego de la página de lista. Si la valoración es `null`, muestra "No rating yet". Conserva el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una nueva funcionalidad con Copilot CLI!

## Revisa las diferencias

Todos los cambios generados por IA deben revisarse antes de combinarlos, por pequeños que sean. Vamos a inspeccionar los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Comprueba que la tarjeta del juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Comprueba que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que solucione cualquier fallo.
5. Cuando hayas terminado la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Como Copilot, igual que todas las herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Vamos a pedir a Copilot que inicie el sitio web para poder examinar la interfaz de usuario (UI) actualizada en el navegador redirigido por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de la valoración con estrellas. Dime la URL y deja el servidor en marcha.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Comprueba que las tarjetas de juegos muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora crearás una solicitud de incorporación de cambios (PR) para combinar el código nuevo con el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de la valoración con estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará la URL de esta.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad.
- has indicado al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- has revisado el cambio con `/diff`.
- has ejecutado la aplicación para confirmar la valoración con estrellas en un navegador redirigido.
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad mayor.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
