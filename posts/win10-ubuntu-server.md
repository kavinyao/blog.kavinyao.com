title: Install Ubuntu Server on Windows 10 PC
date: 2020-08-02

---

I've been having lots of fun with home networking recently. Now that I'm in the rabbit hole I've reached the stage of needing a dedicated home server. For now the server will only run an nginx server as reverse proxy. Before I splurge on a new server, I want to see if I can repurpose any existing hardware for this – and my eyes fell on the [NUC](/nuc), which is supposed to be the living room PC but it's not getting used at all sitting in the living room. Since I *may* actually use it for entertainment in the future and this is an experiment I don't want to fully convert it to a Linux box so I want to have dual boot so I can still switch back to Windows 10 when I need. I chose the good old Ubuntu Linux distro, specifically 20.04 LTS server version. I chose server because of the much smaller footprint compared to desktop version (and CLI is just way cooler than GUI). As it turns out the installation of Ubuntu server isn't as dummy proof as desktop so the journey wasn't as smooth as expected. Here's the steps I ~~took~~ should have taken, just for posterity.

## Preparation

First, you need to allocate some disk space for Ubuntu server, specifically a partition Ubuntu can use to mount as root. I used Windows Disk Manager to shrink one partition by 30GB to create free space and [MiniTool](https://www.minitool.com/partition-manager/) free version to create an ext4 partition. Formatting to ext4 is important as the server installation tool doesn't offer you the ability to create partition on the fly, unlike the desktop version. Specific parameters don't matter as the partition will be reformatted during the installation.

Next, create a bootable USB stick. Just follow [the official guide](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows).

Finally, turn off Secure Boot from BIOS. According to Internet, if Secure Boot is on, dual boot doesn't work.

## Installation

Plug in the USB stick and boot from it. I mostly followed [the official guide](https://ubuntu.com/tutorials/install-ubuntu-server). The only thing to pay attention to is in the "Configure Storage" step, make sure you select the option other than "Use An Entire Disk" as that'll erase Windows and in the next step select the new ext4 partition you just created, format as ext4 (again) and mount it to `/`. Check out this [imgur album](https://imgur.com/a/hYxdxVj) for demo. After that, continue the installation it should complete installation and set grub as the default UEFI boot device.

## Post Installation

It may happen if you boot to Windows 10, Windows Boot Manager will override grub and next boot will load Windows directly. There are multiple ways to fix this but the simplest solution i found is to follow [this guide](https://askubuntu.com/a/655279/1112741):

- open an admin command line in Windows 10
- run `bcdedit /enum firmware`
- note down the grub EFI path
- run `bcdedit /set {bootmgr} path <<insert_path>>` (the path was `\EFI\ubuntu\shimx64.efi` in my case)

After that Windows Boot Manager will boot with grub.
