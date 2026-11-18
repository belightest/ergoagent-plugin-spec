# 01 · Manifest（`ergoagent.plugin.json`）

每个插件根目录必须包含 `ergoagent.plugin.json`。这是插件与核心产品、市场、用户之间的唯一契约文件。**字段缺失或与实际行为不符 = 审核阻断**。

## 完整 Schema（v1）

```jsonc
{
  // 固定值，标识规范版本
  "schema": "ergoagent.plugin/v1",

  // 反向域名式唯一 ID，提交后不可更改
  "id": "com.example.weather-lookup",

  // 展示名（与实际功能一致，禁夸大）
  "name": "天气查询",

  // 语义化版本；升级须重新签名与扫描
  "version": "1.2.0",

  // 作者与联系方式（有人维护的渠道，至少一个）
  "author": {
    "name": "示例团队",
    "contact": "https://github.com/example/ergoagent-weather/issues",
    "signing_key": "base64…  // 作者公钥指纹（SHA-256 前 16 位）"
  },

  // 公开源码仓库——必须指向具体仓库，不接受组织主页或无说明的 monorepo 根
  "source": "https://github.com/example/ergoagent-weather",

  // 插件自身代码的许可证（SPDX 标识）
  "license": "MIT",

  // 风险等级自声明：low | medium | high（就高不就低，规则见 02）
  "risk_level": "medium",

  // 能力声明——白名单制，未声明的能力运行时不可用（规则见 02）
  "capabilities": [
    "network:api.weather.com",     // 访问指定域名
    "workspace:read"               // 只读工作区
  ],

  // 网络申报（有 network:* 能力时必填，规则见 04）
  "network": {
    "domains": ["api.weather.com", "*.tile.openweathermap.org"]
  },

  // 隐私政策（必填）：收集什么、存在哪、发给谁；无收集也要明示"不收集"
  "privacy": "https://github.com/example/ergoagent-weather/blob/main/PRIVACY.md",

  // 要求的最低客户端版本
  "min_client": "1.0.4",

  // 入口（相对包内路径）
  "entry": "dist/index.js"
}
```

## 字段规则

| 字段 | 规则 |
|---|---|
| `id` | 反向域名式；全局唯一；**提交后不可改**（改名 = 新插件） |
| `version` | 语义化；每次提交必须递增；升级后**必须重新签名 + 重新扫描**，旧评分不继承 |
| `author.contact` | 必须真实可维护；仓库 Issue 或邮箱；失效超过合理时限无响应的插件将被标记「维护缺失」并限制新安装 |
| `source` | 公开可读；审核者与用户都靠它理解包体证明不了的行为 |
| `risk_level` | 多档并存就高；**故意低报 = 审核阻断 + 记录在案** |
| `capabilities` | 白名单制；运行时按声明注入能力，未声明即不存在；T2 能力对第三方关闭（见 02） |
| `network.domains` | 精确域名或 `*.` 前缀通配；**静态分析抓不到运行时拼接的 URL，申报是唯一可见性手段**（见 04） |
| `privacy` | 必填；敏感数据（健康/金融/儿童）相关插件面临额外审查 |
| `min_client` | 语义化版本号；低于此版本的客户端拒绝安装并提示升级 |

## 与包体的关系

- 提交物是打包文件（`.ergoplugin`），不是源码树；审核者通过 `source` 字段核对行为
- 包内禁止包含：`.git/`、虚拟环境、缓存、日志、`.env`、任何密钥/凭证/私钥
- 包内文件清单须与 manifest、README、PRIVACY 描述一致

## 兼容性

- 本 Schema 版本化；破坏性变更递增 major，并提前一个 minor 版本在本文档与仓库 Release Notes 预告
- 客户端遇到不认识的 `schema` 值：拒绝安装并提示升级客户端
