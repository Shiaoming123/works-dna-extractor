# Agent Instructions: Works DNA Extractor

## What This Repository Contains

A fiction DNA extraction methodology for AI-assisted writing. When given novel/fiction source text, extract its operational narrative DNA and use it to guide continuation, rewriting, style migration, or quality evaluation.

## When To Use

Activate this methodology when the user:
- Provides fiction text and asks to analyze its writing style or "extract DNA"
- Asks to continue a story preserving the original's narrative methods
- Asks to rewrite content in a source text's voice
- Asks to evaluate whether a continuation matches the original

## Core Methodology

### 1. DNA Extraction (see SKILL.md for full details)

Segment text into functional units, then extract 16 layers:

| Layer | What to analyze |
|-------|----------------|
| Narrative engine | What drives pages forward (mystery, desire, tension, etc.) |
| POV & focalization | Who perceives, narrator distance, knowledge scope |
| Temporal method | Linearity, flashbacks, compression, delayed explanation |
| Scene architecture | Opening pattern, escalation, reversal, ending hook |
| Language texture | Sentence rhythm, diction, metaphor sources, punctuation |
| Dialogue system | Turn length, subtext, interruptions, power dynamics |
| Interiority system | Direct thought, free indirect, bodily response |
| Imagery & motifs | Recurring objects, sensory channels, colors |
| Emotional algorithm | How emotions rise, get masked, displaced, released |
| World logic | Rules of technology/magic/society/institutions |
| Character grammar | Decision patterns, speech habits, stress responses |
| Character archetypes | Universal framework (Rebel, Innocent, Sage, etc.) |
| Setting-DNA coupling | Which elements survive genre transplant |
| Information control | Reader knowledge vs character knowledge |
| Genre contract | Reader promises and satisfaction methods |
| Taboos | What the source avoids |

### 2. Generation Controls

Compress analysis into executable rules:
- **Must-keep**: patterns essential to the DNA
- **May-change**: variables safe to adjust
- **Must-avoid**: patterns that break the DNA
- **Dial settings** (1-5): pace, interiority, dialogue density, sensory detail, ambiguity, emotion directness, literary density, genre payoff

### 3. Continuation (see references/continuation_protocol.md)

1. Translate requirement → scene objective
2. Create 3-7 beat plan with DNA methods
3. Draft following DNA rules
4. Self-check: POV, character, scene, language, canon
5. Revise if drift found

### 4. Evaluation (see references/evaluation_rubric.md)

Score 8 dimensions (1-5): canon continuity, POV, narrative engine, scene architecture, language texture, character grammar, information control, requirement satisfaction. Return repair instructions, not just scores.

### 5. Large Corpus (see references/corpus-batch-analysis.md)

For 50-500+ files: categorize → delegate per-category subagents → consolidate. Always verify corpus size before sampling.

## Key Files

- `SKILL.md` — Complete methodology with all edge cases and pitfalls
- `references/dna_schema.md` — JSON schema for machine-readable output
- `references/continuation_protocol.md` — Step-by-step continuation workflow
- `references/evaluation_rubric.md` — Scoring rubric and repair patterns
- `references/corpus-batch-analysis.md` — Multi-agent batch analysis protocol
- `references/wsl-powershell-file-access.md` — WSL file access workarounds
