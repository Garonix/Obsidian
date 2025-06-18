`git tag -a v1.0 -m "Release version 1.0"`
默认情况下，`git push` 命令并不会把标签推送到远程仓库。你需要显式地推送标签： 推送单个标签：

```
git push origin v1.0
```