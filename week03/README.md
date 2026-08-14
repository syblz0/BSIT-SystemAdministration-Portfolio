Week 03 – Ubuntu Server Deployment
BSIT Self-Paced Learning Module: System Administration and Maintenance

Prepared by: Syron B. Blaza

Project Overview

For this project, I was asked to set up the first Linux server for ABC Startup Solutions. This server will be used for file sharing, remote administration, database hosting, web hosting, and some internal services. I installed Ubuntu Server on VirtualBox, checked that everything was working after install, compared BIOS and UEFI, made a boot process flowchart, and also installed Windows Server as a bring-home activity to compare it with Ubuntu and Rocky Linux.

Learning Objectives
-Install Ubuntu Server the correct way, following good practices.
-Check that the server is working properly after installation (login, hostname, IP, internet, updates, SSH).
-Learn the difference between BIOS and UEFI and why UEFI is used more now.
-Understand the steps a Linux server goes through when it boots up.
-Install Windows Server Evaluation as an extra activity.
-Compare Windows Server, Ubuntu Server, and Rocky Linux.
-Make documentation that another person could follow and understand.

Virtual Machine Specifications

Component		Value I Used
Name			Ubuntu-Server-Week03
RAM				1536–2048 MB (I lowered this from 4 GB because my laptop only has 3.5 GB total RAM)
CPU				2 Virtual Processors
Storage			40 GB (VDI, dynamically allocated)
Network			NAT
Optical Drive	ubuntu-2


Installation Summary

Here's what I did to install Ubuntu Server:
1. Picked Ubuntu Server (the normal one, not the minimized version).
2. Used DHCP for the network — it gave my server the IP 10.0.2.15/24 on enp0s3.
3. For storage, I used guided setup with the whole disk, set up as LVM, formatted as ext4.
4. Set the hostname to server01 and made a normal (non-root) admin user.
5. Turned on OpenSSH server so I could connect to it remotely later.
6. Didn't add any extra packages, since the instructions said not to.
7. Let it finish installing, removed the ISO, then rebooted.

More details and screenshots are in InstallationGuide.pdf.

Configuration Summary

After the first login, I updated the system and made sure SSH was actually running:
sudo apt update
sudo apt upgrade -y
sudo systemctl enable ssh
sudo systemctl start ssh


Verification Results

Task			Command Used			What Happened
Login			—						Logged in fine with the account I made
Hostname		hostname				Showed server01
IP Address		ip addr					Showed 10.0.2.15/24 on enp0s3
Internet		ping -c 4 google.com	4/4 packets received, 0% lost
Update			sudo apt update &&      Finished with no errors
				sudo apt upgrade -y	
SSH				systemctl status ssh	Showed active (running)


BIOS vs UEFI Highlights

					BIOS							UEFI
Mode				16-bit							32/64-bit
Partition type		MBR (max 2 TB, 4 partitions)	GPT (bigger than 2 TB, up to 128 partitions)
Security			Nothing built in				Has Secure Boot
Boot speed			Slower							Faster

Why UEFI is used more now: BIOS is old technology — it's slow and can only handle disks up to 2 TB because of how MBR works. UEFI fixes that with GPT, which supports way bigger disks and more partitions. UEFI also has Secure Boot, which stops untrusted software from loading when the computer starts, something BIOS can't do at all. Because of this, almost every computer and server made in the last several years uses UEFI instead of BIOS. Full comparison is in BIOS_vs_UEFI.pdf.


Boot Process Flowchart

<img width="850" height="1100" alt="BootProcessFlowchart drawio" src="https://github.com/user-attachments/assets/e59c2de7-56e0-4f61-ace4-20d3066f1c71" />

Power On → BIOS/UEFI Initialization → Boot Device Detection → Boot Loader (GRUB) → Linux Kernel → init/systemd → Services Start → Login Prompt
Full version: BootProcessFlowchart.pdf


Challenges Encountered

- My laptop only has 3.5 GB of RAM, so VirtualBox wouldn't let me use 4 GB for the VM like the assignment said. I had to lower it to        1536–2048 MB instead.
- The VM froze while installing Ubuntu, and the screen showed something about the CPU being "stuck." This happened because I had too many   other programs open at the same time.
- After VirtualBox crashed once, my VM disappeared from the list completely. I had to go into a file called VirtualBox.xml and delete one   line to fix it. My actual files were okay the whole time, VirtualBox just lost track of them.
- SSH was supposed to be installed already, but when I checked it, it said "inactive." I had to turn it on manually.
- Installing Windows Server took a really long time (a few hours) because of the same low RAM/CPU problem.

Reflection

This was my first time actually installing a Linux server on my own, and honestly most of what I learned came from things breaking, not from the parts that went smoothly. The assignment wanted 4 GB of RAM for the VM but my laptop doesn't even have that much total, so I had to figure out how to lower it without breaking anything else. That was annoying at first but it also taught me that you can't just follow instructions exactly if your hardware is different — you have to adjust and explain why.

The scariest part was when my VM disappeared from VirtualBox after it crashed. I thought I lost everything I did that day. But after looking into it, I found out the actual files were still saved on my computer, VirtualBox just "forgot" about them because of some ID mismatch in a settings file. I had to edit that file directly, which I had never done before, and it worked. That taught me not to panic right away and to actually check what's going on before assuming the worst.

I also didn't know that a service could be "installed" but still not running. When I checked SSH with systemctl status ssh, it said inactive even though I picked the option to install it during setup. I had to turn it on myself with a command. This showed me that checking a box during install doesn't always mean something is actually working, you have to verify it yourself after.

Comparing BIOS and UEFI, and also comparing Windows Server, Ubuntu, and Rocky Linux, helped me understand that choosing an OS isn't only about which one is "better" technically. It's also about cost, what the company already uses, and who is going to manage it. Overall this project took me a lot longer than I thought it would, mostly because of my laptop's limits, but I think I learned more from fixing all these problems than I would have if everything just worked on the first try.


References
Ubuntu Server Documentation — https://ubuntu.com/server/docs
Oracle VirtualBox User Manual — https://www.virtualbox.org/wiki/Documentation
Microsoft Windows Server 2025 Evaluation Center — https://www.microsoft.com/evalcenter/download-windows-server-2025
Rocky Linux Documentation — https://docs.rockylinux.org
