# Linux通用配置指南

- [Englist](../README.md) | [中文](./README.zh-CN.md)

## 一、需要安装软件

- git
- stow
- zsh
- kitty or terminator

## 二、配置和优化

### 1.oh-my-zsh

- 在"dotfiles"项目中，oh-my-zsh是其子项目,可以直接通过软连接安装

```bash
# 从github拉取dotfiles项目
mkdir -p ~/Documents/github
git clone https://github.com/ZavierPei/dotfiles.git ~/Documents/github/dotfiles --recurse-submodules

# 进入dotfiles目录
cd ~/Documents/github/dotfiles

# 切换到zsh
/bin/zsh

# 修改默认shell
chsh -s /bin/zsh

# 链接oh-my-zsh及配置文件 
stow -t ~ oh-my-zsh

# 安装插件和主题
git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k

# 如果github无法访问，可以用gitee备用地址
# 项目地址
git clone https://gitee.com/ZavierPei/dotfiles.git ~/Documents/github/dotfiles --recurse-submodules
# 插件地址
git clone https://gitee.com/qingmengfengyun/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://gitee.com/qingmengfengyun/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://gitee.com/qingmengfengyun/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k

# 使用配置
source ~/.zshrc
```

**如果powerlevel10k字体缺失，可以到[powerlevel10k-media](https://gitee.com/qingmengfengyun/powerlevel10k-media)下载.ttf结尾的四个文件，新建"~/.local/share/fonts/ttf/MesloLGS NF"目录，并将下载的文件存放到这个目录下即可**

### 2.neovim

- [neovim官方github地址](https://github.com/neovim/neovim)

#### 安装方法

```bash
# 1.安装依赖：官方预编译包需要一些基础库才能运行
sudo dnf install compat-lua-libs libtermkey libtree-sitter libvterm luajit luajit2.1-luv msgpack unibilium xsel

# 2. 根据系统版本(如x86、arm等)下载对应neovim安装包，解压后存放在"/opt/nvim-linux64"下面，并且在`~/.local/bin`中创建软连接
ln -s /opt/nvim-linux64/bin/nvim ~/.local/bin/
```

### 3.gnome-yaru主题安装

- 前提准备

```bash
# 安装核心依赖包
sudo dnf install gtk-murrine-engine sassc gnome-themes-extra

# 安装GNOME优化工具(GNOME Tweaks)
sudo dnf install gnome-tweaks

# 安装User Themes扩展
sudo dnf install gnome-shell-extension-user-theme
```

- 安装Yaru主题

```bash
# 选项一：安装基础Yaru主题包(此包会自动安装GTK和Shell主题)
sudo dnf install gnome-shell-theme-yaru

# 选项二：安装整套Ubuntu风格(图标主题、光标主题、声音主题，以及GTK2/3/4的完整支持)
sudo dnf install yaru-icon-theme yaru-cursor-theme yaru-sound-theme yaru-gtk2-theme yaru-gtk3-theme yaru-gtk4-theme
```

### 4.gnome桌面优化

- 所需程序：gnome-tweaks、gnome-shell-extensions

#### 必备插件

- `Dash to Dock`  
快速启动应用程序，更快地在 windows 和桌面之间切换
- `NetSpeed`  
显示网速、内存、电池用量等等
- `User Themes`  
加载主题插件
- `AppIndicator and KStatusNotifierItem Support`  
状态栏插件
- `Blur my Shell`  
背景透明插件
- `Hide Top Bar`  
隐藏状态栏插件

## 三、附录

### 1.ssh服务启用方式

```bash
# 安装SSH服务
sudo dnf install openssh-server

# 启动SSH服务
sudo systemctl start sshd

# 使SSH服务在系统启动时自动运行
sudo systemctl enable sshd

# 检查SSH服务状态
sudo systemctl status sshd
```

### 2.vmtools开机挂载命令

- 虚拟机开机自动挂载主机路径
- 需要使用crontab工具

```bash
sudo crontab -e
# 输入以下命令
@reboot mount -t fuse.vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other
```

### 3.添加ssh密钥

- 生成ssh密钥

```bash
ssh-keygen -t rsa -b 4096
```

- 如果需要指定密钥名(如“gitkey”)，则需要在config中添加如下内容

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

### 4.fish shell配置

- fish shell安装和优化配置流程

```bash
# 安装fish shell
sudo dnf install fish

# 安装oh-my-fish
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish

# 关联配置文件
cd ~/Documents/github/dotfiles && stow -t ~ fish
```

- **[oh-my-fish官方github地址](https://github.com/oh-my-fish/oh-my-fish)**
