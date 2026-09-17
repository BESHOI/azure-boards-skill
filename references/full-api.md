# Full Azure DevOps REST API — Examples (all use PAT + Basic)

Base: `https://dev.azure.com/{org}/{project}/_apis/...?api-version=7.1` with `Authorization: Basic base64(:PAT)` (`curl -u ":$AZURE_PAT"`). Create PAT at `https://dev.azure.com/{org}/_usersSettings/tokens` with the scope for the area you need.

## Boards (Work)

Covered in `api.md` — work items, WIQL, boards. Same PAT pattern.

## Repos / Git

```bash
# list repos
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/git/repositories?api-version=7.1" | python3 -m json.tool
# create PR
curl -s -u ":$AZURE_PAT" -H "Content-Type: application/json" -d '{"sourceRefName":"refs/heads/feature","targetRefName":"refs/heads/main","title":"My PR"}' "https://dev.azure.com/{org}/$ENC/_apis/git/repositories/{repoId}/pullRequests?api-version=7.1"
# PAT scope: Code Read & Write
```

## Pipelines / Build

```bash
# queue a build
curl -s -u ":$AZURE_PAT" -H "Content-Type: application/json" -d '{"definition":{"id":1}}' "https://dev.azure.com/{org}/$ENC/_apis/build/builds?api-version=7.1"
# list pipelines
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/pipelines?api-version=7.1"
# PAT scope: Build Read & Execute
```

## Release

```bash
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/release/releases?api-version=7.1"
# PAT scope: Release Read & Write
```

## Artifacts / Packaging

```bash
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/_apis/packaging/feeds?api-version=7.1"
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/packaging/feeds/{feedId}/packages?api-version=7.1"
# PAT scope: Packaging Read & Write
```

## Test

```bash
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/test/plans?api-version=7.1"
# PAT scope: Test Management Read & Write
```

## Projects / Teams / Iterations

```bash
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/_apis/projects?api-version=7.1"
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/wit/classificationNodes/Iterations?\$depth=10&api-version=7.1"
```

All endpoints listed at https://learn.microsoft.com/en-us/rest/api/azure/devops?view=azure-devops-rest-7.1 work the same way — swap `/_apis/{area}/...` and use the matching PAT scope.
