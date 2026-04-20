## Tema 105. Shells y scripts  

<details>
<summary>Personalizar y usar el entorno de shell</summary>

**Leccion 1**

Tipos de shells 


|Tipo|Ejemplo real|
|----|------------|
|Interactivo + Login | Conectarse por SSH o Ctrl+Alt+F1-7|
|Interactivo + No-login | Abrir una terminal en el escritorio | 
|No interactivo + No-login | scripts en cronjobs |
|No interactivo + Login | Muy raro, casi sin uso practivo |

Para saber en que shell esta usamos:

```
echo $0
```

* `-bash` o `-su` -> Interactivo con login
* `bash` o `/bin/bash` -> Interactuvo sin login
* `nomre_del_script` -> no interactivo sin login 

```
bash -l  o  bash --login      # Inicia shell de login
bash -i                        # Inicia shell interactivo
bash --noprofile               # Ignora /etc/profile y ~/.bash_profile
bash --norc                    # Ignora /etc/bash.bashrc y ~/.bashrc
bash --rcfile <archivo>        # Usa un archivo rc personalizado
```

```
su - user2                     # Shell interactivo CON login como user2
su user2                       # Shell interactivo SIN login como user2
sudo -i                        # Shell interactivo CON login como root
sudo -s                        # Shell SIN login como root
```

```
ps aux | grep bash             # Ver cuántos shells bash están corriendo
```

```
. ~/.bashrc                    # Ejecuta/recarga un archivo sin reiniciar
source ~/.bashrc               # Igual que el punto, son sinónimos

```

---
Archivos de inicio y sus rutas

Shell interactivo CON login  (Se leen en este orden, y si encuentra uno local los demas se omiten)


|Archivo|Nivel|Description|
|-------|-----|-----------|
|/etc/profile| Global| Establece PATH, PS1, ejecuta /etc/bash.bashrc|
|/etc/profile.d/* | Global | Scripts adicionales llamados por /etc/profile|
|~/.bash_profile | Usuario | Configura entorno del usuario. Si existe, se ignoran los dos siguientes |
|~/.bash_login |Usuario | Solo se ejecuta si NO existe ~/.bash_profile |
|~/.profile | Usuario | Solo se ejecuta si NO existen los dos anteriores. También llama a ~/.bashrc | 
| ~/.bash_logout | Usuario | Se ejecuta al cerrar session, para limpieza | 

Shell interactivo SIN login 

|Archivo|Nivel|Description|
|-------|-----|-----------|
| /etc/bash.bashrc | Global | Verifica modo interactivo, actualiza tamaños de ventana | 
| ~/.bashrc | Usuario | Alias, funciones, variables del usuario. Tambien llamado por ~/.profile | 

Shell no interactivo SIN login (scripts)

No lee ningun archivo anterior, solo busca la variable: 
```
$BASH_ENV       # Si esta definida busca ejecutar el archivo  que apunte 
```

Rutas importantes 

|Ruta|Para que sirve|
|----|--------------|
| /etc/profile | Configuracion global | 
| /etc/profile.d/ | Scripts globales adicionales | 
| /etc/bash.bashrc | Configuracion global de shells interactivos | 
| ~/.bash_profile | Config de login a nivel usuario (específico bash)|
|~/.bash_login | Alternativa a .bash_profile |
|~/.profile | Config de login a nivel usuario (genérico, no solo bash) | 
| ~/.bashrc | Config de shells sin login a nivel usuario |
|~/.bash_logout | Limpieza al cerrar sesión | 
| /etc/skel/ | Plantilla que se copia al crear nuevos usuarios | 
| /etc/adduser.conf | Configuración de adduser, donde se define la variable SKEL | 


Nota: Los archivos locales (~/) siempre tienen prioridad sobre los globales (/etc/)








**Leccion 2**

**Leccion 3**



</details>


<details>
<summary>Personalizar y scribir scripts sencillos </summary>

**Leccion 1**
**Leccion 2**

</details>



## Tema 106. Interfaces de usuario y escritorios 

<details>
<summary> Instalar y configurar X11</summary>

**Leccion 1**

</details>

<details>
<summary>Escritorios Graficos </summary>

**Leccion 1**

</details>

<details>
<summary>Accesibilidad</summary>

**Leccion 1**
</details>



## Tema 107. Tareas administrativas

<details>
<summary>Administrar cuentas de usuario y de grupo y los archivos de sistema relacionados con ellas</summary>

**Leccion 1**
**Leccion 2**


</details>

<details>
<summary>Automatizar tareas administrativas del sistema mediante la programación de trabajos</summary>

**Leccion 1**
**Leccion 2**

</details>

<details>
<summary>Localización e internacionalización</summary>

**Leccion 1**

</details>


## Tema 108. Servicios esenciales del sistema  

<details>
<summary>Mantener la hora del sistema</summary>

**Leccion 1**
**Leccion 2**
</details>



<details>
<summary>Registros del sistema</summary>

**Leccion 1**
**Leccion 2**
</details>




## Tema 109. Shells y scripts 
