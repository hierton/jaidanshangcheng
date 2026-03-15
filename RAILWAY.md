# Railway 免费部署指南

## 步骤

1. 注册 [Railway](https://railway.app/)（支持 GitHub 登录）
2. 创建新项目 → Deploy from GitHub repo
3. 选择本项目仓库
4. 添加环境变量：
   - `NODE_ENV=production`
   - `PORT=3000`
5. 自动生成域名（如 `wosuo-store.up.railway.app`）

## 绑定自定义域名

1. Railway Dashboard → Settings → Domains
2. 添加你的域名
3. 在域名解析商添加 CNAME 记录指向 Railway 提供的地址
