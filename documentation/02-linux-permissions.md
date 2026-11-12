# Linux File Permissions

## Objective

Learn how Linux file permissions work and how to restrict access to sensitive files.

## Commands Used

`mkdir ~/permission-lab`

Created a directory for practicing Linux permissions.

`touch ~/permission-lab/test.txt`

Created a test file.

`chmod 600 ~/permission-lab/test.txt`

Changed the permissions so only the owner can read and write the file.

`ls -l ~/permission-lab/test.txt`

Verified the file permissions.

## Results

The test file has the following permissions:

`-rw-------`

This means:

- Owner: Read and write
- Group: No permissions
- Others: No permissions

I also checked the permissions of the SSH key files:

- Private key (`id_ed25519`): `600`
- Public key (`id_ed25519.pub`): `644`

## What I Learned

Linux permissions control which users can access files and directories.

The `chmod` command changes permissions, while `ls -l` displays them.

Restricting access to private SSH keys is important because an exposed private key could allow unauthorized access to a server.
