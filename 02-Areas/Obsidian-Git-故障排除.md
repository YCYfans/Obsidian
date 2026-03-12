

# Obsidian + GitHub 同步完全排错指南

> 适用于：Obsidian Git 插件 + GitHub 私有仓库备份方案
> 最后更新：2026-03-03

---

## 📋 快速诊断表

| 错误提示                                     | 原因                | 快速解决                  |
| ---------------------------------------- | ----------------- | --------------------- |
| `Author identity unknown`                | Git 未配置用户名/邮箱     | [查看章节 1](#1-身份认证问题)   |
| `does not appear to be a git repository` | Remote 地址错误       | [查看章节 2](#2-远程仓库配置错误) |
| `No upstream branch is set`              | 未关联远程分支           | [查看章节 3](#3-上游分支未设置)  |
| `failed to push some refs`               | 远程有内容本地没有         | [查看章节 4](#4-推送冲突)     |
| `port 443: Could not connect`            | 网络/代理问题           | [查看章节 5](#5-网络连接失败)   |
| 自动备份没反应                                  | 时间设置/upstream/未保存 | [查看章节 6](#6-自动备份不工作)  |

---

## 1. 身份认证问题

### ❌ 错误信息
```

Author identity unknown
Please tell me who you are.
fatal: unable to auto-detect email address

```

### ✅ 解决方法
在终端执行（使用你的真实信息）：

```bash
git config --global user.email "你的邮箱@example.com"
git config --global user.name "你的GitHub用户名"
```

💡 验证

```bash
git config --global user.name
git config --global user.email
```

---

2. 远程仓库配置错误

❌ 错误信息

```
fatal: 'w731327930@hotmail.com' does not appear to be a git repository
fatal: Could not read from remote repository
```

或

```
fatal: remote name 'origin' is a superset of existing remote 'origin/main'
```

✅ 解决方法

情况 A：Remote 地址拼错（如把邮箱当地址）

```bash
# 查看当前配置（确认错误）
git remote -v

# 删除错误的 remote
git remote remove origin
# 如果有 origin/main 也删除
git remote remove "origin/main"

# 添加正确的地址（HTTPS 格式）
git remote add origin https://github.com/你的用户名/仓库名.git

# 验证
git remote -v
```

情况 B：地址正确但协议不对
- HTTPS 格式：`https://github.com/用户名/仓库.git`（推荐，用 Token 认证）
- SSH 格式：`git@github.com:用户名/仓库.git`（需要配置 SSH 密钥）

---

3. 上游分支未设置

❌ 错误信息

```
No upstream branch is set. Please select one.
error: src refspec main does not match any
```

✅ 解决方法

方法 1：命令行（一劳永逸）

```bash
# 关联本地 main 分支到远程 main
git branch --set-upstream-to=origin/main main

# 或简写
git branch -u origin/main
```

方法 2：Obsidian 内操作
1. `Ctrl+P` → `Obsidian Git: Push`
2. 选择 `origin/main`
3. 以后插件就记住了

方法 3：推送时直接建立关联

```bash
git push -u origin main
```

---

4. 推送冲突

❌ 错误信息

```
! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'https://github.com/...'
hint: Updates were rejected because the tip of your current branch is behind
```

✅ 解决方法

方案 A：先拉取再推送（安全，推荐）

```bash
# 获取远程内容并合并
git pull origin main

# 如果有冲突，手动解决后提交
git add .
git commit -m "merge remote changes"

# 再推送
git push origin main
```

方案 B：强制推送（会覆盖远程内容，慎用！）
⚠️ 警告：这会删除 GitHub 上现有的所有文件！

```bash
git push -f origin main
```

---

5. 网络连接失败

❌ 错误信息

```
fatal: unable to access 'https://github.com/...': 
Failed to connect to github.com port 443 after 21097 ms: Could not connect to server
```

✅ 解决方法

方法 1：配置代理（如果你有 VPN）

```bash
# 设置代理（7890 是常见端口，根据你的软件调整）
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# 取消代理（当不需要时）
git config --global --unset http.proxy
git config --global --unset https.proxy
```

方法 2：换网络环境
- 手机开热点给电脑
- 切换 WiFi/有线网络
- 尝试不同时段（凌晨通常更稳定）

方法 3：使用镜像（临时）

```bash
# 临时改为镜像地址（不稳定，仅应急）
git remote set-url origin https://mirror.ghproxy.com/https://github.com/用户名/仓库.git
```

---

6. 自动备份不工作

❌ 现象
- 编辑文件后没有自动提交
- 右下角没有 "Committed" 提示
- 等了很久没反应

✅ 排查清单

1. 检查设置
- Auto commit-and-sync interval: 设为 `1`（分钟）测试，成功后再改 `5` 或 `15`
- Auto commit-and-sync after stopping file edits: 必须开启
- Pull changes before push: 建议开启（多设备必需）

2. 检查 Upstream
如果手动 Push 提示 `No upstream`，先执行：

```bash
git branch -u origin/main
```

3. 检查文件是否保存
- Obsidian 自动保存可能延迟，手动按 `Ctrl+S` 确保文件已写入
- 查看左侧 Source Control 图标是否有数字标记

4. 检查网络
如果配置了代理，确保代理软件正在运行。

---

🔧 常用维护命令

```bash
# 查看当前状态
git status

# 查看修改了哪些文件
git diff

# 手动提交（当自动备份失败时）
git add .
git commit -m "手动备份"
git push origin main

# 查看提交历史
git log --oneline -10

# 放弃本地修改（危险！会丢失未提交的修改）
git checkout -- .

# 克隆仓库到新设备
git clone https://github.com/用户名/仓库.git
```

---

⚠️ 重要提醒

关于认证
- GitHub 已不支持密码登录，必须使用 Personal Access Token
- 获取地址：GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
- 权限勾选：`repo`（完全控制私有仓库）

关于多设备同步
1. 切换设备前：在旧设备点击 "Backup" 确保推送完成
2. 打开 Obsidian 时：等待自动 Pull 完成（或手动点击 Pull）再编辑
3. 冲突避免：不要同时在两台设备编辑同一个文件

关于移动端
- Obsidian Git 在 iOS/Android 上性能较差，可能卡顿
- 建议移动端使用独立 Git 客户端（如 Working Copy、Git Sync）手动同步

---

🚀 首次配置完整流程（参考）

```bash
# 1. 初始化
cd 你的Obsidian库路径
git init

# 2. 配置身份
git config user.name "你的名字"
git config user.email "你的邮箱"

# 3. 添加远程
git remote add origin https://github.com/用户名/仓库.git

# 4. 首次提交
git add .
git commit -m "初始提交"

# 5. 强制推送（如果远程是空仓库）
git push -u origin main
```

---

📝 问题记录模板

遇到新问题时，按此格式记录：

```
日期：
错误信息：
已尝试的解决方法：
最终解决方案：
备注：
```

---

保存此文档后，建议同时在 GitHub 仓库根目录保存一份，方便随时查阅！