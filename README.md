# azure-boards-skill

Full **Azure DevOps REST API** skill — Boards (Tasks/Stories/Bugs, WIQL, sprints), Repos (Git, PRs), Pipelines (builds, releases), Artifacts, Test Plans — via PAT. Works in **OpenCode, Claude Code, Cursor, Windsurf, Codex** (open standard `SKILL.md`).


## Install for all agents

**OpenCode / Claude / Cursor / Codex (open standard):**
```bash
# local copy (this repo is already a skill)
cp -r . ~/.config/opencode/skills/azure-boards   # global
# or project
cp -r . .agents/skills/azure-boards
```

**Via `npx skills` (any agent):**
```bash
npx skills add BESHOI/azure-boards-skill
```

**Via opencode HTTP catalog:**
```jsonc
// opencode.jsonc
{
  "skills": ["https://beshoi.github.io/azure-boards-skill/"]
}
```

## Use

Ask the agent:
- "add 5 tasks under story 123 in your project"
- "move finished tasks to Closed"
- "read work item 123"
- "trigger pipeline 1" / "create PR from feature to main" — same PAT pattern, different `/_apis/...` + scope (see `references/full-api.md`).

The skill will first ask for:
1. **Project link** (`https://dev.azure.com/{org}/{project}`)
2. **PAT** (`https://dev.azure.com/{org}/_usersSettings/tokens` → 1-day `Work Items Read & Write` / `Code` / `Build` as needed)

## Contents

- `SKILL.md` — full instructions (asks for link+PAT, then CRUD)
- `references/api.md` — Boards copy-paste `curl`/Node
- `references/full-api.md` — all Azure areas table
- `references/security.md` — PAT 1-day, `read -s`, revoke

## License

MIT
