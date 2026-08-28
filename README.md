# Mira Plugin Registry（官方插件商店仓库）

Mira（智影）AgentHub 插件商店的官方索引仓库。

## 消费方式

Mira 设置中心 → **Agent 插件 → 插件商店** → 仓库地址填：

```
https://raw.githubusercontent.com/jiyi1990118/mira-plugin-registry/main/registry.json
```

国内网络可加配镜像（见下方「镜像」），hub 将并发探测、快者优先、失败自动切换。

## 仓库结构

- `registry.json` — 插件索引（格式规范：AgentHub 仓库 `Docs/plans/2026-08-27-agent-plugin-store-design.md` §2）
- `<id>-<version>.zip` — 插件包（解压根即插件目录，含 `agent.json` 清单）

## 安装链路（hub 自动完成）

拉取索引 → 选包 → 下载 zip → 核对 `sha256 + bytes` → 安全解压（路径穿越/符号链接防护）→ 校验清单 → 导入 `~/.local/share/mini/agents/<id>/`。

## 当前包

| id | 版本 | 信任级 | 说明 |
|---|---|---|---|
| `store-echo-demo` | 1.0.0 | official | ABP 桥接演示适配器（全平台，Bun） |
| `opencode` | 1.0.0 | official | Mira OpenCode 一方集成包（opencode-data 守护进程 + mini-bridge 插件声明式部署）。**平台限制：macOS Apple Silicon（arm64）**；已装 Mira.app 的设备内置同源版本，此包供独立 hub 部署使用 |

## 镜像


国内镜像：**`https://gitee.com/xiyuan/mira-plugin-registry`**（内容逐字节同步，消费地址 `https://gitee.com/xiyuan/mira-plugin-registry/raw/main/registry.json`）。

推荐双仓库配置（设置中心「插件商店」）：

- 仓库地址：`https://raw.githubusercontent.com/jiyi1990118/mira-plugin-registry/main/registry.json`
- 镜像：`https://gitee.com/xiyuan/mira-plugin-registry/raw/main/registry.json`

hub 按「并发探测、快者优先、失败切换」策略自动选择，无需手工切换。

## 发布

维护者发布流程（需 `tools/registry-publish.mjs`，位于 AgentHub 仓库）：

```bash
node tools/registry-publish.mjs add <plugin.zip> --registry ./registry.json \
  --base-url https://raw.githubusercontent.com/jiyi1990118/mira-plugin-registry/main \
  --trust official --author "..."
node tools/registry-publish.mjs verify --registry ./registry.json --strict
git add registry.json <plugin.zip> && git commit -m "publish ..." && git push
```

发布后同步镜像（在本仓库克隆目录内）：

```bash
node tools/registry-publish.mjs sync --from . --to <gitee 镜像仓库克隆目录>
cd <gitee 镜像仓库克隆目录> && git add -A && git commit -m "sync" && git push
```
