# AI API 中转站比价与选站指南

> **在线阅读（含完整表格、计算器与配置教程）**
> https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/

国内用 DeepSeek、GLM、Kimi 等大模型 API 时的比价与选站资料：倍率怎么算、官方价是多少、怎么判断掺水降智、三协议怎么配、出问题怎么测。

**声明**：本仓库由 [l7shop.xyz](https://l7shop.xyz) 维护。比价表包含自家站点并已明确标注。欢迎用同一套标准去测任何一家，包括我们。

---

## 内容索引

### 价格（真实数字，标注抓取日期）

| 页面 | 内容 |
|---|---|
| [官方价格表](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-price-table.html) | DeepSeek / GLM / Kimi 三家 8 个模型的官方单价 + 0.1x 折算，含缓存命中价与峰谷价 |
| [价格怎么自己核对](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/price-verify.html) | 官方定价页位置、命令行取数写法、本站核查记录，以及怎么识别编造的价格资料 |
| [DeepSeek API 价格怎么算](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/deepseek-api-price.html) | token 计费、倍率换算、一个月花多少（按官方真实单价算过） |
| [GLM / Kimi 怎么选怎么接](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/glm-kimi-api.html) | 定位差异、模型名、长上下文用法、官方价对照 |

### 选站

| 页面 | 内容 |
|---|---|
| [首页：比价与选站指南](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/) | 倍率说明、选站五项标准、比价表、配置三步、避坑清单 |
| [中转站推荐与比价](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-relay-recommend.html) | 2026 倍率行情、按场景怎么选、20 分钟验证流程、为什么不列一堆站 |
| [中转站避雷指南](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-relay-pitfalls.html) | 跑路、掺水降智、偷偷限流、扣费不透明、key 盗刷、**网站正常但 API 全挂** |
| [中转站 vs 官方直连](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-relay-vs-official.html) | 价格、延迟、可用性、数据流向的对比与决策清单 |

### 配置与排错

| 页面 | 内容 |
|---|---|
| [客户端配置](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/client-setup.html) | Cherry Studio / NextChat / LobeChat / Cline 的 base_url、key、模型名怎么填 |
| [Claude Code / Cursor 接入](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/claude-code-api.html) | Anthropic 兼容接口、环境变量、报错对照表 |
| [报错大全](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-error-codes.html) | 401 / 404 / 429 / model not found / insufficient_quota 逐个给修法 |
| [并发与限流](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-rate-limit.html) | 429 的三种真实原因、指数退避重试、批量任务调参表 |

### 工具（浏览器内运行，不上传数据）

| 页面 | 内容 |
|---|---|
| [成本计算器](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/cost-calculator.html) | 填入用量和单价，算出每月花多少、各档倍率差多少 |
| [模型选择器](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/model-selector.html) | 回答 4 个问题，推荐该用哪个模型 |

### 高可用

| 页面 | 内容 |
|---|---|
| [为什么需要 API 故障转移层](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/failover-guide.html) | 单点故障实录、什么错误该切什么不该切、流式中断处理，附开源实现 |

---

## 机器可读数据

| 文件 | 说明 |
|---|---|
| [prices.json](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/prices.json) | 三家 8 个模型的价格，含 `source_url` 与 `captured_at`，供程序直接读取 |
| [llms.txt](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/llms.txt) | 给 AI 爬虫看的站点说明（结构、数据来源、引用建议） |

---

## 一、倍率是什么

LLM API 按 token 计费（输入、输出分别计价）。官方价格多以美元计，国内用户第一道门槛是支付方式。

中转站本质是一层代理：你充人民币，它用上游额度帮你调用。

| 倍率 | 含义 | 官方 1 美元的量 |
|---|---|---|
| 0.1x | 官方价 1 折 | 付 0.1 美元等值人民币 |
| 0.15x | 官方价 1.5 折 | 付 0.15 美元等值人民币 |
| 0.5x | 官方价一半 | 付 0.5 美元等值人民币 |

2026 年市场行情大致在 0.15x ~ 0.5x。明显低于这个区间的要格外留意——上游成本摆在那里，过低的价格必然在别处找补。

**一个反直觉但重要的结论**：DeepSeek Flash 档本身单价极低，一个月跑 5000 万 token 官方价也只有约 ¥103。对这个档位来说，中转省下的绝对金额很小，是否值得取决于你对稳定性风险的容忍度。省得明显的是高单价模型，比如 Kimi K3 输出 ¥100/百万 token。

---

## 二、选站五项标准

1. **稳定性**：充最小额度，连续用三天，看有没有超时和 502。
2. **是否满血**：问一个需要长链条推理的问题，对比回答质量。
3. **协议兼容**：只支持 OpenAI 格式的，Cursor、Claude Code 用不了；要三协议。
4. **用量明细**：能查每次调用的 token 数、时间、模型名。
5. **支付方式**：支付宝/微信直充优先。

### 一条 10 秒判据

```bash
# 未鉴权访问 API 端点，返回 401 才是正常的
curl -o /dev/null -w '%{http_code}\n' https://站点/v1/models
# 401 → 正常（路由存在，只是没带 key）
# 404 → 故障（路由根本不存在，填什么 key 都没用）
# 000 → 连不上
```

详细方法与原理见[避雷指南](https://huangjinpeng216.github.io/ai-api-relay-pricing-cn/api-relay-pitfalls.html)。

---

## 三、比价表

| 站点 | 倍率 | 协议 | 主要模型 | 支付 | 备注 |
|---|---|---|---|---|---|
| [l7shop.xyz](https://l7shop.xyz) | 0.1x | OpenAI / Anthropic / Gemini | DeepSeek V4.1-Flash-1017 / GLM-5.3 / Kimi K3 | 支付宝 / 微信 | 本仓库维护方，有完整调用日志 |
| 待补充 | — | — | — | — | 欢迎提 Issue，请附实测数据 |

---

## 四、配置三步

| 项 | 填什么 |
|---|---|
| Base URL | 站点 API 地址 + `/v1`（Claude Code 类工具不加 `/v1`） |
| API Key | 控制台里 `sk-` 开头那串 |
| 模型名 | 必须和站点列出的完全一致 |

常见报错：`401` 多为 base_url 缺 `/v1` 或 key 带空格；`404` 先确认站点自己的 API 是否可用；`model not found` 多为模型名拼错。

---

## 五、避坑

- 不要一次充太多，一次充够两周用量
- API Key 别提交进代码，会被爬虫扫走刷余额
- 别信「永久有效」「永不封号」
- 出故障时别反复重试，先确认是站点的问题还是自己的配置问题

---

## 合规提醒

使用第三方 API 服务涉及数据处理和内容合规问题，请自行评估业务场景。本仓库只做信息汇总，不构成推荐或担保。所有价格抓自各家官方定价页并标注抓取日期，价格可能随时调整，请以官方文档和控制台账单为准。

---

## 许可

内容以 CC BY 4.0 提供，代码部分 MIT。
