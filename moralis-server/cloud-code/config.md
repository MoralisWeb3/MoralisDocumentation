---
description: Environment Variables.
---

# Config

## Config <a href="config" id="config"></a>

Moralis Config offers a convenient way to configure parameters in Cloud Code.

```javascript
const config = await Moralis.Config.get({useMasterKey: true});
const privateParam = config.get("privateParam");
```

By default, Moralis Config parameters can be publicly read, which may be undesired if the parameter contains sensitive information that should not be exposed to clients. A parameter can be made readable only with the master key by setting the `Requires master key?` property via the Moralis Dashboard to `Yes`.

### How to set Moralis Config parameters from Dashboard UI
![](../.gitbook/assets/image_Config_Parse_Dashboard_UI.png)
