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

Custom configuration files observed on the VM:

- `/etc/fail2ban/jail.local` (existing)
- `/etc/fail2ban/jail.d/sshd.local` (edited in the lab)
- `/etc/fail2ban/jail.d/defaults-debian.conf` (distribution defaults; not modified)

The active SSH jail may be affected by both custom files, so effective values are checked with `fail2ban-client get`.

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

The first observed `sshd.local` content contained `bantime = 1h0`; I later reported correcting it to `1h`. The post-restart active `bantime` value was confirmed as 3600 seconds. Other active values still need captured output.

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

Actual terminal results from the lab:

```text
sudo fail2ban-client get sshd bantime
600
sudo fail2ban-client -t
OK: configuration test is successful
sudo systemctl restart fail2ban
sudo fail2ban-client get sshd bantime
3600
```

This demonstrates a successful syntax test and an applied ban-time change from 10 minutes to one hour. A successful restart command alone does not prove all protections are operating as intended.

I later reported completing the other suggested verification commands, but their outputs have not been shared.

- [x] Confirm `sudo fail2ban-client -t` passes
- [ ] Confirm Fail2ban service is active
- [ ] Confirm `sshd` appears in `sudo fail2ban-client status`
- [ ] Confirm `sudo fail2ban-client status sshd` succeeds
- [x] Confirm runtime `bantime` is 3600 seconds
- [ ] Record runtime `maxretry` and `findtime` outputs
- [ ] Record that SSH access from macOS still works

**Safety:** Do not deliberately make repeated failed login attempts from the Mac, because the Mac's IP could be temporarily banned.

## 7. Security Benefit

Fail2ban can slow repeated authentication attacks by enforcing temporary IP bans based on detected failures. It complements, but does not replace, SSH key authentication, restrictive SSH settings or the firewall.

## 8. Next Steps

Record remaining runtime values and SSH jail status, then finish the security updates and final server audit.
