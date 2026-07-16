# RDP

GitHub Actions workflows 集合，用于通过 [Tailscale](https://tailscale.com) 创建安全的远程桌面（RDP / VNC）连接。

## 工作流

| 工作流 | 触发方式 | 描述 |
|---|---|---|
| **windows** | `workflow_dispatch`（手动） | 在 Windows runner 上配置 RDP，安装 Tailscale，清理同名旧设备，启用子网路由 / 出口节点，保持连接 |
| **ubuntu** | `workflow_dispatch`（手动） | 配置 Ubuntu VNC + noVNC 网页访问，通过 Tailscale 安全连接 |

## 前置条件

在 GitHub 仓库的 **Settings → Secrets and variables → Actions** 中配置以下 Secrets：

| Secret | 用途 |
|---|---|
| `RDP_USERNAME` | Windows RDP 用户名 |
| `RDP_PASSWORD` | Windows RDP 密码 |
| `TAILSCALE_AUTH_KEY` | Tailscale 认证密钥（[生成方式](https://login.tailscale.com/admin/settings/authkeys)） |
| `TAILSCALE_API_TOKEN` | Tailscale API 令牌 |

## 使用方式

1. 确保上述 Secrets 已配置
2. 进入 GitHub 仓库的 **Actions** 标签页
3. 选择要运行的工作流（`windows` 或 `ubuntu`）
4. 点击 **Run workflow**
5. 工作流运行后，控制台会输出 Tailscale IP / DNS 地址及凭据

## 注意事项

- Windows runner 最长运行 3600 分钟（60 小时），超时自动停止
- 工作流会**自动清理** tailnet 中名为 `windows` 的旧设备，避免主机名冲突
- Tailscale IP 为自动分配，免费版不支持自定义 IP

## 文件夹结构

```
.github/workflows/
├── windows.yml   # Windows RDP 工作流
└── ubuntu.yml    # Ubuntu VNC 工作流
```