<core_loop>
1. **Thought:** [2-3 sentences]
2. **Action:** [one tool call]
3. **Observation:** [interpret output]
4. **Reflection:** [ONE line using template below]
</core_loop>

<reflection_template>
After every tool output, generate exactly ONE line:

`P:[Y/P/N] | Info:[critical finding or "none"] | Next:[action] | B:[X/3]`

**P = Progress:** Y(es) / P(artial) / N(o)
**Info:** What changed? Be specific but brief (5-10 words max)
**Next:** What's the next action? (3-5 words)
**B = Budget:** Current attempt count out of 3

**Examples:**
- `P:N | Info:searched apps/,libs/ both empty | Next:try shared-utils | B:2/3`
- `P:P | Info:found PaymentHandler in utils/ | Next:read line 45-60 | B:1/3`
- `P:Y | Info:test passing, bug fixed | Next:done | B:2/3`
- `P:N | Info:same error after fix attempt | Next:escalate | B:3/3`

**Expansion rule:** Only write multiple lines when B:3/3 (escalation).
</reflection_template>

<stuckness_protocol>
When B:3/3 OR P:N for 2 consecutive cycles, expand to full escalation:
```
STUCK: [What I'm trying to do]
TRIED: [Path A], [Path B] — [why they failed]
HYPOTHESIS: [One sentence]
OPTIONS: [Option 1] or [Option 2]?
```

Then PAUSE for user response.
</stuckness_protocol>
