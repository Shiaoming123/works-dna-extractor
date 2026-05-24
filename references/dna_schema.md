# Work DNA Schema

Use this schema to extract operational narrative DNA from fiction.

## Recommended JSON shape

```json
{
  "profile_name": "",
  "source_scope": {
    "text_length_estimate": "",
    "coverage": "excerpt|scene|chapter|multi_chapter|full_work",
    "confidence_overall": "low|medium|high",
    "limitations": []
  },
  "one_sentence_dna": "",
  "canon": {
    "characters": [
      {
        "name": "",
        "role": "",
        "known_facts": [],
        "goals": [],
        "fears_or_wounds": [],
        "speech_habits": [],
        "behavior_rules": [],
        "must_not_do": []
      }
    ],
    "setting": [],
    "timeline": [],
    "unresolved_threads": [],
    "world_rules": [],
    "important_objects_or_symbols": []
  },
  "narrative_system": {
    "engine": [],
    "pov": "",
    "focalization": "",
    "narrator_distance": "close|medium|distant|variable",
    "tense": "",
    "time_method": [],
    "information_control": [],
    "scene_architecture": {
      "openings": [],
      "escalation": [],
      "turns": [],
      "endings": []
    }
  },
  "language_system": {
    "sentence_rhythm": [],
    "diction": [],
    "syntax": [],
    "punctuation": [],
    "metaphor_sources": [],
    "sensory_channels": [],
    "abstraction_level": "low|medium|high|variable",
    "dialogue": [],
    "interiority": []
  },
  "emotional_system": {
    "dominant_mood": [],
    "emotion_expression_rules": [],
    "tension_release_pattern": [],
    "humor_or_irony": []
  },
  "genre_contract": {
    "genre": [],
    "reader_promises": [],
    "satisfaction_methods": [],
    "delayed_gratifications": []
  },
  "motifs": [
    {
      "motif": "",
      "function": "atmosphere|theme|foreshadowing|characterization|worldbuilding|other",
      "reuse_guidance": ""
    }
  ],
  "character_archetypes": [
    {
      "archetype_name": "通用于任何题材的框架术语，如 Rebel/Innocent/Sage/Outlaw/Caregiver/Ruler/Trickster 等",
      "characters_that_fit": ["character_names"],
      "speech_pattern": "句式特点、词汇偏好、称呼风格",
      "behavioral_signature": ["标志性行为"],
      "stress_signature": "压力下的典型反应模式（战/逃/僵/讨好/解离）",
      "visual_shorthand": "体型/姿势/衣着/道具的速写",
      "power_dynamic": "如何表达/屈服/抵抗权力",
      "setting_agnostic_core": "哪些特质换世界观也不变",
      "setting_coupled_traits": "哪些特质依赖特定世界观设定"
    }
  ],
  "setting_transposition": {
    "original_setting": "",
    "universal_elements": ["不依赖设定的元素"],
    "function_map": [
      {
        "element": "需要替换的表面元素",
        "narrative_function": "它在叙事中承担的功能",
        "fantasy_equivalent": "",
        "scifi_equivalent": "",
        "historical_equivalent": "",
        "cultivation_equivalent": ""
      }
    ]
  },
  "generation_controls": {
    "must_keep": [],
    "may_change": [],
    "must_avoid": [],
    "dial_settings": {
      "pace_speed": 3,
      "interiority": 3,
      "dialogue_density": 3,
      "sensory_detail": 3,
      "ambiguity": 3,
      "emotion_directness": 3,
      "literary_density": 3,
      "genre_payoff": 3
    }
  },
  "evidence_summary": [
    {
      "claim": "",
      "signals": [],
      "confidence": "low|medium|high"
    }
  ],
  "continuation_prompt_skeleton": ""
}
```

## Field guidance

### source_scope
Record whether the profile is based on an excerpt, a scene, a chapter, several chapters, or a full work. This controls confidence.

### canon
Canon is not style. Canon contains facts that a continuation must not contradict unless the user asks for an alternate universe.

### narrative_system
Describe how the story controls reader attention. Useful questions:

- What makes the reader continue?
- Does the narrator reveal causes before or after effects?
- Does the scene begin with action, atmosphere, summary, memory, dialogue, or a problem?
- Are endings hooks, emotional landings, reversals, jokes, or quiet images?

### language_system
Prefer concrete observable rules:

- "Short declarative sentences after reveals" is useful.
- "Powerful writing" is not useful.
- "Metaphors come from machinery and weather" is useful.
- "Beautiful metaphors" is not useful.

### emotional_system
Track the route from stimulus to emotion to behavior. Many works avoid naming emotion directly and instead use body, object handling, silence, or displaced action.

### generation_controls
This is the most important section for automated continuation. Convert analysis into constraints that can be inserted into prompts.

## Evidence standards

Classify evidence without overquoting:

- Lexical signal: repeated word class, register, slang, technical vocabulary.
- Syntactic signal: sentence length, parallelism, fragments, clause order.
- Rhetorical signal: metaphor source, irony, contrast, repetition.
- Structural signal: scene opening, delayed reveal, ending hook, alternating dialogue/action.
- Character signal: repeated decision pattern or speech move.
- Genre signal: recurring payoff, threat, puzzle, romantic beat, comedic beat.

## Anti-overfitting rules

- Do not infer global genre from one image unless supported by plot/voice.
- Do not turn a single unusual sentence into a global rule.
- Do not require exact punctuation quirks unless they repeat.
- Do not preserve accidental names, typos, or formatting errors unless they are intentional.
- Do not force all future scenes to use the same mood; identify allowed emotional range.
