+++
date = '2025-06-07T01:12:21+08:00'
draft = false
title = '基础shell命令'
categories = ["编程相关"]
tags = ["shell"]
+++

## 用户与用户组
`useradd`、`groupadd`、`chown`、`chgrp`、`chmod`、`usermod`、`userdel`、`groupdel`
### 🧑‍💻 一、useradd：添加用户
~~~cmd
sudo useradd [选项] 用户名
~~~
常用选项：
| 选项             | 说明                       |
| -------------- | ------------------------ |
| `-m`           | 自动创建用户主目录（如 `/home/用户名`） |
| `-s /bin/bash` | 指定默认 shell               |
| `-g 组名`        | 指定主组                     |
| `-G 组1,组2`     | 加入附加组                    |
示例：
~~~cmd
sudo useradd -m -s /bin/bash -g users -G wheel,developers alice
~~~
然后设置密码：
~~~cmd
sudo passwd alice
~~~
### 👥 二、groupadd：添加用户组
~~~cmd
sudo groupadd 组名
~~~
示例：
~~~cmd
sudo groupadd developers
~~~
常用选项：
| 选项             | 说明                       |
| -------------- | ------------------------ |
| `-g GID`           | 设置自定义的GID |
| `-U users` |      设置组成员         |
| `-f`        |      如果设定的GID已经存在则强制覆盖原组          |
### 📂 三、chown：更改文件/目录的属主和属组（change owner）
~~~cmd
sudo chown [选项] 用户[:组] 文件
~~~
常用选项：
| 选项   | 说明          |
| ---- | ----------- |
| `-R` | 递归更改目录下所有文件 |
示例：
~~~cmd
sudo chown alice file.txt # 改文件属主为 alice
sudo chown alice:developers file.txt # 改文件属主为 alice 属组为 developers 
sudo chown -R bob: /var/www/ # 递归更改整个目录
~~~
### 👪 四、chgrp：只更改属组（change group）
~~~cmd
sudo chgrp [选项] 组 文件
~~~
示例：
~~~cmd
sudo chgrp developers file.txt
sudo chgrp -R staff /opt/shared # 递归更改整个目录
~~~
### 🔐 五、chmod：更改权限（change mode）
~~~cmd
chmod [选项] 模式 文件
~~~
权限符号简记：
| 符号  | 含义      |
| --- | ------- |
| `r` | 读权限（4）  |
| `w` | 写权限（2）  |
| `x` | 执行权限（1） |
模式表示法：
1. 数字表示法（常用）：
~~~cmd
chmod 755 file
# 表示：
# owner: rwx (7), group: rx (5), others: rx (5)
~~~
2. 符号表示法：
~~~cmd
chmod u+x file     # 给属主添加执行权限
chmod g-w file     # 移除属组写权限
chmod o=r file     # 设置其他用户为只读
~~~
递归修改目录：
~~~cmd
chmod -R 755 /some/dir
~~~
🎯 权限组合实例：
| 权限    | 数字        | 含义            |
| ----- | --------- | ------------- |
| `777` | rwxrwxrwx | 所有人可读写执行（不安全） |
| `755` | rwxr-xr-x | 常用于脚本或网站目录    |
| `700` | rwx------ | 仅属主可读写执行      |
| `644` | rw-r--r-- | 常用于文本配置文件     |
| `600` | rw------- | 私密文件（如密钥）     |

### 👤 六、usermod：修改用户信息
~~~cmd
usermod [选项] 用户名
~~~
常用选项：
| 选项             | 功能                   |
| -------------- | -------------------- |
| `-l 新用户名`      | 修改用户名                |
| `-d /新路径`      | 修改主目录（需配合 `-m` 移动内容） |
| `-m`           | 移动用户主目录内容到新路径        |
| `-s /bin/bash` | 修改默认 shell           |
| `-g 组名`        | 设置主组                 |
| `-G 组1,组2,...` | 设置附加组（会覆盖原有附加组）      |
| `-aG 组名`       | 追加附加组（不删除原有附加组）      |
| `-L` / `-U`    | 锁定/解锁账号（禁止/恢复登录）     |
🧪 示例：
➤ 添加用户 alice 到 `sudo` 组：
~~~cmd
sudo usermod -aG sudo alice
~~~
➤ 修改用户 shell 为 bash：
~~~cmd
sudo usermod -s /bin/bash alice
~~~
➤ 改用户名 alice 为 alina：
~~~cmd
sudo usermod -l alina alice
~~~

### ❌ 七、userdel：删除用户
~~~cmd
userdel [选项] 用户名
~~~
常用选项：
| 选项   | 功能              |
| ---- | --------------- |
| `-r` | 同时删除用户主目录、邮箱等文件 |
🧪 示例：
➤ 删除用户但保留主目录：
~~~cmd
sudo userdel alice
~~~
➤ 删除用户并清理其主目录：
~~~cmd
sudo userdel -r alice
~~~
> ⚠️ 注意：如果用户当前正在登录，userdel 会失败，需先 pkill -u alice 再删。

### 👥 八、groupdel：删除组
~~~cmd
sudo groupdel 组名
~~~
> ⚠️ 不能删除有用户主组为该组的情况，需先修改相关用户的主组。
🧪 示例：
➤ 删除 developers 组：
~~~cmd
sudo groupdel developers
~~~
**🔍 用户和组信息查看补充命令**
| 命令              | 功能                |
| --------------- | ----------------- |
| `id 用户名`        | 查看用户 UID/GID 和所属组 |
| `groups 用户名`    | 查看该用户所属所有组        |
| `getent passwd` | 列出所有用户            |
| `getent group`  | 列出所有组             |

**✅ 总结小贴士**
| 目的       | 命令示例                           |
| -------- | ------------------------------ |
| 修改用户主目录  | `usermod -d /new/home -m user` |
| 添加附加组    | `usermod -aG group user`       |
| 删除用户和主目录 | `userdel -r user`              |
| 删除无用组    | `groupdel group`               |


### 🧪 示例：设置目录权限给指定用户和组：
~~~cmd
sudo useradd -m alice # -G devs
sudo groupadd devs # -U alice
sudo usermod -aG devs alice

sudo mkdir /data/project
sudo chown alice:devs /data/project
sudo chmod 770 /data/project
~~~
此时 `/data/project`：
- 属主是 `alice`；
- 属组是 `devs`；
- 只有属主和组用户有读写执行权限，其他用户无权访问。


### 访问权限相关
- 对文件来讲，权限的功能为：
  - `r`：可读取此一文件的实际内容，如读取文本文件的文字内容等；
  - `w`：可以编辑、新增或是修改该文件的内容(但不删除该文件)；
  - `x`：该文件具有可以被系统执行的权限。
- 对目录来说，权限的功能为：
  - `r`：读取目录中的内容；
  - `w`：修改目录中的内容；
  - `x`：访问目录。

## 目录与路径
~~~cmd
.           代表此层目录。
..          代表上一层目录。
-           代表前一个工作目录。
~           代表目前使用者身份所在的家目录。
~username   代表username这个使用者的家目录
~~~


### 😎 cd - change directory
<ins>cd changes the current working directory.</ins>
~~~cmd
cd /path/to/dir
~~~

### 😋 pwd - output the current working directory
<ins>pwd outputs (prints) the current working directory</ins>

**SYNOPSIS(摘要)**

`pwd [-P | --physical]`<br>
`pwd [-L | --logical]`
~~~cmd
pwd # -p
~~~

### 🆕 mkdir - make directories
<ins> Create the DIRECTORY(ies), if they do not already exist.</ins>
~~~cmd
mkdir [options] dirName
~~~
****Common options(常见选项)：****
| 选项   | 含义                  |
| ---- | ------------------- |
| `-p` | 递归创建多级目录，不存在时自动建父目录 |
| `-m`| 设置file mode 而不是使用umask |
| `-v` | verbose: print a message for each created directory |

### ❌ rmdir - remove empty directories
<ins>Remove the DIRECTORY(ies), if they are empty.</ins>
~~~cmd
rmdir -p/-v emptyDir
~~~
**✅ 总结表：**
| 命令      | 功能     | 常见用法示例                  |
| ------- | ------ | ----------------------- |
| `cd`    | 切换目录   | `cd /etc`, `cd ..`      |
| `pwd`   | 显示当前路径 | `pwd`                   |
| `mkdir` | 创建目录   | `mkdir -p my/data/logs` |
| `rmdir` | 删除空目录  | `rmdir temp_dir`        |

### 📁 一、文件与目录操作类
**🔸ls：列出目录内容**
~~~cmd
ls [选项] [目录]
~~~
- `-l`： 详细列表
- `-a`： 显示隐藏文件
- `-h`： 人类可读的文件大小
- `-R`：递归列出子目录

例：
~~~cmd
ls -lah /etc
~~~
**🔸cp：复制文件或目录**
~~~cmd
cp [选项] 源文件 目标
~~~
- `-r`：递归复制目录
- `-i`：覆盖前询问
- `-u`：仅复制较新文件

****其他选项：****
`-a`:将原有属性复制和链接文件原带复制，
`-p`:将文件属性一起复制，不复制链接文件(和`-a`的区别)，`-l`:硬链接文件建立，`-s`:软链接文件建立

例：
~~~cmd
cp -ri ~/docs /mnt/usb/
cp -rp ~/docs /mnt/usb/
cp -a ~/docs /tmp
~~~
**🔸rm：删除文件或目录**
~~~cmd
rm [选项] 文件/目录
~~~
- `-r`：递归删除目录
- `-f`：强制删除
- `-i`：每次删除前确认

[注释]: #

例：
~~~cmd
rm -rf ~/temp/
~~~
**🔸mv：移动或重命名文件**
~~~cmd
mv [选项] 源 目标
~~~
- `-i`：覆盖时提示
- `-u`：仅移动更新文件
例：
~~~cmd
mv file.txt backup/file.txt
~~~
**🔸basename：获取路径中的文件名部分**
~~~cmd
basename 路径 [后缀]
~~~
例：
~~~cmd
basename /etc/passwd # 输出: passwd
basename /path/file.txt .txt # 输出: file
~~~
**🔸dirname：获取路径中的目录部分**
~~~cmd
dirname 路径
~~~
例：
~~~cmd
dirname /etc/passwd # 输出：/etc
~~~
**🔸touch：创建空文件或更新文件时间戳**
~~~cmd
touch [-acdmt] 文件
~~~
- `-a`：仅自定义access time；
- `-t`：后面可以接自定义的日期而不用目前的日期，格式为[YYYYMMDDhhmm];
- `-d`：接自定义日期 

例：
~~~cmd
touch test
touch -d "2 days ago" bashrc
touch -t 202506051111 bashrc
~~~
> ⚠修改的是mtime和atime

### 📖 二、查看文件内容类
**🔸cat：连接并显示文件内容**
~~~cmd
cat [-AbEnTv] files
~~~
- `-A`：相当于-vET的整合选项，可列出一些特殊字符而不是空白而已；
- `-n`：打印出行号，连同空白行也有行号，与`-b`选项不同
- `-b`：列出行号，空白行不列

例：
~~~cmd
cat file.txt
cat -n file.txt 
cat -b /etc/passwd
cat -A /etc/pacman.conf
~~~
**🔸tac：逆序显示文件内容**
~~~cmd
tac file
~~~
**🔸more：分页显示（不能向上滚动）**
> <ins>空格翻页，less同理</ins>

**🔸less：分页显示（支持上下滚动）**
> ❤ 支持向上向下翻动一行，`/`查找字符串，`n`向下查找，`?`向上查找
>> <font color="red">善用``man``和``tldr``指令</font>

**🔸head：显示前几行**
- `-n`：最常用，代表显示前几行
~~~cmd
head -n 10 /etc/pacman.conf
~~~
显示文件前十行

**🔸tail：显示末尾几行**
- `-n`：显示末尾前几行
- `-f`：跟踪文件变化
> `-f`用于
>> <p style="color:pink"><ins>日志随时写入情况</ins></p>

~~~cmd
tail -n 20 /etc/passwd
tail -f /var/log/syslog
~~~
> <font color="#E3A212"><ins>tail常和head结合使用查找一个ascii文本文件区间范围文本</ins></font>
>> **显示/etc/pacman.conf文件11到20行**
>>> `head -n 20 /etc/pacman.conf | tail -n 10`
>>>> _先显示前20行后显示后10行_ <br>

>`cat -n /etc/passwd | head -n 10 | tail -n 5`
>> _可以列出行号的截取_

**🔸od（Octal Dump）：以八进制或十六进制显示文件内容（用于二进制分析）**
~~~cmd
od [-t type] 文件
~~~
`-t`后面可以接各种类型的输出，例如：

a   ：利用默认字符来输出 <br>
c   ：使用ASCII字符来输出<br>
d [size]  ：使用十进制(decimal)来输出数据，每个整数占用size Bytes；<br>
f [size]  ：使用浮点数输出，其他同上<br>
o [size]  ：八进制<br>
x [size]  ：十六进制<br>
- 范例一：将`/usr/bin/passwd`的内容使用ASCII方式来显示。
~~~cmd
od -t c /usr/bin/passwd
~~~
- 范例二：将`/etc/issue`这个文件内容以八进制列出存储值
~~~cmd
od -t oCc /etc/issue
~~~
- 范例三：用作ascii对照表查找字符的ASCII码
~~~cmd
echo password | od -t oCc
~~~
> 其实不用带`-t`也可以

### 🔒 三、权限与属性管理
**🔸chattr：改变文件属性（适用于 ext 和部分文件系统）**
~~~cmd
chattr [+-=] [ASacdistu] 文件或目录名称
~~~
- `-i`：让一个文件「不能被删除，改名，设置链接也无法写入或新增数据」
- `-c`：设置这个属性会使文件自动被压缩，读取时解压缩

~~~cmd
chattr +i file
chattr -i file
chattr +c file
chattr -c file
~~~
**🔸lsattr：查看文件属性**
~~~cmd
lsattr [-adR] 文件或目录
~~~
> **man chattr** or *tldr lsattr*

**🔸umask：显示或设置默认权限掩码**
~~~cmd
umask / umask -S/s和-p都是显示隐藏的默认创建文件权限码
umask 022 设置创建文件时的文件权限
~~~

### 🧩 四、文件类型与状态
**🔸file：识别文件类型**
~~~cmd
file /bin/ls
~~~
**🔸stat：查看详细状态（权限、大小、时间戳）**
~~~cmd
stat filename
~~~
### 🔎 五、文件查找与定位
**🔸which：查命令路径（依赖 $PATH）**
~~~cmd
which bash
~~~
**🔸whereis：查命令、源码、man 手册位置**
~~~cmd
whereis ls
~~~
**🔸locate：快速查找文件（依赖数据库）**
~~~cmd
sudo updatedb
locate passwd
~~~
**🔸find：遍历文件系统查找文件(最常用)**
`find 路径 条件 动作`
1. **与时间有关的选项：`-atime`,`-citme`,`mtime`，以`mtime`举例**
- `-mtime n`：n为数字，意义为在n天之前的「一天之内」被修改过内容的文件；
- `-mtime +n`：列出在n天之前(不含n天本身)被修改过内容的文件；
- `-mtime -n`：列出在n天之内(含n天本身)被修改过内容的文件；
- `-newer file`：file为一个存在的文件，列出比file还要新的文件

范例一：将系统上24小时内有修改过的文件列出

`find / -mtime 0`

将三天前那一天的24小时内有修改过的文件列出

`find / -mtime 3`

范例二：寻找`/etc`下的文件，如果文件日期比`/etc/passwd`新就列出

`find /etc -newer /etc/passwd`

三天内<br>
`find / -mtime -3`

三天外<br>
`find / -mtime +3`


2. **与使用者或用户组有关的参数：`-uid n`,`-gid n`,`-user name`,`-group name`,`-nouser`,`-nogroup`**

范例三：查找/home下属于Nagisa的文件
