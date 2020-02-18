# Sermo Events

## "Inbound" (skickat från kunden)

Hur vi ser på eventen som skickas från kunderna.

### text

```json
{
  "type": "text",
  "platform": "webchat",
  "user": {
    "id": "aed0bbd9-545f-4f88-bc14-3a62cf206477",
    "userId": "aed0bbd9-545f-4f88-bc14-3a62cf206477",
    "platform": "webchat",
    "gender": "unknown",
    "timezone": null,
    "picture_url": null,
    "first_name": null,
    "last_name": null,
    "locale": null,
    "created_on": "2020-02-18T10:26:57.583Z"
  },
  "text": "client 5 Jag vill ändra faktura 70557076",
  "raw": {
    "text": "client 5 Jag vill ändra faktura 70557076",
    "type": "text",
    "platformId": "webchat",
    "conversationId": 826
  }
}
```

### text (with image)

```json
{
  "type": "text",
  "platform": "webchat",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "userId": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "platform": "webchat",
    "gender": "unknown",
    "timezone": null,
    "picture_url": null,
    "first_name": null,
    "last_name": null,
    "locale": null,
    "created_on": "2020-02-18T10:27:20.021Z"
  },
  "text": "Bifogad bild:",
  "raw": {
    "text": "Bifogad bild:",
    "type": "text",
    "data": {
      "image": "data:image/jpeg;base64AAA=="
    }
  }
}
```

### heartbeat

```json
{
  "platform": "webchat",
  "type": "heartbeat",
  "text": "beat",
  "user": {
    "id": "78b48070-1069-4e16-b432-acfc5debb411",
    "userId": "78b48070-1069-4e16-b432-acfc5debb411",
    "platform": "webchat",
    "gender": "unknown",
    "timezone": null,
    "picture_url": null,
    "first_name": null,
    "last_name": null,
    "locale": null,
    "created_on": "2020-02-18T10:26:56.031Z"
  },
  "raw": {}
}
```

### message read

```json
{
  "type": "messages_read",
  "platform": "webchat",
  "text": "read",
  "user": {
    "id": "78b48070-1069-4e16-b432-acfc5debb411",
    "userId": "78b48070-1069-4e16-b432-acfc5debb411",
    "platform": "webchat",
    "gender": "unknown",
    "timezone": null,
    "picture_url": null,
    "first_name": null,
    "last_name": null,
    "locale": null,
    "created_on": "2020-02-18T10:26:56.031Z"
  },
  "raw": {
    "to": "78b48070-1069-4e16-b432-acfc5debb411",
    "at": "2020-02-18T10:27:06.060Z"
  }
}
```

### client_left

```json
{
  "type": "client_left",
  "platform": "webchat",
  "text": "Kunden har stängt chatten.",
  "user": {
    "id": "fe5b4dae-7f97-4d8a-90b6-b1f7b1fa067c",
    "userId": "fe5b4dae-7f97-4d8a-90b6-b1f7b1fa067c",
    "platform": "webchat",
    "gender": "unknown",
    "timezone": null,
    "picture_url": null,
    "first_name": null,
    "last_name": null,
    "locale": null,
    "created_on": "2020-02-18T10:25:53.559Z"
  },
  "raw": {
    "to": "fe5b4dae-7f97-4d8a-90b6-b1f7b1fa067c",
    "at": "2020-02-18T10:25:58.120Z"
  }
}
```

### client_unload

```json
{
  "type": "client_unload",
  "platform": "webchat",
  "text": "NO_MESSAGE",
  "user": {
    "id": "fcd8237b-f29a-44aa-b4e8-f439653680ea",
    "userId": "fcd8237b-f29a-44aa-b4e8-f439653680ea",
    "platform": "webchat",
    "gender": "unknown",
    "timezone": null,
    "picture_url": null,
    "first_name": null,
    "last_name": null,
    "locale": null,
    "created_on": "2020-02-18T10:26:11.808Z"
  },
  "raw": {
    "to": "fcd8237b-f29a-44aa-b4e8-f439653680ea",
    "at": "2020-02-18T10:26:15.581Z"
  }
}
```

## "Outbound" (skickat från agenten)

Hur vi ser på eventen som skickas från agenterna.

### queue number

```json
{
  "platform": "webchat",
  "type": "queue_number",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43"
  },
  "text": "1",
  "raw": {}
}
```

### text

```json
{
  "type": "text",
  "platform": "webchat",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43"
  },
  "raw": {
    "to": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "message": "Jag skickar ett SMS till dig nu med betalningsuppgifterna, det kommer plinga i din telefonen inom en minut.",
    "data": {},
    "agent": {
      "id": "alote1",
      "name": "alote1",
      "type": "human"
    }
  },
  "data": {},
  "text": "Jag skickar ett SMS till dig nu med betalningsuppgifterna, det kommer plinga i din telefonen inom en minut."
}
```

### text (från Boten)

```json
{
  "raw": {
    "to": "10e4d33c-19e8-4680-b764-ff69a18a3909",
    "agent": {
      "id": "bot",
      "name": "Bot"
    },
    "message": "Hej!",
    "typing": 1000
  },
  "type": "text",
  "user": {
    "id": "10e4d33c-19e8-4680-b764-ff69a18a3909"
  },
  "text": "Hej!",
  "platform": "webchat"
}
```

### text (systemmeddelanden)

```json
{
  "type": "text",
  "platform": "webchat",
  "text": "Chatten har placerats i kön \"Com Hem allmänt\"",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43"
  },
  "raw": {
    "to": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "agent": {
      "id": "system",
      "name": "Info"
    },
    "message": "Chatten har placerats i kön \"Com Hem allmänt\""
  }
}
```

### text (with image)

```json
{
  "type": "text",
  "platform": "webchat",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43"
  },
  "raw": {
    "to": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "message": "Bifogad bild:",
    "data": {
      "image": "data:image/jpeg;base64=="
    },
    "text": "Bifogad bild:"
  }
}
```

### agent done

```json
{
  "type": "agent_done",
  "platform": "webchat",
  "user": {
    "id": "bba79883-31e5-42f1-979f-0697184ddd73"
  },
  "raw": {
    "to": "bba79883-31e5-42f1-979f-0697184ddd73",
    "agent": {
      "id": "system",
      "name": "Info"
    },
    "message": "Agenten är inte längre aktiv i chatten. Om något är oklart och du behöver komma i kontakt med oss igen, skriv ytterligare ett meddelande här, så tar vi vid där vi slutade."
  },
  "text": "Agenten är inte längre aktiv i chatten. Om något är oklart och du behöver komma i kontakt med oss igen, skriv ytterligare ett meddelande här, så tar vi vid där vi slutade."
}
```


### typing on (agenten börjar skriva ett meddelande)

```json
{
  "type": "typing_on",
  "platform": "webchat",
  "text": "sdf",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43"
  },
  "raw": {
    "to": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "agent": {
      "name": ""
    }
  }
}
```

### typing off (agenten har slutat skriva ett meddelande)

```json
{
  "type": "typing_off",
  "platform": "webchat",
  "text": "sdf",
  "user": {
    "id": "79d6dc18-c2c8-4377-b01f-96d21d619b43"
  },
  "raw": {
    "to": "79d6dc18-c2c8-4377-b01f-96d21d619b43",
    "agent": {
      "name": ""
    }
  }
}
```

### Heartbeat

Skickas ej som ett event genom vår Broker från agenter. Den logiken är i vår Agent-tjänst.

### Agent handover

Skickas ej som ett event genom vår Broker. Görs ett direkt REST-anrop mellan tjänsterna nu.
