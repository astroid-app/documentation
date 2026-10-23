---
icon: code
description: >-
  Everything you need to talk to the Astroid API: base URL, authentication,
  rate limits and error handling.
---

# API overview

The Astroid API is what the Astroid bots use under the hood. You can use it too, to read and change your endpoint, send messages into your bridge or clean up your data.

## Base URL

```
https://api.astroid.cc
```

All paths in this documentation are relative to this URL.

## Endpoints and IDs

Every bridge is stored as an **endpoint**. The endpoint ID is the ID of the Discord server you ran <mark style="color:purple;">`/register`</mark> in.

{% hint style="info" %}
Discord IDs are bigger than JavaScript can safely store as a number. Send them as **strings** in JSON bodies (`"endpointId": "1045437427965231284"`), not as numbers.
{% endhint %}

## Authentication

Every request that reads or changes an endpoint needs your <mark style="color:yellow;">Astroid token</mark>, passed as the `token` **query parameter**:

```
GET https://api.astroid.cc/YOUR-ENDPOINT-ID?token=YOUR-TOKEN
```

* You get your token when you run <mark style="color:purple;">`/register`</mark> on Discord.
* Lost it? Run <mark style="color:purple;">`/generate-token`</mark> on Discord to get a new one.
* A token only works for the endpoint it belongs to.
* An endpoint can have several valid tokens. Generating a new token does **not** invalidate the old ones. Delete old tokens with [`DELETE /deletetoken`](tokens-and-deletion.md#delete-a-token).

{% hint style="danger" %}
Your token gives full control over your bridge. Never share it and never commit it to a public repository.
{% endhint %}

Some routes need the **master token** instead. Only the Astroid team (or you, if you self-host) has it. Those routes are listed in [Admin & self-hosting](admin.md).

## Request format

* Only `token` (and a few lookup values such as `id` on `GET` routes) go into the query string.
* Everything else is sent as a **JSON body**, with the `Content-Type: application/json` header.

```shell
curl --request POST 'https://api.astroid.cc/YOUR-ENDPOINT-ID/config?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{"blacklist": ["badword"]}'
```

## Rate limit

The API allows **10 requests per second per IP address**. If you go over it, you get:

```json
{
    "message": "Too Many Requests",
    "status": 429
}
```

Wait a moment and try again.

## Errors

Errors are returned as JSON with a `message` and, on most routes, the HTTP `status`:

```json
{
    "message": "Unauthorized: Invalid token",
    "status": 401
}
```

| Status | Meaning                                                                 |
| ------ | ----------------------------------------------------------------------- |
| `400`  | The request is missing something or a value has the wrong type.        |
| `401`  | No token, or the token is wrong for this endpoint.                      |
| `403`  | The endpoint is suspended.                                              |
| `404`  | The endpoint (or channel, asset, …) does not exist.                     |
| `409`  | The endpoint already exists.                                            |
| `429`  | Rate limit exceeded.                                                    |
| `500`  | Something went wrong on our side. Tell us on a [support server](../useful/support-server-list.md). |

Validation errors include a `details` array that says exactly what is wrong:

```json
{
    "message": "Bad Request: Invalid config",
    "status": 400,
    "details": ["blacklist must be an array"]
}
```

## Supported platforms

These platform keys are used throughout the API:

| Key        | Platform                  |
| ---------- | ------------------------- |
| `discord`  | Discord                   |
| `guilded`  | Guilded                   |
| `revolt`   | Stoat (formerly Revolt)   |
| `nerimity` | Nerimity                  |

## Machine-readable route list

`GET /references` returns every route the running API knows, with its path parameters, query parameters and body fields. It is generated from the source code when the API starts, so it is always current.

## Where to go next

* [Endpoint object](endpoint-object.md): what an endpoint looks like
* [Endpoints & configuration](endpoints.md): read and change your bridge
* [Messages](messages.md): send messages into a bridge
* [Emojis](emojis.md): map custom emojis between platforms
* [Tokens & deletion](tokens-and-deletion.md): remove tokens, data or the whole endpoint
* [Public routes](public.md): statistics, health, invites and more
