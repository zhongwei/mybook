# This is my handbook

## Operating System Development

```shell
sudo apt install qemu nasm
```

## Ethereum

### geth install

```shell
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
sudo apt install ethereum  
```

### Init node

```json
// genesis.json
{
 "config": {
   "chainID": 1234,
   "homesteadBlock": 0,
   "eip150Block": 0,
   "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
   "eip155Block": 0,
   "eip158Block": 0
 },
 "alloc": {},
 "difficulty": "0x4000",
 "gasLimit": "0xffffffff",
 "nonce": "0x0000000000000000",
 "coinbase": "0x0000000000000000000000000000000000000000",
 "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
 "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
 "extraData": "0x123458db4e347b1234537c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa",
 "timestamp": "0x00"
}
```

```shell
mkdir myethernet && cd myethernet
geth --datadir node1 init genesis.json
geth --datadir node1 --networkid 1234 --port 30301 --nodiscover # console
geth --datadir node2 init genesis.json
geth --datadir node2 --networkid 1234 --port 30302 --nodiscover
geth attach ipc:node1/geth.ipc
# > eth.accounts
# > personal.newAccount("111")
# > personal.newAccount("222")
# > eth.coinbase
# > miner.setEtherbase(eth.accounts[1])
# > eth.getBalance(eth.accounts[0])
# > miner.start()
## Generating DAG in progress 环节要运行半天，两个epoch都要跑到percentage=100
# > miner.stop()
# > web3.fromWei(eth.getBalance(eth.accounts[1]), 'ether')
# > admin.peers
# > admin.nodeInfo
# > admin.addPeer("enode://e65da810c663cf72447d1231989fdd0201d871d4c37fdbe3073ac9278a85f430c8844da02cbd296458a82779dd065aa58f60e5de1340f613c84c4712300c6eac@127.0.0.1:30302?discport=0")
# > admin.peers
# > eth.blockNumber
# > personal.unlockAccount(eth.accounts[0])
# > eth.sendTransaction({from: eth.account[0], to: "0x8f1a13e6edd6a48bcca7d1d610c1a5821449c2f1", value: web3.toWei(15, 'ether')})
# > txpool.status
# > miner.start()
# > miner.stop()
# > web3.fromWei(eth.getBalance(eth.accounts[0]), 'ether')
```

## Linux book

### Linux Admin

#### 生成证书

```shell
openssl req -x509 -nodes -newkey rsa:1024 -keyout private.key -out server.crt -days 365 -subj "/C=CN/ST=Shanghai/L=Shanghai/O=Global lib
erty/OU=IT Department/CN=*"
```

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
wget https://github.com/wagoodman/dive/releases/download/v0.8.1/dive_0.8.1_linux_amd64.deb
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


- 自动安装docker

```shell
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker zhongwei
exit #退出用户，重新登录权限生效
```

- 手动下载安装docker

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
- 安装docker-compose

```shell
sudo curl -L https://github.com/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

- 安装oh-my-zsh

```shell
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
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
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
npm config set registry https://registry.npm.taobao.org
```

- Install dapp dev env
```shell
sudo npm install -g truffle --unsafe-perm
```

- Install Rust

```shell
# .zshrc
export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static
export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup
. ./.zshrc

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# $HOME/.cargo/config
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"

# registry = "https://mirrors.ustc.edu.cn/crates.io-index"

# cargo/bin path activate
. ./.profile

apt install clang-8 build-essential -y

cargo install bat lsd exa fd-find xsv tokei ripgrep genact hyperfine
```

- Install Go

```shell
sudo tar -C /usr/local -xzf go1.11.4.linux-amd64.tar.gz
vi ~/.zshrc
export PATH="/usr/local/go/bin:$HOME/.cargo/bin:$PATH:$HOME/.local/bin"
export GOPROXY="https://goproxy.cn/"
export GO111MODULE="on"
source ~/.zshrc
go env
```

#### Emacs config

```shell
sudo apt install -y emacs
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
git clone https://github.com/zhongwei/.spacemacs.d ~/.spacemacs.d
```
## Python

```shell
# $HOME/.config/pip/pip.conf
[global]
index-url = https://mirrors.ustc.edu.cn/pypi/web/simple
format = columns
sudo apt install python3-pip -y
```

## Tensorflow

### Install

```shell
pip3 install tensorflow
```

## Pytorch

### Install

```shell
pip3 install torch==1.3.0+cpu torchvision==0.4.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
```

## JupyterLab

### Install

```shell
pip3 install jupyterlab
vi ~/.zprofile # 添加./local/bin到PATH变量
jupyter lab --ip=10.105.201.248
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
docker container run --name mysql  --restart always -p 3306:3306 -v ~/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=zhongwei -e MYSQL_PASSWORD=zhongwei -e MYSQL_DATABASE=demo  -d mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
docker container run --name phpmyadmin --restart always -p 8033:80 -e PMA_ARBITRARY=1 -d phpmyadmin/phpmyadmin
```

- Create postgres container and web manage tools
```shell
docker container run --name postgres --restart always -p 5432:5432 -v ~/data/postgres:/var/lib/postgresql/data  -e POSTGRES_PASSWORD=zhongwei -d postgres:alpine
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
docker container run --name gitlab -p 8989:80 -p 2289:22 -v ~/data/gitlab/config:/etc/gitlab -v ~/data/gitlab/logs:/var/log/gitlab -v ~/data/gitlab/data:/var/opt/gitlab -d gitlab/gitlab-ce
```

- Create sonic container

```shell
docker container run --name sonic -p 1491:1491 -v ~/data/sonic/config.cfg:/etc/sonic.cfg -v ~/data/sonic/store/:/var/lib/sonic/store/ -d valeriansaliou/sonic:v1.2.0
```

## Create ElasticSearch container

```shell
docker container run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch
```

## Create Kafka container

```shell
docker network create k-net --driver bridge
docker container run --name zookeeper --network k-net -e ALLOW_ANONYMOUS_LOGIN=yes -d bitnami/zookeeper
docker container run --name kafka --network k-net -e ALLOW_PLAINTEXT_LISTENER=yes -e KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181 -d bitnami/kafka
docker container run -it --rm --network k-net -e KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181 bitnami/kafka kafka-topics.sh --list  --zookeeper zookeeper:2181
```

## Create NSQ container

```shell
docker-compose up -d
```

### docker-compose.yml

```yaml
version: '3'
services:
  nsqlookupd:
    image: nsqio/nsq
    command: /nsqlookupd
    ports:
      - "4160:4160"
      - "4161:4161"
  nsqd:
    image: nsqio/nsq
    command: /nsqd --lookupd-tcp-address=nsqlookupd:4160
    depends_on:
      - nsqlookupd
    ports:
      - "4150:4150"
      - "4151:4151"
  nsqadmin:
    image: nsqio/nsq
    command: /nsqadmin --lookupd-http-address=nsqlookupd:4161
    depends_on:
      - nsqlookupd  
    ports:
      - "4171:4171"

```
