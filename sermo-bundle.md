# Sermo JavaScript Bundle

The Sermo JavaScript bundle provides a straight forward way to interface with Sermo Webchat. Communication between your website and Sermo is done through `window.postMessage` (https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). The bundle is minified and manages the lifecycle of the Sermo Webchat window through an insterted DOM-element in the body-tag.

The bundle will listen for messages with the receiver "sermo" and open an iFrame for the customer to `https://sermo.comhem.com`.

A key will be saved in the Session Storage by the bundle to keep track of the state of the chat.
Cookies will be saved for the domain `https://sermo.comhem.com` so that Sermo can identify the customer.

## TLDR

1. Include [sermo.ci1.js](https://sermo-webchat-ci1.sermo.dev-dockeree.int.comhem.com/bundle/sermo.ci1.js) (dev) or [sermo.min.js](https://sermo.comhem.com/bundle/sermo.min.js) (prod).
1. Post a START_CHAT event. 
```js
window.postMessage({ 
  sender: 'comhem/comviq/boxer', 
  receiver: 'sermo', 
  event: 'START_CHAT', 
  platformId: '<platform>'
});
```
3. Chat should open. If not - check error logs. If that doesn't help, keep reading.

## Setup

The script is hosted on webchat and should be included within the `<head>` tag of your website. For development purposes, use the `dev` bundle as specified below.

| env  | host |
| -    | -    |
| dev  | https://sermo-webchat-ci1.sermo.dev-dockeree.int.comhem.com/bundle/sermo.ci1.js |
| prod | https://sermo.comhem.com/bundle/sermo.min.js |

See [sermo-example.html](./sermo-example.html) for a working minimal example site using the development bundle.

## Brands

Sermo is used by multiple brands. Currently, these brands are supported:

- `comhem`
- `comviq`
- `comhemplay`

The chat window appearance is based on brand and/or platform.

## Platforms

Sermo supports multiple platforms or chat _channels_ for each brand. For example, comhem has a support platform, a sales platform and a save platform. The platform indicates to Sermo _how_ the customer entered the chat.

Ask the Sermo team which platforms are setup for your brand.

Before opening a chat on a specific platform, make sure it is open and has available agents. This information is available on the `/platforms/<PLATFORM>` endpoint. Below is the endpoint for the platform `webchat` on the development and production environments.

| env  | host |
| -    | -    |
| dev  | https://sermo-webchat-ci1.sermo.dev-dockeree.int.comhem.com/api/platforms/webchat |
| prod | https://sermo.comhem.com/api/platforms/webchat |

The response will look like this if the platform is available

```json
{
  "queueId": "ch",
  "available": true,
  "closed": false,
  "hasWorkingAgents": true
}
```

It could look like this when the platform is _closed_

```json
{
  "queueId": "ch",
  "available": false,
  "closed": true
}
```

- `queueId` - Not applicable here. This is the name of the queue that sermo will place the customer in.
- `available` - Specifies if there are enough agents to take more chats. This will be set to `false` if the queue is too long. No new conversations should be started if this value is `false`.
- `closed` - Specifies if the platform is closed. Sermo will not accept any new conversations if this value is `true`.
- `hasWorkingAgents` - Specifies if there are any agents working to take conversations from this platform.

## Start chat

Send a message using `window.postMessage`, replace `<BRAND>` and `<PLATFORM>` with your specific values.

```js
window.postMessage({
  sender: '<BRAND>',
  receiver: 'sermo',
  event: 'START_CHAT',
  platformId: '<PLATFORM>'
});
```

### Options

In addition to the required properties above, it is also possible to include

- `zIndex` - set to a specific value to determine which zIndex the chat window will be placed at. Defaults to 10000. 
- `customerId` - if the customer is logged in, provide their customerId to inform the customer support agent who it is.


## Common errors

Below are some common sources of errors if the chat window fails to appear. Remember to check the console for any errors.

### ERR_CERT_AUTHORITY_INVALID / Invalid cert - "Your connection is not private"

The development environment does not have a valid cert. It needs to be explicitly trusted. Otherwise it will not download the script from the development environment.

If your browser prints an ERR_CERT_AUTHORITY_INVALID error, make sure your browser can download and show the script on this URL without errors: https://sermo-webchat-ci1.sermo.dev-dockeree.int.comhem.com/bundle/sermo.ci1.js. If you get an error or warning saying "Your connection is not private", follow your browsers instructions to proceed. In chrome, click "Advanced" then "proceed to ... (unsafe)"

### Invalid platform

If you see an error similar to this: 

```
GET https://sermo-webchat-ci1.sermo.dev-dockeree.int.comhem.com/api/platforms/webbchatt 404 (Not Found)
```

It means that the specified platform does not exist in Sermo. Check the spelling and try again. In this case it should be `webbchatt -> webchat`.

## Additional events 

### The customer places an order

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
