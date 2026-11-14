# Linux Server Hardening

## Project Overview

This project documents my hands-on practice securing an Ubuntu Linux virtual machine using common Linux security tools and best practices. I use UTM on a Mac with an Apple M2 chip.

## Environment

- Host: macOS (Apple M2)
- Virtualization: UTM
- Guest OS: Ubuntu 26.04.1 LTS (ARM64/aarch64)
- Ubuntu user: `nikoo`
- Lab network: UTM private network; Ubuntu VM observed at `192.168.64.3` (address may change)
- SSH: Remote password login from macOS was successfully tested

## Project Objectives

- Understand Linux accounts, file permissions and network settings.
- Configure and verify secure SSH remote access.
- Restrict incoming traffic with UFW.
- Explore Fail2ban protection for SSH.
- Apply security updates and verify improvements.
- Document actual commands and results.

## Project Progress

- [x] Set up Ubuntu in UTM.
- [x] Identify Ubuntu version and VM network address; test SSH password connection from macOS.
- [x] Practice basic file permissions (`chmod 600`).
- [x] Generate an Ed25519 SSH key pair on macOS.
- [x] SSH key-only access tested (user-confirmed; output not archived).
- [x] SSH authentication restrictions and remote access checked (user-confirmed; effective settings not archived).
- [ ] Record full user/sudo permission audit (only a permissions practice exercise is documented).
- [x] UFW firewall and continuing SSH access checked (user-confirmed; rule output not archived).
- [x] Configure Fail2ban and confirm configuration test passes and SSH jail ban time is 3600 seconds.
- [x] Fail2ban SSH jail and settings checked (user-confirmed; full output not archived).
- [x] Check package update availability with `apt update` and `apt list --upgradable` (user-reported; output not yet reviewed).
- [x] Final command checks reported working (user-confirmed; previous APT date error no longer reported).
- [ ] Archive successful `apt update` / upgrade results and reboot check; verify any pending upgrades were installed.

## Documentation

- [Initial assessment](documentation/01-initial-assessment.md)
- [File permissions practice](documentation/02-linux-permissions.md)
- [SSH hardening](documentation/03-ssh-hardening.md)
- [UFW firewall](documentation/04-ufw-firewall.md)
- [Fail2ban SSH protection](documentation/05-fail2ban.md)
- [Security updates and final audit](documentation/06-security-updates-and-audit.md)

## Verification policy

Checked tasks marked user-confirmed were reported successful by the VM operator, but raw output is not retained. Tasks needing supporting evidence remain open. Never commit private SSH keys, passwords or other secrets.
