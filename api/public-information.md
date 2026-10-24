---
description: >-
  Find Astroid's public accessable information such as API health or useful
  documents.
icon: info
---

# Public Information

These routes don't need a token.

## API info

<mark style="color:green;">`GET`</mark> `/`

```json
{
    "heading": "Astroid API",
    "description": "Astroid API for getting and modifying endpoints.",
    "website": "https://astroid.cc",
    "privacy": "https://astroid.cc/privacy",
    "terms": "https://astroid.cc/terms",
    "imprint": "https://astroid.cc/imprint",
    "docs": "https://astroid.cc/docs",
    "discord": "https://discord.gg/DbrFADj6Xw"
}
```

## Statistics

<mark style="color:green;">`GET`</mark> `/statistics`

```json
{
    "messages": {
        "id": "statistics:messages",
        "month": 12345,
        "periodStart": "2026-10-01T00:00:00.000Z",
        "total": 1234567,
        "total_rounded": 1234000,
        "month_rounded": 12300
    },
    "endpoints": 42
}
```

The `*_rounded` values are rounded down, which suits display such as "over 1,234,000 messages".

## API health

| Route                                                     | Returns                                                         |
| --------------------------------------------------------- | --------------------------------------------------------------- |
| <mark style="color:green;">`GET`</mark> `/health`         | Current status of the API, its databases and the message queue. |
| <mark style="color:green;">`GET`</mark> `/health/latency` | Latency history.                                                |

## Channel names

<mark style="color:green;">`GET`</mark> `/channel/name/{platform}?id={channelId}`

Returns the name of a channel the Astroid bot can see. `platform` is `discord`, `nerimity` or `revolt`.

```json
"general"
```

Returns `404` if the channel doesn't exist or Astroid can't see it.

## Invite links

<mark style="color:green;">`GET`</mark> `/invite/{platform}`

Redirects to the invite page of the Astroid bot for that platform. `platform` is `discord`, `guilded` or `nerimity`.

<mark style="color:green;">`GET`</mark> `/discord` redirects to the Astroid Discord server.

## Files

| Route                                                     | Returns                                        |
| --------------------------------------------------------- | ---------------------------------------------- |
| <mark style="color:green;">`GET`</mark> `/assets/{asset}` | Astroid assets, for example `logo`.            |
| <mark style="color:green;">`GET`</mark> `/cdn/{assetId}`  | A file that was attached to a bridged message. |

## Contributors

| Route                                                                                      | Returns                                   |
| ------------------------------------------------------------------------------------------ | ----------------------------------------- |
| <mark style="color:green;">`GET`</mark> `/contribution/contributors`                       | All Astroid contributors.                 |
| <mark style="color:green;">`GET`</mark> `/contribution/contributor?name=` or `?id=`        | One contributor. Pass `name` **or** `id`. |
| <mark style="color:green;">`GET`</mark> `/contribution/contributor/avatar?name=` or `?id=` | That contributor's avatar URL.            |

## Route list

<mark style="color:green;">`GET`</mark> `/references`

Every route of the API with its parameters, generated when the API starts.

```json
{
    "generatedAt": "2026-10-13T12:00:00.000Z",
    "count": 60,
    "endpoints": [
        {
            "method": "GET",
            "path": "/bridges/:id",
            "params": ["id"],
            "query": ["token"],
            "body": []
        }
    ]
}
```
