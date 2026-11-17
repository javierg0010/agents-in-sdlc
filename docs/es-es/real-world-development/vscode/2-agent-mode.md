---
title: "Ejercicio 2 - Añade funcionalidad con el modo Agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo de una persona desarrolladora suele consistir en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a modificar el código... y así sucesivamente.

Aquí es donde entra en juego el modo Agente de Copilot.

El modo Agente está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a una persona desarrolladora: primero explora la estructura del proyecto, realiza las modificaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo puedes usar el modo Agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo Agente y el agente en la nube están diseñados para experiencias diferentes. El modo Agente trabaja en el IDE y permite interacciones y ciclos de comentarios rápidos. El agente en la nube trabaja de forma asíncrona como una persona del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo Agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- el modo Agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos;
- revisar los cambios y las pruebas generados por el modo Agente antes de fusionarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que las personas los filtren por categoría y editorial. En el ejercicio anterior ya creaste una función auxiliar para obtener editoriales; ahora completarás el resto del trabajo en la capa de datos, la interfaz de usuario y las pruebas con el modo Agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar juegos y sus detalles. Toda la aplicación está implementada con Astro y genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter, mediante Drizzle ORM. No hay una API de backend independiente ni un framework de interfaz de cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro con ámbito limitado y las API estándar del DOM.

### Inicia el sitio web

Para facilitar el inicio del sitio, se ha preparado la tarea `npm run dev`, que ejecuta el servidor de desarrollo Astro. Sigue estos pasos para iniciarlo en el codespace de GitHub:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsado <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace local en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página de inicio**: muestra la lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; los pasos correspondientes indicarán qué opción se necesita.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Al crear el taller, usamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias sean relativamente coherentes y la experiencia sea satisfactoria. Sin embargo, como los LLM son probabilísticos, es posible que recibas sugerencias distintas a las que se describen aquí. Es completamente normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot podría usar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía a Copilot este prompt para consultar las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según cuáles podrían resultar más útiles para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidad:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a archivos de instrucciones concretos para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar las pautas, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios independientes:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, antes de fusionar los cambios, hay que ejecutar las pruebas y comprobar que se superan. El modo Agente de Copilot puede encargarse de estas tareas. Vamos a añadir la función.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función de acuerdo con la incidencia correspondiente del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función. Debería encontrar tanto las funciones auxiliares de la capa de datos y la interfaz como las pruebas. Después, empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas e incluso fallos en algunas pruebas durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completa con el modo Agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva función.
11. Cuando hayas comprobado que funciona y hayas revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Enhorabuena. En este ejercicio has aprendido a usar el modo Agente de GitHub Copilot para añadir capacidades al sitio Tailspin Toys:

- el modo Agente puede implementar funciones en la interfaz de usuario y la capa de datos;
- puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- puedes revisar los cambios y las pruebas que genera antes de fusionarlos en el código.

Ahora [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Aplica lo que has aprendido para pedir a Copilot que actualice el sitio:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide cómo quieres implementar la paginación: por ejemplo, si permitirás que la persona elija el tamaño de página o si tendrá un valor fijo;
- proporciona a Copilot las indicaciones necesarias para que implemente la solución como quieres;
- quizá tengas que iterar con GitHub Copilot, pedir cambios y aportar contexto. Es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo Agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
