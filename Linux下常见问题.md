# `MySQL`安装与配置
### 安装 `mysql`
```bash
$ sudo apt-get install mysql-server-5.7
```
### 关于安装完成后不能登陆的解决方法：
1. 修改 `MySQL` 配置文件 `vim /etc/mysql/mysql.conf.d/mysqld.cnf`
2. 在 配置文件 `[mysqld]` 末尾追加 `skip-grant-tables` 跳过登陆验证 
3. 重启服务 `service mysql restart`
4. 进入数据库,使用 `mysql` 表
5. 执行语句更新用户密码 `update user set authentication_string=password("你的密码"),plugin='mysql_native_password' where user='root';`
6. 执行语句刷新权限 `flush privileges;`
7. 退出数据库，并且重新修改 `MySQL` 配置文件删除 `skip-grant-tables` 后重启服务

### 运行 `SQL` 文件
```
source sql文件路径
```

```
外部主机不允许连接Mysql设置的解决方法
Mysql:is not allowed to connect to this MySQL server
如果你想连接你的mysql的时候发生这个错误：

ERROR 1130: Host '192.168.1.3' is not allowed to connect to this MySQL server

解决方法：

改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"
mysql -u root -pvmwaremysql>use mysql;mysql>update user set host = '%' where user = 'root';mysql>select host, user from user;
授权法。例如，你想myuser使用mypassword从任何主机连接到mysql服务器的话。
GRANT ALL PRIVILEGES ON . TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码
GRANT ALL PRIVILEGES ON . TO 'root'@'192.168.1.3' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON . TO 'root'@'10.10.40.54' IDENTIFIED BY '123456' WITH GRANT OPTION
```

# 没有`ipconfig`命令
### 安装 `net-tools`
```
sudo apt-get install net-tools
```

# 时间同步
```
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```

# 音质提升
```
sudo apt install jackd pulseaudio-module-jack caps
```

# `vim`基本配置
```
# 编辑 ~/.vimrc文件

set ts=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
```

# 搜狗拼音
```
# pacman -S base-devel

sudo pacman -S fcitx-sogoupinyin fcitx-configtool fcitx-im

sudo pacman -U https://arch-archive.tuna.tsinghua.edu.cn/2019/04-29/community/os/x86_64/fcitx-qt4-4.2.9.6-1-x86_64.pkg.tar.xz

vim  ~/.xprofile

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

qtconfig-qt4
```



# ubuntu 命令行与图形化模式切换



一、开机默认进入命令行模式

1、输入命令：sudo systemctl set-default multi-user.target 

2、重启：sudo reboot

要进入图形界面，只需要输入命令startx

从图形界面切换回命令行：ctrl+alt+F7

二、开机默认进入图形用户界面

1、输入命令：sudo systemctl set-default graphical.target 

2、重启：reboot

要进入命令行模式：ctrl+alt+F2

从命令行切换到图形界面：ctrl+alt+F7

