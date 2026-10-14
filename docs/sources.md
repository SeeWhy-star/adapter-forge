# 原始来源与待核实项

核实日期：2026-10-14。下列是公开资料核实，不代表已完成账号申请、API 实测或源码能力审计。

## PayPal

| 原始来源 | 已核实内容 | 仍需验证 |
| --- | --- | --- |
| [REST API 起步](https://developer.paypal.com/api/get-started/) | OAuth 2.0；沙箱账户流程；文档注明美国以外测试需要 Business account | 用户地区、个人账户资格与申请结果 |
| [沙箱概览](https://developer.paypal.com/sandbox-testing/overview/) | 提供虚拟测试环境 | 本人能否创建应用并调用 API |
| [Webhook 接入指南](https://developer.paypal.com/api/rest/webhooks/rest/) | HTTPS 443 接收；非 2xx 会重投；自验签或回传验签；模拟器不支持回传验签 | 真实事件投递、认证及验证接口实测 |
| [Webhook API](https://developer.paypal.com/api/webhooks/v1) / [官方 OpenAPI](https://github.com/paypal/paypal-rest-api-specifications/blob/main/openapi/notifications_webhooks_v1.json) | POST `/v1/notifications/verify-webhook-signature`；响应 `verification_status` 为 `SUCCESS` 或 `FAILURE` | 实际错误响应和账号权限 |
| [官方事件名称](https://developer.paypal.com/api/rest/webhooks/event-names/) | Payments v2 列出 `COMPLETED`、`PENDING`、`DECLINED` capture 事件，v1 列出 `DENIED` | 冻结 v2 事件样本的具体字段路径 |
| [API 请求说明](https://developer.paypal.com/api/make-api-requests/) | 沙箱基地址为 `https://api-m.sandbox.paypal.com` | 网络可达性及访问条件 |

本版采用回传验签，理由是先验证请求构造和失败处理，控制范围。官方指南更推荐自验签以避免额外 API 依赖；我们的选择会增加网络延迟和服务依赖，必须纳入超时与成本记录。未实现自验签，不能展示为已具备本地密码学验签能力。

官方 OpenAPI 在线读取内容的 SHA-256：`4702a4c2601a087cdde7481f1b55800abb568906e50dde867bb958b4b25afecf`。这是查阅时指纹，本轮未保存全文；正式冻结任务时重新核实来源版本、许可与哈希，保证两组实验使用相同资料包。

## AutoAgents

原始仓库：[liquidos-ai/AutoAgents](https://github.com/liquidos-ai/AutoAgents)。通过 `git ls-remote ... HEAD` 得到 `6301004371aa3fe274a309e493cdd3bfb07592db`，并读取该 commit 的[英文 README](https://github.com/liquidos-ai/AutoAgents/blob/6301004371aa3fe274a309e493cdd3bfb07592db/README.md)。

该版本 README 列出 DeepSeek 支持；项目主体是 Rust，并提供 Python bindings；声明 MIT / Apache-2.0 双许可。这里只核实了 README，没有检查对应实现或执行测试，也没有确认任何待贡献缺口。

因此本轮不使用“给 AutoAgents 新增 DeepSeek”作为任务，也不把已有支持当成我们的成果。不为首个 Java 案例引入其框架。

第二案例可从真实 issue 和源码行为中寻找范围小的缺口，但须先核实缺口仍存在。若使用历史回放，必须冻结修复前 commit、当时资料与任务描述，不提供后续 diff、PR 评论、测试答案或 Git 历史给 Agent；最终 grader 与答案单独保存。即便隔离本地答案，也不能排除模型预训练见过公开修复，因此只能叫历史回放，不能等同真实新贡献或无污染基准。

## 尚未核实的技术条件

- Java / Maven / Docker 本地版本、容器限制与构建依赖可用性：实现前检查，不提前保证。
- 模型供应商的结构化工具调用、usage 字段、取消语义及价格：供应商确定后查官方 API，做最小探针并锁定版本；本轮不选框架或声称模型能力已验证。
- 新宿主或个人 Demo 的具体扩展点：宿主冻结后以代码证据确认，不凭空假设现成接口存在。
