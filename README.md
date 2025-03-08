# Cifrado César

## Función que computa/estrategia 

Este programa aplica el cifrado de César sobre una cadena de caracteres usando la clave ingresada antes del mensaje a cifrar.

El cifrado César consiste en sustituir cada letra del abecedario por una letra desplazada un número determinado de posiciones (clave). Por ejemplo, si ciframos usando la clave 1, reemplazaríamos la letra A con la B, la B con la C, y así sucesivamente hasta sustituir la Z por la A.

La estrategia que usa el programa consiste en, en primera instancia, leer la clave ingresada de derecha a izquierda y, valiéndose del funcionamiento de la suma en números binarios, si el valor que lee es un 0, continúa leyendo el próximo dígito de la clave, si es un 1, avanza hacia la derecha ignorando los 1, 0, - y (de ya haber computado alguna suma sobre un número anteriormente, los . [esto se explicará más adelante]) hasta llegar a un # (separador de caracteres) o un blanco, al llegar a este punto, dependiendo de la posición que tenga el 1 dentro de la clave, se realiza la suma en la misma posición del caracter a cifrar, por ejemplo si la clave es 00001, al leer el 1 se realizará la suma sobre el último dígito del caracter en binario, si es 00010, se realizará la suma en el anteúltimo caracter.

Cabe recordar que la suma binaria funciona de la siguiente manera, si hay que sumar 0, se deja el número que se encontraba inicialmente en la posición a realizar la suma. Si hay que sumar 1 tenemos dos casos, en el caso de que en la posición inicialmente se encontrase un 0, se reemplaza ese 0 por un 1, es decir, al sumar 1 al 10 obtenemos 11. Finalmente, si sumamos 1 a un 1 debemos reemplazar de derecha a izquierda todos los 1 por 0 hasta llegar al primer 0, que será reemplazado por un 1 (por ejemplo, sumar 1 a 1011 nos daría como resultado 1100).

## Alcance (cantidad de letras que abarca, números, etc.)
Proyecto que realiza el cifrado César en binario con clave variable
