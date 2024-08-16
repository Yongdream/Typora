## 单终端双git账号

> 对于在一台电脑上配置多个Git账号的需求，使用场景是非常常见的。很多开发者会在不同的平台（如GitLab, GitHub, Gitee等）上拥有多个账号用于不同的目的，例如工作和个人学习。假设读者已经安装并配置好 Git，并且在本地拥有一个 Git 账号
>
> 下面是一些关于如何在同一台终端上配置和管理多个Git账号的提示，做下记录：

1. **删除全局Git配置**：如果之前配置了全局的用户名称和邮箱，需要先清除这些设置，以免它们影响到特定仓库的配置。

   - 查看global配置

   ```bash
   git config --global --l
   ```

   - 释放全局名字和邮箱变量

   ```bash
   git config --global --unset user.name
   git config --global --unset user.email
   ```

2. **生成新的SSH密钥**：为每个Git账户生成一个新的SSH密钥。

   在生成密钥时，可以为每个密钥指定一个独特的文件名 **`id_rsa_github`**。

   ```bash
   ssh-keygen -t rsa -C "your_email@example.com" -f ~/.ssh/id_rsa_github
   ```

   转到指定生成公私钥的目录 `~/.ssh`

3. **将公钥添加到各个Git平台**：打开 `id_rsa_github.pub` 文件并复制里面的内容。登录到Git平台，选择 `Settings`，将生成的公钥添加到对应账户的SSH keys设置中。

4. **配置SSH config文件**：新建`~/.ssh/config`文件，为每个账户设置配置，这包括指定每个账户的HostName、User、IdentityFile等。

   ```yaml
   # GitHub Account
   Host HostNameA
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_rsa_github
   
   # GitLab Account
   Host HostNameB
     HostName gitlab.com
     User git
     IdentityFile ~/.ssh/id_rsa_gitlab
   ```

   **配置文件说明**

   ```ini
   # Host：这是一个别名，用于在Git命令中指定要使用的配置，比如git clone git@gitlab:repo.git中的gitlab就是Host的别名。
   # HostName：这是Git服务器的实际地址，比如gitlab.com、github.com等。
   # User：Git服务器的登录用户名，对于大多数Git服务器，如GitHub和GitLab，这里通常是git。
   # IdentityFile：这是SSH密钥文件的路径，用于身份验证。它指向你为特定Git账户生成的私钥文件。
   ```

   配置完成后，Git 和 SSH 将自动使用你为每个主机指定的配置。

5. **测试SSH连接**：确保每个SSH连接都可以正确工作。

   ```bash
   ssh -T git@HostNameA
   ssh -T git@HostNameB
   # 正确工作输出
   > Hi <Your GitHub Username>! You've successfully authenticated, but GitHub does not provide shell access.
   > Hi <Your Gitlab Username>! You've successfully authenticated, but Gitlab does not provide shell access.
   ```

6. **项目级别的用户信息配置**：在每个具体的项目目录下，可以根据需要设置特定的用户信息。

   ```bash
   git config user.name "Your Name"
   git config user.email "your_email@example.com"
   ```

   当然这也可以在vscode添加字段**`[user]`**

   ![image-20240815140955319](.\typora_photo\单PC双Git配置\image-20240815140955319.png)

7. **git clone 下载**

   - 将原来github.com替换为Host别名

   ```bash
   git clone git@github.com:user/demo.git
   git clone git@HostNameA:user/demo.git
   ```

   不然会出现以下错误

   ```bash
   git@github.com: Permission denied (publickey). fatal: Could not read from remote repository. 
   Please make sure you have the correct access rights and the repository exists.
   ```

​	