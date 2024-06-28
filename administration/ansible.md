# Ansible
- [Ansible.com](https://www.ansible.com/)
- [Ansible.com - community docs](https://docs.ansible.com/)
- [YouTube.com - Learn Linux TV - Getting started with Ansible (playlist)](https://www.youtube.com/playlist?list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70)
- [GitHub.com - Ansible for DevOps - Manuscript](https://github.com/geerlingguy/ansible-for-devops-manuscript) ([Buy on LeanPub](https://leanpub.com/ansible-for-devops))
- [GitHub.com - Ansible for DevOps Examples](https://github.com/geerlingguy/ansible-for-devops)

> [!abstract]- Note metadata
>
> Date: 2024.06.21
> Distro: Fedora Linux 40
> Kernel: 6.9.4-200.fc40.x86_64
> Desktop: KDE Plasma v: 6.1.0

# Priming

- Created 3 VirtualBox VMs: 
    - 2x [Rocky Linux 9.4 minimal](https://rockylinux.org/download) 
    - 1x [Debian 12 minimal](https://www.debian.org/CD/netinst/)
- Portforwarding created from host 51122, 51222, 51322 to guests 22
- add ansible sudo user: `ansibinsi`
    - No sudo password needed for ansibinsi: `echo 'ansibinsi ALL=(ALL:ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/ansibinsi`
- ssh key (no passphrase) added to authorizes_keys:
```
ssh-keygen -t ed25519 -f ~/.ssh/ansible-test_vms-ed25519 -C ansible-test_vms-ed25519 && \
ssh-copy-id -i ~/.ssh/ansible-test_vms-ed25519.pub -p 51122 ansibinsi@127.0.0.1
```

Install ansible on host (fedora 40):
```
sudo dnf install ansible.noarch
```

# Project test-vms
- [Ansible.com - Getting started with Ansible](https://docs.ansible.com/ansible/latest/getting_started/index.html)

## Inventory
- [Ansible.com - inventory parameters](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#connecting-to-hosts-behavioral-inventory-parameters)

Create inventory.yaml file:
```
mkdir -p ansible/testVMs && \
cd ansible/testVMs && \
vim inventory.yaml
```
`inventory.yaml`:
```yaml
---
test_vms:
  hosts:
    testvm_debian12:
      ansible_host: 127.0.0.1
      ansible_port: 51122
      ansible_ssh_private_key_file: ~/.ssh/ansible-test_vms-ed25519
      ansible_user: ansibinsi
  hosts:
    testvm_rocky9:
      ansible_host: 127.0.0.1
      ansible_port: 51222
      ansible_ssh_private_key_file: ~/.ssh/ansible-test_vms-ed25519
      ansible_user: ansibinsi
  hosts:
    testvm_rocky9_n2:
      ansible_host: 127.0.0.1
      ansible_port: 51322
      ansible_ssh_private_key_file: ~/.ssh/ansible-test_vms-ed25519
      ansible_user: ansibinsi
```
_Don't use spaces ` `, hyphens `-` or preceding numbers `1337_server` in group names._

> [!tip] Optimizing `inventory.yaml`
> 
> > [!todo]- Assigning a variables to multiple hosts:
> >
> > ```
> > ---
> > test_vms:
> >   hosts:
> >     testvm-debian12:
> >       ansible_port: 51122
> >     testvm_rocky9:
> >       ansible_port: 51222
> >     testvm_rocky9_n2:
> >       ansible_port: 51322
> >     vars:
> >       ansible_host: 127.0.0.1
> >       ansible_ssh_private_key_file: ~/.ssh/ansible-test_vms-ed25519
> >       ansible_user: ansibinsi
> > ```



Verify inventory:
```shell
ansible-inventory -i inventory.yaml --list
```

> [!quote]- &nbsp;Output
> 
> ```
> {
>     "_meta": {
>         "hostvars": {
>             "testvm-debian12": {
>                 "ansible_host": "127.0.0.1",
>                 "ansible_port": 51122,
>                 "ansible_ssh_private_key_file": "~/.ssh/ansible-test_vms-ed25519",
>                 "ansible_user": "ansibinsi"
>             },
>             "testvm_rocky9": {
>                 "ansible_host": "127.0.0.1",
>                 "ansible_port": 51222,
>                 "ansible_ssh_private_key_file": "~/.ssh/ansible-test_vms-ed25519",
>                 "ansible_user": "ansibinsi"
>             },
>             "testvm_rocky9_n2": {
>                 "ansible_host": "127.0.0.1",
>                 "ansible_port": 51322,
>                 "ansible_ssh_private_key_file": "~/.ssh/ansible-test_vms-ed25519",
>                 "ansible_user": "ansibinsi"
>             }
>         }
>     },
>     "all": {
>         "children": [
>             "ungrouped",
>             "test_vms"
>         ]
>     },
>     "test_vms": {
>         "hosts": [
>             "testvm-debian12",
>             "testvm_rocky9",
>             "testvm_rocky9_n2"
>         ]
>     }
> }
> ```

Ping the test_vms group:
```
ansible test_vms -m ping -i inventory.yaml
```

> [!quote]- &nbsp;Output
> 
> ```
> testvm-debian12 | SUCCESS => {
>     "ansible_facts": {
>         "discovered_interpreter_python": "/usr/bin/python3"
>     },
>     "changed": false,
>     "ping": "pong"
> }
> testvm_rocky9_n2 | SUCCESS => {
>     "ansible_facts": {
>         "discovered_interpreter_python": "/usr/bin/python3"
>     },
>     "changed": false,
>     "ping": "pong"
> }
> testvm_rocky9 | SUCCESS => {
>     "ansible_facts": {
>         "discovered_interpreter_python": "/usr/bin/python3"
>     },
>     "changed": false,
>     "ping": "pong"
> }
> ```

> [!info] Further details:
>
> - [How to build your inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#intro-inventory)
> - [Adding variables to inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#variables-in-inventory)
> - [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/vault.html#vault)

## Playbook
- [Ansible.com - Creating a playbook](https://docs.ansible.com/ansible/latest/getting_started/get_started_playbook.html)
# TODO
