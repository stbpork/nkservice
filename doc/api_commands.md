# NkSERVICE External API

## Core Commands

The currently supported External API commands as described here. See [External API](api_intro.md) for an introducition to the External API. See the documentation of each plugin to learn the supported classes and commands.

Cmd|Description
---|---
[`subscribe`](#subscribe)|Subscribe to events
[`unsubscribe`](#unsubscribe)|Remove a previously registered subscription
[`send_event`](#send-event)|Fires an event


### Subscribe

Allows the connection to subscribe to specific events or event classes. The only mandatory field is `class`. 

See each plugin documentation to learn the supported event subclasses and types of each supported class.

Field|Default|Description
---|---|---|---
class|(mandatory)|Class to subscribe to
subclass|`"*"`|Subscribe only to this subclass (`"*"` to subscribe to all)
type|`"*"`|Only to this event type (`"*"` to subscribe to all)
obj_id|`"*"`|Only to events generated by this specific object (`"*"` to all)
body|`{}`|Body to merge to incoming message's body
service|(this service)|Subscribe to events generated at a different _service_

The subscription attemp must be authorized by the server side code. If the connection is dropped, all subscriptions are deleted.

Fired events can have a body. If both the event's body and the subscription body are objects, they will be merged in the incoming event.

**Sample**

```js
{
	class: "core",
	cmd: "subscribe",
	data: {
		class: "media",
		subclass: "session"
		body: {
			my_key: "my value"
		}
	}
	tid: 1
}
```

This example would subscribe the connection to all specific sessions and event types for sessions beloging to the `media` class. All events sent from class `media`, generated at object `session` (with any `type` and `obj_id`) will be sent to this connection.

Fields omitted or with value `"*"` will match all possible values for that field, including the case were the sender didn't use that field.


### Unsubscribe

Removes a previously registered subscription. Must match exactly the same fields used for the subscription (except `body`).


**Sample**

```js
{
	class: "core",
	cmd: "unsubscribe",
	data: {
		class: "media",
		subclass: "session"
	},
	tid: 1
}
```


### Send event

Allows to fire an event. 

Field|Default|Description
---|---|---|---
class|(mandatory)|Class to send the even to
subclass|`"*"`|Subclass to send the event to
type|`"*"`|Event type to send
obj_id|`"*"`|Specific object id
body|`{}`|Body to include in the message
service|(this service)|Send the event to a different _service_

Fields omited or with value "`*`" will be omitted in the event. Only clients subscribed to the value `"*"` for that field (or that omitted them in the subscription request) will receive the message


**Sample**

```js
{
	class: "core",
	cmd: "send_event",
	data: {
		class: "my_class",
		subclass: "my_subclass"
		body: {
			my_key: "my value"
		}
	}
	tid: 1
}
```





