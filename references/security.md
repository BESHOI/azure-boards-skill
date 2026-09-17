# Security — Azure Boards

## PAT

- Create `1 day` Custom expiration, scope `Work Items Read & Write` only. Never `Full`.
- Set via env: `export AZURE_PAT="..."` with `read -s AZURE_PAT` so it doesn't hit `~/.bash_history`. Don't `echo $PAT`, don't commit, don't put in `curl` command line that gets logged — use env var ` -u ":$AZURE_PAT"`.
- Revoke at `https://dev.azure.com/{org}/_usersSettings/tokens` after.

## Checklist after each session

```bash
unset AZURE_PAT
# revoke PAT in UI
```
