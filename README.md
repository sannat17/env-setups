# env-setups
## Terminal Productivity
- [Oh My Zsh](https://ohmyz.sh/#install) : Better terminal experience (auto complete, colouring, plugins, etc.)
- 

## Macbook Productivity Tools
- [iterm2](https://iterm2.com/) : Much better terminal experience (still have to try the newer "hot" terminals but this is reliable and tested)
- [brew.sh](https://brew.sh/) : De facto package manager for Mac
- [Raycast](https://www.raycast.com/) : Supercharged spotlight search (also has clipboard history which is another great windows feature missing on macs).
- [Rectangle](https://rectangleapp.com/) : Better window management (I still like the way, surprise surprise, windows handles windows and desktop management).
- [Mac Mouse Fix](https://macmousefix.com/) : Better for mice (mission control, window switching, horizontal scroll, etc.) to bridge gap between regular mice and trackpad / logitech mx master.

## Windows Productivity Tools
- [PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/)
  - [Command Pallette](https://learn.microsoft.com/en-us/windows/powertoys/#command-palette)
  - [Keyboard Manager](https://learn.microsoft.com/en-us/windows/powertoys/#keyboard-manager)
  - [Workspaces](https://learn.microsoft.com/en-us/windows/powertoys/#workspaces)
- In terminal run as admin: `wsl --install`

## Git
- On Ubuntu:
  - Install: `sudo apt update && sudo apt install git`
  - Setup default username and email for commits: `git config --global user.name "" && git config --global user.email ""`
  - Signing commits (for github, for example):
    - `gpg --full-generate-key`
    - Keep pressing enter to accept all defaults and set name, email, password, etc. to protect the key
    - `gpg --list-secret-keys --keyid-format=long` -> Copy the key id (should be after / on line with sec)
    - `gpg --armor --export <key_id>` -> Copy -> https://github.com/settings/keys -> Generate new gpg key (give a name to the machine / session for future management) and paste
    - Set the key for signing commits `git config --global user.signingkey <key_id>` (exclude global to make this signing logic only for project)
    - `git config --global commit.gpgsign true && git config --global tag.gpgSign true` (if you dont want to add `-S` with every commit to make sure it is signed)
    - Tell GPG which terminal to use for the passphrase prompt (if you set a password for key): `[ -f ~/.bashrc ] && echo -e '\nexport GPG_TTY=$(tty)' >> ~/.bashrc`
      - Alternatively, add this whatever shell setup file is being used (zshrc, config.fish, etc.)
  - For pulling from github private repos, use finegrained PAT: https://github.com/settings/personal-access-tokens/new
    - [UNSECURE] Optionally, store token locally using `git config --global credential.helper store`. NOTE: Stores the credentials in plaintext in .git-credentials so understand the risk before storing (and only on trusted machine)

## Conda
- [https://docs.anaconda.com/miniconda/#quick-command-line-install](https://docs.anaconda.com/miniconda/#quick-command-line-install)
  - Change to the correct platform and cpu arch (eg. replace the x86 installer link from script to aarch64 based installer when working on Nvidia Jetson Devices).
- `conda config --set auto_activate_base false`
  - I only want to use conda for some projects so I don't want all terminal sessions to have a conda base already activated.
- `conda create -n my_env python=3.10`
  - for python 3.10, else use `conda create -n my_env python` to install the latest
- `conda env config vars set PYTHONNOUSERSITE=True -n my_env`
  - Alternatively, include the environment variable preset in `conda_env.yml`:
    ```
    name: my_env
    channels:
      - defaults
    dependencies:
      - python=3.10
    variables:
      PYTHONNOUSERSITE: True
    ```
    Folllowed by `conda env create -f conda_env.yml` to create the env with this preset
  - Sometimes in conda envs, if there are packages installed at user level (by `pip install --user`), then they take precedence over site-packages within the conda env.
  - This was causing some serious bugs in one instance when I was trying to `pip install` packages to conda env, but pip was a locally installed site-package in `~/.local/...` and thus installed any new packages there. 
    - NOTE: This is very esoteric as, in most cases, one would use conda to manage the packages instead of pip.

- To delete: `conda remove -n ENV_NAME --all`
