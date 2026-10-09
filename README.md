# Tailscale | Stash Override

在 Stash 中集成 Tailscale 组网，访问 NAS、Home Assistant、WebDAV 等内网服务。

## 功能

- 声明 Tailscale 代理节点（`type: tailscale`）
- 创建 Tailscale 策略组，供手动切换
- 自动路由 Tailnet 流量（`100.64.0.0/10` + MagicDNS 域名）
- 适用于所有设备，各设备独立认证，互不影响

## 安装

> **注意：此覆写不能直接从 URL 安装使用。**
>
> 因为 auth-key 不能放在公开仓库里，而 Stash 中 URL 安装的覆写不支持本地编辑。
> 所以必须**下载文件 → 本地填入 auth-key → 本地导入**。

### 步骤

1. 下载 [Tailscale.stoverride](https://github.com/liusanbai/stash-tailscale/releases/latest/download/Tailscale.stoverride)
2. 用文本编辑器打开，找到 `auth-key` 那行（约第 44 行），取消注释并填入你的 Auth Key：

   ```yaml
   auth-key: tskey-auth-xxxxxxxxxxxxx-CNTRL-xxxxxxxxxxxxxxxx
   ```

3. 在 [Tailscale 管理后台](https://login.tailscale.com/admin/settings/keys) 生成 Auth Key（建议每台设备单独生成，不勾选 Reusable）
4. 将修改后的文件通过 AirDrop / iCloud 发到手机
5. Stash → 配置 → 覆写 → `+` → **从文件导入**（不是从 URL 下载）
6. 启用覆写 → 回首页点重载
7. 确认 Tailscale 节点上线后，可以把 auth-key 重新注释掉（已认证设备不需要重复认证）

## 规则说明

| 规则 | 说明 |
|---|---|
| `IP-CIDR,100.64.0.0/10,Tailscale,no-resolve` | Tailnet 内网 IP 段（全网通用） |
| `DOMAIN-SUFFIX,ts.net,Tailscale` | MagicDNS 域名（请改成你自己的 Tailnet 域名） |

`auto-route-disabled: false` 表示 Stash 自动处理 Tailnet 路由，访问内网服务时不需要额外写 rules。

## 技术依据

- [Stash 配置样例](https://stash.wiki/configuration/example-config) — Tailscale 节点声明
- [Stash Override 文档](https://stash.wiki/en/configuration/override) — 覆写机制

## License

MIT
