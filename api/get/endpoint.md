---
description: The /{endpoint} path lets you get the full data of an endpoint.
icon: folder-tree
---

# /{endpoint}

### Path parameters

endpoint: The ID of your endpoint (usually your Discord server id)

### Query parameters

token: Your Astroid token ([see Getting a token](../get-started/getting-an-endpoint-token.md))

## Example request

{% tabs %}
{% tab title="Python" %}
```python
import requests

endpoint_id = 1
token = "S3Kjd0LHDLhPWJDnxeh..."

request = requests.get(f"https://api.astroid.cc/{endpoint_id}?token={token}")
data = request.json()
print(data)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const endpoint_id = 1
const token = "S3Kjd0LHDLhPWJDnxeh..."

await fetch(`https://api.astroid.cc/${endpoint_id}?token=${token}`, 
    {"method": "GET"}
)
    .then((response) => response.json())
    .then((data) => {
        console.log(data)
    })
```
{% endtab %}

{% tab title="cURL" %}
```sh
curl --location -- request GET "https://api.astroid.cc/{endpoint_id}?token={token}"

```
{% endtab %}
{% endtabs %}

## Example response

```json
{
    "config": {
            "allowed-ids": [],
            "blacklist": [],
            "channels": { 
                "discord": [
                        "10481283..."
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

Depending on your current state, the meta tab may look different from the example.
