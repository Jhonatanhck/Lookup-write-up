como siempre empezamos con un scaneo de nmap  
![escaneo nmap](images/Pasted image 20251030115808.png)

![resultado nmap detalle](images/Pasted%20image%2020251030130706.png)

vemos que nos esta redirigiendo a otra pagina, vamos a agregarla al /etc/hosts

![redirección / hosts](images/Pasted%20image%2020251030131131.png)

vemos que estamos en en una pagina llamada elFinder

![elfinder pagina](images/Pasted%20image%2020251030131218.png)

vemos que la version es la 2.1.47 vamos a ver cuales vulnerabilidades hay para esa version

![version elfinder 2.1.47 - 1](images/Pasted%20image%2020251030131919.png)  
![version elfinder 2.1.47 - 2](images/Pasted%20image%2020251030131945.png)

utilizamos metaesploit y vemos esto

![metasploit exploit info](images/Pasted%20image%2020251030132941.png)

tenemos que poner la opcion RHOSTS en  files.lookup.thm y la LHOSTS a nuestra ip

![metasploit RHOSTS LHOSTS](images/Pasted%20image%2020251030133357.png)



![meterpreter ejecución](images/Pasted%20image%2020251030133428.png)  
ya tenemos una shell  
para entrar a la shell solo tenemos que poner shell en meterpreter

Ahora va la parte de escalada de privilegios pt1

leyendo el /etc/passwd vemos que hay un usuario think y root

![etc passwd muestra usuarios](images/Pasted%20image%2020251030133721.png)

vamos a chequear el directorio de think

![listar home think - 1](images/Pasted%20image%2020251030134530.png)



![listar home think - 2](images/Pasted%20image%2020251030134809.png)

hay un archivo .passwords pero no lo podemos leer porque no tenemos permisos

vamos a buscar binarios SUID  
![busqueda binarios SUID - 1](images/Pasted%20image%2020251030135239.png)  
![busqueda binarios SUID - 2](images/Pasted%20image%2020251030135228.png)

hay un archivo /usr/sbin/pwm que se escapa dentro de lo default de linux

![pwm en /usr/sbin](images/Pasted%20image%2020251030135436.png)

este archivo pertenece a root

vamos a ver que pasa cuando lo ejecutamos

![ejecución pwm muestra output](images/Pasted%20image%2020251030135542.png)

el binario ejecuta el comando "ID" y luego extrae el username y luego pone el username dentro de /home/<username>/.passwords e intenta hacer algo con eso

vamos a intentar enganar al programa ejecutando un comando de id diferente uno que resulte extrayendo el username de think

podemos anadir /tmp a nuestro path con `export PATH=/tmp:$PATH`
