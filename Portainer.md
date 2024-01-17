# Portainer
In this tutorial I assume you already have installed [Raspberry Pi OS Lite](https://www.raspberrypi.com/software/) on your Raspberry Pi.

##### Sources
Project Homepage: [Portainer](https://www.portainer.io)
Documentation: [Portainer Docs](http://documentation.portainer.io)

---
# Setup Portainer with Pi-Hosted on your RPI
Before doing anything you should definently update and upgrade your apt.
Do so by typing this into the terminal of your RPI.
```bash
sudo apt update
sudo apt full-upgrade
sudo reboot
```
After check if you have git installed.
```bash
git
```
If it doesn't recognize the command you should install git by typing this.
```bash
sudo apt install git
```
after you need to make a directory for where the download scripts are gonna be installed, I will be calling this directory "Downloads" and I will also move into the directory.
```bash
mkdir Downloads
cd Downloads/
```
after install the download files by cloning [the Github repository](https://github.com/novaspirit/pi-hosted) and moving inside the repository that you're installing.
```bash
git clone https://github.com/novaspirit/pi-hosted
cd pi-hosted/
```
After you can do these steps (if you don't want a custom icon to appear in portainer). You can also skip this step by going below the highlighted text.
>Remove text below from **install_portainer.sh**, **reinstall_portainer.sh** and **update_portainer.sh**
>```
>--logo "https://pi-hosted.com/pi-hosted-logo.png"
>```
>You can do so by using nano like this
>```bash
>sudo nano install_portainer.sh
>```

After you install docker with
```bash
./install_docker.sh
```
then you exit, login and check if your user has the **docker** group
```bash
exit
ssh exampleusername@123.456.7.890
groups
```
*Image below shows you how it should look like with the docker group highlighted*
![[Pasted image 20240117171012.png]]
After you can easily install portainer with
```bash
./install_portainer.sh
```
when portainer is up and running you can access it at 
```bash
[ip]:9000
```
---