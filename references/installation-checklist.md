# 企微 OpenClaw 插件安装检查清单

## 安装前准备

- [ ] OpenClaw 版本 >= 2026.3.28（运行 `openclaw --version` 确认）
- [ ] Node.js >= 18（运行 `node --version` 确认）
- [ ] 企微管理后台登录权限
- [ ] 服务器有公网 IP（Agent 模式需要）

## Bot 模式安装（个人使用推荐）

- [ ] 企微后台创建机器人：应用管理 → 机器人 → 创建
- [ ] 记录 Bot ID 和 Secret
- [ ] 设置机器人可见范围
- [ ] 运行安装命令：`npx -y @wecom/wecom-openclaw-cli install`
- [ ] 配置 Bot ID 和 Secret：`openclaw channels add` 或命令行
- [ ] 重启 Gateway：`openclaw gateway restart`
- [ ] 发一条消息测试连接

## Agent 模式安装（企业部署推荐）

- [ ] 企微后台创建自建应用：应用管理 → 自建 → 创建应用
- [ ] 记录 CorpID、CorpSecret、AgentId
- [ ] 进入应用 → API 接收消息 → 记录 Token 和 EncodingAESKey
- [ ] ⚠️ 先不点保存
- [ ] 配置 Agent 信息到 OpenClaw
- [ ] 重启 Gateway
- [ ] 回到企微后台，设置回调 URL：`https://域名/plugins/wecom/agent/default`
- [ ] 点击保存，验证通过

## 验证清单

- [ ] `openclaw gateway status` 显示正常
- [ ] 私聊机器人能收到回复
- [ ] 群聊（如需要）能收到回复
- [ ] Cron 任务能正常投递（需要 Agent 模式）
- [ ] 语音消息能正常发送（需要语音 Skill）
