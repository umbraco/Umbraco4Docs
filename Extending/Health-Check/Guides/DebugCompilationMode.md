---
versionFrom: 9.0.0
state: complete
updated-links: true
verified-against: alpha-3
---

# Health check: Debug Compilation Mode

_Leaving debug compilation mode enabled can severely slow down a website and take up more memory on the server._

## How to fix this health check

This health check can be fixed by providing configuration on the following path: `Umbraco:CMS:Hosting:Debug`.

This configuration can be setup in a configuration source of your choice. This guide shows how to set it up in one of the json file sources.

### Updating the json configuration

The following json needs to be merged into one of you json sources. By default the following json sources are used: `appSettings.json` and `appSettings.<environment>.json`, e.g. `appSettings.Development.json` or `appSettings.Production.json`.

```json
{
  "Umbraco": {
    "CMS": {
      "Hosting": {
        "Debug": <true|false>
      }
    }
  }
}
```

One example that can be used for production:

```json
{
  "Umbraco": {
    "CMS": {
      "Hosting": {
        "Debug": false
      }
    }
  }
}
```
