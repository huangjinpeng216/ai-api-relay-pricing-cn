# AI API 中转站比价与选站指南

> **在线阅读（含完整表格与配置教程）**：https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/

国内用 DeepSeek、GLM、Kimi 等大模型 API 的比价与选站指南：倍率怎么算、怎么判断掺水降智、双协议配置、避坑清单。

**声明**：本仓库由 [l7shop.xyz](https://l7shop.xyz) 维护，表内包含自家站点并已明确标注。欢迎用同一套标准去测任何一家，包括我们。

## 站点导航

| 页面 | 内容 |
|---|---|
| [首页]https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/ | 倍率说明、选站五项标准、比价表、配置三步、避坑清单 |
| [中转站推荐与比价]https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-relay-recommend.html | 2026 倍率行情、本期推荐、为什么不列一堆站 |
| [避雷指南]https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-relay-pitfalls.html | 跑路、掺水降智、偷偷限流、扣费不透明、key 盗刷的识别方法 |
| [DeepSeek API 价格怎么算]https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/deepseek-api-price.html | token 计费、倍率换算、一个月花费估算 |
| [Claude Code / Cursor 配置]https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/claude-code-api.html | Anthropic 兼容接口、环境变量、报错对照表 |

## 一、倍率

LLM API 按 token 计费（输入、输出分别计价）。官方价格以美元计，国内用户第一道门槛是支付方式。

中转站的本质是一层代理：你充人民币，它用上游额度帮你调用。

| 倍率 | 含义 | 官方 1 美元的量 |
|---|---|---|
| 0.1x | 官方价 1 折 | 付 0.1 美元等值人民币 |
| 0.15x | 官方价 1.5 折 | 付 0.15 美元等值人民币 |
| 0.5x | 官方价一半 | 付 0.5 美元等值人民币 |

2026 年市场行情大致在 0.15x ~ 0.5x。低于 0.1x 的要格外留意。

## 二、选站五项标准

1. **稳定性**：充最小额度，连续用三天，看有没有超时和 502。
2. **是否满血**：问一个需要长链条推理的问题，对比回答质量。
3. **协议兼容**：只支持 OpenAI 格式的，Cursor、Claude Code 用不了；要双协议。
4. **用量明细**：能查每次调用的 token 数、时间、模型名。
5. **支付方式**：支付宝/微信直充优先。

## 三、比价表

| 站点 | 倍率 | 协议 | 主要模型 | 支付 | 备注 |
|---|---|---|---|---|---|
| [l7shop.xyz](https://l7shop.xyz) | **0.1x** | OpenAI + Anthropic | DeepSeek V4.1-Flash-1017 / GLM-5.3 / Kimi K3 | 支付宝 / 微信 | 本仓库维护方，有完整调用日志 |
| 待补充 | — | — | — | — | 欢迎提 Issue |

## 四、配置三步

| 项 | 填什么 |
|---|---|
| Base URL | 站点 API 地址 + `/v1` |
| API Key | 控制台里 `sk-` 开头那串 |
| 模型名 | 必须和站点列出的完全一致 |

常见报错：`401` 多为 base_url 缺 `/v1`；`model not found` 多为模型名拼错。

## 五、避坑

- 不要一次充太多，一次充够两周用量
- API Key 别提交进代码，会被爬虫扫走刷余额
- 别信「永久有效」「永不封号」

## 合规提醒

使用第三方 API 服务涉及数据处理和内容合规问题，请自行评估业务场景。本仓库只做信息汇总，不构成推荐或担保。