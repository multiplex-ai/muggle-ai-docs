# 按角色快速入门

选择你的角色，查看借助 Muggle Test 最快获得价值的路径。

## 快速导航

| 角色 | 最适合 | 跳转到 |
| :--- | :------- | :------ |
| **开发者** | 本地构建、在 CI/CD 中测试、放心发布 | [开发者指南](#developers) |
| **Vibe Coder** | 提示词驱动的开发，配合预览网址测试 | [Vibe Coder 指南](#vibe-coders) |
| **产品团队** | 基于 PRD 生成测试，以及问题分级处理工作流 | [产品团队指南](#product-team) |

**常用资源**：[排查失败](#investigating-failures) | [管理已发现的流程](#managing-discovered-flows) | [查看结果](#reviewing-results) | [后续步骤](#next-steps)

---

## 开发者

**你的工作流**：构建功能 → 本地测试 → 修复问题 → 放心发布

### 推荐路径

| 步骤 | 操作 | 指南 |
| :--- | :----- | :---- |
| 1 | 为你的 AI 助手安装 Muggle Test Local | [本地测试安装与配置](local-testing/setup.md) |
| 2 | 编码时测试你的 localhost 应用 | [本地测试示例](local-testing/examples.md) |
| 3 | 根据测试报告修复问题 | [排查失败](#investigating-failures) |
| 4 | 为你的预发布环境创建项目 | [运行你的第一批测试](quickstart-running-tests.md) |
| 5 | 集成到 CI/CD 中，实现自动化回归测试 | [CI/CD 集成](mcp/mcp-cicd-integration.md) |

### 关键工具

- **本地测试**：直接从 Cursor、Claude 或 VS Code 测试 localhost
- **MCP 集成**：让你的 AI 助手在你编码时运行测试
- **CI/CD 钩子**：关键用户旅程失败时阻止部署

---

## Vibe Coder

**你的工作流**：用提示词描述功能 → 在云端验证 → 快速修复 → 放心发布

### 推荐路径

| 步骤 | 操作 | 指南 |
| :--- | :----- | :---- |
| 1 | 将 Muggle Test 连接到你的 AI 助手 | [MCP 快速入门](mcp-quickstart.md) |
| 2 | 让你的 AI 测试你的预览网址 | [本地测试示例](local-testing/examples.md) |
| 3 | 查看报告并修复问题 | [排查失败](#investigating-failures) |
| 4 | 重新测试，直到检查通过 | 重复第 2 步 |

### 关键工具

- **MCP 技能**：只需说“测试我的应用”，剩下的交给你的 AI 处理
- **预览网址测试**：支持 Vercel、Netlify、Cloudflare 预览
- **快速反馈循环**：几分钟内就能出结果，而不是几小时

### 示例提示词

```
"Test my Vercel preview at https://my-app-abc123.vercel.app"

"Run Muggle Test on my staging site and tell me what's broken"

"Check if the signup flow works on localhost:3000"
```

---

## 产品团队

**你的工作流**：导入需求 → 生成测试 → 审阅发现的问题 → 分级处理修复

### 推荐路径

| 步骤 | 操作 | 指南 |
| :--- | :----- | :---- |
| 1 | 为你的预发布环境创建项目 | [运行你的第一批测试](quickstart-running-tests.md) |
| 2 | 上传你的 PRD 或需求文档 | [上传 PRD 文件](concepts/prd-upload.md) |
| 3 | 审阅并批准生成的用户旅程 | [管理已发现的流程](#managing-discovered-flows) |
| 4 | 运行测试并查看报告 | [查看结果](#reviewing-results) |
| 5 | 按严重程度对问题分级处理，并分配给开发者 | [排查失败](#investigating-failures) |

### 关键工具

- **PRD 上传**：从现有文档中提取测试场景
- **AI 生成的用户旅程**：用自然语言描述你想测试的内容
- **可分享的报告**：将发现的问题导出到 Jira、Linear 或 Slack

### 可上传的内容

| 格式 | 最适合 |
| :----- | :------- |
| PDF | 产品规格说明、设计文档 |
| DOCX | 需求文档、用户故事 |
| TXT | 纯文本规格说明 |

---

## 通用参考

### 排查失败

对于每个失败的测试，你可以看到：

| 信息 | 用途 |
| :---------- | :------ |
| **操作** | 失败的具体步骤 |
| **截图** | 失败时的页面视觉状态 |
| **错误消息** | 用于调试的技术细节 |
| **页面网址** | 失败发生的位置 |

### 管理已发现的流程

| 操作 | 使用时机 |
| :----- | :---------- |
| **重命名** | 与你所在领域的用语保持一致 |
| **停用** | 排除你不关心的流程 |
| **设为优先** | 标记关键业务流程 |

### 查看结果

使用筛选条件聚焦于重要内容：

| 筛选条件 | 用途 |
| :----- | :------- |
| **状态** | 仅显示通过或失败的测试 |
| **流程名称** | 聚焦于特定的用户旅程 |
| **严重程度** | 优先处理影响大的问题 |

---

## 后续步骤

| 目标 | 资源 |
| :--- | :------- |
| 了解 Muggle Test 的工作原理 | [Muggle Test 的工作原理](concepts/how-muggle-test-works.md) |
| 搭建自动化测试 | [CI/CD 集成](mcp/mcp-cicd-integration.md) |
| 了解配额和定价 | [订阅套餐](billing/subscription-plans.md) |
| 解决问题 | [常见问题](troubleshooting/common-issues.md) |
