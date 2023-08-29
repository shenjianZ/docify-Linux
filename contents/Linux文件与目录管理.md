[TOC]



### 2.1	目录与路径

###### 2.1.1	目录的有关操作

```shell
. 当前目录
.. 上一级目录
-  前一个工作目录
~  当前用户家目录
~username 指定用户的家目录
```

- cd：切换工作目录
- pwd：显示当前工作目录
- mkdir：创建新目录
- rmdir：删除空目录

ex：

```shell
[root@hadoop100 ~]# cd ~
[root@hadoop100 ~]# cd ~root
[root@hadoop100 ~]# cd ~iboom
[root@hadoop100 /home/iboom]# cd ../
[root@hadoop100 /home]# pwd
/home
[root@hadoop100 /home]# mkdir test && ls -ld test
drwxr-xr-x 2 root root 6 8月  18 16:54 test
[root@hadoop100 /home]# rmdir test && ls -ld test
ls: 无法访问test: 没有那个文件或目录
```

> pwd [ -P ] （Print Working Directory）

```shell
[root@hadoop100 /var/mail]# cd /var/mail && ls -ld /var/mail && pwd && pwd -P
lrwxrwxrwx. 1 root root 10 1月  23 2023 /var/mail -> spool/mail
/var/mail
/var/spool/mail
-P 参数显示文件真正的路径，而非链接文件所在路径
```

> mkdir [ -mp ] dirname (Make Directory)

-m :直接设置目录权限，不使用默认权限（umask）

-p: 递归创建多级目录

```shell
[root@hadoop100 /tmp]# cd /tmp && mkdir -m 711 -p test1/test2/test3/test4 && ls -ld /tmp/test1 && tree /tmp
drwxr-xr-x 3 root root 19 8月  18 17:11 /tmp/test1
/tmp
└── test1
    └── test2
        └── test3
            └── test4

4 directories, 0 files
这里不知道为啥-m参数没有生效，应该是系统配置的原因吧，不用管
```

> rmdir [ -p ] dirname

-p:删除多级空目录

```shell
[root@hadoop100 /tmp]# rmdir -p test1/test2/test3
rmdir: 删除 "test1/test2/test3" 失败: 目录非空
[root@hadoop100 /tmp]# rmdir -p test1/test2/test3/test4 && ls /tmp
只有从最底层的空目录开始删除，才能将上层的空目录删除
```



###### 2.1.2	`$PATH`执行文件路径的变量

```shell
[root@hadoop100 /tmp]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@hadoop100 /tmp]# su - iboom
上一次登录：五 8月 18 17:28:35 CST 2023pts/2 上
[iboom@hadoop100 ~]$ echo $PATH
/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/iboom/.local/bin:/home/iboom/bin
```

可以看出root和iboom的PATH变量是不一样的

**使用绝对路径执行命令**

```shell
[root@hadoop100 /]# /bin/ls /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  txt12  usr  var
```

### 2.2	文件与目录管理

###### 2.2.1	文件与目录的查看：ls

> `ls [ -aAdfFhilnrRSt ]  filename/dirname`
> `ls [ --color={never,auto,always}]  filename/dirname`
> `ls [ --full-time]  filename/dirname`

| -a : 显示所有文件和目录，包括以`.`开头的隐藏文件             |
| ------------------------------------------------------------ |
| -A:类似于 `-a`，但不包括特殊的 `.` 和 `..` 目录项            |
| -d:仅显示目录本身，而不是目录内部的内容。                    |
| -f:禁止排序，以文件系统中的顺序显示文件和目录                |
| -F:在每个目录名后面添加一个符号，表示其类型，例如 `/` 表示目录，`*` 表示可执行文件，`@` 表示链接等 |
| -h:以人类可读的方式显示文件大小，例如 KB、MB、GB             |
| -i:显示文件的 inode 号码                                     |
| -l:以长格式显示，显示文件的详细信息，包括权限、所有者、大小等 |
| -n:以数字 UID 和 GID 显示文件所有者和组                      |
| -r:反向排序，以相反的顺序显示文件和目录。(依据文件名的字母排序) |
| -R:递归地列出所有子目录的内容。                              |
| -S:以文件容量大小排序，而不是文件名大小                      |
| -t：以时间排序，而不是文件名大小                             |
| --color=never 禁止按文件特性给予颜色显示                     |
| --color=auto 系统来决定是否给予颜色                          |
| --color=always 总是显示颜色                                  |
| --full-time  以完整的时间格式（年、月、日、时、分、秒、毫秒） |
| --time={atime,ctime}  输出access访问时间或改变权限时间（atime），而非内容修改时间（mtime） |

可以看到长格式列出，显示了隐藏文件，普通文件是白色，目录是蓝色，可执行文件是绿色

```shell
[root@hadoop100 ~]# ls -al /root
总用量 3136
dr-xr-x---. 17 root root    4096 8月  14 23:40 .
dr-xr-xr-x. 18 root root     269 2月  22 12:53 ..
-rwx--x--x.  1 root root    1780 1月  23 2023 anaconda-ks.cfg
-rw-------.  1 root root   14652 8月  18 18:02 .bash_history
-rw-r--r--.  1 root root      18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root     176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root     176 12月 29 2013 .bashrc
drwx------. 15 root root    4096 3月  18 15:22 .cache
drwx------. 16 root root    4096 3月  18 15:23 .config
-rw-r--r--.  1 root root     100 12月 29 2013 .cshrc
drwx------.  3 root root      25 1月  23 2023 .dbus
-rw-------   1 root root      16 3月  18 15:22 .esd_auth
drwx------   2 root root       6 3月  18 12:36 .gvfs
-rw-------   1 root root     310 3月  18 15:22 .ICEauthority
-rw-r--r--.  1 root root    1811 1月  23 2023 initial-setup-ks.cfg
-rw-------   1 root root      67 3月  19 08:03 .lesshst
drwxr-xr-x.  3 root root      19 1月  24 2023 .local
drwxr-----   3 root root      19 7月  12 22:54 .pki
drwx------   2 root root      25 1月  24 2023 .ssh
```

可以看到长格式列出，显示了隐藏文件，所有文件颜色都是白色，且目录后面加了   `/` ，可执行文件后面加了 `*`

```shell
[root@hadoop100 ~]# ls -alF --color=never /root
总用量 3136
dr-xr-x---. 17 root root    4096 8月  14 23:40 ./
dr-xr-xr-x. 18 root root     269 2月  22 12:53 ../
-rwx--x--x.  1 root root    1780 1月  23 2023 anaconda-ks.cfg*
-rw-------.  1 root root   14652 8月  18 18:02 .bash_history
-rw-r--r--.  1 root root      18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root     176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root     176 12月 29 2013 .bashrc
drwx------. 15 root root    4096 3月  18 15:22 .cache/
drwx------. 16 root root    4096 3月  18 15:23 .config/
-rw-r--r--.  1 root root     100 12月 29 2013 .cshrc
drwx------.  3 root root      25 1月  23 2023 .dbus/
-rw-------   1 root root      16 3月  18 15:22 .esd_auth
drwx------   2 root root       6 3月  18 12:36 .gvfs/
-rw-------   1 root root     310 3月  18 15:22 .ICEauthority
-rw-r--r--.  1 root root    1811 1月  23 2023 initial-setup-ks.cfg
-rw-------   1 root root      67 3月  19 08:03 .lesshst
drwxr-xr-x.  3 root root      19 1月  24 2023 .local/
drwxr-----   3 root root      19 7月  12 22:54 .pki/
drwx------   2 root root      25 1月  24 2023 .ssh/
```

可以看到长格式列出，显示了隐藏文件，普通文件是白色，目录是蓝色，可执行文件是绿色，且完整显示上一次修改时间（mtime）

```shell
[root@hadoop100 ~]# ls -al --full-time /root
总用量 3136
dr-xr-x---. 17 root root    4096 2023-08-14 23:40:09.380565852 +0800 .
dr-xr-xr-x. 18 root root     269 2023-02-22 12:53:32.469948984 +0800 ..
-rwx--x--x.  1 root root    1780 2023-01-23 22:22:43.719033726 +0800 anaconda-ks.cfg
-rw-------.  1 root root   14652 2023-08-18 18:02:12.617997119 +0800 .bash_history
-rw-r--r--.  1 root root      18 2013-12-29 10:26:31.000000000 +0800 .bash_logout
-rw-r--r--.  1 root root     176 2013-12-29 10:26:31.000000000 +0800 .bash_profile
-rw-r--r--.  1 root root     176 2013-12-29 10:26:31.000000000 +0800 .bashrc
drwx------. 15 root root    4096 2023-03-18 15:22:59.667075563 +0800 .cache
drwx------. 16 root root    4096 2023-03-18 15:23:10.974021882 +0800 .config
-rw-r--r--.  1 root root     100 2013-12-29 10:26:31.000000000 +0800 .cshrc
drwx------.  3 root root      25 2023-01-23 22:25:16.567000484 +0800 .dbus
-rw-------   1 root root      16 2023-03-18 15:22:42.273906072 +0800 .esd_auth
drwx------   2 root root       6 2023-03-18 12:36:34.939069467 +0800 .gvfs
-rw-------   1 root root     310 2023-03-18 15:22:40.672890470 +0800 .ICEauthority
-rw-r--r--.  1 root root    1811 2023-01-23 22:26:01.194002274 +0800 initial-setup-ks.cfg
-rw-------   1 root root      67 2023-03-19 08:03:52.142478064 +0800 .lesshst
drwxr-xr-x.  3 root root      19 2023-01-24 01:52:37.532998522 +0800 .local
drwxr-----   3 root root      19 2023-07-12 22:54:03.027971330 +0800 .pki
drwx------   2 root root      25 2023-01-24 02:36:45.075990269 +0800 .ssh
```



###### 2.2.2	复制、删除、移动：cp、rm、mv

> cp [ -adfilprsu ] source destination
>
> cp [ options ] source1 source2 ... directory

| -a 相当于 -dr --preserve=all                                 |
| ------------------------------------------------------------ |
| -d 若源文件为链接文件的属性，则复制链接文件属性，而非文件本身 |
| -f 若目标文件存在，则强行覆盖                                |
| -i 若目标文件存在，则先进行询问是否覆盖                      |
| -l 建立硬链接的连接文件，而非复制文件本身                    |
| -p 连同文件属性（权限，用户，时间）一同复制，而非使用默认的属性 |
| -r 递归复制目录下的文件及子目录                              |
| -s 复制为符号链接，即快捷方式                                |
| -u source比destination 更新时才复制并覆盖，或者destination不存在时才复制 |
|                                                              |



```shell
[root@hadoop100 ~]# cp -i .tcshrc /tmp   # 由于/tmp目录下已存在 .tcshrc 这个文件，使用cp覆盖时询问
cp：是否覆盖"/tmp/.tcshrc"？ y

[root@hadoop100 /tmp]# cp /var/log/wtmp /tmp
[root@hadoop100 /tmp]# ls -l /var/log/wtmp /tmp/wtmp
-rw-r--r--  1 root root 174720 8月  18 19:39 /tmp/wtmp   # 文件的属性改变（所属组由wtmp -> root，文件修改时间也变了）
-rw-rw-r--. 1 root utmp 174720 8月  18 19:29 /var/log/wtmp  

[root@hadoop100 /tmp]# cp -a /var/log/wtmp /tmp/wtmp_1
[root@hadoop100 /tmp]# ls -l /var/log/wtmp /tmp/wtmp_1
-rw-rw-r--. 1 root utmp 174720 8月  18 19:29 /tmp/wtmp_1  # 文件属性未发生变化，这是加了 -a 参数的原因
-rw-rw-r--. 1 root utmp 174720 8月  18 19:29 /var/log/wtmp

[root@hadoop100 /tmp]# cp /etc /tmp
cp: 略过目录"/etc"                                       # 复制目录需要使用 -r 参数递归复制
[root@hadoop100 /tmp]# cp  -r /etc /tmp && ll -d etc
drwxr-xr-x 140 root root 8192 8月  18 19:50 etc

[root@hadoop100 /tmp]# cp ~/.bashrc ~/.bash_profile /tmp && ls -al |grep .bash  # 复制多个文件到一个目录
-rw-r--r--    1 root root    176 8月  18 19:56 .bash_profile
-rw-r--r--    1 root root    176 8月  18 19:56 .bashrc

[root@hadoop100 /tmp]# cp -s .bashrc bashrc_slink &&  cp -l .bashrc bashrc_hlink && ls -l bashrc*
-rw-r--r-- 2 root root 176 8月  18 19:56 bashrc_hlink       # 硬链接 inode由 1 -> 2
lrwxrwxrwx 1 root root   7 8月  18 20:03 bashrc_slink -> .bashrc  # 软连接（符号链接）

[root@hadoop100 /tmp]# cp -d bashrc_slink  bashrc_slink_1
[root@hadoop100 /tmp]# cp  bashrc_slink  bashrc_slink_2
[root@hadoop100 /tmp]# ls -l .
总用量 364
-rw-r--r--    2 root root    176 8月  18 19:56 bashrc_hlink
lrwxrwxrwx    1 root root      7 8月  18 20:03 bashrc_slink -> .bashrc   
lrwxrwxrwx    1 root root      7 8月  18 20:06 bashrc_slink_1 -> .bashrc   # 使用 -d 参数复制链接文件的属性
-rw-r--r--    1 root root    176 8月  18 20:06 bashrc_slink_2              # 没有使用参数复制的是文件本身属性（原始文件）
drwxr-xr-x  140 root root   8192 8月  18 19:50 etc
-rw-r--r--    1 root root 174720 8月  18 19:39 wtmp
-rw-rw-r--.   1 root utmp 174720 8月  18 19:29 wtmp_1
```

可见：默认情况下复制过来的文件属性与当前用户相关

> rm [-fir] file/dir

| -f 若文件不存在或使用了-i参数，不发出警告 |
| ----------------------------------------- |
| -i询问是否删除                            |
| -r 用于目录的递归删除                     |

```shell
[root@hadoop100 /tmp]# rm .bashrc
rm：是否删除普通文件 ".bashrc"？   # rm 相当于 rm -i

[root@hadoop100 /tmp]# \rm .bashrc  # 使用\反斜线去除alias的别名

[root@hadoop100 /tmp]# cp -a /etc .  
[root@hadoop100 /tmp]# rm -rf etc   # 使用 -r递归删除etc目录 -f不发出询问警告
```

> mv [-fiu] source destination
>
> mv [options] source1 source2 ... directory

|      |
| ---- |
|      |
|      |









