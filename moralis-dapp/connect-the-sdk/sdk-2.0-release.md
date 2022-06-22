# 🔥 SDK 2.0 release

{% hint style="warning" %}
Moralis JS SDK 2.0 alpha version is currently out for use. \
**Important:** **Do not use this alpha version in production**

This version is an **alpha** release and is under active development. Expect breaking changes until it is out of alpha.
{% endhint %}

{% hint style="info" %}
For Moralis JS SDK 2.0 features - please check out [Moralis JS SK 2.0 readme](https://github.com/MoralisWeb3/Moralis-JS-SDK/blob/alpha/README.md).
{% endhint %}

To test out the features of Moralis JS SDK 2.0, please follow the following steps -

### 1. Creating React App

To start a new Create React App project with TypeScript, you can run:

{% tabs %}
{% tab title="npx" %}
```shell
npx create-react-app my-app --template typescript
```
{% endtab %}

{% tab title="yarn" %}
```shell
yarn create react-app my-app --template typescript
```
{% endtab %}
{% endtabs %}

### 2. Install the SDK

Make sure to have **react**, **react-dom** and **moralis** installed as dependencies.&#x20;

{% tabs %}
{% tab title="npm" %}
```
npm install moralis
```
{% endtab %}

{% tab title="yarn" %}
```
yarn add moralis 
```
{% endtab %}
{% endtabs %}

### 3. Create env file

{% code title=".env" %}
```
SKIP_PREFLIGHT_CHECK=true

#Your server url from moralis admin
REACT_APP_SERVER_URL = "" 

#Your app id from moralis admin
REACT_APP_APP_ID = ""

#Optional
REACT_APP_API_KEY = ""

#Optional
REACT_APP_MORALIS_SECRET = ""
```
{% endcode %}

_Server URL_ and _APP ID_ you can get from your Moralis Dashboard. Go to your Moralis Dashboard and click on _View Details_ next to the server name of your server.

![Click on Settings below the server name of your server.](<../../.gitbook/assets/Server-dashboard.png>)

![Dapp Details](<../../.gitbook/assets/Server-credentials.png>)

### 4. Edit App.tsx

We can authenticate the user and make a native transaction

{% code title="App.tsx" %}
```tsx
import Moralis from 'moralis';
import { AuthMethod } from '@moralisweb3/server/lib/AuthMethods/types';

// @ts-ignore
// symlinks npm install --no-delete-symlinks
window.Moralis = Moralis;

Moralis.start({
  serverUrl: process.env.REACT_APP_SERVER_URL,
  appId: process.env.REACT_APP_APP_ID,
  apiKey: process.env.REACT_APP_API_KEY,
  logLevel: 'verbose',
});

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <div>
          <h1>Test app</h1>
          <h2>Authenticate User</h2>
          {/* Authenticate User*/}
          <button onClick={() => Moralis.Server.authenticate(AuthMethod.EVM, { connector: 'metamask', silent: false })}>
              Authenticate via EVM metamask
          </button>
          {/* Transfer Eth from wallet*/}
          <button
          onClick={async () => {
            console.log('Transfer native');
            const txResponse = await Moralis.Evm.transferNative({
              to: '0x295522b61890c3672D12eFbFf4358a6411CE996F',
              value: EvmNative.create('0.0001'),
            });
  
            console.log('TX', txResponse);
            console.log('TX json', txResponse.toJSON());
            console.log('TX exporerUrl', txResponse.exporerUrl);
  
            const receipt = await txResponse.wait();
            console.log('RECEIPT', receipt);
            console.log('RECEIPT json', receipt.toJSON());
            console.log('RECEIPT totalGasCost', receipt.totalGasCost);
        }}
      >
        Transfer eth
      </button>
        </div>
      </header>
    </div>
  );
}

export default App;

```
{% endcode %}



### 5. View the page from localhost

Run your app on `localhost` with the following command in your project directory where `package.json` is located

{% tabs %}
{% tab title="npm" %}
```bash
npm start
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn start
```
{% endtab %}
{% endtabs %}



