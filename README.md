# Bandit

### ✅ **Level 0**

**The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1page to find out how to beat Level 1.**

Nos conectamos por ssh

`ssh bandit0@bandit.labs.overthewire.org -p 2220` 

Otro método:

`sshpass -p 'bandit0' ssh bandit0@bandit.labs.overthewire.org -p 2220`

---

---

### ✅ **Level 0 → Level 1**

**The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.**

Listamos con `ls` y hacemos un `cat` al archivo `readme`

---

---

### ✅ **Level 1 → Level 2**

**The password for the next level is stored in a file called - located in the home directory.**

En este caso nos dicen que la contraseña se encuentra archivo `-`, y como no podemos realizar un `cat` con un `-`, realizamos el `cat` pasándole la ruta completa del archivo `cat $(pwd)/-`

---

---

### ✅ **Level 2 → Level 3**

**The password for the next level is stored in a file called spaces in this filename located in the home directory.**

Como el nombre del archivo donde se encuentra el pass es `“spaces in this filename”`, tenemos que realizar el `cat` con `‘’`

`cat 'spaces in this filename’`

---

---

### ✅ **Level 3 → Level 4**

**The password for the next level is stored in a hidden file in the inhere directory.**

El pass se encuentra en un archivo oculto, por lo que realizamos `ls -a` para que se nos listen todos los archivos, incluidos los ocultos.

---

---

### ✅ **Level 4 → Level 5**

**The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.**

Como el archivo el cuál contiene la pass nos indica que es el único leíble por el humano, realizamos una búsqueda enlazándola con `xargs`para que nos muestre el tipo de archivo que es cada uno: 

`find . | xargs file`

---

---

### ✅ **Level 5 → Level 6**

**The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:**

- **human-readable**
- **1033 bytes in size**
- **not executable**

Con la descripción del archivo, realizamos una búsqueda acotándonos a lo que nos piden: 

`find . -size 1033c | xargs file`

---

---

### ✅ **Level 6 → Level 7**

**The password for the next level is stored somewhere on the server and has all of the following properties:**

- **owned by user bandit7**
- **owned by group bandit6**
- **33 bytes in size**

Con la descripción que tenemos, debemos de buscar, en todo el servidor, con las características que nos proporcionan, pasando en esta búsqueda el patrón `2>/dev/null` para evitar que nos muestre los permisos denegados: 

`find . -user bandit7 -group bandit6 2>/dev/null`

---

---

### ✅ **Level 7 → Level 8**

**The password for the next level is stored in the file data.txt next to the word millionth.**

Nos dice que el pass se encuentra dentro del archivo data.txt cerca de la palabra millionth, así que realizamos la búsqueda con `grep` para que nos busque esa palabra en cuestión: 

`cat data.txt | grep millionth`

---

---

### ✅ **Level 8 → Level 9**

**The password for the next level is stored in the file data.txt and is the only line of text that occurs only once.**

Como nos dice que el pass que se encuentra en el archivo, es el único que se repite una vez, ordenamos las cadenas de texto y contamos las veces que aparecen cada una de ellas, para quedarnos con la única:

`cat data.txt | sort | uniq -c`

---

---

### ✅ **Level 9 → Level 10**

**The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.**

Como el pass se encuentra dentro del archivo como “human readable” y cerca de los caracteres ´=´, mostramos los strings del archivo: 

`cat data.txt | strings`

Otra manera:

`cat data.txt | strings | grep ==` 

---

---

### ✅ **Level 10 → Level 11**

**The password for the next level is stored in the file data.txt, which contains base64 encoded data.**

Como el archivo contiene datos codificados en base 64, mostramos estos decodificando los datos: 

`cat data.txt | base64 -d`

---

---

### ✅ **Level 11 → Level 12**

**The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.**

El pass se encuentra dentro del archivo, pero se le ha aplicado una rotación de 13 posiciones a los caracteres, por lo que utilizaremos el comando `tr` pasándole como parámetros todo el abecedario en mayúsculas y minúsculas y la rotación que tiene: 

`cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m’`

---

---

### ✅ **Level 12 → Level 13**

**The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)**

Nos encontramos ante un archivo con un dumpeo hexadecimal que se ha comprimido varias veces, por lo que nos copiaremos el contenido del archivo en nuestra máquina, ya que utilizaremos la herramienta 7z y en la máquina bandit no la tenemos disponible, y a este archivo que hemos creado le pasamos el comando `xxd -r`para reversearlo y `sponge` para guardar la salida en el mismo archivo:

`cat leve12 | xxd -r | sponge leve12` Puede que no nos funcione el comando sponge, y en el caso que no queramos instalar la herramienta, podemos crear un archivo nuevo ya reverseado: `cat leve12 | xxd -r > copyleve12`

Después de esto, hacemos un `file leve12` para que nos muestre que tipo de archivo es, donde vemos que nos encontramos ante un archivo gzip. Esto que hemos hecho, lo podíamos haber buscado de forma manual, consultando en internet alguna lista de firmas hexadecimales y buscando aquella que nos arroja el archivo `1f8b 0808 0650` 

Como hemos visto que nos encontramos ante un tipo de archivo comprimido, lo renombramos `mv leve12 leve12.gz` para poder trabajar con el, descomprimiéndolo con `7z x leve12.gz`  y realizando este proceso las veces que sean necesarias, ya que cada nos van apareciendo cada vez más comprimidos, hasta que demos con nuestra pass.

Todo el proceso anterior lo podemos automatizar con un script en bash. Recordemos que para ejecutar scripts en bash, estos tienen que tener permisos de ejecución: `chmod +x script.sh`

```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n[!] Saliendo...\n"
  exit 1
}

# Ctrl+C
trap ctrl_c INT

first_file_name="leve12.gz"
decompressed_file_name="$(7z l leve12.gz | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"

7z x $first_file_name &>/dev/null

while [ $decompressed_file_name ]; do
  echo -e "\n[+] Nuevo archivo descomprimido: $decompressed_file_name"
  7z x $decompressed_file_name &>/dev/null
  decompressed_file_name="$(7z l $decompressed_file_name | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
done
```

---

---

### ✅ **Level 13 → Level 14**

**The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on.**

Listando los archivos del directorio, nos encontramos ante una clave privada ssh la cuál utilizaremos para conectarnos al usuario bandit14 por el localhost: `ssh -i sshkey.private bandit14@localhost -p 2220` 

Una vez dentro, listaremos el directorio que nos indican (`/etc/bandit_pass/bandit14`) para poder leer la clave del usuario bandit14, ya que solo él pude acceder a ella.

---

---

### ✅ **Level 14 → Level 15**

**The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.**

Utilizaremos el servicio **netcat** con el comando `nc`, para poder conectarnos al puerto 30000:

`nc localhost 30000`

Una vez nos conectados tenemos que proporcionar la contraseña del nivel actual para recibir la nueva pass.

→ en lugar de localhost podemos utilizar `127.0.0.1`

---

---

### ✅ **Level 15 → Level 16**

**The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.**

**Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**

Nos piden utilizar una conexión encriptada ssl en el puerto 30001, por lo que utilizaremos la herramienta `ncat`, la cuál nos permite encriptar el tráfico por ssl con `—ssl`: 

`ncat --ssl localhost 30001`

Una vez nos conectados tenemos que proporcionar la contraseña del nivel actual para recibir la nueva pass.

→ en lugar de localhost podemos utilizar `127.0.0.1`

---

---

### **✅ Level 16 → Level 17**

**The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.**

Podemos realizar el escaneo de puertos con `nmap --open -T5 -v -p31000-32000 127.0.0.1` y una vez listados los puertos abiertos, hacer una conexión encriptada ssl con `ncat —ssl localohost puertoabierto` y pasarle la contraseña del nivel actual para recibir una pass.

Una vez consigamos conectarnos al puerto correcto, este nos devolverá una clave privada, la cuál copiaremos para crearnos nuestra clave privada ssh, que utilizaremos para conectarnos como hicimos en el level 13:

→ Lo primero que haremos es crearnos un directorio temporal para poder trabajar: `mktemp -d`

→ Iremos al directorio que se nos ha creado.

→ Creamos nuestro archivo que utilizaremos como clave privada ssh, copiando dentro la clave privada que nos ha proporcionado anterirmente: `nano sskey.private`

→ Le asignamos el privilegio 600, que es el que tienen que disponer las claves privadas: `chmod 600 sskey.private`

→ Utilizamos la clave privada que hemos creado para conectarnos a bandit17: `ssh -i sskey.private bandit17@localhost -p 2220`

→ Una vez conectados como bandit17, podemos obtener el pass en la ruta donde se almacenan estas: `cat /etc/bandit_pass/bandit17`

Alternativamente, podemos crearnos un script en bash para descubrir también estos puertos abiertos, en donde mandamos un paquete vacío para recibir respuesta de aquellos que están abiertos. Recordemos que para ejecutar scripts en bash, estos tienen que tener permisos de ejecución: 

`chmod +x script.sh`:

```bash
#!/bin/bash

function ctrl_c(){
  echo -e "\n\n[!] Saliendo...\n"
}

# Ctrl+C
trap ctrl_c INT

for port in $(seq 31000 32000); do
  (echo '' > /dev/tcp/127.0.0.1/$port) 2>/dev/null && echo "[+] $port - OPEN" &
done; wait
```

---

---

### ✅ **Level 17 → Level 18**

**There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

Utilizamos `diff passwords.new passwords.old` para ver cuál es la diferencia entre los archivos, siendo `<` el dato nuevo y `>` el viejo.

---

---

### ✅ **Level 18 → Level 19**

**The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.**

Utilizaremos la conexión ssh típica, pero añadiremos al final de la instrucción bash para que no procese el `.basrc` que nos expulsa de la máquina: `sshpass -p 'kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd' ssh bandit18@bandit.labs.overthewire.org -p 2220 bash`

Una vez que tenemos nuestra bash, navegamos por los directorios hasta el archivo que nos indican que tiene el pass.

---

---

### ✅ **Level 19 → Level 20**

**To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.**

Nos encontramos ante un archivo SUID, el cuál se ejecuta como propietario de manera temporal, por lo que podemos hacerlo de dos formas: `./bandit20-do bash -p`y siendo ya bandit20, navegamos hasta la ruta indicada para conseguir nuestra pass: `cat /etc/bandit_pass/bandit20`

O directamente : `./bandit20-do cat /etc/bandit_pass/bandit20`

---

---

### **✅ Level 20 → Level 21**

**There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).**

**NOTE: Try connecting to your own network daemon to see if it works as you think**

Si listamos los archivos, vemos que tenemos un binario SUID en el cuál nos piden la conexión mediante un puerto, así que tenemos que ponernos en escucha por un puerto que nosotros especifiquemos con netcat: `nc -nlvp 4646`

Cuando ya hayamos puesto nuestro puerto en escucha, abrimos otra termina y nos conectamos a bandit 20 para conectarnos al puerto que hemos puesto en escucha anteriormente: `./suconnect 4646` 

Volvemos a la terminal donde nos hemos puesto en escucha, y mandamos la contraseña del nivel actual para recibir el pass de bandit21.

---

---

### **✅ Level 21 → Level 22**

**A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.**

Estamos ante un nivel en el que una tarea está corriendo automáticamente a intervalos regulares de tiempo mediante CRON.

Si vamos hasta la ruta que nos dicen `/etc/cron.d`, tenemos que fijarnos en la de bandit22 (ya que es la que queremos conseguir su pass), y para ello, hacemos un `cat cronjob_bandit22`, devolviéndonos:

`* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null`

Como vemos, cada minuto se ejecuta un script de bash, echándole un ojo a este script con `cat /usr/bin/cronjob_bandit22.sh` podemos ver lo siguiente:

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Analizando el script vemos que:

→ se asigna el privilegio a la ruta indicada

→ hace un cat de su contraseña (ya que recordemos que esta tarea la ejecuta el usuario bandit22)

Haciendo un `ls -l` a la ruta donde se encuentra alojada la contraseña, `ls -l /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` vemos que otros usuarios pueden leer:

`rw-r--r-- 1 bandit22 bandit22 33 Sep 3 09:58 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`

Así que, aprovechándonos de esto, hacemos un cat a la ruta para obtener el pass:

`cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`

---

---

### **✅ Level 22 → Level 23**

**A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.**

**NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.**

Igual que en el nivel anterior, tenemos una tarea que se ejecuta de manera regular a intervalos de tiempo.

Vamos a la ruta que nos indican cd `/etc/cron.d` y nos fijamos en la tarea cron del usuario bandit23:

`@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null`

`* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null`

Como vemos, cada minuto, el usuario bandit23 va ejecutar un script en bash, así que vamos a analizarlo con `cat /usr/bin/cronjob_bandit23.sh`

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

El usuario bandit23, está haciendo un cat a la ruta `/tmp/$mytarget` de la contraseña. Vemos que 

`mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)`

Siendo `myname=$(whoami)` bandit23 (recordemos que esta tarea CRON la ejecuta el usuario bandit23) podemos completar la ruta para poder ver el pass, para ello hacemos `echo I am user bandit23 | md5sum | cut -d ' ' -f 1` tal como lo indica el script y vemos el md5sum resultante para poder completar la ruta `/tmp/$mytarget`

---

---

### ✅ **Level 23 → Level 24**

**A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.**

**NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!**

**NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…**

Igual que en los niveles anteriores, vamos a la ruta que nos indican y vemos que el usuario bandit24 ejecuta un script a intervalos regulares de tiempo:

`@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null`

`* * * * *bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null`

Veamos el script:

```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

En el script vemos que se ejecutan y borran todos los scripts que haya almacenados en la ruta que nos indican. Si el creador del script es bandit23 se hace lo que tenemos en el do.

Si vamos a la ruta `/var/spool/bandit24/foo` y hacemos un `ls -l` vemos que no tenemos permisos, por lo que subimos un nivel y volvemos a realizar el `ls -l`, viendo los siguientes permisos:

`drwxrwx-wx 3 root bandit24 4096 Sep 3 17:02 foo`

Nosotros, al ser bandit23, tenemos los permisos de “otros”, y como vemos, no podemos leer los archivos, pero si podemos introducirnos en el directorio y crear archivos, así que volvamos al directorio `foo` para crear el script que necesitamos para obtener nuestro pass.

En el enunciado nos recomiendan que nos creemos una ruta temporal para no perder el script `temporal=$(mktemp -d)` y dentro de esta crearemos el script con `nano script.sh`(recordemos dar permiso de ejecución al script, así como permisos de escritura y entrada a nuestra carpeta temporal para que otros usuarios puedan acceder con `chmod o+wx /rutatemporal` ya que recordemos que es bandit24 el que ejecuta el script)

```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.pkLg0e70ad/pass.log
chmod o+r /tmp/tmp.pkLg0e70ad/pass.log
```

En el script, lo que hacemos es hacer un `cat` del pass de bandit24 y pasarlo a nuestra carpeta temporal como `pass.log` así como asignación de los permisos, para que otros usuarios lo puedan leer.

Creamos una copia en la ruta donde se ejecutará el script `cp script.sh /var/spool/bandit24/foo`

Hacemos un `watch -n 1 ls -l` para monitorizar lo que ocurre en nuestro directorio y podemos ver que se ha creado el archivo `pass.log` por lo que ya podemos obtener la pass.

---

---

### **✅ Level 24 → Level 25**

**A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.**

Nos dicen que tenemos que conectarnos por el puerto 30002 y proporcionar la clave del usuario bandit24 así como un pin de 4 dígitos para obtener el pass, y para ello vamos a recurrir a la fuerza bruta.

→ Nos creamos una carpeta temporal con `mktemp -d` 

→ Creamos un bucle que nos recorra el rango completo del pin y lo guardamos en un txt

`for i in {0000..9999}; do echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar" $i; done > combinaciones.txt`

→ Pipeamos el archivo txt con la conexión al puerto 30002, haciéndole un `grep` para que no nos muestre aquellas combinaciones incorrectas

`cat combinaciones.txt | nc localhost 30002 | grep -v "Wrong”`

Ya tenemos nuestra pass

---

---

### **✅ Level 25 → Level 26**

**Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.**

Si listamos con `ls` podemos ver que tenemos una clave encriptada para la conexión por ssh al usuario bandit26, pero no será suficiente, ya que como nos dicen, el usuario bandit26 no utiliza una bash, y si nos conectamos con la clave privada `ssh -i bandit26.sshkey bandit26@localhost -p2220` nos aparece lo siguiente:

```bash
For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

  _                     _ _ _   ___   __  
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to localhost closed.
```

Si listamos con `cat /etc/passwd | grep bandit26` nos arroja que estamos utilizando showtext en lugar de bash como hemos dicho anteriormente. Si hacemos un `cat` a la ruta que nos ha aparecido anteriormente nos aparece lo siguiente:

```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

Así que podemos ver que se utiliza el comando `more` que hace que se muestre el resultado de la ejecución en un formato “navegable”, por lo que tenemos que reducir nuestra terminal de tal manera que no nos muestre el texto completo y tengamos la opción de navegar por él, de esta manera podremos entrar al modo edición.

Lo primero que haremos será pulsar `v` para entrar en modo visual para poder ejecutar instrucciones y hacemos lo siguiente:

→ Pulsamos `escape shift :`

→ Definimos la variable de shell con `set shell=/bin/bash`

→ Volvemos a pulsar `escape shift :`

→ Escribimos `shell` lo que hace que gracias a que antes la hemos definido como una bash, nos entregue una bash y podamos navegar por los directorios. 

En este caso, podemos obtener la clave de bandit26, pero debido a todo lo descrito anteriormente, aunque realizáramos la conexión con esta contraseña, al no tener definida una bash, tendríamos que hacer el proceso anterior.

---

---

### **✅ Level 26 → Level 27**

**Good job getting a shell! Now hurry and grab the password for bandit27!**

Al listar el directorio vemos que nos encontramos ante un archivo SUID, así que nos aprovechamos de poder utilizar este archivo como su propietario, que es bandit27, para obtener el pass de la siguiente manera:

`./bandit27-do cat /etc/bandit_pass/bandit27`

---

---

### **✅ Level 27 → Level 28**

**There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo`. The password for the user `bandit27-git` is the same as for the user `bandit27`.**

**Clone the repository and find the password for the next level.**

Creamos un directorio temporal para poder clonar nuestro repositorio con `git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo` 

Una vez hemos clonado nuestro repo, navegamos hacia él y en el `README` tendremos la contraseña.

---

---

### **✅ Level 28 → Level 29**

**There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo`. The password for the user `bandit28-git` is the same as for the user `bandit28`.**

**Clone the repository and find the password for the next level.**

Creamos un directorio temporal para clonar nuestro repositorio `git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo`

En este caso el archivo README no nos arroja nada interesante, por lo que miraremos los commits que haya tenido este repo para ver si tiene algo interesante con `git log`

Comprobamos que se han realizado una serie de commits, y uno de ellos pone `fix info leak` por lo que, utilizando el identificador de este commit procedemos a revisarlo con `git show 43032edb2fb868dea2ceda9cb3882b2c336c09ec` 

Viendo los cambios que se han realizado en este commit conseguimos nuestra pass.

---

---

### **✅ Level 29 → Level 30**

**There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo`. The password for the user `bandit29-git` is the same as for the user `bandit29`.**

**Clone the repository and find the password for the next level.**

Creamos un directorio temporal para clonar nuestro repositorio `git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo`

Leyendo el archivo README del repo, vemos que en este caso, en el password aparece: “no passwords in production!”

Mirando los commits que se han realizado tampoco nos arroja nada, así que exploraremos las ramas que tenga el repositorio con `git branch -a` 

Cambiamos a la rama `dev` con `git checkout dev` y nos aparecerá la siguiente leyenda:

```bash
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
Switched to a new branch 'dev'
```

Listamos los archivos de esta rama, donde el archivo `README.md` nos arrojará la pass.

---

---

### **✅ Level 30 → Level 31**

**There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo`. The password for the user `bandit30-git` is the same as for the user `bandit30`.**

**Clone the repository and find the password for the next level.**

Creamos un directorio temporal para clonar nuestro repositorio `git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo`

En este caso ni el archivo `README`, ni los commits ni las ramas nos arrojan nada interesante, por lo que jugaremos con los tags de git con `git tag`

Cuando listamos los tags, nos aparece un archivo `secret` por lo que lo listamos con `git show secret` para conseguir nuestra pass.

---

---

### **✅ Level 31 → Level 32**

**There is a git repository at `ssh://bandit31-git@localhost/home/bandit31-git/repo`. The password for the user `bandit31-git` is the same as for the user `bandit31`.**

**Clone the repository and find the password for the next level.**

Creamos un directorio temporal para clonar nuestro repositorio `git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo`

Cuando leemos el archivo `README` tenemos las siguientes instrucciones:

```bash
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

Por lo que creamos el archivo con `nano key.txt` poniendo el contenido que nos indican y lo subimos a git de la siguiente manera:

→ `git add -f key.txt`

→ `git commit -m "Subimos el archivo key”` mostrándonos lo siguiente:

```bash
[master 915b55d] Subimos el archivo key
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
```

→ `git push -u origin master` y una vez pongamos la contraseña de bandit 31 nos aparecerá el siguiente mensaje con el pass:

```bash
bandit31-git@localhost's password: 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 333 bytes | 333.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: rmCBvG56y58BXzv98yZGdO7ATVL5dW8y 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://localhost:2220/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://localhost:2220/home/bandit31-git/repo'
```

---

---

### **✅ Level 32→ Level 33**

**After all this `git` stuff its time for another escape. Good luck!**

Cuando nos conectamos, se nos muestra el mensaje “WELCCOME TO THE UPPERCASE SHELL”, así que utilizaremos `$0` y pediremos una bash. Una vez que tengamos la bash, navegaremos hasta el directorio `/home/bandit33` para leer el README que nos indica el fin del juego.

---

---
