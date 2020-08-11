# dev-vm

## Template for the .secret.yml file

```yml
---

remote_sshuttle_host: 1.2.3.4
remote_sshuttle_port: 22

ssh_user: MyUser
ssh_password: MyPass

insecure_registries:
  - mydockerregisty.io:18444

sshuttle_subnets:
  - 8.6.7.8/25
  - 1.2.3.4/25
```
