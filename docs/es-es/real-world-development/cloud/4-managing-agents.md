---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma asíncrona y autónoma, también es importante poder supervisar estas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. Puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería buen momento para añadir un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo controles de cambio de modo, parece razonable incluir esta funcionalidad. Quieres dirigir el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Entre ellas puede haber:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) que aparece a la derecha. Desde allí puedes ver la PR y los archivos modificados para supervisar mejor el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, vamos a pedir a Copilot que añada el control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto dependerá del nombre que utilice Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade esta indicación:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot confirma la indicación y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para hacer una pausa y repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente de GitHub Copilot en la nube. Puedes hacer seguimiento de todas las tareas activas y completadas, revisar el trabajo realizado e incluso redirigir las tareas en curso para ajustar su alcance o proporcionar indicaciones adicionales.

Has aprendido:

- a utilizar el panel de control y la página de agentes para supervisar las tareas del agente en la nube.
- a redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
