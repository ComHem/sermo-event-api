# Sermo JavaScript Bundle

The Sermo JavaScript bundle can be included on a website and can be interfaced with `window.postMessage` (https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage).
It's a minified bundle that handles the lifecycle of the Sermo chat window.

## Setup

Include a script that with any of these scripts within the head block of your site.

| env  | host |
| -    | -    |
| dev  | https://sermo-webchat-ci1.sermo.dev-dockeree.int.comhem.com/bundle/sermo.ci1.js |
| prod | https://sermo.comhem.com/bundle/sermo.min.js |

All interactions with the sermo bundle is done through window.postMessage. The sender of each message should be the brand of the site that is sending the message.

Available brands are

- comhem
- comviq
- boxer

## Starting chats

To start a chat a message is sent through `window.postMessage` with a payload that corresponds to the type of chat you want to start.

### Common

The common parts for all events are:

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "...",
  "customerId": "12345678",
  "zIndex": 1501
  ...
}
```

Where the `WEBSITE_BRAND` string should be the brand of the website.

The value of `customerId` should be provided if the customer is logged in on the site. Doing so enables the bot to not ask for it and only request the intent of the customer. This is an optional field.

The value for `zIndex` is used so that we can make sure that the CSS `z-index` we open the iFrame with is the highest on the page. If is this value is not provided it will be defaulted to 10000

### How to start a support chat

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "START_SUPPORT_CHAT",
  "customerId": "12345678",
  "zIndex": 1501
}
```

### How to start a sales chat

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "START_SALES_CHAT",
  "customerId": "12345678",
  "niSalesLeadId": "lead-123",
  "niSessionId": "session-123",
  "zIndex": 1501
}
```

The value for `niSalesLeadId` should be the ID of the lead that NowInteract provides to the website.

The value for `niSessionId` should be the ID of the session that NowInteract provides to the website.

### How to start a save chat

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "START_SAVE_CHAT",
  "customerId": "12345678",
  "zIndex": 1501
}
```

### How to start a chat on a specific platform

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "START_CHAT",
  "customerId": "12345678",
  "zIndex": 1501,
  "platformId": "PLATFORM"
}
```

Where platformId is the specific platform you would like to open a chat on.

## What the Sermo Bundle will do

The bundle will listen for messages with the receiver "sermo" and open an iFrame for the customer to `https://sermo.comhem.com`.

A key will be saved in the Session Storage by the bundle to keep track of the state of the chat.
Cookies will be saved for the domain `https://sermo.comhem.com` so that Sermo can identify the customer.

There are also certain events that the bundle will emit for that can be listened to if needed.

### Events published by the sermo-bundle

#### The chat conversation with the customer has started

A chat conversation is seen as started when the customer has sent their first message to the bot/agent.

A message will be sent through `window.postMessage` with the following payload when that occurs:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_CONVERSATION_STARTED"
}
```

#### The chat is closed by the customer

When the chat is closed the bundle will clean up the iFrame by itself.

A message will also be sent through `window.postMessage` with this format:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_CLOSED"
}
```

#### The chat is minimized by the customer

A message will be sent through `window.postMessage` when the Sermo chat window is minimized:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_MINIMIZED"
}
```

#### The chat is maximized by the customer

A message will be sent through `window.postMessage` when the Sermo chat window is maximized again:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_MAXIMIZED"
}
```

### Additional events that the Sermo Bundle will listen for

In addition to the event to start a chat the Sermo bundle will listen for more events.
Customer identification and order placement will be used to connect chat sessions to what the customer does on the website during the chat session and after.


#### The user logs in

Send a message through `window.postMessage` with a payload of this format:

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "CUSTOMER_LOGGED_IN",
  "customerId": "12345678"
}
```

#### The user logs out

Send a message through `window.postMessage` with a payload of this format:

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "CUSTOMER_LOGGED_OUT",
}
```

#### The customer places an order

This event is specific for the comhem brand. Not usable on other brands.

Send a message through `window.postMessage` with a payload of this format:

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "ORDER_PLACED",
  "customerId": "12345678",
  "ssn": "190001011234",
  "purchaseId": "499d2848150",
  "products": [
    {
      "id": "BBG062",
      "name": "Bredband 300",
      "category": "Broadband - Base subscription - New customer",
      "price": 5988,
      "quantity": 1
    },
    {
      "id": "3101",
      "name": "Wi-Fi Hub",
      "category": "Broadband - Equipment - New customer",
      "price": 0,
      "quantity": 1
    }
  ]
}
```

