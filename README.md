[README.md](https://github.com/user-attachments/files/27851952/README.md)
# AI & Design Daily Digest

每日推送 AI 前沿动态 × 设计工具更新 × Builder 观点的交互式资讯摘要，由 Claude Cowork Skill 驱动。

---

## 内容覆盖

| 版块 | 内容 |
|------|------|
| 🤖 AI 前沿 · 行业动态 | Anthropic、OpenAI、Google DeepMind、xAI、Meta AI 等模型发布与行业动态 |
| 🎨 设计 · 工具 | Figma、Cursor、Zed、Midjourney、Lovart 等设计工具更新，NN/g、Smashing Magazine 等设计媒体 |
| 💬 Builder 观点 | 来自 25 位 AI/设计/产品领域 builder 的近期 X 发帖（@karpathy、@karrisaarinen、@rauchg 等） |

资讯严格限定**近 3 天内**发布，支持中文 / EN / 双语三种语言切换。

---

## 项目结构

```
ai-design-digest/
├── SKILL.md                                       # 完整推送流程
└── references/
    ├── ai_design_digest_STYLE_REFERENCE.html      # 锁定样式（CSS + JS）
    ├── sources.md                                 # 搜索来源 + 时间规则
    └── builders.md                                # Builder 账号白名单（25人）
```

---

## 使用方式

### 在 Claude Cowork 中推送

安装 `ai-design-digest.skill` 后，直接说：

> 推送资讯

Claude 会自动执行 4 路并行搜索 → 组装数据 → 渲染交互式摘要 widget。

### 自定义

- **修改来源**：编辑 `ai-design-digest/references/sources.md`
- **增减 Builder**：编辑 `ai-design-digest/references/builders.md`
- **修改样式**：编辑 `ai-design-digest/references/ai_design_digest_STYLE_REFERENCE.html`（修改后需重新打包 `.skill`）

---

## 推送规则

- 时间范围：近 3 天内发布的内容，`time` 字段格式为「5月14日」，禁止写「近期」「最近」等模糊表达
- Builder 帖子：必须来自白名单 25 个账号，引用真实发帖，禁止编造
- 数量：新闻 6–10 条（AI / 设计各半），Builder 观点 3–6 条

---

## 技术栈

- **前端**：纯 HTML + CSS + JavaScript，无框架依赖
- **部署**：Vercel（目前直接使用skill做推送，不做在线网站）
- **驱动**：Claude Cowork Skill（`SKILL.md` 定义完整工作流）
- **样式**：Georgia serif 标题，暖米色 `#faf9f5` 背景，珊瑚色 `#C84B2F` 强调色
