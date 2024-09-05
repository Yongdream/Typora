# SoftAuth说明以及Nginx配置SSL操作



**SoftAuth** 是一个用于管理授权许可的工具，用户能够通过提供的产品序列号，获取 RGP 激活码，适用于设备授权管理。该项目包含前端界面和后端服务，能够处理 RGF 文件转 RGP 的编码操作，嵌入包括 **SN、Deadline** 在内的关键产品信息，帮助用户高效地管理设备授权。

## 项目结构

本项目分为前端和后端两个部分，项目文件结构保存在名为 **`/softauth`** 的目录下： 

- **`licensing-frontend`**：前端项目，基于 **Vue3** 开发,构建用户界面和单页应用。负责与用户交互，展示授权数据和接收用户查询请求。

  ![image-20240903160346939](./typora_photo/Nginx配置ssl/image-20240903160346939.png) 

- **`licensing-backend`**：后端项目，基于 Flask 框架和纷享销客 API 开发。负责处理业务逻辑、数据库交互和为前端提供数据，具体的 API 使用细节可在[[5]](##ref🔗)中查询。

- **`coderRGF`**：Win端 RGF-RGP 文件转换器，包含编码和解码功能

## 环境准备

- [ ] ### **操作系统：**

  阿里云Linux服务器 **Ubuntu 24.04 LTS**

  公网IP: **`116.62.113.100`**

  **!!!账号密码和端口防火墙开启**请询问管理员

- [ ] ### **后端部分**

- 使用 **python3.8** 环境搭建，需要使用到的库包括flask、request

  推荐使用 **Anaconda** 管理 **python** 虚拟环境[[1]](##ref🔗)

  ```yaml
  # 依赖库：
  - Python 3.8
  - flask 3.0.3
  - requests 2.32.3
  ```

  ```bash
  $ pip3 install flask -i https://pypi.tuna.tsinghua.edu.cn/simple
  $ pip3 install request -i https://pypi.tuna.tsinghua.edu.cn/simple
  ```

- 创建新的虚拟 **python** 环境，**SoftAuth** 指定后端运行环境为 **`scanTech`**

  ```bash
  $ conda activate scanenv
  $ conda env list
  # conda environments:
  #
  base                     /root/anaconda3
  scanenv               *  /root/anaconda3/envs/scanenv
  ```

- [ ] ### **前端部分**

  使用[**Vue3**](https://cn.vuejs.org/guide/quick-start.html)前端框架开发，

  需先安装 [**Node.js**](https://nodejs.org/)，项目中已经包含 **`element-plus ui、axios`** 模块

- [ ] ### CMR白名单添加

  [**纷享销客官网**](https://www.fxiaoke.com/XV/UI/Home#chats)

  添加操作流程如下:现已经添加公网IP

  <img src="./typora_photo/Nginx配置ssl/image-20240903155738115.png" alt="image-20240903155738115" style="zoom:67%;" />

## Nginx配置SSL

使用Nginx搭建HTTPS服务器需要几个步骤，包括安装Nginx、生成或获取SSL证书，以及配置Nginx以使用HTTPS。下面是一个详细的步骤指南：

### Step 1：构建前端应用

```bash
npm install
npm run build
```

`npm run build`命令将根据你的项目配置生成一个用于生产环境的优化版本。生成的文件位于**`dist`**（Vue、Angular）目录中。(React框架保存在`build`)

在你提供的`dist`目录中，有以下文件和文件夹：

```bash
drwxr-xr-x 2 root root 4096 Sep  3 17:11 assets
-rw-r--r-- 1 root root 4286 Sep  3 17:11 favicon.ico
-rw-r--r-- 1 root root  425 Sep  3 17:11 index.html
drwxr-xr-x 2 root root 4096 Sep  3 17:11 static
```

1. **`index.html` 文件**：

- index.html 是前端应用的主HTML文件，也是Nginx或其他Web服务器用来服务前端应用的入口文件。
- 在单页应用程序中，index.html 文件通常会包含一个 `<div id="app"></div>` 或类似的容器元素，Vue框架会在此处挂载整个应用。

2. **`static` 文件夹**：

- static文件夹通常用于存放不需要通过构建工具处理的静态资源，这些资源可以直接被Web服务器提供。
- 这里保存着用于用户下载的存储授权文件**`.RGP`**

### Step 2：Nginx配置

- [ ] #### 安装 Nginx

首先，确保你的包管理器和软件包列表是最新的：

```bash
sudo apt update
```

```bash
sudo apt install nginx -y
```

安装完成后，启动Nginx并设置为开机自启动：

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

- [ ] #### 生成或获取SSL证书

**`pengding...`**

- [ ] #### 配置 Nginx 来服务静态文件

1. **编辑 Nginx 配置文件**

   创建或编辑一个Nginx配置文件以配置Nginx服务你的前端项目。假设你使用的是默认的Nginx配置文件：

   ```bash
   sudo nano /etc/nginx/sites-available/licensing-frontend
   ```

2. **添加或修改配置来服务静态文件**

   确保`root`指令指向构建目录**`/softauth/licensing-frontend`**

   ```nginx
   server {
       listen 80;
       server_name 116.62.113.100;  # 服务器IP或域名
   
       root /softauth/licensing-frontend;  # 指向构建后的文件目录
       index index.html;
   
       location / {
           try_files $uri $uri/ /index.html;  # 确保路由能正确工作
       }
   
       # 可选：访问日志和错误日志路径
       access_log /var/log/nginx/licensing-frontend.access.log;
       error_log /var/log/nginx/licensing-frontend.error.log;
   }
   ```

3. **启用配置并重启Nginx**

   将这个配置文件链接到`sites-enabled`目录中：

   ```bash
   sudo ln -s /etc/nginx/sites-available/licensing-frontend /etc/nginx/sites-enabled/
   ```

   在重新加载 Nginx 之前，测试配置文件是否有语法错误：

   ```bash
   sudo nginx -t
   ```

   如果测试通过，可以重新加载 Nginx 以应用更改：

   ```bash
   sudo systemctl reload nginx
   ```

4. 设置防火墙 (可选)

   如果使用的是`ufw`防火墙，可以允许`Nginx`的HTTPS流量：

   ```bash
   sudo ufw allow 'Nginx Full'
   sudo ufw delete allow 'Nginx HTTP'
   ```

   这样，HTTPS的443端口会被放行，HTTP的80端口会被删除。

### Step 3：测试前端部署

通过访问 `http://116.62.113.100` 来测试Nginx是否将请求正确代理到你的后端服务，是否能成功访问你的 HTTP 网站。

### Step 4: 后端运行部署

1. **激活 Conda 环境**

   首先，确保你已经激活了适当的Python环境。这里的Conda环境名为`scanenv`

   ```bash
   $ conda activate scanenv
   ```

   进入后端项目目录`/softauth/licensing-backend`，确保所有必要的Python文件（如`app.py`、`CRMConn.py`、`encoder.py`等）都在此目录下。

2. **启动 Flask 应用程序**

   使用`nohup`命令在后台启动Flask应用，并将输出重定向到日志文件`output.log`：

   ```bash
   $ nohup python app.py > output.log 2>&1 &
   ```

   - `nohup`命令允许程序在退出终端后继续运行。

   - `python app.py`启动Flask应用程序。

   - `> output.log 2>&1`将标准输出和标准错误输出重定向到`output.log`文件。

   - `&`符号表示在后台运行进程
   
   ```bash
   // 终端显示以下内容，表明进程已在后台启动，46072是运行中的进程ID：
   [1] 46072
   ```

3. **验证 Flask 应用是否在运行**

   ```bash
   $ jobs
   [1]+  Running                 nohup python app.py > output.log 2>&1 &
   ```

## 项目运行演示

1. 打开浏览器，输入以下网址来访问前端应用程序：

   ```apl
   http://116.62.113.100
   ```

2. 输入待获取授权的**产品序列号**，点击**查询**

2. **RGP下载**

   ![image-20240904132748580](./typora_photo/Nginx配置ssl/image-20240904132748580.png)

---

## ref🔗

1. [使用Anaconda管理多个版本的Python环境](https://blog.csdn.net/cai454692590/article/details/84672385)
2. [Ubuntu 安装Nginx服务](https://gitcode.csdn.net/66275b74a2b051225565dfd7.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MjI4NTIyLCJleHAiOjE3MjUwMDg3NDUsImlhdCI6MTcyNDQwMzk0NSwidXNlcm5hbWUiOiJ3ZWl4aW5fNDUzNjc0MzQifQ.26pAnBBKoSTfdkbLVzXpcykearcWEt21VCFyyuymEqY)
3. [小白*进阶ing|nginx安装、使用及搭建nginx服务器](https://blog.csdn.net/qq_44948213/article/details/122308840)
4. [解决ssh登录后闲置时间过长而断开连接](https://developer.aliyun.com/article/586175#:~:text=%E7%AE%80%E4%BB%8B%EF%BC%9A%20%E8%A7%A3%E5%86%B3ssh%E7%99%BB%E5%BD%95%E5%90%8E%E9%97%B2%E7%BD%AE%E6%97%B6%E9%97%B4%E8%BF%87%E9%95%BF%E8%80%8C%E6%96%AD%E5%BC%80%E8%BF%9E%E6%8E%A5%20%E6%97%B6%E6%88%91%E4%BB%AC%E9%80%9A%E8%BF%87%E7%BB%88%E7%AB%AF%E8%BF%9E%E6%8E%A5%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%97%B6%EF%BC%8C%E5%BD%93%E9%BC%A0%E6%A0%87%E5%92%8C%E9%94%AE%E7%9B%98%E9%95%BF%E6%97%B6%E9%97%B4%E4%B8%8D%E6%93%8D%E4%BD%9C%EF%BC%8C%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B0%B1%E4%BC%9A%E8%87%AA%E5%8A%A8%E6%96%AD%E5%BC%80%E8%BF%9E%E6%8E%A5%EF%BC%8C%E6%88%91%E4%BB%AC%E8%BF%98%E7%9A%84%E9%9C%80%E8%A6%81%E9%87%8D%E6%96%B0%E8%BF%9E%E6%8E%A5%EF%BC%8C%E6%84%9F%E8%A7%89%E5%BE%88%E9%BA%BB%E7%83%A6%EF%BC%8C%E6%80%BB%E7%BB%93%E4%B8%80%E4%B8%8B%E8%A7%A3%E5%86%B3%E6%AD%A4%E9%97%AE%E9%A2%98%E7%9A%84%E6%96%B9%E6%B3%95%E6%96%B9%E6%B3%95%E4%B8%80%E3%80%81%E4%BF%AE%E6%94%B9%2Fetc%2Fssh%2Fsshd_config%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%EF%BC%8C%E6%89%BE%E5%88%B0ClientAliveCountMax%EF%BC%88%E5%8D%95%E4%BD%8D%E4%B8%BA%E5%88%86%E9%92%9F%EF%BC%89%E4%BF%AE%E6%94%B9%E4%BD%A0%E6%83%B3%E8%A6%81%E7%9A%84%E5%80%BC%EF%BC%8C%E6%89%A7%E8%A1%8Cservice,sshd%20reload%20%E6%96%B9%E6%B3%95%E4%BA%8C%E3%80%81%E6%89%BE%E5%88%B0%E6%89%80%E5%9C%A8%E7%94%A8%E6%88%B7%E7%9A%84.)
5. [纷享开放平台开发手册](https://open.fxiaoke.com/wiki.html#artiId=169)