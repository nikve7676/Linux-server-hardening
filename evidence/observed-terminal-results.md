# Observed Terminal Evidence — 14 November 2026 Screenshot Review

This file records **only what is visible** in four screenshots submitted for the Linux Server Hardening project. The screenshots were cleaned by cropping the terminal windows and provided separately to the operator as a ZIP. The original image files have **not** been uploaded to this repository.

## 1. Effective SSH Server Settings

Screenshot: `01-ssh-effective-settings.png` (prepared for upload).

Command:

```bash
sudo sshd -T | grep -E '^(permitrootlogin|passwordauthentication|kbdinteractiveauthentication|pubkeyauthentication) '
```

Observed output:

```text
permitrootlogin no
pubkeyauthentication yes
passwordauthentication no
kbdinteractiveauthentication no
```

**Finding:** The effective SSH daemon configuration disables direct root login, password login and keyboard-interactive login, while enabling public-key authentication. This confirms settings, **not** a successful Mac-to-VM key-only connection.

## 2. UFW Firewall

Screenshot: `03-ufw-fail2ban-sudo.png` (prepared for upload).

Observed:

```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
22/tcp       ALLOW IN    Anywhere
22/tcp (v6)  ALLOW IN    Anywhere (v6)
```

**Finding:** UFW is active and defaults to denying incoming connections, with SSH permitted from any IPv4/IPv6 address. In this UTM lab, this was an intentional access rule. For an internet-accessible host, a narrower source range or private-network access should be considered.

## 3. Fail2ban SSH Jail

Screenshot: `03-ufw-fail2ban-sudo.png`.

Observed:

```text
Status for the jail: sshd
Currently failed: 0
Total failed: 0
Journal matches: _SYSTEMD_UNIT=ssh.service + _COMM=sshd
Currently banned: 0
Total banned: 0
```

**Finding:** The `sshd` jail responds and is matching the systemd SSH journal. Zero bans and failed attempts are expected if no matching failures occurred; this does not measure efficacy against an attack. Earlier recorded terminal output verified `bantime` became `3600` seconds.

## 4. Sudo Membership

Screenshot: `03-ufw-fail2ban-sudo.png`.

Command and observed output:

```text
getent group sudo
sudo:x:27:nikoo
```

**Finding:** The `sudo` group shows `nikoo` as a member. This is evidence of group membership, not a complete review of all possible administrator accounts or sudoers rules.

## 5. Listening Sockets and Processes

Screenshot: `04-listening-ports.png` (prepared for upload).

The visible portion of `sudo ss -tulpen` shows:

| Local address / port | Service or process | Interpretation |
| --- | --- | --- |
| `0.0.0.0:22`, `[::]:22` | `sshd` / `systemd` | SSH listens on IPv4 and IPv6 |
| `127.0.0.54:53`, `127.0.0.53%lo:53` | `systemd-resolved` | Local DNS stub listeners |
| `127.0.0.1:631`, `[::1]:631` | `cupsd` | Printing service bound to loopback |
| `*:45386` UDP | `python3` (GNOME gvfs daemon context) | UDP socket displayed; investigate if unnecessary |

**Finding:** SSH is the clearly observed network-facing TCP listener. DNS and CUPS listeners shown are bound to loopback. The UDP socket deserves review before asserting that no unnecessary network-facing services exist. The screenshot shows the final part of the command output, so other sockets may exist above the visible region.

## 6. Clock and APT Follow-up

The Ubuntu VM desktop clock in the screenshots displays **Nov 12**, while the macOS host menu bar displays **Sat 14 Nov**. These dates are visibly inconsistent.

An earlier `apt update` reported `Release file ... is not valid yet`. A misaligned VM clock is a plausible cause, but the screenshot does **not** show `timedatectl`, a successful subsequent APT refresh or a completed upgrade.

**Priority:** Investigate clock synchronization on the VM and save output from:

```bash
timedatectl
date -u
sudo apt update
apt list --upgradable
```

Do not disable APT repository timestamp checks.

## Evidence Handling

- These observations are grounded in the user's uploaded screenshots.
- Cropping removed unrelated desktop content without altering the terminal results.
- The actual PNG files were provided as a ZIP for local download, but are **not hosted in this GitHub repository**.
- Do not commit credentials, private keys or sensitive information.
