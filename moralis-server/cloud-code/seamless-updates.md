---
description: Update cloud code without downtime.
---

# 🥳 Seamless Updates

### Seamless Update explained

You can enable `Seamless updates` in your server settings to avoid downtime each time you update cloud code.

You Moralis app is run by several workers (independent processes).

When the setting is enabled Moralis will roll-out your new cloud code to one worker at a time which will lead to the following side-effects you need to keep in mind:

* Different users will interact with different versions during a short period of time while the updated code is rolled out to all workers.
* Rolling out an update fully will take much more time (several minutes) as instead of rebooting all workers with new code Moralis reboots only one worker at a time.&#x20;

### When to use

This setting is important if you are in production and don't want downtime when you push new versions of your code. You are fine with the fact that rolling out new code will take a few minutes and that each request to your cloud code may be routed to either the new or the old version during the roll out process.

### When not to use

This setting is not for you if you want as fast updates as possible. For example if you are still developing your app you most likely want fast updates and are not interested in seamless rollouts.

This setting is also not for you if you are not ok with the fact that old and new versions will be served to different users for a few minutes while the roll out is happening.
