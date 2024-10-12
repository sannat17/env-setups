# env-setups

## Conda
- [https://docs.anaconda.com/miniconda/#quick-command-line-install](https://docs.anaconda.com/miniconda/#quick-command-line-install)
  - Change to the correct platform and cpu arch (eg. replace the x86 installer link from script to aarch64 based installer when working on jetson).
- `conda config --set auto_activate_base false`
  - I only want to use conda for some projects so I don't want all terminal sessions to have a conda base already activated.
- `conda env config vars set PYTHONNOUSERSITE=True`
  - Sometimes in conda envs, if there are packages installed at user level (by `pip install --user`), then they take precedence over site-packages within the conda env.
  - This was causing some serious bugs when I was trying to `pip install` packages to conda env, since pip was a locally installed site-package in `~/.local/...` and thust installed any new packages there.
