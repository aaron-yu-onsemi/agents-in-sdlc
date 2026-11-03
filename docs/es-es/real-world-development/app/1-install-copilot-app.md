---
title: "Lección 1 - Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de hacer todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Con Node.js instalado y tu copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- te familiarizarás con el espacio de trabajo, incluido el backlog creado de antemano por la plantilla.
- probarás un chat rápido para conocer mejor la aplicación.

## Escenario

Tu equipo está empezando a usar agentes de IA para resolver un backlog cada vez mayor. La aplicación Copilot permite dirigir ese trabajo desde un solo lugar: elegir incidencias, ejecutar agentes, revisar cambios y fusionar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva de la **aplicación GitHub Copilot**.

## Instala y configura la aplicación GitHub Copilot

El primer paso para usar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Vamos a instalar la aplicación, autenticarnos y añadir el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si durante la configuración inicial se te pide que selecciones un repositorio, selecciona el repositorio Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadirlo después.
6. Cuando se te solicite, elige un tema y termina la configuración inicial.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no tienes que añadirlo otra vez.
8. Si aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Usa el nombre del propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En la disposición que se muestra aquí, las carpetas de repositorio, como `tailspin-toys`, están agrupadas en **Sessions** y las sesiones individuales aparecen debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza la información en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: tus solicitudes de incorporación de cambios, disponibles mediante la integración nativa con GitHub de la aplicación. Puedes explorarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: tus incidencias, disponibles del mismo modo. Puedes explorarlas y filtrarlas, e iniciar una sesión a partir de una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para administrar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar otras tareas repetitivas. El resumen final las presenta como siguiente paso, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. Lo usarás para configurar Playwright MCP.
- **Chats**: conversaciones sencillas para formular preguntas o plantear ideas sin crear una rama o un espacio de trabajo. Esta sección aparece después de iniciar el primer chat. Si aún no aparece, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y sus sesiones de agente. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a la conversación. Cada sesión se ejecuta en un espacio de trabajo aislado, así que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás la primera sesión al añadir valoraciones por estrellas.

Durante el taller explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede orientarte.

### Localiza el backlog creado de antemano

Probablemente todos los proyectos tengan trabajo pendiente, y Tailspin Toys no es una excepción. Vamos a explorar el backlog creado al generar el repositorio desde la plantilla.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título en lugar de dar por hecho su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también sirve como punto de partida para una sesión de agente. Partirás de la incidencia de filtrado después de completar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarse con la aplicación es usarla para aprender sobre la propia aplicación. Un **chat rápido** es justo lo que necesitas: puedes hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo, así que es ideal para consultas breves y desechables que no requieren una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no aparece, selecciona **+** junto a **Sessions** para iniciar tu primer chat; después aparecerá **Chats**.
2. Pregunta a la aplicación cómo funcionan sus sesiones:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra en cualquier momento.

## Resumen y próximos pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- usar un chat rápido para formular una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás la valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
