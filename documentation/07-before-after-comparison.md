# Before and After: Security Controls

This comparison summarizes my Ubuntu hardening lab. **Observed** means an actual terminal result is recorded in the project or chat. **User-confirmed** means I performed the check successfully but have not added the terminal output. **Not recorded** means no baseline evidence exists, so no before-state claim is made.

| Control | Before | After / current state | Evidence |
| --- | --- | --- | --- |
| Remote SSH access | Password login succeeded | SSH key login and authentication restrictions checked (user-confirmed) | [SSH hardening](03-ssh-hardening.md); effective settings screenshot still to archive |
| Direct root SSH login | Original setting not recorded | Disabled (user-confirmed) | `sshd -T` output still to archive |
| File permissions | Created a lab file | `test.txt` set to `600`; private SSH key `600`, public key `644` | [Permissions practice](02-linux-permissions.md) |
| Firewall | Original UFW status not recorded | UFW rules and continuing SSH access checked (user-confirmed) | [UFW](04-ufw-firewall.md); live status output still to archive |
| SSH brute-force mitigation | Initial Fail2ban state not recorded | SSH jail configured; ban duration changed from 600 to 3600 seconds | [Fail2ban](05-fail2ban.md); actual client output |
| Package updates | APT initially rejected future-dated repository metadata | Checks later reported working; upgrade completion not recorded | [Updates and audit](06-security-updates-and-audit.md) |
| User access audit | Baseline not recorded | Individual permission practice completed; full sudo/user audit not archived | [Permissions](02-linux-permissions.md) |

## What changed and why

- **SSH:** Replacing password-based remote authentication with public-key authentication reduces exposure to password guessing. Root remote login is restricted separately.
- **Firewall:** UFW restricts incoming traffic while preserving the required SSH connection.
- **Fail2ban:** The `sshd` jail watches for repeated authentication failures and can temporarily ban source IP addresses.
- **Permissions:** A private key and other sensitive data should not be accessible to unrelated local users.
- **Updates:** APT's timestamp validation is a useful security check; it must not be bypassed to hide clock errors.

## Limitations

This is a controlled UTM lab, not a production penetration test. The current documents do not include a complete baseline, external attack simulation, vulnerability scan, raw screenshots, or a full administrative-account audit. Do not treat these as completed activities.
