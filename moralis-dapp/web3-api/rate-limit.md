---
description: Limit the number of requests to the web3-api for users
---

# Rate limit

You might want to limit the number of requests that a user can make to the web3Api. To do this, you can modify. You can do this to call `Moralis.settings.setAPIRateLimit` in your cloud-code:

```
Moralis.settings.setAPIRateLimit({
  anonymous:10, authenticated:20, windowMs:60000
})
```

This will limit the number of requests for unauthenticated users to 10 per minute (60000 ms) and 20 per minute for authenticated users.

The values are set by default:

- anonymous: 30
- authenticated: 50
- windowMs: 60000 (1 minute)
