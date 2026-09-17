# Azure Boards API — Copy-paste

All examples use placeholders `{org}`, `{project}`, `{team}`. Set:

```bash
export AZURE_PAT="paste" # read -s
ENC=$(python3 -c "import urllib.parse; print(urllib.parse.quote('{project}'))")
BASE="https://dev.azure.com/{org}/$ENC"
```

## Create 1 Task under Story

```bash
curl -s -u ":$AZURE_PAT" -H "Content-Type: application/json-patch+json" -d '[
  {"op":"add","path":"/fields/System.Title","value":"Add schema: minimal draft"},
  {"op":"add","path":"/fields/System.AreaPath","value":"{area}"},
  {"op":"add","path":"/fields/System.IterationPath","value":"{project}\\Sprint 1"},
  {"op":"add","path":"/fields/System.AssignedTo","value":"{assignee}"},
  {"op":"add","path":"/relations/-","value":{"rel":"System.LinkTypes.Hierarchy-Reverse","url":"https://dev.azure.com/{org}/'"$ENC"'/_apis/wit/workItems/123"}}
]' "$BASE/_apis/wit/workitems/\$Task?api-version=7.1" | python3 -m json.tool
```

Node (proper escaping):
```js
const auth="Basic "+Buffer.from(":"+process.env.AZURE_PAT).toString("base64");
const body=[
  {op:"add",path:"/fields/System.Title",value:"Add schema: minimal draft"},
  {op:"add",path:"/fields/System.AreaPath",value:"{area}"},
  {op:"add",path:"/fields/System.IterationPath",value:"{project}\\Sprint 1"},
  {op:"add",path:"/fields/System.AssignedTo",value:"{assignee}"},
  {op:"add",path:"/relations/-",value:{rel:"System.LinkTypes.Hierarchy-Reverse",url:`https://dev.azure.com/{org}/${ENC}/_apis/wit/workItems/123`}}
];
await fetch(`https://dev.azure.com/{org}/${ENC}/_apis/wit/workitems/$Task?api-version=7.1`,{method:"POST",headers:{"Content-Type":"application/json-patch+json",Authorization:auth},body:JSON.stringify(body)});
```

## Read

```bash
# single
curl -s -u ":$AZURE_PAT" "$BASE/_apis/wit/workitems/123?\$expand=all&api-version=7.1" | python3 -m json.tool
# batch
curl -s -u ":$AZURE_PAT" "$BASE/_apis/wit/workitems?ids=123,124&fields=System.Id,System.Title,System.State&api-version=7.1"
# WIQL
curl -s -u ":$AZURE_PAT" -H "Content-Type: application/json" -d '{"query":"Select [System.Id] From WorkItems Where [System.TeamProject]='\''{project}'\'' And [System.IterationPath]='\''{project}\\Sprint 1'\'' And [System.Parent]=123"}' "$BASE/_apis/wit/wiql?api-version=7.1"
```

## Update state

```bash
curl -s -u ":$AZURE_PAT" -X PATCH -H "Content-Type: application/json-patch+json" -d '[{"op":"add","path":"/fields/System.State","value":"Closed"},{"op":"add","path":"/fields/System.Reason","value":"Completed"}]' "$BASE/_apis/wit/workitems/123?api-version=7.1"
```

## Delete

```bash
curl -s -u ":$AZURE_PAT" -X DELETE "$BASE/_apis/wit/workitems/123?api-version=7.1" # recycle bin
```

## Taskboard columns

```bash
TEAM=$(python3 -c "import urllib.parse; print(urllib.parse.quote('{team}'))")
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/$TEAM/_apis/work/taskboardcolumns?iterationId={iterationId}&api-version=7.1" | python3 -m json.tool
curl -s -u ":$AZURE_PAT" "https://dev.azure.com/{org}/$ENC/_apis/wit/workitemtypes/Task/states?api-version=7.1" | python3 -m json.tool
```
