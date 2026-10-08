# monitor

文档：[monitor-document.pages.dev](https://monitor-document.pages.dev)，安装、配置、反向代理与主题开发都在这里。

主题站：[monitor-themes.pages.dev](https://monitor-themes.pages.dev)，在线预览各个公开页主题，复制地址即可在面板安装。

## 特性

- 实时监控：秒级实时数据展示
- 轻量高效：Rust 语言构建，低资源占用，极简高效
- 自托管：完全掌控数据隐私，部署简单
- 通知：节点掉线、流量、到期与登录，推送到 Telegram 或自定义 Webhook
- 分时流量历史：后台节点的「历史」可查看每日 00–08、08–14、14–20、20–24 的上传、下载与已记录合计，沿用历史保留天数。

分时流量按采样当时的 hub 本地时间记账，跨时段采样和断报补记归后一次采样所在时段，无法精确还原实际时间分布。旧每日累计不拆分回填，没有记录的时段显示「未记录」。本 fork 使用独立扩展表，不占用上游数据库迁移编号。

## 安装本 fork

发布 Release 后，在使用 systemd 的 Linux 服务器上运行：

```sh
curl -fsSL https://github.com/white-u/monitor/releases/latest/download/install-hub.sh -o install-hub.sh
sudo sh install-hub.sh --yes --site https://monitor.example.com
```

将 `monitor.example.com` 换成你的 HTTPS 域名，并配置反向代理到 `127.0.0.1:28080`。hub 的安装和更新检查使用 `white-u/monitor`，agent 和默认主题继续使用上游版本。在后台添加本机节点，复制生成的 agent 安装命令到同一台服务器执行即可。

## 组成

| 仓库 | 说明 |
|---|---|
| [monitor](https://github.com/monitor-probe/monitor) | hub：后台、API、公开页宿主 |
| [agent](https://github.com/monitor-probe/agent) | Linux agent |
| [monitor-theme-default](https://github.com/monitor-probe/monitor-theme-default) | 内置默认主题 |
| [themes](https://github.com/monitor-probe/themes) | 主题站：收录第三方主题，提供在线预览 |

```
agent (Linux)  ──WebSocket / JSON-RPC 2.0──▶  hub (axum + SQLite)  ──▶  后台 + 状态页
```
