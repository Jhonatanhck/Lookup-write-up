como siempre empezamos con un scaneo de nmap

![escaneo nmap](images/Pasted%20image%2020251030115808.png)

![resultado nmap detalle](images/Pasted%20image%2020251030115834.png)

vemos que tenemos un servicio ssh en el puerto 22 y un servicio http en el puerto 80

vemos que nos da una pagina, la agreagamos al /etc/hosts usando sudo nano /etc/hosts  
![hosts edit](images/Pasted%20image%2020251030120645.png)

![hosts guardado](images/Pasted%20image%2020251030120754.png)

accesamos a la pagina mediante la url o la ip de THM, nos encontramos con un login simple

intentamos fuerza bruta con admin:admin, admin:password y no funciona, tambien los ataques de SQLI tampoco funcionaban

utilizamos este script de python  
![script python](images/Pasted%20image%2020251030120943.png)

![salida script](images/Pasted%20image%2020251030124927.png)

ejecutamos el script y encontramos las credenciales

probemos un ataque de fuerza bruta en jose 

![hydra configuracion](images/Pasted%20image%2020251030130124.png)  
Básicamente aquí le estamos diciendo a hydra (herramienta para hacer ataques de fuerza bruta) que en el path.php con el usuario jose pruebe todas las contraseñas que están dentro de rockyou.txt; si la contraseña es incorrecta que pase a la siguiente.

![hydra ejecución](images/Pasted%20image%2020251030130320.png)

aqui encontramos la contrasena para el loging

![password encontrada](images/Pasted%20image%2020251030130420.png)

![login jose 1](images/Pasted%20image%2020251030130706.png)

vemos que nos esta redirigiendo a otra pagina, vamos a agregarla al /etc/hosts

![hosts añadir nueva ruta](images/Pasted%20image%2020251030131131.png)

vemos que estamos en en una pagina llamada elFinder 

![elfinder pagina](images/Pasted%20image%2020251030131218.png)

vemos que la version es la 2.1.47 vamos a ver cuales vulnerabilidades hay para esa version 

![vuln 1](images/Pasted%20image%2020251030131919.png)  
![vuln 2](images/Pasted%20image%2020251030131945.png)

utilizamos metasploit y vemos esto

![metasploit](images/Pasted%20image%2020251030132941.png)

tenemos que poner la opcion RHOSTS en files.lookup.thm y la LHOSTS a nuestra ip

![msf RHOSTS LHOSTS](images/Pasted%20image%2020251030133337.png)

![meterpreter exploit run](images/Pasted%20image%2020251030133428.png)  
ya tenemos una shell  
para entrar a la shell solo tenemos que poner `shell` en meterpreter

Ahora va la parte de escalada de privilegios pt1

leyendo el `/etc/passwd` vemos que hay un usuario `think` y `root`

![etc passwd](images/Pasted%20image%2020251030133721.png)

vamos a chequear el directorio de `think`

![listar home think 1](images/Pasted%20image%2020251030134530.png)

![listar home think 2](images/Pasted%20image%2020251030134809.png)

hay un archivo `.passwords` pero no lo podemos leer porque no tenemos permisos

vamos a buscar binarios SUID  
![suid find 1](images/Pasted%20image%2020251030135239.png)  
![suid find 2](images/Pasted%20image%2020251030135228.png)

hay un archivo `/usr/sbin/pwm` que se escapa dentro de lo default de linux

![pwm encontrado](images/Pasted%20image%2020251030135436.png)

este archivo pertenece a root

vamos a ver que pasa cuando lo ejecutamos 

![ejecución pwm](images/Pasted%20image%2020251030135542.png)

el binario ejecuta el comando `id` y luego extrae el username y luego pone el username dentro de `/home/<username>/.passwords` e intenta hacer algo con eso

vamos a intentar engañar al programa ejecutando un comando `id` diferente uno que resulte extrayendo el username de `think`

podemos añadir `/tmp` a nuestro `PATH` con `export PATH=/tmp:$PATH`

CONTINUACION

https://github.com/Jhonatanhck/Lookup-write-up/blob/main/look%20up%20pt2.md

