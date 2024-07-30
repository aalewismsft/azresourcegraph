### 1. List all solutions in a Log Analytics Workspace

```kql
resources
| where type == "microsoft.operationsmanagement/solutions"
| extend Workspace = tostring(split(properties.workspaceResourceId, '/')[8])
| project Workspace, Solution = name
| summarize Solutions = make_list(Solution) by Workspace
```
