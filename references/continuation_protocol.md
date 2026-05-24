# Continuation Protocol

Use this protocol to write new fiction from a Work DNA profile and a user requirement.

## Inputs

- Source text or existing Work DNA profile
- User requirement: what should happen next or what adaptation is needed
- Target length and language
- Any forbidden directions

## Step 1: requirement translation

Turn the user's request into a scene objective:

```markdown
Scene objective: [what must happen]
Reader experience: [what the reader should feel / learn / wonder]
Continuity obligations: [facts that must remain true]
DNA obligations: [methods that must be preserved]
```

## Step 2: beat plan

Create 3-7 beats. For each beat, attach a DNA method.

```markdown
| Beat | Plot function | Reader effect | DNA method to use |
|---|---|---|---|
```

## Step 3: drafting rules

- Start from the last known state unless the user asks for a jump.
- Maintain the same POV and focalization unless changing viewpoint is an established source pattern or explicit user instruction.
- Continue unresolved threads before adding new threads.
- Keep exposition inside the source's normal delivery method: dialogue, action, sensory detail, memory, object inspection, official documents, etc.
- Preserve character decision grammar: a cautious character should not suddenly become reckless unless the beat motivates it.
- Reuse motifs by function, not decoration. If rain signals concealment in the source, do not reuse rain merely as scenery.
- Preserve the source's information-control habit: delayed reveal, dramatic irony, unreliable narration, procedural clues, etc.
- End with the source's typical ending move: hook, reversal, emotional image, line of dialogue, unresolved action, or quiet closure.

## Step 4: revision pass

Check for common drift:

- Voice became generic or too polished.
- Characters explain feelings more directly than the source allows.
- Scene resolves too much too fast.
- Dialogue carries exposition unnaturally.
- Continuation uses motifs without their original function.
- Plot satisfies the prompt but breaks canon.
- POV leaks information the focal character should not know.
- New content imitates surface words but not scene mechanics.

## Step 5: final response format

For continuation tasks, use this structure unless the user asks for fiction only:

```markdown
## 续写策略
- [2-4 bullets explaining the DNA methods used]

## 正文
[continuation]

## DNA 自检
| 项目 | 结果 | 说明 |
|---|---|---|
```

If the user asks for only the prose, output only the prose.
