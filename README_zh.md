![](https://github.com/ngoduykhanh/wireguard-ui/workflows/wireguard-ui%20build%20release/badge.svg)

# wireguard-ui

一个用于管理 WireGuard 配置的 Web 用户界面。

## 功能

- 友好的图形界面
- 登录认证
- 维护额外的客户端信息（姓名、邮箱等）
- 通过二维码 / 文件 / 邮件获取客户端配置

![wireguard-ui 0.3.7](https://user-images.githubusercontent.com/37958026/177041280-e3e7ca16-d4cf-4e95-9920-68af15e780dd.png)

## 运行 WireGuard-UI

> ⚠️ 默认用户名和密码均为 `admin`。请务必修改以保障安全。

### 使用二进制文件

从发布页下载二进制文件，在宿主机上直接运行：

```
./wireguard-ui
```

### 使用 docker compose

[examples/docker-compose](examples/docker-compose) 目录包含多个 docker-compose 示例。
选择最适合自己的示例，按需调整配置后执行：

```
docker-compose up
```

## 环境变量

| 变量 | 说明 | 默认值 |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| `BASE_PATH` | 当通过反向代理的子路径（例如 /wireguard）访问 wireguard-ui 时设置此变量 | N/A |
| `BIND_ADDRESS` | 可访问 Web 界面的地址与端口；如需 Unix Domain Socket，使用 `unix:///绝对路径/file.socket` | 0.0.0.0:80 |
| `SESSION_SECRET` | 用于加密会话 Cookie 的密钥，请设置为随机值 | N/A |
| `SESSION_SECRET_FILE` | 会话 Cookie 密钥的可选文件路径。需将 `SESSION_SECRET` 置空后方可生效 | N/A |
| `SESSION_MAX_DURATION` | 记住登录的最大刷新天数。未刷新会话最多有效 7 天，与此设置无关 | 90 |
| `SUBNET_RANGES` | 地址分段范围列表。格式：`SR Name:10.0.1.0/24; SR2:10.0.2.0/24,10.0.3.0/24`，每个 CIDR 必须位于某个服务器网卡范围内 | N/A |
| `WGUI_USERNAME` | 登录页用户名，仅在初始化数据库时使用 | `admin` |
| `WGUI_PASSWORD` | 登录页用户密码，初始化时自动哈希，仅在初始化数据库时使用 | `admin` |
| `WGUI_PASSWORD_FILE` | 登录密码的可选文件路径。需将 `WGUI_PASSWORD` 置空后生效，仅在初始化时使用 | N/A |
| `WGUI_PASSWORD_HASH` | 登录页用户的密码哈希（`WGUI_PASSWORD` 的替代）。仅在初始化数据库时使用 | N/A |
| `WGUI_PASSWORD_HASH_FILE` | 登录密码哈希的可选文件路径（`WGUI_PASSWORD_FILE` 的替代）。需将 `WGUI_PASSWORD_HASH` 置空后生效，仅在初始化时使用 | N/A |
| `WGUI_ENDPOINT_ADDRESS` | 全局设置中客户端应连接的默认 Endpoint。可包含端口，适用于监听 `WGUI_SERVER_LISTEN_PORT`、但对外映射到其他端口的情况（如 9000），示例：`myvpn.dyndns.com:9000` | 解析为你的公网 IP |
| `WGUI_FAVICON_FILE_PATH` | 网站 favicon 的文件路径 | 内置 WireGuard 标志 |
| `WGUI_DNS` | 全局设置中默认 DNS 服务器（逗号分隔） | `1.1.1.1` |
| `WGUI_MTU` | 全局设置中的默认 MTU | `1450` |
| `WGUI_PERSISTENT_KEEPALIVE` | 全局设置中的 WireGuard 默认 keepalive | `15` |
| `WGUI_FIREWALL_MARK` | 默认 WireGuard 防火墙标记 | `0xca6c` (51820) |
| `WGUI_TABLE` | WireGuard 默认路由表设置 | `auto` |
| `WGUI_CONFIG_FILE_PATH` | 全局设置中 WireGuard 默认配置文件路径 | `/etc/wireguard/wg0.conf` |
| `WGUI_LOG_LEVEL` | 默认日志级别，可选：`DEBUG`、`INFO`、`WARN`、`ERROR`、`OFF` | `INFO` |
| `WG_CONF_TEMPLATE` | 自定义 `wg.conf` 模板，参考[默认模板](https://github.com/ngoduykhanh/wireguard-ui/blob/master/templates/wg.conf) | N/A |
| `EMAIL_FROM_ADDRESS` | 邮件发送人地址 | N/A |
| `EMAIL_FROM_NAME` | 邮件发送人名称 | `WireGuard UI` |
| `SENDGRID_API_KEY` | SendGrid API 密钥 | N/A |
| `SENDGRID_API_KEY_FILE` | SendGrid API 密钥的可选文件路径。需将 `SENDGRID_API_KEY` 置空后生效 | N/A |
| `SMTP_HOSTNAME` | SMTP IP 或主机名 | `127.0.0.1` |
| `SMTP_PORT` | SMTP 端口 | `25` |
| `SMTP_USERNAME` | SMTP 用户名 | N/A |
| `SMTP_PASSWORD` | SMTP 用户密码 | N/A |
| `SMTP_PASSWORD_FILE` | SMTP 用户密码的可选文件路径。需将 `SMTP_PASSWORD` 置空后生效 | N/A |
| `SMTP_AUTH_TYPE` | SMTP 认证方式，可选：`PLAIN`、`LOGIN`、`NONE` | `NONE` |
| `SMTP_ENCRYPTION` | 加密方式，可选：`NONE`、`SSL`、`SSLTLS`、`TLS`、`STARTTLS` | `STARTTLS` |
| `SMTP_HELO` | HELO 消息中使用的主机名。`smtp-relay.gmail.com` 需要设置为非 `localhost` 的值 | `localhost` |
### 服务器配置默认值

这些环境变量控制初始化数据库时的服务器默认设置。

| 变量 | 说明 | 默认值 |
|-----------------------------------|-----------------------------------------------------------------------------------------------|-----------------|
| `WGUI_SERVER_INTERFACE_ADDRESSES` | WireGuard 服务器配置中的默认网卡地址（逗号分隔） | `10.252.1.0/24` |
| `WGUI_SERVER_LISTEN_PORT` | 服务器默认监听端口 | `51820` |
| `WGUI_SERVER_POST_UP_SCRIPT` | 默认的 PostUp 脚本 | N/A |
| `WGUI_SERVER_POST_DOWN_SCRIPT` | 默认的 PostDown 脚本 | N/A |

### 新客户端默认值

这些环境变量用于设置“新建客户端”对话框的默认选项。

| 变量 | 说明 | 默认值 |
|---------------------------------------------|-------------------------------------------------------------------------------------------------|-------------|
| `WGUI_DEFAULT_CLIENT_ALLOWED_IPS` | `Allowed IPs` 字段的 CIDR 列表（逗号分隔） | `0.0.0.0/0` |
| `WGUI_DEFAULT_CLIENT_EXTRA_ALLOWED_IPS` | `Extra Allowed IPs` 字段的 CIDR 列表（逗号分隔，默认为空） | N/A |
| `WGUI_DEFAULT_CLIENT_USE_SERVER_DNS` | 布尔值，允许的形式：[`0`, `f`, `F`, `false`, `False`, `FALSE`, `1`, `t`, `T`, `true`, `True`, `TRUE`] | `true` |
| `WGUI_DEFAULT_CLIENT_ENABLE_AFTER_CREATION` | 布尔值，允许的形式同上，表示创建后是否默认启用 | `true` |

### Docker 专用

以下环境变量仅适用于 Docker 容器。

| 变量 | 说明 | 默认值 |
|-----------------------|---------------------------------------------------------------|---------|
| `WGUI_MANAGE_START` | 容器启动/停止时同步启停 WireGuard | `false` |
| `WGUI_MANAGE_RESTART` | 在 UI 中“应用配置”后自动重启 WireGuard | `false` |

## 自动重启 WireGuard 守护进程

WireGuard-UI 只负责生成配置；可借助 systemd 等工具监听文件变化并重启服务。以下示例供参考。

### 使用 systemd

创建 `/etc/systemd/system/wgui.service`

```bash
cd /etc/systemd/system/
cat << EOF > wgui.service
[Unit]
Description=Restart WireGuard
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/bin/systemctl restart wg-quick@wg0.service

[Install]
RequiredBy=wgui.path
EOF
```

创建 `/etc/systemd/system/wgui.path`

```bash
cd /etc/systemd/system/
cat << EOF > wgui.path
[Unit]
Description=Watch /etc/wireguard/wg0.conf for changes

[Path]
PathModified=/etc/wireguard/wg0.conf

[Install]
WantedBy=multi-user.target
EOF
```

应用配置：

```sh
systemctl enable wgui.{path,service}
systemctl start wgui.{path,service}
```

### 使用 openrc

创建 `/usr/local/bin/wgui` 并赋予执行权限：

```sh
cd /usr/local/bin/
cat << EOF > wgui
#!/bin/sh
wg-quick down wg0
wg-quick up wg0
EOF
chmod +x wgui
```

创建 `/etc/init.d/wgui` 并赋予执行权限：

```sh
cd /etc/init.d/
cat << EOF > wgui
#!/sbin/openrc-run

command=/sbin/inotifyd
command_args="/usr/local/bin/wgui /etc/wireguard/wg0.conf:w"
pidfile=/run/${RC_SVCNAME}.pid
command_background=yes
EOF
chmod +x wgui
```

应用配置：

```sh
rc-service wgui start
rc-update add wgui default
```

### 使用 Docker

将 `WGUI_MANAGE_RESTART=true` 可让容器管理 WireGuard 接口重启。再配合 `WGUI_MANAGE_START=true`，即可替代 `wg-quick@wg0` 服务，在容器以 `restart: unless-stopped` 方式运行时实现开机启动。重启容器后也能生效于修改过的 WireGuard 配置文件路径。请确保容器配置包含 `--cap-add=NET_ADMIN` 以启用该功能。

## 构建

### 构建 Docker 镜像

进入项目根目录执行：

```sh
docker build --build-arg=GIT_COMMIT=$(git rev-parse --short HEAD) -t wireguard-ui .
```

或：

```sh
docker compose build --build-arg=GIT_COMMIT=$(git rev-parse --short HEAD)
```

ℹ️ 你也可以直接拉取已构建的镜像：[Docker Hub](https://hub.docker.com/r/ngoduykhanh/wireguard-ui)

```
docker pull ngoduykhanh/wireguard-ui
````

### 构建二进制文件

准备静态资源目录：

```sh
./prepare_assets.sh
```

随后构建可执行文件：

```sh
go build -o wireguard-ui
```

## 许可证

MIT，详见 [LICENSE](https://github.com/ngoduykhanh/wireguard-ui/blob/master/LICENSE)。

## 支持项目

如果你喜欢本项目并希望支持，请 *buy me a coffee* ☕

<a href="https://www.buymeacoffee.com/khanhngo" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>
