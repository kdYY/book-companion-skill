---
name: book-companion
description: 通用企业读书伴侣 — 同一本书，不同岗位看到不同的价值。/bc init 创建项目 → /bc guide 岗位化路径 → /bc explore 深度引导 → /bc discuss 苏格拉底讨论 → /bc club 读书会运营
license: MIT
---

# Book Companion 通用企业读书伴侣

同一本书，不同岗位看到不同的价值。AI 帮你精准匹配，激发兴趣，引导读纸质书。

## 设计原则（铁律）

1. **方法论不外露** — 不在引导文档中提及 SQ3R/费曼/布鲁姆等方法名，但逻辑嵌入流程
2. **批判与证据等比** — 引导问题必须引用原文锚点（人名+引言+场景），无锚点不输出
3. **行动三要素** — 每个行动建议必须包含：具体事 + 产出物 + 时间框架

## 质量门禁（5 项检查，每次 guide/explore 输出必须通过）

所有引导类输出交付前必须逐项验证：

- [ ] **锚点密度** — 每个核心观点有原文引用（人名/引言/场景）
- [ ] **岗位匹配度** — 引导问题与目标岗位的核心关注点对齐
- [ ] **行动可执行性** — 行动建议包含三要素（具体事+产出物+时间框架）
- [ ] **方法论隐身** — 全文无 SQ3R/费曼/布鲁姆等方法名出现
- [ ] **纸质书引导** — 至少一处自然引导读者回到纸质书原文

## 命令路由

| 命令 | 功能 | 核心引用 |
|------|------|---------|
| `/bc init` | 创建新书项目 | `references/role-profiles.md` |
| `/bc guide` | 岗位化阅读路径 | `references/role-profiles.md`, `references/methodology.md`, `references/quality-gate.md` |
| `/bc explore` | 章节深度引导 | `references/v4-template.md`, `references/role-profiles.md`, `references/patterns.md`, `references/quality-gate.md`, `references/wiki-hierarchy.md` |
| `/bc discuss` | 苏格拉底式讨论 | `references/quality-gate.md` |
| `/bc club` | 读书会运营 | `references/club-engine.md` |

项目根目录：`~/.claude/books/{slug}/`

---

## `/bc init` — 创建新书项目

**触发词**: `/bc init`, `创建读书项目`, `新建书籍`, `init book`

### 输入

- 书名（必填）
- 目录结构（文字 / 照片 OCR / 语音 — 任选一种）
- 行业选择（从 `references/role-profiles.md` 行业列表中选择）
- 参与岗位（可选，从行业对应岗位中选子集；不选则默认全部岗位）

### 流程

1. 生成 slug：书名拼音/英文缩写，如 `principles-ray-dalio`
2. 创建项目目录 `~/.claude/books/{slug}/`，含子目录：
   - `chapters/` — 原文提取
   - `guides/` — 岗位化引导
   - `discussions/` — 讨论记录
   - `club/` — 读书会
3. 生成 `_config.json`：
   ```json
   {
     "title": "书名",
     "slug": "slug",
     "industry": "行业",
     "chapters": [{"id": 1, "title": "章节名", "status": "pending"}],
     "createdAt": "ISO时间"
   }
   ```
4. 从 `references/role-profiles.md` 加载对应行业的岗位列表，写入配置。如果用户指定了参与岗位子集，只在 `activeRoles` 中写入选定的岗位；否则写入全部岗位。`roles` 数组保留全部岗位信息供参考，`activeRoles` 数组只包含本次参与的岗位 slug
5. 创建 `_glossary.md`（空术语表，后续自动积累）
6. 创建 `_progress.json`（阅读进度追踪，初始全 pending）。如果文件已存在则跳过，否则必须创建。init 完成后验证这 2 个文件存在。
6b. 验证项目完整性：确认 _config.json、_glossary.md、_progress.json 均已创建，目录 chapters/、guides/、discussions/、club/ 均已存在。
7. 可选：输入章节原文（文字/OCR/语音），提取摘要存入 `chapters/{id}.md`

### 输出

项目骨架 + `_config.json` + 目录结构确认

---

## `/bc guide` — 岗位化阅读路径

**触发词**: `/bc guide`, `阅读路径`, `岗位引导`, `guide`

### 输入

- 岗位选择（从 `_config.json` 行业对应岗位列表选）
- 经验层级（新手 0-2年 / 老手 2年+）

### 流程

1. 从 `_config.json` 读取章节数和行业
2. 从 `references/role-profiles.md` 读取岗位子画像（阅读目标 / 核心关注 / 思维模式 / 典型问题）
3. 从 `references/methodology.md` 读取岗位 x 层级 -> 方法论映射
4. 基于章节摘要生成：
   - 必读章节（标注原因）
   - 选读章节（标注原因）
   - 推荐阅读顺序
   - 每章引导问题（2-3 个）
   - 阅读方法建议（隐含方法论，不提方法名）
5. 执行质量门禁 5 项检查

### 输出

岗位专属阅读路径文档，写入 `guides/{role}-path.md`

---

## `/bc explore` — 章节深度引导（核心命令）

**触发词**: `/bc explore`, `深度引导`, `章节引导`, `explore`

### 输入

- 章节编号（从 `_config.json` 选择）
- 岗位（同上）
- 经验层级（同上）

### 流程

1. 读取章节原文（`chapters/{id}.md`）
2. 从 `references/role-profiles.md` 读取岗位画像
2b. 从 `references/wiki-hierarchy.md` 加载飞书层级架构（如需推送到飞书）
3. 从 `references/patterns.md` 加载内容模式（15 好模式 + 9 反模式）
4. 从 `references/v4-template.md` 加载 V4 模板结构
5. 按 9 段式结构生成引导文档：
   - **Callout** — "你拿走三样东西"（3 个核心收获）
   - **章节全景脑图** — 画板占位（Mermaid mindmap 语法）
   - **关键时间线** — 画板占位（关键事件流）
   - **深度引导** — 两遍阅读法：
     - 第一遍扫骨架（整体结构 3-5 句）
     - 第二遍精读三段（每段用 5W1H 框架：Who/What/When/Where/Why/How）
   - **阅读路径图** — 画板占位（本章在全书中的位置）
   - **核心概念速查表** — 术语 | 一句话解释 | 和你的关系
   - **知识 -> 映射 -> 行动** — 三层洞察（画板占位）
   - **关键问题** — 3 个递进深度：理解 -> 分析 -> 迁移
   - **行动建议** — 1 件具体可做的事 + 明确产出物 + 时间框架
5b. 画板生成与上传（如有飞书环境）：
   - 提取文档中 4 处 Mermaid/DSL 代码块（脑图/时间线/阅读路径/三层洞察）
   - 保存为临时文件（.mmd 或 .json）
   - 用 whiteboard-cli 转换为 OpenAPI 格式
   - 用 lark-cli docs +whiteboard-update 上传到文档画板
   - 详见 `references/wiki-hierarchy.md` §4.3-§4.7
6. 质量门禁 + 迭代修订（强制两轮）：
   - **第一轮：生成初稿** → 按 9 段式结构完整输出
   - **自评**：用 `references/quality-gate.md` 的 5 维量化评分（1-5 分）对初稿逐项打分，记录每项分数和扣分原因
   - **定位弱项**：任何维度 < 4 分的，定位到具体段落，明确写出"哪一段弱、为什么弱、怎么改"
   - **第二轮：定向修订** → 只改弱项段落，不改已达标的段落。修订后重新评估改过的段落
   - **交付标准**：5 个维度均 ≥ 4 分方可交付。如有维度仍 < 4 分，在文档末尾标注"[质量门禁警告] XX 维度评分 X 分，建议人工审阅"
6b. 画板生成与上传（如有飞书环境）：
   - 提取文档中 4 处 Mermaid/DSL 代码块（脑图/时间线/阅读路径/三层洞察）
   - 保存为临时文件（.mmd 或 .json）
   - 用 whiteboard-cli 转换为 OpenAPI 格式
   - 用 lark-cli docs +whiteboard-update 上传到文档画板
   - 详见 `references/wiki-hierarchy.md` §4.3-§4.7
7. 写入 `guides/{chapter}-{role}-{level}.md`
8. 飞书推送交互：
   - 生成完成后，用 AskUserQuestion 询问用户："引导文档已生成到 guides/{filename}.md。是否需要推送到飞书？"
   选项：
   - "推送到飞书" — 执行步骤 8a
   - "不推送，只保留本地文件" — 跳过，任务完成
   - 用户选择"推送到飞书"时：
   8a. 读取 `references/wiki-hierarchy.md` 获取飞书层级架构和推送规范
   8b. 将 markdown 内容通过飞书 MCP（mcp__feishu__create-doc）推送到对应知识库节点
   8c. 提取文档中的 Mermaid 代码块（脑图/时间线/阅读路径/三层洞察），如需画板则按 wiki-hierarchy.md 的画板流程处理
   8d. 推送完成后返回飞书文档 URL

### 输出

V4 结构的岗位化阅读引导文档（Markdown + 可选飞书画板）

---

## `/bc discuss` — 苏格拉底式深度讨论

**触发词**: `/bc discuss`, `讨论`, `苏格拉底`, `discuss`

### 输入

- 章节编号或主题关键词
- 讨论深度（浅层回顾 / 中层分析 / 深层迁移 — 默认中层）

### 流程

1. 读取对应章节内容和引导文档
2. 按深度等级生成讨论题：
   - 浅层：记忆 + 理解级（布鲁姆 L1-L2）
   - 中层：应用 + 分析级（布鲁姆 L3-L4）
   - 深层：评估 + 创造级（布鲁姆 L5-L6）
3. 混合苏格拉底 6 类追问：
   - 澄清（"你说的 X 具体指什么？"）
   - 假设（"这里隐含了什么前提？"）
   - 证据（"原文哪里支持这个观点？"）
   - 视角（"换个岗位会怎么看？"）
   - 后果（"如果这样，会导致什么？"）
   - 元问题（"这个问题本身为什么重要？"）
3b. 每个讨论题配套追问树（3 分支）：
   - **深化追问**（正面展开）："你说 X 很重要，能举一个你工作中的具体例子吗？"
   - **挑战追问**（反面质疑）："如果反过来看，X 在什么条件下反而不成立？"
   - **迁移追问**（跨场景应用）："如果把 X 迁移到[其他岗位]的场景，它会变成什么？"
   
   执行规则：用户首次回答后先用深化追问展开，再用挑战追问挑战，最后用迁移追问收尾。不要随机跳跃分支。
4. **进入互动模式**：用户回答一个问题 -> AI 追问 -> 深入，不是一次性输出
5. 引用原文锚点（人名 + 引言 + 场景）
6. 跨岗位视角："如果从 XX 岗位的角度看这个问题..."
7. 讨论结束后更新 `_glossary.md`（新出现的术语）

### 输出

互动式讨论（非一次性输出），记录存入 `discussions/{chapter}-{timestamp}.md`

---

## `/bc club` — 读书会运营

**触发词**: `/bc club`, `读书会`, `club`, `book club`

### 输入

- 读书会规模（人数）
- 周期选择（默认 6 周，可调整）
- 参与岗位列表（从 `_config.json` 的 `activeRoles` 字段读取；如未指定则使用全部岗位）

### 流程

1. 从 `references/club-engine.md` 加载运营框架
2. 生成周阅读计划（按章节分组，考虑篇幅和难度）
3. 设计积分系统：
   - 签到 +1
   - 完成章节阅读 +5
   - 提交阅读笔记 +10
   - 参与讨论 +15
   - 最佳分享 +30
4. 设计徽章系统：完读者 / 洞察者 / 演讲者 / 完美出席者
5. 跨岗位讨论设计：2 轮同角色深挖 + 1 轮跨界碰撞
6. 生成每周讨论主题 + 主持人指南
7. 可选：如有飞书 MCP，推送到飞书群/知识库

### 输出

- 6 周阅读计划（`club/plan.md`）
- 积分规则（`club/scoring.md`）
- 每周讨论主题 + 主持人指南（`club/week-{N}.md`）

---

## 参考文件索引

所有参考文件位于 `references/` 目录，按需加载以节省 token：

| 文件 | 用途 | 加载时机 |
|------|------|---------|
| `role-profiles.md` | 行业 x 岗位画像模板 | init, guide, explore |
| `methodology.md` | 岗位 x 层级 -> 方法论映射 | guide |
| `v4-template.md` | 9 段式引导文档模板 | explore |
| `patterns.md` | 内容模式（15 好 + 9 反） | explore |
| `quality-gate.md` | 5 项质量检查细则 | guide, explore |
| `club-engine.md` | 读书会运营框架 | club |
| `wiki-hierarchy.md` | 飞书 Wiki 层级架构（通用抽象模板） | explore, club |

## 项目目录结构

```
~/.claude/books/{slug}/
  _config.json          # 项目配置
  _glossary.md          # 术语表（自动积累）
  _progress.json        # 阅读进度
  chapters/
    {id}.md             # 章节原文/摘要
  guides/
    {role}-path.md      # 岗位阅读路径
    {chapter}-{role}-{level}.md  # 章节引导
  discussions/
    {chapter}-{ts}.md   # 讨论记录
  club/
    plan.md             # 读书会计划
    scoring.md          # 积分规则
    week-{N}.md         # 每周主题
```
