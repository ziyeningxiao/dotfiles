# Linux General Configuration Guide

- [English](./README.md) | [中文](./docs/README.zh-CN.md)

## 1. Required Software

- git
- stow
- zsh
- kitty or terminator

## 2. Configuration and Optimization

### 1. oh-my-zsh

- In the "dotfiles" project, oh-my-zsh is a submodule and can be installed directly via symlinks

```bash
# Clone the dotfiles project from GitHub
mkdir -p ~/Documents/github
git clone https://github.com/ZavierPei/dotfiles.git ~/Documents/github/dotfiles --recurse-submodules

# Enter the dotfiles directory
cd ~/Documents/github/dotfiles

# Switch to zsh
/bin/zsh

# Change the default shell
chsh -s /bin/zsh

# Link oh-my-zsh and configuration files
stow -t ~ oh-my-zsh

# Install plugins and themes
git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k

# If GitHub is not accessible, use Gitee mirrors as fallback
# Project repository
git clone https://gitee.com/ZavierPei/dotfiles.git ~/Documents/github/dotfiles --recurse-submodules
# Plugin mirrors
git clone https://gitee.com/qingmengfengyun/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://gitee.com/qingmengfengyun/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://gitee.com/qingmengfengyun/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k

# Apply the configuration
source ~/.zshrc
```

**If the Powerlevel10k fonts are missing, you can download the four `.ttf` files from [powerlevel10k-media](https://gitee.com/qingmengfengyun/powerlevel10k-media), create the directory `~/.local/share/fonts/ttf/MesloLGS NF`, and place the downloaded files there.**

### 2. Neovim

- [Neovim official GitHub repository](https://github.com/neovim/neovim)

#### Installation method

```bash
# 1. Install dependencies: the official pre-built binary requires some base libraries to run
sudo dnf install compat-lua-libs libtermkey libtree-sitter libvterm luajit luajit2.1-luv msgpack unibilium xsel

# 2. Download the appropriate Neovim package for your system architecture (e.g., x86, arm), extract it to "/opt/nvim-linux64", and create a symbolic link in `~/.local/bin`
ln -s /opt/nvim-linux64/bin/nvim ~/.local/bin/
```

### 3. Installing GNOME Yaru Theme

- Prerequisites

```bash
# Install core dependencies
sudo dnf install gtk-murrine-engine sassc gnome-themes-extra

# Install GNOME Tweaks
sudo dnf install gnome-tweaks

# Install the User Themes extension
sudo dnf install gnome-shell-extension-user-theme
```

- Installing the Yaru theme

```bash
# Option 1: Install the base Yaru theme package (this automatically installs GTK and Shell themes)
sudo dnf install gnome-shell-theme-yaru

# Option 2: Install the full Ubuntu-style suite (icon theme, cursor theme, sound theme, and complete GTK2/3/4 support)
sudo dnf install yaru-icon-theme yaru-cursor-theme yaru-sound-theme yaru-gtk2-theme yaru-gtk3-theme yaru-gtk4-theme
```

### 4. GNOME Desktop Optimization

- Required applications: gnome-tweaks, gnome-shell-extensions

#### Essential extensions

- `Dash to Dock`  
  Quickly launch applications and switch between windows and desktops
- `NetSpeed`  
  Display network speed, memory usage, battery status, etc.
- `User Themes`  
  Load theme extensions
- `AppIndicator and KStatusNotifierItem Support`  
  Status bar plugin
- `Blur my Shell`  
  Background blur plugin
- `Hide Top Bar`  
  Hide the top bar plugin

## 3. Appendix

### 1. Enabling the SSH Service

```bash
# Install the SSH service
sudo dnf install openssh-server

# Start the SSH service
sudo systemctl start sshd

# Enable the SSH service to start automatically on boot
sudo systemctl enable sshd

# Check the SSH service status
sudo systemctl status sshd
```

### 2. VMtools Mount Command at Boot

- Automatically mount host paths when the virtual machine boots
- Requires the crontab tool

```bash
sudo crontab -e
# Enter the following command
@reboot mount -t fuse.vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other
```

### 3. Adding SSH Keys

- Generate an SSH key

```bash
ssh-keygen -t rsa -b 4096
```

- If you need to specify a key name (e.g., "gitkey"), add the following content to the config file

```bash
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitkey

# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitkey
```

### 4. Fish Shell Configuration

- Installation and optimization steps for the fish shell

```bash
# Install fish shell
sudo dnf install fish

# Install oh-my-fish
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish

# Link configuration files
cd ~/Documents/github/dotfiles && stow -t ~ fish
```

- **[oh-my-fish official GitHub repository](https://github.com/oh-my-fish/oh-my-fish)**
