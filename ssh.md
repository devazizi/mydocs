# SSH related commands

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


## ssh common topics

### run a command directly in ssh 
```ssh user@remote-server "uptime && df -h"```

### get file from server to local or reverse 
```scp -i ~/.ssh/key.pem file.txt user@host:/path/```
### get it recursively
```scp -r ./mydir user@host:/remote/path/```

### socks proxy 
```ssh -D 1080 user@remote-host```

### local portforward and tunneling 
```ssh -L 8888:localhost:3306 user@remote```

### remote portforward
```ssh -R 9000:localhost:8080 user@remote```


##Option	Description
- User	Specify user (e.g. -o User=root)
- Port	Use specific port (e.g. -o Port=2222)
- IdentityFile	Use a custom SSH key (e.g. -o IdentityFile=~/.ssh/id_custom)
- StrictHostKeyChecking=no	Do not prompt for unknown host keys
- UserKnownHostsFile=/dev/null	Don’t save the host key (use with the above)
- ProxyJump	Define a jump/bastion host
- ConnectTimeout=10	Set connection timeout
- ForwardAgent=yes	Forward SSH agent to remote


## sshuttle
### sshuttle is a VPN-like tool that transparently routes traffic over SSH using iptables and Python. It lets you access remote private networks through an SSH connection — without needing root access on the server.

## You have
- A jump server user@jump-host with access to private subnet 192.168.100.0/24
- You want your local machine to access 192.168.100.x servers as if they were on your LAN.

```sshuttle -r user@jump-host 192.168.100.0/24```

## Now you can:
```
ping 192.168.100.10
ssh 192.168.100.20
curl http://192.168.100.30
```
