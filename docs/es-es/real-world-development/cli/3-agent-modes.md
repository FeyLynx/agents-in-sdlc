---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios mayores requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso típico de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que funciona como se espera y, finalmente, la integrarás en el proyecto.

> [!NOTE]
> A medida que avances con la funcionalidad, utilizarás la misma conversación y rama. Normalmente se usarían ramas o PR diferentes para los distintos tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador redirigido.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la aptitud existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Utilizarás el modo Plan para resolver estas decisiones y, a continuación, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos. ¡Si acaso, los hace aún más importantes! La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan con lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y el proceso de integración continua haya finalizado correctamente.

> [!NOTE]
> Los detalles exactos variarán según el equipo y la organización, pero la mayoría de los procesos serán variantes del ejemplo anterior.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este propone cambios y espera a que le indiques cómo continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en los filtros, vuelve a tu Codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de la valoración con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

Observa que la indicación empieza ahora por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o PR de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de asignársela a Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo de la indicación muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe esta indicación:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot empezará a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo deben funcionar los filtros. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez escrito y revisado el plan, es el momento de implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite a Copilot iterar en la tarea hasta que considere que está terminada.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo de la indicación muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Como con cualquier otro código, hay que revisar el código generado antes de combinarlo. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación de los filtros y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Iniciaremos la aplicación como antes y abriremos el sitio mediante el puerto redirigido de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la función de filtrado en el navegador. Dime la URL y deja el servidor en marcha.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría y editor, así como las combinaciones acordadas en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en un navegador redirigido.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que sigue las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
