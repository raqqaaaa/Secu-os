```bash

HostKey /etc/ssh/ssh_host_ed25519_key

KexAlgorithms curve25519-sha256@libssh.org

Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com

MACs hmac-sha2-512-etm@openssh.com

PasswordAuthentication yes
PubKeyAuthentication yes
AuthenticationMethods publickey password

LogLevel VERBOSE

Subsystem sftp  /usr/lib/ssh/sftp-server -f AUTHPRIV -l INFO

PermitRootLogin No

Port 2222

Match User it4
    ChrootDirectory /jail/it4
    AllowTcpForwarding no
    X11Forwarding no
    PermitTunnel no
    AllowAgentForwarding no
```

```bash
cat .ssh/authorized_keys
```

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIn29vGuYAyEcmwkSS4pmpgxjsVSgs5n/1qF2DfP0vBZ it4@nowhere
```