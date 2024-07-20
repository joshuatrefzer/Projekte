# V-Server Setup

 This guide provides a step-by-step process to set up a V-Server. It begins with generating SSH key pairs on your local machine and progresses through logging into the server, copying your public key to the server, and disabling password login for  security reasons. Finally, it shows the configuration of your server and the addition of a webserver, including setting up an alternative HTML page served on a different port. 


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
Following command to log in:

```sh 
ssh *username*@*123.123.12.12* 
```

type in your password.

## 3. Copy public key to Server 

```sh
ssh-copy-id -i ~/.ssh/*nameofyourkey*.pub 
```

In general:

```sh
ssh-copy-id -i *pathtoyourssh*/*nameofyourkey*.pub
```

### Now you can login with ssh key 

```sh
ssh -i *~/.ssh/id_rsa* *username*@*123.123.12.12*
```
> [!Warning]
> Don't skip this part. Before you cannot log in with your ssh key, you should not disable the passwort login (point 4 of this documentation) on your server.

## 4. Disable password login 
(On your server)

Edit your ssh config: 

```sh
sudo nano /etc/ssh/sshd_config
```

Turn 
```sh
    #PasswordAuthentication yes
``` 
... to
```sh
    PasswordAuthentication no
``` 

...then save changes and exit 

### Restart ssh service

Run this command:
```sh
    sudo systemctl restart ssh.service 
``` 

Test if it works -> open new console und run this command:

```sh
    ssh -o PubkeyAuthentication=no *username*@*123.123.12.12*
``` 

Resonse should be:
```sh
 Permission denied (publickey).
 ```


## 5. Configure your server & add webserver

Log in on your v-server an run this commands: 

```sh
   sudo apt update
   sudo apt install nginx -y
``` 

### create alternative html page 

Run this command:

```sh
sudo mkdir /var/www/alternatives
sudo touch /var/www/alternatives/alternate-index.html
```
... then type in your html code, write out and exit the editor.

### change nginx configuration

Run this command, to create new file:

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

...then write out, exit and  restart nginx server: 

````sh
sudo service nginx restart
````

Check in your browser this url *youripadress*:8081 
Now you should see you alternate HTML Page.















