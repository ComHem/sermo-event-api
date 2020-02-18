# Sermo Events

## "Inbound" (skickat från kunden)

Hur vi ser på eventen som skickas från kunderna.

### text

Vanliga meddelanden från kunden.

```json
{
  "type": "text",
  "platform": "webchat",
  "text": "Jag vill ändra faktura 70557076",
  "user": {
    "id": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
  },
  "raw": {
    "text": "Jag vill ändra faktura 70557076",
    "type": "text",
    "platformId": "webchat",
    "conversationId": 862
  }
}
```

### text (med bild)

Meddelanden från kunden med en bifogad bild. Skickar med ett meddelande också som alltid visas med bilden.

```json
{
  "type": "text",
  "platform": "webchat",
  "text": "Bifogad bild:",
  "user": {
    "id": "470e84d7-abcf-4360-8f3b-c39cd50eeb3c"
  },
  "raw": {
    "text": "Bifogad bild:",
    "type": "text",
    "data": {
      "image": "data:image/jpeg;base64,/9j/4AAQSkZJRgAB+UinhpBuBqJlnCSSdoePgIDo//2Q=="
    },
    "platformId": "webchat",
    "conversationId": 846
  }
}
```

### heartbeat

Skickas från kunden för att kunna hålla koll på om kunden har en stabil uppkoppling.

```json
{
  "type": "heartbeat",
  "platform": "webchat",
  "text": "beat",
  "user": {
    "id": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
  },
  "raw": {}
}
```

### message_read

Skickar en timestamp när kunden senast har läst ett meddelande.

```json
{
  "type": "messages_read",
  "platform": "webchat",
  "text": "read",
  "user": {
    "id": "38d93922-77c9-4f47-9ce2-0df72a728fa9"
  },
  "raw": {
    "to": "38d93922-77c9-4f47-9ce2-0df72a728fa9",
    "at": "2020-02-18T12:14:11.386Z"
  }
}
```

### client_left

Kunden har tryckt på krysset i iFramen och bekräftat att den vill lämna chatten.

```json
{
  "type": "client_left",
  "platform": "webchat",
  "text": "Kunden har stängt chatten.",
  "user": {
    "id": "f9653bf3-0c6c-4d22-9547-7cd00382c775"
  },
  "raw": {
    "to": "f9653bf3-0c6c-4d22-9547-7cd00382c775",
    "at": "2020-02-18T12:12:33.766Z"
  }
}
```

### client_unload

Kunden har stängt ner fliken den har chatten i. Använder https://developer.mozilla.org/en-US/docs/Web/API/Window/unload_event.

```json
{
  "type": "client_unload",
  "platform": "webchat",
  "text": "NO_MESSAGE",
  "user": {
    "id": "0a1f5b59-8d7e-4649-87ef-274bda362c56"
  },
  "raw": {
    "to": "0a1f5b59-8d7e-4649-87ef-274bda362c56",
    "at": "2020-02-18T12:10:18.968Z"
  }
}
```

## "Outbound" (skickat från agenten)

Hur vi ser på eventen som skickas från agenterna.

### queue_number

Skickas när kunden placeras i en kö. Skickas ut ett könummer.

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

### text (med bild)

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
      "image": "data:image/jpeg;base64,/9j/4AAQSkZJRgAB+UinhpBuBqJlnCSSdoePgIDo//2Q=="
    },
    "text": "Bifogad bild:"
  }
}
```

### agent_done

Agenten klickar bort chatten som att den är klar med denna session.

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

### typing_on

Agenten har börjat att skriva ett meddelande.

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

### typing_off

Agenten har slutat att skriva ett meddelande. Innebär att de inte har skrivit något på cirka 20s.

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

### platform_closed

Skickas när den primära kön för en platform har stängts.

```json
{
  "type": "platform_closed",
  "platform": "webchat",
  "text": "",
  "user": {
    "id": "c2e24ee0-d503-4177-955c-eb89508caa63"
  },
  "raw": {}
}
```

### heartbeat

Skickas ej som ett event genom vår Broker från agenter. Den logiken är i vår Agent-tjänst.

### agent_handover

Skickas ej som ett event genom vår Broker. Görs ett direkt REST-anrop mellan tjänsterna nu.
