---
description: Here you will learn how to authorize Astroid's API
icon: key
---

# Authorization

Astroid API uses Tokens (long random-generated strings) to authorize.

## Providing a token

Tokens are provided as a query parameter.\
For example:

```sh
curl --location --request GET "https://api.astroid.cc/1?token=shKJ5QgdsKWDgk7lskE..."
```

### Diffrent types of declarations

Depending where the <mark style="color:yellow;">`token`</mark> parameter is located in the URL the prefixes are different.

#### <mark style="color:yellow;">`token`</mark> parameter at the beginning of parameters

The <mark style="color:yellow;">`token`</mark> parameter is intoroduced via a <mark style="color:purple;">`?`</mark> after the last <mark style="color:blue;">path</mark> declaration:

<mark style="color:blue;">`https://api.astroid.cc/1`</mark><mark style="color:purple;">`?`</mark><mark style="color:yellow;">`token=shKJ5QgdsKWDgk7lskE...`</mark>

#### <mark style="color:yellow;">`token`</mark> paramter inside or after other parameters

The <mark style="color:yellow;">`token`</mark> parameter is intoduced with a <mark style="color:purple;">`&`</mark> after <mark style="color:blue;">a parameter</mark> (doesn't matter if between other or at the end)

`https://api.astroid.cc/update/1?`<mark style="color:blue;">`message_content=test+message`</mark><mark style="color:purple;">`&`</mark><mark style="color:yellow;">`token=shKJ5QgdsKWDgk7lskE...`</mark>
