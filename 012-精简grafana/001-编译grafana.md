## 1. 克隆代码
```bash 
git clone https://github.com/grafana/grafana.git 
``` 
## 2. 安装 nvm
```bash 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash source ~/.nvm/nvm.sh nvm --version 
``` 
## 3. 安装 Node.js
```bash 
nvm list-remote nvm install <version> node -v npm -v 
``` 
> 切换版本 
```bash 
nvm use <version> 
``` 
> 设置默认版本 
```bash
nvm alias default <version>
```
## 4. 安装 Yarn
```bash
npm install -g yarn 
yarn --version
```
## 5. 编译前端
```bash 
# 安装前端依赖 
yarn install 
# 编译
export NODE_OPTIONS="--max_old_space_size=4096"
yarn build 
``` 
## 6. 编译后端
需要安装go-1.23版本
```bash 
make build-go 
``` 
## 7. 启动
```bash 
./bin/linux-amd/grafana server
``` 
访问本地3000端口