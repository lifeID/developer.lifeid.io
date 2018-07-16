---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - ruby
  - python
  - javascript
  - go

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

The way you interact with the lifeID ecosystem will depend on who are and what your goal is. For individual people, it will likely be through an app on their mobile phone. You will use it to authenticate yourself anytime you need to prove that you are who you say you are. The main advantages for individuals are the convenience of not having to remember a password and important privacy benefits, for example, knowing that your data will not be correlated between websites you login to (for more information about this see: <something>)

Governments, corporations and non-profit organizations will interact with the lifeID platform primarily through an integration SDK that you can set up and use to verify that people are who they say they are. Organizations will also be able to issue verified credentials to any lifeID holder. The holder will control these credentials and decide who to share them with. (for more information about verified credentials see: <something>) Having a certain bank account balance (verified by the bank), being over a certain age (verified by a government), and having a college degree (verified by the college) are all examples that could be verified credentials.

For a more detailed description of the architecture and processes, go here<something>

# Getting Started Guides

### I want to let people sign into my website with lifeID

First you will need to sign into the lifeID integration portal. The lifeID integration portal is an open source project that exists to help you integrate your organization with lifeID. It will provide you with a way to assign trustees who can act on behalf of your organization, as well as a developer SDK

If you don't already have it, download the lifeID app on your mobile device.

Go to the lifeID integration portal: <lifeid.io/integration-portal-or-whatever>

Follow the steps to set up your organization which will be registered to the flippin' BLOCKCHAIN. AWESOME!!

_how do you pay for the blockchain writes at this stage do we need to implement payment processing?_

The integration portal will allow you to assign trustees for your organization, trustees are individual people who are authorized to transact the organization's identity. After you create at least one trustee, the integration portal will generate your customized SDK which will contain two parts. Each is explained in more detail below:

1.  A Javascript component to include in your front-end. The integration portal will allow you to select your front-end framework, style the component as you wish and then generate Javascript code to add to your site. The javascript component will be responsible for displaying a QR code that your end users must scan to login. If you wish to include any lifeID branded buttons the Javascript component can generate those as well. _(how do we do this? jquery, react, angular?)_

2.  An OpenID Connect ("OIDC") core implementation that handles user authentication. The integration portal will generate code for your OIDC implementation in one of lifeID's supported languages. The OIDC client will integrate with your existing server code to handle all of the interactions with blockchains and the mobile app on your users' phones.

_here will need code examples_

There is a small cost associated with each new user that creates an account on your site. The exact cost will depend on which blockchain you use and the transaction costs associated with it. When it goes live the lifeID portal will process the payments for you.

_Some example costs and the approximate range_

lifeID does not currently keep track of your users.

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

That's all right now

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
