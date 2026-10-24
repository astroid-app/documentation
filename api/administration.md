---
description: >-
  Routes that need the master token. Only relevant for the Astroid team and
  self-hosters.
icon: server
---

# Administration

These routes only accept the **master token** (`MASTER_TOKEN` in the API's `.env`). Regular endpoint tokens are rejected.

The master token also works on every route that takes an endpoint token.

## Endpoints

| Route                                                                       | Body                               | Description                                                                    |
| --------------------------------------------------------------------------- | ---------------------------------- | ------------------------------------------------------------------------------ |
| <mark style="color:orange;">`POST`</mark> `/create`                         | `{"endpointId": "..."}`            | Creates an endpoint. Returns `{"endpointId", "token"}`, or `409` if it exists. |
| <mark style="color:orange;">`POST`</mark> `/newtoken`                       | `{"endpointId": "..."}`            | Adds a new token to an endpoint. Returns `{"endpointId", "token"}`.            |
| <mark style="color:green;">`GET`</mark> `/allendpoints`                     | none                               | IDs of all endpoints.                                                          |
| <mark style="color:green;">`GET`</mark> `/getendpoint/{platform}?serverid=` | none                               | Finds the endpoint a Guilded, Stoat or Nerimity server belongs to.             |
| <mark style="color:orange;">`POST`</mark> `/createendpoint/{platform}`      | `{"endpoint": "...", "id": "..."}` | Links a Guilded, Stoat or Nerimity server (`id`) to an endpoint.               |
| <mark style="color:orange;">`POST`</mark> `/update/{endpointId}/json`       | full endpoint object               | Overwrites the whole endpoint.                                                 |
| <mark style="color:purple;">`PATCH`</mark> `/syncserverrelations`           | none                               | Removes server links whose endpoint no longer exists.                          |

## Maintenance

| Route                                                                              | Description                                                                                                                   |
| ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:purple;">`PATCH`</mark> `/daily_endpoint_check`                 | Checks every endpoint. Returns a link to the summary under `/healtcheck_summaries/{id}`.                                      |
| <mark style="color:red;">`DELETE`</mark> `/tempattachments`                        | Clears temporary attachments.                                                                                                 |
| <mark style="color:green;">`GET`</mark> `/errors/{id}`                             | The traceback of a reported error.                                                                                            |
| <mark style="color:orange;">`POST`</mark> `/migrate/endpoints/surreal-to-postgres` | Migrates endpoints from SurrealDB to PostgreSQL. Body: `dryRun`, `overwriteExisting`, `migrateOutOfRangeToOverflow`, `limit`. |

## Opt-out

| Route                                                        | Description                  |
| ------------------------------------------------------------ | ---------------------------- |
| <mark style="color:green;">`GET`</mark> `/optout/{userId}`   | Whether a user opted out.    |
| <mark style="color:orange;">`POST`</mark> `/optout/{userId}` | Opts a user out of bridging. |
| <mark style="color:orange;">`POST`</mark> `/optin/{userId}`  | Opts a user back in.         |

## Contributors

<mark style="color:orange;">`POST`</mark> `/contribution/addcontributor/{id}`: body `{"username": "...", "avatar": "..."}`. Creates or updates a contributor.

## Logs

These routes use `LOG_TOKEN` instead of the master token:

| Route                                                         | Description                           |
| ------------------------------------------------------------- | ------------------------------------- |
| <mark style="color:green;">`GET`</mark> `/logs?token=`        | The current log file.                 |
| <mark style="color:green;">`GET`</mark> `/logs/stream?token=` | Live log lines as Server-Sent Events. |
| <mark style="color:green;">`GET`</mark> `/viewlogs?token=`    | A log console for your browser.       |

## Configuration

| Variable               | Default | Description                                |
| ---------------------- | ------- | ------------------------------------------ |
| `RATE_LIMIT_PER_IP`    | `10`    | Requests allowed per IP within the window. |
| `RATE_LIMIT_WINDOW_MS` | `1000`  | Length of the rate-limit window in ms.     |

The API listens on port `9921` by default. Change it with `-p` or `--port`.
