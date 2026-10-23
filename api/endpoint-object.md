---
icon: brackets-curly
description: The structure of an endpoint, as returned by GET /{endpointId}.
---

# Endpoint object

An endpoint has two parts:

* `config`: your settings. You change these with [`/{endpointId}/config`](endpoints.md).
* `meta`: Astroid's internal state (the last message, read status, message queue). You normally don't touch it.

## Full example

This is what a freshly registered endpoint looks like, with one Discord channel bridged:

```json
{
    "config": {
        "allowed-ids": [],
        "blacklist": [],
        "channels": {
            "discord": ["1234567890123456789"],
            "guilded": [],
            "nerimity": [],
            "revolt": []
        },
        "emoji_filtering": false,
        "emojis": [],
        "isbeta": false,
        "logs": {
            "discord": "",
            "guilded": null,
            "nerimity": null,
            "revolt": null
        },
        "self-user": false,
        "webhooks": {
            "discord": ["https://discord.com/api/webhooks/..."],
            "guilded": [],
            "nerimity": [],
            "revolt": []
        }
    },
    "meta": {
        "_message_cache": [],
        "message": {
            "attachments": [],
            "author": { "avatar": null, "id": null, "name": null },
            "content": null,
            "embed": {
                "description": "None",
                "fields": [],
                "footer": "None",
                "image": "None",
                "thumbnail": "None",
                "title": "None"
            },
            "isReply": false,
            "reply": { "author": null, "message": null },
            "sender": null,
            "sender-channel": null
        },
        "read": { "discord": false, "guilded": false, "nerimity": false, "revolt": false },
        "sender": null,
        "sender-channel": null,
        "trigger": false
    }
}
```

## `config`

| Key               | Type     | Description                                                                                       |
| ----------------- | -------- | ------------------------------------------------------------------------------------------------- |
| `allowed-ids`     | string[] | IDs of bots or webhooks whose messages are still bridged. By default, bot messages are ignored.   |
| `blacklist`       | string[] | Words that stop a message from being bridged.                                                     |
| `channels`        | object   | Bridged channel IDs per platform. See [How channels are paired](#how-channels-are-paired).        |
| `emoji_filtering` | boolean  | If `true`, custom emojis without a [mapping](emojis.md) are removed instead of sent as text.      |
| `emojis`          | object[] | Emoji mappings between platforms. See [Emojis](emojis.md).                                        |
| `isbeta`          | boolean  | Whether this endpoint uses the Astroid beta bots.                                                 |
| `logs`            | object   | Log channel ID per platform.                                                                      |
| `self-user`       | boolean  | Whether messages Astroid itself sent are bridged again.                                           |
| `webhooks`        | object   | Webhook URLs per platform, used to post bridged messages.                                         |

`channels`, `logs` and `webhooks` each have one key per platform: `discord`, `guilded`, `nerimity` and `revolt`.

### How channels are paired

Channels are paired **by their position** in the lists. The first Discord channel is bridged with the first Guilded, Stoat and Nerimity channel, the second with the second, and so on:

```json
"channels": {
    "discord":  ["111", "222"],
    "nerimity": ["aaa", "bbb"]
}
```

Here `111` ⇄ `aaa` and `222` ⇄ `bbb`.

This is why you should always run <mark style="color:purple;">`add-bridge`</mark> on every platform in the same order. The `webhooks` lists use the same positions as `channels`.

## `meta`

| Key              | Description                                                                   |
| ---------------- | ----------------------------------------------------------------------------- |
| `_message_cache` | The queue of messages waiting to be sent. Filled by [`POST /{endpointId}/messages`](messages.md). |
| `message`        | The last message that was bridged. Same shape as the [message object](messages.md#message-object). |
| `read`           | Per platform: whether that platform's bot has already sent the last message.  |
| `sender`         | Platform the last message came from.                                          |
| `sender-channel` | Channel the last message came from.                                           |
| `trigger`        | Internal flag. Ignore it.                                                     |
