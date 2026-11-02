---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información previa, conviene que ese contexto esté disponible. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio e instrucciones específicas de rutas.
- generarás la primera parte de los datos para el filtrado (una función auxiliar para editores) con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volverás a enviar la misma indicación y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y la base de filtrado en `main` para que Copilot utilice las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del cambio de comportamiento tras actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

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

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de contexto relevante para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y otras directrices generales.
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

El proyecto inicial ya incluye varios archivos de instrucciones. Explorémoslos antes de hacer cambios para observar el efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Lee la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de la carpeta `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del frontmatter. Este define un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y las cabeceras de archivos.

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará lo necesario.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Después lo actualizarás y volverás a enviar la misma indicación para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe guardarse la función auxiliar.
3. En el desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta indicación:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todos los editores y el nombre y el id de cada uno. Aplica los cambios al archivo.
   ```

5. Copilot explora el proyecto y aplica cambios en el código, a menudo tanto en el archivo auxiliar como en sus pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y la cabecera de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se los hayas pedido. No pasa nada; el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un nuevo estándar al repositorio

Como ya hemos indicado, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Añadiremos ahí el estándar de programación para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación del proyecto, pero todavía no incluye documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes para pedir a Copilot que añada cabeceras de comentarios y comentarios TSDoc:

   ```markdown
   - Toda función exportada debe tener un comentario TSDoc que describa su propósito, parámetros y valor devuelto.
   - Antes de las importaciones o del código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden definirse para todo el repositorio en `.github/copilot-instructions.md` o para lenguajes, tipos de archivos y tareas concretos mediante `*.instructions.md`. El archivo de instrucciones del repositorio es el lugar adecuado para estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Vuelve a enviar la indicación y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **la misma indicación** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todos los editores y el nombre y el id de cada uno. Aplica los cambios al archivo.
   ```

4. Observa que ahora el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editores para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editores de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Acabas de orientar a Copilot para que siga un estándar nuevo y lo aplique al código real que utilizará el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan como cualquier otro archivo mediante el control de código fuente. Vamos a confirmar los cambios y publicar la rama.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, comprueba que los cambios en las instrucciones y la función auxiliar están presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos modificados por Copilot para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Después de publicar la rama, crea una solicitud de incorporación de cambios y relaciónala con la incidencia sobre los estándares de documentación del backlog. En vez de hacerlo manualmente, pediremos a Copilot que busque la incidencia, cree una PR que la cierre y, después, la combine.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hay que configurar nada. La primera vez que Copilot utilice una herramienta, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que el selector de agentes muestra **Agent**.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de archivos de instrucciones y cree una PR desde la rama actual. Indícale que incluya tanto las actualizaciones de instrucciones como la base de la función auxiliar para editores:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que la PR cierra esa incidencia y menciona que también hemos añadido la base de la función auxiliar de editores para el trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si procede.
6. Cuando se haya creado la PR, pide a Copilot que la combine y vuelva a la rama `main`:

   ```
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para tener el repositorio actualizado.
   ```

7. Si Copilot te pide permiso para realizar acciones de GitHub o ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de incorporación de cambios con la ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo utiliza Copilot las instrucciones del proyecto y has utilizado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.
- volver a enviar la misma indicación y observar cómo el código propuesto sigue el nuevo estándar.
- confirmar las actualizaciones de instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, pondrás en práctica estas instrucciones en el [modo agente][next-lesson], mientras Copilot añade una nueva funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
