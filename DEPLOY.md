# Wosuo Store - 部署指南

## 前置要求

- Node.js >= 22.0.0
- npm >= 10.0.0
- 服务器内存 >= 1GB

## 方式一：直接部署（推荐）

### 1. 本地构建

```bash
# 安装依赖
npm install

# 执行构建
npm run build

# 构建产物位于 dist/ 目录
```

### 2. 上传服务器

```bash
# 使用 rsync 上传（推荐）
rsync -avz --progress dist/ user@your-server:/var/www/wosuo-store/

# 或使用 scp
scp -r dist/* user@your-server:/var/www/wosuo-store/
```

### 3. 服务器配置

```bash
# SSH 登录服务器
ssh user@your-server

# 进入应用目录
cd /var/www/wosuo-store

# 安装生产依赖
npm install --production

# 创建环境文件
cp .env.example .env
vim .env  # 编辑配置

# 创建日志目录
mkdir -p logs

# 启动服务
npm start
```

### 4. PM2 进程管理（推荐）

```bash
# 全局安装 PM2
npm install -g pm2

# 启动应用
pm2 start ecosystem.config.js

# 保存配置
pm2 save

# 设置开机自启
pm2 startup

# 常用命令
pm2 status          # 查看状态
pm2 logs wosuo-store # 查看日志
pm2 restart wosuo-store # 重启
pm2 stop wosuo-store    # 停止
```

## 方式二：Docker 部署

```bash
# 构建镜像
docker build -t wosuo-store .

# 运行容器
docker run -d \
  --name wosuo-store \
  -p 3000:3000 \
  --restart unless-stopped \
  wosuo-store

# 或使用 docker-compose
docker-compose up -d
```

## 方式三：Docker Compose 部署

```bash
# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

## Nginx 反向代理

```bash
# 复制配置文件
sudo cp deploy/nginx.conf /etc/nginx/sites-available/wosuo-store

# 修改域名和路径
sudo vim /etc/nginx/sites-available/wosuo-store

# 启用配置
sudo ln -s /etc/nginx/sites-available/wosuo-store /etc/nginx/sites-enabled/

# 检查配置
sudo nginx -t

# 重载 Nginx
sudo systemctl reload nginx
```

## 验证部署

访问以下地址验证：

1. **首页**: `http://your-domain.com/`
2. **后台登录**: `http://your-domain.com/admin/login`
3. **健康检查**: `http://your-domain.com/health` (如已配置)

## 故障排查

### 构建失败
```bash
# 清理缓存重试
rm -rf node_modules package-lock.json
npm install
npm run build
```

### 端口占用
```bash
# 查找占用 3000 端口的进程
lsof -i :3000

# 或修改启动端口
PORT=8080 npm start
```

### 权限问题
```bash
# 修改目录权限
chmod -R 755 /var/www/wosuo-store
```

## 环境变量说明

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| NODE_ENV | 运行环境 | production |
| PORT | 服务端口 | 3000 |
| DATABASE_URL | 数据库连接（如需要） | - |

## 文件目录结构

```
dist/
├── client/          # 前端静态资源
├── server/          # 后端代码
├── main.js          # 入口文件
├── package.json     # 生产依赖
└── ecosystem.config.js  # PM2 配置
```
