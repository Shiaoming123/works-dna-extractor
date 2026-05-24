# Works DNA Extractor

## Project Purpose

This repository contains a fiction DNA extraction methodology. When the user provides novel/fiction text and asks to analyze its writing style, continue it, rewrite content in its style, or evaluate continuations — use the DNA extraction workflow below.

## Core File

Read `SKILL.md` for the complete methodology (16-layer DNA extraction, continuation protocol, evaluation rubric).

## Quick Reference

### When to activate

- User provides fiction text and asks "analyze this" / "extract DNA" / "what's the writing style"
- User asks to continue a story "in the same style"
- User asks to rewrite content to match a source text's voice
- User asks to evaluate whether generated text matches the original

### DNA Extraction (abbreviated)

1. Segment text into functional units (narration, dialogue, action, interiority, description, transition)
2. Extract 16 layers:
   - Narrative engine (what drives pages forward)
   - POV & focalization
   - Temporal method (linearity, flashbacks, compression)
   - Scene architecture (opening/escalation/reversal/ending patterns)
   - Language texture (sentence rhythm, diction, metaphor sources)
   - Dialogue system (turn length, subtext, power dynamics)
   - Interiority system (direct thought, free indirect, bodily response)
   - Imagery & motifs
   - Emotional algorithm
   - World logic
   - Character behavior grammar
   - Character personality archetypes
   - Setting-DNA coupling
   - Information control
   - Genre contract
   - Taboos & negative constraints
3. Assign confidence (high/medium/low) per observation
4. Compress into generation controls (must-keep / may-change / must-avoid + dial settings)

### Output Format

Default: structured markdown profile with sections for narrative system, language system, character grammar, generation controls, and continuation prompt skeleton.

Machine-readable: JSON following the schema in `references/dna_schema.md`.

### Continuation Workflow

See `references/continuation_protocol.md`:
1. Translate user requirement → scene objective
2. Create 3-7 beat plan with DNA methods attached
3. Draft following DNA rules
4. Self-check against DNA (POV, character, scene, language, canon)
5. Revise if major drift found

### Evaluation

See `references/evaluation_rubric.md`: 8 dimensions scored 1-5 (canon continuity, POV, narrative engine, scene architecture, language texture, character grammar, information control, requirement satisfaction).

### Large Corpus (50+ files)

See `references/corpus-batch-analysis.md` for the multi-agent batch protocol.
