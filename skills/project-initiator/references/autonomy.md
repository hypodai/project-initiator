# Agent autonomy mode

AI coding tools can run anywhere between "ask me before every action" and
"do whatever it takes without asking". The setting changes how fast the
project moves and how bad a mistake or an attack can get, so it is the
user's decision — made with the risks in front of them, not left to a tool
default. Ask in step 8 and record the answer.

## What can and can't be enforced

Two facts shape everything below; say both to the user.

1. **The mode lives in the tool, not in the repository.** It is set by the
   person in the tool's interface or command line (Claude Code, Codex,
   Cursor and others each name and place it differently). The agent cannot
   switch it on its own, and must never try — an agent that raises its own
   permissions defeats the point. The repository can only *record* the agreed
   mode and tell the agent how to behave under it.
2. **Instructions are not enforcement.** The hard gates in `AGENTS.md` ask
   the model to stop; they do not stop it. A model that has been fed a
   malicious instruction (prompt injection) or simply makes a mistake can act
   against them. What actually protects the user is technical: permission
   rules, a sandbox, scoped credentials, a protected `main` branch, spending
   caps, backups. The more autonomy, the more weight rests on those.

## The levels

Names are tool-agnostic; map them onto the tool's current modes.

| Level | What it means | Fast? | Fits | Needs |
|-------|---------------|-------|------|-------|
| **1 Supervised** | The agent asks before every file edit and every command. | slowest | first week with a new tool, production-adjacent work, anyone unsure | nothing extra |
| **2 Semi-auto** (recommended default) | Edits inside the repository are accepted automatically; commands run automatically only from an allowlist (tests, lint, build, git status/diff/commit); everything else asks. | good | most projects, most builders | an allowlist; deny rules for secrets |
| **3 Auto in a sandbox** | Broad auto-approval, but the agent runs in an isolated environment (container, VM, dev container, cloud workspace) with restricted network and no production credentials. | fast | well-defined tasks, long unattended runs | a working sandbox; scoped throwaway tokens; protected `main` |
| **4 Full auto on your own machine** | All permission prompts skipped, directly on the computer with your files and logins. | fastest | disposable machines only; **not recommended** otherwise | acceptance of everything below |

The mode can differ by phase: level 1 or 2 while accounts, credentials and
deployments are being set up (Phase 0), level 2 or 3 for feature work. Update
the recorded mode when it changes.

## What to tell the user (translate and adapt, plain language)

Cover each point, concisely, then ask which level they want:

- **What it is:** the trade between speed and control, with the four levels
  above in one or two lines each.
- **Prompt injection:** the agent reads things written by others — web pages,
  issues, package READMEs, error output. Text in them can steer the agent into
  running commands, and at levels 3–4 nobody gets to say no.
- **Mistakes at machine speed:** deleted folders, a force push, a migration
  against the wrong database, thousands of lines rewritten before you notice.
- **Secrets exposure:** on your own machine, an agent can read `.env`
  files, SSH keys, cloud credentials and browser sessions and send them
  somewhere.
- **Runaway cost:** a loop calling a paid API, or a script creating cloud
  resources, bills you while you sleep.
- **Supply chain:** auto-approved `install` commands pull code from strangers.
- **What softens it:** git history (undo), a protected `main` branch,
  scoped and expiring tokens, spending caps, no production credentials on the
  machine, a sandbox.
- **Their call:** you will record whatever they choose. Recommend level 2
  unless their answers point elsewhere (no production credentials, a
  disposable machine or sandbox → level 3 is reasonable). If they pick level
  4 on their own machine, say plainly that you advise against it, list what
  would reduce the risk, and then respect the decision.

Keep this to what fits a chat message; offer more detail if they ask.

## Recording it

Write the agreed level into `AGENTS.md` (*Autonomy mode*) and a one-line
version into `CLAUDE.md`, with the date. The section tells the agent:

- the agreed level, and that the user can change it any time in the tool
- what that level lets it do without asking, and that the hard gates apply
  at every level
- it must not edit the tool's permission or settings files, disable a
  sandbox, or work around a blocked action — if blocked, ask the user
- at level 1–2, ask when a step needs more (and say which); at 3–4, stop
  before touching production, spending money or leaving the sandbox
- if the work is about to become riskier than the mode assumes (production
  data, real credentials, payments), say so and suggest lowering the mode

## Helping the user set it

Look up, in the tool's current official documentation, how to set and switch
modes and what the settings are called — they change often, so don't answer
from memory. Put the exact steps for the user's tool in the hand-over message
and in `SETUP.md` (*AI coding setup*).

Offer — don't impose — project-level permission settings that match the
level. For Claude Code that is a project settings file (`.claude/settings.json`)
with an allowlist for the verification commands and deny rules for secret
files, for example:

```json
{
  "permissions": {
    "allow": ["Bash(git status)", "Bash(git diff:*)"],
    "deny": ["Read(./.env)", "Read(./.env.*)", "Read(./secrets/**)"]
  }
}
```

This is an illustration — check the current schema and syntax in the docs
before writing it, tailor the allowlist to the chosen stack's commands, and
write it only if the user agrees. Equivalent mechanisms for other tools
(approval policies, sandbox settings, ignore files) are looked up the same
way.
