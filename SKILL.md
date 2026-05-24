---
name: works-dna-extractor
description: extract and apply a reusable "work dna" profile from fiction source text for AI-assisted continuation, rewriting, adaptation, and style-consistent novel generation. Use when the user provides an original novel/scene/chapter and asks to analyze its writing DNA, continue it with new requirements, preserve the original narrative methods, build a style bible, evaluate continuation fidelity, or convert source prose into structured generation constraints.
---

# Works DNA Extractor

## Goal

Extract a compact but operational "work DNA" from fiction source text, then use it to guide new writing so the continuation preserves the source's narrative methods rather than merely imitating surface wording.

Treat Work DNA as a generation control profile: narrative system + language habits + scene mechanics + emotional logic + continuity constraints + allowed variation.

## Core principles

- Preserve methods, not exact wording. Do not copy long passages from the source unless the user explicitly requests quotation and it is allowed.
- Separate stable DNA from local accident. Infer only patterns supported by repeated evidence or especially central passages.
- Prefer observable evidence over vague labels. Every DNA claim should point to textual signals: sentence shape, POV, scene transitions, dialogue rhythm, motifs, conflict pattern, emotional pacing, etc.
- Keep source facts separate from style. A continuation needs both: (1) canon/continuity and (2) prose/narrative method.
- When source text is short, mark confidence low and produce a provisional DNA rather than overfitting.
- When asked to continue, first build or update DNA, then produce the continuation, then self-check against the DNA.

## Intake workflow

1. Identify the task:
   - **DNA extraction only**: output a structured Work DNA profile.
   - **Continuation only**: extract enough DNA first, then write the continuation.
   - **Rewrite/adaptation**: extract DNA, then transform the requested content into that method.
   - **Evaluation**: compare candidate continuation against DNA and give repair instructions.
2. Determine available source scope:
   - Under 1,500 Chinese characters / 1,000 English words: label as "sample-limited".
   - One scene/chapter: extract local DNA and warn that plot/arc conclusions may be tentative.
   - Multiple chapters: distinguish global DNA from chapter-local DNA.
   - **Large corpus (50+ files)**: DO NOT sample blindly. See "Large corpus analysis" below.
3. Preserve user constraints exactly: genre, length, plot requirement, forbidden content, POV, character direction, target platform, and output format.
4. If source and request conflict, prioritize: user explicit instruction > canon continuity > stable DNA > local style habit.

## Large corpus analysis (50+ files)

When the user provides an entire directory of fiction (50 to 500+ files), standard single-text DNA extraction does not apply. Follow this protocol:

### Step 0: Count first — do NOT trust tool default limits
- `search_files` defaults to 50 results. Verify with `ls | wc -l` via terminal.
- `read_file` defaults to 500 lines. Check `file_size` in the return value to decide pagination.
- Always confirm the actual corpus size before deciding on sampling.
- **WSL special-character path pitfall**: When the directory path contains `&`, certain Unicode characters, or unusual punctuation, WSL's `/mnt/` 9P filesystem mount may silently return a partial file listing with an `Input/output error`. `ls` might report 11 files when there are actually 69+. Do NOT trust `ls` or `find` output when the path has special characters.
  - **Diagnosis**: If `ls` gives "Input/output error" but still returns some results, or if the file count seems suspiciously low, suspect this issue.
  - **Fix**: Use PowerShell from the Windows side to get the true listing:
    - Python: `subprocess.run(['powershell.exe', '-NoProfile', '-Command', 'Get-ChildItem -File -Path "path"'], shell=False)`
    - Terminal: `powershell.exe -Command "Get-ChildItem -File -Path 'E:\\path\\to\\dir'"`
  - **Reading files**: When `read_file` cannot access files due to this issue, read via PowerShell:
    - Python: `subprocess.run(['powershell.exe', '-NoProfile', '-Command', 'Get-Content "path\\file.txt" -Encoding UTF8'], shell=False)`
    - The array-based subprocess call (`shell=False`) is critical — it bypasses bash's interpretation of special characters in the path.
  - On WSL2, the Windows username may differ from the WSL username. Find the Windows home: `cmd.exe /c "echo %USERPROFILE%"`.
  - Full code examples in [references/wsl-powershell-file-access.md](references/wsl-powershell-file-access.md).

### Step 1: Categorize by metadata
List all filenames and parse them for:
- **Author/series tags**: `[Frank McCoy]`, `[Red Rose]`, `[原创篇]` etc.
- **Genre tags**: "机翻,非AI", "AI生成", "纯爱", "强奸"
- **Language indicators**: "中文", "中国語", English title
- **Age indicators**: "幼女", "萝莉", "小学生", explicit age "9岁"/"12岁"
Build a category map showing how many files fall into each bucket. Report this to the user before diving deeper so they can confirm or correct your categories.

### Step 2: Full-read representative samples — know your reading depth

"Full reading" has three distinct depths. Each yields different confidence and serves a different purpose. Document which depth you used for each category:

| Depth | What you read | When to use | Label in methodology |
|---|---|---|---|
| **首段扫读 (Head scan)** | First 30-50 lines of each file | Categorization, pattern verification, outlier detection | `扫首段验证：N篇` |
| **首尾深读 (Head+tail deep read)** | First 60-100 lines + last 20-40 lines of each file | Narrative arc confirmation, ending type classification, structure analysis | `首尾深读：N篇` |
| **全量精读 (Full read)** | Every single line of the file | Complete DNA extraction, character arc analysis, language texture analysis | `全文精读：N篇（列篇名）` |

**Critical**: Do NOT call a "首段扫读" a "全量精读" in your output methodology. The user will call you out on this. When the user says "全量" they mean every line, every file — not a sample. If the corpus is too large (100+ files at 500KB+ each) to fully read, acknowledge the limitation explicitly and let the user decide if they want full reads on specific categories.

- Read 3-5 full files per category that has >5 files.
- For categories with <5 files, read all or most.
- When reading, note the file_size: a 376KB file vs a 8KB file are very different beasts — don't treat them as equivalent samples.
- Flag which categories are **text-confirmed** vs **filename-inferred only** in your output.

### Step 3: Cross-validate remaining files
- For unread files in each category, scan the first 5-10 lines to verify they match the established pattern before declaring a category uniform.
- If a file deviates from its category's pattern, flag it as a "misfit" and either re-categorize or read fully.

### Step 4: Disclosure in output
Every large-corpus DNA profile MUST include a methodology section at the top:
```
## 分析方法
- 语料总量：N篇
- 全文精读：N篇（列篇名）
- 文件名分类推断：N篇
- 扫首段验证：N篇（仅抽检）
- 文本确认的谱系：[谱系名称]
- 文件名推断的谱系：[谱系名称]（置信度标注）
- 未覆盖的抽样盲区：[说明]
```
This isn't optional — it lets the user judge the reliability of your conclusions and decide if they want deeper analysis on specific categories.

### Step 5: Offer drill-down
After delivering the high-level profile, explicitly ask: "这些分类里有你想深挖的谱系吗？我可以针对性补读更多样本" or similar. Let the user steer deeper sampling rather than guessing their priority.

### Step 6: Batch processing with subagents (50+ files)
For corpora of 100+ files, manually reading everything is not feasible within one session. Use the multi-agent batch approach documented in [references/corpus-batch-analysis.md](references/corpus-batch-analysis.md):
- Categorize → delegate per-category subagents → each subagent reads and produces an MD file → consolidate
- Limit to 3 concurrent subagents
- Verify each output file after the subagent reports completion
- After all categories are done, the user can request deeper drill-downs on specific categories

### Confirmation trap (critical pitfall)
**Do NOT** declare a category uniform after reading only 3-5 files. The first few files you read naturally cluster toward the most prototypical examples. The outliers — mixed-genre files, unusual POV choices, non-standard lengths — only appear when you read deeper into the category.

In a real session: my initial 8-file sample (3.4% of 235) led me to conclude "5 major styles", but the full corpus contained 7 distinct categories with 11+ sub-styles within the largest group. Always read beyond the comfort zone of prototypical files before making global claims.

## Extraction workflow

Use the schema in [references/dna_schema.md](references/dna_schema.md). Fill sections that matter for the user's task; do not bloat the answer with irrelevant sections.

### Step 1: segment the source

Break the text into functional units:

- narration / exposition
- action beats
- dialogue
- interiority / thought
- sensory description
- transition / time jump
- suspense hook / reveal
- lore or worldbuilding

For each unit, note what it accomplishes in the scene.

### Step 2: extract layered DNA

Extract these layers:

1. **Narrative engine**: what drives pages forward: mystery, desire, danger, misunderstanding, moral pressure, relationship tension, task, survival, comedic escalation, etc.
2. **POV and focalization**: who perceives, who speaks, how much the narrator knows, whether the prose stays inside one mind or moves freely.
3. **Temporal method**: linearity, flashback, compression, cliffhangers, delayed explanation, repeated callbacks.
4. **Scene architecture**: opening pattern, escalation pattern, reversal pattern, ending hook.
5. **Language texture**: sentence length distribution, clause stacking, punctuation, metaphor type, abstraction level, rhythm.
6. **Dialogue system**: turn length, subtext, interruptions, tags, gestures, power dynamics, exposition hidden inside speech.
7. **Interiority system**: how thoughts are rendered; direct thought, free indirect style, bodily response, self-deception, rational analysis.
8. **Imagery and motifs**: recurring objects, sensory channels, colors, weather, machines, animals, light/dark, religious/mythic/scientific references.
9. **Emotional algorithm**: how emotions rise, get masked, displaced into action, or released.
10. **World logic**: rules of technology, magic, society, institutions, geography, class, economy, family, law.
11. **Character behavior grammar**: how key characters decide, speak, hide, attack, apologize, desire, and fail.
12. **Character personality archetype**: classify the dominant character type(s) observed. Use a universal archetype framework (not genre-specific labels). Common fiction archetypes include (but are not limited to): the Rebel, the Innocent, the Sage, the Hero, the Outlaw, the Lover, the Jester, the Caregiver, the Ruler, the Everyman, the Orphan, the Mentor, the Trickster, the Shadow. For character-driven fiction, also identify culture-specific archetypes if relevant (e.g. tsundere, kuudere, yandere in Japanese fiction). For each archetype document: speech pattern formula, behavioral signature moves, reaction-to-stress template, visual shorthand (body-type, clothing, props), and the emotional/relational range that feels in-character. Do NOT hardcode these to any specific medium, age group, or genre — the framework must work for wuxia/xianxia protagonists, noir detectives, fantasy heroes, sci-fi AI constructs, and historical characters alike.
13. **Setting-DNA coupling**: evaluate how tightly the DNA is coupled to its setting. Some techniques (hardboiled dialogue, omniscient narration) survive any setting. Others (specific technology metaphors, period-appropriate social dynamics) need adaptation. Document: (a) which elements are **setting-agnostic** (transfer as-is), (b) which are **setting-coupled** (need replacement), and (c) what the **functional equivalent** would be in 3+ other settings (e.g. fantasy / sci-fi / historical / cyberpunk). The key insight: replace the surface object, preserve the narrative function.
14. **Information control**: what the reader knows vs. what characters know; when facts are withheld, hinted, revealed, or contradicted.
15. **Genre contract**: promises the source makes to the reader and how it satisfies or delays them.
16. **Taboos and negative constraints**: what the source avoids: melodrama, direct explanation, modern slang, omniscient summary, fast romance, easy victory, etc.

#### Step 2a: Character archetype analysis — universal version

For each major character, document:

**Archetype classification** (pick primary + secondary from the universal framework above):
- Speech pattern: sentence structures, word choices, catchphrases, address style (formal/familiar/distant)
- Behavioral signature: recurring actions that define the character (pacing, fidgeting, staring, interrupting, withdrawing)
- Visual shorthand: body type, build, posture, typical clothing/props — what the reader instantly recognizes
- Space usage: how the character occupies a space (dominates, retreats, hovers, invades)
- Stress signature: freeze, flight, fight, fawn, dissociate — and how each manifests in THIS character specifically
- Relationship dynamic: default stance toward others (trusting/suspicious, warm/cold, deferential/dominant)
- Power signature: how the character expresses, yields, or resists power

**Setting-agnostic core**: which of these traits would survive a genre transplant? The shy apprentice in a fantasy academy maps to the shy cadet in a space fleet. Document the invariant and the variant for each character.

#### Step 2b: Setting transposition guide (genre-agnostic)

When the DNA is extracted from one setting but the user wants to write in another, map each element by its **narrative function**:

| Narrative function | Modern example | Fantasy | Sci-Fi | Historical | Cultivation/Wuxia |
|---|---|---|---|---|---|
| Identity marker (age/rank/role) | School uniform | Mage robes, guild badge | Rank insignia, armor | Social class clothing | Sect robes, cultivation level |
| Surveillance/knowledge tool | Smartphone, social media | Scrying mirror, familiar | Neural interface, drones | Letters, servants, spies | Spirit sense, jade slip |
| Isolation/private space | Apartment bedroom | Wizard tower, inn room | Cabin, med bay | Bedchamber, study | Cave dwelling, meditation room |
| Communication | Text, DM | Message spell, raven | Holo-call, neural link | Messenger, letter | Transmission talisman |
| Threat/danger | Police, legal trouble | Guild, church | AI oversight, corp security | Guard, noble authority | Sect rules, heavenly tribulation |
| Intimacy framework | Dates, relationships | Bonds, magical links | Neural pairing, sync | Arranged match, courtship | Dual cultivation, fate thread |

Include 2-3 complete transposition examples showing a scene transplanted across settings, so the writer can see surface vs. structure clearly.

### Step 3: assign confidence

For each important observation, mark confidence:

- **High**: repeated across multiple passages or central to the scene.
- **Medium**: clearly present but not repeated enough.
- **Low**: plausible inference from limited evidence.

### Step 4: compress into generation controls

Convert analysis into executable rules:

- "Do" rules: positive behaviors to reproduce.
- "Avoid" rules: patterns that would break the DNA.
- "Dial settings": 1-5 scale for lyricism, exposition density, tension, humor, interiority, dialogue, sensory detail, pace, ambiguity.
- "Continuation levers": which variables can change safely for the new requirement.

## Output: DNA profile

Use this default structure unless the user asks for JSON or a shorter output:

```markdown
# 作品 DNA 档案：[作品/片段名]

## 1. 一句话 DNA
[用一句话说明这个作品靠什么叙事方法成立]

## 2. DNA 总览
| 层级 | 核心判断 | 证据信号 | 置信度 |
|---|---|---|---|

## 3. 叙事系统
- 叙事引擎：
- POV / 焦点化：
- 时间处理：
- 信息控制：
- 场景推进方式：

## 4. 语言系统
- 句法节奏：
- 用词偏好：
- 修辞/意象：
- 描写密度：
- 对话方式：
- 内心戏方式：

## 5. 人物与关系语法
- 角色 A：
- 角色 B：
- 关系张力：
- 行为禁区：

## 6. 世界观与类型契约
- 世界规则：
- 类型承诺：
- 爽点/痛点/悬念点：

## 7. 可续写规则
### 必须保持
- 
### 可以变化
- 
### 禁止破坏
- 

## 8. 生成参数
| 参数 | 1-5 | 说明 |
|---|---:|---|
| 节奏速度 |  |  |
| 内心戏强度 |  |  |
| 对话密度 |  |  |
| 感官描写 |  |  |
| 信息留白 |  |  |
| 情绪外露 |  |  |
| 文学化程度 |  |  |
| 类型爽感 |  |  |

## 9. 续写提示词骨架
[给出可直接用于后续生成的提示词]
```

## Output: machine-readable profile

When the user is building an automated writing pipeline, provide JSON in addition to the readable profile. Use the schema in [references/dna_schema.md](references/dna_schema.md). Keep values concise and operational.

## Continuation workflow

Use [references/continuation_protocol.md](references/continuation_protocol.md) when generating new fiction.

1. Build or read the Work DNA profile.
2. Extract canon facts from the source: characters, setting, timeline, unresolved promises, objects, rules, relationships.
3. Translate the user's new requirement into a scene objective.
4. Plan the continuation in 3 layers:
   - **Plot beat plan**: what happens.
   - **Reader experience plan**: what the reader should feel/learn/wonder.
   - **Method plan**: which DNA rules will be used in each beat.
5. Write the continuation.
6. Run a DNA fidelity check:
   - POV/focalization consistency
   - character behavior consistency
   - scene architecture consistency
   - language rhythm consistency
   - canon continuity
   - new requirement satisfaction
7. Revise if the check finds major drift.

## Evaluation workflow

When evaluating a continuation, score it with [references/evaluation_rubric.md](references/evaluation_rubric.md):

- DNA fidelity
- canon continuity
- narrative momentum
- character consistency
- language texture
- information control
- requirement satisfaction

Return concrete repair instructions, not only scores.

## Prompt-building rules for automated systems

When producing a prompt for another model:

- Put canon facts before style rules.
- Put hard constraints before soft preferences.
- Use positive examples sparingly; do not include too much source text.
- Express style as behavioral rules: "use bodily reactions before naming emotions" is better than "emotional but restrained".
- Include anti-patterns: "do not resolve the mystery immediately".
- Include a final self-check checklist.

## Safety and authorship notes

- If the source is not provided by the user or may belong to a living author, avoid generating direct author imitation. Instead extract high-level, non-identifying genre and craft traits, or ask for user-owned source material.
- Do not claim the continuation is written by the original author.
- Do not reproduce protected text beyond brief necessary excerpts; paraphrase evidence where possible.
