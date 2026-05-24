---
name: works-dna-extractor
description: Extract operational narrative DNA from fiction for style-consistent continuation, rewriting, and generation.
---

# Works DNA Extractor

Activate when the user provides fiction/novel text and asks to:
- Analyze writing style or extract "DNA"
- Continue a story in the same style
- Rewrite content to match a source voice
- Evaluate whether generated text matches the original

## Extraction Protocol

Read the full methodology in `SKILL.md` at this repository's root.

### Quick Steps

1. **Segment** text into functional units (narration, dialogue, action, interiority, description, transition)
2. **Extract 16 layers**: narrative engine, POV, temporal method, scene architecture, language texture, dialogue system, interiority system, imagery/motifs, emotional algorithm, world logic, character grammar, character archetypes, setting-DNA coupling, information control, genre contract, taboos
3. **Assign confidence** per observation (high/medium/low)
4. **Compress** into generation controls: must-keep / may-change / must-avoid + 8 dial settings (pace, interiority, dialogue density, sensory detail, ambiguity, emotion directness, literary density, genre payoff)

### Output

Default: structured markdown profile. For pipelines: JSON per `references/dna_schema.md`.

### Continuation

See `references/continuation_protocol.md`: requirement → beat plan → draft → DNA self-check → revise.

### Evaluation

See `references/evaluation_rubric.md`: 8 dimensions × 1-5 scoring + repair instructions.

### Large Corpus

See `references/corpus-batch-analysis.md` for 50-500+ file batch analysis with multi-agent parallel processing.
