# Demo for parsing Ansible Vault via Dynamic Inventory

### Problem

Ansible currently [does not have the functionality](https://github.com/ansible/ansible/issues/9771) to access [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html) for actions occuring during Dynamic Inventory runs. 

### Solution

Use the Dynamic Inventory script to decrypt and parse the Ansible Vault encrypted file in order to access the secrets.

### Prep

In this example, I'm using the [Vagrant Dynamic Inventory](https://gist.github.com/lorin/4cae51e123b596d5c60d) from Lorin Hochstein (author of "Ansible Up and Running"). The Vagrantfile is using the "ubuntu/trusty64" box, but feel free to replace it with whatever box you may already have on your system, it actually doesn't get used other than for the inventory.

### How to run:

```
> vagrant up
> ansible-playbook -i vagrant.py playbook.yml
> cat /tmp/secret_value.yml
```

Note: If you get an error about execution permissions, then do `chmod 755 vagrant.py` and try again.

### Where to look for the action

To see the relevant code, look at `vagrant.py` starting with line 52.

### Explanation

When you run the playbook with the dynamic inventory, the dynamic inventory runs the `ansible-vault view` command in order to get the yaml from the encrypt vault file `credentials.conf`. The dynamic inventory then parses the yaml to get the secret file and then write it to `/tmp/secret_value.yml` as a demo that it can access the secret value.

Note that I have the `--vault-password-file` set to `vault-pass`. You would naturally never do this in real-life. In real-life, I'd recommend putting your vault password files in `~/.ssh/` where you already have the security locked-down.