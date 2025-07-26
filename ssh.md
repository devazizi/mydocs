### create ssh jump server to connect ssh in private network from bastion server

### at first of all open /etc/ssh/ssh_config

```
AllowTcpForwarding yes
PermitTunnel yes
```

### restart ssh service 
```
systemctl restart ssh
```

### you can directly connect using ssh command 
```
ssh -o ProxyJump=root@10.103.30.20:52499 root@192.168.100.224
```

#### go to ~/.ssh/config
```bash
Host serve-jump
  HostName your_public_ip
  Port 52499
  User root
  IdentityFile ~/.ssh/id_public

Host 192.168.100.*
  ProxyJump shm-jump
  IdentityFile ~/.ssh/id_public

```