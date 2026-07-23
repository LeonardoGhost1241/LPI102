# Leccion 1 



### Ejercicios guiados

1. Indique si los siguientes comandos estan mostrando o modificando la hora del sistema o la hora del hardware 

| Comando(s) | Sistema | Hardware | Ambos |
|------------|---------|----------|-------|
| Date -u |x |  | | 
| hwclock --set --date "12:00:00" | | x | | 
| timedatectl |  |  | x |
| timedatectl \| grep RTC |  | x |  | | 
| hwclock --hctosys | x | | | 
|date +%T -s "08:00:00" | x | | | 
| timedatectl set-time 1980-01-10 |   | | x|  

2. Observe la siguiente salida, y luego corrija el formato del argumento para que el comando sea exitoso:


```
$ date --debug --date "20/20/12 0:10 -3"

date: warning: value 20 has less than 4 digits. Assuming MM/DD/YY[YY]
date: parsed date part: (Y-M-D) 0002-20-20
date: parsed time part: 00:10:00 UTC-03
date: input timezone: parsed date/time string (-03)
date: using specified time as starting value: '00:10:00'
date: error: invalid date/time value:
date:     user provided time: '(Y-M-D) 0002-20-20 00:10:00 TZ=-03'
date:        normalized time: '(Y-M-D) 0003-08-20 00:10:00 TZ=-03'
date:                                  ---- --
date:      possible reasons:
date:        numeric values overflow;
date:        incorrect timezone
date: invalid date ‘20/20/2 0:10 -3’
```

Nos dice el problema en la linea de "Possuble reasons", el numero que debe de cambiarse es el mes, ya que esta fuera del buffer, debe de ser:

```
date --debug --set "12/20/20 0:10 -3"
```

3. Use el comando date y las secuencias para que el mes del sistema sea febrero. Deje el resto de la fecha y la hora sin cambios.

```
date +%m -s 2 

```

4. Asumiendo que el comando anterior tuvo éxito, use hwclock para ajustar el reloj del hardware desde el reloj del sistema

```
hwclock --systohc
```

5. Hay un lugar llamado eucla. ¿De qué continente forma parte? Use el comando grep para averiguarlo

```
find  /usr/share/zoneinfo/ -iname "eucla"
find  /usr/share/zoneinfo/ | grep -i " *eucla *"

/usr/share/zoneinfo/Australia/Eucla
/usr/share/zoneinfo/posix/Australia/Eucla
/usr/share/zoneinfo/right/Australia/Eucla

```

6. Establezca su zona horaria actual en la de eucla

```
timedatectl set-timezone "Australia/Eucla"
```

### Ejercicios Exploratorio

1. ¿Qué método de ajuste de tiempo es el óptimo? ¿En qué escenario podría ser imposible el método preferido?
En la mayoría de las distribuciones de Linux, el NTP está habilitado por defecto y debería dejarse que establezca la hora del sistema sin interferencias. Sin embargo, si hay un sistema Linux que no está conectado a Internet, NTP será inaccesible. Por ejemplo, un sistema Linux integrado que funcione en un equipo industrial podría no tener conectividad a la red 


2. ¿Por qué cree que hay tantos métodos para lograr lo mismo, es decir, establecer la fecha y hora del sistema?
Dado que el establecimiento del tiempo ha sido un requisito de todos los sistemas *nix durante décadas, hay muchos métodos heredados para establecer el tiempo que aún se mantienen

3. Después del 19 de enero de 2038, Linux System Time requerirá un número de 64 bits para almacenar. Sin embargo, es posible que podamos elegir simplemente establecer un “nuevo epoch”. Por ejemplo, el 1 de enero de 2038 a medianoche podría establecerse una nueva época de 0. ¿Por qué cree que esto no se ha convertido en la solución preferida?

Para el 2038 la gran mayoría de las computadoras ya estarán funcionando con CPU de 64 bits, y el uso de un número de 64 bits no degradará el rendimiento de manera significativa. Sin embargo, sería imposible estimar los riesgos de “resetear” epoch de tal manera. Hay mucho software antiguo que podría ser afectado. Los bancos y las grandes empresas, por ejemplo, a menudo tienen una gran cantidad de programas antiguos de los que dependen para su uso interno. Así que este escenario, como muchos otros, es un estudio de las compensaciones. Cualquier sistema de 32 bits que siga funcionando en 2038 se vería afectado por un desbordamiento de "Epoch Time", pero el software heredado se vería afectado por el cambio de valor de Epoch.








# Leccion 2 


### Ejercicios guiados

1. Introduzca el termino apropiado para cada definicion: 

| Definicion | Termino |
|------------|---------|
|Un ordenador que compartirá la hora de la red con usted | Provider |
| Distancia de un reloj de referencia, en saltos o pasos. | Stratum  |
| Diferencia entre la hora del sistema y la hora de la red |  Offset|	
| Diferencia entre la hora del sistema y la hora de red desde el último sondeo NTP  | Jitter|  
| Grupo de servidores que proporcionan la hora de la red y que comparten la carga entre ellos | Pool|  

2. Especifique cual de los comandos utilizaria para dar salida a los siguientes valores:

| Valor |	chronyc tracking  |	timedatectl show-timesync --all |  ntpq -pn | chrony ntpdata | chronyc sources |
|-------|---------------------|---------------------------------|-----------|----------------|-----------------|
|jitter |                   |   | x | x |   | 
| drift|                    |   |   |   |   | 
| interval of pool |     x  | x | x | x | x |
| Offset |               x  |   | x | x |   |
| Stratum  |             x  | x | x | x | x |
| IP Adress of provider |   | x | x | x | x |
| Root Delay |           x  |   |   | x |   |  


   
3. Está configurando una red empresarial que consta de un servidor Linux y varios ordenadores de sobremesa Linux. El servidor tiene una dirección IP estática de 192.168.0.101. Usted decide que el servidor se conectará a pool.ntp.org y luego proporcionará la hora NTP a los ordenadores de sobremesa. Describa la configuración del servidor y de los ordenadores de sobremesa.


Tendremos que modifuicar el archivo ntp.conf de cada maquina y agregar hasta el ultimo, la siguiente linea

```
server (server.url.localhost -> Apunta al servidor dentro de la red)  
```

Asegúrese de que el servidor tiene un servicio ntpd en ejecución, en lugar de SNTP. Utilice los pools de pool.ntp.org en el archivo /etc/ntp.conf o /etc/chrony.conf. Para cada cliente, especifique 192.168.0.101 en cada archivo /etc/ntp.conf o /etc/chrony.conf




4. Una máquina Linux tiene la hora incorrecta. Describa los pasos que daría para solucionar el problema de NTP.

- Comprobaria los pool que hay en el archivo /etc/ntp.conf
- Si estan correcto reiniciaria o iniciaria el servicio ntpd 
- Si esta corriendo, comprobaria con ntpq -p o con chrony tracking 


En primer lugar, asegúrese de que la máquina está conectada a Internet. Utilice ping para ello. Compruebe que el servicio ntpd o SNTP se está ejecutando utilizando systemctl status ntpd o systemctl status systemd-timesyncd. Es posible que aparezcan mensajes de error que proporcionen información útil. Por último, utilice un comando como ntpq -p o chrony tracking para verificar si se ha realizado alguna solicitud. Si la hora del sistema es drásticamente diferente de la hora de la red, puede ser que la hora del sistema se considere “insane” y no se cambie sin intervención manual. En este caso, utilice un comando de la lección anterior o un comando como ntpdate pool.ntp.org para realizar una única sincronización ntp.




### Ejercicios Exploratorio

1. Investigue las diferencias entre SNTP y NTP.


| Característica     | ntpd        | sntp           |
| ------------------ | ----------- | -------------- |
| Tipo               | Daemon      | Cliente simple |
| Sincronización     | Continua    | Una sola vez   |
| Precisión          | Alta        | Básica         |
| Algoritmos         | Complejos   | No             |
| Uso de red         | Más tráfico | Muy ligero     |
| Puede ser servidor | SI          | NO              |


Network protocol con ntpd 
- Implementación completa del protocolo NTP
- Corre como daemon (ntpd)
- Sincronización continua
- Ajuste fino del reloj (slewing)
- Algoritmos complejos: selección de servidores, filtrado, corrección de jitter
- Puede actuar como: cliente o servidor
- Uso: Servidores, sistemas criticos y redes donde importa la precicion 

Simple Netowokr Time Protocol
- Versión simplificada de NTP
- Cliente ligero
- Consulta puntual (no continua)
- No usa algoritmos complejos
- Ajuste básico del reloj (a veces stepping)
- No mantiene estado
- Uso: Dispositivos simples, Scripts, sistemas embebidos 

2. ¿Por qué un administrador de sistemas podría elegir no utilizar pool.ntp.org?

Desde ntppool.org: Si es absolutamente crucial tener la hora correcta, debería considerar una alternativa. Del mismo modo, si su proveedor de Internet tiene un servidor de tiempo, se recomienda utilizarlo en su lugar





3. ¿Cómo podría un administrador de sistemas decidir unirse o contribuir de alguna manera al proyecto pool.ntp.org?

Si, para unir un nuevo servidor, las instrucciones son estas: https://www.ntppool.org/en/join.html

Desde www.ntppool.org: Su servidor debe tener una dirección IP estática y una conexión permanente a Internet. La dirección IP estática no debe cambiar en absoluto o al menos menos una vez al año. Además, los requisitos de ancho de banda son modestos: 384 - 512 Kbit de ancho de banda. Los servidores de estrato 3 o 4 son bienvenidos











