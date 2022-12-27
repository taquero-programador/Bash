# Bash

![bash](icons/bash_logo.png)

## Bash
Bash es un shell de Unix escrito por Brian Fox para el proyecto GNU como un reemplazo de software gratuito para Bourne shell. Fue lanzado en 1989 y se ha distribuido como shell predeterminado de Linux Y MacOS durante mucho tiempo.

## Shells y modos
El shell bash del usuario puede funcionar de dos modos: interactivo y no interactivo.

#### Modo interactivo
Si está trabanado en Ubuntu, tiene siete terminales virtuales diferentes. El entorno de escritorio tiene lugar en la séptima terminal virtual, por lo que puede volver a una GUI amigable usando `Ctrl+Alt+F7`.

Puede abrir el shell usando `Ctrl+Alt+F1`. Después de eso, la GUI familiar desaparecerá y se monstrará una de las terminales virtuales.

Si ve algo como esto, entonces está trabajando en modo interactivo:

    user@host:~$

Aquí puede ingresar una variedad de comandos Unix, como `ls`, `grep`, `cd`, `mkdir`, `rm` y ver el resultado de su ejecución.

Llamamos a este shell interactivo porque interactúa directamante con el usuario.

Usar una terminal virtual no es realmente conveniente. Por ejemplo, si desea editar un documento y ejecutar otro comando la mismo tiempo, es mejor usar emuladores de terminales virtuales como:
- Terminal GNOME
- Terminator
- iTerm2
- ConEmu

#### Modo no interactivo
En el modo no interactivo, el shell lee los comandos de un archivo o una tubería y los ejecuta. Cuando el intérprete llega al final del archivo, el proceso de shell finaliza la sesión y vulver al proceso principal.

Comando para ejecutar el shell en modo no interactivo:
```bash
sh my_script.sh
# or
bash my_script.sh
```

Puede simplificar la invocación del script haciendolo un archivo ejecutable:

    chmod +x my_script.sh

Además, la primera línea del script debe indicar qué programa debe usar para ejecutar el archivo:
```bash
#!/bin/bash
echo "Hola, Linux!"
```

Puede usar `#!/bin/bash` o `#!/bin/sh`. La secuencia `#!` de caracteres se conoce como shebang. Ahora el script se puede ejecutar así:

    ./my_script.sh

Otra forma de usar el shebang es la siguiente:
```bash
#!/usr/bin/env bash
echo "Hola, Linux!"
```

La ventaja de esta línea shebang es que buscará el programa (`bash`) basado en el PATH. Esto a menudo se prefiere al primer método, ya que no simpre asumira la ubicación de un programa en un sistema de archivos. Esto es útil si la variable PATH en un sistema se ha configurado para apuntar a una versión alternativa del programa. Por ejemplo, se podria instalar una versión más nueva de `bash` conservando la versión original e instalar la ubicación más nueva en la PATH. El uso de `#!/bin/bash` usara la versión original, mienstras que `#!/usr/bin/env bash` usara la versión más reciente.

#### Códigos de salida
Cada comando devuelve un código de salida (estado de retorno o estado de salida). Un comando exitoso siempre regresa `0`, y un comando que ha fallado devuelve un valor distinto a cero. Los códigos de falla deben ser números enteros positivos dentre 1 y 255.

Otro comando útil es usar `exit`. Este comando se usa para terminar la ejecución actual y entraga un código de salida al shell. Ejecutar `exit` sin ningún argumento, finalizará el script en ejecución y devolverá el código de salida del último comando ejecutado antes de `exit`.

Cuando un programa termina, el shell asigna un código de salida `#?` a la variable. La variable `#?` es como solemos probar si un script ha tenido éxito o no es su ejecución.

De la misma manera podemos usar `exit` para terminal un script. Podemos usar el comando `return` para salir de una función y devolver un código de salida a la persona que llama. Puede usar `exit` dentro de una función y esto saldrá de la función y terminará el programa.

## Comentarios
Los script pueden contener comentarios. Los comentarios son declaraciones especiales ignoradas por el shell. Comienza con con un simbolo `#` y continúe hasta el final de la línea.

Ejemplo:
```bash
#!/bin/bash
# imprime el username
whoami
```

## Variables
Bash no conoce tipos de datos. Las variables pueden contener solo números o una cadena de uno o más caracteres. Hay tres tipos de variables que puede crear: variables locales, variables de entorno y variables como parámetros posicionales.

#### Variables locales
Las variables locales son variables que existen solo dentro de un único script. Son inaccesibles para otros programas y scripts.

Una variable local se puede declarar usando `=` (como regla, no debe haber espacios entre el nombre de la variable y su valor) y su valor se puede recuperar usando `$`. Ejemplo:
```bash
username="Bender" # declarar variable
echo $username # imprime el valor
unset username # elimina variable
```

También podemos declarar una variable local a una sola función usando `local`. Si lo hace, la variable desaparecerá cuando finalice la función.
```bash
local local_var="local value"
```

#### Variables de entorno
Las variables de entorno son variables accesibles para cualquier programa o script que se ejecute en la sesión de shell actual. Se crean como variables locales, pero usando la palabra clave `export`.
```bash
export GLOBAL_VAR=" variable global"
```

Hay muchas variables globales en bash. Encontrará estas variables con bastante frecuencia. Tabla de referencia con las más prácticas:

Variable | Descripción
-- | --
`$HOME` | El directorio de inicio del usuario actual
`$PATH` | Una lista de directorios separados por dos puntos en los que el shell busca comandos
`$PWD` | El directorio de trabajo actual
`$RANDOM` | Entero aleatorio entre 0 y 32767
`$UID` | El ID numérico del usuario real actual
`$PS1` | La cadena de mensaje pricipal
`$PS2` | La cadena de solicitud secundaria

[Enlace](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html#sect_03_02_04) para ver una lista amplia de las variables de entorno.

#### Parámetros posicionales
Los parámetros posicionales son variables asignadas cuando se evalúa una función y se dan posicionalmente. La siguiente tabla enumera las variables de parámetros posicionales y otras variables especiales y sus significados cuando se encuentran dentro de una función.

Parámetro | Descripción
-- | --
`$0` | Nombre del script
`$1 ... $9` | Los elementos de la lista de parámetros del 1 al 9
`${10} .. ${N}` | Los elementos de la lista de parámetros de 10 a N
`$*` O `$@` | Todos los parámetros posicionales excepto `$0`
`$#` | El número de parámetros, sin contar `$0`
`$FUNCNAME` | El nombre de la función (tiene un valor solo dentro de la función)

En el siguiente ejemplo, los parámetros posicionales serán `$0=./script.sh`, `$1='foo'` y `$2='bar'`:

    ./script.sh foo bar

Las variables también pueden tener valores predeterminados:
```bash
# si la variable está vacía, se asigna el valor por defecto
: ${VAR:='default'}
: ${1:='first'}
# or
FOO=${FOO:-'default'}
```

## Expansiones de shell
Las expansiones se realizan en la línea de comandos después de que se haya dividido en tokens. En otras palabras, estas expansiones son un mecanismo para calcular operaciones aritméticas, para guardar resultados de ejecución de comandos, etc.

[Más sobre la expansión de shell](https://www.gnu.org/software/bash/manual/bash.html#Shell-Expansions).

#### Expansión de refuerzo
La expansión de llaves nos permiten generar cadenas arbitrarias. Es similar a la expansión de nombre de archivos. Por ejemplo:
```bash
echo beh{i,a,u} # begin began gebun
```

También se pueden usar expansiones de llaves para crear rangos, que iteran en bucles:
```bash
echo {1..5} # 1 2 3 4 5
echo {00..8..2} # 00 02 04 06 08
```

#### Sustitución de comandos
Las sustitución de comandos nos permite evaluar un comando y sustituir su valor con otro comando o asignación de variable. La sustitución de comandos se realiza cuando un comando está encerrado entre (comillas simples de lado) O `$()`. Por ejemplo:
```bash
now=`date +%T`
# or
now=$(date +%T)

echo $now # date
```

#### Expansión aritmética
En bash somos libres de hacer cualquier operación aritmética. Pero la expresión debe encerrarse entre `$(())`. El formato para la expresiones aritméticas es:
```bash
result=$(( ((10 + 5*3) -7 ) / 2 ))
echo $result
```

Dentro de las expansiones aritméticas, las variables generalmente deben usarse sin un prefijo `$`:
```bash
#!/bin/bash

x=4
y=7

echo $(( x + y ))
echo $(( ++x + y++ ))
echo $(( x + y  ))
```

#### Comillas simples y dobles
Hay una diferecia importante entre comillas simples y dobles. Dentro de las comillas dobles, las variables o sustituciones de comandos se expanden. Dentro de comillas simples no lo son. Por ejemplo:
```bash
echo "Home is: $HOME" # Home is: /home/user
echo 'Home is: $HOME' # Home is: $HOME
```

Tenga cuidado de expandir las variables locales y las variables de entorno entre comillas si pueden contener espacios en blanco. Como un ejemplo, considere user `echo` para imprimir alguna entrada de usuario:
```bash
INPUT="Una cadena  con   espacios"
echo $INPUT # Una cadena con espacios
echo "$INPUT" # Una cadena  con   espacios
```

Ahora considere un ejemplo más serio:
```bash
FILE="Favorite Things.txt"
cat $FILE   # imprime dos archivos: `Favorite` and `Things.txt`
cat "$FILE" # imprime un archivo: `Favorite Things.txt`
```

## Arrays
Como en otros lenguajes de programación. un array en bash es una variable que te permite referirte a múltiples valores. En bash, los arrays también se basan en cero, es decir, el primer elemento de un array es `0`.

Cuando se trata de arrays, debemos tener en cuenta la variable de entorno especial `IFS`. `IFS`, o separador de campos de entrada, es el carácter que separa los elementos de un array. El valor predeterminado es un espacio vacío `IFS=' '`.

#### Declaración de array
En bash, creá un array simplemente asignando un valor a un índice en la variable del array:
```bash
#!/bin/bash

fruits[0]=Apple
fruits[1]=Pear
fruits[2]=Plum
```
Los arrays también se pueden crear utilizando asignaciones compuestas como:
```bash
fruits=(Apple Pear Plum 'Two Apples')
```

#### Expansión de arrays
Los elementos individuales de array se expanden de manera similar a otras variables.
```bash
echo ${fruits[1]} # Pear
```

El array completo se puede expandir usando `*` o `@` en lugar del indice numérico:
```bash
echo ${fruits[*]} # Apple Pear Plum
echo ${fruits[@]} # apple Pear Plum
```

Hay una diferencia importante (y sutil) entre las dos líneas anteriores, considere un elemento de array que contenga espacios en blanco:
```bash
#!/bin/bash

fruits[0]=Apple
fruits[1]="Desert fig"
fruits[2]=Plum
```
Queremos imprimir cada elemento del array en una línea separada, así que tratamos de usar `printf`:
```bash
printf "+ %s\n" ${fruits[*]}

# salida
+ Apple
+ Desert
+ fig
+ Plum
```
Por qué estaba `Desert` y `fig` impreso en líneas separadas? Tratemos de usar comillas:
```bash

printf "+ %s\n" ${fruits[*]}

# salida
+ Apple Desert fig Plum
```
Ahora todo está en una sola línea. Aquí es donde entra `${fruits[@]}`:
```bash

printf "+ %s\n" ${fruits[@]}

# salida
+ Apple
+ Desert fig
+ Plum
```
Dentro de comillas dobles, `${fruits[@]}` se expande a un argumento separado para cada elemento del array; se conserva los espacios en blnaco en los elementos del array.

#### Segmento de array
Además, podemos extraer una porción del array usando los operadores de porción `:`:
```bash
echo ${fruits[@]:0:2} # Appele Desert fig
```
En el ejemplo anterior, `${fruits[@]}` se expande a todo el contenido del array, y `:0:2` extrar el segmento de longitud 2, que comienza en el indice 0.

#### Adición de elementos a un array
Agregar elementos a un array también es bastante simple. Las asignaciones compuestas son especialmente útiles en este caso. Se puede usar así:
```bash
fruits=(Orange ${fruits[@]} Banana Cherry)

echo ${fruits[@]}
```
En el ejemplo anterior, `${fruits[@]}` expande todo el contenido del array y lo sustituye en la asignación compuestas, luego asigna el nuevo valor en el array `fruits` mutando su valor original.

#### Eliminar elementos de un array
Para eliminar elementos de un array, utilice `unset`:
```bash
unset fruits[0]
echo ${fruits[@]} # Apple Desert fig Plum Banana Cherry
```

## Streams, pipes and lists
Bash tiene poderosas herramientas para trabajar con otros programas y sus resultados. Usando flujos (streams), podemos enviar la salida de un programa a otro programa o archivo y, por lo tanto, escribir registros o lo que queramos.

Las tuberías (pipes) nos da la oportunidad de crear transportadores y controlar la ejecución de comandos.

#### Streams
Bash recibe entradas y envía salidas como secuencias o flujos de caracteres. Estos flujos se pueden redirigir a archivos o uno a otro.

Hay tres descriptores:
Código | Descriptor | Descripción
-- | -- | --
`0` | `stdin` | La entrada estándar
`1` | `stdout` | La salida estándar
`2` | `stderr` | La salida de error

La redirección permite controlar a dónde va la salida de un comando y de dónde proviene la entrada de un comando. Para redirigir flujos se utilizan estos operadores:
Operador | Descripción
-- | --
`>` | Redirección de salida
`&>` | Salida de redirección y salida de error
`&>>` | Agragar salida redirigida y salida de error
`<` | Entrada de redirección
`<<` | [Enlace](http://tldp.org/LDP/abs/html/here-docs.html)
`<<<` | [Enlace](http://www.tldp.org/LDP/abs/html/x17837.html)

Ejemplos del uso de redirecciónes:
```bash
# la salida de ls será escrito en list.txt
ls -l > list.list

# la salida la añade al final de archivo list.txt
ls -a >> list.txt

# todos los errores son escritos en errors.txt
grep da * 2> errors.txt

# lee error.txt
less < errors.txt
```

#### Pipes 
Podríamos redirigir transmisiones estándar no solo en archivos, sino también a otros programas. Los pipes (tuberías) nos permiten usar la salida de un programa como la entrada de otro.

En el siguiente ejemplo, `command1` envía su salida a `command2`, que luego lo pasa a la entrada de `command3`:

    command1 | command2 | command3

Las construcciones como estas se llaman pipes.

En la pŕactica, esto se puede utilizar para procesar datos a través de varios programas. Por ejemplo, aquí la salida de `ls -l` se envía `grep`, que imprime solo archivos con extensión `.md`, y esta salida finalmente se envía a `less`:

    ls -l | grep .md$ | less

El estado de salida de una canalización es normalmente el estado de salida del último comando de la canalización. El shell no devolverá un estado hasta que se haya completado todos los comandos de la canalización. Si desea que sus canalizaciones se consideren fallidas si alguno de los comandos falla, debe configurar la opción `pipefail`:

    set -o pipefail

#### Lista de comandos
Una lista de comandos es una secuencia de uno o más conductos separados por `;`, `&`, `&&` o `||`.

Si un comando es terminado por el operador de control `&`, el shell ejecuta el comando de forma asícrona en un subshell. En otras palabras, este comando se ejecuta en segundo plano.

Comando separados por `;` se ejecutan secuencialmente: uno tras otro. El shell espera el final de cada comando.
```bash
command1 ; command2

# es lo mismo que
command1
command2
```

Listas separadas port `&&` y `||` se denominan listas AND y OR.

La lista AND se ve así:
```bash
# command2 se ejecuta si, solo si, command1 termina con exito (return 0 exit status)
command1 && command2
```

La lista OR tiene la siguiente forma:
```bash
# command 2 se ejecuta si, solo si, command1 termina sin exito (retorna el código de error)
command1 || command2
```

## Declaraciones condicionales
Como en otros lenguajes, las condiciones en Bash nos permiten decidir si realizar una acción o no. El resultado se determina evaluando una expresión que debe encerrarse entre `[[  ]]`.

La expresión condicional puede contener `&&` y `||`, que son AND y OR. Además de esto, hay muchas otras expresiones útiles.

Hay dos declaraciones condicionales diferentes: `if` y `case`.

#### Expresiones primarias y combinadas
Expresiones encerradas dentro de `[[  ]]` (o `[  ]` por `sh`) se denomina comandos de prueba o primarios. Estas expresiones nos ayudan a indicar los resultados de un condicional. En las siguients tablas, estamos usando `[  ]`, porque sirve también para `sh`. Aquí hay una respuesta sobre la diferencia, [enlace](http://serverfault.com/a/52050).

Trabajando con el sistema de archivos:
Primario | Significado
-- | --
`[ -e FILE ]` | cierto si `FILE` existe
`[ -f FILE ]` | cierto si `FILE` existe y es un archivo regular
`[ -d FILE ]` | cierto si `FILE` existe y es un directorio
`[ -s FILE ]` | cierto si `FILE` existe y está vacío (peso menor de 0)
`[ -r FILE ]` | cierto si `FILE` existe y es legible
`[ -w FILE ]` | cierto si `FILE` existe y se puede escribir
`[ -x FILE ]` | cierto si `FILE` existe y es ejecutable
`[ -L FILE ]` | cierto si `FILE` existe y es simbólico
`[ FILE1 -nt FILE2 ]` | FILE1 es más reciente que FILE2
`[ FILE1 -ot FILE2 ]` | FILE1 es más antiguo que FILE2

Trabajando con cadenas:
Primario | Significado
-- | --
`[ -z STR ]` | STR está vacío
`[ -n STR ]` | STR no está vacío
`[ STR1 == STR2 ]` | STR1 y STR2 son iguales (`=` y `==` for string)
`[STR1 != STR2]` | no son iguales

Operadores binarios aritméticos:
Primario | Significado
-- | --
`[ ARG1 -eq ARG2 ]` | ARG1 y ARG2 son iguales
`[ ARG1 -ne ARG2 ]` | ARG1 no es igual a ARG2
`[ ARG1 -lt ARG2 ]` | ARG1 es menor que ARG2
`[ ARG1 -le ARG2 ]` | ARG1 es manor o igual a ARG2
`[ ARG1 -gt ARG2 ]` | ARG1 es mayor que ARG2
`[ ARG1 -ge ARG2 ]` | ARG1 es mayor o igual a ARG2

Las condiciones se pueden combinar usando estas expresiones combinadas:
Operación | Efecto
-- | --
`[ ! EXPR ]` | cierto si EXPR es falsa
`[ (EXPR) ]` | devuelve el valor de EXPR
`[ EXPR1 -a EXPR2 ]` | lógico AND. cierto si EXPR1 y EXPR2 son iguales
`[ EXPR1 -o EXPR2 ]` | lógico OR. cierto si EXPR1 o EXPR2 son verdaderas

Hay primarios más útiles y puede encontrarlos en la siguiente [página](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html).

#### Usando declaración `if`
Las sentencias `if` funcionan igual que en otros lenguajes de programación. Si la expresión entre llaves es verdadera, el código entre `then` y `fi` es ejecutado. `fi` indica el final de la condición.
```bash
#!/bin/bash

# una sola línea
if [[ 1 -eq 1 ]]; then echo "true"; fi

# múltiples línea
if [[ 1 -eq 1 ]]; then
    echo "true"
fi
```

De la misma manera, podríamos usar una declaración `if..else` como:
```bash
#!/bin/bash

# single line
if [[ 2 -ne 1 ]]; then echo "true"; else echo "false"; fi

# multi line
if [[ 2 -ne 1 ]]; then
    echo "true"
else
    echo "false"
fi
```

Algunas veces las condiciones `if..else` no son suficientes para hacer lo que queremos hacer. En este caso no debemos olvidarnos de la declaración `if..elif..else`.

Ejemplo:
```bash
#!/bin/bash

if [[ `uname` == "Adam" ]]; then
    echo "Do not eat an Apple!"
elif [[ $(uname) == "Eva" ]]; then
    echo "Do not take an Apple"
else
    echo "Apples are delicius"
fi
```

#### Usando declaraciones `case`
Si se enfrenta a un par de posibles acciones a realizar, entonces usar `case` puede ser más útil que una declaración `if` anidada. Para condiciones más complejas use `case`:
```bash
#!/bin/bash

case "$extension" in
    "jpg|jpeg") echo "image with jpg extension";;
    "png") echo "image with png extension";;
    "gif") echo "Oh, is giphy";;
    *) echo "its not a image";;
esac
```

Cada caso es un expresión que coincide con un patrón. El `|` signo se utiliza para separar múltiples patrones y el `)` operador termina una lista de patrones. Se ejecutan con comandos para el primer partido. `*` es el patrón para cualquier otra cosa que no coincida con los patrones definidos. Cada bloque de comandos debe dividirse con el operador `;;`.

## Bucles
Como en cualquier lenguaje de programación, un bucle en bash es un bucle de código que itera siempre que el control condicional sea verdadero.

Hay cuatro tipos de bucles Bash: `for`, `while`, `until` y `select`.

#### Bucle `for`
`for` es muy similar a su hermano en C. Se ve así:
```bash
for args in elem1 elem2 ... elemN
do
    # pasos
done
```

Durante cada pasada por el bucle, `arg` toma el valor de `elem1` a `elemN`. Los valores también pueden ser comodines o expansiones de llaves.

Además, podemos escribir el bucle `for` en una sola línea, en ese caso debe hacer un `;` antes de `do`:
```bash
for i in {1..5}; do echo $i; done
```

Si `for..in..do` le parece un poco raro, también puede escribir `for` en estilo `C-like`:
```bash
for (( i = 0; i < 10; i++ )); do
    echo $i
done
```

`for` es útil cuando queremos hacer la misma operación sobre cada archivo en un directorio. Por ejemplo, si necesitamos mover todos los archivos `.bash` a la carpeta `script` y darles permiso de ejecución:
```bash
#!/bin/bash

for FILE in $HOME/*; do
    mv "$file" "${HOME}/script"
    chmod +x "${HOME}/script/${FILE}"
done
```

#### Bucle `while`
`while` prueba una condición y repite una secuencia de comandos siempre que esa condición sea verdadera. Una condición no es más que un primario como se usa en `if..then`. Entonces un bucle `while` se ve así:
```bash
while [[ condition ]]
do
    # pasos
done
```

Ejemplo:
```bash
#!/bin/bash

x=0
while [[ $x -lt 10 ]]; do
    echo $(( x * x ))
    x=$(( x + 1 ))
done
```

#### Bucle `until`
Los bucles `until` es exactamente lo contrario de `while`. Como un `while` comprueba una condición de prueba, pero sigue en bucle mientras esta condición sea falsa:
```bash
until [[ condition ]]; do
    # pasos
done
```

#### Bucle `select`
Los bucles `select` nos ayudan a organizar un menú de usuario. Tiene casi la misma sintaxis que el bucle `for`:
```bash
select answer in elem1 elem2 .. elemN; do
    # pasos
done
```

`select` imprime todo `elem1..elemN` en pantalla con sus números de secuecia, después de eso le pregunta al usuario. Por lo regular parece `$?`. La respuesta se guardara en `answer`. Si `answer` es el número entre `1..N`, después ejecutara `statements` y `select` ira a la siguiente iteración, eso es porque deberíamos usar la declaración `break`.

Ejemplo:
```bash
#!/bin/bash

PS3="Choose the package manager "
select ITEM in bower npm gem pip; do
    echo "Selec  \"$ITEM\" "
    echo -n "Enter the package name: " && read PACKAGE
    # same
    # read -p "Enter the package name: " PACKAGE
    case $ITEM in
        bower) bower install $PACKAGE;;
        npm) npm install $PACKAGE;;
        gem) gem install $PACKAGE;;
        pip) pip install $PACKAGE;;
    esac
    break # sale del bucle infinito
done
```

Este ejemplo, le pregunta al usuario qué administrador de paquetes le gustaría usar. Luego, nos preguntará qué paquete queremos instalar y finalmente procederá a instalarlo.

Al ejecutarlo obtendremos esto:
```bash
$ ./my_script
1) bower
2) npm
3) gem
4) pip
Choose the package manager: 2
Enter the package name: bash-handbook
<installing bash-handbook>
```

#### Control de bucle
Hay situaciones en la que necesitamos detener un bucle antes de que finalice normalmente o pasar por alto una iteración. En estos casos, podemos usar el shell incorporado `break` y `continue`. Ambos funcionan con todo tipo de bucle.

Las instrucción `break` se utiliza para salir del bucle actual antes de que finalice.

Pasos para una declaración `continue` sobre una iteración:
```bash
#!/bin/bash

for (( i=0; i < 10; i++ )); do
    if [[ $(( i % 2 )) -eq 0 ]]; then
        continue
    fi
    echo $i
done
```
Imprimirá los números impares del 0 al 9.

## Funciones
En los scripts tenemos la capacidad de definir y llamar funciones. Como en cualquier lenguaje de programación, las funciones en bash son fragmentos de código, pero exiten diferencias.

En bash, las funciones son una secuencia de comandos agrupados sobre solo un nombre, ese es el nombre de la funcion. Llamar una función es lo mismo que llamar a cualquier otro programa, solo se escribe el nombre y se invocará la función.

Podemos declarar nuestra propia función de esta manera:
```bash
my_func () {
...
}

my_func
```
Debemos declarar funciones antes de poder invocarlas.

Las funciones pueden tomar argumentos y devolver un resultado: código de salida. Los argumentos, dentro de las funciones, se tratan de la misma manera que los argumentos dados al script en modo interactivo, utilizando parámetros posicionales. se puede devolver un código de resultado usando `return`.

Ejemplo donde toma un nombre y devuleve `0`, lo que indica una ejecución exitosa:
```bash
#!/bin/bash

# función con parámetros

greeting () {
    if [[ -n $1 ]]; then
        echo "Hello, $1!"
    else
        echo "Hello, unkown!"
    fi
    return 0
}

greeting Denys
greeting
```
El comando `return` sin ningún argumento devuelve el código de salida del último comando ejecutado. Arriba, `return 0` devolverá un código de salida exitoso `0`.

#### Depuración
El shell nos brinda herramientas para depurar scripts. Si queremos ejecutar un script en modo de depuración, usamos una opción especial en el shebang de nuestro script:
```bash
#!/bin/bash options
```
Estas opciones son configuraciones que cambian el comportamiento del shell. La siguiente tabla es una lista de opciones que pueden ser útiles:
Short | Nombre | Descripción
-- | -- | --
`-f` | noglob | Deshabilite la opción del nombre de archivo (globbing).
`-i` | interactive | El script se ejecuta en modo interactivo.
`-n` | noexec | Leer comandos, pero no ejecutarlos (comprobración sin sintaxis).
     | pipefail | Haga que las canalizaciones fallen si algún comando falla, no solo si falla el comando final.
`-t` | - | Salga después del primer comando.
`-v` | verbose | Imprime cada comando para `stderr` antes de ejecutarlo.
`-x` | xtrace | Imprimira cada comando y sus argumentos expandidos para `stderr` antes de ejecutarlo.

Por ejemplo, tenemos un script con la opción `-x`:
```bash
#!/bin/bash -x

num=0

while [[ $num -lt 3 ]]; do
    echo $num
    num=$((num+1))
done
```
Esto imprimirá el valor de las variables a `stdout` junto con otra información útil:
```bash
+ num=0
+ [[ 0 -lt 3 ]]
+ echo 0
+ num=1
+ [[ 1 -lt 3 ]]
+ echo 1
+ num=2
+ [[ 2 -lt 3 ]]
+ echo 2
+ num=3
+ [[ 3 -lt 3 ]]
```

A veces necesitamos depurar una parte del script. En este caso usando el comando `set`. Este comando puede habilitar o deshabilitar opciones. Las opciones se activan usando `-` y se apagan usando `+`:
```bash
#!/bin/bash

echo "xtrace está agado"
set -x
echo "xtrace está habilitado"
set +x
echo "xtraces está apagado de nuevo"
```
