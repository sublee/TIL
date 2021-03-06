## Disable host key checking

in the `/etc/ansible/ansible.cfg` or `~/.ansible.cfg` or `./ansible.cfg`
vi

```ini
[defaults]
host_key_checking = False
```

Or

```bash
$ export ANSIBLE_HOST_KEY_CHECKING=False
```

reference: https://stackoverflow.com/a/23094433


## Slow ansible-playbook commands with dynamic inventory

reference: https://github.com/ansible/ansible/issues/22633#issuecomment-293956861

> if your inventory does not return a `_meta` key 
> Ansible will call `--list` once, and then call `--host` for every host in the inventory. This can often cause confusion as to why when `--list` is run, why running via ansible takes longer.


It would need to at least have `'_meta': {'hostvars': {}}`


## run_once

`When used together with “serial”, tasks marked as “run_once” will be run on one host in each serial batch.`

```yml
---
- hosts: webservers[0]
```

```yml
- name: run once task
  shell: '..'
  when: inventory_hostname == webservers[0]
```