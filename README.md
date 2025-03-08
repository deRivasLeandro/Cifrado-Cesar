# Cifrado César

## Función que computa/estrategia 

Este programa aplica el cifrado de César sobre una cadena de caracteres usando la clave ingresada antes del mensaje a cifrar.

El cifrado César consiste en sustituir cada letra del abecedario por una letra desplazada un número determinado de posiciones (clave). Por ejemplo, si ciframos usando la clave 1, reemplazaríamos la letra A con la B, la B con la C, y así sucesivamente hasta sustituir la Z por la A.

### Primera parte, cifrado del caracter:

La estrategia que usa el programa consiste en, en primera instancia, leer la clave ingresada de derecha a izquierda y, valiéndose del funcionamiento de la suma en números binarios, si el valor que lee es un 0, continúa leyendo el próximo dígito de la clave, si es un 1, avanza hacia la derecha ignorando los 1, 0, - y (de ya haber computado alguna suma sobre un número anteriormente, los . [esto se explicará más adelante]) hasta llegar a un # (separador de caracteres) o un blanco, al llegar a este punto, dependiendo de la posición que tenga el 1 dentro de la clave, se realiza la suma en la misma posición del caracter a cifrar, por ejemplo si la clave es 00001, al leer el 1 se realizará la suma sobre el último dígito del caracter en binario, si es 00010, se realizará la suma en el anteúltimo caracter.

Cabe recordar que la suma binaria funciona de la siguiente manera, si hay que sumar 0, se deja el número que se encontraba inicialmente en la posición a realizar la suma. Si hay que sumar 1 tenemos dos casos, en el caso de que en la posición inicialmente se encontrase un 0, se reemplaza ese 0 por un 1, es decir, al sumar 1 al 10 obtenemos 11. Finalmente, si sumamos 1 a un 1 debemos reemplazar de derecha a izquierda todos los 1 por 0 hasta llegar al primer 0, que será reemplazado por un 1 (por ejemplo, sumar 1 a 1011 nos daría como resultado 1100).

Una vez realizada la suma de la clave sobre un caracter, si quedan más números por leer se reemplaza el # por un ., esto explica por qué anteriormente debíamos ignorar los . encontrados entre los diferentes números a cifrar, porque representan que ese número ya fue cifrado.

### Segunda parte, verificación de tamaño:

El segundo gran bloque de estados y transiciones tiene como función determinar si al aplicar el desplazamiento de la clave a un grupo de caracteres el resultado final es mayor al último componente del alfabeto (Z, expresado en binario por su valor ASCII como 1011010).

Para lograr esto, en primer lugar se debe llegar al final de la cadena de carecteres, luego, al estar posicionado en el último número cifrado, se verifica de izquierda a derecha si el valor luego de cifrar es mayor al valor de Z, esto se consigue comparando bit a bit ambos números y cuando uno tiene un 1 donde el otro tiene un 0, ese número es identificado como mayor, si el número mayor es el que representa a Z, en ese caso el algoritmo reemplaza el . por un # para evitar una verificación y prosigue con la comparación del siguiente número. En caso contrario, si el valor del número cifrado es mayor al valor de Z se debe proceder con la resta.

Si ya verificaron todos los números cifrados, entonces el programa finaliza.

### Tercera parte, resta del tamaño del alfabeto:

Como vimos anteriormente, si ocurriese el caso de que el valor cifrado es mayor que el valor de Z, necesitamos restar la longitud del alfabeto a este número para así obtener el caracter cifrado resultante. Por ejemplo, si ciframos Z con clave 1 tendríamos 1011011 como resultado. Al ser mayor que el valor de Z debemos restar 11010 (26) que es la longitud de nuestro alfabeto, al realizar la resta podemos ver que ahora el valor del número cifrado es de 1000001 que es el valor correspondiente a A.

Teniendo esto en mente, podemos entender el funcionamiento del tercer bloque de estados y transiciones de nuestro programa. Cuando el segundo bloque identifique que un número es mayor, realizará una transición al tercer bloque que comenzará desplazando el cabezal hasta la última celda correspondiente al número cifrado (puede ser un blanco [si es el último número] o un # si hay más números delante). Desde esta posición realizará la resta del número 11010 sobre el número cifrado de derecha izquierda.

El mecanismo de resta es similar al de la suma, podemos identificar 3 casos. El primero ocurre cuando debemos restar 0, en este caso solamente debemos escribir el número que ya se encontraba en esa posición y continuar con el cómputo. En el caso de restar 1 a 1, colocamos un 0 y continuamos con el cómputo. Finalmente, en el caso más complejo debemos restar 1 a 0, para realizar esto tenemos que reemplazar la posición actual (el 0) por un 1, reemplazar a izquierda todos los 0 que encontremos por un 2 (este valor funciona como caracter auxiliar) hasta llegar a un 1, este 1 es reemplazado por un 0 y luego se dezplaza hacia la derecha mientras tenga 2 para reemplazarlos por 1 (estos 2 nos indican que son los valores que acabamos de reemplazar producto de la resta) al llegar al último caracter restado se desplaza a izquierda para continuar con la resta.

Una vez computada la resta si a izquierda del número tenemos un . este es reemplazado por un # para indicar que ya fue computada la resta y se continúa con la verificación del tamaño del siguietne número. En caso de que a izquierda haya un - significa que ya computamos la resta sobre el último número y entonces podemos finalizar el programa.

## Alcance (cantidad de letras que abarca, números, etc.):

En el alcance de esta Máquina de Turing debemos tener algunas cosas en cuenta. 

En primer lugar debemos indicar la clave a utilizar de forma binaria, al tener 26 posibles letras en el alfabeto, podemos utilizar una clave con un valor mayor o igual a 1 y a su vez menor o igual a 25, puesto que cifrar con clave 26 sería lo mismo que no cifrar y hacerlo usando una clave mayor, como 27, sería lo mismo que desplazarse el resto de dividir esa clave entre 26.

Esta restricción nos limita a usar únicamente una de las siguientes claves:

00001 -> 1<br>
00010 -> 2<br>
00011 -> 3<br>
00100 -> 4<br>
00101 -> 5<br>
00110 -> 6<br>
00111 -> 7<br>
01000 -> 8<br>
01001 -> 9<br>
01010 -> 10<br>
01011 -> 11<br>
01100 -> 12<br>
01101 -> 13<br>
01110 -> 14<br>
01111 -> 15<br>
10000 -> 16<br>
10001 -> 17<br>
10010 -> 18<br>
10011 -> 19<br>
10100 -> 20<br>
10101 -> 21<br>
10110 -> 22<br>
10111 -> 23<br>
11000 -> 24<br>
11001 -> 25<br>

Por otro lado, debemos tener en cuenta que luego de la clave debemos ingresar un - como separador, donde posteriormente a la derecha ingresaremos el valor ASCII de las letras del alfabeto que queramos cifrar. Estas mismas deben tener una longitud fija de 7 caracteres y, en caso de tener otra letra a cifrar delante debe tener un # como separador entre ambas. Los valores posibles para las letras a cifrar son los siguientes:

1000001 -> A<br>
1000010 -> B<br>
1000011 -> C<br>
1000100 -> D<br>
1000101 -> E<br>
1000110 -> F<br>
1000111 -> G<br>
1001000 -> H<br>
1001001 -> I<br>
1001010 -> J<br>
1001011 -> K<br>
1001100 -> L<br>
1001101 -> M<br>
1001110 -> N<br>
1001111 -> O<br>
1010000 -> P<br>
1010001 -> Q<br>
1010010 -> R<br>
1010011 -> S<br>
1010100 -> T<br>
1010101 -> U<br>
1010110 -> V<br>
1010111 -> W<br>
1011000 -> X<br>
1011001 -> Y<br>
1011010 -> Z<br>
