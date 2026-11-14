# Fail2ban SSH Protection

## 1. Objective

Configure Fail2ban to monitor failed SSH authentication attempts and temporarily ban IP addresses that repeatedly fail authentication. This is an additional layer alongside SSH key authentication and UFW firewall rules.

## 2. Environment

- Ubuntu 26.04.1 LTS (ARM64) in UTM on a Mac with Apple M2
- SSH service: OpenSSH
- Protection target: `sshd` jail
- Lab VM IP previously observed: `192.168.64.3` (may change)

## 3. Installation

Commands from the lab procedure:

```bash
sudo apt update
sudo apt install fail2ban
sudo systemctl status fail2ban
```

Purpose: Refresh package metadata, install Fail2ban, and check its service status.

**Status:** Installation and configuration steps reported completed. Exact installation output was not saved in this documentation.

## 4. Configuration

Custom configuration file:

`/etc/fail2ban/jail.local`

Configuration used in the lab instructions:

```ini
[sshd]
enabled = true
port = ssh
backend = systemd
maxretry = 5
findtime = 10m
bantime = 1h
```

Meaning of the settings:

| Setting | Purpose |
| --- | --- |
| `enabled = true` | Enables the SSH jail |
| `port = ssh` | Uses the SSH service port (normally TCP 22) |
| `backend = systemd` | Reads SSH authentication events from the systemd journal |
| `maxretry = 5` | Ban threshold of five matching failures |
| `findtime = 10m` | Ten-minute detection window |
| `bantime = 1h` | One-hour ban duration |

The settings above reflect the configuration procedure followed. The running configuration still needs to be checked against the actual Ubuntu VM.

## 5. Apply and Check the Configuration

Commands from the lab:

```bash
sudo fail2ban-client -t
sudo systemctl restart fail2ban
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

To verify the runtime values:

```bash
sudo fail2ban-client get sshd maxretry
sudo fail2ban-client get sshd findtime
sudo fail2ban-client get sshd bantime
```

Expected values:
- maxretry: `5`
- findtime: `600` seconds
- bantime: `3600` seconds

## 6. Reported Results and Verification

I reported completing the Fail2ban configuration steps.

The actual terminal outputs for the configuration test, service status, SSH jail status and active values have **not yet been captured here**. Therefore, the effectiveness of the jail is not yet marked as verified.

- [ ] Confirm `sudo fail2ban-client -t` passes
- [ ] Confirm Fail2ban service is active
- [ ] Confirm `sshd` appears in `sudo fail2ban-client status`
- [ ] Confirm `sudo fail2ban-client status sshd` succeeds
- [ ] Confirm runtime `maxretry`, `findtime`, `bantime` values
- [ ] Record that SSH access from macOS still works

**Safety:** Do not deliberately make repeated failed login attempts from the Mac, because the Mac's IP could be temporarily banned.

## 7. Security Benefit

Fail2ban can slow repeated authentication attacks by enforcing temporary IP bans based on detected failures. It complements, but does not replace, SSH key authentication, restrictive SSH settings or the firewall.

## 8. Next Steps

Verify the live Fail2ban results, then continue with security updates and the final server audit.
