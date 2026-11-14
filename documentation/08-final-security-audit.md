# Final Security Audit and Evidence Register

## Scope

Ubuntu 26.04.1 LTS ARM64 running in UTM on an Apple M2 Mac. The assessment covers SSH, UFW, Fail2ban, local permissions, and package updates.

## Status labels

- **Observed:** output was recorded and can be checked against the transcript.
- **User-confirmed:** the VM operator reported the check successful, but output is not yet stored here.
- **Evidence pending:** more output or a baseline is needed for a recruiter to independently review the claim.

## Security control matrix

| Check | Command or method | Status | Evidence gap |
| --- | --- | --- | --- |
| SSH syntax | `sudo sshd -t` | User-confirmed | Save actual check result |
| SSH restrictions | `sudo sshd -T` with authentication-setting filter | **Observed in screenshot:** root no, password no, keyboard-interactive no, pubkey yes | [Observed results](../evidence/observed-terminal-results.md) |
| SSH key-only access | Run `ssh` from macOS with public-key-only options | User-confirmed | Save sanitized success output |
| UFW policy and SSH allow rule | `sudo ufw status verbose` | **Observed in screenshot:** active, deny incoming, allow outgoing, 22/tcp allowed | [Observed results](../evidence/observed-terminal-results.md) |
| Fail2ban configuration syntax | `sudo fail2ban-client -t` | Observed: successful | Already documented |
| Fail2ban ban duration | `sudo fail2ban-client get sshd bantime` | Observed: `3600` | Already documented |
| Fail2ban SSH jail / other values | `sudo fail2ban-client status sshd` and runtime queries | **Jail status observed:** active status response; zero failures/bans. Other values user-confirmed | [Observed results](../evidence/observed-terminal-results.md); maxretry/findtime output still needed |
| Lab file permissions | `ls -l ~/permission-lab/test.txt` | Observed: `-rw-------` | Already documented |
| Administrative users | `getent group sudo` and `sudo -l` | **Observed:** `sudo:x:27:nikoo` | `sudo -l` and full privilege audit not archived |
| Listening ports | `sudo ss -tulpen` | **Partially observed:** SSH on :22, local DNS :53, CUPS loopback :631, Python UDP :45386 | Review full output and investigate UDP socket if unexpected |
| Package metadata refresh | `sudo apt update` | User-confirmed after earlier timestamp error | Successful complete output not archived |
| Installed upgrades | `sudo apt upgrade`; `apt list --upgradable` | Evidence pending | Verify upgrades, remaining packages |
| Reboot check | `test -f /var/run/reboot-required && echo required || echo not-required` | Evidence pending | Add result |

## Recommended evidence filenames

Put sanitized screenshots or terminal transcripts into an `evidence/` directory:

1. `01-ssh-effective-settings.png`
2. `02-ssh-key-login.png`
3. `03-ufw-status.png`
4. `04-fail2ban-sshd.png`
5. `05-updates.png`
6. `06-sudo-and-listening-ports.png`

Four terminal screenshots have now been submitted and reviewed. Cropped versions were provided separately; the **PNG files themselves have not been committed to GitHub**. The screenshot-derived observations are recorded in [the evidence log](../evidence/observed-terminal-results.md). The filenames above are suggested final organization, not claims that those images exist in the repository.

## Safe evidence collection

- **Never** publish `~/.ssh/id_ed25519`, passwords, passphrases, access tokens, or sensitive logs.
- Avoid posting the full `authorized_keys` file, even though it contains public keys; it may reveal comments and identities.
- Review screenshots for local account names, public addresses, connection histories, and other environment details before publishing.
- If you redact output, make a note that it was redacted; do not change the result itself.

## Findings and improvements

### Confirmed observation: Fail2ban ban duration

The SSH jail's ban duration changed from `600` seconds to `3600` seconds after a valid configuration test and restart. This shows that the lab configuration was applied.

### Additional screenshot observation: VM clock discrepancy

In the submitted screenshots, the Ubuntu VM displayed November 12 while the macOS host displayed November 14, 2026. Check the VM time synchronization before treating APT security updates as complete.

### Troubleshooting: APT repository timestamp

An earlier `sudo apt update` returned `Release file ... is not valid yet`. This is consistent with VM clock and repository metadata date disagreement. Subsequent checks were reported working, but the successful final output has not been archived. Package date checks should not be disabled.

### Remaining work

The main lab controls have been reported working. To make the portfolio evidence-backed, archive the actual screenshot files, finish the sudo permission audit, review the observed listening ports, and confirm whether upgrades and reboot were required.

## Conclusion

The Linux hardening implementation has been **user-confirmed** as working. This repository demonstrates the configuration process and actual screenshot-derived results for the major controls, but the final audit is **not independently evidenced end-to-end** until the remaining checks and screenshots are added.
