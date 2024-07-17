# V-Server Setup

## Table of Contents

1. [Generating SSH Key Pairs on Local Machine](#1-generating-ssh-key-pairs-on-local-machine)
2. [Log in on Server](#2-log-in-on-server)
3. [Copy public key to server](#3-copy-public-key-to-server)
4. [Disable password login](#4-disable-password-login)
5. [Configure your server & add webserver](#5-configure-your-server--add-webserver)


## 1. Generating SSH Key Pairs on Local Machine

Run the following command to create SSH key pairs:

```sh
ssh-keygen -t ed25519
```


## 2. Log in on server
Following command to log in 

```sh 
ssh *username*@*123.123.12.12* 
```

type in your password.

## 3. Copy public key to Server 

```sh
ssh-copy-id -i ~/.ssh/*nameofyourey*.pub 
```

in general:

```sh
ssh-copy-id -i *pathtoyourssh*/*nameofyourey*.pub
```

### Now you can login with ssh key 

```sh
ssh -i *~/.ssh/id_rsa* *username*@*123.123.12.12*
```

## 4. Disable password login 
(On your server)

Edit your ssh config 

```sh
sudo nano /etc/ssh/sshd_config
```

Turn 
```sh
    #PasswordAuthentication yes
``` 
to
```sh
    PasswordAuthentication no
``` 

save changes and exit 

### Restart ssh service

Run this command
```sh
    sudo systemctl restart ssh.service 
``` 

Test if it works -> open new console und run this command:

```sh
    ssh -o PubkeyAuthentication=no *username*@*123.123.12.12*
``` 

Resonse should be "Permission denied"


## 5. Configure your server & add webserver

log in on your v-server an run this commands: 

```sh
   sudo apt update
   sudo apt install nginx -y
``` 

### create alternative html page 

run this command:

```sh
sudo mkdir /var/www/alternatives
sudo touch /var/www/alternatives/alternate-index.html
```
type in your html code, then write out and exit 

### change nginx configuration

run this command, to create new file:

```sh
sudo nano /etc/nginx/sites-enabled/alternatives
```

write this configuration for PORT 8081:

```
server {
    listen 8081;
    listen [::]:8081;

    root /var/www/alternatives;
    index alternate-index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

write out and exit, then restart nginx server 

````sh
sudo service nginx restart
````

check in your browser this url *youripadress*:8081 
Now you should see you alternate HTML Page.















