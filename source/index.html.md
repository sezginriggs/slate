---
title: ModerationPipe API Reference


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>


includes:
  - errors

search: true
---

# Introduction

Welcome to the ModerationPipe API! You can use our API to access ModerationPipe API endpoints, which can check for profanity and provide access to custom whitelist and blacklist.


# Authentication

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl "https://api.moderationpipe.com/v1/profanity/check"
  -H "Authorization: ApiKey your-api-key-here"
```


> Make sure to replace `your-api-key-here` with your API key.

ModerationPipe uses API keys to allow access to the API. You can register a user and get API key at our [website](http://moderationpipe.com).

ModerationPipe API expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: ApiKey your-api-key-here`

or with `api_key` parameter.

<aside class="notice">
You must replace <code>your-api-key-here</code> with your personal API key.
</aside>

<aside class="warning">
Be careful when sharing API keys. Don't publish them in public code repositories or add them to client-side API calls.
</aside>


# Profanity

## Check for Profanity


```shell
curl "https://api.moderationpipe.com/v1/profanity/check"
  -H "Authorization: ApiKey your-api-key-here"
```

```http
POST http://api.moderationpipe.com/test
```

> The above command returns JSON structured like this:

```json
{
  "data": {
   "found": false,
   "count": 0,
   "words": [],
   "language_auto_detection": false,
   "language_used": "tr"
    }
}

```

This endpoint retrieves all kittens.

### HTTP Request

`POST http://api.moderationpipe.com/v1/profanity/check`

### Query Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
text |  | yes     | Text to be checked against profanity or content keywords.
language |  | no | 2 letter language code for the text. If nothing defined, API will try to detect language automatically and if no language has been found, English will be used.
content_relation | 0 | no | If set to 1, method will check against content related keywords for subjects like hacking, drugs, porn etc.
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten


```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```


> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint retrieves a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete



# API Rate limit

ModerationPipe API enforces two kinds of rate limits. Application-level throttling for protecting other users and Account-level throttling.

## Application-level Limits

Request limit for each method is 120 request per minute.

Use the following HTTP headers in order to understand where the application is at for a given rate limit, on the method that was just utilized.

* `X-RateLimit-Limit` the rate limit ceiling for that given endpoint
* `X-RateLimit-Remaining` the number of requests left
* `X-RateLimit-Reset` the remaining time before the rate limit resets, in UTC epoch seconds
* `Retry-After` the remaining time before the rate limit resets, in seconds.

When the rate limit exceeds, the API will return a HTTP 429 "Too Many Requests" response code.

## Account-level Limits

Total request limit for each account is related with subscription package selected for the account. Please refer to membership pages of our website for account level call limits.

When the call limit exceeds for account, the API will return a HTTP 429 "Too Many Requests" response code.
