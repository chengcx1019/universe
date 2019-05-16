AjJbLZnU3sjL

26050

## 构建ubuntu server

创建用户`add user`

添加sudo权限`sudo  vi /etc/sudoers`

load dotfiles:

install pyenv,[pyenv-virtualenvs](https://github.com/pyenv/pyenv-virtualenv)

install oh my zsh:`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"` 

[install docker in ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

```
docker run -d -p 9101:9101 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 9101 -k ccx1993wo -m aes-256-cfb
```

[开启ss服务](https://github.com/oddrationale/docker-shadowsocks)

BBR加速：

1. [升级内核](https://www.howtoing.com/how-to-upgrade-linux-kernel-in-ubuntu-1604-server)
   - 查看内核列表`sudo dpkg --get-selections |grep linux-image`
   - 查看当前使用的内核：`uname -r`
   - 下载对应系统的内核安装包并安装
     - https://kernel.ubuntu.com/~kernel-ppa/mainline/v4.20/
     - `sudo dpkg -i *.deb`
     - ` sudo update-grub`
     - `sudo reboot`
   - 设置bbr