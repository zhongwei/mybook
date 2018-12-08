# This is my handbook

## Linux book

### Linux Admin

#### Network Admin

- 两块网卡时，修改路由表优先级

```shell
    sudo ip route del default via 192.168.99.1 dev enp0s8
    sudo ip route add default via 192.168.99.1 dev enp0s8 proto static metric 101
```

```yaml
#/etc/netplan/50-cloud-init.yaml
network:
    ethernets:
        enp0s3:
            addresses: []
            dhcp4: true
        enp0s8:
            addresses:
            - 192.168.99.100/24
            dhcp4: false
            nameservers:
                addresses: []
                search: []
            routes:
              - to: 0.0.0.0/0
                via: 192.168.99.1
                metric: 101
    version: 2
#sudo netplan apply
```

#### Linux basic config

- 修改主机名

```shell
sudo vi /etc/cloud/cloud.cfg # preserve_hostname  value change to  true
sudo hostnamectl set-hostname zhongwei-ubuntu
```

- 创建sudo用户

```shell
sudo adduser zhongwei
sudo usermod -aG sudo zhongwei
```

- 安装常用工具

```shell
sudo apt install python3-pip -y
sudo apt install sl
wget https://github.com/sharkdp/bat/releases/download/v0.9.0/bat_0.9.0_amd64.deb
sudo dpkg -i bat_0.9.0_amd64.deb
wget https://github.com/wagoodman/dive/releases/download/v0.4.0/dive_0.4.0_linux_amd64.deb
sudo dpkg -i dive_0.4.0_linux_amd64.deb
sudo apt install nnn
sudo apt install mc -y
wget https://github.com/jesseduffield/lazygit/releases/download/v0.5/lazygit_0.5_Linux_x86_64.tar.gz

```

- 安装docker

```shell
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/containerd.io_1.2.0-1_amd64.deb
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce_18.09.0~3-0~ubuntu-bionic_amd64.deb
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce-cli_18.09.0~3-0~ubuntu-bionic_amd64.deb
sudo dpkg -i containerd.io_1.2.0-1_amd64.deb
sudo apt install -y libltdl7
sudo dpkg -i docker-ce-cli_18.09.0~3-0~ubuntu-bionic_amd64.deb
sudo dpkg -i docker-ce_18.09.0~3-0~ubuntu-bionic_amd64.deb
sudo usermod -aG docker zhongwei
exit #退出用户，重新登录权限生效
```

- 安装oh-my-zsh

```shell
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
sudo apt install fonts-powerline
echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"
vi ~/.zshrc #ZSH_THEME="agnoster"
git clone https://github.com/ryanoasis/nerd-fonts
cd nerd-fonts && ./install.sh
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
vi ~/.zshrc #ZSH_THEME="powerlevel9k/powerlevel9k"
```

    [nerd-fonts cheat-sheet](http://nerdfonts.com/#cheat-sheet)
    iTerm -> Preferences -> Profiles -> Text -> Change Font -> Non-ASCII Font -> HeavyData Nerd Font
    get_icon_names

- 配置powerline

    [Show-Off-Your-Config](http://github.com/bhilburn/powerlevel9k/wiki/Show-Off-Your-Config)
    color: https://jonasjacek.github.io/colors/

    POWERLEVEL9K_CUSTOM_PYTHON="echo -n '\ue235' Python"
    POWERLEVEL9K_CUSTOM_PYTHON_FOREGROUND="black"
    POWERLEVEL9K_CUSTOM_PYTHON_BACKGROUND="blue"

    POWERLEVEL9K_CUSTOM_JAVASCRIPT="echo -n '\ue781' JavaScript"
    POWERLEVEL9K_CUSTOM_JAVASCRIPT_FOREGROUND="black"
    POWERLEVEL9K_CUSTOM_JAVASCRIPT_BACKGROUND="yellow"

    POWERLEVEL9K_CUSTOM_RUBY="echo -n '\ue21e' Ruby"
    POWERLEVEL9K_CUSTOM_RUBY_FOREGROUND="black"
    POWERLEVEL9K_CUSTOM_RUBY_BACKGROUND="red"

    POWERLEVEL9K_CUSTOM_RUST="echo -n '\ue7a8' Rust"
    POWERLEVEL9K_CUSTOM_RUST_FOREGROUND="black"
    POWERLEVEL9K_CUSTOM_RUST_BACKGROUND="magenta"

    POWERLEVEL9K_CUSTOM_GO="echo -n '\ue724' Go"
    POWERLEVEL9K_CUSTOM_GO_FOREGROUND="Links"
    POWERLEVEL9K_CUSTOM_GO_BACKGROUND="blue"
    POWERLEVEL9K_VCS_GIT_GITHUB_ICON="\uf09b"

    POWERLEVEL9K_HOME_ICON="\uf015"

    POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(context dir vcs custom_rust custom_go custom_python custom_javascript custom_ruby)

- 配置 zsh-syntax-highlighting

  Oh-my-zsh

1. Clone this repository in oh-my-zsh's plugins directory:
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
2. Activate the plugin in `~/.zshrc`:
    plugins=( [plugins...] zsh-syntax-highlighting)
3. Source `~/.zshrc`  to take changes into account:
    source ~/.zshrc

- Install Nodejs

```shell
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

- Install Rust

```shell
curl https://sh.rustup.rs -sSf | sh
```

#### Emacs config

```shell
sudo apt install -y emacs
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
git clone https://github.com/zhongwei/.spacemacs.d ~/.spacemacs.d
```

## Tensorflow

### Install

```shell
sudo apt install python3-pip -y
pip3 install tensorflow
```

## JupyterLab

### Install

```shell
pip3 install jupyterlab
vi .zprofile # 添加./local/bin到PATH变量
```