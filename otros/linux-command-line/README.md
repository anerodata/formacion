# Introducción a Linux y a la línea de comandos

[Curso de Frontend Masters](https://frontendmasters.com/courses/linux-command-line/). El curso está documentado [aquí](https://btholt.github.io/complete-intro-to-linux-and-the-cli/) por Brian Holt.

Sitemas operativos como Windows u otros de tipo Unix como MacOS o Linux en cualquiera de sus distribuciones incluyen un _shell_ o interprete de comandos y lenguaje de órdenes. El programa donde este _shell_ es ejecutado se conoce como emulador (Terminator para Linux, Terminal para MacOS O Windows Terminal para windows). 

En el caso de Linux, el _shell_ se llama _Bourne Against Shell (bash)_ en honor a Bourne, el antiguo _shell_. _bash_ es un _shell_ que tiene 30 años y es el más común. ZSH o _Z shell_ es el _shell_ de MacOS (basado en UNIX como Linux). Windows tiene _Windows PowerShell_ y _Command Prompt_ (cmd.exe).

_Shell_ tiene una lógica basada en _Read Evaluate Print Loop_ (REPL), es decir, envías un comando una vez y el interprete evalua, ejecuta un proceso y ocasionalmente imprime un resultado. También es posible ejecutar una linea que ejecute multiples lineas (_looping_).

### Comandos

- `pwd --help`: Información sobre `pwd`
- `ls anerodata`: `ls` es un comando `anerodata` sería un parámetro
- `which ls`: dónde está ese programa? está en `bin` (binary): la carpeta dónde están todos los programas ejecutables
- `ls -l`: `-l` es una bandera. Te da información como la fecha de creación, la hora
- `ls -l -a`: Lo anterior más los directorios y los archivos ocultos. `ls --all` es la manera larga de escribir la bandera como `-a`. Una manera comprimida de ejecutar el comando sería `ls -la`. `--all` es una bandera con dos guiones porque si fuera uno, `bash` entendería que quieres concatenar banderas como en `ls -la`.
- `tail -n 3 ~/.bash_history`: imprime las 3 últimas lineas de un fichero. Util para imprimir logs.
- `head`: Imprime las diez primeras líneas de un fichero.
- `tail -f`: Imprime las ultimas lineas del fichero en tiempo real. Útil par logs.
- `!!`: ejecuta el último comando ejecutado
- `less`: + el nombre del archivo, lo lee. Dentro, `/texto` busca el texto en el fichero.
- `more`: la versión antigua de `less`.
- `man`: es un programa que usa `less`. Muestra el manual de un programa
- `cat`: + el nombre del fichero, lo imprime en la consola (_concatenate to the standart out_).
- `rm`: borra directorios
- `rm -f`: borra directorios y su contenido recursívamente
- `rm -rf`: igual pero con _force_. Es decir, no pregunta si estás seguro o no (para módulos de node o repositorios).
- `trash`: [instalar](https://www.tecmint.com/trash-cli-manage-linux-trash-from-command-line/). Manda el fichero a _trash_.
- `cp -R file directory`: lleva el fichero al directorio, lo crea si no existe.
- `rm -r -- !(README.md)`: borra todo menos ese fichero.

#### Archivos comprimidos

- `tar -cf archive.tar pepe/ README.md`: Crea un archivo sin comprimir.
- `tar -zcf archive.tar.gz pepe/ README.md`: crea un archivo comprimido 
- `tar -xzf archive.tar.gz -C some-folder`: extrae un fichero comprimido en `some-folder` (tiene que haber sido creada)

#### _Wildcards and replacements_

- `touch file{1,2,3,pepe}.txt`: {} = _expansion_. Añade _file_ delante de cada valor y crea un fichero para cada uno. Aplica para otros programas como `rm`.
- `ls file*.txt`: * = _wildcard_. Lista todos los ficheros que comiencen por file y terminen por .txt
- `ls file?.txt`: Lista todos los ficheros que comiencen por file y terminen por .txt, descartando `file-1.txt` por ejemplo. `ls file-??.txt`, listaría `file-12.txt`, pero no `file-1.txt`.
- `touch files/file-{1..50}.txt`. Dentro de `files`, crea 50 ficheros.
- `echo {a..z..10}`: imprime `a k u`.
- `echo {a..z}{1..2}`: imprime `a1 a2 b1 b2`...

#### _Streams_

_One of the Linux philosophies is the assumption that the output of a program can be the input to another program through streams and pipes, then is it possible to redirect the output to another file._

##### _Output streams_

- `echo 'text' 1> text.txt`: `>1` redirecciona (_standard out_). Convierte el _output_ del programa _echo_ en el _input_ del fichero `text.txt` y borra todo lo anterior. `echo 'text' 1>> text.txt`: añade el contenido, no borra lo anterior.
- `cat text.text 1> text-2.txt`: con la filosofía anterior, estaríamos copiando un programa.
- `ls -lash 1>> text2.txt`: lleva el output del listado a `text2.txt`.
- `cat non-existant-file.txt 2> error.txt`: `>2` redirecciona (_standard error_). Convierte un _output_ de error en en el _input_ de otro fichero. El fichero no existe, por eso produce un error.
- `ls -lash file.txt 1> ls.txt 2> error.txt`: Separación de _streams_. los errores iran en `error.txt`.
- `ls -lash 1> /dev/null`. Solo imprimirá errores. El resto de mensajes, no. Para ejecutar un programa del que solo queramos saber los errores.

##### _Input streams_

- `cat < file.txt`: `<` = _standard in_. Envia el archivo como _input_ a `cat` y este programa lo devuelve como  _output_.
- `grep ".md\|.txt" < file.txt`: coge el conteindo del fichero y se lo lanza a grep como _input_ el _output_ que devuelve `grep` (programa que filtra) es la línea del fichero que sea `.md` o `.txt`.
- `greap ".md" < file.txt 2> error.txt`: mandamos el error a `error.txt`. `greap` no existe con lo cual el error irá al fichero. Si el comando estuviera bien escrito se mostraría en consola la línea del `README.md` y no escribiría nada en el fichero de errores.
- `grep .md file.txt > output.txt 2> /dev/null`: filtra el `README.md`, se lleva el resultado al `output.txt`

### _Pipes_ o "conductores"

_In order to move from one program to another within the command line, developers have to use pipes._

- `cat output.txt | grep .md`: _`cat` takes the output of the file and pipes into `grep`_. 
- `tasklist.exe | grep firefox`: filtra los procesos de windows en los que ponga "firefox".
- `yes n | rm -i file*`: responde "no" a borrar todos los ficheros.

### Atajos

Para reconfigurar comandos, [aquí](https://btholt.github.io/complete-intro-to-linux-and-the-cli/signals-and-the-power-of-ctrl).

- `CTRL + R`: _reverse search_. Para buscar comandos.
- `CTRL + SHIFT + C` y `CTRL + SHIFT + C`: _copy_ y _paste_ en el _bash_ cuando estamos en Windows.
- `CTRL + A`: takes you to the beginning of the line
- `CTRL + E`: takes you to the end of the line
- `CTRL + K`: "yank" everything after the cursor
- `CTRL + U`: "yank" everything before the cursor
- `CTRL + Y`: "paste" (paste in quotes because it doesn't actually go into your system clipboard) everything you yanked
- `CTRL + L`: clear the screen
- `CTRL + R`: reverse search through history

### Señales

`kill -l` lista todas las señales. Son usadas por los procesos para comunicarse entre ellos o con _shell_, como el caso de SIGALARM.

- `CTRL + C` (SIGINT): interrumpe un proceso.
- `CTRL + D` (SIGQUIT): _quit_. Cierra la sesión del _bash_. Dentro de un programa REPL (Python), este comando lo que haría sería cerrarlo.
- SIGTERM: no tiene atajo, pero es la señal que se envía cuando se usa `kill` + programa. Ocurre cuando el sistema operativo se apaga. Envía SIGTERM  a todos los programas para avisar de que se va a cerrar. Una vez que todos se cierran, cierra la terminal.
- `kill -9` (SIGKILL): detiene un la ejecución de un programa inmediatamente.
-  `echo hi >> README.md`: imprime hi en el fichero, si no existe, lo crea.
- `mkdir -p uno/dos/tres`: anida directorios dentro del directorio uno.

### Usuarios

- `cat /etc/passwd`: imprime los usuarios de esa máquina. Todos los que no tienen `bash` en la ruta significa que no pueden acceder a ella. Ni siquiera teniendo la credencial. Es positivo para evitar hackeos.

Si intentamos hacer un directorio en `/`, no podemos. Solo el usuario `root` tiene permisos para operar ahí: borrar, crear...

- `whoami`: me dice `anerodata`, pero `sudo whoami`, `root`. `sudo` nos permite operar como `root` y volver a nuestro usuario inicial tras acabar la operación.
- `sudo su`: accedemos a `root`.

Creamos un nuevo usuario para realizar una tarea determinada en un directorio, estando como registrado como `root`.
- `useradd pepe`: crea un nuevo usuario.
- `passwd pepe`: contraseña.

Al hacer `cat /etc/passwd` podemos leer los usuarios de la máquina. `pepe:x:1001:1001::/home/pepe:/bin/sh`. Las dos primeras cifras son ID de grupo, la penúltima ruta es su _home_ (directorio al que va al loggearse) y la siguiente es su _shell_.

### Grupos

Usamos los grupos para dar x permisos a x usuarios.

- `sudo usermod` -aG sudo pepe: le da permisos de `sudo` a pepe. Ahora puede usar `sudo`. `-aG`: `a` mete en el grupo, `G` indica en cuál.

### Permisos

Estando en un _Windows Subsystem Linux_ esto solo funcionará en _home_ o _root_ pero no en `/mnt/c/`.

`drwxr-xr-x`: `d` indica directorio, si fuera `-` sería fichero. después `rwx` (_read_, _write_, _execute_ por el usuario en cuestión) + `rwx` (por el grupo del usuario en cuestión) + `rwx` (el resto). _Execute_ se produce al hacer un `ls` sobre el directorio por ejemplo.

`sudo chown anerodata:anerodata folder/`: _change ownership_. Cambia la propiedad del directorio a `anerodata` y al grupo `anerodata`.

`sudo chmod u=rwx,g=rwx,o=rwx hello/`: cambiría los permisos a _read, write and execute_ para el usuario, el grupo y los otros en ese directorio.

`sudo chmod u=rwx, g=rwx, o=rwx file.txt` = `sudo chmod 777 file.txt`.

`sudo chmod -x`: quita los permisos de ejecutar a todos.

### Editores de texto

Vim y Nano. Vim tiene varios modos, el modo comando y el modo de edición. Pulsando sobre `ESC` pasamos al modo de comando.

Algunos comandos de vim:

- `:q` - salir.
- `:q!` - salir sin importar que no haya cambios guardados.
- `:qa!` - salir forzadamente.
- `:d` - borrar una línea.
- `:d100` - desde la primera linea, borra desde la primera hasta la 100.
- `:wq!` - escribir y salir.

### Ejercios

- **Detener un programa**: `yes > /dev/null &` Ejecuta yes en el _background_. `ps aux | grep yes` muestra todos los programas que se llamen _yes_ que se están ejecutando. Con su ID lo detenemos así `kill -9 428`.