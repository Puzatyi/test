#!/bin/bash


loadkeys ru
setfont cyr-sun16

timedatectl set-ntp true
timedatectl set-timezone Europe/Kiev

(
echo y
) | mkfs.ext4  /dev/sda6

mount /dev/sda6 /mnt
mkdir /mnt/boot
mkdir /mnt/WindowsC
mkdir /mnt/WindowsD
mkdir /mnt/home
mount /dev/sda1 /mnt/boot
mount /dev/sda3 /mnt/WindowsC
mount /dev/sda4 /mnt/WindowsD
mount /dev/sda7 /mnt/home

pacstrap /mnt base linux linux-firmware nano

genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt 
dd if=/dev/zero of=/swapfile bs=1M count=4096
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo "/swapfile none swap defaults 0 0" >> /etc/fstab

ln -sf /usr/share/zoneinfo/Europe/Kiev /etc/localtime

echo "arch" > /etc/hostname

echo "en_US.UTF-8 UTF-8" > /etc/locale.gen
echo "ru_RU.UTF-8 UTF-8" >> /etc/locale.gen 

locale-gen

echo "LANG=ru_RU.UTF-8" > /etc/locale.conf

echo "KEYMAP=ru" > /etc/vconsole.conf
echo "FONT=cyr-sun16" >> /etc/vconsole.conf

echo "127.0.0.1 localhost" >> /etc/hosts
echo "::1 localhost" >> /etc/hosts
echo "127.0.1.1 arch.localdomain arch" >> /etc/hosts

passwd

pacman -S grub efibootmgr os-prober ntfs-3g networkmanager network-manager-applet dialog mtools dosfstools base-devel linux-headers git networkmanager-openvpn  --noconfirm 

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

grub-mkconfig -o /boot/grub/grub.cfg

systemctl enable NetworkManager


useradd -mG wheel artem

passwd artem

echo '%wheel ALL=(ALL) ALL' >> /etc/sudoers
echo '[multilib]' >> /etc/pacman.conf
echo 'Include = /etc/pacman.d/mirrorlist' >> /etc/pacman.conf
pacman -Syyy

pacman -S xorg lightdm lightdm-gtk-greeter cinnamon cinnamon-translations gnome-terminal gedit eog evince file-roller gnome-calculator numlockx neofetch htop zsh haveged gnome-system-monitor --noconfirm
systemctl enable haveged
systemctl enable lightdm
exit umount -a
reboot
