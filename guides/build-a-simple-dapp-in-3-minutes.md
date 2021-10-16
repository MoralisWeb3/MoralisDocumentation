---
description: >-
  Build Your First Moralis dApp! This Guide Series Covers the Basics and How to
  Get Started Fast. Part One Covers Server Setup.
---

# Build a Simple dApp in 3 Minutes - Setup (Part 1)

It goes so fast to build on Moralis. In this guide, we'll show you how easy it is to get a simple dApp up and running. Are you ready? Go...!

## Create a Free Moralis Account

Go to [Moralis](https://admin.moralis.io) and register a free account.

## Create a New Moralis Server

![Pick a cool name, select the region closest to you, then choose "Mainnet" for Network.](<../.gitbook/assets/image (92).png>)

This will take a couple of minutes to spin up. While that's working, let's start coding!

## Include Moralis on Your HTML Page

In order to communicate with the Moralis Server, we need to include the Moralis code in our dApp. Open your favorite text editor. We recommend [Visual Studio Code](https://code.visualstudio.com) with the ["Live Server" extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) to make running the web page super easy.

Create a new file called `index.html` and copy the following code.

```markup
<html>
  <head>
    <!-- Moralis SDK code -->
    <script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
    <script src="https://unpkg.com/moralis/dist/moralis.js"></script>
  </head>
  <body>
    <h1>Gas Stats With Moralis</h1>

    <button id="btn-login">Moralis Login</button>
    <button id="btn-logout">Logout</button>

    <script>
      // connect to Moralis server
      Moralis.initialize("paste Moralis APP ID here");
      Moralis.serverURL = "paste Morlis server URL here";
    </script>
  </body>
</html>
```

### Copy Moralis Server URL and Application ID

Once the server instance has been created, find your Moralis server URL and application ID and paste them into the `<script>` tag where indicated.

![Click the "View Details" button](<../.gitbook/assets/image (94).png>)

![Paste URL and ID into your HTML file where indicated.](<../.gitbook/assets/image (96).png>)

### Congratulations, You Made It!

Your server is up and running. Keep going! Next up is part two where we learn how to log in with MetaMask.

{% content-ref url="build-a-simple-dap-in-3-mins-login-part-2.md" %}
[build-a-simple-dap-in-3-mins-login-part-2.md](build-a-simple-dap-in-3-mins-login-part-2.md)
{% endcontent-ref %}

