---
description: Map custom emojis between platforms.
icon: face-smile-plus
---

# Emojis

Custom emojis are different on every platform. An emoji mapping tells Astroid which emoji on one platform matches which emoji on another.

## Emoji formats

| Platform | Format         | Example                |
| -------- | -------------- | ---------------------- |
| Discord  | `<:name:id>`   | `<:test:1234567890>`   |
| Guilded  | `<:name:id>`   | `<:test:1234567890>`   |
| Stoat    | `:name:`       | `:test:`               |
| Nerimity | `[ce:id:name]` | `[ce:1234567890:test]` |

An emoji mapping is one object with one key per platform:

```json
{
    "discord": "<:test:1234567890>",
    "guilded": "<:test:1234567890>",
    "revolt": ":test:",
    "nerimity": "[ce:1234567890:test]"
}
```

If a message contains a custom emoji without a mapping, it is sent as plain text. Set `emoji_filtering` to `true` in your [config](/broken/pages/b74680fcd17608721a869ee7cd55a2dee8039691#add-to-the-configuration) to remove such emojis instead.

## Add an emoji

<mark style="color:orange;">`POST`</mark> `/emojis/{endpointId}/add?token=YOUR-TOKEN`

Body: an emoji mapping.

```shell
curl --request POST 'https://api.astroid.cc/emojis/YOUR-ENDPOINT-ID/add?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{"discord": "<:test:1234567890>", "nerimity": "[ce:1234567890:test]"}'
```

```json
{
    "message": "Added emoji {...} to endpoint YOUR-ENDPOINT-ID"
}
```

## Remove an emoji

<mark style="color:red;">`DELETE`</mark> `/emojis/{endpointId}/delete?token=YOUR-TOKEN`

Body: an emoji mapping. Every mapping that shares at least one platform value with it is removed.

## Update an emoji

<mark style="color:orange;">`POST`</mark> `/emojis/{endpointId}/update?token=YOUR-TOKEN`

Body: the new emoji mapping.

{% hint style="warning" %}
This route currently overwrites **every** emoji mapping of your endpoint with the values you send. Until that is fixed, remove the old mapping and add the new one instead.
{% endhint %}

## Sync emojis from Discord

<mark style="color:purple;">`PATCH`</mark> `/emojis/{endpointId}/sync?token=YOUR-TOKEN`

Body: `{"platform": "discord"}`

Not available yet. The route returns `501 Not Implemented`.

## Errors

If the body is missing or isn't an object, the API returns `400` with an example:

```json
{
    "message": "You must provide an emoji structure.",
    "example": {
        "discord": "<:test:1234567890>",
        "guilded": "<:test:1234567890>",
        "revolt": ":test:",
        "nerimity": "[ce:1234567890:test]"
    }
}
```
