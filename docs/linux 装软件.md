# MYSQL

链接：https://www.jianshu.com/p/cc01f7773346

## 装 mysql

### 1）首先检查系统中是否已经安装了MySQL 

在终端里面输入

>    sudo netstat -tap | grep mysql

若没有反映，没有显示已安装结果，则没有安装

如若已安装，可以选择删除。（删除方法放在下面）

### 2）如果没有安装，则安装MySQL

在终端输入 

>   sudo apt-get install mysql-server mysql-client

在此安装过程中会让你输入root用户(管理MySQL数据库用户，非Linux系统用户)密码，按照要求输入即可。

### 3）测试安装是否成功：

在终端输入 

>   sudo netstat -tap | grep mysql

### 4）也可通过登录MySQL测试

在终端输入

>   mysql -u root -p

接下来会提示你输入密码，输入正确密码，即可进入

## 修改远程访问mysql

https://www.jianshu.com/p/8fc90e518e2c

>   # 踩坑笔记
>
>   估计搞了一个多小时才把这个远程连接搞好。一台本地电脑，一台云服务器，都是linux系统。
>
>   ## 步骤
>
>   1.  在服务器端开启远程访问
>
>       首先进入mysql数据库，然后输入下面两个命令：
>
>       ```
>       grant all privileges on *.* to 'root'@'%' identified by 'password';
>       ```
>
>       ```
>       flush privileges;
>       ```
>
>       -   第一个*是数据库，可以改成允许访问的数据库名称
>
>   -   第二个* 是数据库的表名称，*代表允许访问任意的表
>   -   root代表远程登录使用的用户名，可以自定义
>   -   %代表允许任意ip登录，如果你想指定特定的IP，可以把%替换掉就可以了
>   -   password代表远程登录时使用的密码，可以自定义
>   -   `flush privileges;`这是让权限立即生效
>
>   1.  修改my.cnf配置文件
>        这个是mysql的配置文件，如果你无标题文章找不到在哪里的话，可以输入`find /* -name my.cnf` 找到
>        通过vim编辑该文件，找到`bind-address = 127.0.0.1`这一句，然后在前面加个#号注释掉，保存退出
>   2.  重启服务
>        `sudo service mysql restart`
>   3.  在本地远程连接
>        在终端输入：
>        `mysql -h 服务器ip地址 -P 3306 -u root -p`
>        然后输入密码即可。
>        root是第1点设置的用户名，密码也是第1点设置的密码
>
>   ## 一些细节
>
>   在网上找了很多文章，说要开启3306端口才能连接，但是我开启了却还是无法连接，后来又找到了一些文章，说要更改my.cnf，也就是上面的第2点，更改了然后重启服务器就可以了。
>
>   刚刚在另外一台服务器上面试了一下，没有配置过端口，通过上面三步，很快就连上了。
>
>   所以第二点非常重要，基本上每个人装mysql的时候都会去配置那个文件，因为字符集需要配置。所以肯定有那个文件的，用find命令找找就行了。

## 删除数据库 并清除操作

首先删除mysql:

>   sudo apt-get remove mysql-*

然后清理残留的数据

>   dpkg-l|grep ^rc|awk'{print $2}'|sudo xargs dpkg -P

它会跳出一个对话框，你选择yes就好了

然后安装mysql

装完 mysql 会出现一个问题，就是不能用 c 语言的头文件 

```c++
#include <mysql/mysql.h>
```

这时候需要装 libmysqlclient-dev

>   sudo apt-get install libmysqlclient-dev

安装完在 `usr/include/mysql` 文件夹下有 `mysql.h` 文件，可以用来写连接 mysql 数据库，在编译的时候需要加上 `-lmysqlclient -L /usr/include/mysql` 参数，在 cmake 中如何搞定呢？

