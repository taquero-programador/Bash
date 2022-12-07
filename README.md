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
`$PDW` | El directorio de trabajo actual
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
Hay una diferecia importante entre comillas simples y dobles. Dentro de las comillas dobles. las variables o sustituciones de comandos se expanden. Dentro de comillas simples no lo son. Por ejemplo:
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
