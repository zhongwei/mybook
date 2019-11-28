# Linux programming

## Console shortcut

![Shortcut](./images/moving_cli.png)

## File type

-, d, l, b, c, s, p

## Command

```shell

ln -s hello.c hello.soft
wc hello.c # line, words, bytes, filename
od -tc hello.o # c, d, f, o, u, x
chmod a=x hello
chmod a-x hello
chmod a+x hello
find ~ -size -10k
find ~ -size +10M
find ~ -size +10M -size -100M
find ~ -type p # -type d/f/b/c/s/p/l
mkfifo test
grep -r "stdio.h" ~
sudo apt-get clean # /var/cache/apt/archives
sudo dpkg -r sublime-text
sudo fdisk -l
sudo mount /dev/sdb1 /mnt # sd: SCSI Device hd: Hard Disk sda5: main partition 1-4, extend 5-
tar xvfz abc.tar.gz -C /tmp
# Ctrl + ALT + F1-F7
# Ctrl + Shit + t : new tab
ps aux # a:all u:user x:terminal
kill -l
ping www.yahoo.com -c 4
nslookup www.baidu.com
sudo adduser zhangsan
sudo deluser zhangsan
sudo groupadd dev
sudo useradd -s /bin/bash -g dev -d /home/lisi -m lisi
sudo passwd lisi
sudo userdel -r lisi
sudo apt install vsftpd # /etc/vsftpd
service vsftpd restart
sudo apt install lftp
sudo apt install nfs-kernel-server # /etc/exports /home/zhangsan/share *(rw, sync)
sudo service nfs-kernel-server restart
sudo mount 192.168.100.9:/home/zhangsan/share /mnt
sudo apt install openssh-server # scp: super copy
man man
alias ls
alias pse='ps -ef'
vi hello.c # /etc/vim/vimrc ~/.vim/vimrc
# 500G S s d0 D ctrl+r v->y ? # >> 3K I :%s/zhang/li/g :3,5s/zhang/li/g
# qall ZZ :sp ctrl+ww :vsp stdio.h
gcc hello.c # -ESc .c-.i-.s-.o cpp gcc as ld
ar rcs libcalc.a *.o
gcc hello.c -I./include -D DEBUG -O3 -Wall -g
ar rcs libcalc.a *.o
gcc main.c lib/libcalc.a -Iinclude
gcc main.c -L lib -l calc -Iinclude
nm libcalc.a
nm a.out
gcc -fPIC -c *.c
gcc -shared -o libcalc.so -I../include *.o
gcc main.c lib/libcalc.so -Iinclude
ldd a.out
echo $LD_LIBRARY_PATH
sudo vi /etc/ld.so.conf
sudo ldconfig -v
apt install gdb
gdb app 
# list, l hello.c:98, l hello.c:test, b 12, b 15 if i == 15
# break info, start, next, continue, step, print i, ptype max, display i, i d, undisplay 1
# u, finish, set var i = 10, run, delete
make # makefile Makefile

```

```makefile
# CC CPPFLAGS CFLAGS  

src=$(wildcard ./*.c)
obj=$(patsubst ./%.c, ./%o, $(src))
target=app

$(target):$(obj)
    $(CC) $^ -o $(target)

%.o:%.c
    $(CC) -c $< -o $@

.PHONY:clean
clean:
    -mkdir /test
    rm $(obj) $(target) -f

hello:
    echo "hello, world"

```

```shell
file app

```

