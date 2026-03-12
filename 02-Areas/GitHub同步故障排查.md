
# Obsidian + GitHub 同步常见问题排查

记录一次从零搭建 **Obsidian → GitHub 同步**时遇到的问题和解决方法，避免以后重复踩坑。

---

# 一、初始化 Git 仓库

在 Obsidian 笔记目录打开终端：

```bash
git init
```

创建 `.gitignore`：

```bash
echo .obsidian/ > .gitignore
```

提交：

```bash
git add .
git commit -m "initial commit"
```

设置主分支：

```bash
git branch -M main
```

绑定远程仓库：

```bash
git remote add origin https://github.com/USERNAME/REPO.git
```

推送：

```bash
git push -u origin main
```

---

# 二、常见错误与解决方法

---

# 1 GitHub Push Protection（检测到 secret）

### 报错示例

```
Push cannot contain secrets
OpenAI API Key detected
```

### 原因

GitHub 检测到提交中包含 **API key / secret**。

例如：

```
.obsidian/plugins/copilot/data.json
```

### 解决方法

删除 API key：

```
.obsidian/plugins/copilot/data.json
```

并加入 `.gitignore`：

```
.obsidian/
```

如果 secret 已经进入 commit 历史：

**最简单的办法**

删除 git 历史重新初始化：

```bash
Remove-Item -Recurse -Force .git
git init
```

重新提交即可。

---

# 2 `.gitignore` 不生效

### 原因

文件已经被 Git 跟踪。

### 解决

从 Git 索引中移除：

```bash
git rm -r --cached .obsidian
```

然后重新提交：

```bash
git commit -m "remove obsidian config"
```

---

# 3 remote origin already exists

### 报错

```
error: remote origin already exists
```

### 原因

远程仓库已经绑定过。

### 解决

删除旧 remote：

```bash
git remote remove origin
```

重新添加：

```bash
git remote add origin https://github.com/USERNAME/REPO.git
```

---

# 4 repository not found

### 报错

```
repository not found
```

### 常见原因

远程地址写错，例如：

```
https://github.com/user/repo.git,
```

末尾多了一个 **逗号**。

### 解决

检查 remote：

```bash
git remote -v
```

重新绑定：

```bash
git remote remove origin
git remote add origin https://github.com/USERNAME/REPO.git
```

---

# 5 Connection was reset

### 报错

```
Recv failure: Connection was reset
```

### 原因

网络问题 / 代理问题。

### 可能解决

检查 git 代理：

```bash
git config --global --get http.proxy
git config --global --get https.proxy
```

取消代理：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

或者设置正确代理端口。

---

# 三、推荐 `.gitignore`

建议忽略以下文件：

```
.obsidian/
.trash/
.DS_Store
*.log
```

原因：

- `.obsidian` 包含插件配置
    
- 可能包含 API key
    
- 不需要同步
    

---

# 四、推荐 Obsidian 笔记结构

常见 PARA + Inbox 结构：

```
00-Inbox
01-DailyNotes
02-Areas
03-Projects
04-Resources
```

说明：

|目录|用途|
|---|---|
|Inbox|临时记录|
|DailyNotes|每日笔记|
|Areas|长期责任|
|Projects|项目|
|Resources|知识库|

---

# 五、日常同步流程

提交更新：

```bash
git add .
git commit -m "update notes"
git push
```

拉取更新：

```bash
git pull
```

如果使用 **Obsidian Git 插件**，这些步骤可以自动完成。

---

# 六、彻底重置 Git 仓库（终极解决方案）

当仓库历史污染（例如包含 secret）时：

删除 Git 历史：

```bash
Remove-Item -Recurse -Force .git
```

重新初始化：

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main
```

---

# 七、经验总结

最常见问题：

1️⃣ `.gitignore` 没写  
2️⃣ secret 被提交  
3️⃣ remote 地址写错  
4️⃣ 本地 `.git` 历史污染  
5️⃣ 网络 / 代理问题

解决原则：

> 当 Git 历史混乱时，直接删除 `.git` 重新初始化是最快的方法。

---

如果你愿意，我还能帮你再补一份 **Obsidian + Git + AI 的最佳知识库结构（很多 AI Agent 都这么搭）**，比普通笔记体系要强很多。