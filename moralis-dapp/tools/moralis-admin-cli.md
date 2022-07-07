---
description: >-
  A Command Line Interface (CLI) - Update Cloud Code and Plugins on the Server
  Faster and Easier.
---

# CLI

## Initial Setup

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/WtAlRwkB12w" %}

To use the Moralis Admin CLI, you need to install it by running the following code in the terminal:&#x20;

```bash
npm install -g moralis-admin-cli
```

To see the complete list of available commands, run the `help `command:

```bash
moralis-admin-cli help
```

To get further information about a command, append `help`to the command:

```bash
moralis-admin-cli watch-cloud-file help
```

### Arguments to the Commands

You can specify the arguments for each command in a combination of three ways:

#### 1. Inline Arguments

You can specify the arguments inline by either the short or long flag:

```bash
moralis-admin-cli watch-cloud-file --moralisApiKey 4fj5edfj553jdj5jfd
# Or
moralis-admin-cli watch-cloud-file -k 4fj5edfj553jdj5jfd
```

#### 2. Use an .env File

You can place most arguments in a .env file:

```bash
moralisApiKey=4fj5edfj553jdj5jfd
moralisApiSecret=5jd7kg3kd9d93jfljkb
...
```

By doing this, you don't need to specify the arguments inline as long as the .env file is located in the directory that you are running the command from.

#### 3. Use Environment Variables

You can also store the arguments as environment variables on your local machine.

To do this on a Windows machine, press the "Windows Key" and type "environment variables".

Select "Edit the system environment variables":

![](<../../.gitbook/assets/image (50).png>)

Select "Environment Variables":

![](<../../.gitbook/assets/image (51).png>)

Select: "New":

![](<../../.gitbook/assets/image (52).png>)

Input the name and value for the variable and press OK:

![](<../../.gitbook/assets/image (53).png>)

Repeat for each variable you want to store.

## watch-cloud-folder

You can write your cloud functions in your preferred IDE by using the moralis-admin-cli.

Here is how you can use CLI to upload all JS files in a folder as cloud code. All files will be merged together.

```bash
 moralis-admin-cli watch-cloud-folder --moralisApiKey API_KEY --moralisApiSecret API_SECRET --moralisSubdomain SERVER_URL --autoSave 1 --moralisCloudfolder PATH
```

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/rJ88MPODVRg?t=507" %}
How to write cloud functions locally on your machine, use multiple files and upload to the server.
{% endembed %}

After you've run the command, the cloud code will be updated automatically on the backend with each save!

## connect-local-devchain

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/p8Rr5nMruSI" %}

In order to use this command, you need to have frpc on your computer. You can get it here: [https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases).

You can now run the following command to start the process (Insert your own key, secret, and path):

```bash
moralis-admin-cli connect-local-devchain --moralisApiKey FAM6pYzBKjcM
 --moralisApiSecret 7thQGNKKqX8s --frpcPath "C:\Program Files\frpc\frpc.exe"
```

To get more information you can write:&#x20;

```bash
moralis-admin-cli connect-local-devchain help
```

This will show you a more detailed description of the command and show how you can call the command without the need to specify the arguments each time.

You can leave out the arguments if you follow option [2 ](https://docs.moralis.io/moralis-admin-cli#2-use-an-env-file)or [3 ](https://docs.moralis.io/moralis-admin-cli#3-use-environment-variables)in the [Initial Setup](https://docs.moralis.io/moralis-admin-cli#initial-setup) section.

## update-server

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/xamQDNAQVM0" %}

You can update and restart your Moralis servers from the command line by using the `update-server`command.

You can now run the following command to start the process (Insert your own key, secret, and path):

```bash
moralis-admin-cli update-server --moralisApiKey FAM6pYzBKjcM
 --moralisApiSecret 7thQGNKKqX8s
```

To get more information you can write:&#x20;

```bash
moralis-admin-cli update-server help
```

This will show you a more detailed description of the command and show how you can call the command without the need to specify the arguments each time.

You can leave out the arguments if you follow option [2 ](https://docs.moralis.io/moralis-admin-cli#2-use-an-env-file)or [3 ](https://docs.moralis.io/moralis-admin-cli#3-use-environment-variables)in the [Initial Setup](https://docs.moralis.io/moralis-admin-cli#initial-setup) section.

## create-server

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/Noo_cNLSyqU" %}

You can create new Moralis servers from the command line by using the `create-server`command.

Run the following command to start the process (Insert your own key, secret, and path):

```bash
moralis-admin-cli create-server --moralisApiKey FAM6pYzBKjcM
 --moralisApiSecret 7thQGNKKqX8s
```

To get more information you can write:&#x20;

```bash
moralis-admin-cli create-server help
```

This will show you a more detailed description of the command and show how you can call the command without the need to specify the arguments each time.

You can leave out the arguments if you follow option [2 ](https://docs.moralis.io/moralis-admin-cli#2-use-an-env-file)or [3 ](https://docs.moralis.io/moralis-admin-cli#3-use-environment-variables)in the [Initial Setup](https://docs.moralis.io/moralis-admin-cli#initial-setup) section.

## add-contract

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://youtu.be/oD81b75s6Ik" %}

\
You can choose to listen and sync smart contract events to your Moralis servers from the command line by using the `add-contract`command.‌

Run the following command to start the process (Insert your own key, secret, and path): exit: Ctrl+↩

```bash
moralis-admin-cli add-contract --moralisApiKey FAM6pYzBKjcM 
--moralisApiSecret 7thQGNKKqX8s --abiPath ".\MyContract.json"
```

To get more information you can write:

```bash
moralis-admin-cli add-contract help
```

‌This will show you a more detailed description of the command and show how you can call the command without the need to specify the arguments each time.‌

You can leave out the arguments if you follow option [2](https://docs.moralis.io/moralis-admin-cli#2-use-an-env-file) or [3](https://docs.moralis.io/moralis-admin-cli#3-use-environment-variables) in the [Initial Setup](https://docs.moralis.io/moralis-admin-cli#initial-setup) section.

### Table Name Restrictions

Keep in mind that table names are not allowed to contain numeric characters (0-9).

## get-logs

You can get real-time logs from Moralis by running the "get-logs" script.

{% hint style="info" %}
Legacy UI is present in the video, some things might be different
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=r8kWG-9tAXo&ab_channel=MoralisWeb3" %}

```bash
moralis-admin-cli get-logs --moralisApiKey MORALIS_CLI_API_KEY --moralisApiSecret MORALIS_CLI_SECRET_KEY
```

This command prints logs from Moralis in your terminal and gives real-time information about errors and warnings generated by the Moralis Server.

Make sure to run the "help" command to find out more.

```bash
moralis-admin-cli get-logs help
```
