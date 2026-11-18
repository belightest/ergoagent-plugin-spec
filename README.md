# ergoagent-plugin-spec

ErgoAgent 插件规范——第三方插件与核心产品之间的**合同**。

ErgoAgent 核心闭源（见《ErgoAgent 免费使用许可》），但生态开放：本仓库定义插件如何声明能力、如何签名、如何提交、如何被分发，以及滥用如何处理。本仓库内容（规范、Schema、示例）以 [Apache-2.0](LICENSE) 许可发布。

## 文档目录

| 文档 | 内容 |
|---|---|
| [01 · Manifest](docs/01-manifest.md) | `ergoagent.plugin.json` 字段定义与示例 |
| [02 · 能力分级](docs/02-capability-tiers.md) | T0/T1/T2 三级能力模型、陌生人可发布范围、运行时强制 |
| [03 · 签名与上架](docs/03-signing-and-submission.md) | 密钥签名、PR 提交流程、机器门禁清单、人工审核例外、安全评级 |
| [04 · 网络申报与安装同意](docs/04-network-and-consent.md) | `network.domains` 申报、安装时权限明示与风险提示、运行时一次性授权 |
| [05 · 滥用处理与吊销](docs/05-abuse-and-revocation.md) | 举报渠道、响应时限、签名吊销、下架与事件手册 |

## 插件作者五分钟上手

1. 读 [02 能力分级](docs/02-capability-tiers.md)，确认你的插件属于哪一级（**T2 暂不向第三方开放**）
2. 按 [01 Manifest](docs/01-manifest.md) 写 `ergoagent.plugin.json`——**未声明的能力一律拒绝**
3. 若插件访问网络：按 [04](docs/04-network-and-consent.md) 申报全部域名，并附 `PRIVACY.md`
4. 本地跑 `ergoagent-plugin-toolkit validate`（校验清单见 [03](docs/03-signing-and-submission.md)）
5. 签名（[03](docs/03-signing-and-submission.md)），向插件注册仓库提 PR

## 设计原则

1. **能力即威胁面**——manifest 没声明就没有该能力；分级决定审核深度
2. **机器全量审，人只审例外**——阻断性检查自动化，人工只处理警告与申诉
3. **签名追责代替平台担保**——作者密钥签名上架，滥用可追溯、可吊销
4. **运行时兜底**——能力档位、逐项审批、工作区边界、预算熔断在客户端强制执行；审核只是准入层
5. **公开可审计**——源码仓库、隐私声明、扫描报告对所有人可见

## 状态

v1.0 草案（2026-11-19）。规范演进遵循语义化版本；破坏性变更提前一个 minor 版本预告。
