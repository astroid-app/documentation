---
icon: message
description: Send messages into a bridge and mark them as read.
---

# Messages

## Send a message

<mark style="color:orange;">`POST`</mark> `/{endpointId}/messages?token=YOUR-TOKEN`

Adds a message to your endpoint's queue. Astroid then sends it to every bridged platform except the one it came from.

```shell
curl --request POST 'https://api.astroid.cc/YOUR-ENDPOINT-ID/messages?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{
    "sender": "nerimity",
    "sender-channel": "1570083353837875201",
    "author": {
      "name": "Astroid Fan",
      "id": "1570083353837875300",
      "avatar": "https://example.com/avatar.png"
    },
    "content": "Hello from the API!"
  }'
```

Response:

```json
{
    "message": "Message added to queue",
    "status": 200,
    "queueLength": 1
}
```

| Status | Response                                                                                         |
| ------ | ------------------------------------------------------------------------------------------------ |
| `200`  | `{"message": "Message added to queue", "status": 200, "queueLength": 1}`                          |
| `400`  | `{"message": "Bad Request: No message provided", "status": 400}`                                  |
| `400`  | `{"message": "Bad Request: Invalid message fields", "status": 400, "details": [...]}`             |
| `401`  | `{"message": "Unauthorized: Invalid token", "status": 401}`                                       |
| `404`  | `{"message": "Endpoint not found", "status": 404}`                                                |

{% hint style="info" %}
`sender-channel` decides where the message goes: Astroid looks up the channel's [position](endpoint-object.md#how-channels-are-paired) and sends the message to the channels at the same position on the other platforms. If the channel isn't registered in your endpoint, the message is not sent.
{% endhint %}

## Message object

Only these top-level keys are allowed. Any other key returns `400`.

| Key              | Type           | Description                                                                 |
| ---------------- | -------------- | --------------------------------------------------------------------------- |
| `sender`         | string         | Platform the message comes from: `discord`, `guilded`, `revolt` or `nerimity`. |
| `sender-channel` | string         | ID of the channel the message was sent in.                                  |
| `author`         | object         | `name`, `id` and `avatar` (URL) of the author.                              |
| `content`        | string         | The message text.                                                           |
| `attachments`    | array          | URLs of attached files.                                                     |
| `embed`          | object         | Optional embed: `title`, `description`, `fields` (array), `footer`, `image`, `thumbnail`. |
| `isReply`        | boolean        | `true` if the message replies to another message.                           |
| `reply`          | object         | The message being replied to: `author` and `message`.                       |

### Full example

```json
{
    "sender": "discord",
    "sender-channel": "1234567890123456789",
    "author": {
        "name": "Astroid Fan",
        "id": "987654321098765432",
        "avatar": "https://cdn.discordapp.com/avatars/..."
    },
    "content": "Check this out!",
    "attachments": ["https://cdn.discordapp.com/attachments/.../image.png"],
    "embed": {
        "title": "Title",
        "description": "Description",
        "fields": [],
        "footer": "Footer",
        "image": "None",
        "thumbnail": "None"
    },
    "isReply": true,
    "reply": {
        "author": "Someone",
        "message": "The original message"
    }
}
```

## Mark a message as read

<mark style="color:orange;">`POST`</mark> `/read/{endpointId}?token=YOUR-TOKEN`

Marks the last message as delivered for one or more platforms. Only needed if you run your own bot that picks up messages from Astroid.

Body: set the platforms you want to mark to `true`.

```json
{
    "read_discord": false,
    "read_guilded": false,
    "read_revolt": false,
    "read_nerimity": true
}
```

The camelCase keys (`readDiscord`, `readGuilded`, `readRevolt`, `readNerimity`) work too.

```json
{
    "message": "Success."
}
```
