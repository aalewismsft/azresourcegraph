### 1. List all solutions in a Log Analytics Workspace

```kql
resources
| where type == "microsoft.operationsmanagement/solutions"
| extend Workspace = tostring(split(properties.workspaceResourceId, '/')[8])
| project Workspace, Solution = name
| summarize Solutions = make_list(Solution) by Workspace
```

### 2. List all VMs attached to a workspace

```kql
Heartbeat
| summarize arg_max(TimeGenerated, *) by Category, Computer, _ResourceId
| extend Workspace = tostring(split(_ResourceId, '/')[4])
| summarize
    MachineCount = count(),
    MMACount = countif(Category == "Direct Agent"),
    AMACount = countif(Category == "Azure Monitor Agent")
    by Workspace
| extend counts = pack_array(MMACount, AMACount)
| extend migstatus = case(
                         MMACount != 0 and AMACount != 0,
                         "In Progress",
                         AMACount != 0 and MMACount == 0,
                         "Completed",
                         "Not Started"
                     )
```
