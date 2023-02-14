---
versionFrom: 9.0.0
versionTo: 10.0.0
meta.Title: "Umbraco Tours Settings"
description: "Information on the tours settings section"
---

# Tours settings

The tours section is a simple section that only contains one value `"EnableTours"` which allows you to enable or disable tours, by default tours are enabled. If you wish to disable tours set the `"EnableTours"` to false, like so:

```json
"Umbraco": {
  "CMS": {
    "Tours": {
      "EnableTours": false
    }
  }
}
```
