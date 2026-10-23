---
description: >-
  SolarID, Astroid's OAuth, service is a concept and maybe won't last. Altough
  you can learn how to optain a token here.
icon: user-lock
---

# \[Concept] Getting an user token

## _This page is only dedicated to developers who want to integrate SolarID in their app. For more information please contact us._

Getting a SolarID usertoken is relativeley easy. You can either sign up or login and go to the /me path. This will return your SolarID id and your SolarID token along with your username and connected services.

## Example <mark style="color:purple;">`/signup`</mark> request

{% tabs %}
{% tab title="cURL" %}
```sh
curl --location --request GET "https://auth.astroid.cc/auth/signup"
```
{% endtab %}
{% endtabs %}

## Example <mark style="color:purple;">`/signup`</mark> response

<pre class="language-json"><code class="lang-json">{
    "message": "created solarid",
<strong>    "solarid": "bb3ca..."
</strong>}
</code></pre>

The token will be saved in an encrypted cookie called <mark style="color:purple;">`session`</mark> and cannot be read out directly.

To get your token you'd have to use the <mark style="color:purple;">`/me`</mark> path.

## Example <mark style="color:purple;">`/me`</mark> request

{% tabs %}
{% tab title="cURL" %}
```json
curl --location --request GET "https://auth.astroid.cc/me"
```
{% endtab %}
{% endtabs %}

## Example <mark style="color:purple;">`/me`</mark> response

```json
{
    "services": {
        "discord": "150543...",
        "github": "142..."
        },
    "token": "u_Shd0qkf4LWk...",
    "username": "some_username",
    "solarid": "uao56w..."
}
```

