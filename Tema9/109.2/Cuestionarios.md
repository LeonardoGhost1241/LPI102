=======================================
# 109.2 Configuracion de red persistente
=======================================

-> Leccion 1

### Ejercicios guiados 

1. ¿Qué comandos se pueden utilizar para enumerar los adaptadores de red presentes en el sistema?

```
ip link o nmcli device o ifconfig
```


2. ¿Cuál es el tipo de adaptador de red cuyo nombre de interfaz es wlo1?

```
es un adaptador de tipo wireless (WLAN)
```

3. ¿Qué papel juega el archivo /etc/network/interfaces durante el arranque?

```
El archivo /etc/network/interfaces configura las interfaces, este archivo activa las interfaces en el arranque en el orden que aparecen en la lista
```

4. ¿Qué entrada en /etc/network/interfaces configura la interfaz eno1 para obtener su configuración IP con DHCP?

```
auto eno1
iface eno1 inet dhcp 
```

### Ejercicios exploratorios 

--> Leccion 2

1. ¿Cómo podría usarse el comando hostnamectl para cambiar sólo el nombre de host estático de la máquina local a firewall?

```
hostnamectl --static set-hostname "firewall"
```

2. ¿Qué detalles, además de los nombres de host, pueden ser modificados por el comando hostnamectl?

```
hostnamectl también puede establecer el icono por defecto de la máquina local, su tipo de chasis, la ubicación y el entorno de despliegue.
```

3. ¿Qué entrada en /etc/hosts asocia los nombres firewall y router con la IP 10.8.0.1?

```
10.8.0.1 firewall router

```

4. ¿Cómo se podría modificar el archivo /etc/resolv.conf para enviar todas las peticiones DNS a 1.1.1.1?

```
nameserver 1.1.1.1 

```




-> Leccion 2

### Ejercicios Guiados

1. ¿Qué significa la palabra Portal en la columna CONNECTIVITY en la salida del comando nmcli general status?

```
Que necesita un paso mas de autenticacion, usualmente autenticacion a travez del explorador
```


2. En un terminal de consola, ¿cómo puede un usuario normal utilizar el comando nmcli para conectarse a la red inalámbrica MyWifi protegida por la contraseña MyPassword?

Se puede usar el comando wpa_phrase para generar el hash de autenticacion, el proceso es el siguiente:

Supongamos que el BSSID es INFINITUM123 y la passwrd es 12345 y nuestra interfaz sera wlan0

```
wpa_prhase "INFINUTU123" "12345" > /etc/wpa_supplicant/wpa_supplicant-wlan0.conf

```
y podemos activarlos de la siguiente manera:

```
systemctl start wpa_supplicanr@wlan0.service

o

wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant-wlan0.conf

```

3. ¿Qué comando puede encender el adaptador inalámbrico si el sistema operativo lo ha desactivado previamente?

```

ip link set wlan0 up

o 

nmcli radio wifi on


```

4. ¿En qué directorio deben colocarse los archivos de configuración personalizados cuando systemd-networkd gestiona las interfaces de red?

Hay 3 lugares, en orden de prioridad so:

- /etc/systemd/network/
- /run/systemd/network
- /lib/systemd/network

### Ejercicios Exploratorios 


1. ¿Cómo puede un usuario ejecutar el comando nmcli para eliminar una conexión no utilizada llamada Hotel Internet?

```

nmcli connect delete "Hotel Internet"
```

3. NetworkManager escanea las redes wi-fi periódicamente y el comando nmcli device wifi list sólo lista los puntos de acceso encontrados en el último escaneo. ¿Cómo debería usarse el comando nmcli para pedir a NetworkManager que vuelva a escanear inmediatamente todos los puntos de acceso disponibles?

```
ncli dev wifi rescan 

Despues las podemos ver con:

nmcli dev wifi list

o 

nmcli device wifi list 
```

4. ¿Qué entrada name debe utilizarse en la sección [Match] de un archivo de configuración systemd-networkd para que coincida con todas las interfaces ethernet?

```
[Match]
Name=en*  // Asi comienzan el nombre de la interfaz ethernet

[Network]
...

```

5. ¿Cómo debe ejecutarse el comando `wpa_passphrase` para utilizar la frase de paso dada como argumento y no desde la entrada estándar?

```
wpa_passphrase MyWifi MyPassword
```











