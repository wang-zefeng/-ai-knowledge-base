# OmniFlow 项目 Skill 卡片

## 用途

保存 OmniFlow 项目专用的可复用 Skill。

## Skill 卡片格式

```md
## Skill：标题

### 用途
这个 Skill 是用来解决什么问题？

### 适用场景
什么时候用它？

### 输入
使用这个 Skill 前，需要准备什么资料？

### 执行步骤
1.
2.
3.
4.

### 输出结果
执行完应该得到什么？

### 可复用提示词
可以直接复制给 AI 的提示词。

### 注意事项
使用时容易出什么问题？

### 示例
给一个简单例子。

### 成熟度
试用中 / 可复用 / 已稳定
```

---

## Skill 卡片列表

## Skill：OmniFlow 阿里云完整部署流程

### 用途
从零在阿里云 ECS 上部署 OmniFlow，或日常更新代码到最新版本。

### 适用场景
- 新服务器首次部署
- 日常代码更新上线
- 部署后页面不生效需要排查

### 输入
- 阿里云 ECS 公网 IP
- 阿里云 Workbench 终端访问权限
- GitHub 仓库：`https://github.com/wang-zefeng/omni-flow`（main 分支）
- DeepSeek API Key

### 执行步骤

**首次部署：**
1. 检查环境：`node -v && npm -v && which pm2`
2. 拉代码：`cd /root && git clone https://github.com/wang-zefeng/omni-flow.git && cd omni-flow`
3. 配环境变量：`echo 'DEEPSEEK_API_KEY=你的Key' > .env && echo 'PORT=3000' >> .env`
4. 安装构建：`npm install && npm run build`
5. 启动：`pm2 start npm --name "omni-flow" -- run start && pm2 save`
6. 安全组放行：阿里云控制台 → 安全组 → 入方向 → TCP 3000

**日常更新：**
```bash
cd /root/omni-flow && git pull origin main && npm run build && pm2 restart omni-flow
```

**部署后排查：**
```bash
lsof -i :3000           # 端口归属
ps aux | grep omniflow  # 残留进程
kill [PID]              # 杀僵尸
```

### 输出结果
- 浏览器访问 `http://[IP]:3000` 可看到最新版 OmniFlow
- PM2 进程 `omni-flow` 状态 online，restart 计数正常

### 可复用提示词

```
帮我按 OmniFlow-阿里云部署交接文档部署到阿里云。
服务器已在阿里云 Workbench 中登录。
```

### 注意事项
- 旧部署残留必须手动 `kill`，PM2 管不到
- git pull 后必须 `npm run build`，只 pull 不 rebuild 页面不更新
- 浏览器 Ctrl+F5 强制刷新，常规刷新可能显示缓存

### 成熟度
已稳定 ✅
