Week 03 – Ubuntu Server Deployment
BSIT Self-Paced Learning Module: System Administration and Maintenance

Prepared by: Syron B. Blaza, ABC Startup Solutions

Project Overview
ABC Startup Solutions is deploying its first Linux server to support file sharing, remote administration, database hosting, web hosting, and internal services. This repository documents the full deployment process: installing Ubuntu Server on Oracle VirtualBox following recommended enterprise practices, verifying the resulting server, comparing BIOS and UEFI firmware, mapping the Linux boot process, installing Windows Server Evaluation as a comparison platform, and evaluating Windows Server, Ubuntu Server, and Rocky Linux against each other.

Learning Objectives
Deploy a clean Ubuntu Server virtual machine using recommended enterprise installation practices.
Verify server functionality after installation (login, hostname, IP address, connectivity, updates, SSH).
Compare BIOS and UEFI firmware and explain why UEFI has become the modern standard.
Document the Linux boot process from power-on to login prompt.
Install Windows Server Evaluation as a bring-home comparison exercise.
Compare Windows Server, Ubuntu Server, and Rocky Linux for enterprise use cases.
Produce professional documentation reproducible by another administrator.
Virtual Machine Specifications
Component	Configured Value
Name	Ubuntu-Server-Week03
RAM	1536–2048 MB (reduced from the 4 GB spec to match host hardware — see Challenges Encountered)
CPU	2 Virtual Processors
Storage	40 GB (VDI, dynamically allocated)
Network	NAT (Intel PRO/1000 MT Desktop Adapter)
Optical Drive	ubuntu-24.04.4-live-server-amd64.iso
Installation Summary

Ubuntu Server 24.04.4 LTS was installed via the standard text-based installer:

Selected Ubuntu Server (full, not minimized) as the installation base.
Accepted DHCP networking — interface enp0s3 was assigned 10.0.2.15/24.
Used guided storage configuration with the entire disk set up as an LVM group, formatted as ext4 (/ = 18.996 GB, /boot = 2.000 GB).
Set hostname to server01 and created a non-root administrative user.
Enabled Install OpenSSH server for remote administration.
Skipped optional server snaps (no additional packages, per company policy).
Completed installation, detached the ISO, and rebooted into the installed system.

Full step-by-step details and screenshots are in InstallationGuide.pdf.

Configuration Summary

After first boot, the server was updated and its SSH service was explicitly enabled and started:

bash
sudo apt update
sudo apt upgrade -y
sudo systemctl enable ssh
sudo systemctl start ssh
Verification Results
Task	Command	Result
Login	—	Successful login as configured admin user
Hostname	hostname	server01
IP Address	ip addr	10.0.2.15/24 on enp0s3
Internet Connectivity	ping -c 4 google.com	4/4 packets received, 0% packet loss
System Update	sudo apt update && sudo apt upgrade -y	Completed successfully, system up to date
SSH Service	systemctl status ssh	Active: active (running)
BIOS vs UEFI Highlights
	BIOS	UEFI
Mode	16-bit real mode	32/64-bit mode
Partition style	MBR (max 2 TB, 4 primary partitions)	GPT (>2 TB, up to 128 partitions)
Security	None built-in	Secure Boot
Boot speed	Slower, sequential init	Faster, supports parallel init

Why UEFI has largely replaced BIOS: UEFI resolves BIOS's core limitations — slow 16-bit initialization, the 2 TB/4-partition MBR ceiling, and the total absence of boot-time security. Secure Boot alone (verifying only trusted, signed bootloaders load) is a major reason virtually all PCs and servers built in the last decade ship with UEFI as the default. Full discussion in BIOS_vs_UEFI.pdf.

Boot Process Flowchart

Show Image

Power On → BIOS/UEFI Initialization → Boot Device Detection → Boot Loader (GRUB) → Linux Kernel → init/systemd → Services Start → Login Prompt

Full-size version: BootProcessFlowchart.pdf

Challenges Encountered
Host RAM constraints: VirtualBox refused to save VM settings at 4096 MB RAM (“more than 80% of the host computer's memory is assigned”) on a host with only 3.5 GB total RAM. Resolved by lowering Base Memory to 1536–2048 MB.
CPU soft lockup during installation: The Ubuntu VM froze mid-install with watchdog: BUG: soft lockup — CPU#0 stuck for 58s, caused by host CPU exhaustion from other running applications. Resolved by closing unnecessary host programs and letting the install resume.
Stale VM registration after a crash: VirtualBox lost track of the VM (Failed to open virtual machine … same UUID as an existing virtual machine) after an unexpected shutdown. Resolved by editing VirtualBox.xml to remove the conflicting <MachineEntry> and re-adding the VM — no data was lost, since the .vdi/.vbox files remained intact on disk.
SSH installed but inactive: systemctl status ssh showed inactive (dead) immediately after first boot despite OpenSSH being selected during setup. Resolved with sudo systemctl enable ssh && sudo systemctl start ssh.
Slow Windows Server installation: The bring-home Windows Server Evaluation install took several hours on the same constrained hardware; resolved simply by allowing it to run uninterrupted rather than cancelling it.
Reflection

This project was my first hands-on experience deploying a Linux server from scratch, and most of what I actually learned came from things going wrong rather than the installation steps themselves going smoothly. The VM specs in the assignment assumed more RAM than my laptop actually has, so early on I had to learn how to read VirtualBox's warnings, understand why they were happening, and make a reasonable engineering trade-off — dropping RAM allocation instead of blindly following a spec sheet that didn't fit my hardware. That felt like a small but real taste of what system administration is actually like: adapting standard procedures to the constraints of the environment in front of you.

The scariest moment was when the VM disappeared from VirtualBox Manager entirely after a crash. My first assumption was that I'd lost hours of work. Instead, I learned that the actual virtual disk and configuration files were untouched on disk — only VirtualBox's internal registry had gotten out of sync. Tracking that down meant opening an XML file I'd never seen before and carefully removing one specific line without breaking anything else. That was a good lesson in staying calm and checking the actual state of the filesystem before assuming the worst.

I also didn't expect that a service could show as "installed" during setup but still be inactive after boot. Running systemctl status ssh and seeing inactive (dead) when I expected active (running) taught me not to trust an installer's checkbox as proof that something is actually working — verification has to happen against the live system, not against what you told the installer to do.

Comparing BIOS vs UEFI and researching Windows Server, Ubuntu, and Rocky Linux side by side also connected the hands-on work to the bigger picture: why an enterprise would choose one platform over another isn't just about technical specs, it's about licensing cost, existing infrastructure, and who's going to be maintaining it. Overall, this project took much longer than I expected, mostly due to my hardware limitations, but troubleshooting through those limitations taught me more than a clean, uneventful install would have.

References
Ubuntu Server Documentation — https://ubuntu.com/server/docs
Oracle VirtualBox User Manual — https://www.virtualbox.org/wiki/Documentation
Microsoft Windows Server 2025 Evaluation Center — https://www.microsoft.com/evalcenter/download-windows-server-2025
Rocky Linux Documentation — https://docs.rockylinux.org
