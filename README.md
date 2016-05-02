- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
    - [Obtaining an Access Token](#obtaining-an-access-token)
    - [Install](#install)
- [Use case](#use-case)
    - [Step 0. Initialization](#step-0-initialization)
    - [Step 1. Generate and Publish the Keys](#step-1-generate-and-publish-the-keys)
    - [Step 2. Encrypt and Sign](#step-2-encrypt-and-sign)
    - [Step 3. Send a Message](#step-3-send-a-message)
    - [Step 4. Receive a Message](#step-4-receive-a-message)
    - [Step 5. Verify and Decrypt](#step-5-verify-and-decrypt)
- [Source code](#source-code)

## Introduction

In this guide we will get you up and running quickly with a simple IP messaging chat application you can build as you learn more about Virgil Crypto Library and Virgil Keys Services. Sounds like a plan? Then let's get cracking!

On the diagram below you can see a full picture of how these things interact with each other. ![Use case mail](https://raw.githubusercontent.com/VirgilSecurity/virgil/master/images/IPMessaging.jpg)

## Prerequisites

1. To begin with, you'll need a Virgil Access Token, which you can obtain by passing a few steps described [here](#obtaining-an-access-token).
2. You will also need to [install a NuGet package](#install).

### Obtaining an Access Token

First you must create a free Virgil Security developer's account by signing up [here](https://developer.virgilsecurity.com/account/signup). Once you have your account you can [sign in](https://developer.virgilsecurity.com/account/signin) and generate an access token for your application.

The access token provides authenticated secure access to Virgil Keys Services and is passed with every API call. The access token also allows the API to associate your app’s requests with your Virgil Security developer's account.

Use this token to initialize the SDK client [here](#step-0-initialization).

### Install

Use NuGet Package Manager (Tools -> Library Package Manager -> Package Manager Console) to install Virgil.SDK package, running the command:

```
PM> Install-Package Virgil.SDK
```

## Use Case
**Secure any data end to end**: users need to securely exchange information (text messages, files, audio, video etc) while enabling both in transit and at rest protection. 

- Application generates public and private key pairs using Virgil Crypto library and uses Virgil Keys service to enable secure end to end communications:
    - public key on Virgil Public Keys Service;
    - private key on Virgil Private Keys Service or locally.
- Sender’s information is encrypted in Virgil Crypto Library with the recipient’s public key.
- Sender’s encrypted information is signed with his private key in Virgil Crypto Library.
- Application securely transfers the encrypted data, sender’s digital signature and UDID to the recipient without any risk to be revealed.
- Application on the recipient’s side verifies that the signature of transferred data is valid using the signature and sender’s public key in Virgil Crypto Library.
- The received information is decrypted with the recipient’s private key using Virgil Crypto Library.
- Decrypted data is provided to the recipient.

### Step 0. Initialization

Initialize the service Hub instance using access token obtained [here...](#obtaining-an-access-token)

```csharp
ServiceHub = ServiceHub.Create("%ACCESS_TOKEN%");
```

### Step 1. Generate and Publish the Keys
First a simple IP messaging chat application is generating the keys and publishing them to the Public Keys Service where they are available in open access for other users (e.g. recipient) to verify and encrypt the data for the key owner.

The following code example generates a new public/private key pair.

```csharp
var keyPair = VirgilKeyPair.Generate();
```

The app is registering a Virgil Card which includes a public key and an email address identifier. The Card will be used for the public key identification and searching for it in the Public Keys Service. You can create a Virgil Card with or without identity verification, see both examples [here...](/api-docs/dot-net-csharp/keys-sdk#publish-a-virgil-card)  

```csharp
var senderEmailAddress = 'sender@virgilsecurity.com';
var emailVerifier = await serviceHub.Identity.
VerifyEmail(senderEmailAddress);

// Confirm an identity using code received to email box.

var authorizedIdentity = emailVerifier.Confirm("%CONFIRMATION_CODE%")

var card = await serviceHub.Cards.Create(authorizedIdentity, 
keyPair.PublicKey(), keyPair.PrivateKey());
```

### Step 2. Encrypt and Sign
The app is searching for all channel members' public keys on the Keys Service to encrypt a message for them. The app is signing the encrypted message with sender’s private key so that the recipient can make sure the message had been sent by the declared sender.

```csharp
var messageBytes = Encoding.UTF8.GetBytes(message);

var channelRecipients = await this.GetChannelRecipients();
 
var encryptedMessage = CryptoHelper.Encrypt(messageBytes, 
channelRecipients);
var sign = CryptoHelper.Sign(encryptedMessage, 
             this.currentMember.PrivateKey);
```

### Step 3. Send a Message
The app merges the message text and the signature into one [structure](https://github.com/VirgilSecurity/virgil-sdk-net/blob/master/Examples/Virgil.Examples.IPMessaging/EncryptedMessageModel.cs) then serializes it to json string and sends the message to the channel using a simple IP messaging client.

> We will be using our custom IP Messaging Server in our examples, you may need to adjust the code for your favorite IP Messaging Server.

```csharp
var encryptedModel = new EncryptedMessageModel
{
    Message = encryptedMessage,
    Sign = sign
};

var encryptedModelJson = JsonConvert.SerializeObject(encryptedModel);
await this.channel.SendMessage(encryptedModelJson);
```

### Step 4. Receive a Message
An encrypted message is received on the recipient’s side using an IP messaging client. 
In order to decrypt and verify the received data, the app on recipient’s side needs to get sender’s Virgil Card from the Keys Service.

```csharp
private async Task OnMessageRecived(string sender, string message)
{
    var encryptedModel = JsonConvert
        .DeserializeObject<EncryptedMessageModel>(message);
    
    var foundCards = await serviceHub.Cards.Search(sender, 
IdentityType.Email);
    var senderCard = foundCards.Single();
    ...
}
```

### Step 5. Verify and Decrypt
The application is making sure the message came from the declared sender by getting his card on Virgil Public Keys Service. In case of success, the message is decrypted using the recipient's private key.

```csharp
var isValid = CryptoHelper.Verify(encryptedModel.EncryptedMessage, 
    encryptedModel.Signature, senderCard.PublicKey.Value);

if (!isValid)
{
    throw new Exception("The message signature is not valid");
}

var decryptedMessage =CryptoHelper.Decrypt(encryptedModel.EncryptedMessage, 
    this.currentMember.CardId.ToString(), this.currentMember.PrivateKey);
```

## Source Code

* [Use Case Example](https://github.com/VirgilSecurity/virgil-sdk-net/tree/master/Examples/Virgil.Examples.IPMessaging)
* [IP-Messaging Simple Server](https://github.com/VirgilSecurity/virgil-sdk-javascript/tree/master/examples/ip-messaging/server)

