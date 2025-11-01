como siempre empezamos con un scaneo de nmap

![[Pasted image 20251030115808.png]]

![[Pasted image 20251030115834.png]]

vemos que tenemos un servicio ssh en el puerto 22 y un servicio http en el puerto 80

vemos que nos da una pagina, la agreagamos al /etc/hosts usando sudo nano /etc/hosts
![[Pasted image 20251030120645.png]]

![[Pasted image 20251030120754.png]]

accemos a la pagina mediante la url o la ip de THM, nos encontramos con un login simple

intentamos fuerza bruta con admin:admin, admin:password y no funciona, tambien los ataques de SQLI tampoco funcionaban
 
utilizamos este script de python
![[Pasted image 20251030120943.png]]

![[Pasted image 20251030124927.png]]

ejecutamos el script y encontramos las credenciales

probemos un ataque de fuerza bruta en jose 

![[Pasted image 20251030130124.png]]
Basicamente aqui le estamos diciendo a hydra (herramienta para hacer ataques de fuerza bruta) que en el path.php que con el usuario jose pruebe todas las contrasenas que estan dentro de rockyou.txt, si la contrasena es incorrecta que pase a la siguiente


![[Pasted image 20251030130320.png]]

aqui encontramos la contrasena para jose

![[Pasted image 20251030130420.png]]

![[Pasted image 20251030130706.png]]

vemos que nos esta redirigiendo a otra pagina, vamos a agregarla al /etc/hosts

![[Pasted image 20251030131131.png]]

vemos que estamos en en una pagina llamada elFinder 

![[Pasted image 20251030131218.png]]

vemos que la version es la 2.1.47 vamos a ver cuales vulnerabilidades hay para esa version 

![[Pasted image 20251030131919.png]]
![[Pasted image 20251030131945.png]]

utilizamos metaesploit y vemos esto

![[Pasted image 20251030132941.png]]

tenemos que poner la opcion RHOSTS en  files.lookup.thm y la LHOSTS a nuestra ip

![[Pasted image 20251030133357.png]]



![[Pasted image 20251030133428.png]]
ya tenemos una shell
para entrar a la shell solo tenemos que poner shell en meterpreter

Ahora va la parte de escalada de privilegios pt1


leyendo el /etc/passwd vemos que hay un usuario think y root

![[Pasted image 20251030133721.png]]

vamos a chequear el directorio de think 

![[Pasted image 20251030134530.png]]



![[Pasted image 20251030134809.png]]

hay un archivo .passwords pero no lo podemos leer porque no tenemos permisos

vamos a buscar binarios SUID
![[Pasted image 20251030135239.png]]
![[Pasted image 20251030135228.png]]

hay un archivo /usr/sbin/pwm que se escapa dentro de lo default de linux

![[Pasted image 20251030135436.png]]

este archivo pertenece a root

vamos a ver que pasa cuando lo ejecutamos 

![[Pasted image 20251030135542.png]]

el binario ejecuta el comando "ID" y luego extrae el username y luego pone el username dentro de /home/<username>/.passwords e intenta hacer algo con eso

vamos a intentar enganar al programa ejecutando un comando de id diferente uno que resulte extrayendo el username de think

podemos anadir /tmp a nuestro path con export PATH=/tmp:$PATH
