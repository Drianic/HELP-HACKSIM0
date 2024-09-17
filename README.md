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

Si observamos podemos ver que tiene varios puertos abiertos, hagamos un escaneo con nmap para ver que versiones de estos puertos tienen.
<p align="left">
  <img src="https://i.postimg.cc/bv26b42R/imagen-2024-09-16-233139363.png" width="75%" height="75%" align="">
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
Bien
