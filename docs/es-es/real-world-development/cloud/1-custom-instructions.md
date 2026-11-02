---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información previa, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás una indicación *antes y después*: el agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Los archivos de instrucciones ayudarán a que Copilot disponga de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias para que entienda mejor tu estilo y requisitos de programación. Puedes especificar las convenciones, las bibliotecas y los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de contexto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`, archivos que puedes crear para tareas o tipos de archivos concretos. Permiten definir directrices para lenguajes como TypeScript o Astro, o para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones en otros formatos, como AGENTS.md y CLAUDE.md. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de este ejercicio, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como la descripción, la estructura y los estándares generales de programación.
- Utiliza archivos `*.instructions.md` para proporcionar directrices específicas de tipos de archivos (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Usa lenguaje natural y redacta directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería serlo.

No hay una única manera correcta de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar el enfoque más adecuado para el proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería contar con una colección sólida de archivos de instrucciones. Al explorar este proyecto, verás archivos para varios tipos de tareas, como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección del entorno que estés utilizando lo indicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica unos minutos a leer los archivos de instrucciones que incluye el repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Lee la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del frontmatter. Este define un patrón glob relativo a la raíz del repositorio para determinar a qué archivos se aplican las instrucciones; por ejemplo, a cualquier archivo de prueba TypeScript que coincida con `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir al generar código. Las rutas de esos enlaces son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y las cabeceras de archivos.

## Añade un nuevo estándar al repositorio

Ahora añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y una cabecera de archivo. El agente en la nube utilizará esta regla en las incidencias que le asignes más adelante.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero así podrás confirmar los cambios de forma ordenada:

1. En el terminal de tu codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añadiremos ahí el estándar de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero todavía no incluye documentación en el código.
3. Añade estas líneas de Markdown debajo de los estándares existentes para pedir a Copilot que añada cabeceras de comentarios y comentarios TSDoc:

   ```markdown
   - Toda función exportada debe tener un comentario TSDoc que describa su propósito, parámetros y valor devuelto.
   - Antes de las importaciones o del código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden definirse para todo el repositorio en `.github/copilot-instructions.md` o para lenguajes, tipos de archivos y tareas concretos mediante `*.instructions.md`. El archivo de instrucciones del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`, así que los cambios en las instrucciones deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá leer las instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama sirve para practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y siguientes pasos

Has explorado cómo utiliza Copilot el contexto de los archivos de instrucciones y has añadido un estándar general al repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que el agente en la nube genere durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
