# WeCom Install

面向 OpenClaw 的企业微信安装与排障 Skill。

这个 Skill 用于把 `@wecom/wecom-openclaw-plugin` 从零安装到可用状态，并覆盖 Bot 模式、Agent 模式、配置校验、回调顺序和常见故障诊断。

## 适用场景

- 需要首次接入企业微信通道
- 已安装插件但发不出消息
- Agent 模式回调校验失败
- 需要把安装、验证、排障流程标准化

## 版本要求

- OpenClaw `>= 2026.3.28`
- Node.js `>= 18`
- 具备企业微信后台管理权限

## 快速开始

1. 安装插件

```bash
npx -y @wecom/wecom-openclaw-cli install
```

2. 配置通道凭证

- Bot 模式：`channels.wecom.botId`、`channels.wecom.secret`
- Agent 模式：`channels.wecom.agent.corpId`、`channels.wecom.agent.corpSecret`、`channels.wecom.agent.agentId`、`channels.wecom.agent.token`、`channels.wecom.agent.encodingAESKey`

3. 重启并验证

```bash
openclaw gateway restart
openclaw gateway status
```

## 模式选择

- Bot 模式：个人或轻量接入，优先推荐
- Agent 模式：企业部署、回调消息、Cron 投递
- 双模式：只在明确需要时启用

## 仓库内容

- `SKILL.md`：主工作流
- `agents/openai.yaml`：供 Agent 直接调用的结构化入口
- `references/installation-checklist.md`：安装检查清单
- `references/troubleshooting.md`：常见错误与修复路径

## 质量边界

- 先区分安装问题、凭证问题、网络问题
- 每个关键步骤后立即验证，不把所有问题堆到最后
- Agent 模式必须先启动 Gateway，再去企微后台保存回调

## License

MIT
