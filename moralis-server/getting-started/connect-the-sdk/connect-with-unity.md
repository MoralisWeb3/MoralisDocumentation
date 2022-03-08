---
description: >-
  Once you have your Moralis Server launched it's time to connect to it via the
  Moralis Unity SDK. This guide will show you how you can do it in Unity 3D with
  the Web3 Unity Boilerplate
---

## Web3 Unity Boilerplate

* The Unity Boilerplate include C# Moralis SDK and has an example Unity scene allowing you to login your users via their wallets, read their tokens and NFTs, interact with smart contracts and much more.

{% embed url="https://github.com/ethereum-boilerplate/ethereum-unity-boilerplate" %}
Web3 Unity Boilerplate
{% endembed %}

## Adding Moralis to Your Unity Game

### Downloading the SDK

Downlaod the latest version of the SDK [here](https://github.com/ethereum-boilerplate/ethereum-unity-boilerplate)
Then navigate to the releases and select the lastest version of the sdk. downlaod it as a unity package

![](<../../../.gitbook/assets/downloadtheunitysdk.gif>)

### Creating a unity project

* Create a new Unity project with Unity HUB, you can select any template of your choice, add a name to your project and a location.
* When the project is created, navigate to the folder you downloaded the package, drag and drop the package into the Assets folder in unity, an importing menu will popup and then click import to import the package into your project.

![](<../../../.gitbook/assets/importingthesdk.gif>)

 > NOTE: If after importing but before running the package, you see an error that describes something as "unsafe", it is probably due to a block of code in Nethereum SCrypt.cs. Open Build Settings -> Player Settings, scroll a bit to the bottom and check "Allow unsafe Code".

 ![](<../../../.gitbook/assets/unsafe.gif>)
### Configuring the project

* In your unity project, open MoralisWeb3ApiSdk->Example folder and double-click on the DemoScene to lauch it in the scene view.
* In the "Hierachy" panel under DemoScene click on the "MoralisSetup" gameobject, if the attached script sub-section is not expanded, expand it, also do the same with the "WalletConnect" gameobject, take a look at this script and explore their code and the varibales they links with. 
* Using the information from your Moralis Server, fill in Application Id, and Server URL on the "MoralisController" script attached to the "MoralisSetup". 

![](<../../../.gitbook/assets/addingserverkeys.gif>)

#### For WEBGL
* In Player Settings change the WebGL template to the Moralis WebGL Template. 

![](<../../../.gitbook/assets/buildingforwebgl.gif>)

### Running the application
* Run the application by clicking the Play icon located at the top, center of the Unity3D.
* Click on the "Authenticate" button to authenticate to Moralis using your Wallet.
* Explore the demoscene.

> Webgl can only be tested on build

### See all User Assets in the Moralis Database
As soon as the user logs in Moralis fetches all the on-chain data about that user from all chains and puts it into the Moralis Database. To see the Moralis Database go your server and click on _Dashboard_.

![Click on Dashboard in order to see the database of your server.](<../../../.gitbook/assets/Screenshot 2021-10-15 at 18.38.52.png>)

You will see the database of that server once you click _Dashboard_. Moralis fetches data from all blockchain where the address of the user has been active and you can see and query all tokens, NFTs and past transactions of the user all in one database.

![Moralis Database fetches all user data from all chains and updates it in real time in case users move their assets on chain.](<../../../.gitbook/assets/Screenshot 2021-10-15 at 18.44.04 (1).png>)

## Tutorial guides
Guides on how to use the moralis unity sdk in unity on various platform.

{% embed url="https://www.youtube.com/watch?v=FY77ImUpciI" %}
Unity WEBGL Metamask Wallet Connection
{% endembed %}

{% embed url="https://www.youtube.com/watch?v=5Bz-r1KBres" %}
Multiplayer Web3 game with Unity and Photon
{% endembed %}
