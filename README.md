# This is my handbook

## ssh
```shell
nohup ssh -qTfnN -D 127.0.0.1:38080 root@1.1.1.1 "vmstat 10" 2>&1 >/dev/null &
# add  k8s.gcr.io 127.0,0.1 to /etc/hosts
sudo ssh -L 443:108.177.125.82:443 root@1.1.1.1 //Local 443，forword remote 108.177.125.82:443
```

## Firecracker

### Downloads

- [Getting Started with Firecracker](https://github.com/firecracker-microvm/firecracker/blob/main/docs/getting-started.md)
- [firecracker](https://github.com/firecracker-microvm/firecracker/releases)
- [kernel](https://s3.amazonaws.com/spec.ccfc.min/img/quickstart_guide/x86_64/kernels/vmlinux.bin)
- [rootfs](https://s3.amazonaws.com/spec.ccfc.min/img/quickstart_guide/x86_64/rootfs/bionic.rootfs.ext4)

### Start firecracker

```shell
./firecracker --api-sock /tmp/firecracker.socket
```

### Set the guest kernel 

```shell
 curl --unix-socket /tmp/firecracker.socket -i \
      -X PUT 'http://localhost/boot-source'   \
      -H 'Accept: application/json'           \
      -H 'Content-Type: application/json'     \
      -d "{
            \"kernel_image_path\": \"vmlinux.bin\",
            \"boot_args\": \"console=ttyS0 reboot=k panic=1 pci=off\"
       }"

```
### Set the guest rootfs

```shell
curl --unix-socket /tmp/firecracker.socket -i \                                                                                                           
  -X PUT 'http://localhost/drives/rootfs' \
  -H 'Accept: application/json'           \
  -H 'Content-Type: application/json'     \
  -d "{
        \"drive_id\": \"rootfs\",   
        \"path_on_host\": \"bionic.rootfs.ext4\",
        \"is_root_device\": true,
        \"is_read_only\": false
   }"

```

### Start the guest machine

```shell
curl --unix-socket /tmp/firecracker.socket -i \                                                                                                           
  -X PUT 'http://localhost/actions'       \
  -H  'Accept: application/json'          \
  -H  'Content-Type: application/json'    \
  -d '{
      "action_type": "InstanceStart"
   }'
```

## superset

```shell
git clone https://github.com/apache/superset.git
cd superset
docker-compose -f docker-compose-non-dev.yml up
open http://localhost:8088
# username: admin password: admin
```

## Git

```shell
git clone git@github.com:zhongwei/english
```

## Vim
- [f, F, t, T]{char} 
- ;  
- ,
- \>G
- s
- q 
- CR => <ESC>o
- [a,i][w,W,s,S,p,P,],daw
- 10Ctl-a
- h: operator
- dd,>>,gUgU,gUU double operator 
- >,<,=
- ctl-[h,w,u]
- h: i_CTRl-o
 
## kubectl
``` shell
kubectl api-resources
kubectl explain ingresses
kubectl cordon mynode
kubectl drain mynode
kubectl version
kubectl cluster-info
kubectl apply -f deploy.yaml
kubectl delete -f deploy.yaml
kubectl create deployment my-dep --image=busybox 
kubectl expose rc nginx --port=80 --target-port=8000  
kubectl scale --replicas=3 -f foo.yaml
kubectl annotate pods foo description='my frontend'
kubectl label --overwrite pods foo status=unhealthy 
kubectl delete pod,service baz foo 
kubectl delete pods,services -l name=myLabel
kubectl delete pod foo --grace-period=0 --force
kubectl get services                          
kubectl get pods --all-namespaces             
kubectl get pods -o wide                      
kubectl get deployment my-dep                
kubectl get deployment my-dep --watch         
kubectl get pod my-pod -o yaml               
kubectl get pod my-pod -l app=nginx     
kubectl describe nodes my-node   
kubectl describe pods my-pod   
kubectl logs my-pod             
kubectl exec -t -i nginx-11f5d695dd-czm6t bash
kubectl cp /tmp/foo_dir myubuntu:/tmp/bar_dir  
```

## Start ubuntu dev container

``` shell
docker container run --name dimage --hostname Dev -itd zhongwei/dimage 
```

## Operating System Development

```shell
sudo apt install qemu qemu-kvm nasm
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

#### CentOS

```shell
cat /etc/centos-release /etc/centos-release-upstream
vi /etc/sshd/sshd_config
# ClientAliveInterval 60
# ClientAliveCountMax 3

# Chinese dir name display
sudo yum install langpacks-zh_CN.noarch
vim /etc/locale.conf
# LANG=“zh_CN.UTF-8”
source /etc/locale.conf
vi ./.zshrc
# export LC_ALL="zh_CN.UTF-8"
# export LANG="zh_CN.UTF-8"
$source ./.zshrc
```

#### 生成证书

```shell
sudo ssh-keygen -A #sshd 
```

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

- 修改主机名(Ubuntu)

```shell
sudo vi /etc/cloud/cloud.cfg # preserve_hostname  value change to  true
sudo hostnamectl set-hostname zhongwei-ubuntu
```

- 创建sudo用户

```shell
sudo adduser zhongwei
sudo usermod -aG sudo zhongwei
usermod -aG wheel zhongwei #CentOS
```

- 安装常用工具

```shell
sudo apt install git sl nnn mc -y
wget https://github.com/wagoodman/dive/releases/download/v0.10.0/dive_0.10.0_linux_amd64.deb
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

- 安装oh-my-zsh

```shell
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo apt install fonts-powerline
sudo dnf install powerline-fonts # CentOS
echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"
vi ~/.zshrc #ZSH_THEME="agnoster"
git clone https://github.com/ryanoasis/nerd-fonts
cd nerd-fonts && ./install.sh
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
vi ~/.zshrc #ZSH_THEME="powerlevel10k/powerlevel10k"
```

    [nerd-fonts cheat-sheet](http://nerdfonts.com/#cheat-sheet)
    iTerm -> Preferences -> Profiles -> Text -> Change Font -> Non-ASCII Font -> HeavyData Nerd Font
    get_icon_names

- 自动安装docker

```shell
#Ubuntu
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

#CentOS 8
sudo dnf config-manager --add-repo https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
sudo dnf update
sudo dnf install -y https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/8/x86_64/stable/Packages/containerd.io-1.4.4-3.1.el8.x86_64.rpm
sudo dnf install docker-ce -y
awk -F: '/docker/ {print $1}' /etc/group

#mirror config
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
   "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/"]
}
EOF

#restart and check log
sudo systemctl daemon-reload
sudo systemctl restart docker
journalctl -fu docker

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

- 安装Ubuntu桌面

```shell
sudo apt install -y ubuntu-desktop
sudo apt install -y xrdp
sudo systemctl set-default multi-user.target
sudo systemctl set-default graphical.target
```

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
sudo dnf module install nodejs:14 #CentOS
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
sudo dnf install python38 python38-devel -y #CentOS 8.x
# $HOME/.config/pip/pip.conf
[global]
index-url = https://mirrors.ustc.edu.cn/pypi/web/simple
format = columns
sudo apt install python3-pip -y
```

## Tensorflow

### Install

```shell
pip3 install --user tensorflow
```

## Pytorch

### Install

```shell
pip3 install torch==1.3.0+cpu torchvision==0.4.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
```

## tensorflow-notebook

### Install
```shell
docker run --name tensorflow-notebook -v "${PWD}":/home/jovyan/work -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -d jupyter/tensorflow-notebook
docker exec -it tensorflow-notebook sh
jupyter notebook list
```shell

## JupyterLab

### Install

```shell
pip3 install --user jupyterlab
vi ~/.zprofile # 添加~/.local/bin到PATH变量
jupyter lab --ip=10.105.201.248
```

## Redis

```shell
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm -y
dnf module list | grep redis
sudo dnf module install redis:remi-6.2 -y
sudo systemctl enable redis.service 
sudo systemctl start redis.service 
vi /etc/redis.conf 
```

## MySQL

```shell
wget https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm
sudo yum install mysql80-community-release-el8-1.noarch.rpm
yum repolist enabled | grep "mysql.*-community.*"
sudo yum-config-manager --enable mysql80-community # sudo dnf config-manager --enable mysql80-community
yum repolist all | grep mysql
sudo yum module disable mysql
cat /etc/yum.repos.d/mysql-community.repo 
sudo yum install mysql-community-server
systemctl start mysqld
systemctl status mysqld
sudo grep 'temporary password' /var/log/mysqld.log
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '@Test123';
show variables like 'validate_password%';
set global validate_password.policy=0;
mysql --help|grep my.cnf
vi /etc/my.cnf 
####################################
#validate_password.check_user_name=OFF
#validate_password.length=4
#validate_password.mixed_case_count=0
#validate_password.number_count=0
#validate_password.policy=0
#validate_password.special_char_count=0
#######################################
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
CREATE USER 'zhongwei'@'localhost' IDENTIFIED BY 'zhongwei';
```

## Clickhouse

```shell
sudo yum install yum-utils
sudo rpm --import https://repo.clickhouse.tech/CLICKHOUSE-KEY.GPG
sudo yum-config-manager --add-repo https://repo.clickhouse.tech/rpm/clickhouse.repo
sudo yum install clickhouse-server clickhouse-client

systemctl start clickhouse-server # sudo /etc/init.d/clickhouse-server start
systemctl status clickhouse-server
clickhouse-client
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
docker container run --name pgadmin4 --restart always -p 8054:80  -e PGADMIN_DEFAULT_EMAIL=zhongwei99@163.com -e PGADMIN_DEFAULT_PASSWORD=zhongwei -d dpage/pgadmin4:5
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
## Create redmine container
```shell
docker network create rednet
docker run -d --name mysql -p 3306:3306 --network rednet -e MYSQL_USER=redmine -e MYSQL_PASSWORD=redmine -e MYSQL_DATABASE=redmine -e MYSQL_ROOT_PASSWORD=root mysql:5.7  --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
docker run -d --name redmine -p 3000:3000 --network rednet -e REDMINE_DB_MYSQL=mysql -e REDMINE_DB_USERNAME=redmine -e REDMINE_DB_PASSWORD=redmine redmine
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

## Clickhouse

```shell
docker container run --name clickhouse --volume=$HOME/clickhouse:/var/lib/clickhouse --ulimit nofile=262144:262144 -p 8123:8123 -p 9000:9000 -p 9009:9009 -d yandex/clickhouse-server
docker exec -it clickhouse sh
clickhouse-client
show databases
#/etc/clickhouse-server/config.xml #<listen_host>::</listen_host>
curl https://datasets.clickhouse.tech/hits/tsv/hits_v1.tsv.xz | unxz --threads=`nproc` > hits_v1.tsv
curl https://datasets.clickhouse.tech/visits/tsv/visits_v1.tsv.xz | unxz --threads=`nproc` > visits_v1.tsv
```
