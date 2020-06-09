# Test Cycle API

![Test Cycle](test-cycle.png)

The purpose of this test cycle is to be able to compare different models over time maximizing the revenue.

## Events 

The API is based on events. Following are common attributes for all events.
- ``type`` is the type of event occurred
- ``user_tracking_id`` is a unique id for the web session. If possible it should be the GA id
- ``at`` is the time when the event occurred     

### model-assigned event
This event is sent to assign a model to a web session.  
```json
{
  "type": "model-assigned",
  "user_tracking_id": "GA1.2.1500669880.1586985368",
  "at": "2020-02-18T14:41:37.571Z",
  "model_id": "reference-v16"
}
```

### assistance-triggered event
This event is sent when algorithms trigger to offer an assistance. The attribute ``assisted_offered`` is ``true`` if 
an offer to the user was done and ``false`` if the session was picked for assistance but it was not possible due to
other reasons like no agents available. This enables us to compute the potential of a model even if we have 
temporary restrictions like no available agents. 
```json
{
  "type": "assistance-triggered",
  "user_tracking_id": "GA1.2.1500669880.1586985368",
  "at": "2020-02-18T14:41:37.571Z",
  "assisted_offered": true
}
```

### offer-accepted event
This event is sent when user is accepting assistance. 
```json
{
  "type": "offer-accepted",
  "user_tracking_id": "GA1.2.1500669880.1586985368",  
  "at": "2020-02-18T14:41:37.571Z",
  "user_selection_id": "chat"
}
```

### assistance-started event
This event is sent when the agent started to assist the user. 
```json
{
  "type": "assistance-started",
  "user_tracking_id": "GA1.2.1500669880.1586985368",
  "at": "2020-02-18T14:41:37.571Z"
}
```

### outcome event
This event is sent when the agent started to assist the user. 
```json
{
  "type": "outcome",
  "user_tracking_id": "GA1.2.1500669880.1586985368",  
  "at": "2020-02-18T14:41:37.571Z",
  "outcome_id": "sales_2"
}
```
