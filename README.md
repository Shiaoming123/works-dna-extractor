# Works DNA Extractor 🧬

从任意小说原文中提取可复用的「作品 DNA」——叙事系统、语言纹理、人物语法、情感算法、信息控制——并用它指导 AI 续写、改写、风格迁移，让生成文本保留原作的方法而非模仿表面词句。

> An AI skill that extracts operational "Work DNA" from fiction — narrative engine, language texture, character grammar, emotional algorithm, information control — then uses it to guide continuation, rewriting, and style-consistent generation.

## 这是什么？

Works DNA Extractor 是一个面向 AI 写作助手的技能（skill），**同时兼容 Hermes Agent、Claude Code、OpenAI Codex 三大主流 AI Agent 平台**。

核心思路：**提取方法，而非模仿文字**。

把一部小说拆成 16 层 DNA：
- 叙事引擎（靠什么驱动读者翻页）
- POV 与聚焦（谁在看，谁知道多少）
- 场景架构（开头/升级/转折/收尾的固定模式）
- 语言纹理（句法节奏、用词偏好、比喻来源）
- 对话系统（轮次长度、潜台词、权力动态）
- 内心戏系统（直接思想/自由间接体/身体反应）
- 意象与母题（反复出现的物体、感官通道、颜色）
- 情感算法（情绪如何升起、伪装、转移、释放）
- 信息控制（读者知道多少 vs 角色知道多少）
- 角色行为语法（决策模式、说话风格、压力反应）
- ……以及更多

提取完成后，输出结构化的 DNA 档案，可直接用于：
- **续写**：保持原作风格生成新篇章
- **改写**：将现有内容转换为原作的叙事方法
- **风格迁移**：将原作 DNA 应用到不同题材/世界观
- **质量评估**：对比续写文本与 DNA 的吻合度，给出修复建议

## 兼容平台

| 平台 | 入口文件 | 加载方式 |
|------|----------|----------|
| **Hermes Agent** | `SKILL.md` | `/skill works-dna-extractor` 或自动匹配 |
| **Claude Code** | `.claude/skills/works-dna-extractor.md` + `CLAUDE.md` | 自然语言自动触发 |
| **OpenAI Codex** | `AGENTS.md` | 启动时自动加载 |

## 安装

### Hermes Agent

```bash
hermes skill install works-dna-extractor
# 或手动：
cp -r works-dna-extractor ~/.hermes/skills/
```

### Claude Code

```bash
# 将 .claude/skills/ 目录复制到你的项目中
cp -r .claude/ /path/to/your/project/.claude/

# 或将 SKILL.md 内容追加到项目的 CLAUDE.md 中
cat SKILL.md >> /path/to/your/project/CLAUDE.md
```

### OpenAI Codex

```bash
# 将 AGENTS.md 复制到你的项目根目录
cp AGENTS.md /path/to/your/project/

# Codex 启动时会自动读取 AGENTS.md 作为上下文
```

## 使用

### 1. 提取作品 DNA

```
# 任意平台下
帮我分析这部小说的 DNA：[粘贴原文或指定文件路径]
```

### 2. 用 DNA 续写

```
基于刚才的 DNA，续写下一章，要求：主角发现真相后选择原谅
```

### 3. 评估续写质量

```
评估这段续写是否符合 DNA：[粘贴续写文本]
```

## 项目结构

```
works-dna-extractor/
├── SKILL.md                              # Hermes Agent 技能文件（核心逻辑）
├── CLAUDE.md                             # Claude Code 项目上下文
├── AGENTS.md                             # Codex / 通用 Agent 指令
├── .claude/
│   └── skills/
│       └── works-dna-extractor.md        # Claude Code 自动触发技能
├── agents/
│   └── openai.yaml                       # OpenAI Agent 配置
├── references/
│   ├── dna_schema.md                     # DNA JSON Schema
│   ├── continuation_protocol.md          # 续写协议（5 步）
│   ├── evaluation_rubric.md              # 评估量表（8 维度）
│   ├── corpus-batch-analysis.md          # 大规模语料批量分析
│   └── wsl-powershell-file-access.md     # WSL 环境技巧
├── README.md
└── LICENSE (MIT)
```

## 核心能力

| 能力 | 说明 |
|------|------|
| 单篇 DNA 提取 | 从 1500 字到完整长篇，按置信度标注 |
| 大规模语料分析 | 50-500+ 文件的分类→采样→交叉验证流程 |
| 风格迁移 | 将 DNA 从一个题材/世界观迁移到另一个 |
| 续写评估 | 8 维度打分 + 具体修复指令 |
| 机器可读输出 | JSON Schema，可接入自动化流水线 |

## 适用场景

- 小说作者用 AI 辅助续写，保持自己原有的叙事风格
- 编辑/出版商用 AI 生成样章时确保风格一致性
- AI 写作工具集成，作为风格控制模块
- 写作教学：拆解一部作品的叙事技法

## 许可证

MIT License — 随便用，包括商用。

## 致谢

这个技能诞生于 [Hermes Agent](https://hermes-agent.nousresearch.com/) 生态，最初用于分析《偷偷藏不住》等小说的叙事 DNA，后逐步扩展为通用的小说 DNA 提取框架。
