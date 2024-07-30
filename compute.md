# List VM Extensions, Extension Publisher and Version

```kql
resources
| where type == "microsoft.compute/virtualmachines/extensions"
| project VMName = split(id, '/')[8], ExtensionName = name, Publisher = properties.publisher, Type = properties.type, Version = properties.typeHandlerVersion
```
