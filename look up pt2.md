![imagen](images/Pasted%20image%2020251030145008.png)

ahora en nuestra maquina creamos el siguiente comando 

![imagen](images/Pasted%20image%2020251030145121.png)

lo pegamos en la maquina victima y le damos permisos de ejecutar chmod +x /tmp/id

cuando lo ejecutamos extraemos a think como el username y nos tira devuelta una lista de contrasenas

nos llevamos las contrasenas a nuestra maquina 

![imagen](images/Pasted%20image%2020251030145243.png)

y tratamos de hacer un ataque de fuerza bruta con la lista 

con hydra
![imagen](images/Pasted%20image%2020251030145446.png)

nos conectamos via ssh a think 
![imagen](images/Pasted%20image%2020251030145528.png)

![imagen](images/Pasted%20image%2020251030145619.png)

![imagen](images/Pasted%20image%2020251030145644.png)

aqui esta la flag de user

# Escalada de privilegios parte 2

con sudo -l podemos ver los comandos que puede ejecutar think como root

![imagen](images/Pasted%20image%2020251030145824.png)

y vemos que puede utilizar el comando look

En https://gtfobins.github.io/gtfobins/look/#sudo vemos que el comando look
tiene un exploit

![imagen](images/Pasted%20image%2020251030145916.png)

entonces segun la pagina podemos usar el binario para leer archivos en el sistema y como lo hacemos con root podemos leer cualquier archivo

![imagen](images/Pasted%20image%2020251030150159.png)

usamos el exploit para leer el archivo /root/.ssh/id_rsa y vemos que nos da la clave 

nos llevamos la clase a nuestra maquina 
![imagen](images/Pasted%20image%2020251030150338.png)
![imagen](images/Pasted%20image%2020251030150348.png)

y asi terminamos la maquina look up


