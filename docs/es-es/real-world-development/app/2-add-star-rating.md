---
title: "Lección 2 - Añade valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, haz un pequeño cambio en las tarjetas de juegos y fusiónalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ahora que has explorado el espacio de trabajo y probado un chat rápido, es hora de iniciar una **sesión de agente** y hacer tu primer cambio en el proyecto. Será algo pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás en tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una sesión de agente y aprenderás cómo se estructura.
- pedirás al agente que haga un cambio pequeño y concreto en el proyecto.
- revisarás el cambio en la vista de diferencias del espacio de trabajo.
- ejecutarás la aplicación en local para comprobar el cambio en el navegador.
- abrirás y fusionarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, el editor y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión recibe un **árbol de trabajo y una rama de Git dedicados**, lo que permite ejecutar varias sesiones a la vez —por ejemplo, una para añadir una característica y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral agrupadas por repositorio; selecciona una para cambiar a ella.

Dentro de una sesión verás tres elementos: la **conversación** con el agente, la **actividad de las herramientas** del agente mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Inicia una sesión y solicita el cambio

Inicia una sesión nueva para explorar el proyecto e implementar la característica. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Crearás una sesión nueva para ese repositorio y solicitarás el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona el repositorio `tailspin-toys`.
4. Elige un **new working tree** y el modo **Interactive** debajo del cuadro de mensaje. Usa el mensaje siguiente para solicitar el cambio:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el mensaje a Copilot.

La aplicación Copilot empezará por crear un árbol de trabajo nuevo, una copia aislada del proyecto. Después, explorará el proyecto, localizará los archivos que debe actualizar para añadir la característica y creará el código necesario. Ya has añadido una característica nueva con la aplicación Copilot.

## Revisa las diferencias

Debes revisar todos los cambios generados por IA antes de fusionarlos, aunque sean pequeños. Vamos a explorar los cambios directamente en la aplicación Copilot.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

    ![Barra de herramientas superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías ver código añadido a `GameCard.astro`, el archivo principal que se usa para mostrar los detalles de los juegos. Debería ser parecido al siguiente: un bloque pequeño que muestra la valoración cuando existe y utiliza `No rating yet` cuando `starRating` es `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, ofrece resultados probabilísticos, no deterministas. El código exacto puede variar, pero debería ser relativamente parecido al ejemplo.

## Comprueba los cambios

Antes de abrir el navegador, revisa los resultados de las comprobaciones automáticas del agente. Confirma que las pruebas cubren una valoración numérica de `starRating` y el caso alternativo `null`. Si falta un requisito previo o se ha omitido una comprobación, no la des por superada; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que abra el sitio web para poder examinar la interfaz actualizada. Puede iniciar el sitio y abrirlo en un lienzo de navegador.

> [!TIP]
> Un lienzo es un widget interactivo integrado en la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero por ahora usarás el lienzo de navegador integrado.

1. Envía el mensaje siguiente para pedir a Copilot que inicie la aplicación y la abra en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de los juegos valorados muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abre y fusiona tu primera solicitud de incorporación de cambios

Ya has creado la característica. Ahora abre una solicitud de incorporación de cambios (PR) para fusionar el nuevo código en el proyecto.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las instrucciones para autenticarte.
3. Si Copilot indica que la directiva del repositorio exige vincular una incidencia antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona la burbuja **PR** situada justo encima del chat para abrir la solicitud en el panel de revisión. Allí puedes revisarla.
6. Cuando esté lista, selecciona **Ready to merge**.
7. Selecciona **Merge pull request** en el cuadro de diálogo para fusionar la solicitud.

## Resumen y próximos pasos

¡Enhorabuena! Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- has iniciado una sesión de agente y aprendido cómo se estructuran las sesiones.
- has indicado al agente que haga un cambio pequeño y concreto en las tarjetas de juegos.
- has revisado el cambio en la vista de diferencias del espacio de trabajo.
- has ejecutado la aplicación en local para confirmar la valoración por estrellas en el navegador.
- has abierto la PR 1, revisado sus comprobaciones y la has fusionado explícitamente.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una característica más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instala-y-configura-la-aplicación-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
