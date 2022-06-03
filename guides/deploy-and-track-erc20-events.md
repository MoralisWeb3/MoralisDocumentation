# Deploy and Track ERC-20 Events

The following guide will explain how to deploy an ERC-20 smart contract, sync and watch contract events, and saving the data into the Moralis database:

For this example, we deployed an ERC-20 contract with the symbol MOR with "**ERC20PresetMinterPauser.sol"** using our local Ganache instance, [Remix](https://remix.ethereum.org), and MetaMask. We also want to track all "\_mint" events for our contract _(keep in mind that "\_mint" events in OpenZeppelin emits a transfer event, more info_ [_HERE_](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20#ERC20-\_mint-address-uint256-)_)._

### Set Up Ganache

The first step will be to download and install Ganache from [https://www.trufflesuite.com/ganache](https://www.trufflesuite.com/ganache), install it and quickstart a new Ethereum chain:

![](<../.gitbook/assets/image (7).png>)

After quick-starting, you'll have ten ETH Wallets with 100.00 ETH on each one.

![](<../.gitbook/assets/image (9).png>)

### Set Up MetaMask

The next step will be to set up MetaMask in your browser. If you don't have it already then you can get it from [https://metamask.io/](https://metamask.io) and we'll connect it to our local Ganache instance.

![](<../.gitbook/assets/image (11).png>)

Click the MetaMask icon on the browser, and select the network drop-down menu. Here we should connect to our local Ganache Instance. Click “Custom RPC”.

![](<../.gitbook/assets/image (12).png>)

* **Network Name**: Ganache
* **New RPC URL**: http://localhost:7545
* **Chain Id**: 1337

This information should be enough to connect MetaMask to your local Ganache.

We're now going to connect one of the ETH addresses to our MetaMask. For this, we choose one address and click on the key icon to get the private key:

![](<../.gitbook/assets/image (17).png>)

![](<../.gitbook/assets/image (18).png>)

Then we open MetaMask and click on the top right avatar and click on "Import Account."

![](<../.gitbook/assets/image (19).png>)

![](<../.gitbook/assets/image (20).png>)

Just paste your private key, and the account will be now connected.

### Deploy Smart Contract

Now, we can create our mintable token on Remix. Open Remix on your browser, or go to this link [https://remix.ethereum.org/](https://remix.ethereum.org).

Click on "New File."

![](<../.gitbook/assets/image (14).png>)

Give it a name, we called it "_**erc20.sol".**_

![](<../.gitbook/assets/image (15).png>)

Since we will use an ERC-20 contract from [OpenZeppelin](https://openzeppelin.com/contracts/), just paste this line to the file and save it. **T**o save, just use _CONTROL + S_ on Windows or _COMMAND + S_ on Mac.

```
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/presets/ERC20PresetMinterPauser.sol";
```

After save, you should see the contract imported from GitHub:

![](<../.gitbook/assets/image (16).png>)

Now, to compile our contract we need to pay attention to the following things:

![](<../.gitbook/assets/image (22).png>)

1. We have selected the "**ERC20PresetMinterPauser.sol"** file.
2. We are on the "Solidity Compiler" tab.
3. The Pragma Solidity version matches the compiler version.

Click on "Compile **ERC20PresetMinterPauser.sol**".

![](<../.gitbook/assets/image (23).png>)

For the next step, we need to make sure our account is connected to Remix. So, we just click on MetaMask, and if it's not connected then you should see the following pop-up, just click on "Connect."

![](<../.gitbook/assets/image (24).png>)

If the account is connected, then we can go to the "_**Deploy and Run Transactions Tab**_", and select environment "_**Injected Web3".**_

![](<../.gitbook/assets/image (25).png>)

You should see your wallet with 100 ETH. Now, select "**ERC20PresetMinterPauser.sol"** from the contract list, and click on the arrow to give your token a name.

![](<../.gitbook/assets/image (26).png>)

![](<../.gitbook/assets/image (27).png>)

After clicking on transact, you'll receive a pop-up from MetaMask asking for signature validation, just click on confirm.

![](<../.gitbook/assets/image (28).png>)

You should see your smart contract in the "**Deployed Contracts"** area.

![](<../.gitbook/assets/image (29).png>)

We're going now to mint some tokens to our address. Click on the arrow of your contract and open the mint event, input the address and amount.

Enter your address and an amount in WEI. For example, I will mint 1000 MOR tokes so, I entered “1000000000000000000000”.

![](<../.gitbook/assets/image (30).png>)

After clicking on transact, confirm the transaction with MetaMask and the tokens should now have arrived at the proper address.

### Add Token to MetaMask

To add the token(s) to MetaMask, we first need to get our token address. For this, we just click on the clipboard icon on the "**Deployed Contracts"** area.

![](<../.gitbook/assets/image (31).png>)

Then we go to MetaMask, add the custom token, and enter the token address. The token symbol should be automatically populated.

![](<../.gitbook/assets/image (32).png>)

![](<../.gitbook/assets/image (33).png>)

\
Congratulations! You deployed your own ERC-20 token.

### Connecting Moralis to Your Local Ganache Instance

Now, on the Moralis admin page, go to your Ganache server and click "View Details", and go to the "Ganache Proxy Server" tab:

![](<../.gitbook/assets/image (35).png>)

![](<../.gitbook/assets/image (36).png>)

Moralis comes with a built-in proxy server just for you, that enables direct connection from your Ganache instance to your server without worrying about firewalls.

We need to get FRP from [https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases) depending on your OS/Hardware. In some Windows versions, FRP could be blocked by the firewall. If so, just use an older release, for example, frp\_0.34.3\_windows\_386. This is a known issue, more information here: [https://github.com/fatedier/frp/issues/2095](https://github.com/fatedier/frp/issues/2095)

Replace the content in "frpc.ini", the code assumes that your Ganache instance is hosted at port 7545.

Then you just need to run:

```
./frpc -c frpc.ini (Linux)
```

```
frpc.exe -c frpc.ini (Windows)
```

To verify the connection made is correct, just refresh the page (you can use F5) and you should now see the "CONNECTED" status:

![](<../.gitbook/assets/image (37).png>)

### Syncing and Watching Contract Events From Moralis

The next step will be to create a new plugin in the admin panel to sync and watch contract events:

![](<../.gitbook/assets/image (5).png>)

* **"description" -** Enter "Sync MOR Transfer Events". It's a small description for us to keep track of all the plugins we add to our instance.
* **"topic" -** We use "_**Transfer(address,address,uint256)",**_ but you can also use the sha3 topic "**0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef**".
* **"abi" -** Our abi includes three inputs: from, to, and value. We get this abi directly from Remix.
* **"address" -** "**0x4F27558d3F86670a9E2EfF294b7d10600266533F**" (our MOR contract address).
* **"tableName" -** "_**MORTransferEvent**",_ it's the name of the table that will be created in our database with all the events.

To get the abi from Remix, just go to the "Solidity Compiler" tab, choose the "**ERC20PresetMinterPauser.sol"** contract and click on the abi icon. The full abi will be copied to the clipboard, make sure you use only the abi for the event you are syncing:

![](<../.gitbook/assets/image (34).png>)

Make sure your "frp-ganache" is connected, and you will see the table continuously being filled with historical and real-time data.

![](<../.gitbook/assets/image (6).png>)

More details on [Sync and Watch Smart Contracts here](deploy-and-track-erc20-events.md#syncing-and-watching-contract-events-from-moralis).
