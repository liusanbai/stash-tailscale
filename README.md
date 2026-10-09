# Tailscale | Stash Override

在 Stash 中集成 Tailscale 组网，访问 NAS、Home Assistant、WebDAV 等内网服务。

## 功能

- 声明 Tailscale 代理节点（`type: tailscale`）
- 创建 Tailscale 策略组，供手动切换
- 自动路由 Tailnet 流量（`100.64.0.0/10` + MagicDNS 域名）
- 适用于所有设备，各设备独立认证，互不影响

## 安装

在 Stash 中：

1. 配置 → 覆写 → `+` → 从 URL 下载
2. 填入：

```
https://github.com/liusanbai/stash-tailscale/releases/latest/download/Tailscale.stoverride
```

3. 确认覆写已启用
4. 回首页点重载

## 首次使用（重要）

此覆写发布在公开 GitHub，**auth-key 已注释掉**。

### 方式一：填入 auth-key

1. 在 [Tailscale 管理后台](https://login.tailscale.com/admin/settings/keys) 生成 Auth Key
2. 建议为每台设备单独生成（不勾选 Reusable）
3. 在 Stash 中编辑覆写文件，取消注释 `auth-key` 行，填入你的 Key
4. 重载配置，确认 Tailscale 节点连接成功
5. 连接成功后，重新注释掉 `auth-key` 即可（已认证设备不需要重复认证）

### 方式二：交互式登录

不填 auth-key，直接在 Stash 的节点菜单中打开 Tailscale 页面，通过浏览器完成登录认证。

## 规则说明

| 规则 | 说明 |
|---|---|
| `IP-CIDR,100.64.0.0/10,Tailscale,no-resolve` | Tailnet 内网 IP 段 |
| `DOMAIN-SUFFIX,*.ts.net,Tailscale` | MagicDNS 域名（请改成你自己的 Tailnet 域名） |

`auto-route-disabled: false` 表示 Stash 自动处理 Tailnet 路由，访问内网服务时不需要额外写 rules。

## 技术依据

- [Stash 配置样例](https://stash.wiki/configuration/example-config) — Tailscale 节点声明
- [Stash Override 文档](https://stash.wiki/en/configuration/override) — 覆写机制

## License

MIT
