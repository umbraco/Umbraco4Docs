---
versionFrom: 9.0.0
state: complete
updated-links: true
verified-against: alpha-3
---

# Health check: Macro errors

_Checks to make sure macro errors are not set to throw a YSOD (yellow screen of death), which would prevent certain or all pages from loading completely._

## How to fix this health check

This health check can be fixed by providing configuration on the following path: `Umbraco:CMS:Content:MacroErrors`.

This configuration can be setup in a configuration source of your choice. This guide shows how to set it up in one of the json file sources.

### Updating the json configuration

The following json needs to be merged into one of you json sources. By default the following json sources are used: `appSettings.json` and `appSettings.<environment>.json`, e.g. `appSettings.Development.json` or `appSettings.Production.json`.

```json
{
    "Umbraco": {
        "CMS": {
            "Content": {
                "MacroError": "<Inline(default)|Silent|Throw|Content>"
            }
        }
    }
}
```

One example that can be used in production, but is not recommended for development:

```json
{
    "Umbraco": {
        "CMS": {
            "Content": {
                "MacroError": "Silent"
            }
        }
    }
}
```
