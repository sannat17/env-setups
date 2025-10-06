## Post-Installation Troubleshooting
Notes on critical challenges encountered after a fresh installation and the solutions that worked.

### Pitfall: Black Screen After Boot with Nvidia Drivers
Even after a successful installation of the Nvidia drivers, the system may boot to a black screen. My solution so far was to force the system to start in basic graphics mode after which the nvidia drivers can kick in when running the system.

Solution:
- Edit the file `/etc/default/grub`.
- Append `nomodeset` to the existing kernel options within the `GRUB_CMDLINE_LINUX` string.
- Regenerate the GRUB configuration to apply the changes:
  ```
  sudo grub2-mkconfig -o /boot/grub2/grub.cfg
  ```
- A reboot is required for the changes to take effect.

### Note on Nvidia Driver Installation
For modern Nvidia cards (RTX 40/50-series), the open-kernel modules are often recommended. The installation process can change, so the best practice is to refer to the latest official documentation at the time of installation.
Primary Source: RPM Fusion - Howto/NVIDIA

### Note on DNF Repositories
- When adding a third-party dnf repository, you are trusting its maintainers. When searching for a package, dnf checks all enabled repositories. 
  The source for a package is determined first by repository priority (lower numbers are higher priority), and then by the latest version number if priorities are equal. 
  While Fedora's default priorities prevent core system packages from being replaced, a malicious or compromised repository could still provide altered versions of other software, leading to supply-chain attacks. 
- Only add repositories from sources you explicitly trust (e.g., RPM Fusion, Microsoft, Google). Be aware of potential package conflicts if multiple third-party repos have the same priority.
- When installing using a .rpm (for e.g. Google Chrome), a repo is sometimes automatically added to manage installation and updates.
- Refer to [Fedora COPR](https://copr.fedorainfracloud.org/), a common resource for adding third party repos (reminds me of conda-forge).

## Common Packages and Software
A quick-reference list of preferred tools and packages.
Note: 
> A lot of these are part of official fedora packages, so they may not be the latest version that is otherwise available (for e.g. through their github, or other package managers like snap).
> 
> If the verison on fedora doesn't suffice, refer to maintainer documentation or their dnf repo to get the latest or nightly build / version.

### Productivity
- CopyQ - Clipboard manager (mainly for clipboard history feature)
  ```
  sudo dnf install copyq
  ```
### Development Tools
- [VS Code](https://code.visualstudio.com/docs/setup/linux#_rhel-fedora-and-centos-based-distributions)
  ```
  sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
  echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null

  sudo dnf check-update
  sudo dnf install code
  ```
- [Neovim](https://github.com/neovim/neovim/blob/master/INSTALL.md#fedora)
  ```
  sudo dnf install -y neovim python3-neovim
  ```
  TBA: Configs and plugins

### Terminal & Shell
- [Kitty Terminal](https://sw.kovidgoyal.net/kitty/)
  ```
  sudo dnf install kitty
  ```
  TBA: Kitty config
- Zsh Shell & Oh My Zsh
  ```
  sudo dnf install zsh
  sh -c "$(curl -fsSL [https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh](https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh))"
  ```
  TBA: omz config and plugins
### System Utilities
- htop - Interactive process viewer (`top` on steroids)
  ```
  sudo dnf install htop
  ```
- [fastfetch](https://github.com/fastfetch-cli/fastfetch) - A maintained, feature-rich and performance oriented, neofetch like system information tool.
  ```
  sudo dnf install fastfetch
  ```
### Wayland (IPR - Instructions and Config TBA)
Window Manager / Wayland Compositor with Dynamic Tiling. Known for being highly customizable and visually appealing (built-in features like window animations, rounded corners, and blur effects for a more aesthetic desktop experience).
