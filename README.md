# ubuntu-dev-machine-setup | Ubuntu 24.04

## Description

This repo contains Ansible playbooks to configure Ubuntu wsl2 fro development enviroment.

The playbooks should run in Debian based system with minor modifications but was only tested with:

- **Ubuntu 24.04 LTS (Noble Numbat)**

---

## What gets installed and cofigured?

I am a DevSecOps Engineer and my daily job include working with AWS, docker, ansible, terraform, etc. So if you are in a similar profession the installed system will suit your needs. It is also easy to extend using Ansible roles.

Summary of packages that get installed and configured based on roles:

- **role: base**
  - mount `/tmp` on tmpfs (reduce SSD read writes and increase SSD lifespan; no leftover files on system shutdown)
  - set default system editor to vim instead of nano
  - disable system crash reports
  - tune system swappiness so that swapping is greatly reduced
  - upgrade all packages
  - install archiving tools like zip, rar, etc
  - install development related packages like android-tools, awscli, httpie, clusterssh, docker, filezilla, golang, pipenv, etc
  - install code fomatters and linters like black, ruff, ansible-lint, etc
  - setup golang directories
  - install download tools like axel, transmission, wget, aria2
  - install image, audio and video packages like vlc, totem, gimp, imagemagick, etc
  - install virtualization tools like virtualbox, docker, docker-compose
  - install and configure ssh server if not set to `laptop_mode`
  - option to turn on night light settings for eye comfort (set `base_permanent_night_light.night_light_enabled` to `True`)
  - enable `fzf` fuzzy finder in zsh terminal; check out this [YouTube video](https://www.youtube.com/watch?v=1a5NiMhqAR0) to see how to use it
  - install terminal emulators Tilix and Alacritty
    - Alacritty will have tmux enabled by default (edit `~/.config/alacritty/alacritty.toml` to disable)
  - install [lazygit](https://github.com/jesseduffield/lazygit)
- **role: zsh**
  - install zsh package and set user shell to zsh
  - install antigen zsh plugin manager
  - copy and enable sample `~/.zshrc` file if one does not exist
    - contains function to stop ssh-agent from asking for encrypted ssh key password repeatedly when launching new terminal
    - adds additional shell aliases and functions in `~/.shell_aliases` and `~/.shell_functions`
  - install ohmyzsh/ohmyzsh and enable some bundled plugins
  - enable bullet train zsh theme (others like p10k can be configured as well)
- **role: terminal_customizations**
  - download and install some nerd fonts from ryanoasis/nerd-fonts; these are mono fonts ideal for use in terminal or programming editors
  - copy and enable sample tilix config file with configured nerd font
  - copy sample alacritty config file
  - copy and enable sample tmux config file if one does not exist
  - copy and enable sample `~/.tmux.conf` file with [tmux plugin manager](https://github.com/tmux-plugins/tpm) and several tmux plugins
    - open Tilix terminal and run `tmux` command, or enable custom command option in Tilix
    - edit `~/.tmux.conf` if necessary
- **role: vim**
  - install vim packages
  - install [amix/vimrc](https://github.com/amix/vimrc) vim distribution
  - create sample vim customization file in `~/.vim_runtime/my_configs.vim`
    - additional vim settings are enabled in `~/.vim_runtime/my_configs.vim` which are not part of the Vim Distribution. Edit this file if necessary.
---

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
git clone https://github.com/fazlearefin/ubuntu-dev-machine-setup.git
cd ubuntu-dev-wsl-setup
```

## Step 1 | Running the playbooks to configure your system

**Invoke the following as yourself, the primary user of the system. Do not run as `root`.**

```bash
ansible-playbook main.yml -vv -e "local_username=$(id -un)" -K
```

Enter the sudo password when asked for `BECOME password:`.

The `main.yml` playbook will take anything from 15 minutes to an hour to complete.

After all is done, give your laptop a new life by rebooting.

---