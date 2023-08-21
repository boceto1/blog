---
title: "Problema Longest Substring Without Repeat Characters"
author: Me
type: post
date:  2023-08-18T17:19:36-05:00
# optional
lastmod: 2023-08-18T17:19:36-05:00
cover:
  src: feature.jpg
  caption: Imagen descriptiva del problema "The longest substring without repeat characters"
draft: true
categories:
  - Ciencias de la Computación
  - Algoritmo y Estructura de Datos
description: En este post exploramos los diferentes opciones que tenemos para resolver el problema "The longest substring without repeat characters"
math: katex
---

## Tabla de Contenidos
- [Introducción](#introducción)
- [Descripción del Problema](#descripción-del-problema)
- [Solución Fuerza Bruta](#solución-fuerza-bruta)
- [Solución Lineal](#solución-lineal)
- [Solución Lineal con tiempo constante](#solución-lineal-con-tiempo-constante)
- [Conclusión](#conclusión)
- [Aprendizajes y Reflexiones](#aprendizajes-y-reflexiones)
- [Recursos](#recursos)


## Introducción


## Descripción del Problema

La descripción del problema que tenemos de LeetCode es el siguiente:

> Dado un string `s`, encontrar el tamaño del substring más extenso que no repita caracteres.

El problema viene con los siguientes ejemplos:

```
Ejemplo 1
Entrada: abcabcbb
Salida: 3
Explicación: "abc" es uno de los substring más extenso.

Ejemplo 2
Entrada: bbbbb
Salida: 1
Explicación: "b" es uno de los substring más extenso.

Ejemplo 3
Entrada: pwwkew
Salida: 3
Explicación: "wke" es uno de los substring más extenso.
```

## Solución Fuerza Bruta
Una forma inicial de atacar este problema es la siguiente:

1. Definimos un variable llamado `maxLength` el cual nos ayudará a guardar el valor del máximo string.
2. Tomamos un elemento del arreglo y comenzamos a construir substrings, ejemplo "a" -> "ab" -> "abc".
3. Cada valor del substring es almacenado en una estructura Set, siempre y cuando el valor no haya sido previamente almacenado.
4. Caso contrario verificamos si el tamaño del substring es el más extenso de los registrados previamente y reiniciamos el set.
5. Al final, retornamos el valor `maxLength`.

Para este ejercicio necesitamos utilizar dos for loop aninados.

La implementación sería la siguiente:

```javascript
const maxSubstrings = (string) => {
  let characters = new Set();
  let maxLength = 0;

  for (let i=0; i < string.length; i++) {
      for (let j=i; j < string.length; j++) {
         if (characters.has(string[j])) {
              break;
         } else {
           characters.add(string[j]);
         }
      }
      maxLength = Math.max(characters.size, maxLength);
      characters.clear();
  }
 
 return maxLength;
}

  console.log(maxSubstrings('abcabcbb')); // 3
  console.log(maxSubstrings('bbbbb')); // 1
  console.log(maxSubstrings('pwwkew')); // 3
  console.log(maxSubstrings('bcdefefihkb')); // 6
```

Al momento de ejecutar el algoritmo vemos que practiamente recorremos dos veces por cada valor, cuando es el caracter inicial del substring o cuando es parte de otro. Esto hecho nos índica que lo complejidad de tiempo del algoritmo es $O(n^2)$. Otra manera de verlo es que al momento de utilizar dos loops aninados estamos utilizando una complejidad cuadrática.

En el caso del análisis de espacio vemos que en el peor escenario (que todos los caracteres sean iguales), necesitamos almacenar todos los elementos en el set. Hecho por el cual la complejidad de tiempo es $O(n)$.

## Solución Lineal
Cómo hemos comentado en otros casos, un algoritmo de complejidad cuadrática no es óptimo. Exploremos otra opción.

El problema con la primera solución es que para la construcción del substring leemos cada valor al menos dos veces, lo podemos evitar?

Por ejemplo si tomamos el string `bcdefefihkb`, el primer substring que podemos armar es `bcdef`. Es necesario evaluar valores para c,d,e,f. La respuesta es no, porque terminará la construcción en el mismo elemento, la segunda `e` del arreglo. Si queremos no ser rendundantes, deberíamos comenzar a construir el siguiente substring en la primera letra `f` del arreglo.

Cómo lo logramos? Analizemos el siguiente enfoque.

1. Definimos un variable llamado `maxLength` el cual nos ayudará a guardar el valor del máximo string. Pero también dos indices: `leftIdx`, `rightIdx`. Con `rightIdx` construimos los substring cómo la haciamos en la solución 1. Sin embargo, `leftIdx` nos ayudará a determinar el punto inicial para la construcción del siguiente substring. El cuál debe ser una posición más a la del elemento repetido.
2. Cómo saber las posiciones de los elementos? Utilizamos un HashMap en lugar de un Set para almacenar el valor y su posición.
3. Evaluamos cada valor, si no es parte del HashMap lo agregamos con su posición.
4. Si ya es parte del hash map, en primer lugar calculamos el length de ese string (restando rightIdx - leftIdx) y verificamos si es mayor a `maxLength`. Después movemos `leftIdx` hasta la posición del elemento repetido, borrando su posición del hashMap. Guardamos la nueva posición del elemento repetido y ubicamos `leftIdx` una posición más que este.
5. Repetimos el proceso hasta que `rightIdx` sea menor el tamaños del arreglo.
6. Devolvemos el varlo de `maxLength`.

Veamos la implementación en javascript del algoritmo

```javascript
const isUndefined = (value) => value === undefined;

const maxSubstrings = (string) => {
  const characters = {};
  let leftIdx = 0;
  let rightIdx = 0;
  let maxLength = 0;

  while (rightIdx < string.length) {
    const character = string[rightIdx];
    if (character in characters && !isUndefined(characters[character])) {
      maxLength = Math.max(maxLength, rightIdx - leftIdx);
      const repeatedPos = characters[character];

      while (leftIdx <= repeatedPos) {
        const toClearChar = string[leftIdx];
        characters[toClearChar] = undefined;
        leftIdx++;
      }
      characters[character] = rightIdx;
    } else {
      characters[character] = rightIdx;
    }

    rightIdx++;
  }
  return  Math.max(maxLength, rightIdx - leftIdx);
}
```

_Nota_ Es importate acotar que el substring máximo se dé con los valores al final del string, por lo que es necesario evaluar cuál valor es el máximo también al final.

Si hacemos el análisis vemos que igual tenemos dos loops aninados, cuál es la diferencia? El índice `leftIdx` se va a mover máximo d veces, correspondiente el valor del substring máximo, evitando así movimientos redundantes. La complejidad sería la siguiente $O(n+d)$, siendo d el valor máximo de un substring.

En el caso de espacio se mantiene el valor de la primera solución, dado que en el peor escenario tenemos que guardar todos los valores en el HashMap, el resultado sería $O(n)$.

Este resultado podría ser nuestra respuesta final, verdad? Pues no, les propongo la siguiente solución dónde podemos mejorar la complejidad de tiempo.

## Solución Lineal con tiempo constante
Un aprendizaje que he tenido durante este tiempo resolviendo problemas de algoritmia es que un problema siempre tiene datos ocultos. Estos datos pueden contribuir a que la complejidad de nuestras respuestas disminuya y por ende nuestras soluciones sean las más óptimas.

Una de las reglas al momento de hacer análisis BigO es que las constantes se eliminan por ejemplo:

* $O(n) + O(n) + O(n) = 3*O(n) = O(n)$
* $256 + 0(n) = O(n)$
* $O(3) = O(1)$

Entonces, teniendo esto en cuenta que valor constante tenemos en nuestro problema que podemos utilizar. Pensemos ....

Cuántos caracteres están definidos en el código ASCII? 256 incluyendo caracteres estándar y extendidos. Si nuestro problema utiliza entradas basadas en este formato significa que el valor máximo que puede tener un substring sin que se repitan valores es 256

_Nota:_ El valor 256 puede cambiar dependiendo del formato de texto que usemos, pero siempre será un valor constante. 

En base a este razonamiento, la complejidad de la solución anterior es la siguiente.

* Complejidad de tiempo: $O(n+d) = O(n + 256) = O(n)$
* Complejidad de espacio: $O(d) = O(1)$

De esta manera la segunda solución, en realidad tenía una complejidad de tiempo lineal y una complejidad de espacio constante. Es en verdad una muy buena solución, pero era necesario este razonamiento para justificarlo.

## Conclusión
En otros problemas ya hemos demostrado gráficamente por qué una solución lineal es mejor que una solución cuadrática, lo importante de este ejercicio es el análisis de datos ocultos del problema que me permiten llegar a una solución muy óptima para el resultado.


## Aprendizajes y Reflexiones
1. Trata de identificar que valores constantes maneja tu problema, estos pueden ser: número de caracteres, días de una año, minutos en un día, etc.
2. Es muy importante conocer y entender cómo funcionan las diferentes estructuras de datos. El cambio de una estructura Set a un HashMap nos permitió habilitar una solución lineal.

## Recursos
1. https://leetcode.com/problems/longest-substring-without-repeating-characters/
