# ptpatronus-sites

PTPatronus 站点定义远端源——**站点适配热更新分发仓库**。

## 这是什么

PTPatronus 实例会定时（每 6 小时）拉取本仓库 `main.tar.gz`，把顶层 `*.yaml`
站点定义同步到本地并热加载。**适配一个新站点 = 往本仓库提一个 YAML，
用户十几分钟内自动拿到，主程序不用发版。**

主程序仓库（pt-patronus）的 `backend/definitions/` 是唯一编辑源——
本仓库由 `scripts/sync-site-defs.sh` 从主仓同步，不要直接在这里改。

## 新增站点步骤

1. 确认目标站点协议族已有 schema 支持（nexusphp / mteam / fluxtorrent /
   piggo / unit3d / gazelle / …）。判断依据：站点是不是 NexusPHP 形态、
   是否提供 M-Team 兼容面、是否 FluxTorrent 建站系统等。
2. 在主仓 `backend/definitions/` 照既有站写一份 YAML（可抄最接近的站）。
3. 提交主仓后跑 `bash scripts/sync-site-defs.sh`。
4. 若定义用到了新 schema / 新选择器能力（本版程序不支持），在 YAML 里加：
   `min_app_version: "1.03.28"`——老版本实例会跳过它并在诊断里提示升级，
   而不是抓取报错。

## 定义格式

见主仓 `backend/internal/site/types.go` 的 SiteMetadata 结构与既有定义。
顶层键以 `id/name/schema/urls` 为骨架；选择器类键仅 NexusPHP 形态站需要。
