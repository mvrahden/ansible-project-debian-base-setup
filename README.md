# Debian Base Setup <!-- omit in toc -->

This provisioning helps bringing the remote debian machines (e.g. Raspberry Pis or Rock64 or other debian systems) into a homogenized state that guarantees:

- accessability
- low energy consumption (interface shutdown)
- security
- synchronicity
- package management bandwitdth efficiency
- swapfile-based memory management.

## Table of Contents <!-- omit in toc -->

- [Prerequisites](#Prerequisites)
  - [Operating Systems](#Operating-Systems)
  - [SSH](#SSH)
- [How to de-/encrypt a vault-file](#How-to-de-encrypt-a-vault-file)
- [How to use this Ansible playbook bundle](#How-to-use-this-Ansible-playbook-bundle)
- [OS Versions tested](#OS-Versions-tested)
- [License](#License)
- [Author Information](#Author-Information)

## Prerequisites

- install required roles: `ansible-galaxy install -r requirements.yml`

### Operating Systems

The following list is just an exemplary collection of debian-based operating systems that have been [tested with this playbook](#OS-Versions-tested).

- Ubuntu Server (http://cdimage.ubuntu.com/releases/) or Debian (https://www.debian.org/releases/stable/)
- Debain (Ayufan Rock64 Build) for Rock64 (https://github.com/ayufan-rock64/linux-build/ or http://wiki.pine64.org/index.php/ROCK64_Main_Page)
- Raspbian Lite for Raspberry Pis

Feel free to try other **debian-based** systems – in principle nothing can go wrong, as long as you try it on a device that:

- has no sensible data and
- is ideally physically accessible to you,
- otherwise please take precautionary measures, e.g. as documented [here](http://www.gnu.org/software/grub/manual/legacy/grub.html#Making-your-system-robust).

### SSH

After a fresh OS-flashing, make sure that the remote devices are accessible via ssh (on *Raspbian*, e.g. place an empty file called `ssh` into the `/boot/`-directory) and that your ssh-id is introduced.
To create your SSH key consult a search-engine of your choice and create a key **with** passphrase.
You'll need to discover the IPs of the devices with e.g. your router management interface, arp-scan or wireshark (please consult the web for this).
To achieve that, please execute from your local control machine for every host in the setup:

- `ssh-copy-id {USERNAME, e.g. pi}@{IP-ADDRESS, e.g. 192.168.1.150}` and verify with the default password e.g. `raspberry` or `rock64`

Make sure to repeat this process for **every** machine.
After this process is done, have a look at `inventory/hosts.ini`-file and verify the entries.

After this step, the remote machines are **almost** ready to be provisioned by Ansible.
We just need to set our variables accordingly, namely our public ssh-keys.

For this create a file `inventory/group_vars/all/vault.yml` and paste your public key/s (get it e.g. via `cat ~/.ssh/id_rsa.pub`) into this file.
See `inventory/group_vars/all/vault.example.yml` for the format.
Encrypt the `vault`-file with your public keys by following the subsequent section.

## How to de-/encrypt a vault-file

This setup deals with data, that we better not log into our version control system in an unencrypted manner.
To protect sensible data from being unintendedly exposed to unauthorized third parties, we use the Ansible best practices:

1. prefix sensitive variables with `vault_`, e.g. `vault_public_keys`
2. create a file dedicated for encryption, e.g. `vault` on the same directory level.
3. reference that prefixed variable (`vault_public_keys`) inside this vault.
4. Create a secure token and paste it into the `.vault.key`-file (which is referenced in the ansible.cfg) and encrypt the `vault`-file with e.g. the following command:   
   `ansible-vault encrypt inventory/group_vars/all/vault.yml` (assuming your command line is on project root).
5. To decrypt the file use `decrypt` instead of `encrypt`.

## How to use this Ansible playbook bundle

The playbook bundle consists of one primary **playbook**, divided in three plays:

- The first play ensures, that a python package exists on the hosts, in order to play Ansible plays.
- The second play connects to the hosts defined in `inventory/hosts.ini` (an initial inventory), updates the systems packages and allocates a static IP to each individual host. In the `inventory/hosts.ini`-file you can define the static IPs to be allocated with the `host_static_ip` variable. The other variable `ansible_host` is the **currently** negotiated IP.
  After this play, hosts will be accessible with their freshly allocated static IPs.
- The third play inhibits the actual homogenization tasks and provisions them onto the hosts.

After inventory and all variable and vault-files under `inventory/group_vars/` have been examined carefully and changed accordingly, execute the following command:

```bash
ansible-playbook playbooks/provisioning.yml -k
```

You'll be prompted for your **ssh passphrase** and the process beginns.
For issues with Ansible or the connection, please consult an online search engine of your choice.

## OS Versions tested

This is a list of OS Versions that have been tested.
Feel free to add other Versions after successful runs.

- Raspbian Lite
  - Stretch | Nov 2019
  - Buster | Jun 2019
- Debian (Rock64 – Ayufan Builds)
  - Stretch minimal | 0.7.9 | 0.8.3

## License

MIT

## Author Information

- Menno van Rahden
