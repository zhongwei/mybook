# This is my handbook

## Linux book

### Linux Admin

#### 离线安装Git 2.x

``` shell
yum install -y yum-plugin-downloadonly
curl https://setup.ius.io | sh
mkdir ~/yum-repo
yum install --downloadonly --downloaddir=~/yum-repo/git2u git2u
yum search git
yum remove -y git | yum -y install git2u
```

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
sudo apt install git curl sl nnn mc -y
wget https://github.com/wagoodman/dive/releases/download/v0.7.2/dive_0.7.2_linux_amd64.deb
sudo dpkg -i dive_0.7.2_linux_amd64.deb
wget https://github.com/jesseduffield/lazygit/releases/download/v0.5/lazygit_0.5_Linux_x86_64.tar.gz
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
sudo sh -c "curl https://raw.githubusercontent.com/mrowa44/emojify/master/emojify -o /usr/local/bin/emojify && chmod +x /usr/local/bin/emojify"
emojify "Hey, I just :raising_hand: you, and this is :scream: , but here's my :calling: , so :telephone_receiver: me, maybe?"
emojify "To :bee: , or not to :bee: : that is the question... To take :muscle: against a :ocean: of troubles, and by opposing, end them?"
wget https://github.com/yudai/gotty/releases/download/v2.0.0-alpha.3/gotty_2.0.0-alpha.3_linux_amd64.tar.gz
gotty -w docker run -it --rm busybox
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
vi ~/.zshrc #POWERLEVEL9K_MODE='nerdfont-complete' ZSH_THEME="powerlevel9k/powerlevel9k"
```

    [nerd-fonts cheat-sheet](http://nerdfonts.com/#cheat-sheet)
    iTerm -> Preferences -> Profiles -> Text -> Change Font -> Non-ASCII Font -> HeavyData Nerd Font
    get_icon_names

- 配置powerline

```shell
# [Show-Off-Your-Config](http://github.com/bhilburn/powerlevel9k/wiki/Show-Off-Your-Config)
#  color: https://jonasjacek.github.io/colors/

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
```

- 配置 zsh-syntax-highlighting and ZSH-AutoSuggestion

  Oh-my-zsh

1. Clone this repository in oh-my-zsh's plugins directory:

```shell
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
2. Activate the plugin in `~/.zshrc`:
    plugins=( [plugins...] zsh-syntax-highlighting zsh-autosuggestions)
3. Source `~/.zshrc`  to take changes into account:

```shell
    source ~/.zshrc
```

- Use Ligature Support(FiraCode Font) 

```shell
git clone https://github.com/tonsky/FiraCode
# Navigate to ITerm2 | Preferences | Profiles | Text => change font to Fira Code Regular
```

- Install Nodejs

```shell
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

- Install Rust

```shell
curl https://sh.rustup.rs -sSf | sh
cargo install bat lsd exa fd xsv tokei ripgrep genact hyperfine
```

- Install Go

```shell
sudo tar -C /usr/local -xzf go1.11.4.linux-amd64.tar.gz
vi ~/.zprofile
export PATH="/usr/local/go/bin:$HOME/.cargo/bin:$PATH:$HOME/.local/bin"
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
vi ~/.zprofile # 添加./local/bin到PATH变量
```

## Docker book

- Create portainer container
```shell
docker container run --name portainer --restart always -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -d portainer/portainer
```

- Create redis container and web manage tools
```shell
docker container run --name redis --restart always -p 6379:6379 -v ~/data/redis:/data -d redis:alpine 
docker container run --name redis-commander --restart always -p 8063:8081 --env REDIS_HOSTS=10.105.201.248 -d rediscommander/redis-commander
```

- Create mongo container and web manage tools
```shell
docker container run --name mongo --restart always -p 27017:27017 -v ~/data/mongo:/data/db -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=zhongwei -d mongo
docker container run --name mongo-express --restart always -p 8081:8081 -e ME_CONFIG_MONGODB_SERVER=10.105.201.248 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=zhongwei -d mongo-express
```

- Create rabbitmq container 
```shell
docker container run --name rabbitmq --restart always --hostname rabbitmq -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=zhongwei -d rabbitmq:management-alpine
```

- Create registry container
```shell
docker container run --name registry --restart always -p 5000:5000 -d registry
```

- Create mysql container and web manage tools
```shell
docker container run --name mysql  --restart always --user "$(id -u):$(id -g)" -p 3306:3306 -v ~/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=zhongwei -e MYSQL_PASSWORD=zhongwei -e MYSQL_DATABASE=demo  -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
docker container run --name phpmyadmin --restart always -p 8033:80 -e PMA_ARBITRARY=1 -d phpmyadmin/phpmyadmin
```

- Create postgres container and web manage tools
```shell
docker container run --name postgres --restart always --user "$(id -u):$(id -g)" -p 5432:5432 -v ~/data/postgres:/var/lib/postgresql/data  -e POSTGRES_PASSWORD=zhongwei -d postgres
docker container run --name pgadmin4 --restart always -p 8054:80  -e PGADMIN_DEFAULT_EMAIL=zhongwei99@163.com -e PGADMIN_DEFAULT_PASSWORD=zhongwei -d dpage/pgadmin4
```

- Create nginx container
```shell
docker container run --name nginx -p 80:80 -v ~/data/nginx/conf.d:/etc/nginx/conf.d:ro -v ~/data/nginx/html:/usr/share/nginx/html -d nginx:alpine
docker exec nginx chmod 755 /usr/share/nginx/html
docker container restart nginx
```
- Create drone container
```shell
docker container run --name drone --restart always -p 3001:80 -p 8443:443 -v /var/run/docker.sock:/var/run/docker.sock -v ~/data/drone:/data -e DRONE_GITEA_SERVER=10.105.201.248:3000 -e DRONE_GIT_ALWAYS_AUTH=false -e DRONE_RUNNER_CAPACITY=2 -e DRONE_SERVER_HOST=http://10.105.201.248 -e DRONE_SERVER_PROTO=http -e DRONE_TLS_AUTOCERT=false -d drone/drone
```
- Create Jenkins container 
```shell
docker container run --name jenkins --restart always -p 8080:8080 -p 50000:50000 -v ~/data/jenkins:/var/jenkins_home  -d jenkins/jenkins 
```
- Create Gitlab container 
```shell
docker container rune --name gitlab -p 8929:80 -p 2289:22 -v /data/gitlab/config:/etc/gitlab -v /data/gitlab/logs:/var/log/gitlab -v /data/gitlab/data:/var/opt/gitlab -d gitlab/gitlab-ce
```
