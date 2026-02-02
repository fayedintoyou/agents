# Claude Code Tips from the Claude Code Team

*Sourced from Boris Cherny (@bcherny), creator of Claude Code*

---

## 1. Parallelize Your Work

Spin up 3–5 git worktrees, each running its own Claude session simultaneously. This is the team's top productivity unlock.

- Use shell aliases (`za`, `zb`, `zc`) to hop between worktrees instantly
- Keep a dedicated "analysis" worktree for logs and database queries
- Docs: [Run parallel sessions with git worktrees](https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees)

## 2. Start Complex Tasks in Plan Mode

Pour energy into the plan so Claude can one-shot the implementation.

- Have one Claude write the plan, spin up a second Claude to review it as a staff engineer
- When things go sideways, switch back to plan mode immediately—don't keep pushing
- Use plan mode for verification steps, not just building

## 3. Invest in Your CLAUDE.md

After every correction, tell Claude: *"Update your CLAUDE.md so you don't make that mistake again."*

- Claude is excellent at writing rules for itself
- Ruthlessly edit over time until Claude's mistake rate measurably drops
- Consider maintaining a notes directory for every task/project, updated after every PR

## 4. Create Reusable Skills

Build skills and commit them to git. Reuse across every project.

- If you do something more than once a day, turn it into a skill or command
- Ideas:
  - `/techdebt` command to find and kill duplicated code at end of session
  - Slash command that syncs 7 days of Slack, GDrive, Asana, and GitHub into one context dump
  - Analytics-engineer agents that write dbt models, review code, and test changes

## 5. Let Claude Fix Bugs Autonomously

- Enable the Slack MCP, paste a bug thread, say "fix"—zero context switching
- Say "Go fix the failing CI tests" without micromanaging how
- Point Claude at docker logs for distributed systems troubleshooting

## 6. Level Up Your Prompting

**Challenge Claude:**
- *"Grill me on these changes and don't make a PR until I pass your test"*
- *"Prove to me this works"*—have Claude diff behavior between main and feature branch

**Iterate on solutions:**
- After a mediocre fix: *"Knowing everything you know now, scrap this and implement the elegant solution"*

**Reduce ambiguity:**
- Write detailed specs before handing work off—specificity improves output

## 7. Terminal & Environment Setup

- **Ghostty** is popular for synchronized rendering, 24-bit color, and unicode support
- Use `/statusline` to show context usage and current git branch
- Color-code and name terminal tabs (tmux helps)
- **Use voice dictation** (fn×2 on macOS)—you speak 3× faster than you type
- More: [Terminal config docs](https://code.claude.com/docs/en/terminal-config)

## 8. Use Subagents

- Append "use subagents" when you want Claude to throw more compute at a problem
- Offload individual tasks to subagents to keep your main agent's context clean
- Route permission requests to Opus 4.5 via a hook to auto-approve safe ones
- Docs: [Hooks - permission request](https://code.claude.com/docs/en/hooks#permissionrequest)

## 9. Use Claude for Data & Analytics

- Use the `bq` CLI (or any database CLI/MCP/API) for on-the-fly analysis
- Commit a BigQuery skill to your codebase for team-wide use
- Boris hasn't written SQL in 6+ months

## 10. Learning with Claude

- Enable "Explanatory" or "Learning" output style in `/config` to understand the *why*
- Have Claude generate visual HTML presentations explaining unfamiliar code
- Ask for ASCII diagrams of protocols and codebases
- Build a spaced-repetition skill: explain your understanding → Claude asks follow-ups → stores results

---

*"There is no one right way to use Claude Code—everyone's setup is different. Experiment to see what works for you!"*

