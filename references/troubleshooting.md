# 企微 OpenClaw 故障排查速查表

## 快速诊断流程

```
企微不通？
├─ 插件没装？ → npx -y @wecom/wecom-openclaw-cli install
├─ 插件装了但没启用？ → openclaw config set plugins.entries.'wecom-openclaw-plugin'.enabled true
├─ 通道没启用？ → openclaw config set channels.wecom.enabled true
├─ Bot 模式连不上？
│  ├─ 检查网络：ping openws.work.weixin.qq.com
│  ├─ 检查凭证：Bot ID / Secret 是否正确
│  └─ 检查版本：openclaw --version >= 2026.3.28
├─ Agent 模式回调失败？
│  ├─ Gateway 是否先重启？ → 必须先重启再在企微后台保存
│  ├─ 回调 URL 是否正确？ → https://域名/plugins/wecom/agent/default
│  └─ Token/AESKey 是否完整？ → AESKey 必须 43 字符
└─ 收不到消息？
   ├─ dmPolicy 是否为 open？ → openclaw config get channels.wecom.dmPolicy
   └─ 群聊不通？ → 检查 groupPolicy 和 groupAllowFrom
```

## 常见错误代码

| 错误 | 原因 | 解决 |
|------|------|------|
| `connect ECONNREFUSED` | Gateway 未启动 | `openclaw gateway start` |
| `401 Unauthorized` | Bot ID/Secret 错误 | 企微后台重新获取 |
| `404 Not Found` | 回调路径错误 | 使用 `/plugins/wecom/agent/default` |
| `ETIMEDOUT` | 网络不通 | 检查防火墙/代理 |
| `invalid signature` | Token/AESKey 不匹配 | 检查企微后台和配置是否一致 |
| `callback verify failed` | 先保存了企微再启动 Gateway | 顺序反了，先重启 Gateway 再保存 |
| `plugin not found` | 插件未安装 | `openclaw plugins install @wecom/wecom-openclaw-plugin` |
| `version mismatch` | OpenClaw 版本太旧 | `openclaw update` 或升级到 >= 2026.3.28 |

## 一键修复脚本

```bash
# 完整修复流程
echo "=== 企微通道修复 ==="
echo "1. 检查版本..."
openclaw --version

echo "2. 安装/更新插件..."
npx -y @wecom/wecom-openclaw-cli install --force

echo "3. 检查配置..."
openclaw config get channels.wecom.enabled
openclaw config get channels.wecom.botId

echo "4. 重启 Gateway..."
openclaw gateway restart

echo "5. 检查状态..."
sleep 3
openclaw gateway status

echo "=== 修复完成，请发一条消息测试 ==="
```
