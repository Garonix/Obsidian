## 从零开始配置 GitHub Actions 自动更新私有子模块

以下是从零开始配置 GitHub Actions 工作流以自动更新私有子模块的关键步骤：

1.  **在子模块仓库中创建 Deploy Key：**
    * 生成一个新的 SSH 密钥对（公钥和私钥）。
    * 将**公钥**添加到你的私有子模块仓库的 "Settings" -> "Deploy keys" 中，并确保勾选 "Allow write access"。
    * 将**私钥**作为 Secret 添加到你的主仓库的 "Settings" -> "Secrets" -> "Actions" 中（例如，命名为 `SUBMODULE_PRIVATE_KEY`）。

2.  **在你的主仓库中创建 Workflow 文件：**
    * 在你的主仓库的 `.github/workflows` 目录下创建一个 YAML 文件（例如 `update-submodule.yml`）。

3.  **配置 Workflow 的基本信息：**
    ```yaml
    name: 更新子模块

    on:
      schedule:
        - cron: '0 0 * * *' # 设置触发时间（例如每天午夜）
      workflow_dispatch: # 允许手动触发

    permissions:
      contents: write # 授予工作流写入仓库内容的权限
    ```

4.  **定义 Job 和 Steps：**
    ```yaml
    jobs:
      update-submodule:
        runs-on: ubuntu-latest
        steps:
          - name: 🚚 检出主仓库
            uses: actions/checkout@v3

          - name: 🔑 添加 SSH 密钥到 SSH Agent
            uses: webfactory/ssh-agent@v0.8.0
            with:
              ssh-private-key: ${{ secrets.SUBMODULE_PRIVATE_KEY }}

          - name: 📝 配置 Git 使用 SSH
            run: |
              git config --global core.sshCommand "ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no"

          - name: ⬇️ 检出子模块
            run: git submodule update --init --recursive

          - name: 👀 检查子模块更新
            id: check_submodule
            run: |
              cd src/docs/Obisidian # 替换为你的子模块路径
              git fetch origin master # 或你子模块的主分支
              if [[ -n "$(git log --oneline HEAD..origin/master)" ]]; then
                echo "子模块发现新提交, 准备更新……"
                echo "has_updates=true" >> "$GITHUB_OUTPUT"
              else
                echo "子模块没有新提交。"
                echo "has_updates=false" >> "$GITHUB_OUTPUT"
              fi
              cd ../..
              git submodule update --remote # 拉取子模块的最新代码

          - name: 🚀 提交并推送更改到主仓库
            if: steps.check_submodule.outputs.has_updates == 'true'
            env:
              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
            run: |
              git config --global user.email "你的邮箱"
              git config --global user.name "你的用户名"
              git add src/docs/Obisidian # 替换为你的子模块路径
              git commit -m "文章更新"
              git push
    ```

**关键点总结：**

* 使用 Deploy Key 进行私有子模块的身份验证。
* 使用 `webfactory/ssh-agent` Action 加载 SSH 私钥。
* 使用 `git submodule update --init --recursive` 初始化子模块。
* 使用 `git fetch` 和 `git log` 检查子模块是否有更新。
* 使用 `git submodule update --remote` 拉取子模块的最新代码。
* 使用条件步骤 (`if`) 仅在子模块有更新时才提交和推送主仓库。
* 确保你的子模块在 `.gitmodules` 文件中的 `url` 使用的是 SSH 协议 (`git@github.com:...`)。

请根据你的实际仓库和子模块路径替换代码中的相应部分。