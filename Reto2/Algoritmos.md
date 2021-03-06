##Algoritmos para hallar las cifras de Cifras y Letras

####Algoritmo 0 (no presentado):
* __Entrada__: número a buscar, números disponibles.
* __Procedimiento__:
  * Si el número de números disponibles es 2, se comprueba si hay una operación entre ellos cuyo resultado sea la solución: si es posible, se devuelve la operación realizada.
  * En caso contrario, para cada número disponible `i` y cada operación posible `op` (incluyendo la operación descartar el número actual, que puede entenderse como aquella que cumple `a op b = b`), se calcula `x` tal que `i` `op` `x` es igual al número a buscar y (si `x` es un número temporal válido y distinto del número a buscar) se llama a este mismo algoritmo con `x` como número a buscar y todos los números disponibles excepto `i` como números disponibles. En caso de que tal llamada devuelva una solución, se devolverá tal solución junto con la operación que se ha hecho en este paso.
* __Salida__: una serie de operaciones que llevan al número buscado o un indicador de que no se ha encontrado solución.
* __Implementaciones__:
  * __C++__: [try1.cpp]  (try1/try1.cpp). Incluye generador de valores de entrada aleatorios.
  * __JavaScript__: [try1.js]  (try1/try1.js). Incluye generador de valores de entrada aleatorios y lector de valores de entrada.
* __Problemas__:
  * __No puede aproximar la solución__: si no encuentra solución exacta, no busca soluciones cercanas. Podría imponerse que, si no se encuentra solución, pruebe con los números anterior y siguiente, pero es una chapuza.
  * __No es exhaustivo__ (pese a que la intención es esa misma): en el pdf se afirma que con los números disponibles `C = {2,6,7,9,50,75}` puede obtenerse todo número de tres cifras. Si eso es cierto, el algoritmo no es exhaustivo, dado que no puede encontrar solución para `767, C` ni `821, C` (aunque para el resto de números de 100 a 999 sí). Esto se debe a que el algoritmo no considera soluciones en las que se haga una operación con dos números temporales, sino que obliga a que todas las operaciones incluyan un número proporcionado al principio (en el pdf no quedaba claro si esto era posible o no). Una solución (que este algoritmo no encuentra y a mí me ha costado bastante encontrar) para `767, {2,6,7,9,50,75}` es: `7 * 6 = 42;
9 * 75 = 675;
675 + 42 = 717;
717 + 50 = 767`

####Algoritmo 1:
* __Entrada__: número a buscar, números disponibles.
* __Definiciones__:
  * __Nodo__: estructura que almacena al menos un valor, la posición de dos nodos previos (o inválidos) e información sobre qué números iniciales fueron necesarios para obtenerlo.
  * __Árbol__: el conjunto de nodos que serán obtenidos durante el procedimiento.
  * __Ampliar el árbol__: recorrer el árbol (de forma adecuada) y añadirle nodos obtenidos a partir de los propios nodos que tenía.
  * __Generación__: se dirá que un nodo es de primera generación si procede directamente de los números disponibles al inicio del procedimiento; y se dirá que un nodo es de `n`-ésima generación si se obtuvo tras `n-1` ampliaciones del árbol.
* __Procedimiento__: se conforma el árbol como 6 nodos consistentes en los valores iniciales, con nodos previos inválidos y con su propio valor como número inicial usado. Cinco veces, o hasta que se encuentre el número a buscar, se amplía el árbol añadiendo al mismo los nodos que pueden obtenerse usando operaciones (la suma, el producto, la resta del mayor menos el menor si no es nula y la división del mayor entre el menor si es posible) con parejas de nodos disponibles que cumplan las siguientes condiciones:
  * La suma de sus generaciones es 1 más que el número de ampliaciones comenzadas.
  * Proceden de números iniciales distintos (no necesariamente en valor, pero sí en posición).
  * El resultado no es igual al valor de ninguno de los nodos de la pareja.
  * El nodo de la pareja que se añadió antes al árbol no procede de la misma operación.
  * Si la operación no es conmutativa (resta, cociente), el nodo de la pareja que se añadió después al árbol no procede de la misma operación.
  * Si la operación es conmutativa (suma, producto) y el nodo de la pareja que se añadió después al árbol (sea `N` tal nodo) no procede de la misma operación, el primer nodo del que salió `N` se añadió al árbol después que el nodo de la pareja que se añadió antes.

  Una vez se producen las ampliaciones, se construye la mejor solución obtenida.
* __Salida__: una serie de operaciones que llevan al número buscado o al más próximo al mismo que se haya encontrado, con la particularidad de que la cantidad de números iniciales empleados para llegar a la solución será mínima.
* __Implementaciones__:
  * __C++__: [Algoritmo1.cpp]  (Implementaciones/Algoritmo1/Algoritmo1.cpp).
* __Características__:
  * __Muy rápido__: en el peor de los casos tarda menos de 5 milisegundos en devolver un resultado.
  * __Utiliza el menor número de operaciones necesario__: siempre ofrece una solución que use el mínimo número posible de operaciones.
  
  Debido al reducido tiempo de ejecución, las combinaciones mágicas se obtendrán con este algoritmo. El programa que las explora es [Algoritmo1magic.cpp] (Implementaciones/Algoritmo1/Algoritmo1magic.cpp).
  
####Algoritmo 2:
* __Entrada__: número a buscar, números disponibles.
* __Procedimiento__: [POR RELLENAR]
* __Salida__: una serie de operaciones que llevan al número buscado, o si eso no es posible, se imprime el número más cercano al mismo que puede obtenerse.
* __Implementaciones__:
  * __C++__: [Algoritmo2.cpp]  (Implementaciones/Algoritmo2/Algoritmo2.cpp) (depende de más archivos)
* __Características__:
  * __Relativamente lento__: tiene el mayor tiempo de ejecución de los tres algoritmos presentados.
  * __No conserva las operaciones de ningún resultado que no sea exacto__: por ello, para obtener las operaciones para un resultado aproximado, es necesario llamar al mismo algoritmo con el número más cercano obtenido como entrada. La implementación del programa no hace esto.

####Algoritmo 3:
* __Entrada__: número a buscar, números disponibles.
* __Procedimiento__: [POR RELLENAR]
* __Salida__: una serie de operaciones que llevan al número buscado, o si eso no es posible, se imprime el número más cercano al mismo que puede obtenerse.
* __Implementaciones__:
  * __C++__: [Algoritmo3.cpp]  (Implementaciones/Algoritmo3/Algoritmo3.cpp) (depende de más archivos)
* __Características__:
  * __Bajo uso de memoria__: es el algoritmo que menos memoria requiere de los tres presentados.
  * __No conserva las operaciones de ningún resultado que no sea exacto__: por ello, para obtener las operaciones para un resultado aproximado, es necesario llamar al mismo algoritmo con el número más cercano obtenido como entrada. La implementación del programa se ocupa de hacer esto en caso de resultado no exacto.