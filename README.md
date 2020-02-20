# Sermo Events

Det finns två separata köer där event skickas. En för "Inbound" och en för "Outbound".

* "Inbound" är från vad kunderna gör.
* "Outbound" är från vad boten och agenterna gör.

## Typiskt scenario

### Standardflöde

1. Ett ny slutkund öppnar en chat och direkt så börjar det komma in `heartbeat`-events som visar att klienten är där.
1. När kunden börjar skriva ett meddelande så skickas `typing`-event vilket gör det möljigt för Agent-GUI:et att lyssna in och visa agenten vad kunden skriver i realtid.
1. När kunden skickar sitt första meddelande så kommer det som ett `text`-event med självaste texten.
1. Boten läser meddelandet skickar ett svar i form av ett outbound `text`-event.
1. Om Boten behöver lämna över till en agent så skickas ett `handover_to`-event.
1. Avaya har lyssnat på alla event ovan och kan då visa agenten historiken.
1. När agenten har skrivit ett meddelande så skickar Avaya ett outbound `text`-event.
1. Kunden stänger chattfönstret vilket skickar ett `client_closed`-event.


## "Inbound" (skickat från kunden)

Hur vi ser på eventen som skickas från kunderna.

### text

Vanliga meddelanden från kunden.

```json
{
  "type": "text",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Jag vill ändra faktura 70557076"
}
```

### text (med bild)

Meddelanden från kunden med en bifogad bild. Skickar med ett meddelande också som alltid visas med bilden.

```json
{
  "type": "text",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Bifogad bild:",
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJRgAB+UinhpBuBqJlnCSSdoePgIDo//2Q=="
}
```

### heartbeat

Skickas från kunden för att kunna hålla koll på om kunden har en stabil uppkoppling.

```json
{
  "type": "heartbeat",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
}
```

### messages_read

Skickar en timestamp när kunden senast har läst ett meddelande.

```json
{
  "type": "messages_read",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "read",
  "at": "2020-02-18T12:14:11.386Z"
}
```

### client_left

Kunden har tryckt på krysset i iFramen och bekräftat att den vill lämna chatten.

```json
{
  "type": "client_left",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Kunden har stängt chatten.",
  "at": "2020-02-18T12:12:33.766Z"
}
```

### client_unload

Kunden har stängt ner fliken den har chatten i. Använder https://developer.mozilla.org/en-US/docs/Web/API/Window/unload_event.

```json
{
  "type": "client_unload",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "at": "2020-02-18T12:10:18.968Z"
}
```

### typing

Skickas nuvarande text kunden har skrivit till agenten innan meddelandet har skickats.

```json
{
  "type": "typing",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Hej, jag mitt na",
  "at": "2020-02-18T14:41:37.571Z"
}
```

## "Outbound" (skickat från agenten)

Hur vi ser på eventen som skickas från agenterna.

### queue_number

Skickas när kunden placeras i en kö. Skickas ut ett könummer.

```json
{
  "type": "queue_number",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "queueNumber": 1
}
```

### text

```json
{
  "type": "text",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Jag skickar ett SMS till dig nu med betalningsuppgifterna, det kommer plinga i din telefonen inom en minut.",
  "agent": {
    "id": "anan01",
    "name": "Anders Andersson",
    "type": "human"
  }
}
```

### text (från Boten)

Boten har en simulerad "typing" för att svaret inte ska dyka upp för
snabbt för kunden och det ska se ut som att den skriver.

```json
{
  "type": "text",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Hej!",
  "typing": 1000,
  "agent": {
    "id": "bot",
    "name": "Bot"
  }
}
```

### text (med bild)

```json
{
  "type": "text",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Bifogad bild:",
  "image": "data:image/jpeg;base64,/9j/4AAQSkZJRgAB+UinhpBuBqJlnCSSdoePgIDo//2Q=="
}
```

### system_text

Meddelanden för att kunden ska veta att den blivit tilldelad en agent och kö.

```json
{
  "type": "system_text",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "text": "Chatten har placerats i kön \"Com Hem allmänt\""
}
```

### agent_done

Agenten klickar bort chatten som att den är klar med denna session.

```json
{
  "type": "agent_done",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
}
```

### typing_on

Agenten har börjat att skriva ett meddelande.

```json
{
  "type": "typing_on",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
}
```

### typing_off

Agenten har slutat att skriva ett meddelande. Innebär att de inte har skrivit på några sekunder.

```json
{
  "type": "typing_off",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
}
```

### platform_closed

Skickas när den primära kön för en platform har stängts.

```json
{
  "type": "platform_closed",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
}
```

### handover_to_bot

Sessionen lämnas över till Boten och den kör sina flöden.

```json
{
  "type": "handover_to_bot",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d",
  "flowGroupId": "end"
}
```

### handover_to_agent

Här lämnar boten över till en agent via en kö.

```json
{
  "type": "handover_to_agent",
  "platformId": "comviq-web",
  "userId": "8a8b5c18-c0dd-467b-bd77-7e7685fadf6d"
}
```
