# Streaming Media Server Installer

The sms-installer is a collective of ansible tasks that will download and install the software necessary to run the following applications.

**Plex Media Server** 
**Sabnzbd** 
**Sickgear** 
**Radarr** 
**Lidarr** 

## Getting Started

These instructions will get you a copy of the project up and running on your machine for development and testing purposes.  This playbook was
designed with the intention of having an ansible controller execute the playbook, and configure the applications on a remote server.  The ansible controller can
be any device capable of running ansible.  This project assumes you have a functioning ansible controller.

### Prerequisites

#### On the Remote Server
1. Install Ubuntu Server 20.04, configure an IP address, a user account (ex: foghornleghorn), and install the openssh-server package, during installation.
2. Give the user (ex: foghornleghorn) password-less sudo privledges on the Ubuntu 20.04 server.

Edit visudo
```
sudo visudo
```

add the below to the bottom of the file, save, and exit.
```
foghornleghorn     ALL=(ALL) NOPASSWD:ALL
```


#### On the Ansible Controller
1. Establish ssh keybased authentication between the ansible controller and the remote server

Type the following, replacing server_ip with the ip address of the remote server

```
ssh-copy-id foghornleghorn@server_ip
```

#### Test
If you did it correctly, you should be able to ssh from the ansible controller into the remote server using key-based authentication, and be able to update your packages without having to type a password. 

From the ansible controller

```
ssh foghornleghorn@server_ip "sudo apt update && sudo apt upgrade -y"
```


### Installation

1. Extract or copy the sms-installer folder to a location on the ansible controller.  
2. Edit the sms-installer/sms_inventory file and change the server "server_ip" to your actual IP or resolvable hostname of your remote server.


### Execution

From within the sms-installer folder, issue the following command.


```
ansible-playbook -i sms_inventory sms-install.yml
```

Upon execution, the playbook will ask for user input, and install the applications.


### Access
Access your applications at the following URL's, replacing server_ip with your remote servers IP address.  It's up to you to customize the services to your liking, enjoy!

Plex
http://server_ip:32400/web/index.html

Sabnzbd
http://server_ip:8080/sabnzbd/wizard/

Sickgear
http://server_ip:8081/

Radarr
http://server_ip:7878/

Lidarr
http://server_ip:8686/


### Notes and Caveats
If you're running these applications on a server that is not local to you, meaning, you're not on the same IP subnet, you might run into issues accessing or configuring your plex application.  In one instance, the server was browsable, but not able to add itself as a server (error on the server said to install plex media server).  To resolve this, read the section "On a different network" at this URL.

https://support.plex.tv/articles/200288586-installation/

Essentially you need to setup an SSH tunnel to trick the server into thinking you're local to it.  Browse to the section on allowing remote networks, and add your remote networks to this allow list. 

You're other option is to use xQuartz to establish X11 forwarding.  Install firefox on the GUI-less server "sudo apt install firefox", and forward the firefox app on the server to your laptop or pc. Browsing through xQuartz to localhost:32400/web/index.html will bring up the plex server on your laptop or pc, as if you were on the server itself.   


Sabnzbd has been templated to prevent such issues, and the configuration wizard should be accessible from remote networks.

If you want a more recent version of the software being used, replace the URL in these variables with the new full length URL download location, in the sms-install.yml file.  (hint, it'll probably be similar with the exception of the new version number, and should still end in the file extension.)

 ``` 
  vars:
    plex_dl_url: 'https://downloads.plex.tv/plex-media-server-new/1.21.1.3830-6c22540d5/debian/plexmediaserver_1.21.1.3830-6c22540d5_amd64.deb'
    radarr_dl_url: 'https://github.com/Radarr/Radarr/releases/download/v0.2.0.1504/Radarr.develop.0.2.0.1504.linux.tar.gz'
    lidarr_dl_url: 'https://github.com/lidarr/Lidarr/releases/download/v0.6.2.883/Lidarr.develop.0.6.2.883.linux.tar.gz'
```


### Built With

* [Ansible](https://www.ansible.com/)

### Authors

* **James Kayser** - *Initial work* - [kayserj](https://github.com/kayserj)

### License

@ 2020 James Kayser All Rights Reserved


