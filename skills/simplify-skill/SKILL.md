---
name: simplify-skill
description: Simplify SKILL.md and other agent instruction files without changing behavior.
---

# Simplify Skill

Rewrite agent instructions to be shorter, clearer, and less repetitive while preserving behavior.

## Rules

- Preserve triggers, constraints, MUST/NEVER rules, safety requirements, edge cases, and examples that define behavior.
- Remove duplicated instructions, filler, throat-clearing, summaries, and explanations that do not affect behavior.
- State each rule once, in the strongest appropriate location.
- Prefer short declarative sentences and exact, common words.
- Do not use em dashes.
- Keep terminology consistent. Do not introduce synonyms for the same concept.
- Merge related rules only when meaning stays unambiguous.
- Remove examples that merely repeat a rule. Keep examples that resolve ambiguity or define edge cases.
- Reduce headings and bullets when they add no structure.
- Optimize for fewer characters and tokens, not merely fewer lines.
- Delete sentences whose only purpose is to explain why an adjacent instruction is sensible.
- Do not sacrifice precision for brevity.

## Verification

After rewriting:

1. Compare the original and simplified versions.
2. Identify any behavior or constraint that was lost, weakened, or made ambiguous.
3. Restore it.
4. Prefer the shortest behaviorally equivalent version.
5. Report character counts before and after, including the percentage reduction.

## Aggressive mode

If the user asks for aggressive simplification, also remove prose that can be inferred reliably from adjacent rules while preserving every behavioral requirement.
