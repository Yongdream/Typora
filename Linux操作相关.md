# Linux

## Linux中，vim里替换一段字符串怎么做

在 Vim 中替换一段字符串可以使用 `:s` 命令。以下是具体步骤：

1. 打开 Vim 并进入你需要编辑的文件。

2. 确保你处于正常模式（按 `Esc` 键可以进入正常模式）。

3. 使用以下格式的命令进行替换：

   - **替换当前行的字符串：**

     ```vim
     :s/旧字符串/新字符串/g
     ```

   - **替换整个文件的字符串：**

     ```vim
     :%s/旧字符串/新字符串/g
     ```

   - **替换第 n 行开始到最后一行中每一行的第一个 str1 为 str2**

     ```vim
     :n,$s/str1/str2/
     ```

   - **替换第 n 行开始到最后一行中所有的 str1 为 str2**

     ```vim
     :n,$s/str1/str2/g
     ```

这些命令中的 `/g` 表示全局替换，也就是替换行中的所有匹配项。去掉 `/g` 只会替换每行中第一个匹配项。

## 在Linux下查看进程所耗资源

- **`top`:**用于实时监视系统中运行的进程。可以使用top命令查看各个进程所占用CPU、内存、虚拟内存、磁盘I/O等资源占用情况；
- **`PS`:**⽤于列出当前系统中运行的进程。可以使用ps命令查看各进程的 PID、CPU 占用、内存占用、状态等信息  
- **`htop`:**是top命令的升级版，提供了更丰富的功能和界面。可以使用htop命令查看进程的CPU 占用、内存占用、线程数量、进程状态等信息 
- **`pidstat`:**用于显示进程的资源使用情况。可以使用pidstat命令查看各进程的 CPU 使用情况、内存使用情况、I/O 操作情况等  
- **`iostat`:**用于监视系统的磁盘 I/O 性能。可以使用iostat命令查看各进程的磁盘 I/O 操作情况，包括每个进程的读写速度、请求队列长度、磁盘利用率等。  

## Linux中查找某个字符串

在 Linux 中，可以使用 `grep` 命令来查找某个字符串。以下是一些常见的用法和示例：

假设你有一个文件 `example.txt`，要在其中查找字符串 `hello`：

```sh
grep "hello" example.txt
```

要在当前目录及其子目录中的所有文件中查找字符串 `hello`，可以使用 `-r` 选项：

```sh
grep -r "hello" .
```

## 删除一行

```vim
" 直接删除
dd

" 使用:<行号>d命令：
:3d
```

## linux命令|网路相关命令|`netstat`

`netstat` 是一个网络相关的命令，用于显示网络连接、路由表、接口统计、伪装连接等信息。

- `-a`：显示所有的连接和监听端口。

- `-n`：以数字形式显示地址和端口号，而不是解析为主机名和服务名称。

- `-p`：显示哪个进程正在使用该连接或端口。

  ```bash
  netstat -anp | grep <port>
  ```

### `netstat`能查看的协议

- TCP、UDP
- Unix域套接字：本地进程中Socket通信
- RAW协议：RAW协议是一种基本的传输层协议，提供对IP层的直接访问
- ICMP：ICMP是一种网络层协议，用于发送控制和错误信息

## linux命令|判断远程服务端口开启情况

### 1.ping`**

### 2.telnet`** 测试一个特定端口是否对外开放

   ```bash
   telnet <hostname> <port>
   telnet example.com 80
   ```

#### 退出 `telnet` 会话**

要退出 `telnet` 会话，可以按以下组合键：

- **`Ctrl+]`**：进入 `telnet` 命令提示符。
- 在提示符下输入 `quit` 或者按 `Ctrl+D` 直接断开连接。

### 3.netcat（通常简称为 `nc`）

#### 查看端口绑定状态

```bash
nc <hostname> <port>
nc example.com 80
```

#### 传输文件

```bash
# 发送方
nc -l -p 1234 < file_to_send.txt
# 接收方
nc <sender_ip> 1234 > received_file.txt
```

#### 创建临时服务器

```bash
echo -e "HTTP/1.1 200 OK\n\nHello, World" | nc -l -p 8080
```

在端口 `8080` 上创建一个简单的 HTTP 服务器，返回 `Hello, World` 给连接到该端口的客户端。

### 4.**`nmap`**命令

扫描指定主机的 1000 个最常用的端口 并显示哪些端口是开放的

```bash
nmap 192.168.1.1
```

### 5. `/dev/tcp` 特殊文件

```bash
echo > /dev/tcp/<hostname>/<port>
echo > /dev/tcp/example.com/80 && echo "Port is open"
```

如果远程服务器端⼝是开着的，则会打印“Port is open“，如果没有打开会提示”Connection refused“

## linux命令|查看CPU利用率

`vmstat` 提供了系统的整体性能监控信息，包括 CPU 利用率

```bash
vmstat [options] [delay [count]]
vmstat -n 1
```

执行这个命令后，你会看到类似以下的输出（每秒钟更新一次）：

```bash
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs   us sy id wa st
 1  0      0  74560   6208 149340    0    0     0     1    2    2    1  1 97  0  0
 0  0      0  74624   6208 149340    0    0     0     0   29   39    0  0 100  0  0
 0  0      0  74600   6208 149340    0    0     0     0   20   36    0  0 100  0  0
 0  0      0  74600   6208 149340    0    0     0     0   25   45    0  0 100  0  0
 ...
```

## 波浪号 `~`

波浪号 `~` 用作路径中的一个特殊符号，代表当前用户（即执行命令的用户）的家目录（home directory）。

**示例**：

- 如果当前用户是 `ljm`，那么 `~/.ssh` 就表示 `/home/ljm/.ssh`。

- 如果你以用户 `root` 执行命令，`~` 就代表 `/root`。

  ```bash
  cd ~           # 切换到当前用户的家目录
  ls ~/Documents # 列出当前用户家目录下的 Documents 文件夹内容
  cp file.txt ~   # 将 file.txt 复制到当前用户的家目录
  ```

## linux命令|chmod，scp，chattr，pgrep，awk

1. `chmod` 更改文件或目录的权限。

   ```bash
   # 示例：为文件 example.txt 添加可执行权限
   chmod +x example.txt
   
   # 示例：设置文件 example.txt 的权限为 644（所有者可读写，其他用户只读）
   chmod 644 example.txt
   ```

2. `scp` 安全地复制文件或目录到远程主机或从远程主机复制到本地

   ```bash
   scp [选项] [源文件或目录] [目标]
   # 将本地文件 file.txt 复制到远程主机的 /home/user/ 目录
   scp file.txt user@remotehost:/home/user/
   # 从远程主机复制文件 file.txt 到本地当前目录
   scp user@remotehost:/home/user/file.txt .
   ```

3. `chattr` 更改文件或目录的属性

   ```bash
   chattr [选项] [属性] [文件或目录]
   # 设置文件 important.txt 为不可更改
   chattr +i resolv.conf
   # 取消 important.txt 的不可更改属性 命令防⽌系统中某个关键⽂件被修改
   chattr -i resolv.conf
   ```

4. `pgrep` 根据进程名称或其他属性查找进程的进程 ID

   **`-l`** 列出与模式匹配的进程 ID 及其名称

   **`-a`** 列出所有与模式 nginx 匹配的进程 ID 和完整路径

   ```bash
   pgrep [选项] [模式]
   
   pgrep -l nginx
   1234 nginx
   5678 nginx
   
   pgrep -a nginx
   1234 /usr/sbin/nginx -g daemon on; master_process on;
   5678 /usr/sbin/nginx -g daemon on; master_process on;
   ```

5. `awk` 一个强大的文本处理工具，通常用于模式匹配和数据提取

   ```bash
   awk '脚本' [文件]
   
   # 示例：打印文件 data.txt 的第一列
   awk '{print $1}' data.txt
   
   # 示例：打印文件 data.txt 中第2列大于100的行
   awk '$2 > 100' data.txt
   
   # 示例：统计文件 data.txt 中每个唯一单词的出现次数
   awk '{for(i=1;i<=NF;i++) word[$i]++} END {for(w in word) print w, word[w]}' data.txt
   ```

## 程序在后台运行

1. **加 `&` 符号**

   在命令的末尾加上 `&`，可以将该命令放到后台执行

   ```bash
   # 执行文件
   ./test.py &
   # 执行该命令后，Linux 会立即返回后台任务的 PID
   
   # 查看是否在后台运行
   ps -ef|grep test
    
   # 后台的程序 需要关闭时，需要kill命令停止
   killall [程序名] 
   ```

2. **使用 `nohup`**

   如果要在终端**会话终止后保持任务继续处于活动状态**，`nohup` 就是首选工具。`nohup` 是「no hang up」的缩写，它可以确保命令的不间断执行。

   ```bash
   nohup ./example.sh &
   ```

   `nohup` 会忽略所有 SIGHUP 信号，程序的标准输出和标准错误通常会被重定向到 `nohup.out` 文件中

   ```bash
   nohup example.sh > myoutput.log 2>&1 &
   ```

   ```bash
   ">"		符号用于将命令的`标准输出`重定向到指定的文件中
   "> myoutput.log" 	表示将终端输出保存到`myoutput.log`文件中
   "2>" 	表示将命令的`标准错误`输出重定向到指定的文件中
   "2>&1" 	表示将命令的标准错误输出重定向到标准输出1中一起输出 （ps:标准输出1指的是>outlog.log文件，具体可见下面的内容）
   "&" 	后台挂起
   ```

   [🔗Linux nohup后台启动/ 后台启动命令中nohup 、&、重定向的使用](https://blog.csdn.net/weixin_49114503/article/details/134266408)

3. **使用 screen 在后台运行 Linux 命令和管理会话**

   `screen` 是一个强大的 Linux 实用程序，专为高级会话管理而设计。

   它能够创建多个终端会话，并轻松地在会话之间切换，还可以从当前会话中分离出来以便稍后重新连接

   ```bash
   # 创建一个新的 screen 会话
   screen -S session_name
   # 从当前会话中分离
   Ctrl + a，然后按 d
   # 重新连接到之前分离的会话
   screen -r session_name
   ```

4. **使用 `Ctrl+Z` 暂停进程，并用 `bg` 将其放到后台**

   - 按 **`Ctrl+Z`** 暂停进程

   ```bash
   $ pct
   
   # 键盘输入Ctrl + Z
   ^Z
   [1]+ Stopped    pct
   ```

   - **`jobs`**

     **基本用法**：`jobs` 命令显示当前 shell 会话中所有后台任务的列表及其状态。

     **状态标识**：

     - `+` （加号）：表示当前的作业。
     - `-` （减号）：表示当前作业之后的一个作业。

     **状态**：

     - `Running`：正在运行的任务。
     - `Stopped`：被暂停的任务。
     - `Terminated`：已终止的任务（通常由 `kill` 命令终止）。

   ```bash
   $ jobs -l
   
   [1]+ 12345 Stopped           pct
   [2]- 12346 Running           ping google.com &
   # `[1]+` 表示当前的作业（`pct`）
   # `[2]-` 表示当前作业之后的一个作业（`ping google.com`）
   ```

   - **`bg`** 在后台暂停的命令，变成继续执行

   ```bash
   $ bg %1      # 将作业号为 1 的进程放到后台运行
   $ jobs -l
   [1]+ 12345 Running           pct &
   [2]- 12346 Running           ping google.com &
   ```

   - **`fg`** 将后台中的命令调至前台继续运行

   ```bash
   $ fg %1
   $ jobs -l
   [1]+ 12345 Running           pct
   [2]- 12346 Running           ping google.com &
   ```

## 查找一个字符串是否在filename文件中

```bash
grep string filename
awk '/string/ {print NR":", $0}' filename

$ less filename
$ vi filename
# 在 less 环境中，输入 /string 后按“Enter”，less 会高亮显示第一个匹配项，之后可以按 n 跳转到下一个匹配项，按 N 跳转到前一个匹配项
```

## Git|merge时候遇到conflict怎么办

进行协作开发时，不同分支对同一文件的同一区域进行了修改

Git 会提示你哪些文件存在冲突，并在这些文件中插入冲突标记。需要手动解决冲突，选择保留哪部分代码，或合并两部分代码，然后filename.txt中**删除冲突标记**。重新add commit

```shell
<<<<<<< HEAD
// 你的当前分支中的代码
=======
<<<<<<<
冲突的分支中的代码
>>>>>>> branch-name
```



---

