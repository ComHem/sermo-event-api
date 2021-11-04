# Sermo JavaScript Bundle

The Sermo JavaScript bundle provides a straight forward way to interface with Sermo Webchat. Communication between your website and Sermo is done through `window.postMessage` (https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). The bundle is minified and manages the lifecycle of the Sermo Webchat window through an insterted DOM-element in the body-tag.

The bundle will listen for messages with the receiver "sermo" and open an iFrame for the customer to `https://sermo.comhem.com`.

A key will be saved in the Session Storage by the bundle to keep track of the state of the chat.
Cookies will be saved for the domain `https://sermo.comhem.com` so that Sermo can identify the customer.

## TLDR

1. Include [sermo.ci1.js](https://sermo-webchat-ci1.dev-dockeree.int.comhem.com/bundle/sermo.ci1.js) (dev) or [sermo.min.js](https://sermo.comhem.com/bundle/sermo.min.js) (prod).
1. Post a START_CHAT event.

```js
window.postMessage({
  sender: "comhem/comviq/boxer",
  receiver: "sermo",
  event: "START_CHAT",
  platformId: "<platform>",
});
```

3. Chat should open. If not - check error logs. If that doesn't help, keep reading.

## Setup

The script is hosted on webchat and should be included within the `<head>` tag of your website. For development purposes, use the `dev` bundle as specified below.

| env  | host                                                                      |
| ---- | ------------------------------------------------------------------------- |
| dev  | https://sermo-webchat-ci1.dev-dockeree.int.comhem.com/bundle/sermo.ci1.js |
| prod | https://sermo.comhem.com/bundle/sermo.min.js                              |

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

Before opening a chat on a specific platform, make sure it is open and has available agents. The bundle exposes a `SERMO` global variable that emits platform availability information. For example:

```js
window.SERMO.setAvailabilityListener(
  "webchat",
  (err, info) => {
    // err is either null or an instance of Error
    if (err) {
      // err indicates there was a failure to fetch the availability status
      // in this case, `info` will be null so we can only acknowledge the error and return
      console.error(err.message);
      return;
    }

    if (info.open) {
      // is within opening hours 
      if (info.available) {
        // there are also available agents ready to chat
      } else {
        // within opening hours but no available agents
      }
    } else {
      // chat is not available
    }
  },
  { pollingInterval: 2000 }
);

// window.SERMO.clearAvailabilityListener("webchat") to remove the listener
```

This will start a polling loop to the public sermo-api at `/api/platforms/<PLATFORM>` and emit the response periodically. You can also check the endpoint directly at the following URL:

| env  | host                                                                       |
| ---- | -------------------------------------------------------------------------- |
| dev  | https://sermo-api-ci1.dev-dockeree.int.comhem.com/api/platforms/comhemplay |
| prod | https://sermo-api.comhem.com/api/platforms/comhemplay                      |

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
  sender: "<BRAND>",
  receiver: "sermo",
  event: "START_CHAT",
  platformId: "<PLATFORM>",
});
```

### Options

In addition to the required properties above, it is also possible to include

- `zIndex` - set to a specific value to determine which zIndex the chat window will be placed at. Defaults to 10000.
- `customerId` - if the customer is logged in, provide their customerId to inform the customer support agent who it is.

## Common errors

Below are some common sources of errors if the chat window fails to appear. Remember to check the console for any errors.

### Invalid platform

If you see an error similar to this:

```
GET https://sermo-api-ci1.dev-dockeree.int.comhem.com/api/platforms/webbchatt 404 (Not Found)
```

It means that the specified platform does not exist in Sermo. Check the spelling and try again. In this case it should be `webbchatt -> webchat`.

## NowInteract

The script also integrates with NowInteract using `window.postMessage`.

### Incoming messages

Incoming messages from NowInteract are used to open chat windows.

```json
{
  "sender": "nowinteract",
  "receiver": "comhem",
  "event": "start_sales_chat",
  "platformId": "<PLATFORM>",
  "nowinteract_visitorid": "<NI_VISITOR_ID>",
  "nowinteract_leadid": "<NI_LEAD_ID>"
}
```

`PLATFORM` is an optional property and can be used to explicitly specify which platform to use. For example, this can be used to route some customers to the bot, and others straight to a human agent.

`NI_VISITOR_ID` specifies the NowInteract visitor or session id.

`NI_LEAD_ID` specifies the NowInteract lead id. Also known as _assistance id_.

### Outgoing messages

Outgoing messages are used to notify NowInteract about the users interaction with the chat window.

```json
{
  "sender": "comhem",
  "receiver": "nowinteract",
  "eventName": "<EVENT>"
}
```

Where `eventName` is one of the following

- `chat_window_opened` - when sermo has succesfully opened the chat window.
- `chat_conversation_ended` - when the chat window has been closed.
- `chat_window_minimized` - when the chat window is minimized.
- `chat_window_maximized` - when the chat window is maximized.
- `chat_conversation_started` - when the customer sends their first message through the chat window.
