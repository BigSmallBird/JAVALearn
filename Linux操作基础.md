# 

# 目录介绍：

在linux里面，都是文件：![示意图](./assets/similar-diagram.svg)![示意图](./assets/similar-diagram.svg)![示意图](./assets/similar-diagram.svg)

* **/sbin：**就是super User的意思，这里存放的是系统管理员使用的系统程序
* **/home：**存放的是普通用户的主目录，在 Linux 里面每个用户都有自己的目录，以用户名来命名
* **/root：**该文件是系统管理员，也称为草鸡权限用户目录
* **/lib：**系统开机所需要的基本动态链接共享库，类似 Windows 下面的 DLL 文件，所有的文件程序都需要用这个共享库
* **/etc：**所有的系统管理所需要的配置文件和子目录
* **/usr：** 用户的很多应用程序和文件都在这个目录下面类似于 Windows的 progromfile
* **boot：** 这里面是启动 linux 时用的核心文件，包括一些连接文件以及镜像文件
* **/srv：** service ，该目录存放一些服务启动之后需要的数据
* **/tmp：** 存放临时文件
* **/dev：** 类似 Windows 的管理器，把所有的硬件用文件的形式存储
* **/media：** linux 系统会自动识别一些设备例如 u 盘，光驱，识别后的设备挂载这个目录下
* **/mnt：**  系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将外部的存储挂载在/mnt上，然后进入该目录就可以查看里面的内容了。和media差不多
* **/opt：** 这是给主机额外安装软件所摆放的目录。比如你安装一个mysql数据库则就可以放在这个目录下。默认是空的。
* **/var：**这个目录中存放着在不断扩充的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。
* **/lost+found：**这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
* **/www：** /www目录可以用于存储Web应用程序或网站文件，/www目录没有特别的用途，它只是一个常规目录，可以

# vi / vim 编辑器：

用户 @ 主机名 组成，~ 号代表当前目录位置， # 号代表管理员用户， $ 一般代表普通玩家![示意图](./assets/similar-diagram.svg)

![示意图](./assets/similar-diagram.svg)

VI 是 Unix 操作系统和类似 Unix 操作系统中最常用的文本编辑器。VIM 编辑器是从 VI 发展出来的一个性能更强大的文本编辑器。可以主动的以字体颜色辨别语法正确性，方便程序设计，VIM 与 VI 编辑器完全相融

![示意图](./assets/similar-diagram.svg)

模式转换：

进来默认是一般模式，想要编辑文本就 按 i 进入编辑模式

![示意图](./assets/similar-diagram.svg)

## vim 常用语法:

### 一般模式下：

![示意图](./assets/similar-diagram.svg)

### 编辑模式：

![示意图](./assets/similar-diagram.svg)

### 命令模式：

![示意图](./assets/similar-diagram.svg)

# 网络配置：

## 修改 ip 地址:

```bash
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

![示意图](./assets/similar-diagram.svg)

执行 service network restart 重启网络，使网络生效。

## 配置主机名：

### 修改主机名：

```bash
# 查看当前服务器名称
hostname

# 通过编辑 /etc/hostname 文件修改主机名
vim /etc/hostname

# 修改完重启后就生效了
```

### 修改 hosts 映射文件：

```bash
vim /etc/hostname
```

添加映射，保存后，可以直接访问 service，就可以访问到 220.151 ip 了

![示意图](./assets/similar-diagram.svg)

# 系统配置：

略：

# 常用命令：

## 文件目录类：

### pwd：

```bash
#  pwd 显示当前工作目录的绝对路径
pwd
```

```bash
# ls 列出目录的内容

ls: 列出当前目录中的文件和字目录。
ls -1 ：以长格式列出当前目录中的文件和子目录，包括文件权限，所有者，文件大小，修改日期等。
ls -a ：列出当前目录中的所有文件和字目录，包括隐藏文件和目录。
ls -t ：按修改时间排序列出文件和子目录，最新修改的文件和目录将显示在最上面。
ls -R ：递归列出当前目录及其子目录中所有文件和子目录。
ls -i ：显示每个文件和目录的 inode 号码
```

### ls

ls [选项] [目录或是文件]

可以多个参数组合，如： ls -al

每行列出的信息依次是：文件类型与权限，链接数，文件属主，文件属组，文件大小用 byte 表示，建立或者修改时间

### cd 切换目录：

![示意图](./assets/similar-diagram.svg)

### mkdir / rmdir 创建或删除一个空目录:

mkdir [选项] 要创建一个新目录：

```bash
mkdir you
mkdir -p you/dssz/menoutihouwang  # -p 创建多层目录
```

rmdir 要删除的空目录

```bash
rmdir you/dssz/menotihouwang
```

### touch 创建空文件：

touch 文件名称

```bash
touch you/dssz/A.txt
```

### cp 复制文件目录：

#### cp 源文件 目标文件

```bash
cp you/A.txt me/
```

#### 递归复制整个文件夹

参数 ： -r 递归复制整个文件夹

```bash
cp -r you/me/./
```

### rm 删除文件或目录：

rm [选项] deleteFile (功能描述：递归删除目录中所有内容)

![示意图](./assets/similar-diagram.svg)

```bash
rm yourL.txt # 删除文件

rm -rf dssz/ # 递归删除目录里面的所有内容
```

### mv 移动文件与目录或重命名：

mv 源文件 目标文件

1. mv oldNameFile newNameFile （重命名）
2. mv /temp/movefile /targetFolder （移动文件）

```bash
# 重命名
mv your/Love/UP.txt your/Love/BREAK.txt

# 移动文件
mv your/Love.txt /Bin
```

### cat  查看文件内容

查看文件内容，从第一行开始显示。

一般查看比较小的文件

```bash
cat -n Love.txt # -n 参数显示行号
```

### more 文件内容分屏查看器：

more 要查看的文件

![示意图](./assets/similar-diagram.svg)

```bash
more Love.txt
```

### less 分屏显示文件内容：

less 指令用来分屏查看文件内容，它的功能与 more 指令类似，但是比 more 指令更加强大，支持各种显示终端。less 指令在显示文件内容时，并不是一次将整个文件加载出来之后才显示，而是根据显示需要加载内容，对于显示大型文件具有较高的效率。

![示意图](./assets/similar-diagram.svg)

```bash
less Love.txt
```

### echo 输出内容到控制台：

echo [选项] [输出内容]

-e ：支持反斜线控制的字符转换

![示意图](./assets/similar-diagram.svg)

```bash
echo "yy\yy"  # yy\yy

echo -e "yyy\tyyy"  # yyy    yyy
```

### tail 输出文件尾部内容：

tail  用于输出文件中尾部的内容，默认情况下 tail 指令显示文件的后 10 行内容。

```bash
# 查看文件尾 1 行内容
tail -n 1 smartd.txt

# 实时追踪文档所有的更新
tail -f yyy.txt
```

ctrl + s = 暂停

ctrl + q = 继续

### .   > 输出重定向 和 >> 追加：

#### 将 ls 查看信息写入文件里面：

```bash
ls -l>y.txt
```

#### 将 ls 查看信息追加到文件中

```bash
ls -l>>y.txt
```

#### 采用 echo 将 hello 单词追加到文件里

```bash
echo hello>>y.txt
```

### ln 软连接：

软连接也叫符号链接，类似于 windows 里面的快捷方式，有自己的数据块，主要存放了链接其他文件的路径。

ln -s[原文件或目录] [软链接名]  （给原文件创建一个软连接）

#### 创建软连接

```bash
ln -s /y/s.txt ./s
```

#### 删除软连接： rm -rf 软连接名，

如果使用 rm -rf 软连接名/ 删除，会把软连接对应的真实目录俩面的内容删除掉

```bash
rm -rf s
```

查询： 通过 || 就可以查看，列表属性第一位是 | ，尾部会有位置指向。

### histoey 查看已经执行的历命令：

#### 查看已经执行过的历史命令

```bash
history
```

#### 清空历史记录

```bash
history -c
```

### 时间日期类:

#### 显示当前时间信息：

```bash
date
```

#### 设置系统时间：

```bash
date -s "2025-6-12 11:33:22"
```

## 用户管理命令：

略。。。

## 文件权限类：

### 查看文件权限：

文件属性：![示意图](./assets/similar-diagram.svg)

**使用 ll 或者 ls -lh 命令查看权限**

如果没有权限，就会出现 [ - ] 。从左到右用 0 - 9 表示

#### 对于  rwx 作用文件夹与目录的不同解释：

* 作用到文件：

[ r ] 代表可读 （read）: 可以读取，查看

[ w ] 代表可写 （write）：可以修改，但是不代表可以删除该文件。

[ x ] 代表可执行 （execute） ： 可以被系统执行

* 作用到目录： 

[ r ] 代表课读 （read）： 可以读取，ls查看目录内容

[ w ] 代表课写 （write） ： 可以修改，目录内创建，删除，重命名目录

[ x ] 代表可执行 （execute） ： 可以进入该目录

#### ll 命令：![示意图](./assets/similar-diagram.svg)

#### ls -lh 命令，文件大小清楚：

![示意图](./assets/similar-diagram.svg)

1. 如果查看到的是文件：链接数指硬链接个数
2. 如果是文件夹：指的是子文件个数

### 文件权限修改：

#### 规则：

## chmod 改变权限：

chmod [{ugoa}{+-=}{rwx}] 文件目录

```bash
u:所有者（user）        g:所有组(group)           o:其他人(other)        a:所有人(u、g、o 的总和)

+号代表增加      -号代表取消     = 号代表直接赋予
```

#### 修改文件使其所属用户具有执行权限：

```bash
chmod u+x your.txt
```

#### 修改文件使其所属组用户具有执行权限：

```bash
chmod g+x your.txt
```

#### 修改文件所属主用户执行权限，并使其他用户具有执行权限

```bash
chmod u-x,0+x you.txt
```

### 第二种方式更变权限：

#### 规则：

chmod [mode=421 ] [文件或目录]

```bash
r=4         w=2        x=1       rwx=4+2+1=7
```

#### 采用数字的方式，设置文件所有者，所属组，其他用户都具有 rwx 权限

```bash
chmod 777 y.txt
```

#### 修改整个文件夹里面的所有文件的所有者，所属组，其他用户都具有 rwx 权限：

```bash
chmod -R 777 you/
```

## chown 改变所有者：

#### 主要参数

- `-R` 或 `--recursive`：递归地更改目录及其内容的所有权
- `-v` 或 `--verbose`：显示详细的操作信息
- `-h` 或 `--no-dereference`：只更改符号链接本身，而不是链接指向的文件
- `--from=当前所有者:当前组`：只在文件当前所有者或组匹配时才更改

chown [选项] [最终用户] [文件或目录]  (改变文件或目录所有者)

#### 修改文件所有者：

```bash
chown newuser file.txt
```

#### 递归改变文件所有者和所有组

```bash
chown -R newuser:newgroup yyy/
```

## chgrp 改变所有组：

chgrp [最终用户组] [文件或目录]  (改变文件或者目录的所属组)

```bash
chgrp root s.txt
```

## 搜索查找类：

### find 查找文件或者目录：

find 指令将从指定目录向下递归地历遍其各个子目录，将满足条件的文件显示在终端。![示意图](./assets/similar-diagram.svg)

#### 按照文件名：根据名称查找  /目录  下的 XXX.txt 文件

```bash
find -name "*.txt"

find yourLove/ -name "rel_i_me.txt"
```

#### 按照拥有者：查找 /opt 目录下，用户名称 -user 的文件：

```bash
find opt/ -user XXX
```

按照文件大小：在 /LoveMe 目录下查找大于 200m 的文件（+n 大于 -n小于  n等于）

```bash
find /LoveMe -size +1314
```

#### locate 快速定位文件路径：

locate 指令利用事先建立的系统中所有文件名称及路径的 locate 数据库事先快速定位给定的文件。 Locate 指令不会历遍整个文件系统，查询速度块。

> 注意需要定期更新 Loacate

**基本语法：**

locate [搜索文件]

**因为 locate 指令基于数据库进行查询，所以第一次运行前，必须使用 updatedb 指令创建 locate 数据库。**(**database** 数据库)

#### 查询文件夹：

```bash
updatedb

locate XXXX
```

### grep 过滤查找与  |   管道符

管道符：  “ | ”  ，表示将前一个命令的处理结果输出传递给后面的命令进行处理。可以前面查出来的东西传给管道符后端处理，例如： ls  |  grep -n test

grep 是 linux 系统中常用的文本搜索工具，可以在文件中查找指定的字符串或者正则表达式，并将匹配的行输出到终端或者文件里面

#### 参数：

- **`-i`**：忽略大小写（如 `grep -in Test` 也会匹配 `testfile.txt`）。
- **`-r`**：递归搜索目录（如 `grep -rn test /path/to/dir`）。
- **`-v`**：反向匹配（显示不包含 `test` 的行）。
- **`-l`**：只输出文件名，不显示具体内容（如 `grep -l test *.txt`）。
- **`-n`**：显示行数

#### 在文件里面查找字符

```bash
# 单个文件查询
grep "Love" Love.txt

# 多个文件查询
grep "Love" file1 file2 file3

# 忽略大小写查询指定文件
grep -i "Love" filename
```

#### 在目录里面递归查找指定字符串：

```bash
grep -r "Love" directory
```

#### 显示匹配行：

```bash
grep -n "XXX" filename
```

#### 反向查找不包含指定字符串的行

```bash
grep -v "UNLOVE" filename
```

#### 匹配的文本输出到文件里面：

```bash
grep "Love" filename > oo.txt
```

## 压缩和解压类：

### gzip / gunzip 压缩:

> 注意：
> 
> 1. 只能压缩文件
> 2. 不保留原文件
> 3. 同时压缩多个文件会产生多个压缩包

#### gzip 压缩：

```bash
gizp 文件

gzip Love.txt
```

![示意图](./assets/similar-diagram.svg)

#### gunzip 压缩：

```bash
gunzip inTest.txt.gz
```

### zip / unzip 压缩 / 解压：

zip 压缩命令在Windows / linux 都通用**可以保留原文件**

#### zip 压缩

```bash
zip 压缩文件名。zip 源文件

# 压缩文件
zip inTest.zip inTest.txt

# -r 压缩文件夹（递归历遍文件）
zip -r XXX.zip XXX
```

#### unzip 解压：

```bash
unzip inTest.zip

# -d 指定解压后文件存放目录
unzip inTest.zip -d /MyLove
```

### tar 打包：(将多个文件或者目录合成一个，不改变文件大小)

tar [选项] XXX.tar,gz 将要打包进去的内容 （打包目录，压缩后的文件格式.tar.gz）

#### 参数：

![示意图](./assets/similar-diagram.svg)

#### 压缩文件：

```bash
tar -zcvf 打包文件名.tar.gz [源文件]

#多个文件打包
tar -zcvf filname.tar.gz filename1.txt filename2.txt
#打包文件夹
tar -zcvf Love.tar.gz dimo/
```

#### 解压：

```bash
# 解压当前目录
tar -zxvf XXX.tar.gz
# -C 解压到某个地方
tar -zxvf XXX.tar.gz -C /Love
```
