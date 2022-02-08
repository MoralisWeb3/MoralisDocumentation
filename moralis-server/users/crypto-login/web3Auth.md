
---

description: Moralis is fully integrated with Web3Auth

---

  

# Web3️⃣Auth

  

## Integrating Morlais and Web3Auth

 
Moralis and Web3Auth Integration

Moralis supports authentication using [Web3Auth](https://web3auth.io/). This allows for user onboarding through both social logins, and web3 wallets.

To get started, make an account [here](https://dashboard.web3auth.io/)  and get the publishable clinetId. 
```javascript

clinetId: 'ABC*****************'

```
Next, add the sdk into your app.

_If you import moralis via a CDN:_
```javascript
<script  src="https://unpkg.com/@web3auth/base@0.2.2/dist/base.umd.min.js"></script>

<script  src="https://unpkg.com/@web3auth/web3auth@0.2.3/dist/web3auth.umd.min.js"></script>
```
_If you import moralis via a NPM or another package manager:_

```bash
npm install --save @web3auth/web3auth

npm i --save @web3auth/base
```

Then call authenticate like above, but with a provider option, and the required params. The `chainId`, `rpcTarget`, and `clientId` are all required params.

* `chainId`: The chainId of the supported network you are looking to connect to. 
	*Supported networks are highlighted below*
* `clinetId`: The publishable clientId that you got from the web3Auth [dashboard](https://web3auth.io/).
* `rpcTarget`: An RPC target for your corresponding chain. Moralis speedy nodes provide quick and easy access to RPC URLs.

  

```javascript

const user = await Moralis.authenticate({
	provider: "web3Auth",
	chainId: Moralis.Chains.ETH_ROPSTEN,
	clientId: "ABC*****************",
	rpcTarget: "htttps:.../eth/ropsten",
})

```
Supported Chains
* Eth Mainent 
* Eth Ropsten 
* Eth Rinkbey 
* Eth Kovan 
* Eth Goerli 
* Matic Mainet 
 > Note: Chain IDs for corresponding chains can be found within Moralis.Chains. See the example above.
