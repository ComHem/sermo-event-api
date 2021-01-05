
# Midas JavaScript Bundle

The Midas JavaScript bundle provides functionality to record customer navigation and actions on the site in question. This information can then be used by Sermo to provide customer service or sales agents with more information regarding what the customer is doing on the site or what he/she might be interested in.

It also listens for incoming messages through `window.postMessage` (https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) for specific customer actions that is not recorded through user navigation.

The bundle will listen for messages with the receiver "sermo".

Two keys will be saved as client cookies by the bundle to keep track of what user and what session is currently ongoing. One short cookie with a 1 hour expiry that identifies the session and one cookie with a duration 365 days that identifies the user across multiple sessions. 

## TLDR

1. Include [midas.ci1.js](https://sermo-midas-ci1.sermo.dev-dockeree.int.comhem.com/bundle/midas.ci1.js) (dev) or [midas.min.js](https://sermo-midas.comhem.com/bundle/midas.min.js) (prod) in the head-block of your site.
1. Open up your site in any browser. 
1. Navigate to a new page on your site.
1. A track-session event should have been sent. Check the network log in your browser.

## Manually Triggered Events

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