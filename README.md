# DERP Docker

这是一个用于运行 Tailscale 的 DERP (Designated Encrypted Relay for Packets) 服务器的 Docker 镜像。DERP 服务器用于在 Tailscale 网络中处理客户端之间无法直接建立连接时的数据中继。

## 功能特点

- 基于最新版本的 Go 和 Ubuntu
- 支持 Let's Encrypt 自动证书管理
- 支持 STUN 服务器功能
- 可配置的客户端验证
- 灵活的环境变量配置

## 环境变量

| 环境变量 | 描述 | 默认值 |
|----------|------|--------|
| DERP_DOMAIN | DERP 服务器的域名 | your-hostname.com |
| DERP_IP | DERP 服务器的 IP 地址 | 1.1.1.1 |
| DERP_CERT_MODE | 证书模式 | letsencrypt |
| DERP_CERT_DIR | 证书存储目录 | /app/certs |
| DERP_ADDR | 服务监听地址 | :443 |
| DERP_STUN | 是否启用 STUN 服务 | true |
| DERP_STUN_PORT | STUN 服务端口 | 3478 |
| DERP_HTTP_PORT | HTTP 服务端口 | 80 |
| DERP_VERIFY_CLIENTS | 是否验证客户端 | false |
| DERP_VERIFY_CLIENT_URL | 客户端验证 URL | "" |

## 使用方法

### 使用 Docker 运行

```bash
docker run -d \
  -p 443:443 \
  -p 3478:3478/udp \
  -p 80:80 \
  -e DERP_DOMAIN=your-domain.com \
  -e DERP_IP=your-server-ip \
  -v /path/to/certs:/app/certs \
  your-image-name
```

### 使用 Docker Compose

创建 `docker-compose.yml` 文件：

```yaml
version: '3'
services:
  derp:
    build: .
    ports:
      - "443:443"
      - "3478:3478/udp"
      - "80:80"
    environment:
      - DERP_DOMAIN=your-domain.com
      - DERP_IP=your-server-ip
    volumes:
      - ./certs:/app/certs
```

然后运行：

```bash
docker-compose up -d
```

## 构建说明

要构建特定版本的 DERP 服务器，可以使用 `DERP_VERSION` 构建参数：

```bash
docker build --build-arg DERP_VERSION=v1.x.x .
```

## 注意事项

1. 确保您的域名已正确配置 DNS 记录，指向服务器 IP
2. 使用 Let's Encrypt 模式时，需要确保 80 端口可访问
3. 建议在生产环境中使用持久化存储来保存证书

## 许可证

此项目遵循 MIT 许可证。

## 相关链接

- [Tailscale 官方文档](https://tailscale.com/kb/)
- [DERP 协议说明](https://tailscale.com/blog/how-tailscale-works/) 