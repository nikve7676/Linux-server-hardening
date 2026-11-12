# Initial Server Security Assessment

## 1. Environment

This project uses an Ubuntu virtual machine running in UTM on an Apple M2 Mac.

The purpose of this stage is to identify the initial server configuration before applying security improvements.

## 2. Operating System Identification

Command:

`cat /etc/os-release`

Purpose: Identify the Ubuntu version and operating system information.

Result: Pending verification.

## 3. User Identification

Command:

`whoami`

Purpose: Identify the currently logged-in Linux user.

Command:

`id`

Purpose: Display the user's UID, GID and group memberships.

Result: Pending verification.

## 4. Network Configuration

Command:

`ip -br addr`

Purpose: Display the network interfaces and their IP addresses in a brief format.

Result: Pending verification.

## 5. SSH Server Assessment

Command:

`systemctl status ssh`

Purpose: Check whether the SSH service is installed and running.

Result:

- SSH Server: Installed
- SSH Service: Inactive (dead)
- Service startup: Disabled
- SSH socket: Configured (status not yet verified)

Explanation:

The SSH server is installed, but the SSH service is currently inactive.

The system shows that SSH can be activated through ssh.socket. However, we still need to check whether the socket is active.

Security observation:

SSH security settings have not been verified or hardened yet.

Next step:

Check the SSH socket status using:

`systemctl status ssh.socket`

## 6. SSH Server Installation

If OpenSSH Server is missing, it can be installed using:

`sudo apt update`

`sudo apt install openssh-server`

Installation status: Not yet confirmed.

## 7. Next Steps

- Verify the initial system configuration.
- Verify SSH installation and service status.
- Configure SSH key authentication.
- Disable direct root SSH login.
- Test secure remote access.
