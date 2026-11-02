---
title: "Lección 10: Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante todo el flujo de trabajo de Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- has añadido las valoraciones con estrellas, revisado el resultado en un navegador redirigido y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en Autopilot y revisado el resultado en Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la aptitud existente `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar los filtros en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la aptitud y las pruebas del navegador.
- has revisado el cambio completo de filtrado y autorizado Agent Merge para la PR.
- has explorado los comandos de barra para gestionar el contexto, los modelos, las sesiones compartidas y la delegación opcional en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en la versión actualizada de `main`:

1. **Valoraciones con estrellas:** mostrar la valoración existente `starRating` y un estado explícito para los juegos sin valorar.
2. **Flujo de filtrado y calidad:** implementar los filtros, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe `quality-checks`, crear un perfil de QA e incluir las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Para agilizar el taller, hemos incluido todo ese trabajo en una PR.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, inspeccionando el sitio en el navegador y dejando que Copilot lo explorara mediante MCP. La aptitud `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y generó un informe en el nuevo formato. Antes de la PR, QA reunió esos resultados con la revisión de requisitos y cobertura de pruebas.

Las pruebas nuevas deben cubrir carencias reales; si QA no necesita añadir pruebas, el resultado puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una aptitud, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares, las aptitudes describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para definir un enfoque antes de construir, **Interactive** para mantener el control de los cambios acotados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más funciones por explorar

Has recorrido el flujo principal. También puedes probar algunas funciones más de la CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
