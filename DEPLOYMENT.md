# 🚒 消防数字人部署指南

## 🌐 生产环境部署要求

### ⚠️ 重要提醒
- **麦克风功能需要HTTPS**：浏览器安全策略要求HTTPS才能访问麦克风
- **海外服务器连接限制**：可能需要网络优化

## 🔧 HTTPS部署方案

### 方案1：使用反向代理 (推荐)
```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://127.0.0.1:5173;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 方案2：Cloudflare + 域名
1. 添加域名到Cloudflare
2. 启用SSL/TLS加密
3. 设置DNS记录指向服务器IP
4. 通过 https://your-domain.com 访问

### 方案3：Let's Encrypt免费证书
```bash
# 安装certbot
sudo apt install certbot

# 获取证书
sudo certbot certonly --standalone -d your-domain.com

# 配置证书自动续期
sudo crontab -e
# 添加: 0 12 * * * /usr/bin/certbot renew --quiet
```

## 🌍 海外部署网络优化

### 1. API连接优化
```bash
# 检查网络连通性
curl -I https://api.stepfun.com/v1/realtime

# 使用代理(如需要)
export https_proxy=your-proxy-server:port
```

### 2. 防火墙设置
```bash
# 开放端口
sudo ufw allow 5173
sudo ufw allow 443
sudo ufw allow 80
```

### 3. DNS设置
```bash
# 优化DNS解析
echo "nameserver 8.8.8.8" >> /etc/resolv.conf
echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

## 📱 客户端访问指南

### 浏览器设置
1. **Chrome**: 设置 → 隐私和安全 → 网站设置 → 麦克风 → 允许
2. **Firefox**: 设置 → 隐私与安全 → 权限 → 麦克风 → 设置
3. **Safari**: Safari菜单 → 偏好设置 → 网站 → 麦克风

### 测试步骤
1. 访问 https://your-domain.com
2. 点击"服务器设置"填写API Key
3. 点击"点击连接"
4. 允许麦克风权限提示
5. 开始对话测试

## 🚀 快速部署命令

### 使用PM2部署
```bash
# 安装PM2
npm install -g pm2

# 启动应用
pm2 start "bun dev" --name "firefighter-ai"

# 查看状态
pm2 status

# 查看日志
pm2 logs firefighter-ai
```

### Docker部署
```dockerfile
FROM node:18
WORKDIR /app
COPY package.json bun.lockb ./
RUN npm install -g bun
RUN bun install
COPY . .
EXPOSE 5173
CMD ["bun", "dev", "--host", "0.0.0.0"]
```

## 🔍 故障排除

### 常见问题

1. **麦克风无法访问**
   - 确认使用HTTPS访问
   - 检查浏览器权限设置
   - 确认设备有麦克风

2. **API连接失败**
   - 验证API Key有效性
   - 检查网络连通性
   - 确认账户余额充足

3. **页面加载慢**
   - 使用CDN加速
   - 启用gzip压缩
   - 优化图片资源

## 📞 技术支持

如果遇到部署问题：
1. 查看浏览器控制台错误
2. 检查服务器日志
3. 测试网络连通性
4. 验证证书配置

---

🚒 **消防数字人 - 专业安全，触手可及**