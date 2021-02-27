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














