### 初始化和配置

- **`git init`**
  - 作用：在当前目录初始化一个新的 Git 仓库。
  - 示例：`git init`

- **`git config --global user.name "Your Name"`**
  - 作用：设置全局用户名。
  - 示例：`git config --global user.name "John Doe"`

- **`git config --global user.email "youremail@example.com"`**
  - 作用：设置全局用户邮箱。
  - 示例：`git config --global user.email "johndoe@example.com"`

### 文件操作

- **`git add <file>`**
  - 作用：将指定文件添加到暂存区。
  - 示例：`git add README.md`

- **`git add .`**
  - 作用：将当前目录的所有变化添加到暂存区。
  - 示例：`git add .`

- **`git commit -m "commit message"`**
  - 作用：将暂存区的内容提交到仓库，并添加提交信息。
  - 示例：`git commit -m "Initial commit"`

- **`git status`**
  - 作用：查看当前仓库的状态，包括文件的更改和暂存区的状态。
  - 示例：`git status`

### 分支管理

- **`git branch`**
  - 作用：列出所有分支。
  - 示例：`git branch`

- **`git branch <branch-name>`**
  - 作用：创建一个新的分支。
  - 示例：`git branch feature`

- **`git checkout <branch-name>`**
  - 作用：切换到指定分支。
  - 示例：`git checkout feature`

- **`git checkout -b <branch-name>`**
  - 作用：创建一个新的分支并切换到该分支。
  - 示例：`git checkout -b feature`

- **`git merge <branch-name>`**
  - 作用：将指定分支合并到当前分支。
  - 示例：`git merge feature`

- **`git branch -d <branch-name>`**
  - 作用：删除指定分支。
  - 示例：`git branch -d feature`

### 远程仓库

- **`git remote -v`**
  - 作用：显示远程仓库的 URL。
  - 示例：`git remote -v`

- **`git remote add origin <repository-url>`**
  - 作用：添加远程仓库。
  - 示例：`git remote add origin https://github.com/user/repo.git`

- **`git push -u origin <branch-name>`**
  - 作用：将本地分支推送到远程仓库，并设置为默认分支。
  - 示例：`git push -u origin master`

- **`git pull`**
  - 作用：从远程仓库拉取内容并合并到当前分支。
  - 示例：`git pull`

### 日志和回退

- **`git log`**
  - 作用：显示提交历史。
  - 示例：`git log`

- **`git log --oneline`**
  - 作用：以简化的方式显示提交历史。
  - 示例：`git log --oneline`

- **`git reset --hard <commit-hash>`**
  - 作用：将 HEAD、索引和工作目录回退到指定的提交。
  - 示例：`git reset --hard a1b2c3d`

- **`git revert <commit-hash>`**
  - 作用：创建一个新的提交，撤销指定提交的更改。
  - 示例：`git revert a1b2c3d`

### 标签管理

- **`git tag`**
  - 作用：列出所有标签。
  - 示例：`git tag`

- **`git tag <tag-name>`**
  - 作用：创建一个新的标签。
  - 示例：`git tag v1.0`

- **`git push origin <tag-name>`**
  - 作用：将本地标签推送到远程仓库。
  - 示例：`git push origin v1.0`

- **`git tag -d <tag-name>`**
  - 作用：删除本地标签。
  - 示例：`git tag -d v1.0`

- **`git push origin :<tag-name>`**
  - 作用：删除远程标签。
  - 示例：`git push origin :v1.0`