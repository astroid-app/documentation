---
description: >-
  The /bridges/{endpoint} endpoint returns a list of bridged channels, with only
  channel IDs
icon: bridge
---

# /bridges/{endpoint}

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

request = requests.get(f"https://api.astroid.cc/bridges/{endpoint_id}?token={token}")
data = request.json()
print(data)
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const endpoint_id = 1
const token = "S3Kjd0LHDLhPWJDnxeh..."

await fetch(`https://api.astroid.cc/bridges/${endpoint_id}?token=${token}`, 
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
curl --location --request GET "https://api.astroid.cc/bridges/{endpoint}?token={token}"
```
{% endtab %}
{% endtabs %}

## Example response

```json
{
    "discord": [
        "124643..."
        ],
    "guilded": [
        "76b27d56-0b..."
        ],
    "revolt": [
        "0KN5XD2..."
        ],
    "nerimity": [
        "152712..."
        ]
}
```



