# RHCloud1 备份说明

本仓库是 [cedar2025/Xboard](https://github.com/cedar2025/Xboard) 的备份 fork，目的是在原作者删库后仍能独立搭建。所有依赖原作者的地址都已改为 RHCloud1：

| 用途 | 原地址 | 备份地址 |
|---|---|---|
| 面板源码（Dockerfile 构建时 clone） | cedar2025/Xboard | RHCloud1/Xboard |
| 后台前端（子模块 public/assets/admin） | cedar2025/xboard-admin-dist | RHCloud1/xboard-admin-dist |
| Docker 镜像 | ghcr.io/cedar2025/xboard | ghcr.io/rhcloud1/xboard |
| 后台“检查更新” | api.github.com/repos/cedar2025/xboard | api.github.com/repos/RHCloud1/Xboard |
| 机器节点一键安装（MachineController） | cedar2025/xboard-node | RHCloud1/Xboard-Node（含 release 二进制） |
| Xboard-Node 依赖的内核 fork | cedar2025/sing-box、cedar2025/Xray-core | RHCloud1/sing-box、RHCloud1/Xray-core |

Docker 镜像由 `.github/workflows/docker-publish.yml` 在 push 到 master 时构建（amd64 + arm64），按 `latest`、`new` 和短 commit 打标签。

Xboard-Node 的 `go.mod` 仍 replace 到 `github.com/cedar2025/sing-box` 和 `github.com/cedar2025/Xray-core`，这两个版本已被 proxy.golang.org 缓存；若缓存不可用，把 replace 改为 RHCloud1 下同名 fork 的同一 commit 并 `go mod tidy`。

## 部署

与原版相同，见 README 的 Quick Start（`compose` 分支已指向 ghcr.io/rhcloud1/xboard）。

## 同步原作者更新

```bash
git remote add upstream https://github.com/cedar2025/Xboard
git fetch upstream
git merge upstream/master   # 冲突只会出现在上表这几处地址，保留 RHCloud1 的写法
git push origin master      # 触发镜像构建
```

后台前端更新：在 RHCloud1/xboard-admin-dist 上点 “Sync fork”，再在本仓库执行 `git submodule update --remote public/assets/admin` 并提交。
