---
title: "Lección 9: Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier CLI de calidad, GitHub Copilot CLI incluye numerosos comandos de barra para interactuar con la herramienta. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- utilizarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

## Escenario

Has terminado el flujo principal de CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, opcionalmente, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta, y también puedes inspeccionarla o compactarla con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` otra vez y compara el resultado. Si la conversación todavía es corta, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Utiliza `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` cuando pases a una tarea independiente y quieras empezar una conversación nueva.

## Elige el modelo

Los distintos modelos tienen puntos fuertes diferentes y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar y seleccionar el modelo que quieres utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todo el mundo a mejorar el uso de las herramientas de IA. El comando `/share` permite exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado. Por ejemplo, `/share file` crea una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones de sesiones pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales, ya que pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa el resultado con el flujo habitual. Si quieres profundizar en el trabajo de agentes asíncronos, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y ver qué ocurre en segundo plano. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has utilizado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en el agente en la nube.

Hay más comandos de barra disponibles y más funciones de Copilot CLI por descubrir. Para cerrar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
