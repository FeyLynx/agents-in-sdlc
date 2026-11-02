---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tu equipo en tiempo real, Copilot Chat de VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que se producen. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con indicaciones de seguimiento para ampliar o ajustar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado un modo claro para las personas a quienes les resulte demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea, con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas. Verás lecturas de archivos, ediciones y los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, las diferencias aparecerán en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un control de modo claro. Como continuarás en la misma sesión, el agente conservará el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. En la entrada de Copilot Chat, envía esta indicación de seguimiento:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservar la preferencia en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el alto contraste de forma independiente.
    ```

2. El agente continuará desde donde lo dejó, revisará los cambios anteriores y propondrá nuevas modificaciones para el modo claro.
3. Revisa cada edición nueva cuando aparezca, igual que hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta refactorizar los controles de una forma que no te convence—, detenlo con el botón **Stop** junto a la entrada de chat y envía una indicación para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza indicaciones de seguimiento para solicitar ajustes («simplifica el CSS: utiliza variables en vez de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías encontrar cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Comprueba que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con el teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado las ediciones de archivos a medida que aparecían, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos concretas.
- has dirigido una sesión mientras estaba en curso mediante indicaciones de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que haya generado el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
