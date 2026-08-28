# Mira Plugin Registry（官方插件商店仓库）

Mira（智影）AgentHub 插件商店的官方索引仓库。

## 消费方式

Mira 设置中心 → **Agent 插件 → 插件商店** → 仓库地址填：

```
https://raw.githubusercontent.com/jiyi1990118/mira-plugin-registry/main/registry.json
```

（国内镜像仓库规划中，届时 hub 将按网络探测自动分流，见下方「镜像」。）

## 仓库结构

- `registry.json` — 插件索引（格式规范：AgentHub 仓库 `Docs/plans/2026-08-27-agent-plugin-store-design.md` §2）
- `<id>-<version>.zip` — 插件包（解压根即插件目录，含 `agent.json` 清单）

## 安装链路（hub 自动完成）

拉取索引 → 选包 → 下载 zip → 核对 `sha256 + bytes` → 安全解压（路径穿越/符号链接防护）→ 校验清单 → 导入 `~/.local/share/mini/agents/<id>/`。

## 镜像

国内访问 GitHub raw 不稳定时，将建立 Gitee 镜像仓库（内容逐字节同步）。
hub 支持配置多仓库地址（主仓库 + 镜像），按「并发探测、快者优先、失败切换」策略自动选择，无需手工切换。

## 发布

维护者发布流程（需 `tools/registry-publish.mjs`，位于 AgentHub 仓库）：

```bash
node tools/registry-publish.mjs add <plugin.zip> --registry ./registry.json \
  --base-url https://raw.githubusercontent.com/jiyi1990118/mira-plugin-registry/main \
  --trust official --author "..."
node tools/registry-publish.mjs verify --registry ./registry.json --strict
git add registry.json <plugin.zip> && git commit -m "publish ..." && git push
```
