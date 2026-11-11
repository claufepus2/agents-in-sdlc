---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de Copilot en la nube que realice tres tareas diferentes para mejorar la experiencia de usuario y añadir funciones. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agente con solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de contraste alto, el equipo decidió que también era un buen momento para añadir un tema claro. Como ya se estaba trabajando en actualizar los estilos del sitio y añadir selectores, parecía lógico incluir esa función. Quieres orientar el trabajo del agente para que añada tanto el tema claro como el modo de contraste alto.

## Revisar las tareas del agente de Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - Actualizar la documentación del código base.
    - Generar API para modificar juegos.
    - Añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en ejecución. Revisa las tareas realizadas por Copilot. Pueden incluir:
    - Obtener el código del repositorio.
    - Crear el entorno de trabajo de Copilot.
    - Configurar servidores MCP.
    - Realizar varias tareas para completar el trabajo asignado.

> [!NOTE]
> Los pasos concretos que se muestran varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios (PR), que aparece a la derecha. Permite ver la PR y los archivos modificados para supervisar el trabajo.

## Orientar el trabajo del agente en la nube

Ya has visto las tareas activas. Ahora pediremos a Copilot que añada un selector de tema claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que Copilot haya elegido y del estado actual del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Supervisa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estuviera trabajando al recibir el mensaje.

El proceso tardará varios minutos, así que es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y orientan las tareas del agente de GitHub Copilot en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- cómo usar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- cómo redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, ahora [iteraremos sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
