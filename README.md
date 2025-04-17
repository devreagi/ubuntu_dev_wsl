# ubuntu-dev-machine-setup | Ubuntu 24.04
Base repo from https://github.com/fazlearefin/ubuntu-dev-machine-setup
## Description

This repo contains Ansible playbooks to configure Ubuntu wsl2 with zsh, docker without docker desktop and some useful tools, its a personal modification from **fazlearefin** repository.

The playbooks should run in Debian based system with minor modifications but was only tested with:

- **Ubuntu 24.04 LTS (Noble Numbat) WSL2**


## Step 0 | Pre-requisites for running the ansible playbooks

On the system which you are going to setup using Ansible, perform these steps.

You need to install `ansible` and `git` before running the playbooks. You can either install it using `pip` or `apt`.

```bash
/usr/bin/sudo apt update
/usr/bin/sudo apt full-upgrade -y
/usr/bin/sudo apt install ansible git -y
```

And clone this repo (do not clone in `/tmp` as this dir is cleaned and mounted in tmpfs)

```bash
git clone https://github.com/devreagi/ubuntu_dev_wsl.git
cd ubuntu_dev_wsl
```

## Step 1 | Running the playbooks to configure your system

**Invoke the following as yourself, the primary user of the system. Do not run as `root`.**

```bash
ansible-playbook main.yml -vv -e "local_username=$(id -un)" -K
```

Enter the sudo password when asked for `BECOME password:`.

The `main.yml` playbook will take anything from 15 minutes to an hour to complete.

After all is done, restart wsl.

```bash
wsl --shutdown
wsl -d Ubuntu
```

---