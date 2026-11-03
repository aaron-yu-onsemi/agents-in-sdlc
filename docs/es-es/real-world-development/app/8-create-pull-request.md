---
title: "Lección 8 - Crea y fusiona la PR de la característica"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge para crear y fusionar la PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya fusionaste tú la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la característica, las actualizaciones de instrucciones y de la habilidad, y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás la PR completa de la característica y las pruebas de validación.
- autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Escenario

Durante el proceso de filtrado, has usado Copilot para planificar, implementar y validar una característica. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control del permiso de fusión en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo activas, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a establecer la base cuando hace falta. Después, fusiona la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque reinicies la aplicación y se desactiva cuando la PR se fusiona.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad, pero sigue necesitando tu autorización explícita para editar el código y fusionarlo. Revisa las acciones permitidas y el trabajo antes de concederle permiso para fusionar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora deja que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge, que hará lo siguiente:

- crear la solicitud de incorporación de cambios con un título y una descripción.
- incluir una referencia a la incidencia relacionada en el cuerpo de la descripción si has iniciado la sesión desde una incidencia.
- volver a establecer la base o resolver posibles conflictos de fusión con la rama de destino.
- supervisar el proceso de CI para asegurarse de que se completen correctamente todas las comprobaciones.
- supervisar la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot, y realizar cambios para responder a ellos.
- opcionalmente, fusionar automáticamente la PR cuando todo se haya completado correctamente.

Dejaremos que Agent Merge también fusione la PR cuando se completen correctamente todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de validación antes de conceder permiso para fusionarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que la casilla de **Merge pull request** esté marcada.
8. Pide expresamente al agente que fusione la solicitud:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el proceso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de fusión o un agente en ejecución no confirma que la PR se haya fusionado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora también el proceso de solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado la PR completa de la característica y las pruebas de validación.
- has autorizado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha fusionado.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
