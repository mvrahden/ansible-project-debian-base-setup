# Debian Base Setup

This provisioning helps bringing the remote debian machines (e.g. Raspberry Pis or Rock64 or other debian systems) into a homogenized state that guarantees:

- accessability
- low energy consumption from unused interfaces
- security.

## Pre-Requirements

### Operating Systems

The following is just an exemplary list.
Feel free to try your own **debian-based** system.

- Ubuntu Server (http://cdimage.ubuntu.com/releases/) or Debian (https://www.debian.org/releases/stable/)
- Debain (Ayufan Rock64 Build) for Rock64 (https://github.com/ayufan-rock64/linux-build/ or http://wiki.pine64.org/index.php/ROCK64_Main_Page)
- Raspbian Lite for Raspberry Pis

### SSH

After a fresh OS-flashing, make sure that the remote devices are accessible via ssh (on a Raspberry pi, e.g. place an empty file called `ssh` into the `/boot/`-directory) and that your ssh-id is introduced.
You'll need to discover the IPs of the devices with e.g. your router management interface, arp-scan or wireshark (please consult the web for this).
To achieve that, please execute from your local control machine for every host in the setup:

- `ssh-copy-id pi@{IP-ADDRESS, e.g. 192.168.1.150}` and verify with the default password e.g. `raspberry` or `rock64`

Make sure to repeat this process for **every** machine.
After this process is done, have a look at `inventory/hosts.ini`-file and verify the entries.

After this step, the remote machines are ready to be provisioned by Ansible.

## How to use this ansible playbook

```
ansible-playbook playbooks/provisioning.yml -k
```

After executing this command, you'll be prompted for your **ssh passphrase** and the process beginns.
For issues with Ansible or the connection, please consult an online search engine of your choice.

## OS Versions tested

This is a list of OS Versions that have been tested.
Feel free to add other Versions after successful runs.

- Raspbian Lite
  - Stretch | Nov 2019
  - Buster | Jun 2019

## License

MIT

## Author Information

- Menno van Rahden
