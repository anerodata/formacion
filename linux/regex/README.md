# Expresiones Regulares

Este [artículo](https://www.cyberciti.biz/faq/grep-regular-expressions/) explica algunas. Este [otro](https://regexone.com/lesson/) incluye ejemplos prácticos

## `grep`

Con `grep` podemos filtrar y así podríamos buscar todas las lineas que empiecen por "amp" `grep -Po '(?<=amp).*' los40.html`.

### Ejemplos básicos

- `grep -iw 'empleo' files/mocion-de-censura.html` devuelve todas las líneas en las que sale la palabra empleo en mayúsculas o minúsculas (`-i`). Sin `-w` también saldría subempleo.
- `grep -iwE 'abascal|arrimadas|IGLESIAS' files/mocion-de-censura.html` devuelve todas las líneas con algunas de esas palabras. `-E` significa _extended expresion_.
- `grep 'Señor...' files/mocion-de-censura.html`. Coincidiría con Señor y con tres caracteres más. El punto es cualquier caracter, pero si quisiéramos matchear con un punto? Lo haríamos así `grep '\.' file.html`

### _Anchors_

- `grep '^<' files/mocion-de-censura.html` filtra todas las líneas que empiecen por '<'.
- `grep -w '^$' files/mocion-de-censura.html` Todas las líneas vacías.
- `grep -w '^Abascal$' files/mocion-de-censura.html` las líneas que solo contengan Abascal.

### _Grupos de caracteres_

- `grep [aA]hí files/mocion-de-censura.html` el `[]` sirve para acotar caracteres. Es decir, encontrar ahí o Ahí
- `grep [A-Z]bascal files/mocion-de-censura.html` definimos rangos. Es decir, Abascal y Obascal coincidirían aquí. `grep [1-9]bascal files/mocion-de-censura.html` pillaría 1bascal

Las clases se definen de la siguiente manera:

- `[[:alnum:]]` – Alphanumeric characters.
- `[[:alpha:]]` – Alphabetic characters
- `[[:blank:]]` – Blank characters: space and tab.
- `[[:digit:]]` – Digits: ‘0 1 2 3 4 5 6 7 8 9’.
- `[[:lower:]]` – Lower-case letters: ‘a b c d e f g h i j k l m n o p q r s t u v w x y z’.
- `[[:space:]]` – Space characters: tab, newline, vertical tab, form feed, carriage return, and space.
- `[[:upper:]]` – Upper-case letters: ‘A B C D E F G H I J K L M N O P Q R S T U V W X Y Z’.
- `grep [[:digit:]] files/mocion-de-censura.html` Encontraría todos los caracteres numéricos

### Negación grupal

- `grep '[aB]ascal[^0-9]' files/mocion-de-censura.html` encuentra abascal y Abascal, pero no abascal1. Porque los números se ignoran.

### _Wildcards_

Se puede usar `.` para reemplazar caracteres

- `grep '\<S.....z\>' files/mocion-de-censura.html`: Encontraría todo lo que empieze y termine por esas letras
- `grep '^..$' files/mocion-de-censura.html` Encuentra todas las líneas con dos caracteres
- `grep '^\.[0-9]' file.html` Encuentra las líneas con un punto y un número. `\.` escapa el punto. Ya que el punto coincide con cualquier caracter.  

### `egrep` o `grep -E`

Los patrones son expresiones regulares

- `egrep [[:digit:]]{4} files/mocion-de-censura.html` busca cuatro dígitos juntos.
- `egrep word1|word2 files/mocion-de-censura.html`: una palabra o la otra. Es lo mismo que `egrep 'word1\|word2' files/mocion-de-censura.html`.

### Secuencias

- `grep -E A{1} files/mocion-de-censura.html` encuentra todas las líneas con A una vez
- `grep -E A{,3} files/mocion-de-censura.html` encuentra todas las líneas con A tres veces
- `grep -E 's{,2}' files/mocion-de-censura.html` encuentra la s una o dos veces. Sería igual que `grep 's\{,2\}' files/mocion-de-censura.html`
- `grep -E 's{2,4}' files/mocion-de-censura.html` encuentra la s dos, tres o cuatro veces.
- `grep -E 'co{1,2}l' files/mocion-de-censura.html` encuentra col o cool.

### Contar

`grep -c Abascal files/mocion-de-censura.html` Cuenta cuántas veces sale Abascal (161).

## Expresiones regulares en [RegexOne](https://regexone.com/lesson/)

Son patrones que buscan atajar partes de un texto.

El índice de caracteres básicos de la web de [RegexOne](https://regexone.com/lesson/) es el siguiente:

|Character|Meaning|
|---|---|
|abc…|Letters|
|123…|Digits|
|\d|Any Digit|
|\D|Any Non-digit character|
|.|Any Character|
|\\.|Period|
|[abc]|Only a, b, or c|
|[^abc]|Not a, b, nor c|
|[a-z]|Characters a to z|
|[0-9]|Numbers 0 to 9|
|\w|Any Alphanumeric character|
|\W|Any Non-alphanumeric character|
|{m}|m Repetitions|
|{m,n}|m to n Repetitions|
|*|Zero or more repetitions|
|+|One or more repetitions|
|?|Optional character|
|\s|Any Whitespace|
|\S|Any Non-whitespace character|
|^…$|Starts and ends|
|(…)|Capture Group|
|(a(bc))|Capture Sub-group|
|(.*)|Capture all|
|(abc|def)|Matches abc or def|

- `\d` busca un dígito del 0 al 9.
- `.` coincide con cualquier caracter. Si quisiéramos omitirlo tendríamos que escaparlo como he comentado arriba.
- `[]` sirve para buscar coincidencias con unos caracteres determinados `[cmf]` por ejemplo ligaría con can, man y fan. Pero no con dan, ran o pan, porque no tiene ni un caracter-
- `[^bog]`. El corcherte con el circunflejo sirve para descartar. Es decir, esa expresión regular ligaría con cualquier texto que no tuviera ninguno de esos caracteres
- `[A-C]` ligaría con Abc pero no con abc ya que tiene una A. Es una forma'de secuenciar caracteres. Con el guión expresamos series. `[A-Za-z0-9ñ]` Coincidiría con cualquier caracter alfanumérico. es igual que hacer `\w`.