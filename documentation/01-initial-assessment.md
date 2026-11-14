# Initial Server Security Assessment

## 1. Environment

Ubuntu runs in UTM on an Apple M2 Mac. This assessment records observations made before or during hardening.

## 2. Operating System Identification

Command: `cat /etc/os-release`

Observed from the Ubuntu SSH login banner:
- Ubuntu 26.04.1 LTS
- Architecture: aarch64 (ARM64)

The exact output of `cat /etc/os-release` has not been saved here.

## 3. User Identification

Commands:
- `whoami` — show current username
- `id` — show UID, primary GID and group memberships

Observed username from terminal prompt and SSH test: `nikoo`.

The full `id` output is still pending documentation.

## 4. Network Configuration

Command: `ip -br addr`

Observed:
```text
lo       UNKNOWN 127.0.0.1/8 ::1/128
enp0s1   UP      192.168.64.3/24 fd1e:784f:5bb0:41e1:51:42ff:feb0:e06c/64 fe80::51:42ff:feb0:e06c/64
```

The VM's private IPv4 address was `192.168.64.3` when tested. It can change.

## 5. SSH Server Assessment

Command: `systemctl status ssh`

Initial observed output:
```text
Loaded: loaded (...; disabled; preset: enabled)
Active: inactive (dead)
TriggeredBy: ssh.socket
```

Interpretation:
- An SSH service unit was installed.
- The service was not running at that moment.
- Socket activation was configured; a separate socket status result was not recorded.

Subsequent test: Connecting to `nikoo@192.168.64.3` succeeded using password authentication. Remote SSH from macOS was also reported successful, confirming that SSH access was available.

The hardened SSH authentication settings have not yet been independently verified in this documentation.

## 6. SSH Server Installation

OpenSSH Server was already installed; no additional installation was needed for the initial SSH test.

If absent, it could be installed with:
```bash
sudo apt update
sudo apt install openssh-server
```

These are instructions, not a claim that both commands were run.

## 7. Next Steps

- Verify public-key-only SSH access and effective SSH restrictions.
- Confirm UFW status and rules.
- Review sudo and account permissions.
- Configure Fail2ban, apply updates and run a final audit.
