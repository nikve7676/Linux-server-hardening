# Security Updates and Final Audit

## 1. Objective

Keep Ubuntu packages current and verify that SSH, firewall and Fail2ban settings are active. The aim is to document real checks, not just planned commands.

## 2. Environment

- Ubuntu 26.04.1 LTS (ARM64/aarch64)
- UTM virtual machine on an Apple M2 Mac
- SSH lab username: `nikoo`

## 3. Package Update Check

Commands reported executed:

```bash
sudo apt update
apt list --upgradable
```

- `apt update` refreshes package information; it does **not** install upgrades.
- `apt list --upgradable` lists packages with available updates.

**Observed result:** An earlier `sudo apt update` encountered `Release file ... is not valid yet` for the `resolute-security`, `resolute-updates`, and `resolute-backports` repositories. This means the update check did not successfully refresh all repositories. VM clock or repository timestamp alignment needs attention. The user later reported completing the suggested time checks, but their outputs and a successful new `apt update` have not been shared. Do not bypass APT date checks.

## 4. Installing Available Upgrades

If upgrades are listed, review them and run:

```bash
sudo apt upgrade
```

Then check again:

```bash
apt list --upgradable
```

**Status:** Upgrade installation not yet confirmed.

## 5. Reboot Check

```bash
if [ -f /var/run/reboot-required ]; then echo "Reboot required"; else echo "No reboot required"; fi
```

If a reboot is required, save work, keep UTM console access available, and reboot at a suitable point.

**Status:** Not yet verified.

## 6. Final Security Audit

Run the following checks on Ubuntu and record the actual results:

```bash
sudo sshd -t
sudo sshd -T | grep -E '^(permitrootlogin|passwordauthentication|kbdinteractiveauthentication|pubkeyauthentication) '
sudo ufw status verbose
sudo fail2ban-client status sshd
apt list --upgradable
```

From macOS, also verify public-key-only SSH access (keep a working session and UTM console available):

```bash
ssh -o PreferredAuthentications=publickey -o PasswordAuthentication=no -i ~/.ssh/id_ed25519 nikoo@192.168.64.3
```

The VM address may change.

## 7. Verification Checklist

- [ ] Confirm `sudo apt update` now succeeds without future-dated release-file errors
- [ ] Record installed package upgrade results
- [ ] Verify whether a reboot is needed and complete it if necessary
- [ ] Confirm SSH config syntax passes
- [ ] Confirm effective SSH settings disable password, keyboard-interactive and root SSH login
- [ ] Confirm key-based SSH login from macOS still works
- [ ] Confirm UFW is active and SSH port permitted
- [ ] Confirm Fail2ban `sshd` jail is active with intended values
- [ ] Review user, sudo and sensitive file permissions

## 8. Conclusion

The security update check was performed, but the observed repository timestamp error means update success is not yet confirmed. The final hardening audit is **in progress**, not yet verified complete. Results will be updated when terminal outputs are available.
