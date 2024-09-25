# Flask 应用跨域问题及 Nginx 配置解决方案

[TOC]

## 什么是跨域？

跨域是指在不同的源之间进行请求。根据浏览器的同源策略，同源是指协议、域名和端口必须完全相同。当一个网页试图从不同的域名、端口或协议请求资源时，就会产生跨域问题。

例如，假设前端应用运行在 `http://example.com`，而它尝试访问 `http://api.example.com`，这就构成了跨域请求。

由于安全原因，浏览器会拦截这些跨域请求，导致前端无法获取所需的数据。这种限制可以通过跨源资源共享（CORS）等机制来解决。

跨域请求会受到浏览器的同源策略限制。浏览器会拦截不符合同源策略的请求，导致跨域请求失败。

为了解决这个问题，可以使用以下方法：

## 1.CORS（跨源资源共享）：

在服务器上设置响应头，允许特定来源的请求。Flask 可以通过 `flask-cors` 库来配置。

```python
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)  # 允许来自所有域的请求
```

虽然在后端使用 CORS 是一种有效的方法，但在某些情况下，跨域请求可能在到达后端之前就被浏览器拦截。因此，更好的做法是使用相对路径进行请求。这可以确保请求被正确发送到目标服务器，减少跨域问题的发生。

## 2.前端使用相对路径请求

在前端代码中，可以使用绝对路径来直接指定 API 的完整 URL。例如：

```javascript
import axios from 'axios';

const queryInstance = axios.create({
    baseURL: '/api/',  // 使用绝对路径
    headers: {
        Accept: 'application/json',
        'Content-Type': 'application/json',
    },
});

export default {
    queryData(serialNumber) {
        return queryInstance.get('/query', {
            params: { serialNumber }
        });
    }
};
```

在确保 CORS 和绝对路径的解决方案后，接下来需要配置 Nginx 以正确转发 API 请求到后端应用。这一步非常关键，因为如果 Nginx 没有正确配置，仍然会导致请求无法到达 Flask 应用。

## 3.Nginx配置

```nginx
server {
    listen 443 ssl;  # 启用 SSL
    server_name your-server-name;

    # SSL 证书路径
    ssl_certificate /etc/nginx/ssl/your_certificate.pem;
    ssl_certificate_key /etc/nginx/ssl/your_certificate_key.key;

    root /path/to/your/frontend;  # 前端构建后的文件目录
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;  # 确保单页应用路由能正确工作
    }

    location /api/ {
        proxy_pass http://127.0.0.1:your_flask_port/api/;  # 转发到 Flask 应用
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 路径映射原理

1. **发起请求**：
   当调用 `queryData` 函数并传入参数时，Axios 会发起请求 `GET /api/query?serialNumber=example`。

2. **Nginx 请求处理和代理转发**：

   在 Nginx 中配置 `proxy_pass` 时，通常我们将其设置为与匹配路径保持一致的根路径

   Nginx 接收到这个请求后，根据配置匹配到 location /api/，并将请求转发到 Flask 应用。

   在转发请求时，Nginx 会去掉请求路径中的 /api/，剩下的路径 /query 将直接附加到 `proxy_pass` 指定的基础路径上。

3. **请求路径组合**：

   最终转发给 Flask 的请求将是 `http://127.0.0.1:your_flask_port/api/query?serialNumber=example`。这样 Flask 就能够正确处理请求并返回相应的数据。

**注意斜杠的使用**：

- 如果使用 `proxy_pass http://127.0.0.1:your_flask_port/api/query/;`，Nginx 将会将整个请求路径 `/api/query` 发送到 `/api/query/`，这可能导致路径不匹配。
- 为了确保路径正确，`proxy_pass` 后的路径必须以斜杠结尾（如 `/api/`），这样 Nginx 可以正确地组合请求。

---

## 配置Nginx以正确转发API请求到后端应用

**`其实了解了路径映射原理后，后面的问题就能一步到位解决了，但我这属于上帝视角了，还是记录一下整个排查过程`**

> [**博客园|检查是否存在跨域问题**](https://www.cnblogs.com/yxxcl51/p/17515991.html#:~:text=%E6%9F%A5%E7%9C%8B%E6%8E%A7%E5%88%B6%E5%8F%B0%E9%94%99%E8%AF%AF%EF%BC%9A)

1. **初步发现跨域问题**：
   在 F12 开发者工具的网络选项卡中，发现请求中出现了 `"Access-Control-Allow-Origin"`，表明存在跨域问题。

2. **初步处理**：
   将 `baseURL` 改为相对路径以处理跨域问题：

   ```javascript
   baseURL: 'https://your-api-domain/api/',
   baseURL: '/api/',  // 使用相对路径
   ```

3. **404 错误检查**：
   在 F12 开发者工具中，看到请求发生 404 错误。使用 `curl` 请求 IP 路径时能正常返回，这表明 Flask 应用本身工作正常：

   ```bash
   curl -G "http://116.62.113.100:9680/api/query" --data-urlencode "serialNumber=test123"  # 成功
   curl -G "http://your-api-domain/api/query" --data-urlencode "serialNumber=test123"  # 失败
   ```

4. **Nginx 配置检查**：
   检查 Nginx 的 API 路由设置，尤其是 `location /api/` 的代理配置。

   通过后端和 Nginx 日志，发现来自域名路径的请求变成了 `GET /query?serialNumber=test123 HTTP/1.0`，没有 `/api/` 前缀。

   ```bash
   tail -f /var/log/nginx/project.access.log
   tail -f backend/output.log
   ```

   **`可以断定是 Nginx 配置出现问题。`**

5. **修正 Nginx 配置**：
   原配置为：

   ```nginx
   location /api/ {
       proxy_pass http://127.0.0.1:9680/;  # 不正确
   }
   ```

   关键在于保持路径一致性：
   - 前端代码使用相对路径：
     ```javascript
     const queryInstance = axios.create({
         baseURL: '/api/', // 发送请求时自动加上这个前缀
     });
     ```
   - Flask 应用中定义的路由：
     ```python
     @app.route('/api/query', methods=['GET'])
     def query():
         ...
     ```

   发现 Nginx 的 `location /api/` 配置存在问题，`proxy_pass` 设置不正确。需要修改为：
   ```nginx
   location /api/ {
       proxy_pass http://127.0.0.1:9680/api/; 
   }
   ```
   这样，Nginx 会将 `/api/query` 转发到 Flask 的 `/query`，确保请求能够正确匹配。

6. **请求日志验证**：
   通过日志确认请求是否正确转发：

   ```plaintext
   127.0.0.1 - - [24/Sep/2024 16:30:37] "GET /api/query?serialNumber=SK04UT050209 HTTP/1.1" 200 -
   ```


## 写在最后

如何操作（"知其然"），还要了解为什么这样做（"自其所以然"）！