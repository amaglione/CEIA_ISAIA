# Trabajo Práctico Bad UI - IISAIA
**Materia:** Introducción al Desarrollo de Software Asistido por IA
**Carrera:** Especialización en Inteligencia Artificial (CEIA - FIUBA)
**Autor:** Agustin Maglione

## Descripción del Proyecto
Este proyecto es una implementación de una **Bad UI**. El objetivo es transofrmar una interfaz clasica y sencilla, como seleccionar una fecha, en una experiencia frustante para el usuario. 

Inspirado en la comunidad `r/badUIbattles`.

## Características de la solución
Para cumplir con el objetivo, se implementaron las siguientes funcionalidades:
* **Selección Compleja:** El método para elegir día, mes y año no sigue patrones convencionales, dificultando una tarea que debería ser sencilla.
* **Restricción Arbitraria:** Solo una combinación de fecha específica está habilitada para continuar.
* **Botón de Reset Evasivo:** El botón para reiniciar el ejercicio es físicamente difícil de cliquear.

## Restricciones Técnicas (Constraints)
El desarrollo se ajustó estrictamente a las reglas de la consigna:
* **Archivo Único:** Todo el código (HTML, CSS y JavaScript) reside en un solo archivo `index.html`.
* **Cero Dependencias:** Sin build tools, sin archivos externos y sin librerías de terceros.
* **IA-Assisted:** Construido íntegramente utilizando una sola conversación en **ChatGPT Canvas**.

## Análisis del Proceso (Qué funcionó y qué no)

### Lo que funcionó:
Se logró una interfaz es "fea" pero con un codigo que funciona. Como indica la consigna: "el código tiene que andar sin romperse; si rompe, es un bug, no bad UI".

### Desafíos y lo que no funcionó:
La principal dificultad técnica fue programar la lógica de los selectores de días, meses y años para que fueran difíciles de usar pero que mantuvieran coherencia interna (ej. que no se rompa la validación al cambiar de año).

---

## Estructura del Entregable
Este repositorio contiene los tres archivos requeridos:
1.  `index.html`: El artefacto funcional (single-file).
2.  `prompts.md`: Registro ordenado de la secuencia de prompts y la lógica detrás de cada uno.
3.  `README.md`: Este documento descriptivo.
