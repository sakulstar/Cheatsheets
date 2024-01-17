# Jellyfin Tizen App for Samsung TV
##### How to read
1. All comments in code blocks are to be ignored, for example:
```bash
ipconfig # Hello this is a comment
```
Write:
```bash
ipconfig
```
2. All [word-inside] in code blocks are to be replaced with something, for example:
```bash
docker run --rm georift/install-jellyfin-tizen [samsung tv ip]
```
Write:
```bash
docker run --rm georift/install-jellyfin-tizen 123.456.7.910
```

### Prerequisites
1. Make sure you're installing everything on a amd64 system (Like Kali Linux). You can check on Linux via the *lscpu* command.
2. Make sure these repositories and docker image are up: [install-jellyfin-tizen](https://github.com/Georift/install-jellyfin-tizen) [jellyfin-tizen-builds](https://github.com/jeppevinkel/jellyfin-tizen-builds) [georift/install-jellyfin-tizen](https://hub.docker.com/r/georift/install-jellyfin-tizen)
3. Make sure your tv has access to your home network so it can connect to your computer.

---
### Installation
To clarify, I am using Kali Linux on a Virtualbox VM for this installation (These commands should work with any Debian based OS).
1. Install docker. (You can check after if you have installed it by typing docker)
```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker --now
```
2. Get your private/home network IP address (Do this outside of your VM if you're using one)
```bash
ipconfig # Windows
ifconfig # Linux / MacOS
```
3. Turn on developer mode on your Samsung TV (if you have a Tizen TV 2016 [try this](https://stackoverflow.com/a/42337316)) and type in your computers IP address below while turning developer mode on.
4. Get your Samsung TVs IP address. This can be done via a app or [the settings](https://smarthomeowl.com/ip-address-tv/).
5. Run the command below in your VM.
```bash
docker run --rm georift/install-jellyfin-tizen [samsung tv ip]
```

The final command should output something like this:
```bash
┌──(kali㉿kali)-[~]
└─$ sudo docker run --rm georift/install-jellyfin-tizen 192.168.1.184
[sudo] password for kali: 
Unable to find image 'georift/install-jellyfin-tizen:latest' locally
latest: Pulling from georift/install-jellyfin-tizen
25fa05cd42bd: Pull complete 
e5a5d5cb6ef2: Pull complete 
9712f387ce65: Pull complete 
d0a0401b6393: Pull complete 
f9f06c056c10: Pull complete 
33061d3c0f85: Pull complete 
644704475390: Pull complete 
f70fd06e0efd: Pull complete 
1a470cc199d2: Pull complete 
591a47c121c8: Pull complete 
4b625ed7ee58: Pull complete 
25d5f2a6523d: Pull complete 
d8b3662dc1f1: Pull complete 
896b991ce261: Pull complete 
Digest: sha256:c7d6c4a9994a152526d05514d613d25818ca6ac1b0501f336a7aa4f868ef96f7
Status: Downloaded newer image for georift/install-jellyfin-tizen:latest


        Thanks to https://github.com/jeppevinkel for providing the pre-packaged jellyfin-tizen builds!
        These builds can be found at https://github.com/jeppevinkel/jellyfin-tizen-builds


Attempting to connect to Samsung TV at IP address 192.168.1.184
* Server is not running. Start it now on port 26099 *
* Server has started successfully *
connecting to 192.168.1.184:26101 ...
connected to 192.168.1.184:26101
Attempting to get the TV name...
Found TV name: UE55KS9005
Attempting to install jellyfin-tizen-builds version:
https://github.com/jeppevinkel/jellyfin-tizen-builds/releases/tag/2023-06-07

     0K .......... .......... .......... .......... ..........  0% 5.01M 2s
    50K .......... .......... .......... .......... ..........  1% 9.55M 1s
...
  9200K .......... .......... .......... .......... .......... 98% 15.2M 0s
  9250K .......... .......... .......... .......... .......... 99% 14.1M 0s
  9300K .......... .......... .......... .......... .......... 99% 12.6M 0s
  9350K .......... .........                                  100% 42.8M=0.3sTransferring the package...
Transferred the package: /home/developer/Jellyfin.wgt -> /opt/usr/apps/tmp
Installing the package...
--------------------
Platform log view
--------------------
install AprZAARz4r.Jellyfin
package_path /opt/usr/apps/tmp/Jellyfin.wgt
was_install_app return WAS_TRUE
app_id[AprZAARz4r.Jellyfin] install start
app_id[AprZAARz4r.Jellyfin] install completed
spend time for wascmd is [6452]ms
cmd_ret:0
Installed the package: Id(AprZAARz4r.Jellyfin)
Tizen application is successfully installed.
Total time: 00:00:21.975
```