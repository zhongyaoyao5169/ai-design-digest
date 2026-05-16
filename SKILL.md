---
name: ai-design-digest
description: >
  Generate a rich, up-to-date AI × Design daily digest widget — three-section coverage:
  AI 前沿·行业动态 (frontier AI + industry news), 设计·工具 (design tools & media), and
  Builder 观点 (specific recent X posts from curated AI/design builders).
  Use this skill whenever the user asks for:
  - "最新 AI 资讯" / "今日 AI 动态" / "AI 设计资讯" / "推送资讯"
  - "刷新资讯" / "推送最新内容" / "获取今日摘要" / "来一遍"
  - Anything about tracking AI news, design tool updates, builder X posts, or product design trends
  - Requests to build, update, or refresh a news/digest site for AI or design topics
  Always use this skill when the user wants a rendered news digest widget — even if they just
  say "给我看看最新的" or "资讯网站来一下". The skill produces a fully rendered interactive
  HTML widget with language switching (中文/EN/双语) and tab filtering.
---

# AI × Design Daily Digest — 推送流程

## 每次推送前必须完成的三件事

1. Read `references/ai_design_digest_STYLE_REFERENCE.html` — 锁定 CSS、HTML skeleton 和 JS 函数，全部照抄，不做任何修改
2. Read `references/sources.md` — 搜索来源清单（含时间规则：近3天）
3. Read `references/builders.md` — Builder 账号白名单（含日期规则：必须 X月X日）

---

## Step 1 — 读取样式文件（必须最先执行）

Read `references/ai_design_digest_STYLE_REFERENCE.html`。

该文件包含三部分，全部照搬，不做任何修改：
- `<style id="aid-style-locked">` — 完整 CSS 块
- HTML skeleton 注释块 — 页面结构（.aid-page / .aid-hd / .aid-lang / .aid-tabs / .aid-content）
- `<script id="aid-js-locked">` — 包含 `aidApplyTabStyle`、`aidApplyLangStyle`、`aidSetLang`、`aidSetTab` 四个函数

> 样式文件版本锁定，除非用户明确要求修改样式，否则禁止改动任何 CSS 或 JS。

---

## Step 2 — 4路并行搜索（严格限定近3天内）

同时发起以下4条搜索，用当前月份和年份替换占位符：

```
搜索1: "AI news [month] [year] Anthropic OpenAI Google DeepMind xAI Meta AI"
搜索2: "Figma Cursor Zed Raycast design tool update [month] [year]"
搜索3: "Nielsen Norman UX Collective Smashing Magazine design [month] [year]"
搜索4: "[builder handles] post X [month] [year]"
```

**时间规则（不可违反）：**
- 优先抓取发布日期在近3天内的内容（今日往前倒数3天）
- 无当日内容时可向前延伸，但最多3天
- 所有 `time` 字段必须写具体日期，如「5月14日」，禁止写「近期」「最近」「今日」「5月初」「5月中旬」等模糊表达

详细来源清单见 `references/sources.md`。

**目标数量：**
- `news[]`：6–10 条，AI 与 design 各约一半
- `builders[]`：3–6 条，来自 `references/builders.md` 白名单，跨至少2个类别

---

## Step 3 — 按 schema 组装数据

### news[] 字段规范

```json
{
  "cat": "ai 或 design（小写，严格二选一）",
  "title_zh": "完整中文句子，≤25字，禁止关键词堆砌",
  "title_en": "Complete English sentence, ≤20 words, no keyword stuffing",
  "body_zh": "2–3句中文摘要，说清楚是什么、有什么意义",
  "body_en": "2–3 sentence English summary explaining what happened and why it matters",
  "author": "来源机构，格式：机构A · 机构B",
  "tag": "AI 前沿 | 行业动态 | 设计动态 | 工具更新（四选一）",
  "time": "X月X日（具体日期，禁止模糊表达）",
  "url": "原文真实 URL，必须可访问，不得是占位符或编造链接"
}
```

`cat` 与 `tag` 对应关系：
- `cat: "ai"` → `tag: "AI 前沿"` 或 `"行业动态"`
- `cat: "design"` → `tag: "设计动态"` 或 `"工具更新"`

### builders[] 字段规范

```json
{
  "name": "发布者真实姓名（如 Andrej Karpathy）",
  "title_zh": "核心观点，≤25字，完整句子",
  "title_en": "Core point, ≤25 words, complete sentence",
  "body_zh": "2–3句话解释发言背景和意义",
  "body_en": "2–3 sentences explaining context and significance",
  "author": "@twitter_handle · X",
  "time": "X月X日（具体日期，禁止「近期」「最近」「5月初」等）",
  "url": "具体推文链接；无法确认时用账号主页"
}
```

Builder 白名单和搜索模板见 `references/builders.md`。

---

## Step 4 — 渲染 widget

用 `show_widget` 输出。复用 Step 1 读取的 CSS、skeleton 和 JS 函数，只填入数据数组。

**初始化顺序（严格按此顺序，不可调换）：**
```js
aidRender();
var allTabs = Array.from(document.querySelectorAll(".aid-stab"));
aidApplyTabStyle(allTabs[0], true);
allTabs.slice(1).forEach(function(btn){ aidApplyTabStyle(btn, false); });
aidApplyLangStyle(Array.from(document.querySelectorAll(".aid-lt")), 0);
```

这个顺序确保 Tab 激活态（`#CC785C` 背景）和语言切换器激活态（`#1A1A18` 黑底 + `#ffffff` 白字）在渲染后立即正确显示。

---

## 质量红线（渲染前自检）

- [ ] `news[]` 数量 6–10 条，ai / design 各约一半
- [ ] `builders[]` 数量 3–6 条，来自白名单，跨至少2个类别
- [ ] 所有 `time` 字段格式为「X月X日」，无模糊表达
- [ ] 所有 `url` 为真实可访问链接，无占位符
- [ ] 所有 `title` 为完整句子，非关键词堆砌
- [ ] 所有 `body` 不少于2句
- [ ] CSS、HTML skeleton、JS 函数与 STYLE_REFERENCE 完全一致，未做任何改动
