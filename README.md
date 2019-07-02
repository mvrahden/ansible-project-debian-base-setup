# Initialization for Raspberry Pi (ARM-Provisioning)

This provisioning helps bringing the remote machines (specifically Raspberry Pis in this case) into a state that guarantees:

- accessability
- low energy consumption
- security.

## Pre-Requirements

### Operating Systems

- Raspbian Lite for RaspberryPi
- Debain (Ayufan Rock64 Build) for Rock64 (https://github.com/ayufan-rock64/linux-build/ or http://wiki.pine64.org/index.php/ROCK64_Main_Page) 

### SSH

After a fresh OS-flashing, make sure that the remote devices are accessible via ssh (on a Raspberry pi, e.g. place an empty file called `ssh` into the `/boot/`-directory) and that your ssh-id is introduced.
To achieve that, please execute from your local machine:

- `ssh-copy-id pi@{IP-ADDRESS, e.g. 192.168.1.150}` and verify with the default password `raspberry`

After this step, the remote machines are ready to be provisioned by Ansible

## How to use this ansible playbook

```
ansible-playbook provisioning.yml -k --extra-vars "ansible_become_pass=raspberry"
```

After executing this command, you'll be prompted for your **ssh passphrase**.

