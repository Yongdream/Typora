## `CRM`软授权测试修改入口

![image-20240807101353472](C:\Users\Ouser\AppData\Roaming\Typora\typora-user-images\image-20240807101353472.png)

![image-20240807101416463](C:\Users\Ouser\AppData\Roaming\Typora\typora-user-images\image-20240807101416463.png)

![image-20240807101427861](C:\Users\Ouser\AppData\Roaming\Typora\typora-user-images\image-20240807101427861.png)

---

## 加密软件导致 `node_modules` 读取乱码

![image-20240807155208266](.\typora_photo\3dScan\image-20240807155208266.png)

- 为什么使用 Python 脚本无法正常查看
  
  加密软件通常在操作系统内核和应用程序之间插入一个中间层，以实现对文件的加密和解密。这种设计允许加密软件在文件被写入磁盘时对其进行加密，在文件被读取时对其进行解密，从而确保数据在存储时的安全性。
  
  `VSCode `是一个图形化应用程序，可能在处理文件 I/O 时调用了操作系统的高级 `API`，这些 `API `会自动与加密软件的过滤驱动协同工作，确保文件在读取时被正确解密。
  
  Python 脚本在读取文件时，可能直接使用了低级文件 I/O 操作，如 `open` 函数。这些操作有时可能不会触发加密软件的解密过程，特别是在处理二进制文件。
  
  Python 默认使用的编码（如 `utf-8`）可能不适用于被加密软件修改过的文件内容，从而导致编码错误或乱码。

---

## SSH远程管理与配置

### 初识 `SSH`

1. SSH（Secure Shell）

   - 一种安全通道协议，主要用来实现字符界面的远程的登录、远程复制等功能
   - SSH通过加密来保护数据传输，包括在登录过程中输入的密码，确保了通信的安全性。

2. **`sshd `(SSH Daemon)**:

   - `sshd` 是 SSH 服务端程序，即守护进程（daemon）。
   - 它在服务器上运行，监听来自客户端（通常是 `ssh` 客户端）的连接请求。
   - `sshd` 负责认证、加密和维护与客户端的安全连接。
   - 它通常配置在 `/etc/ssh/sshd_config` 文件中，并作为系统服务在后台运行。

3. **`SSH`** 服务的管理和配置

   - **启动SSH服务**

   ```bash
   sudo systemctl start sshd
   ```

   - **SSH服务的端口号**

     - **默认端口号**：SSH服务默认监听端口号为**22**。

     - **更改端口**：出于安全考虑，系统管理员有时会更改默认端口以避免自动化的扫描和攻击。你可以在`sshd_config`文件中设置新的端口号：

   ```c++
   # 要将SSH服务端口更改为2222
   Port 2222
   # 修改端口后，需要重启SSH服务使设置生效
   sudo systemctl restart sshd
   ```

   - **SSH服务的配置文件**

     **`sshd_config`**：这是服务端的配置文件，通常位于`/etc/ssh/sshd_config`

     **`ssh_config`**：这是客户端的配置文件，通常位于`/etc/ssh/ssh_config`或用户的家目录下的`.ssh/config`

### 配置`OpenSSH`服务端

- **`sshd_config`配置文件的常用选项**

  ```bash
  vim /etc/ssh/sshd_config
  ```

  ```toml
  Port 22 								#监听端口为22
  ListenAddress 0.0.0.0 					#监听地址为任意网段，也可以指定OpenSSH服务器的具体IP
  
  LoginGraceTime 2m 						#登录验证时间为2分钟
  PermitRootLogin no 						#禁止root用户登录
  MaxAuthTries 6 							#最大重试次数为 6
  
  PermitEmptyPasswords no 				#禁止空密码用户登录
  UseDNS no 								#禁用 DNS 反向解析，以提高服务器的响应速度
  
  # 只允许zhangsan、lisi、wangwu用户登录，
  # 且其中wangwu用户仅能够从IP地址为61.23.24.25 的主机远程登录
  AllowUsers zhangsan lisi wangwu@61.23.24.25 					#多个用户以空格分隔
  # 禁止某些用户登录，用法于 AllowUsers 类似（注意不要同时使用）
  DenyUsers zhangsan
  
  Allowusers……    #仅允许某某用户登陆
  Denyusers ……    #禁止某些用户登录，用法于AllowUsers 类似（注意不要同时使用）
  ```

  Tip：配置文件的格式有 **`ini`** **`XML`** **`JSON`** **`TOML`** 等

### 使用SSH客户端程序

### `sshd`服务支持验证方式

**`sshd`**支持多种验证方式来确保只有授权用户才能访问服务器

1. **密码认证（Password Authentication）**：
   - 最基本的认证方式，用户需要输入用户名和密码来登录。
   - 密码在网络上传输可能被截获，容易受到中间人攻击或暴力破解攻击
2. **公钥/私钥认证（Public Key Authentication）**：
   - 用户**生成一对密钥（公钥和私钥）**，公钥存储在服务器上，私钥保留在用户端。
   - 登录时，服务器会向用户发送一个随机数，用户用自己的私钥对其进行加密并发送回服务器，服务器使用公钥解密，如果成功，则认证通过
   - 需要额外的密钥生成和管理步骤，如果私钥丢失或泄露，将无法登录

#### 公钥/私钥认证认证过程

1. 在客户端创建密码获得一对密钥

   ```bash
   useradd yang7hi	# 创建新用户yang7hi:
   echo "123456" | passwd --stdin yang7hi	# 为用户yang7hi设置密码
   su - yang7hi	# 切换到用户yang7hi
   
   ssh-keygen -t ecdsa	# 生成 ECDSA 密钥对
   ```

   ```bash
   Generating public/private ecdsa key pair.
   Enter file in which to save the key (/home/yang7hi/.ssh/id_ecdsa): 	#指定私钥位置，直接回车使用默认位置
   Created directory '/home/yang7hi/.ssh'.		    #生成的私钥、公钥文件默认存放在宿主目录中的隐藏目录.ssh/下
   Enter passphrase (empty for no passphrase): 	#设置私钥的密码
   Enter same passphrase again: 					#确认输入
   ```

   ```bash
   ls -l .ssh/id_ecdsa*      #id_ecdsa是私钥文件，权限默认为600；id_ecdsa.pub是公钥文件，用来提供给 SSH 服务器
   ```

   完成这些步骤后，你将拥有一对 `ECDSA `密钥，可以安全地用于 SSH 认证。

2. 需要将公钥复制到远程服务器

   可以使用 `scp` 和 `ssh-copy-id` 命令：

   ```bash
   scp ~/.ssh/id_ecdsa.pub root@192.168.184.20:/opt
   # 使用 scp 仅复制文件，不自动将公钥追加到 authorized_keys 文件中
   
   cd ~/.ssh/
   ssh-copy-id -i id_ecdsa.pub yang7hi@192.168.184.20
   # 希望自动化地设置无密码SSH登录时，ssh-copy-id 会自动处理公钥的追加和权限的设置。
   # ssh-copy-id 默认使用 ~/.ssh/id_rsa.pub 作为公钥文件，使用 -i 选项可以指定其他公钥文件
   ```

3. **手动导入公钥到 `authorized_keys`**（可选，`scp `仅复制文件，不会导入公钥文本）

   ```bash
   mkdir -p ~/.ssh
   cat /tmp/id_ecdsa.pub >> ~/.ssh/authorized_keys
   ```

4. **查看 `authorized_keys` 文件**

   ```bash
   cat ~/.ssh/authorized_keys
   less ~/.ssh/authorized_keys	# 更易读的格式
   ```

5. **权限设置**

   检查远程服务器上 `~/.ssh` 目录和 `authorized_keys` 文件的权限设置是否正确。

   通常，`~/.ssh` 目录的权限应该是 `700`，`authorized_keys` 文件的权限应该是 `600`

   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

   - `700` 意味着只有所有者可以**读取**、**修改**目录中的文件，可以**执行**目录中的程序，但没有其他人可以访问该目录 *`rwx`(4+2+1,0,0)*
   - `600` 意味着只有所有者可以读取和修改该文件，其他人不能读取或修改这个文件。

6. **使用SSH连接**

   ```bash
   ssh yang7hi@remote_host
   yang7hi@remote_host password:          #输入私钥的密码
   ```

7. **使用SSH代理**

   ```bash
   # 启动SSH代理并设置环境变量，使其在当前shell会话中有效
   eval $(ssh-agent bash)	# eval 用于执行命令替换输出的shell命令
   
   # 添加私钥到SSH代理
   ssh-add ~/.ssh/id_ecdsa
   # 输入私钥密码
   Enter passphrase for ~/.ssh/id_ecdsa:
   
   # 再次使用SSH连接
   ssh yang7hi@remote_host
   ```

## TCP Wrappers

TCP Wrappers 是 Linux 系统中提供的一种访问控制机制，其主要作用是基于客户端的 `IP `地址**`对特定的服务进行访问控制`**，相当于我们手机的黑白名单。

#### 工作原理

TCP Wrappers 在传输层（OSI 模型的第 4 层）工作。它通过拦截传入的网络连接请求，检查配置的访问控制策略，来决定是否允许或拒绝该请求。TCP Wrappers 通过两个主要的配置文件 `/etc/hosts.allow` 和 `/etc/hosts.deny` 来实现对连接的管理。

- **允许策略**：当服务程序接收到连接请求时，TCP Wrappers 会首先检查 `/etc/hosts.allow` 文件，如果找到匹配的规则，则允许访问。
- **拒绝策略**：如果没有找到匹配的规则，则检查 `/etc/hosts.deny` 文件，如果找到匹配规则，则拒绝访问。
- **默认策略**：如果两个文件中都没有匹配的规则，默认情况下是允许访问的。
- **允许个别，拒绝所有**：除了在 `/etc/hosts.allow` 中添加允许策略外，还需要在 `/etc/hosts.deny` 文件中设置 `“ALL:ALL”` 的拒绝策略。

### 核心库 `libwrap`

**`libwrap `**是 TCP Wrappers 实现的核心，它是一个库文件（通常为 `libwrap.so.*`），提供了对网络服务进行访问控制的功能。当服务程序在编译时链接了这个库，它就能够利用 TCP Wrappers 进行安全检测和访问控制。凡是包含有`libwrao.so`库文件的程序就可以受 TCP wrappers 的管理。

要检查系统中的某个服务是否支持 TCP Wrappers，可以使用 `ldd` 命令来查看该服务程序是否使用了 `libwrap.so` 这个动态链接库。

- 检查 `sshd `服务是否支持 TCP Wrappers

  ```bash
  ldd /usr/sbin/sshd | grep libwrap
  ```

  如果输出中包含 `libwrap.so.0`，则表示该服务支持 TCP Wrappers。

- 查询 RPM 数据库来获取使用 `libwrap.so.0` 这个动态库的软件包列表

  ```bash
  rpm -q --whatrequires libwrap.so.0
  ```

  这将列出所有依赖 `libwrap.so.0` 的软件包，从而可以了解哪些服务支持 TCP Wrappers

### 策略的配置格式

```bash
[服务程序列表]:[客户端地址列表]
```

<table><thead>
  <tr>
    <th colspan="3">策略的配置格式</th>
  </tr></thead>
<tbody>
  <tr>
    <td rowspan="3">服务程序列表</td>
    <td>ALL</td>
    <td>代表所有服务</td>
  </tr>
  <tr>
    <td>单个服务程序</td>
    <td>例如：vsftpd</td>
  </tr>
  <tr>
    <td>多个服务程序组成的列表</td>
    <td>例如 `vsftpd,ssh, http`，规则将适用于多个服务，名称由逗号分隔</td>
  </tr>
  <tr>
    <td rowspan="5">客户端地址列表</td>
    <td>ALL</td>
    <td>所有地址，如果你想允许或拒绝所有客户端的访问</td>
  </tr>
  <tr>
    <td>LOCAL</td>
    <td>本机地址，即允许或拒绝来自本地机器的访问</td>
  </tr>
  <tr>
    <td>单个IP地址</td>
    <td>例如 192.168.8.5，指定单个 IP 地址的访问</td>
  </tr>
  <tr>
    <td>网段地址</td>
    <td>例如 192.168.4.0/255.255.255.0 CIDR 表示法应为 192.168.4.0/24</td>
  </tr>
  <tr>
    <td>多个客户端地址组成的列表</td>
    <td>之间用逗号分隔</td>
  </tr>
</tbody>
</table>
### 配置实例

下面是一个配置实例，展示了如何设置特定服务和客户端的访问策略：

1. 在 `/etc/hosts.allow` 文件中允许特定服务的特定客户端访问：

   ```bash
   vi /etc/hosts.allow
   sshd: 192.168.1.100
   vsftpd: 192.168.1.0/24
   ```

2. 在 `/etc/hosts.deny` 文件中拒绝所有其他访问：

   ```bash
   vi /etc/hosts.deny
   ALL: ALL
   ```

   在这个配置中，只有来自 `192.168.1.100` 的客户端可以访问 SSH 服务，`192.168.1.0/24` 网段的客户端可以访问 FTP 服务（vsftpd）。其他任何访问请求都会被拒绝。

---

## 无分类编值(`CIDR`)表示法

### 传统模式（有类别的IP地址划分）

在IP地址的早期分配中，地址被分为A、B、C三类，每类具有不同的网络和主机部分的长度：

- **A类**：第一个八位字节范围从1到126，后三个八位字节作为主机号。例如，`10.0.0.0`是A类地址，网络部分是第一个字节`10`，主机部分是后三个字节。
- **B类**：第一个八位字节范围从128到191，前两个字节作为网络号，后两个字节作为主机号。例如，`172.16.0.0`是B类地址。
- **C类**：第一个八位字节范围从192到223，前三个字节作为网络号，最后一个字节作为主机号。例如，`192.168.1.0`是C类地址。

**缺点：**

1. **不灵活**：固定的网络和主机位长使得地址分配不够灵活，无法满足不同规模网络的需求。
2. **地址浪费**：由于固定的网络和主机位划分，很多地址被分配给了不需要那么多主机地址的大型网络，造成地址空间的浪费。
3. **路由表膨胀**：随着网络数量的增加，路由器的路由表迅速增长，导致路由效率降低。

### CIDR（Classless Inter-Domain Routing）

CIDR 表示法是一种用于表示IP地址和它们的子网掩码的方法。CIDR 提供了一种更灵活的方式来定义网络的大小，与传统的A、B、C类地址划分相比，CIDR 允许更有效地分配和使用IP地址空间。

1. **无类别限制**：CIDR消除了传统的A、B、C类地址的限制，允许任意长度的网络前缀。
2. **更有效的地址分配**：CIDR允许基于网络的具体需求来分配地址，减少了地址空间的浪费。
3. **路由聚合**：CIDR允许多个网络地址块聚合为一个单一的路由，简化了路由表，提高了路由效率。

### 基本组成部分：

1. **IP地址**：这是标准的点分十进制格式的IP地址，如 `192.168.1.0`。

2. **子网掩码长度**：一个正斜杠 `/` 后面跟着一个数字，表示网络部分的位数。**例如 `/24` 表示前24位是网络地址，剩下的8位是主机地址。**

### 计算逻辑

让我们通过一个具体的例子来详细说明CIDR的计算逻辑。假设我们有一个IP地址 **`192.168.1.64`** 和一个CIDR表示法 **`/26`**，我们将计算这个网络的相关参数。

1. **确定网络前缀长度**

- `/26` 表示网络前缀长度为26位。这意味着IP地址的前26位是网络部分，剩下的6位是主机部分。

2. **转换为子网掩码**
- 将26位网络前缀转换为子网掩码，得到 `255.255.255.192`。这是因为前26位都是1，转换成十进制就是：
  ```ini
  11111111.11111111.11111111.11000000
  ```

3. **计算网络地址**
- 要找到网络地址，我们保留IP地址 `192.168.1.64` 的**前26位**，并将主机部分的位清零。
  
- 由于 `64` 的二进制是 `01000000`，它的前6位（主机部分）是000000，因此：
  
  ```ini
  192.168.1.64 的二进制： 11000000.10101000.00000001.01000000
  网络地址（清零主机部分）： 11000000.10101000.00000001.00000000（即192.168.1.0）
  ```

4. **计算广播地址**
- 广播地址是将**网络地址的主机部分所有位设置为1得到的**。因此，对于网络 `192.168.1.0`（二进制中的主机部分是000000），我们将其转换为：
  ```ini
  主机部分全为1： 11000000.10101000.00000001.00111111（即192.168.1.63）
  ```

5. **确定IP地址范围**
- 网络地址是 `192.168.1.0`。
- 广播地址是 `192.168.1.63`。
- 这意味着在这个网络中，从 `192.168.1.0` 到 `192.168.1.63` 的所有IP地址都属于这个网络。注意，这个范围内的地址包括网络地址和广播地址。

6. **可用的主机地址数量**
  - 由于网络地址和广播地址不能分配给主机，可用的主机地址数量是 `2^(主机位数) - 2`。

  - **主机位数 = 子网掩码中主机部分为0的个数** 也即 32 - 子网掩码长度

  - 在这个例子中，是 `2^6 - 2 = 64 - 2 = 62` 个可用的主机地址。 

    192.168.1.64/26 表示 192.168.1.0 到 192.168.1.63 的IP地址范围。其中 `/26` 表示网络部分占用了前26位，剩下的6位用于主机。

- `10.0.0.0/8`：表示 `10.0.0.0` 到 `10.255.255.255` 的IP地址范围，其中 `/8` 表示网络部分占用了前8位，剩下的24位用于主机。这个网络可以有 `256 * 256 = 65,536` 个可能的IP地址。

CIDR 地址表示方法通过结合基础 IP 地址和前缀长度，提供了一种有效的方法来描述 IP 地址块的大小和位置。它在现代网络中广泛应用，为路由和 IP 地址管理提供了极大的灵活性和效率。冲突处理scan

## 部署Python工程

[在服务器上安装python3.8.2环境](https://blog.csdn.net/qq_45163122/article/details/105725033)

[远程服务器安装Anaconda并创建conda环境（超详细）](https://blog.csdn.net/weixin_60701343/article/details/140529674)

---



















