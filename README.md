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
- [ ] Verify Mac public key is installed on Ubuntu and test key-only authentication.
- [ ] Verify SSH password and direct root login are disabled, and confirm remote access still works.
- [ ] Audit Linux users, sudo permissions and sensitive-file permissions.
- [ ] Verify UFW rules and successful SSH access after enabling the firewall (user reports UFW stage performed).
- [ ] Install, configure and verify Fail2ban.
- [ ] Apply and verify security updates and complete final audit.

## Documentation

- [Initial assessment](documentation/01-initial-assessment.md)
- [File permissions practice](documentation/02-linux-permissions.md)
- [SSH hardening](documentation/03-ssh-hardening.md)
- [UFW firewall](documentation/04-ufw-firewall.md)

## Verification policy

A task is marked complete only when its result has been confirmed. Some commands are documented as planned or user-reported while verification output is still pending. Never commit private SSH keys, passwords or other secrets.
