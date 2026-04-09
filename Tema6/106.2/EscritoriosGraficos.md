# Escritorios graficos 


### Sistema X window 

Los sistemas que emplean el sistema X, proporcionan los recursos de bajo nivel relacionados con la representacion de la interfaz grafica y la interaccion del usuario con ella. por ejemplo:

- El manejo de los eventos de entrada, como movimientos del mouse o las pulsaciones de teclas 
- La capacidad de cortar, copiar y pegar el contenido del texto entre aplicaciones separadas.
- La interfaz de programación que otros programas utilizan para dibujar los elementos gráficos.

Todas las formas, colores, matices y cualquier otro efecto visial son generados por la aplicacion que se ejecuta sobre X, X no pretende dibujar elemenos visuales complejos, aunque se encarga de controlar la pantalla grafica



### Ambientes de escritorio 
En un sistema, el componente mas importante para dibujar ventanas, colocacion de las mismas y decoracion, es el el administrador de ventanas (windows manager). Este es el gestor que añade la barra de titulo a la ventana, los botones de control, generalemente asociados con las acciones de minimizar, maximizar y cerrar 

Todos los entornos de escritorio proporcionan un gestor de ventanas que se ajusta al aspecto y a la sensacion de su kit de herramientas de widgets, estos son elementos visuales informaticos o interactivos como botones, campos de entrada de texto que estan distribuidos dentro de la ventana de la aplicacion. Los componentes estandares del escritorio, como el lanzafor de aplicacion, la barra de tareas, etc, y el propio gestor de ventanas depende de takes kits de herramientas de widgets para ensamblar sus interfaces



Las bibliotecas de softwrae como: 
- GTK+
- Qt

Proporcionan widgets que los programadores pueden usar para construir elaboradas interfaces graficas para sus aplicaciones 

Generalmente GTK+ y Qt ofrecen las mismas carcateristicas en cuanto widgets, Los elementos interactivos simples pueden ser indistinguibles, mientras que los widgets compuestos, como la ventana de diálogo que utilizan las aplicaciones para abrir o guardar archivos, sin embargo, pueden tener un aspecto bastante diferente.


Las variaciones de las siguientes aplicaciones son comunmente proporcionadas por todos los principales entornos de escritorio:

- Aplicaciones relacionadas con el sistema: Emulador de terminales, gestor de archivos, gestor de instalacino de paquetes, herramientas de configuracion del sistema 

- Comunicacion e internet: Administrador de contactos, clientes de correo electronico, navegador web

- Aplicaciones de oficina: Calendario, calculadora, editor de texto




s entornos de escritorio pueden incluir muchos otros servicios y aplicaciones como un saludo al inicio de la pantalla de inicio de sesion, gestor de sesiones, comunicacion entre procesos, pulseAudio, cups para impresion, etc.Estas carcateristicas no necesitan el entorno grafico para funcionar, pero el entorno de escritorio proporciona graficos frontend para facilitar la configuracion y el funcionamiento de esos recursos 


### Entornos de escritorio
Muchos So solo admiten un entorno oficla de escritorio, a diferencia de estos, linux soporta diferentes opciones de entornos de escritorio que pueden ser utilizados en conjunto con X, cada entorno de escritorio tiene sus propias carcateristicas, pero algunso conceptos de diseño son comunes entre ellos:

- Un lanzador (launcher) de aplicaciones que lista las aplicaciones integradas y de terceros disponibles en el sistema.
- Reglas que definen las aplicaciones predeterminadas asociadas a los tipos de archivos y protocolos.
- Herramientas de configuración para personalizar la apariencia y el comportamiento del entorno de escritorio


* GNOME
* KDE 
* XFCE


### Acceso no local 


El Sistema X Window adopta un diseño basado en pantallas autónomas, donde el mismo administrador de pantalla puede controlar más de una sesión de escritorio gráfico al mismo tiempo. En esencia, una pantalla es análoga para un terminal de texto: ambos se refieren a una máquina o aplicación de software utilizada como punto de entrada para establecer una sesión de sistema operativo independiente. Aunque la configuración más común implica una sesión gráfica singular que se ejecuta en la máquina local, también son posibles otras configuraciones menos convencionales:

- Cambiar entre sesiones de escritorio gráfico activas en la misma máquina.
- Más de un conjunto de dispositivos de visualización (por ejemplo, pantalla, teclado, mouse) conectados a la misma máquina, cada uno controlando su propia sesión de escritorio gráfico.
- Sesiones remotas de escritorio gráfico, donde la interfaz gráfica se envía a través de la red a una pantalla remota.


Las sesiones de escritorio remoto son soportadas nativamente por X, que emplea el protocolo (XDMCP) X Display Manager Control Protocol  para comunicaciones remotas: 
- Se usa en LAB o redes de baja velocidad dado a su alto uso de ancho de banda 
-  tiene problemas de seguridad, dado a que una pantalla remota se ejecuta con privilegios
- XDMCP requiere que se ejeuten X instancias en ambos extremos de la conexion, lo que si no esta disposnible en todas las maquinas involucradas, sera un problema 


### Virtual Network Computing (VNC)
Es una herramienta de plataforma independiente para ver y controlar entornos de escritorio remotos usando el protocolo Remote Frame Buffer (RFB). A través de este, los eventos producidos por el teclado y el mouse, se transmiten al escritorio remoto, que a su vez devuelven cualquier actualización de la pantalla para ser mostrada localmente. Es posible ejecutar muchos servidores VNC en la misma máquina, pero cada servidor VNC necesita un puerto TCP exclusivo en la interfaz de red que acepte las solicitudes de sesión entrantes. Por convención, el primer servidor de VNC debe usar el puerto TCP 5900, el segundo debe usar el 5901, y así sucesivamente

### Remote Desktop Protocol (RDP)
se utiliza principalmente para acceder de forma remota al escritorio de un sistema operativo Microsoft Windows a través del puerto de red TCP 3389. Aunque utiliza el protocolo RDP de Microsoft, la implementación cliente utilizada en los sistemas Linux son programas de código abierto licenciados bajo GNU General Public License (GPL) y no tienen restricciones legales de uso

### Simple Protocol for Independent Computing Enviroments (Spice)
comprende un conjunto de herramientas destinadas para acceder al entorno de escritorio de los sistemas virtualizados, ya sea en la máquina local o en una ubicación remota. Además, el protocolo Spice ofrece características nativas para integrar los sistemas locales y remotos como la posibilidad de acceder a los dispositivos locales (por ejemplo, los altavoces de sonido y los dispositivos USB conectados) desde la máquina remota y el intercambio de archivos entre los dos sistemas











