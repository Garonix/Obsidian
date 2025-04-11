**基本操作：**

- **添加子模块:**
    
    Bash
    
    ```
    git submodule add <仓库URL> <子模块路径>
    ```
    
    例如：`git submodule add https://github.com/example/submodule.git external/sub`
    
- **初始化子模块 (在克隆包含子模块的项目后需要执行):**
    
    Bash
    
    ```
    git submodule init
    ```
    
- **更新子模块 (拉取子模块的最新代码):**
    
    Bash
    
    ```
    git submodule update [--init] [--recursive]
    ```
    
    - `--init`: 如果子模块尚未初始化，则进行初始化。
    - `--recursive`: 如果子模块中还包含其他子模块（嵌套子模块），则递归地更新它们。

**查看子模块状态：**

- **查看子模块状态:**
    
    Bash
    
    ```
    git submodule status
    ```
    
    这会列出每个子模块的状态，包括当前检出的提交和是否有未提交的更改。

**高级操作：**

- **对所有子模块执行命令:**
    
    Bash
    
    ```
    git submodule foreach <命令>
    ```
    
    例如：`git submodule foreach git pull origin main` (在所有子模块中拉取主分支的最新代码)
    
- **同步子模块 (更新 `.gitmodules` 中的 URL):**
    
    Bash
    
    ```
    git submodule sync [--recursive]
    ```
    
    当子模块的远程仓库 URL 发生更改时，可以使用此命令更新本地配置。
    
- **取消初始化子模块 (从项目中移除子模块的引用，但不删除子模块目录):**
    
    Bash
    
    ```
    git submodule deinit <子模块路径>
    ```
    
    例如：`git submodule deinit external/sub` 要取消初始化所有子模块，可以使用 `git submodule deinit -all`。
    
- **彻底移除子模块 (包括删除子模块目录和 Git 历史):** 移除子模块需要多个步骤：
    
    1. 取消初始化子模块：
        
        Bash
        
        ```
        git submodule deinit <子模块路径>
        ```
        
    2. 删除工作区中的子模块目录：
        
        Bash
        
        ```
        rm -rf <子模块路径>
        ```
        
    3. 从 Git 索引中移除子模块：
        
        Bash
        
        ```
        git rm --cached <子模块路径>
        ```
        
    4. 编辑 `.gitmodules` 文件，删除子模块的相关条目。
    5. 编辑 `.git/config` 文件，删除 `[submodule "<子模块路径>"]` 部分。
    6. 提交你的更改：
        
        Bash
        
        ```
        git commit -m "Remove submodule <子模块路径>"
        ```
        
- **更新子模块到远程仓库的最新版本:**
    
    Bash
    
    ```
    git submodule update --remote [--merge | --rebase] <子模块路径>
    ```
    
    这会从子模块的远程仓库拉取最新的提交。你可以选择使用 `--merge` 或 `--rebase` 来合并更改。
    

**在克隆包含子模块的项目后，通常需要执行以下操作：**

Bash

```
git clone --recurse-submodules <仓库URL>
```

或者，如果已经克隆了项目，可以执行：

Bash

```
git submodule init
git submodule update
```