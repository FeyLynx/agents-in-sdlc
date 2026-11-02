---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Has pedido a Copilot que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar e implemente funciones de accesibilidad con modos de alto contraste y claro. Vamos a explorar los cambios y, si hace falta, proporcionar comentarios a Copilot para mejorar su trabajo.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian con la llegada de la IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las propuestas de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de GitHub Copilot en la nube realiza sus tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que el proceso sea seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) generada por el agente de GitHub Copilot en la nube: añadir documentación al código. Para ello, utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que aparezca una advertencia indicando que un firewall ha bloqueado a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de GitHub Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine; puede tardar unos minutos.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba que se han añadido comentarios TSDoc, que se siguen las convenciones de TypeScript y que hay comentarios en las cabeceras de los archivos. Estas indicaciones proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso indicando que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Captura de pantalla del botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es una interacción unidireccional. Puedes mencionar a Copilot en comentarios de la solicitud, igual que harías con otras personas del equipo, o añadir comentarios en línea al código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos indicar un texto exacto. Algunas ideas de cambios que puedes solicitar:

- añadir en la parte superior de cada archivo de código un comentario breve que explique su función.
- añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Utiliza una de las ideas anteriores u otra sugerencia que quieras aplicar al código.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marca la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la PR generada por Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un aviso indicando que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Captura de pantalla del botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ha creado la función de juegos relacionados! Como antes, puedes iterar con el agente en la nube para solicitar cambios. Por ejemplo, puedes pedirle que modifique cuántos juegos relacionados se muestran o que añada comentarios en las cabeceras y documentación TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Para ello, añade otro comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el panel de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la solicitud tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para cambiar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado los cambios y te parezcan correctos, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso indicando que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Captura de pantalla del botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan como esperas y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o una posible mejora, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, como en las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar tareas reales del backlog mientras te ocupas de otras cosas. Para ponerlo en práctica, registra otras incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar interés en la página de detalles de un juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para comprobar su efecto en el resultado del agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como panel de control** para supervisar y dirigir la sesión de accesibilidad mientras se ejecutaba.
- **Has revisado las solicitudes de incorporación de cambios del agente e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir aprendiendo, puedes complementar lo que has practicado con otros itinerarios:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal mediante los modos Plan y Autopilot, aptitudes de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para configurar el flujo de trabajo del agente de GitHub Copilot en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de GitHub Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
