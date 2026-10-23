---
description: You can update values of the data stored in the endpoint config or metadata.
---

# /update/{endpoint}

### Path parameters

endpoint: endpoint: The ID of your endpoint (usually your Discord server id)

### Query parameters

<table><thead><tr><th>query name</th><th width="117">type</th><th width="188">min-max length</th><th>description</th></tr></thead><tbody><tr><td>index</td><td>integer</td><td>-</td><td>Index of the value in a list to edit (will overwrite the index' value)</td></tr><tr><td>webhook_discord</td><td>string</td><td> 50 .. 350 characters</td><td>Discord Webhook URL</td></tr><tr><td>webhook_guilded</td><td>string</td><td>50 .. 350 characters</td><td>Guilded Webhook URL</td></tr><tr><td>webhook_revolt</td><td>string</td><td>50 .. 350 characters</td><td>Revolt Webhook URL (not supported by Revolt)</td></tr><tr><td>webhook_nerimity</td><td>string</td><td>50 .. 350 characters</td><td>Nerimity Webhook URL (not supported by Nerimity)</td></tr><tr><td>log_discord</td><td>string</td><td>-</td><td>Discord Log Channel ID</td></tr><tr><td>log_guilded</td><td>string</td><td>5 .. 50 characters</td><td>Guilded Log Channel ID</td></tr><tr><td>log_revolt</td><td>string</td><td>5 .. 50 characters</td><td>Revolt Log Channel ID</td></tr><tr><td>log_nerimity</td><td>string</td><td>5 .. 50 characters</td><td>Nerimity Log Channel ID</td></tr><tr><td>channel_discord</td><td>string</td><td>-</td><td>Discord Channel ID</td></tr><tr><td>channel_guilded</td><td>string</td><td>5 .. 150 characters</td><td>Guilded Channel ID</td></tr><tr><td>channel_revolt</td><td>string</td><td>5 .. 50 characters</td><td>Revolt Channel ID</td></tr><tr><td>channel_nerimity</td><td>string</td><td>5 .. 50 characters</td><td>Nerimity Channel ID</td></tr><tr><td>blacklist</td><td>string</td><td>1 .. 250 characters</td><td>Word appending to the Blacklist</td></tr><tr><td>sender_channel</td><td>string</td><td>10 .. 80 characters</td><td>Sender Channel ID</td></tr><tr><td>trigger</td><td>boolean</td><td>-</td><td>Value if a new message exists</td></tr><tr><td>sender</td><td>string</td><td>5 .. 10 characters</td><td>Sender platform</td></tr><tr><td>message_author_name</td><td>string</td><td>1 .. 50 characters</td><td>Message author name</td></tr><tr><td>message_author_avatar</td><td>string</td><td>50 .. 250 characters</td><td>Message author avatar as URL</td></tr><tr><td>allowed_ids</td><td>string</td><td>5 .. 50 characters</td><td>An ID appending to the allowed ids list</td></tr><tr><td>message_reply</td><td>boolean<br>Default: false</td><td>-</td><td>If the message if a reply</td></tr><tr><td>message_reply_message</td><td>string </td><td>&#x3C;= 1500 characters</td><td>The content of the message that got replied to </td></tr><tr><td>message_reply_author</td><td>string</td><td>5 .. 50 characters</td><td>The author avatar as URL of the message that got replied ti</td></tr><tr><td>message_author_id</td><td>string</td><td>5 .. 50 characters</td><td>Message author ID</td></tr><tr><td>message_content</td><td>string</td><td>&#x3C;= 1500 characters</td><td>Message content</td></tr><tr><td>message_attachments</td><td>string</td><td>20 .. 1550 characters</td><td>URL of the attachment of the message</td></tr><tr><td>message_embed</td><td>string (json formatted)</td><td>&#x3C;= 1500 characters</td><td>JSON object as string of the messages embed</td></tr><tr><td>selfuse</td><td>boolean<br>Default: false</td><td>-</td><td>Value of the API should bridge messages -> See selfuse <mark style="color:red;">[ADD LINK]</mark></td></tr><tr><td>token</td><td>string</td><td>71 .. 85 characters</td><td>Astroid endpoint token</td></tr><tr><td>beta</td><td>boolean <br>Default: false</td><td>-</td><td>Value if the beta is in the beta program</td></tr><tr><td>only_check</td><td>boolean<br>Default: false</td><td>-</td><td>Only check unread messages and ignore other data completely</td></tr></tbody></table>

### Example request

{% tabs %}
{% tab title="Python" %}
```python
import requests

endpoint_id = 1
token = "S3Kjd0LHDLhPWJDnxeh..."

log_discord = "1234..."
blacklist = "badword,anotherbadword" # words can be passed with a , to insert multiple

params = f"log_discord={log_discord}&blacklist={blacklist}"

request = requests.post(f"https://api.astroid.cc/update/{endpoint_id}?token={token}&{params}")
data = request.json()
print(data)
```
{% endtab %}
{% endtabs %}

### Example response

The response you get when updating data in an endpoint is the live updated data

```json
{
    "config": {
            "allowed-ids": [],
            "blacklist": [
                    "badword",
                    "anotherbadword"
            ],
            "channels": { 
                "discord": [
                        "10481283...",
                        "1234..."
                        ],
                "guilded": [
                        "76a74hGS-0b..."
                        ],
                "nerimity": [
                        "154820..."
                        ],
                "revolt": [
                        "0KN5XD2..."
                        ]
                },
        "isbeta": false,
        "logs": {
                "discord": "10703...",
                "guilded": "093c5096-06...",
                "nerimity": null,
                "revolt": null
                },
        "self-user": false,
        "webhooks": {
                "discord": [
                        "https://discord.com/api/webhooks/1247..."
                        ],
                "guilded": [
                        "https://media.guilded.gg/webhooks/c4295ddb-f8...",
                        ]
                "nerimity": [ 
                        // No Webhooks supported
                        ],
                "revolt": [
                        // No Webhooks supported
                        ]
                }
        },
"meta": {
        "message": {
                "attachments": [],
                "author":{
                        "avatar": null,
                        "id": null,
                        "name": null
                        },
                "content": null,
                "embed":{
                        "description": "None",
                        "fields": [],
                        "footer": "None",
                        "image": "None",
                        "thumbnail": "None",
                        "title": "None"},
                        // isReply and the reply object may be missing in your data
                        "isReply": false,
                        "reply":{
                                "author": null,
                                "message": null
                                }
                        },
                        "read":{
                                "discord": false,
                                "guilded": false,
                                "nerimity": false,
                                "revolt": false
                                },
                        "sender": null,
                        "sender-channel": null,
                        "trigger": false
                }
        }
```

