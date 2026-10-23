---
icon: sliders
description: Read your endpoint and change its configuration.
---

# Endpoints & configuration

## Get an endpoint

<mark style="color:green;">`GET`</mark> `/{endpointId}?token=YOUR-TOKEN`

Returns the full [endpoint object](endpoint-object.md).

```shell
curl 'https://api.astroid.cc/YOUR-ENDPOINT-ID?token=YOUR-TOKEN'
```

| Status | Response                                                     |
| ------ | ------------------------------------------------------------ |
| `200`  | The [endpoint object](endpoint-object.md)                    |
| `401`  | `{"message": "Unauthorized: Invalid token", "status": 401}`  |
| `403`  | `{"message": "Forbidden: Endpoint is suspended", "status": 403}` |
| `404`  | `{"message": "Endpoint not found", "status": 404}`           |

## Add to the configuration

<mark style="color:orange;">`POST`</mark> `/{endpointId}/config?token=YOUR-TOKEN`

Changes your endpoint's `config`. The body is a **partial** config: send only the keys you want to change.

* **Lists** (`channels.*`, `webhooks.*`, `blacklist`, `allowed-ids`, `emojis`): the values you send are **added** to the existing list. They don't replace it.
* **Single values** (`logs.*`, `emoji_filtering`, `self-user`, `isbeta`): the value you send replaces the old one.

Allowed top-level keys: `allowed-ids`, `blacklist`, `channels`, `emoji_filtering`, `emojis`, `isbeta`, `logs`, `self-user`, `webhooks`. See the [endpoint object](endpoint-object.md#config) for what each one does.

{% tabs %}
{% tab title="Add a blacklisted word" %}
```json
{
    "blacklist": ["badword"]
}
```
{% endtab %}

{% tab title="Bridge another channel" %}
```json
{
    "channels": { "nerimity": ["1570083353837875201"] },
    "webhooks": { "nerimity": ["https://..."] }
}
```
{% endtab %}

{% tab title="Set the log channel" %}
```json
{
    "logs": { "discord": "1234567890123456789" }
}
```
{% endtab %}
{% endtabs %}

```shell
curl --request POST 'https://api.astroid.cc/YOUR-ENDPOINT-ID/config?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{"blacklist": ["badword"]}'
```

| Status | Response                                                                                     |
| ------ | -------------------------------------------------------------------------------------------- |
| `200`  | The updated `config` object                                                                  |
| `400`  | `{"message": "Bad Request: Invalid config", "status": 400, "details": [...]}`                |
| `401`  | `{"message": "Unauthorized: Invalid token", "status": 401}`                                  |
| `403`  | `{"message": "Forbidden: Endpoint is suspended", "status": 403}`                             |
| `404`  | `{"message": "Endpoint not found", "status": 404}`                                           |

{% hint style="info" %}
`channels` and `webhooks` must contain at least one platform key when you send them. Keep the [channel order](endpoint-object.md#how-channels-are-paired) in mind when you add channels.
{% endhint %}

## Remove from the configuration

<mark style="color:red;">`DELETE`</mark> `/{endpointId}/config?token=YOUR-TOKEN`

Removes items from list values in your `config`. The body has the same shape as for [adding](#add-to-the-configuration). Every value you list is removed. Non-list values are ignored.

```shell
curl --request DELETE 'https://api.astroid.cc/YOUR-ENDPOINT-ID/config?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{"blacklist": ["badword"]}'
```

The responses are the same as for [adding](#add-to-the-configuration). An empty body returns `400` with `"Bad Request: No config provided"`.

{% hint style="warning" %}
Removing a channel from the middle of a list shifts every channel after it, which changes [which channels are paired](endpoint-object.md#how-channels-are-paired). Remove the matching channel on every platform at the same time.
{% endhint %}

## List bridged channels

<mark style="color:green;">`GET`</mark> `/bridges/{endpointId}?token=YOUR-TOKEN`

Returns only the bridged channel IDs per platform.

```json
{
    "discord": ["1234567890123456789"],
    "guilded": [],
    "revolt": [],
    "nerimity": ["1570083353837875201"]
}
```

## Check endpoint health

<mark style="color:green;">`GET`</mark> `/healthcheck/{endpointId}?token=YOUR-TOKEN`

Checks that your endpoint has every key it needs. Use this if your bridge behaves strangely.

| Status | Response                                                   |
| ------ | ---------------------------------------------------------- |
| `200`  | `{"message": "Endpoint is healthy", "status": 200}`        |
| `500`  | `{"message": "Endpoint is not healthy", "status": 500}`    |

## Repair an endpoint

<mark style="color:orange;">`POST`</mark> `/healthcheck/{endpointId}/repair?token=YOUR-TOKEN`

Adds back any keys that are missing from your endpoint, with their default values. Existing values are kept.

```json
{
    "message": "Endpoint repair initiated",
    "status": 200
}
```

## Get your Discord channel structure

<mark style="color:green;">`GET`</mark> `/getserverstructure?id={endpointId}&token=YOUR-TOKEN`

Returns the text and voice channels of your Discord server, grouped by category. Useful for building a channel picker.

```json
{
    "General": [
        { "name": "chat", "id": "1234567890123456789", "type": "text" },
        { "name": "Voice", "id": "1234567890123456790", "type": "voice" }
    ],
    "uncategorized": [
        { "name": "rules", "id": "1234567890123456791", "type": "text" }
    ]
}
```
