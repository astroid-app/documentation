---
description: Mark an endpoint's saved messsage read on the corresponding platform.
---

# /read/{endpoint}

### Path parameters

endpoint: The ID of your endpoint (usually your Discord server id)

### Query Parameters

| query name     | type    | min-max length      | description                          |
| -------------- | ------- | ------------------- | ------------------------------------ |
| token          | string  | 71 .. 85 characters | Astroid endpoint token               |
| read\_discord  | boolean | -                   | If the messages was sent on Discord  |
| read\_guilded  | boolean | -                   | If the messages was sent on Guilded  |
| read\_revolt   | boolean | -                   | If the messages was sent on Revolt   |
| read\_nerimity | boolean | -                   | If the messages was sent on Nerimity |

## Example request

{% tabs %}
{% tab title="Python" %}
```python
import requests

endpoint_id = 1
token = "S3Kjd0LHDLhPWJDnxeh..."

read_on_discord = True

params = f"read_discord={read_on_discord}"

request = requests.post(f"https://api.astroid.cc/read/{endpoint_id}?token={token}&{params}")
data = request.json()
print(data)
```
{% endtab %}
{% endtabs %}

## Example responses

{% tabs %}
{% tab title="200 OK" %}
<pre class="language-json"><code class="lang-json"><strong>{
</strong><strong>    "message": "Success."
</strong><strong>}
</strong></code></pre>
{% endtab %}

{% tab title="500 Internal Server Error" %}
```json
{
    "message": f"An error occurred: {some-error}"
}
```
{% endtab %}

{% tab title="403 Forbidden" %}
```ruby
{
    "message": "This endpoint is suspended."
}
```
{% endtab %}

{% tab title="401 Unauthorized" %}
```json
{
    "message": "The provided token is invalid."
}
```


{% endtab %}
{% endtabs %}



