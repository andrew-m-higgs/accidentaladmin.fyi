+++
title = "A Beginner's Guide to Running a Samba Server on Linux"
date = 2026-08-11T22:15:00+02:00
draft = false
tags = ["linux", "samba", "tutorial", "networking"]
categories = ["Guides"]
+++


Sharing files between different operating systems can sometimes feel like a hassle. Fortunately, **Samba** makes it easy to share files and printers across a network, allowing Windows, macOS, and Linux machines to communicate seamlessly.

This guide will walk you through setting up a basic, password-protected Samba file share on a Linux system (specifically using Ubuntu/Debian-based commands).

## Step 1: Install Samba

First, you need to update your package list and install the Samba software. Open your terminal and run:

```bash
sudo apt update
sudo apt install samba -y
```

To verify that Samba installed correctly, you can check its status:

```bash
sudo systemctl status smbd
```

## Step 2: Create a Shared Directory

Next, let's create the folder you want to share over the network. We'll create a folder called `sambashare` in your home directory.

```bash
mkdir ~/sambashare
```

## Step 3: Configure the Samba Share

Samba's configuration is handled by a single file located at `/etc/samba/smb.conf`. 

Before making changes, it's always a good idea to back up the original configuration file:

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

Now, open the configuration file in a text editor (like `nano`):

```bash
sudo nano /etc/samba/smb.conf
```

Scroll all the way to the bottom of the file and add the following lines:

```ini
[sambashare]
    comment = Samba on Ubuntu
    path = /home/username/sambashare
    read only = no
    browsable = yes
```
*Note: Replace `username` with your actual Linux username.*

Save and exit the file (in nano, press `CTRL+O`, `Enter`, then `CTRL+X`).

## Step 4: Set Up a Samba User Password

Samba uses its own password management system. You need to add your current Linux user to Samba and set a password.

```bash
sudo smbpasswd -a username
```
*Note: Again, replace `username` with your actual Linux username. You will be prompted to create and confirm a password.*

## Step 5: Restart the Samba Service

For your new configuration to take effect, you need to restart the Samba service:

```bash
sudo systemctl restart smbd
```

*(Optional) If you have a firewall enabled (like UFW), you'll need to allow Samba traffic:*
```bash
sudo ufw allow samba
```

## Step 6: Connect to Your Share

Your Samba server is now up and running! Here is how to access it from different devices:

### From Windows:
1. Open **File Explorer**.
2. In the address bar at the top, type `\\IP_ADDRESS\sambashare` (Replace `IP_ADDRESS` with your Linux machine's local IP address, which you can find by typing `ip a` in the terminal).
3. When prompted, enter the username and the Samba password you created in Step 4.

### From macOS:
1. Open **Finder**.
2. Go to the menu bar and click **Go** -> **Connect to Server** (or press `Cmd + K`).
3. Type `smb://IP_ADDRESS/sambashare` and click Connect.
4. Enter your Samba credentials.

### From another Linux machine:
Open your file manager and look for a "Network" or "Other Locations" tab, then type:
`smb://IP_ADDRESS/sambashare`

---
**Congratulations!** You've just set up your own cross-platform file server using Samba.
