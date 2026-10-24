---
description: Remove tokens, single pieces of data or your whole endpoint.
icon: user-key
---

# Tokens and Deletion

## Get a new token

New tokens are created by the Discord bot. Run <mark style="color:purple;">`/generate-token`</mark> on your Discord server.

The new token is **added** to your endpoint. Your old tokens keep working until you [delete them](tokens-and-deletion.md#delete-a-token).

## Delete a token

<mark style="color:red;">`DELETE`</mark> `/deletetoken?token=YOUR-TOKEN`

Removes one token from your endpoint. You can authenticate with any of your valid tokens, including the one you're deleting.

Body:

```json
{
    "endpointId": "YOUR-ENDPOINT-ID",
    "token": "THE-TOKEN-TO-DELETE"
}
```

```shell
curl --request DELETE 'https://api.astroid.cc/deletetoken?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{"endpointId": "YOUR-ENDPOINT-ID", "token": "THE-TOKEN-TO-DELETE"}'
```

```json
{
    "message": "Token deleted",
    "endpointId": "YOUR-ENDPOINT-ID",
    "token": "THE-TOKEN-TO-DELETE"
}
```

## Delete your endpoint

<mark style="color:red;">`DELETE`</mark> `/delete?token=YOUR-TOKEN`

Deletes your endpoint and all its tokens. **This cannot be undone.** Your bridge stops working right away.

Body:

```json
{
    "endpointId": "YOUR-ENDPOINT-ID"
}
```

```shell
curl --request DELETE 'https://api.astroid.cc/delete?token=YOUR-TOKEN' \
  --header 'Content-Type: application/json' \
  --data '{"endpointId": "YOUR-ENDPOINT-ID"}'
```

| Status | Response                                                                       |
| ------ | ------------------------------------------------------------------------------ |
| `200`  | `{"message": "Endpoint and tokens deleted", "endpointId": "YOUR-ENDPOINT-ID"}` |
| `400`  | `{"message": "Missing endpointId", "status": 400}`                             |
| `401`  | `{"message": "Unauthorized: Invalid token", "status": 401}`                    |
| `500`  | `{"message": "Failed to delete endpoint", "status": 500}`                      |

A step-by-step guide is on [Deleting your endpoint](/broken/pages/cddfa4fc3ee9c69a16acceaf9bc722b2a048eb8f).

## Delete single values

<mark style="color:red;">`DELETE`</mark> `/delete/data/{endpointId}?token=YOUR-TOKEN`

Removes single values from your endpoint without deleting it. Send only the keys you need.

| Body key                                                                               | Value                 | Removes                               |
| -------------------------------------------------------------------------------------- | --------------------- | ------------------------------------- |
| `channel_discord`, `channel_guilded`, `channel_revolt`, `channel_nerimity`             | channel ID            | That channel from `config.channels`   |
| `webhook_discord`, `webhook_guilded`, `webhook_revolt`, `webhook_nerimity`             | webhook URL           | That webhook from `config.webhooks`   |
| `log_discord`, `log_guilded`, `log_revolt`, `log_nerimity`                             | `true`                | The log channel of that platform      |
| `blacklist`                                                                            | comma-separated words | Those words from `config.blacklist`   |
| `allowed_ids`                                                                          | comma-separated IDs   | Those IDs from `config.allowed-ids`   |
| `sender`, `sender_channel`                                                             | `true`                | The last sender info in `meta`        |
| `message_author_name`, `message_author_id`, `message_author_avatar`, `message_content` | `true`                | That field of the last message        |
| `message_attachments`                                                                  | attachment URL        | That attachment from the last message |

Example: remove a log channel and two blacklisted words.

```json
{
    "log_discord": true,
    "blacklist": "badword,worseword"
}
```

The response is the updated [endpoint object](/broken/pages/7c6cd40e8c1d6fb5eecb80939d208052710ea7d9).
