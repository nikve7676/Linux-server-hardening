# UFW Firewall Configuration

## 1. Objective

Use UFW (Uncomplicated Firewall) to restrict unwanted incoming connections while preserving SSH access from my Mac to the Ubuntu VM.

## 2. Environment

- Ubuntu 26.04.1 LTS in UTM on an Apple M2 Mac
- VM address observed: `192.168.64.3` (may change)
- SSH uses port 22 unless the server has been configured otherwise

## 3. Commands for Firewall Setup

The following are the commands from the lab procedure. They must be compared with the actual commands run on the VM.

```bash
sudo ufw status verbose
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status verbose
```

- `status verbose`: displays firewall status and rules
- `default deny incoming`: blocks unsolicited incoming connections unless explicitly permitted
- `default allow outgoing`: permits outgoing connections
- `allow 22/tcp`: permits SSH connections on TCP port 22
- `enable`: activates UFW

**Caution:** SSH must be allowed on the actual SSH port **before** enabling UFW, or remote access may be interrupted. Keep access through the UTM console available during testing.

## 4. Implementation Status

I reported completing the UFW stage.

**Verification pending:** The output of `sudo ufw status verbose` and a fresh SSH connection test after enabling the firewall have not yet been added to this repository.

Do not treat the example rules above as confirmed live settings until they are checked.

## 5. Verification Checklist

- [ ] Capture `sudo ufw status verbose` showing `Status: active`
- [ ] Confirm the expected default incoming and outgoing policies
- [ ] Confirm the rule allowing the actual SSH port
- [ ] From macOS, establish a fresh SSH connection to Ubuntu after firewall activation
- [ ] Record any differences between the intended and actual rules

## 6. Security Benefit

A default-deny incoming firewall policy reduces the network services reachable by other devices, while an explicit SSH rule keeps authorized remote administration available.

## 7. Next Step

After verifying the live firewall rules, proceed to Fail2ban setup and validation.
