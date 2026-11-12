# SSH Hardening

## 1. Objective

The objective of this stage is to secure remote access to my Ubuntu server using SSH key authentication and reduce the risk of unauthorized access.

## 2. Environment

- Operating System: Ubuntu 26.04.1 LTS
- Virtual Machine: UTM
- Host Computer: macOS (Apple M2)
- SSH Server IP: 192.168.64.3
- Ubuntu Username: nikoo

## 3. Initial SSH Connection

Command:

`ssh nikoo@192.168.64.3`

Purpose:

Connect to the Ubuntu server remotely from my Mac.

Result:

Successfully connected to Ubuntu using password authentication.

## 4. Generate SSH Keys

Command executed on macOS:

`ssh-keygen -t ed25519 -C "mac-ubuntu-hardening"`

Purpose:

Generate a public and private SSH key pair for secure authentication.

Two files were created:

- `id_ed25519` — Private key stored on the Mac.
- `id_ed25519.pub` — Public key to be copied to Ubuntu.

The private key must remain secret.

Result:

SSH key pair generated successfully.

## 5. Copy the Public Key to Ubuntu

Command executed on macOS:

`cat ~/.ssh/id_ed25519.pub | ssh nikoo@192.168.64.3 'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'`

Purpose:

Add the Mac's public key to the Ubuntu user's authorized keys.

The `authorized_keys` file contains public keys permitted to authenticate to the user account.

Result: Pending verification.

## 6. Test SSH Key Authentication

Command executed on macOS:

`ssh -o PreferredAuthentications=publickey -o PasswordAuthentication=no -i ~/.ssh/id_ed25519 nikoo@192.168.64.3`

Purpose:

Verify that SSH authentication works using the private key without falling back to the Ubuntu account password.

Expected result:

Successfully connect to Ubuntu using SSH key authentication.

Result: Pending verification.

## 7. Disable Insecure SSH Authentication

Planned security improvements:

- Disable direct SSH login for the root account.
- Disable SSH password authentication after confirming that key-based login works.
- Check SSH server configuration.
- Verify that remote access continues working.

Status: Not yet completed.

## 8. Security Benefits

SSH hardening helps reduce the risk of unauthorized remote access.

Public-key authentication reduces exposure to password-guessing attacks.

Disabling direct root login limits direct access to the administrative account.

## 9. Next Steps

- Verify SSH key authentication.
- Configure SSH authentication restrictions.
- Test the final configuration.
- Document the results.
