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


# Structure

Our API is designed to be as simple to use as possible. We use these top-level members for all services.

- `data` contains the main result data
- `errors` shows errors with descriptions. Also please check [errors](#errors) section for HTTP response codes.


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

curl https://api.moderationpipe.com/v1/profanity/check \
   -H "Authorization: ApiKey your-api-key-here" \
   -d text="Your text here" \
   -d language=en \
   -d content_relation=1

```


> The above command returns JSON structured like this:

```json

{
  "data": {
     "found": true,
     "count": 4,
     "words": [
          {
            "word": "some-bad-word-here",
            "type": "profanity",
            "content_relation": null
          },
          {
            "word": "some-another-bad-word-here",
            "type": "profanity",
            "content_relation": null
          },
          {
            "word": "casino",
            "type": "content_related",
            "content_relation": "gambling"
          },
          {
            "word": "keylogger",
            "type": "content_related",
            "content_relation": "hacking"
          }
     ],
     "language_auto_detection": false,
     "language_used": "en"
  }
}


```

This endpoint checks text for profanity and returns list of found words and related information.

### HTTP Request

`POST https://api.moderationpipe.com/v1/profanity/check`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
text |  | yes     | Text to be checked against profanity or content keywords.
language |  | no | 2 letter language code for the text. If nothing defined, API will try to detect language automatically and if no language has been found, English will be used.
content_relation | 0 | no | If set to 1, method will check against content related keywords for subjects like hacking, drugs, porn etc.
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.


### Result Attributes

Key | Description
--------- | -----------
found | `true` if profanity word(s) found
count | number of found profanity words
words | array of objects for found words. each object includes following values<br>* `word` for found word <br>* `type` for detection type<br>* `content_relation` for related content if *content_relation* parameter was set to 1
language_auto_detection | `true` if language parameter was empty or unsupported language and language detected automatically by the service.
language_used | 2-letter language code of used language for detection.


## Get Whitelist


```shell
curl "https://api.moderationpipe.com/v1/profanity/whitelist" \
  -H "Authorization: ApiKey your-api-key-here"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "word": "myfirstword",
      "deepsearch": false
    },
    {
      "word": "mysecondword",
      "deepsearch": true
    },
    {
      "word": "mythirdword",
      "deepsearch": false
    }
 ]
}

```

This endpoint retrieves whitelist for user account.


### HTTP Request

`GET https://api.moderationpipe.com/v1/profanity/whitelist`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.


### Result Attributes

Key | Description
--------- | -----------
word | specified word
deepsearch | `true` if search in substring has been prefered



## Add Word to Whitelist


```shell
curl "https://api.moderationpipe.com/v1/profanity/whitelist" \
   -H "Authorization: ApiKey your-api-key-here" \
   -d word="mysampleword" \
   -d deepsearch=1
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "word": "mysampleword",
    "deepsearch": true
  }
}


```

This endpoint adds words to whitelist of associated user account.


### HTTP Request

`POST https://api.moderationpipe.com/v1/profanity/whitelist`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
word      |         | yes    | word to be added to whitelist.
deepsearch| 0       | no     | `1` for searching word in substrings.
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.


### Result Attributes

Key | Description
--------- | -----------
word | specified word
deepsearch | `true` if search in substring has been prefered



## Remove Word from Whitelist

```shell
curl "https://api.moderationpipe.com/v1/profanity/whitelist" \
  -X DELETE \
  -H "Authorization: ApiKey your-api-key-here" \
  -d word="yourwordhere"
```


> The above command won't return any JSON response but will return following HTTP status:

```shell
204 No Content
```

This endpoint removes word from whitelist

### HTTP Request

`DELETE https://api.moderationpipe.com/v1/profanity/whitelist`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
word      |         | yes    | word to be removed from whitelist.
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.



<!--------------------------------------------- BLACKLIST ------------------------------------------->

## Get Blacklist


```shell
curl "https://api.moderationpipe.com/v1/profanity/blacklist" \
  -H "Authorization: ApiKey your-api-key-here"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "word": "myfirstword",
      "deepsearch": false
    },
    {
      "word": "mysecondword",
      "deepsearch": true
    },
    {
      "word": "mythirdword",
      "deepsearch": false
    }
 ]
}

```

This endpoint retrieves blacklist for user account.


### HTTP Request

`GET https://api.moderationpipe.com/v1/profanity/blacklist`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.


### Result Attributes

Key | Description
--------- | -----------
word | specified word
deepsearch | `true` if search in substring has been prefered



## Add Word to Blacklist


```shell
curl "https://api.moderationpipe.com/v1/profanity/blacklist" \
   -H "Authorization: ApiKey your-api-key-here" \
   -d word="mysampleword" \
   -d deepsearch=1
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "word": "mysampleword",
    "deepsearch": true
  }
}


```

This endpoint adds words to blacklist of associated user account.


### HTTP Request

`POST https://api.moderationpipe.com/v1/profanity/blacklist`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
word      |         | yes    | word to be added to blacklist.
deepsearch| 0       | no     | `1` for searching word in substrings.
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.


### Result Attributes

Key | Description
--------- | -----------
word | specified word
deepsearch | `true` if search in substring has been prefered



## Remove Word from Blacklist

```shell
curl "https://api.moderationpipe.com/v1/profanity/blacklist" \
  -X DELETE \
  -H "Authorization: ApiKey your-api-key-here" \
  -d word="yourwordhere"
```


> The above command won't return any JSON response but will return following HTTP status:

```shell
204 No Content
```

This endpoint removes word from blacklist

### HTTP Request

`DELETE https://api.moderationpipe.com/v1/profanity/blacklist`

### Parameters

Parameter | Default | Required | Description
--------- | ------- | ------ | -----------
word      |         | yes    | word to be removed from blacklist.
api_key | | yes if no Authorization header provided | You can use this parameter with your API key instead of providing with Authorization header.





<!--------------------------------------------- /BLACKLIST ------------------------------------------>




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
