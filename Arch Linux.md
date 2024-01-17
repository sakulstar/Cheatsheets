# Arch Linux
[Arch Linux](https://archlinux.org/) is a lightweight and flexible Linux distribution that *tries* to Keep It Simple.

##### Sources
[Arch Linux Wiki](https://wiki.archlinux.org/)
[Arch Linux: A Comfy Install Guide by DenshiVideo](https://www.youtube.com/watch?v=68z11VAYMS8&t=1118s)

##### How to read
1. All comments in code blocks are to be ignored, for example:
```bash
setfont -d # Hello this is a comment
```
Write:
```bash
setfont -d
```
2. All [word-inside] in code blocks are to be replaced with something, for example:
```bash
loadkeys [keymap]
```
Write:
```bash
loadkeys dk-latin1
```

##### Good to know
1. **CTRL + L** clears the terminal
2. **CTRL + C** stops a running command
3. **CTRL + X** closes nano
4. **CTTRL + O** saves the edited file in nano

---
# Arch Linux Install
For this install I will assume you will be installing it on bare metal with the UEFI boot mode and 64-bit. (You will be shown how to check this).

### Prerequisites
1. Download the latest Arch Linux ISO from [archlinux.org](https://archlinux.org/download/).
2. Remember to check the files hash to verify that the downloaded file hasn't been tampered with, you can do this by using [hash-file.online](https://hash-file.online/), [toolsley.com](https://www.toolsley.com/hash.html) or any other software/website you know.
3. You can either use [Rufus](https://rufus.ie/en/), [Ventoy](https://www.ventoy.net/en/index.html) or any other software to create a bootable USB with the Arch Linux ISO on it.
4. Make sure you don't have secure boot enabled in the BIOS as that is not supported by the arch linux installation medium.
### Boot the live environment
1. Keyboard layout
```bash
localectl list-keymaps # Lists all keyboard layouts
loadkeys [keymap] # Loads the keyboard layout you want
loadkeys en-latin1 # English layout
loadkeys dk-latin1 # Danish layout
loadkeys de-latin1 # German layout
```
2. Fonts
```bash
ls /usr/share/kbd/consolefonts/ # Displays all fonts you can choose (exclude the .psf, .psfu and .gz)
setfont [font] # Sets your desired font
setfont -d # Doubles the font
setfont # Resets font
```
3. Verify that you're using UEFI boot mode and 64-bit.
	1. If the file doesn't exist then you're not in UEFI boot mode.
	2. If the file outputs "32" then you're in UEFI boot mode and 32-bit.
	3. If the file outputs "64" then you're in UEFI boot mode and 64-bit.
```bash
cat /sys/firmware/efi/fw_platform_size
```
4. Connect to the internet.
	1. If you receive all packets continue.
	2. If you don't receive any packets then plug in an ethernet cable or use [iwd](https://wiki.archlinux.org/title/Iwd#iwctl) to connect to the internet.
```bash
ping [website] # Pings your chosen website
ping archlinux.org # Pings the archlinux.org website
```
5. Update system clock.
```bash
timedatectl # Outputs your timezone.
timedatectl set-timezone [Region]/[Capital] # Sets your chosen timezone
timedatectl set-timezone Europe/Copenhagen # Sets your timezone to CET/CEST
```
6. Partition the disks
	1. Select label type "gpt".
	2. Delete all other current partitions and create the new ones as layout below.
	3. Select "write" to write the partitions and then exit by pressing "quit".
```bash
cfdisk # Start cfdisk.
```
Make this layout with cfdisk:

| Mount point   | Partition                   |    Partition type     | Size                                                                                            |
| ------------- | --------------------------- |:---------------------:|:----------------------------------------------------------------------------------------------- |
| /mnt/boot/efi | /dev/_efi_system_partition_ | EFI system partition  | Minimum: 300M                          If you want multiple kernels installed then minimum: 1G. |
| [SWAP]        | /dev/_swap_partition_       |      Linux swap       | Minimum: 521M           Recommended: 4G, 8G or 16G.                                             |
| /mnt          | /dev/_root_partition_       | Linux X86-64 root (/) | Remainder of the device.                                                                        |
7. Format the disks
```bash
lsblk # Look at all your partitions (Usually /dev/)
mkfs.ext4 /dev/[root_partition] # Formats your root partition
mkfs.fat -F 32 /dev/[efi_system_partition] # Formats your efi system partition
mkswap /dev/[swap_partition] # Formats your swap partition
```
8. Mount the file system
```bash
mount /dev/[root_partition] /mnt # Mounts the root partition to /mnt
mkdir /mnt/boot/efi # Makes efi system partition mount directory
mount /dev/[efi_system_partition] /mnt/boot/efi # Mounts the efi system partition to /mnt/boot/efi
swapon /dev/[swap_partition] # Turns on the swap partition
```
### Installation and generate fstab file
1. Install essential packages and other needed packages
```bash
pacstrap /mnt base linux linux-firmware sof-firmware base-devel grub efibootmgr nano networkmanager # Installs Arch Linux
```
2. Generate a fstab file
```bash
genfstab -U /mnt >> /mnt/etc/fstab # Generate a fstab file into /mnt/etc/fstab
nano /mnt/etc/fstab # Check and edit the fstab file for errors
```
### Configure the new system
1. Move into the new system
```bash
arch-chroot /mnt # Move into the new system
```
2. Set timezone
```bash
ln -sf /usr/share/zoneinfo/[Region]/[Capital] /etc/localtime # Generate etc/localtime
hwclock --systohc # Generate /etc/adjtime
```
3. Keyboard layout and language (Remember to save the files)
```bash
nano /etc/locale.gen # Uncomment the desired language for example: en_US.UTF-8 for American English
nano /etc/locale.conf # Edit the file and write LANG=en_US.UTF-8 for American English
nano /etc/vconsole.conf # Edit the file and write KEYMAP=us for American English keyboarrd layout or KEYMAP=dk-latin1 for Danish keyboard layout
```
4. Network configuration
```bash
nano /etc/hostname # Edit the file and type in your hostname (the desired name of your device)
```
5. Passwords and creating accounts
```bash
passwd # Change the root password
useradd -m -s /bin/bash [name] # Guest account
useradd -m -G wheel -s /bin/bash [name] # Account with sudo permissions
passwd [name] # Set password for account
```
6. Setting up the sudo (so you can run commands that need permissions without root)
```bash
EDITOR=nano visudo # Move down to the bottom of the file and uncomment the "%wheel ALL=(ALL) ALL" line
```
7. Enable core NetworkManager service
```bash
systemctl enable NetworkManager # Enables NetworkManager
```
8. Setup bootloader (GRUB)
```bash
grub-install /dev/sda # Installs GRUB
grub-mkconfig -o /boot/grub/grub.cfg # Configures GRUB
```
9. Unmount and reboot
```bash
exit # exits arch linux and goes back to bootable usb
umount -a # Unmount all drives that aren't busy.
reboot # Reboots device
```
10. Congratulations! You should now be booted into GNU GRUB where you can select "Arch Linux" to boot into Arch Linux where you can login.
4. Connect to the internet.
	1. If you receive all packets continue.
	2. If you don't receive any packets then plug in an ethernet cable or use [NetworkManager](https://wiki.archlinux.org/title/NetworkManager) to connect to the internet.
```bash
ping [website] # Pings your chosen website
ping archlinux.org # Pings the archlinux.org website
```

---
# Install KDE Plasma
1. Install KDE Plasma packages (Press enter on all the questions)
```bash
sudo pacman -S plasma sddm
```
2. Install additional packages (You can replace these if you want to)
	1. [Konsole](https://konsole.kde.org/) is a terminal emulator by [KDE](https://kde.org/).
	2. [Kate](https://apps.kde.org/kate/) is a text editor made by [KDE](https://kde.org/).
	3. [Firefox](https://www.mozilla.org/en-US/firefox/new/) is a browser made by [Mozilla](https://www.mozilla.org/en-US/).
```bash
sudo pacman -S konsole kate firefox # Installs konsole, kate and firefox.
```
3. Start the Display Manager
```bash
sudo systemctl enable sddm # Starts Display Manager at boot.
sudo systemctl enable --now sddm # Starts Display Manager now.
```
4. After you have booted into KDE plasma install neofetch if you feel the need.
```bash
sudo pacman -S neofetch # Install neofetch
neofetch # Run neofetch
```
5. Congratulations! You have now installed [KDE Plasma](https://kde.org/plasma-desktop/).