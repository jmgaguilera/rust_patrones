# Introducción

## Participación

Si estás interesado en contribuir a este libro, revisa las [recomendaciones de contribución](contribuir.md).

## Diseño de patrones

En el desarrollo de software es habitual que nos encontremos con problemas que comparten similitudes independientemente del entorno en el que aparecen. Aunque los detallas de implementación son determinantes para resolver la tarea que tengamos entre manos, podemos abstraernos de estas particularidades para encontrar aquellas prácticas habituales que se pueden aplicar de manera general.

Los patrones de diseño son colecciones de soluciones, probadas y reutilizables, a problemas recurrentes de ingeniería. Permiten que nuestro software sea modular, mantenible y extensible. Más aún, estos patrones proporcionan un lenguaje común a los desarrolladores, lo que los convierte en una herramienta excelente para que la comunicación de los equipos de trabajo sea efectiva durante la resolución de cualquier problema.

## Los patrones de diseño en Rust

Rust no es un lenguaje orientado a objetos. Tiene un conjunto de características que lo hacen único:

- Elementos funcionales.
- Sistema de tipos fuerte.
- Revisor de préstamos.

Por este motivo, los patrones de diseño de Rust son diferentes a los de otros lenguajes de programación tradicionales orientados a objetos. Por eso decidimos escribir este libro. ¡Esperamos que disfrutes de su lectura!

El libro está dividido en tres capítulos principales:

- [Idiomatismos (idioms)](./idiomatismos/indice.md): recomendaciones a seguir al codificar. Son normas sociales de la comunidad. Solo se deberían romper cuando se tenga una buena razón para ello.
- [Patrones de diseño](./patterns/indice.md): métodos para resolver problemas habituales de codificación.
- [Antipatrones](./anti-patrones/indice.md): métodos para resolver problemas habituales de codificación. Sin embargo, mientras los patrones de diseño aportan beneficios al código, los antipatrones crean más problemas que los que intentan resolver.
