---
title: "El punto de partida de DDD: hablar el idioma del negocio"
author: Me
type: post
date:  2025-08-20T22:01:24-05:00
# optional
lastmod: 2025-08-20T22:01:24-05:00
cover:
  src: feature.jpeg
  caption: 
draft: true
categories:
  - Cuando el negocio habla y el código escucha, una serie sobre DDD
tags:
  - DDD
  - Software Architecture
  - Software Design
description: Read this interesting post, it's totally worth it.
---
## Tabla de Contenidos

- [Introducción](#introducción)
- [Entender qué hacemos](#entender-que-hacemos)
- [La importancia del lenguaje](#la-importancia-del-lenguaje)
- [Poniendo límites a nuestro lenguaje](#poniendo-l%c3%admites-a-nuestro-lenguaje)
- [Conclusión](#conclusión)
- [Recursos](#recursos)


## Introducción

Hace algunos meses me encontraba presentando una iniciativa a las diferentes áreas interesadas en esta: experiencia, negocio, seguridades, riesgos, canales de atención, legal. Para mí, esta funcionalidad no representaba un cambio significativo en la arquitectura del sistema, era una variante sobre un flujo ya existente.

Me sorprendió descubrir que solo para mí no era algo nuevo. Para las otras áreas, esto sí representaba una nueva funcionalidad que tenía sus propias reglas de negocio y restricciones.

Ante esto traté de explicar que: *la arquitectura de **máquina de estados** implementada permitía guardar **pasos** sin alterar el estado de las **bases de datos** y que al final los **registros** tenían **esquemas** similares.*

¿Pueden ustedes identificar mi error? Yo tampoco lo vi en su momento hasta que alguien del área legal me dijo *¿Qué es una máquina de estados?*. En ese momento comprendí que la mayoría de términos que usé no los conocía gran parte de mi audiencia. Eso hacía que no estuviéramos alineados en lo que se iba a construir.

Reflexionando sobre este tema regresé a ver a mi arquitectura y me di cuenta que su estructura representaba claramente el patrón técnico que utilizábamos. Pero las reglas de negocio se diluían en el código y era difícil identificarlas. En pocas palabras, nuestro centro no era el negocio, era la tecnología.

Este fue mi punto de partida hacia Domain-Driven Design (DDD). En esta serie de blogs les compartiré mi aprendizaje sobre este tema y cómo puede aportar a construir soluciones centradas en el negocio, no en la tecnología.

## Entender que hacemos

Como desarrolladores y líderes técnicos nuestra área de confort es la creación de software, somos buenos en ello. Pero por un momento nos hemos puesto a pensar en qué hacemos. No me refiero si habilitamos un servicio “REST” o estamos implementando un “Dead Letter Queue”. Lo que pregunto es  ¿conocemos qué hace nuestro sistema? ¿por qué la empresa considera que es importante lo que estamos construyendo? ¿cuál es el giro de negocio? ¿cómo aportamos valor?

> “El corazón del software es su capacidad para resolver problemas relacionados con el dominio de un usuario.”
> 
> 
> — Eric Evans, *Domain-Driven Design*
> 

Tener claro qué hace el negocio y cuál es su razón de ser nos permitirá construir software que aporte a ese objetivo y no piezas costosas de software que nadie las utiliza.

Una forma útil para identificar el “qué” de nuestros sistemas es a través de las conversaciones que se tienen con las diferentes áreas que interactúan con el sistema de forma directa o indirecta. 

Te planteo este ejercicio. En tu siguiente conversación con algún área diferente a la de desarrollo busca identificar:

1. Qué acciones describen tu sistema (Ejemplo: Usuario agrega orden a carrito de compra, Cliente realiza una transferencia)
2. Dentro de esas acciones señala los términos comunes utilizados (Ejemplo: Carrito, Compra, Transacción, Transferencia)
3. Qué restricciones o reglas se comentan (límite de compra, máximo valor de transacción)

Esto te dará unas pistas no solo de qué hace tu sistema, sino lo que los otros piensan que hace tu sistema. Y lo más importante, reconocerás el siguiente desafío que necesitas afrontar. 

Alinear el lenguaje.

## La importancia del lenguaje

Si eres de Hispanoamérica te habrás percatado que una misma palabra puede significar cosas diferentes dependiendo de la región. Por ejemplo “guagua” en Ecuador significa bebé mientras que en países como Chile o Cuba significa autobús, no existe ninguna relación entre los dos significados.  

Esto mismo ocurre con nuestros sistemas, por ello antes de lanzarnos a construir soluciones es importante **alinearnos en el lenguaje**. Y así evitar que terminemos en una torre de Babel donde cada quien entiende o infiere lo que quiere y el resultado final no representa a nadie.

Pongamos un ejemplo de un “lenguaje común” exitoso, HTML. Este lenguaje de marcado creado por Tim Berners Lee generó un esquema para presentar páginas en Internet. Su éxito es tal que no importa el país, las creencias, el nivel técnico, todos quienes interactuamos con la web sabemos que es un `<h1>` o `<p>`.

De este último párrafo me gustaría que nos quedemos con la frase **“todos quienes interactuamos con”.**

Un Lenguaje Universal no existe. Volviendo a mi ejemplo de HTML, tal vez un abogado no sabe qué es una marca **<p>,** pero ¿eso importa?, no. Sin embargo, si el abogado desea implementar su página web y se aventura a hacerlo por sí solo deberá aprender el lenguaje de la web, en donde la marca `<p>` define un párrafo.

> Eric Evans define a este conjunto de conceptos y términos que evolucionan juntos dentro de un contexto delimitado como **Ubiquitous Language.**
> 

De la definición de Eric Evans me gustaría rescatar la última parte **“contexto delimitado”**. Que exista ambigüedad en lenguaje no es un problema como tal. De igual manera que podemos usar guagua mientras estemos en Ecuador para referirnos a un bebé. Es importante identificar el contexto sobre el cual se desenvuelve nuestro sistema y definir límites alrededor de él. 

De esta manera nuestro lenguaje nos permitirá describir los problemas y acciones que son relevantes para nuestra área del negocio y sus interesados

## Poniendo límites a nuestro lenguaje

Regresemos un momento al ejemplo de HTML, en una frase anterior comentaba que **“todos quienes interactuamos con la web sabemos”**

De esta expresión podemos conocer el contexto general en el cual nos estamos desenvolviendo, la web.

¿Quiénes podrán estar interesados en la web? La respuesta puede ser muy amplia. Eso no nos ayuda a construir un lenguaje común. Pero qué tal si replanteamos la pregunta en ¿Quiénes están interesados en cómo publicar información en internet a través de páginas?

En ese contexto vamos a encontrar acciones comunes:

- Escribir un artículo
- Dar formato al contenido
- Insertar enlaces, multimedia o componentes interactivos
- Configuración de SEO

También podemos definir nuestros actores:

- Desarrolladores web
- Autores
- Diseñadores web
- Especialistas en Marketing

Finalmente vamos a descubrir términos comunes como:

- Página HTML
- Hipervínculo
- SEO
- Estilo

En este contexto se puede explicar la razón de ciertas etiquetas de HTML:

- `<h1>` y `<p>` nos ayudan a definir títulos y párrafos
- `<a>` hace referencia a hipervínculos
- `<style>` a la capacidad de añadir estilos a nuestras páginas

Dentro de un contexto general muy amplio (la web) hemos puesto límites basados en las acciones que queremos realizar en la web (publicar información), de esta manera ha sido más fácil identificar las capacidades de nuestro sistema y sus involucrados.

> Eric Evans define al entorno en el cual se desarrollan nuestros sistemas como **dominio** , y las capacidades específicas en las que se divide como **subdominios**.
> 

Este ejercicio lo podemos hacer nosotros con nuestros sistemas, por ejemplo. Si trabajamos para una empresa que vende artículos por internet podemos decir claramente que estamos en el dominio de “E-commerce”. Pero en qué área nos desenvolvemos: pagos, entregas, marketing, catálogo. Cuando tengamos esto identificado comenzaremos a ver que tenemos términos que son propios de nuestro subdominio como “transacción” para pagos o “alcance” para marketing. Incluso términos que se utilizan entre subdominios cómo “orden” que puede significar una cosa para un pago y otra para una entrega.

## Conclusión

Construir sistemas es complejo y mucho más cuando existen una desconexión entre los participantes de un proyecto. Para nosotros, como personas de tecnología, esa desconexión es un riesgo.  Significa construir sistemas centrados en las herramientas que usamos y no en los problemas del negocio que queremos resolver. 

Para ello es importante detenerse un momento e identificar dónde estamos parados, cuál es el dominio de nuestro negocio y en cuál subdominio están mis soluciones. Para así trabajar en construir un lenguaje que permita al sistema representar el negocio y alinear a los interesados.

Aquí lo dejamos por ahora. En la siguiente parte explicaremos cómo estos límites se convierten en contextos claros que guían la arquitectura: los Bounded Contexts.

## Recursos

- https://app.pluralsight.com/library/courses/clean-architecture-patterns-practices-principles/table-of-contents
- https://learning.oreilly.com/library/view/domain-driven-design-tackling/0321125215/
