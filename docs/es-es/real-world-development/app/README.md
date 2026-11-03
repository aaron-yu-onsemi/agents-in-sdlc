---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un espacio de trabajo específico. Añade sesiones de agente en paralelo, modos de sesión intercambiables, lienzos compartidos y administración nativa de incidencias y solicitudes de incorporación de cambios de GitHub, incluido **Agent Merge**, que guía una solicitud durante los cambios de base, los comentarios de revisión, las correcciones de integración continua (CI) y la fusión.

El taller sigue un flujo de trabajo continuo en Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y el backlog creado de antemano.
2. Haz un cambio concreto en la valoración por estrellas, revisa el resultado en el navegador y fusiona manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, impleméntalo en modo **Autopilot** y revísalo en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de validación.
8. Revisa todos los cambios de filtrado y usa Agent Merge para la segunda PR.
9. Comprueba si hay lienzos disponibles y, después, usa Database Explorer para crear y probar un lienzo de clasificación respaldado por el repositorio. Si no están disponibles las funciones necesarias, registra el bloqueo y usa la alternativa de solo lectura o continúa con el resumen.

Para simplificar el taller, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas. Empieza cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una sesión, un árbol de trabajo y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta. El ejercicio final del lienzo permanece en su propia sesión para que puedas centrarte en crear y probar la interfaz compartida en lugar de repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instala la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añade valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y la alternativa para null; después, fusiona la PR 1 |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la característica a partir de una incidencia, créala con Autopilot y revísala en Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personaliza y usa la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Pruebas en navegador | Configura MCP desde Customize e inspecciona el comportamiento del filtrado |
| [7. Crea y usa un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne pruebas de validación |
| [8. Crea y fusiona la PR de la característica][ex8] | Revisión y fusión | Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, usa Agent Merge para la segunda PR |
| [9. Explora y crea lienzos][ex9] | Colaboración | Usa Database Explorer y, después, crea y prueba un lienzo de clasificación respaldado por el repositorio |
| [10. Resumen y próximos pasos][ex10] | Repaso | Repasa el flujo de trabajo, los recursos creados y otros recursos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un equipo con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el equipo

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y Copilot CLI que se usan en este taller, así que permite completar todos los recorridos.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu equipo y no en un Codespace, el [ejercicio de requisitos previos][ex0] explica cómo instalar Node.js y crear tu copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva de la **aplicación GitHub Copilot**. Es una directiva independiente de la de Copilot CLI.

## Empezar

**[Empieza con los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
