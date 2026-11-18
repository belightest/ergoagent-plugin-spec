# 参与贡献

欢迎参与本规范的改进。本仓库接受 PR，但请注意：**这是规范仓库，不是插件提交处**——插件提交见 [03 · 签名与上架](docs/03-signing-and-submission.md) 的 `ergoagent-plugins` 注册仓库。

## 签署（DCO）

所有 commit 必须携带 `Signed-off-by`（开发者原创证书）：

```bash
git commit -s   # 自动附加 Signed-off-by: 姓名 <邮箱>
```

签署即声明：你有权提交该内容，并同意其按本仓库的 [Apache-2.0](LICENSE) 许可发布。未签署的 commit 将在 CI 被阻断。

## 提交流程

1. Fork 并新建分支（`spec/xxx` 或 `docs/xxx`）
2. 修改后本地自查：文档间引用有效、示例 JSON 可解析、术语一致
3. PR 描述写清：改什么、为什么、影响哪些文档
4. 破坏性变更（breaking）必须在 PR 标题标注 `BREAKING:` 并给出迁移说明

## 术语约定

- **能力（capability）**：插件可使用的受控资源，白名单制
- **申报（declaration）**：manifest 中显式声明的事实；未申报视为不存在
- **门禁（gate）**：提交时的自动化阻断检查
- **评级（grade）**：S/A/B/C/Unscanned，绑定具体版本
- **吊销（revocation）**：对签名指纹的永久性禁用

## 行为准则

对事不对人；安全相关发现请走私密渠道，不在公开 Issue 披露利用细节。
