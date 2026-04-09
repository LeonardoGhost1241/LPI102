# Arquitectura de sistema X Window 

- Se divide en un clinente y un servidor (El cliente toma forma de una aplicacion, como un emulador de temrinal, un juego o un navegador web, cada aplicacion cliente informa al servidor X sobre la ubicacion y el tamaño de su ventana en la pantalla de una computadora y el servidoir X coloca el dibujo solicitado en la pantalla)
- El sistema X windows tambien maneja la entrada de dispositivos como mouse, teclados, trackpads y mas
- Un administrador de pantalla proporciona un acceso grafico a un sistema, este sistema puede ser un ordenador ocal o un ordenador a traves de una red 


Cada instancia de un servidor X en funcionamiento tiene un nombre de pantalla para identificarlo, el nombre seria algo asi:
hostname:displaynumber.screennumber

* hostname: se refiere al nombre del sistema que mostrara la aplicacion. Si falta un nombre de host en el nombre de pantalla, entonces se asume que es el host local
* displaynumber: hace referencia a la coleccion de "pantallas" que estan en uso, ya sea una sola o varias pantallas en una estacion de trabajo. A cada sesion de servidor X en ejecucion se le da un numero de pantalla que comienza en 0
* screennumber: por defecto es 0, Esto puede ser el caso si solo una pantalla o varias pantallas fisicas estan configuradas para trabajar como una sola pantalla. Cada pantalla independiente tendra su propio numero asignado. Si solo  hay una logica en uso, entonces se omite el punto y el numero de pantalla

El nombre de usa sesion X en curso se almacena en la variable de entorno:

```
$ echo $DISPLAY
:0
```
1. El servidor X en uo esta en el sistema local, por lo tanto no hay nada impreso a la izquierda delos dos puntos
2. La sesion actual del servidor X es la primera como indica el 0 que sigue a los dos puntos 
3. Solo hay una pantalla logica en uso, por lo que un numero de pantalla no es visible 











# Instalar y configurar X11

X11 es un protocolo de comunicacion, el entonrno se comunica de forma cliente-servidor 


Usuario (user)
^
|
v
Interfaz grafica (Ejemplos:KDE Plasma, Aqua, Gnome shell) (se puede ejecuta de forma remota)
^
| (x11, wayland, quartz)
v
Servidor de pantalla (display server) <--> gestor de ventanas (Ejemplos: awesome, compiz, openBox) 
    (Ejemplos: X.org server, weston, kwin, mutter, quarz compositor)
^
|
v
Nucle (kernel)
^
|
v
equipamento (hardware)


El gestor de pantalla (display manager) para mostrarla pantalla de autenticacion de usuarios e iniciar el escritorio grafico una vez que se autentico el usuario (gdm, lightdm,..)

El gestor de ventanas muestra las "decoravciones" al rededor de las ventanas que pertenecen a las aplicaciones que estan ejecutando actualmente, lo que incluye marcos y la barra de titulo



## Instalacion de servidor X 
```
Deabian:

apt-get install xserver-xorg gnome-session 

Redhat:
yum groupinstall general-desktop 

```

**Las versiones actuales de X.org pueden detectar automaticamente el hardware disponible y no se crea el archivo de configuracion /etc/x11/xorg.conf**

Para generar uno usamos:

```
x -configure 
```
cuandoñ no estemos usando el entorno grafico, nos generara un fichero en el directorio actual que luego tendremos que poner en la ruta correcta

**Tambien se pueden establecer ficheros de configuracion modulares y añadirlos al directorio /etc/x11/xorg.conf.d/**
los cuales tendran secciones como:

- Archivos: nombres de ruta de archivos
- serverFlags : banderas del servidor
- Modulo: carga dinamica del modulo
- Extensiones: Habilitacion de extensiones
- inputDevice: Descripcion del dispositivo de entrada
- InputClass: Descripcion de la clase de entrada
- OutputClass: descripcion de salida
- Dispositivo: descricion del dispositivo grafico
- VideoAdaptador: Descripcion del adaptador de video Xv
- Monitor: descripcion del monitor
- Modos: Descripciones de modos de video
- Pantalla: Configuracion de pantalla
- ServerLayout: Diseño general
- DRI: configuracion especifica de DRI


