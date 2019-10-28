---
layout: post
title:  "Hola, mundo!"
date:   2019-10-28 12:36:39 +0000
categories: updates
---

**Despacito** es un lenguaje de programación basado en la cultura latinoamericana, de código libre y _open source_ (licencia BSD). **Despacito** es [turing-completo](https://es.wikipedia.org/wiki/Turing_completo), compilado, estáticamente tipado, y tiene soporte de arreglos, funciones y recursión. 

El compilador de **Despacito** se encuentra implementado en `Python 3` en poco más de 1000 líneas de código, sin utilizar librerías externas ni expresiones regulares. El código fuente está en GitHub:

<https://github.com/Despacito-Lang/Despacito>

El código fuente de Despacito es copyright (c) 2019, Alejandro Santos.

## Introducción

La mejor forma de introducir el lenguaje es con el clásico programa `Hola, Mundo!`:

{% highlight despacito %}
ay Esto es un comentario!

despacito ProgramaHolaMundo
bailar
    respirar
    firmar ("Hola, mundo!\n")
{% endhighlight %}

Similar al lenguaje Python, la identación de código es parte del lenguaje. Un bloque de código comienza con `bailar` y debe tener al menos una sentencia. La sentencia `respirar` no hace nada, y es equivalente al `pass` de Python.

El lenguaje Despacito es compilado y estáticamente tipado, y por el momento el compilador de referencia produce código `C`. Entonces, para ejecutar programas Despacito hace falta un compilador del lenguaje `C`. Por ejemplo con `gcc`:

{% highlight bash %}
$ ./despacito.py ejemplos/helloworld.despacito > hello.c
$ gcc -Wall -Wextra -o hello hello.c 
$ ./hello 
Hola, mundo!
{% endhighlight %}

### acuerdate (variables)

Las variables hace falta declararlas con un tipo de datos adecuado mediante un bloque de `acuerdate`, el nombre de la variable, el uso de la palabra clave `conmigo` seguido del tipo de datos. El tipo de datos `entero` es un alias de un `int` en C, pero tambien es posible usar los mismos del lenguaje `C`. Al momento, los nombres de operadores, variables, tipos de datos y nombres de funciones no permiten acentos.

La asignación de valores se hace con el operador `es`, y los operadores aritméticos son: `mas`, `menos`, `por`, `div` y `mod`, y siguen las reglas clásicas de precedencia. 

{% highlight despacito %}
ay programa uno.despacito

despacito ProgramaUno
acuerdate
    x conmigo entero

bailar
    x es 10 mas 5 por (1 mas 1)
    firmar ("El valor de x es %d\n", x)
{% endhighlight %}

La salida del programa `uno.despacito`:

{% highlight bash %}
$ ./despacito.py uno.despacito > uno.c
$ gcc -Wall -o uno uno.c 
$ ./uno 
El valor de x es 20
{% endhighlight %}

### arreglos

Para crear un arreglo hace falta indicar su tamaño mediante el uso de paréntesis. Los arreglos, al igual que en el lenguaje `C`, comienzan con el índice cero.

{% highlight despacito %}
ay programa dos.despacito

despacito ProgramaDos
acuerdate
    x conmigo entero(10)

bailar
    x(3) es 1234
    firmar ("El valor de x(1) es %d, y x(3) es %d\n", x(1), x(3))
{% endhighlight %}

La salida del programa `dos.despacito`:

{% highlight bash %}
$ ./despacito.py dos.despacito > dos.c
$ gcc -Wall -o dos dos.c
$ ./dos
El valor de x(1) es 0, y x(3) es 1234
{% endhighlight %}

### quiero

La estructura de selección (o sea, el `if` de Python) se hace con `quiero`, seguido de la condición, seguido del código identado. Cuando la condición es verdadera, el bloque se ejecuta. Usando `sino` es posible indicar el caso contrario, ya sea con una expresión, o sin ninguna expresión. Los operadores condicionales son: `igual`, `mayor`, y `menor`.

{% highlight despacito %}
ay programa tres.despacito

despacito ProgramaTres
acuerdate
    i conmigo entero

bailar
    i es 2020
    quiero (i mod 3) igual 0
        firmar ("i es divisible por 3\n")
    sino (i mod 2) igual 0 
        firmar ("i es divisible por 2\n")
    sino
        firmar ("i es: %d\n", i)
{% endhighlight %}

La salida del programa `tres.despacito`:

{% highlight bash %}
$ ./despacito.py tres.despacito > tres.c
$ gcc -o tres tres.c 
$ ./tres
i es divisible por 2
{% endhighlight %}

### pasito

Para repetir un bloque de código una cantidad limitada de veces se puede usar la estructura `pasito` con una expresión de tipo entero. La expresión se evalúa una sola vez al comenzar el bloque:

{% highlight despacito %}
ay programa cuatro.despacito

despacito ProgramaCuatro
bailar
    pasito 10
        firmar ("hola, ")
{% endhighlight %}

La salida del programa `cuatro.despacito`:

{% highlight bash %}
$ ./despacito.py cuatro.despacito > cuatro.c
$ gcc -Wall -o cuatro cuatro.c 
$ ./cuatro
hola, hola, hola, hola, hola, hola, hola, hola, hola, hola, 
{% endhighlight %}

### repetir

Para repetir un bloque de código mientras una condición sea verdadera se puede usar la estructura `mientras` con una expresion de tipo booleana. La expresión se evalúa siguiendo las reglas normales de un `while`.

{% highlight despacito %}
ay programa cinco.despacito

despacito ProgramaCinco
acuerdate
    i conmigo entero
bailar
    i es 0
    mientras i menor 10
        firmar ("hola %d, ", i)
        i es i mas 1
{% endhighlight %}

La salida del programa `cinco.despacito`:

{% highlight bash %}
$ ./despacito.py cinco.despacito > cinco.c 
$ gcc -Wall -o cinco cinco.c 
$ ./cinco 
hola 0, hola 1, hola 2, hola 3, hola 4, hola 5, hola 6, hola 7, hola 8, hola 9, 
{% endhighlight %}

### mirada (funciones)

Para modularizar el código se hace con la palabra clave `mirada`, seguido del tipo de datos de retorno de la funcion y opcionalmente los parámetros de la mirada separados por comas:

{% highlight despacito %}
ay Implementacion de FACTORIAL

despacito Factorial

mirada entero fact x conmigo entero
bailar
    quiero x mayor 1
        fact es x por fact(x menos 1)
    sino
        fact es 1

bailar
    firmar ("%d\n", fact(6))
{% endhighlight %}

La salida del programa `factorial.despacito`:

{% highlight bash %}
$ ./despacito.py factorial.despacito > factorial.c
$ gcc -Wall -Wextra -o factorial factorial.c 
$ ./factorial 
720
{% endhighlight %}

