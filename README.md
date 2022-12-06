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
```bash
user@host:~$
```

Aquí puede ingresar una variedad de comandos Unix, como `ls`, `grep`, `cd`, `mkdir`, `rm` y ver el resultado de su ejecución.

Llamamos a este shell interactivo porque interactúa directamante con el usuario.

Usar una terminal virtual no es realmente conveniente. Por ejemplo, si desea editar un documento y ejecutar otro comando la mismo tiempo, es mejor usar emuladores de terminales virtuales como:
- Terminal GNOME
- Terminator
- iTerm2
- ConEmu

## Modo no interactivo
En el modo no interactivo, el shell lee los comandos de un archivo o una tubería y los ejecuta. Cuando el intérprete llega al final del archivo, el proceso de shell finaliza la sesión y vulver al proceso principal.

Comando para ejecutar el shell en modo no interactivo:
```bash
sh my_script.sh
# or
bash my_script.sh
```

Puede simplificar la invicación del script haciendolo un archivo ejecutable:

    chmod +x my_script.sh

Además, la primera línea del script debe indicar qué programa debe usar para ejecutar el archivo:
```bash
#!/bin/bas
echo "Hola, Linux!"
```
