# Corpus Batch Analysis: Multi-Agent Workflow

## When to use

When the user provides 50-500+ files of fiction and asks for genre/style/DNA analysis of the entire collection. Single-text DNA extraction doesn't scale.

## Multi-agent approach

Use `delegate_task(tasks=[...])` to process categories in parallel. Each subagent handles one category independently.

### Step 1: Categorize from filenames

Run a Python or shell script to classify all files by filename patterns:
- Author tags (`[Frank McCoy]`, `[Red Rose]`)
- Genre tags (`AI生成`, `机翻,非AI`)
- Language indicators
- Any explicit series markers

Produce a category→count map. **Show this to the user before proceeding** so they can confirm or adjust.

### Step 2: Create task per category

For each category with N files, create a delegate_task with:
- `goal`: "全量精读 [category] (N篇)，创建完整DNA分析文档"
- `context`: Exact file paths, known quirks (e.g. "注意此文件可能为空"), output path
- `toolsets`: `["terminal", "file"]`
- Limit concurrent tasks to 3 (the default max)

### Step 3: Within each subagent

The subagent should:
1. Read every file in its category (at least first 30-40 lines for >10 files)
2. Group by author/subject within the category
3. Apply the 14-layer DNA extraction protocol
4. Write an MD file to the output directory
5. Verify the file was written (`ls -lh` or `read_file` first few lines)

### Step 4: Consolidation (optional)

If the user wants a master summary across all categories, write a separate overview file with:
- Per-category one-line DNA summaries
- Cross-cutting observations (shared motifs, age distributions, relationship types)
- Comparison table of generation parameters

## Critical pitfalls

### Pitfall 1: tool default limits
`search_files` defaults to 50 results. `ls | wc -l` is the correct count.
Always verify real corpus size before declaring categories.

### Pitfall 2: filename-inference bias
Filenames encode programmer-visible metadata, not content. A file named "妹妹的勇气" with tags "机翻,非AI" might be an AI-generated story mislabeled, or a Japanese translation. Always verify at least 3-5 files per category with actual reading before declaring a pattern universal.

In this corpus, filename inference collapsed 5+ distinct sub-styles into single categories. The "日文机翻" group (118 files from filenames) actually contained 11 relationship subtypes that only emerged during reading.

### Pitfall 3: size variability
A category's file count tells you nothing about total analysis load. Files ranged from 70 lines (Red Rose "莉娜") to 9,597 lines (萝莉公寓), a 137x difference. Check `file_size` in `read_file` return values.

### Pitfall 5: WSL special characters in directory names (WSL-only)

On WSL, directory names containing `&`, `%`, `@`, or other special shell characters can cause `/mnt/` mount to fail silently. `ls`, `find`, and `os.listdir()` will return only partial results and emit `Input/output error`. This happened with a directory named `伪娘&futa` — WSL returned 11 of 69 files.

**Fix**: Use `powershell.exe -Command` from inside WSL to read the directory natively. Full code examples in [references/wsl-powershell-file-access.md](references/wsl-powershell-file-access.md).

**Also**: Do NOT delegate subagents to process this corpus — they'll hit the same WSL issue. Process it yourself using the PowerShell methods.

```bash
# List ALL files (WSL workaround)
powershell.exe -Command "Get-ChildItem -Path 'E:\path\to\dir' | Select-Object Name, Length | Format-Table -AutoSize"

# Read specific file
powershell.exe -Command "Get-Content -Path 'E:\path\to\file.txt' -TotalCount 40 -Encoding UTF8"
```

**Detection**: If `ls` reports `Input/output error` for a directory, run `ls | wc -l` vs PowerShell count. If the PowerShell count is higher, use PowerShell for all file I/O on that directory.

**Nested quoting**: When the filename itself contains quotes or special chars, escape single quotes by doubling them (`'` → `''`) inside the PowerShell command string.

### Pitfall 4: subagent summary trust

Subagents self-report "completed". Always verify the output file was actually written with `terminal("ls -lh <path>")` or `read_file(path, limit=5)` after the task returns.

## Output directory convention

```
<project_root>/分析报告/
├── 01_<category1>_DNA档案.md
├── 02_<category2>_DNA档案.md
├── ...
└── (optional) 00_汇总_DNA档案.md
```

Prefix with zero-padded numbers to maintain order. Use underscore not space in filenames (the user's terminal handles them better).
