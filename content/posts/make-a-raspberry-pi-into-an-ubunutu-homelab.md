---
title: Make a Raspberry Pi into an Ubuntu homelab
date: 2020-12-22
summary: "If you want to try out Ubuntu intstead of Raspbian, here's a brief guide on how to get it set up."
draft: false
categories:
- homelab
- python
tags:
- homelab
- raspberry-pi
- ubuntu
---

 {{< figure src="/images/ubuntu-logo.png" alt="Image of a blue ruler" style="border-radius: 4px; width:40%" >}}

If you want to try out Ubuntu intstead of Raspbian, here's a brief guide on how to get it set up.

**Warning**: follow the steps will result in everythin on the raspberry pi being erased, so proceed with caution.


## Step 1: (Optional) Get an image of Ubuntu to load onto the device

Visit [https://releases.ubuntu.com](https://releases.ubuntu.com) to find the latest LTS (Long Term Support) version and download it locally. As of this writing it’s 20.04, Focal Fossa.

## Step 2: Put the image onto a SD card

This can be done manually, but there’s an official [Raspberry Pi Imager](https://www.raspberrypi.org/software/) (Windows/Ubuntu/macOS supported) that makes this easy. Download this, run it, and choose to download an image, or import the one used from Step 1.

## Step 3: Boot the device and reset the password

Insert the SD card and an HDMI cord that is connected to a monitor. Wait for the pi to boot (which should only take a minute or two). It will show a prompt to login (ubuntu is the default username and password for a fresh install), and then initiate a password reset (choose something secure, we’ll be using SSH keys in a second).

## Step 4: Get the the IP address.

When logging in, this should be shown by default. If this isn’t present, use `ip a` to get the IP address. This will be used to remotely authenticate/authorize access to the pi.

## Step 5: Create an SSH key for secure, remote login

From your local machine (and the one you’ll be using to SSH into the server), run `ssh-keygen` , and at the prompt, enter a path to save the file (something like `~/.ssh/raspberry_pi_homelab`) . View the file with the public key. This file is stored in the same place that was provided, but with a file extension of `.pub`. Using my example, it’s saved at `~/.ssh/raspberry_pi_homelab.pub`. Copy the contents of this file into your clipboard.

## Step 6: Configure file permissions

Ubuntu won’t let SSH keys be used if the permissions are too lax, so use `chmod` to ensure that the file permissions are [sufficiently strict](https://superuser.com/questions/215504/permissions-on-private-key-in-ssh-folder#215506).

## Step 7: Configure and test logging in without a password

SSH into the pi using the password that was set in step 3: `ssh ubuntu@<ip-address-here>`.

Using your favorite editor, edit the authorized keys file: `sudo vim ~/.ssh/authorized_keys` and paste the public key copied from step 5.

Log out of the pi using `exit`, and log back in with: `ssh ubuntu@ip-address -i ~/.ssh/raspberry_pi_homelab`.

## Step 8: Add an SSH shorcut by adding to your SSH config:

To avoid typing this out everytime, add this snippet to your SSH config (`~/.ssh/config`):

```bash
Host homelab
   HostName=<ip-address>
   User=<ubuntu-or-customer-username>
   PasswordAuthentication=no
   IdentityFile ~/.ssh/raspberry_pi_homelab
```

With this config in place, you can now SSH with `ssh homelab`. Note that this config disables using password authentication during SSH negotiation from the client side. To prevent anyone from being able to SSH in with a password, add `PasswordAuthentication no` to the raspberry pi's config (`sudo vim /etc/ssh/sshd_config`).

Remember that this current IP has been temporarily leased, and isn’t necessarily a permanent IP. Additional work will be needed to configure this, but I’ll save that for another time.

Congratulations! You now have an accessible homelab. Go forth and enjoy. When doing this for myself, I used this tutorial and googled as necessary.
