# OpenSpec Adoption Plan

> **Context for Claude Code:** This plan was drafted in a separate Claude.ai session where I talked through adopting OpenSpec into an existing skill stack. That session only had a rough idea of my actual skills (`/research`, `/design`, `/do`, a hub skill, a Jira skill). **You have access to the real skill files** — read them before making concrete changes. Where this plan describes skill behavior, treat it as "what we think is roughly true; verify." Where it describes architecture decisions, those are the conclusions I want to execute against.

---

## Goal

Adopt [OpenSpec](https://github.com/Fission-AI/OpenSpec) as the per-repo spec + change-proposal format, while:

- Keeping the hub skill as the cross-repo index and Confluence-sync layer
- Keeping the ADR output path in `/design` (ADRs and OpenSpec serve different purposes)
- Letting `/do` stay OpenSpec-agnostic — OpenSpec-awareness lives in per-repo instruction files and the hub
- Handling archive via CI on merge, not inside `/do`

## Target architecture

```
Hub skill (cross-repo index + Confluence sync)
  ├── /research  — finds precedents (other repos, web, existing specs)
  ├── /design    — classifies scope, routes to right artifact(s):
  │                 ├── ADR (existing template) — for architectural decisions
  │                 ├── Confluence effort page — for multi-repo coordination
  │                 └── OpenSpec propose per-repo — for the actual spec changes
  ├── /do        — reads a work plan (Jira ticket OR openspec/changes/<id>/tasks.md),
  │                executes with existing TDD + fresh-context subagent review
  └── OpenSpec installed globally → Claude natively speaks the format in every repo

Post-merge:
  CI runs `openspec archive <change-id>` → deltas fold into openspec/specs/,
                                            change folder moves to changes/archive/
  Hub skill observes archive commits → updates Confluence effort status
```

### Why this shape

- **OpenSpec skills/commands global, spec content per-repo.** The format knowledge (skills) is universal; the spec content (requirements for a given codebase) is per-repo. These install separately.
- **`/design` stays an orchestrator, not a writer.** It classifies the change and routes to the right artifact generator. It delegates OpenSpec proposal writing to OpenSpec's own skill, keeps ADR generation in-house, syncs effort pages to Confluence.
- **`/do` stays format-agnostic.** It reads whatever work plan the current context points it at. OpenSpec awareness lives in the repo's instruction file (`CLAUDE.md` / `AGENTS.md`), which tells Claude where `tasks.md` lives. This keeps `/do` reusable for non-OpenSpec repos and ad-hoc work.
- **Archive runs in CI, not in `/do`.** Archive modifies `openspec/specs/` — the source of truth for shipped behavior. It must only run after PR merge, or specs will drift from reality. CI on merge is the right trigger.

### Key mental model: three altitudes, three artifacts

| Altitude | Artifact | Location | Lifecycle |
|---|---|---|---|
| Architectural decision | ADR | `docs/adr/` (or repo convention) | Permanent history |
| Cross-repo effort coordination | Confluence effort page | Confluence (synced by hub) | Closes when all pieces ship |
| Per-repo spec change | OpenSpec change folder | `openspec/changes/<id>/` | Archives on merge |
| Current system behavior | OpenSpec spec | `openspec/specs/<capability>/` | Updated on archive |

A big change might produce all three top rows. They're not alternatives.

---

## What we think is currently true (please verify against actual skill files)

Read the actual skill files before acting on any of this. These are my recollections from the Claude.ai session:

- **Hub skill** — maps all work repos with context about each. Used for cross-repo exploration and (intended) effort tracking synced to Confluence.
- **`/research`** — searches for examples online and across internal company repos (all public internally). Used pre-design to find precedents.
- **`/design`** — currently drafts specifications in some format. Has output paths for things like ADRs when the change is large enough to need broader org approval. **This is the skill most affected by OpenSpec adoption.**
- **`/do`** — reads Jira tickets (via a separate Jira skill, referenced indirectly), creates appropriate branch names, executes work in chunks if needed, does TDD with separate context-window subagents for tests vs. implementation, does self-review with a fresh-context subagent.
- **Jira skill** — exists, consumed by `/do` without `/do` naming it explicitly.

**Things I specifically want you to check against reality before proceeding:**

1. Does `/design` output specs in a structured format today, or more prose-y? (Determines how much format-level migration is needed.)
2. Does `/do` already have a generalized "find the work plan" step, or is it hard-coded to Jira? (Determines whether we need a small abstraction layer.)
3. Does the hub skill currently write anything to Confluence, or is that aspirational? (Determines scope of the Confluence-sync piece.)
4. Are there any conventions in the existing design output that aren't in OpenSpec's schema and would be missed?

---

## Phased execution

### Phase 0: Verify current state

Before anything else, read the actual skill files and confirm or correct the assumptions above. Adjust later phases based on what you find. Surface anything surprising before proceeding.

### Phase 1: Global install

Goal: get OpenSpec's skills and slash commands available user-scope so Claude knows the format in every repo without per-project config.

1. Install the CLI: `npm install -g openspec` (or confirm via `openspec --version`).
2. Install skills globally for Claude: `cd ~ && openspec init --tools claude`. This writes to `~/.claude/skills/` and related user-scope locations.
3. Verify: open a fresh Claude Code session in any directory (even outside a repo) and confirm OpenSpec slash commands / skills are recognized.
4. Note: there's an open proposal ([issue #752](https://github.com/Fission-AI/OpenSpec/issues/752)) for a first-class `openspec init --global` flag. The `cd ~ && openspec init` approach is the current workaround.

### Phase 2: Pilot one repo

Pick one repo — **not my most important one** — to be the pilot. Goal: run one full OpenSpec cycle end-to-end before touching any skill files.

1. In the pilot repo: `openspec init --tools none` (skills are already global, no need to pollute the repo with `.claude/`).
   - Known issue: `--tools none` currently skips creating `openspec/config.yaml` ([issue #712](https://github.com/Fission-AI/OpenSpec/issues/712)). If we want project-level config, either create it by hand or run `openspec init` interactively and select "none" for tools.
2. Convert one existing spec (or area of the codebase currently lacking a spec) into `openspec/specs/<capability>/spec.md` format. Hand-written is fine for this pilot.
3. Run one full cycle:
   - `/opsx:propose` (or whatever the current propose command is — verify against installed version) to generate a change folder for a small real change.
   - Review the generated artifacts. Do they read well? Is the delta spec format an improvement over what we currently produce?
   - Implement via `/do` (or manually — don't touch `/do` yet; we're evaluating format first).
   - On merge, run `openspec archive <change-id>` manually. Observe the specs/ merge and change folder move.
4. **Decision point:** after one cycle, is this format an improvement over current design output?
   - If yes → proceed to Phase 3.
   - If no → document why, stop here. Don't rework skills for a format that doesn't earn its keep.

### Phase 3: Skill integration

**Only proceed if Phase 2 was a clear win.** This phase does the actual skill changes. All of this requires reading the actual skill files first.

#### 3a. `/design` — reshape from writer to router

Current (assumed): drafts specifications directly in some format.

Target: classifies the change, routes to the right artifact generator(s):

- **Classification step at the top.** Given the user's intent, decide: is this a direct PR (too small to spec), a single-repo change (needs OpenSpec proposal in one repo), a multi-repo effort (needs Confluence effort page + OpenSpec proposals in N repos), an architectural decision (needs ADR, possibly alongside OpenSpec changes)?
- **ADR path: keep as-is.** OpenSpec does not replace ADRs. ADRs capture *decisions and their reasoning* and persist as navigable history. OpenSpec changes capture *behavior changes* and archive into specs/. A big shift might produce both.
- **OpenSpec path: delegate to OpenSpec's propose skill.** `/design` decides a change warrants OpenSpec proposals in repos A, B, C → it invokes OpenSpec's propose flow per repo rather than writing the proposal content itself.
- **Confluence effort page: generate + sync via hub skill.** For multi-repo efforts, `/design` produces the effort-level doc (business context, cross-repo scope, stakeholders, success criteria) and hands it to the hub skill for Confluence sync.
- **What to preserve from current `/design`:** any company-specific templates, conventions, or taste that isn't captured in OpenSpec's schema. If these exist, either encode them in `openspec/config.yaml` per-artifact rules, or keep them in `/design`'s generation logic layered on top.

**Important:** the Confluence effort page should **link** to per-repo change folders / PRs, not duplicate their content. Repo is source of truth for execution detail; Confluence is source of truth for effort-level coordination. Drift is the enemy.

#### 3b. `/do` — stay OpenSpec-agnostic, add work-plan source flexibility

Current (assumed): hard-coded to read Jira tickets.

Target: generalized "find the work plan" step that can consume multiple sources.

- Add a lightweight resolution step: check for tasks.md pointed at by the repo's instruction file → Jira ticket → inline prompt description. Pick the richest available source.
- `/do` itself never needs to know the word "openspec." It just reads a work plan and executes.
- Preserve all existing behavior: branch naming, chunking, TDD with fresh-context subagents for tests vs. implementation, self-review subagent.
- `/do` does **not** archive. Archive is CI's job (see Phase 4).

#### 3c. Per-repo instruction files

For each repo that adopts OpenSpec, add (or update) `CLAUDE.md` / `AGENTS.md` at the repo root with something like:

```markdown
## OpenSpec

This repo uses OpenSpec for spec-driven development.

- Current system specs: `openspec/specs/<capability>/spec.md`
- Active change proposals: `openspec/changes/<change-id>/`
- When implementing a change, the work plan is at `openspec/changes/<change-id>/tasks.md`
  and the target behavior is in `openspec/changes/<change-id>/specs/<capability>/spec.md`
- Mark tasks complete in tasks.md as you finish them
- Commit change-folder updates alongside implementation code on the same branch
```

This is what tells a repo-agnostic `/do` how to find the work plan. It's also what makes gradual per-repo adoption possible — repos without this file fall through to Jira as the work plan source.

#### 3d. Hub skill — extend with OpenSpec awareness

- **Discovery:** glob `openspec/changes/*/proposal.md` across mapped repos. Read front matter to find `hub_effort` IDs. Group by effort.
- **Status rollup:** for a given effort, which per-repo changes exist, which are in PR, which are merged, which are archived? (Changes in `changes/archive/` = shipped.)
- **Confluence sync:** when effort status changes, update Confluence page. When all per-repo changes archive, close the effort.
- **Routing:** given an effort ID, identify the affected repos and dispatch `/do` into whichever one I want to work in next.
- **Cross-repo research integration:** `/research` can now specifically look for existing OpenSpec specs across company repos as precedent examples.

### Phase 4: CI archive automation

For each repo adopting OpenSpec, add a CI step that runs on merge to main:

```yaml
# rough shape — adapt to actual CI system
on_merge_to_main:
  - if: files_changed includes "openspec/changes/<id>/**"
    run: openspec archive <change-id>
    commit: "chore: archive OpenSpec change <id>"
```

This:
- Guarantees archive only runs post-merge (invariant enforced by CI trigger, not by skill logic).
- Works for human-written PRs too, not just agent-driven ones.
- Produces a commit on main that the hub skill can observe for status rollup.

Detect the change-id from the merged PR's change folder path. If multiple changes merged in one PR (rare), archive each.

### Phase 5: Conventions for cross-repo efforts

Once the above is working for single-repo changes, formalize the multi-repo pattern:

- **Shared slug convention.** Hub effort `EFFORT-2026-042-unified-billing` → per-repo change IDs like `unified-billing-service`, `unified-billing-web`. Prefix-matching lets the hub correlate without explicit config.
- **Front matter linkage.** Each per-repo `proposal.md` gets a `hub_effort: EFFORT-2026-042` field (or similar). Authoritative cross-linking.
- **Review flow:** draft PRs per repo, opened at proposal time (before implementation). Confluence effort page links all drafts. Reviewers comment on specifics in GitHub, on effort-level direction in Confluence.
- **Implementation:** push implementation commits to the same branch as the proposal. Convert draft → ready when done.
- **Archive:** CI handles each repo independently on its own merge. Hub closes the effort when all repos have archived.

### Phase 6: Rollout to remaining repos

Once the pattern is proven on one repo and the skills are reshaped:

- Add `openspec init --tools none` + instruction file to each additional repo as you start using OpenSpec-shaped changes there.
- Doesn't need to be all-at-once. Repos without OpenSpec keep using the old flow. `/do` handles both via its generalized work-plan resolution.
- Prioritize adding to repos that are frequent sources of multi-repo efforts — those benefit most.

---

## Open questions for Claude Code to surface

These are things the Claude.ai session couldn't answer without access to real files. Please investigate and bring back findings before executing the affected phase:

1. **Exact command namespace in installed OpenSpec version.** The project recently rebuilt around `/opsx:*` commands vs. older `/openspec:*`. Confirm which is current post-install and update all references.
2. **Existing `/design` output format specifics.** What structure, what fields, what conventions? Determines migration shape.
3. **Existing `/do` work-plan resolution.** Is it already abstracted, or hard-coded to Jira? Affects Phase 3b scope.
4. **Hub skill's current Confluence capability.** Does it already write, or is sync aspirational? Affects Phase 3d scope.
5. **Our CI system.** What's the actual merge trigger syntax for Phase 4?
6. **Repo layout for ADRs.** Where do ADRs currently live? (Assumed `docs/adr/` but may be company-specific.)
7. **Any existing specs that would need migration.** If `/design` has been producing spec-shaped output already, there may be content to port into `openspec/specs/` format.

---

## Non-goals / explicit scope limits

- **Not adopting OpenSpec's `/apply` skill.** Our `/do` is more capable (TDD, subagent review, chunking). `/do` stays; `/apply` is not installed or used.
- **Not duplicating spec/change content to Confluence.** Confluence holds effort-level coordination and links to repo artifacts. Repos remain source of truth for specs, proposals, tasks, deltas.
- **Not waiting for OpenSpec Workspaces.** That's a commercial/team feature they're designing for multi-repo coordination. Our hub skill + Confluence already handles this use case; we don't need to block on their product.
- **Not editing `openspec/specs/` during a change.** Specs only update via archive, post-merge. This is an invariant — preserve it.
- **Not making `/do` aware of OpenSpec.** Integration lives in repo instruction files and the hub skill.

---

## Success criteria

This migration is successful when:

1. Global install works — Claude recognizes OpenSpec commands/skills in any repo without per-project `.claude/` pollution.
2. One repo has run a full propose → implement → archive cycle end-to-end, and the resulting spec evolution feels cleaner than the current flow.
3. `/design` routes to the right artifact(s) based on scope classification; ADR path still works; OpenSpec path works for single- and multi-repo cases.
4. `/do` executes against OpenSpec tasks.md in adopted repos and against Jira in non-adopted repos, with no changes to its core TDD/subagent discipline.
5. CI archives changes on merge; hub skill observes and updates Confluence effort status.
6. Multi-repo effort shipped end-to-end: Confluence effort page + N draft PRs + linked-via-front-matter per-repo changes + per-repo CI archives + hub closes effort.

If we hit 1–3 and the format feels like a win, that's enough to call adoption a success even if 4–6 are still in progress.
