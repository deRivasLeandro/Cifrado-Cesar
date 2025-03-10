# Cifrado César

## Función que computa

Este programa aplica el cifrado de César sobre una cadena de caracteres usando la clave ingresada antes del mensaje a cifrar.

El cifrado César consiste en sustituir cada letra del abecedario por una letra desplazada un número determinado de posiciones (clave). Por ejemplo, si ciframos usando la clave 1, reemplazaríamos la letra A con la B, la B con la C, y así sucesivamente hasta sustituir la Z por la A.

## Estrategia 

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

## Alcance

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

## Formalismo

MT=<Γ,Σ,b,Q,q0,F,δ><br>
Alfabeto de la Cinta: Γ= {0,1,2,-,#,.,□}<br>
Alfabeto de Entrada: Σ= {0,1,-,#}<br>
Símbolo en Blanco: b=□<br>
Estados: Q= {q1,q2,q3,q4,q5,q6,q7,q8,q9,q10,q11,q12,q13,q14,q15,q16,q17,q18,q19,q20,q21,q22,q23,q24,q25,q26,q27,q28,q29,q30,q31,q32,q33,q34,q35,q36,q37,q38,q39,q40,q41,q42,q43,q44,q45,q46,q47,q48,q49,q50,q51,q52,q53,q54,q55,q56,q57,q58,q59,q60,q61,q62,q63,q64,q65,q66,q67,q68,q69,q70,q71,q72,q73,q74}<br>
Estado Inicial: q0 = q0<br>
Estados de Aceptación: F= { q54 }<br>
Transiciones:
δ(q0, -) = (q1, -, L)<br>
δ(q0, 0) = (q0, 0, R)<br>
δ(q0, 1) = (q0, 1, R)<br>
δ(q1, 1) = (q8, 1, R)<br>
δ(q1, 0) = (q2, 0, L)<br>
δ(q2, 0) = (q3, 0, L)<br>
δ(q2, 1) = (q16, 1, R)<br>
δ(q3, 0) = (q4, 0, L)<br>
δ(q3, 1) = (q24, 1, R)<br>
δ(q4, 0) = (q5, 0, L)<br>
δ(q4, 1) = (q33, 1, R)<br>
δ(q5, 1) = (q47, 1, R)<br>
δ(q5, 0) = (q6, 0, R)<br>
δ(q6, 0) = (q6, 0, R)<br>
δ(q6, .) = (q6, ., R)<br>
δ(q6, -) = (q6, -, R)<br>
δ(q6, 1) = (q6, 1, R)<br>
δ(q6, #) = (q15, ., L)<br>
δ(q6, ☐) = (q7, ☐, L)<br>
δ(q7, .) = (q23, ., R)<br>
δ(q7, -) = (q23, -, R)<br>
δ(q7, 1) = (q7, 1, L)<br>
δ(q7, 0) = (q7, 0, L)<br>
δ(q8, -) = (q9, -, R)<br>
δ(q9, #) = (q10, #, L)<br>
δ(q9, ☐) = (q10, ☐, L)<br>
δ(q9, .) = (q9, ., R)<br>
δ(q9, 0) = (q9, 0, R)<br>
δ(q9, 1) = (q9, 1, R)<br>
δ(q10, 1) = (q14, 0, L)<br>
δ(q10, 0) = (q11, 1, L)<br>
δ(q11, -) = (q12, -, L)<br>
δ(q11, 0) = (q11, 0, L)<br>
δ(q11, 1) = (q11, 1, L)<br>
δ(q11, .) = (q11, ., L)<br>
δ(q12, 1) = (q2, 1, L)<br>
δ(q13, 0) = (q18, 0, L)<br>
δ(q13, 1) = (q18, 1, L)<br>
δ(q14, 1) = (q14, 0, L)<br>
δ(q14, 0) = (q11, 1, L)<br>
δ(q15, -) = (q15, -, L)<br>
δ(q15, .) = (q15, ., L)<br>
δ(q15, 1) = (q15, 1, L)<br>
δ(q15, 0) = (q15, 0, L)<br>
δ(q15, ☐) = (q0, ☐, R)<br>
δ(q16, -) = (q17, -, R)<br>
δ(q16, 0) = (q16, 0, R)<br>
δ(q16, 1) = (q16, 1, R)<br>
δ(q17, #) = (q13, #, L)<br>
δ(q17, ☐) = (q13, ☐, L)<br>
δ(q17, 0) = (q17, 0, R)<br>
δ(q17, 1) = (q17, 1, R)<br>
δ(q17, .) = (q17, ., R)<br>
δ(q18, 1) = (q19, 0, L)<br>
δ(q18, 0) = (q20, 1, L)<br>
δ(q19, 0) = (q20, 1, L)<br>
δ(q19, 1) = (q19, 0, L)<br>
δ(q20, .) = (q20, ., L)<br>
δ(q20, 1) = (q20, 1, L)<br>
δ(q20, 0) = (q20, 0, L)<br>
δ(q20, -) = (q21, -, L)<br>
δ(q21, 0) = (q22, 0, L)<br>
δ(q21, 1) = (q22, 1, L)<br>
δ(q22, 1) = (q3, 1, L)<br>
δ(q23, 0) = (q55, 0, L)<br>
δ(q23, 1) = (q56, 1, R)<br>
δ(q24, -) = (q25, -, R)<br>
δ(q24, 0) = (q24, 0, R)<br>
δ(q24, 1) = (q24, 1, R)<br>
δ(q25, 1) = (q25, 1, R)<br>
δ(q25, 0) = (q25, 0, R)<br>
δ(q25, .) = (q25, ., R)<br>
δ(q25, #) = (q26, #, L)<br>
δ(q25, ☐) = (q26, ☐, L)<br>
δ(q26, 0) = (q27, 0, L)<br>
δ(q26, 1) = (q27, 1, L)<br>
δ(q27, 0) ⇒ (q28, 0, L)<br>
δ(q27, 1) ⇒ (q28, 1, L)<br>
δ(q28, 0) ⇒ (q29, 1, L)<br>
δ(q28, 1) ⇒ (q31, 0, L)<br>
δ(q29, 0) ⇒ (q29, 0, L)<br>
δ(q29, .) ⇒ (q29, ., L)<br>
δ(q29, 1) ⇒ (q29, 1, L)<br>
δ(q29, -) ⇒ (q30, -, L)<br>
δ(q30, 0) ⇒ (q36, 0, L)<br>
δ(q30, 1) ⇒ (q36, 1, L)<br>
δ(q31, 1) ⇒ (q31, 0, L)<br>
δ(q31, 0) ⇒ (q29, 1, L)<br>
δ(q32, 0) ⇒ (q35, 0, L)<br>
δ(q32, 1) ⇒ (q35, 1, L)<br>
δ(q33, -) ⇒ (q34, -, R)<br>
δ(q33, 0) ⇒ (q33, 0, R)<br>
δ(q33, 1) ⇒ (q33, 1, R)<br>
δ(q34, ☐) ⇒ (q32, ☐, L)<br>
δ(q34, #) ⇒ (q32, #, L)<br>
δ(q34, 1) ⇒ (q34, 1, R)<br>
δ(q34, 0) ⇒ (q34, 0, R)<br>
δ(q34, .) ⇒ (q34, ., R)<br>
δ(q35, 0) ⇒ (q39, 0, L)<br>
δ(q35, 1) ⇒ (q39, 1, L)<br>
δ(q36, 1) ⇒ (q37, 1, L)<br>
δ(q37, 1) ⇒ (q4, 1, L)<br>
δ(q38, 1) ⇒ (q45, 0, L)<br>
δ(q38, 0) ⇒ (q40, 1, L)<br>
δ(q39, 0) ⇒ (q38, 0, L)<br>
δ(q39, 1) ⇒ (q38, 1, L)<br>
δ(q40, -) ⇒ (q41, -, L)<br>
δ(q40, 1) ⇒ (q40, 1, L)<br>
δ(q40, 0) ⇒ (q40, 0, L)<br>
δ(q40, .) ⇒ (q40, ., L)<br>
δ(q41, 0) ⇒ (q42, 0, L)<br>
δ(q41, 1) ⇒ (q42, 1, L)<br>
δ(q42, 0) ⇒ (q43, 0, L)<br>
δ(q42, 1) ⇒ (q43, 1, L)<br>
δ(q43, 0) ⇒ (q44, 0, L)<br>
δ(q43, 1) ⇒ (q44, 1, L)<br>
δ(q44, 1) ⇒ (q5, 1, L)<br>
δ(q45, 0) ⇒ (q40, 1, L)<br>
δ(q45, 1) ⇒ (q45, 0, L)<br>
δ(q46, 0) ⇒ (q49, 0, L)<br>
δ(q46, 1) ⇒ (q49, 1, L)<br>
δ(q47, -) ⇒ (q48, -, R)<br>
δ(q47, 1) ⇒ (q47, 1, R)<br>
δ(q47, 0) ⇒ (q47, 0, R)<br>
δ(q48, 0) ⇒ (q48, 0, R)<br>
δ(q48, 1) ⇒ (q48, 1, R)<br>
δ(q48, .) ⇒ (q48, ., R)<br>
δ(q48, #) ⇒ (q76, #, L)<br>
δ(q48, ☐) ⇒ (q76, ☐, L)<br>
δ(q49, 0) ⇒ (q53, 0, L)<br>
δ(q49, 1) ⇒ (q53, 1, L)<br>
δ(q50, 1) ⇒ (q51, 0, L)<br>
δ(q50, 0) ⇒ (q52, 1, R)<br>
δ(q51, 1) ⇒ (q51, 0, L)<br>
δ(q51, 0) ⇒ (q52, 1, R)<br>
δ(q52, ☐) ⇒ (q7, ☐, L)<br>
δ(q52, #) ⇒ (q15, ., L)<br>
δ(q52, 0) ⇒ (q52, 0, R)<br>
δ(q52, 1) ⇒ (q52, 1, R)<br>
δ(q53, 0) ⇒ (q50, 0, L)<br>
δ(q53, 1) ⇒ (q50, 1, L)<br>
δ(q55, -) ⇒ (q54, -, R)<br>
δ(q55, 0) ⇒ (q55, 0, L)<br>
δ(q55, 1) ⇒ (q55, 1, L)<br>
δ(q55, .) ⇒ (q7, #, L)<br>
δ(q56, 0) ⇒ (q59, 0, R)<br>
δ(q56, 1) ⇒ (q57, 1, R)<br>
δ(q57, ☐) ⇒ (q58, ☐, L)<br>
δ(q57, #) ⇒ (q58, #, L)<br>
δ(q57, 0) ⇒ (q57, 0, R)<br>
δ(q57, 1) ⇒ (q57, 1, R)<br>
δ(q58, 1) ⇒ (q64, 1, L)<br>
δ(q58, 0) ⇒ (q64, 0, L)<br>
δ(q59, 0) ⇒ (q55, 0, L)<br>
δ(q59, 1) ⇒ (q60, 1, R)<br>
δ(q60, 1) ⇒ (q61, 1, R)<br>
δ(q60, 0) ⇒ (q55, 0, L)<br>
δ(q61, 1) ⇒ (q57, 1, R)<br>
δ(q61, 0) ⇒ (q62, 0, R)<br>
δ(q62, 0) ⇒ (q55, 0, R)<br>
δ(q62, 1) ⇒ (q63, 1, R)<br>
δ(q63, 1) ⇒ (q57, 1, R)<br>
δ(q63, 0) ⇒ (q55, 0, L)<br>
δ(q64, 1) ⇒ (q65, 0, L)<br>
δ(q64, 0) ⇒ (q70, 1, L)<br>
δ(q65, 0) ⇒ (q66, 0, L)<br>
δ(q65, 1) ⇒ (q66, 1, L)<br>
δ(q66, 0) ⇒ (q72, 1, L)<br>
δ(q66, 1) ⇒ (q67, 0, L)<br>
δ(q67, 1) ⇒ (q68, 0, L)<br>
δ(q67, 0) ⇒ (q74, 1, L)<br>
δ(q68, 1) ⇒ (q68, 1, L)<br>
δ(q68, 0) ⇒ (q68, 0, L)<br>
δ(q68, .) ⇒ (q7, #, L)<br>
δ(q68, -) ⇒ (q54, -, R)<br>
δ(q70, 1) ⇒ (q71, 0, R)<br>
δ(q70, 0) ⇒ (q70, 2, L)<br>
δ(q71, 1) ⇒ (q65, 1, L)<br>
δ(q71, 2) ⇒ (q71, 1, R)<br>
δ(q72, 0) ⇒ (q72, 2, L)<br>
δ(q72, 1) ⇒ (q73, 0, R)<br>
δ(q73, 1) ⇒ (q67, 1, L)<br>
δ(q73, 2) ⇒ (q73, 1, R)<br>
δ(q74, 1) ⇒ (q75, 0, R)<br>
δ(q74, 0) ⇒ (q74, 2, L)<br>
δ(q69, 2) ⇒ (q75, 1, R)<br>
δ(q69, 1) ⇒ (q68, 1, L)<br>
δ(q76, 0) ⇒ (q46, 0, L)<br>
δ(q76, 1) ⇒ (q46, 1, L)<br>

## Diseño de JFLAP:

### Diseño de la primera parte:

![JFLAP-primera-parte](/resources/img/primera-parte.png)

### Diseño de la segunda parte:

![JFLAP-segunda-parte](/resources/img/segunda-parte.png)

### Diseño de la tercera parte:

![JFLAP-tercera-parte](/resources/img/tercera-parte.png)

## Programa en simulator

Simulator: <a>http://turingmachinesimulator.com/shared/rowquykrdy/</a>

Configuración: [mt-conf](/resources/MT.mt)

## Inputs X 10:

|         Input          |         Output         |
| ---------------------- | ---------------------- |
|     00001-1000001      |     00001-1000001      |
|     00010-1000001      |     00010-1000011      |
|     00011-1000001      |     00011-1000100      |
|     00001-1011010      |     00001-1000001      |
|     11001-1011010      |     11001-1011001      |
| 00001-1000001#1000010  | 00001-1000010#1000011  |
| 00010-1000001#1011001  | 00010-1000011#1000001  |
| 00111-1000001#1000010  | 00001-1001000#1001001  |
| 10011-1011001#1001111  | 00010-1010010#1001000  |
| 11001-1011010#1011010  | 11001-1011001#1011010  |

## Complejidad espacial:

|         Input          |         Output         |   Long. w de entrada   |   Long. w de salida    |   
| ---------------------- | ---------------------- | ---------------------- | ---------------------- |
|     00001-1000001      |     00001-1000001      |           13           |           13           |
|     00010-1000001      |     00010-1000011      |           13           |           13           |
|     00011-1000001      |     00011-1000100      |           13           |           13           |
|     00001-1011010      |     00001-1000001      |           13           |           13           |
|     11001-1011010      |     11001-1011001      |           13           |           13           |
| 00001-1000001#1000010  | 00001-1000010#1000011  |           21           |           21           |
| 00010-1000001#1011001  | 00010-1000011#1000001  |           21           |           21           |
| 00111-1000001#1000010  | 00001-1001000#1001001  |           21           |           21           |
| 10011-1011001#1001111  | 00010-1010010#1001000  |           21           |           21           |
| 11001-1011010#1011010  | 11001-1011001#1011010  |           21           |           21           |

Podemos observar que la complejidad espacial es exactamente la misma que la del input ingresado, puesto que todos los computos se realizan sobre los carácteres ingresados en el mismo.

El programa toma una entrada de la forma "A-B", "A-B#C", "A-B#C#D", etc. y produce una salida de igual longitud. En todos los casos, la longitud de la salida es proporcional a la de la entrada.

Entradas de la forma "A-B": La longitud de la salida es siempre la misma que la longitud de la entrada.
Entradas de la forma "A-B#C": La longitud de la salida es la suma de las longitudes de A, B y C, más los caracteres adicionales ('-', '#'). Nuevamente, esto es lineal con respecto a la longitud de la entrada.

Por lo que podemos asegurar que la complejidad espacial se encuentra representada por <b>O(n)<b>.

## Complejidad temporal:

|         Input          |         Output         |Cantidad de movimientos |      Incremento        |   
| ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| Clave | ---------------------- | ---------------------- | ---------------------- |
|     00001-1000001      |     00001-1000010      |           56           |           -            |
|     00010-1000001      |     00010-1000011      |           58           |           2            |
|     00100-1000001      |     00100-1000101      |           60           |           2            |
|     01000-1000001      |     01000-1001001      |           62           |           2            |
|     01001-1000001      |     01001-1001010      |           80           |           18           |
|     01010-1000001      |     01010-1001011      |           82           |           2            |
|     01011-1000001      |     01011-1001100      |           100          |           18           |
|     01101-1000001      |     01101-1001100      |           100          |           18           |
|     01110-1000001      |     01110-1001111      |           104          |           4            |
|     01111-1000001      |     01111-1010000      |           124          |           20           |
| ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| Caracter | ---------------------- | ---------------------- | ---------------------- |
|     00100-1000010      |     00100-1000110      |           60           |0 (respecto al input 3) |
|     00100-1000100      |     00100-1001000      |           60           |           0            |
|     00100-1001000      |     00100-1001100      |           60           |           0            |
|     00100-1001001      |     00100-1001101      |           60           |           0            |
| ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| Resta | ---------------------- | ---------------------- | ---------------------- |
|     00100-1011000      |     00100-1000010      |           72           |12 (respecto al input 3)|
|     01111-1011000      |     01111-1001101      |           136          |12 (respecto al input 9)|
| ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| Agregar caracter | ---------------------- | ---------------------- | ---------------------- |
| 00001-1000001#1000001  | 00001-1000010#1000010  | 150 |94 (respecto al input 1)|
| 00001-1000001#1000001#1000001  | 00001-1000010#1000010#1000010  | 276 | 126 |
| 00001-1000001#1000001#1000001#1000001  | 00001-1000010#1000010#1000010#1000010  | 434 | 158 |
| 00001-1000001#1000001#1000001#1000001#1000001  | 00001-1000010#1000010#1000010#1000010#1000010  | 624 | 190 |
| ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| Casos más complejos por long | ---------------------- | ---------------------- | ---------------------- |
| 01111-1011000  | 01111-101101  |           136          |           -          |
| 01111-1011000#1011000  | 01111-101101#101101  |           358          |           222           |
| 01111-1011000#1011000#1011000  | 01111-1001100#1001100#1001100  |           660          |           302           |
| 01111-1011000#1011000#1011000#1011000  | 01111-1001100#1001100#1001100#1001100  |           1042         |         382        |

### Incremento Lineal con el input de la clave:
En los primeros casos, donde solo se incrementa el número binario de la clave, se observa un incremento constante de 2 movimientos por cada bit desplazado a izquierda en el input. Esto sugiere una complejidad lineal, O(n), donde "n" crece con el tamaño del número de la clave.

### Incrementos Mayores en Casos Específicos:
Los incrementos de 18 movimientos indican que hay casos específicos que requieren un mayor número de operaciones. Estos casos pueden deberse a la necesidad de realizar operaciones adicionales, como "acarreo" en la suma binaria, que requieren recorrer una mayor parte del input.

### Operaciones Constantes (Caracter):
Los casos donde se mantiene constante la cantidad de movimientos indican que ciertas operaciones no dependen del valor del input de caracter, lo que sugiere una complejidad constante, O(1). En este caso puntual, observamos que modificar el valor que tiene el caracter, pareciera no impactar en la cantidad de operaciones a realizar.

### Operaciones de Resta:
Los casos de resta muestran un incremento proporcional a la longitud, pero con un factor mayor que la suma simple, lo que podría indicar una complejidad de O(n) pero con una constante multiplicativa mayor.

### Agregar Caracter:
Agregar carácteres, o concatenar inputs, aumenta significativamente la cantidad de movimientos. El incremento no es estrictamente lineal, sino que parece aumentar de forma cuadrática o cercana a ella. Esto sugiere que al concatenar una gran cantidad de numeros, la complejidad aumentara considerablemente.

### Casos Complejos por Longitud:
Estos casos reflejan "la peor situación" posible para cada longitud del input y confirman que a medida que aumenta la longitud del mismo, la cantidad de movimientos crece de forma acelerada. Esto refuerza la idea de una complejidad mayor que lineal para operaciones que involucran concatenación.

## Gráficos de complejidad temporal y espacial (aproximados)

### Original:

![grafico-temporal-espacial](/resources/img/grafico-movimientos-espacio.png)

### Con escala logaritmica:
(Con esta escala se visualiza mejor el crecimiento exponencial de la complejidad temporal)
![grafico-temporal-espacial](/resources/img/grafico-movimientos-espacio-log.png)
