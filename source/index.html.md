---
title: API Reference

toc_footers:
  - 

includes:
  - errors

search: true

---

[on_chain_solution]: images/on_chain_solution.png
[architecture]: images/architecture.png
[verified_claims]: images/verified_claims.png
[open_id_connect]: images/open_id_connect.png
[login_process]: images/login_process.png
[integration_portal]: images/integration_portal.png

# Introduction

### Welcome to the official lifeID developer documentation

lifeID is a self-sovereign, open-source, decentralized platform designed to give everyone the tools they need to transact their identity as they wish. For more information on self-sovereign identity (SSI) and lifeID's mission click ....

### How is it used?

The way you interact with the lifeID ecosystem will depend on who are and what your goal is. For individual people, it will likely be through an app on their mobile phone. You will use it to authenticate yourself anytime you need to prove that you are who you say you are. The main advantages for individuals are the convenience of not having to remember a password and important privacy benefits, for example, knowing that your data will not be correlated between websites you login to (for more information about this see: _link to white paper_

Governments, corporations and non-profit organizations will interact with the lifeID platform primarily through an integration SDK that you can set up and use to verify that people are who they say they are. Organizations will also be able to issue verified credentials to any lifeID holder. The holder will control these credentials and decide who to share them with. (for more information about verified credentials see: _link to explanation of verified credentials_) Having a certain bank account balance (verified by the bank), being over a certain age (verified by a government), and having a college degree (verified by the college) are all examples that could be verified credentials.

For a more detailed description of the architecture and processes, see the architecture diagrams and reference section below

# Getting Started Guides

## I want to let people sign into my website with lifeID

### 1. Sign into the lifeID integration portal and create at least one trustee

First you will need to sign into the lifeID integration portal. The lifeID integration portal is an open source project that exists to help you integrate your organization with lifeID. It will provide you with a way to authenticate your users with lifeID, as well as assign trustees who can act on behalf of your organization.

If you don't already have it, download the lifeID app on your mobile device.

Go to the lifeID integration portal: _link to portal when live_

Follow the steps to set up your organization, which will be issued a decentralized identified ("DID") and registered to the blockchain of your choice. _while in alpha, the only choice available will be an ethereum testnet run by lifeID_

_Eventually, once lifeID launches on a main-net blockchain, a cost will be associated with creating a DID and registering it to a blockchain. Currently DID creatiion is free, but later payment processing will occur at this step._

The integration portal will allow you to assign trustees for your organization, trustees are individual people who are authorized to transact the organization's identity. Your organization must always have at least one trustee.

### 2. Configure and download your SDK

### After you create at least one trustee, the integration portal will generate your customized SDK which will contain two parts

1 A Javascript component to include in your front-end. The integration portal will allow you to select your front-end framework, style the component as you wish and then generate Javascript code to add to your site. The javascript component will be responsible for displaying a QR code that your end users must scan to login. If you wish to include any lifeID branded buttons the Javascript component can generate those as well. _(how many frameworks do we want to support? jquery, react, angular?)_

The Javascript SDK in detail:

```javascript
var payload = {
  did: did,
  application_id: applicationId,
  uid: sessionId,
  callback_url: callbackUrl,
  client_id: clientId,
  nonce: nonce,
  encryption_key: encryptionKey,
  scope_request: scopeRequest,
  credential_request: credentailRequest
};
```

The first part of the code to the right is forming an object that will be encoded into the QR code and sent to the lifeID holder.

```javascript
var web3 = new Web3();
var signature = web3.eth.accounts.sign(
  JSON.stringify(payload),
  "0x<signing-service-private-key>"
);
payload.signature = signature.signature;
var qrcode = new QRCode("qrcode", {
  height: 500,
  width: 500
});
qrcode.makeCode(JSON.stringify(payload));
```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

The second part of the Javascript component will convert that object into a JSON string that is signed with the private key of the lifeID signing service. In the final version, organizations with a DID will sign their own QR data, but currently lifeID signs the data to make the login process easier to implement.

```javascript
$("#auth_state").val(sessionId);
$("#nonce").val(nonce);
var url = "http://lifeid.oidc/codes/" + sessionId;
function checkServer(url) {
  var check = setInterval(function() {
    fetch(url).then(response => {
      if (response.status === 200) {
        response.json().then(body => {
          clearInterval(check);
          parent.postMessage({ sessionId, nonce, authCode: body.url }, "*");
        });
      }
    });
  }, 3000);
}
checkServer(url);
```

<br/>
<br/>
<br/>
<br/>
<br/>

The final part of the Javascript component polls the OIDC server and waits for the authentication. As soon as the component recieves the auth code from the OIDC server, the page will redirect and the user will be logged in.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

2 An OpenID Connect ("OIDC") core implementation that handles user authentication. The integration portal will generate code for your OIDC implementation in one of lifeID's supported languages. The OIDC client will integrate with your existing server code to handle all of the interactions with blockchains and the mobile app on your users' phones.

`what does this loook like?`
lifeID does not keep track of your users. If you wish to track and store any information about your uses, you will need to specifically request that information as part of the openID Connect scope. The user will then decide whether to share the information with you, at which point you may use it according to any terms that they have included. https://w3c.github.io/vc-data-model/#terms-of-use

### 3. Integrate the components with your site

You will need to add the Javascript component to your front-end as well as the OIDC implementation on the back-end.

There is a small cost associated with each new user that creates an account on your site. The exact cost will depend on which blockchain you use and the transaction costs associated with it. When it goes live the lifeID portal will process the payments for you.

_Some example costs and the approximate range_

_Need a way to allow the administrators to revoke users through the integration portal_

_do each of these components need a wallet?_

### I want to issue verified claims to lifeID holders

You will need to create an organization registered on a blockchain using the integration portal (see above). Next, you will recieve a verified claims toolkit, which has two services.

The first service is the signing API service, currently run by lifeID although this will likely change as we move out of beta. The signing API service will keep track of your cryptographic keys and provide a RESTful interface for you to sign claims.

The second service will run locally and allow you to verify claims that you have issued.

You'll need a GUI to manage and issue the claims

You'll need a way to process payments

_how does this work? what are you selling?_

You'll need a way to send ID tokens to a valid API bridge (part of the VC Toolkit?)

### I want to earn ID tokens by running an instance of the API bridge

You'll need an instance of the API bridge (Dockerized?)

You'll need instructions on how to deploy it

### I want to build an app that uses lifeID

Currently, your lifeID, such as it is, is the HD master private key. We generate it the first time you start up the lifeID mobile app. Do we let people share it with other apps? Or does "using lifeID" mean necessarily using the proprietary mobile app.

### Need to do something else?

Let us know at some@email.com or on discord at lifeid.discordorwhatever

# Components

## The admin portal

The lifeID admin portal is available on github at <https://github.com/lifeID/gssl-admin-portal>

The lifeID admin portal provides a way to manage users who have logged in with lifeID.

It can pay ID token to an instance of the lifeID API bridge

_this will probably end up being part of the integration portal, leave it out?_

## The multi-blockchain API bridge

It can write DID pairs to the blockchain

It will collect payments in ID token and then distribute some of that to the app developer (DIFFICULT TO DO SAFELY!!!) and some to the foundation.

## The multi-network DID encoder

It can encode and decode off-chain DIDs

## The lifeID openID Connect provider

The lifeID openID Connect provider will communicate between the integration portal, the mobile app and your website. It will run on your server and provide an API for your backend to work with.

## The smart contracts on the Ethereum blockchain

The contracts are currently

A set of utility functions to assist in encoding and decoding DIDs and transferring their ownership.

A contract that writes user and organization DIDs to a registry on the Ethereum blockchain

A contract that revokes user and organization DIDs from the Ethereum blockchain

A contract that migrates DIDs in bulk to the Ethereum blockchain

A contract that writes verified claim revocations to the blockchain registry.

# Architecture Diagrams

## LifeID Component Overview

![On Chain Solution][on_chain_solution]

## LifeID Architecture

![lifeID archtitecture][architecture]

## LifeID Verified Claims Flow

![lifeID verified claims flow][verified_claims]

## LifeID Interation Portal

![lifeID Integration Portal][integration_portal]

## LifeID Passwordless Login

![Login process diagram][login_process]

## LifeID OpenID Connect Integration in detail

![OpenID Connect flow][open_id_connect]

# Reference

Here will be definitions of all the individual terms such as DID?

Maybe duplicative?

```

```

```

```

```

```
