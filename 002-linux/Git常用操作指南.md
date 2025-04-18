## 分支同步与合并

### 同步本地分支到远端最新状态

```bash
# 获取远端最新更改
git fetch origin

# 切换到本地分支
git checkout <你的本地分支名>

# 合并远端分支的更改
git merge origin/main  # 或 git merge origin/master

# 或者使用rebase同步(可选)
git rebase origin/main

# 解决冲突(如有)
git add <冲突的文件>
git rebase --continue  # 如果使用的是rebase

# 推送更改到远端
git push origin <你的本地分支名>
```

> 注意：合并操作不会直接覆盖本地修改，但可能产生冲突需要手动解决。

### 合并本地分支到远程develop

```bash
# 切换并更新本地develop分支
git checkout develop
git pull origin develop

# 合并feature分支
git merge <feature-branch>

# 解决冲突(如有)
git add .
git commit -m "Merge feature branch and resolve conflicts"

# 推送到远程
git push origin develop
```

## 未提交修改的管理

### 处理未提交的修改

当本地有未提交修改时，通常无法执行 `merge`操作，Git需要确保工作目录干净以避免数据丢失。

#### 方法一：提交本地更改

```bash
git add .
git commit -m "保存本地更改"
```

#### 方法二：使用stash临时保存

```bash
git stash                  # 保存修改
git fetch origin           # 获取远端更新
git merge origin/develop   # 合并远端更新
git stash pop              # 恢复之前的修改
```

#### 方法三：创建临时分支

```bash
git checkout -b temp-branch
git add .
git commit -m "临时保存更改"
git checkout <原始分支>
git fetch origin
git merge origin/develop
```

### 放弃本地修改

```bash
# 放弃未暂存更改
git checkout -- .

# 放弃所有更改(包括暂存)
git reset --hard

# 删除未追踪文件
git clean -fd
```

## Git Stash详解

### 基本用法

`git stash`用于临时保存未提交的更改，清空工作区以进行其他操作。

```bash
# 保存当前更改
git stash

# 查看stash列表
git stash list

# 恢复stash(保留stash记录)
git stash apply
git stash apply stash@{1}  # 恢复特定stash

# 恢复并删除stash
git stash pop

# 删除stash
git stash drop stash@{0}
git stash clear            # 删除所有stash

# 包含未跟踪文件
git stash -u  # 包括未跟踪文件
git stash -a  # 包括未跟踪和被忽略文件
```

### 使用策略

- **继续之前工作**：先恢复stash，再开始新修改
- **开始全新工作**：完成后再决定是否恢复stash

## 查看与撤销更改

### 查看提交历史

```bash
# 查看某次提交详情
git show <commit-hash>

# 查看提交历史
git log
git log --stat            # 显示文件修改统计

# 比较两次提交
git diff commit1 commit2

# 查看特定文件历史
git log -p <文件路径>

# 图形化显示历史
git log --graph --oneline --all

# 查看最后一次提交
git show
git log -p -1
```

### Git Revert操作

`git revert`用于撤销指定提交，不删除提交历史，而是创建新提交来抵消指定更改。

```bash
# 查看提交历史
git log --oneline

# 撤销特定提交
git revert <commit_hash>

# 撤销最近一次提交
git revert HEAD
```

**特点**：

- 创建新的反向提交，不修改历史记录
- 适合公共分支使用，适合撤销已推送的更改

## 协作工作流

### 合并请求(PR/MR)

- **Pull Request (PR)**：GitHub、Bitbucket等平台使用的术语
- **Merge Request (MR)**：GitLab平台使用的术语
- 两者功能相同，都用于请求将一个分支的更改合并到另一个分支

### 标准工作流程

1. 创建功能分支
2. 开发并提交更改
3. 创建PR/MR请求合并
4. 进行代码审查
5. 审核通过后合并
