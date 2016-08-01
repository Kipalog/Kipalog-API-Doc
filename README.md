# Welcome
Welcome to kipalog API guide, we're happy to have you as a developer who are willing to make kipalog a better place.
This document provide some simple guides to call kipalog api.

# Prepare
To call kipalog api, two below conditions need to be satisfied:
- You must be a registered kipalog user
- You must have "at least" two written post at kipalog

You may ask: Why we need to write post to using an API.
The answer is simple: We need to confirm you as a human :P. Moreover, we want you to be an user who is humble to share your knowledge, before giving you a way to get knowledge from another people :). To write post is easy, you can write 2 short [TIL Post](http://kipalog.com/til), to share whatever you've learnt today.

# API guide

Kipalog provide api-authenticate by using http header named "X-Kipalog-Token".
The token is automatically granted to all users who wrote 2 posts at kipalog.

## Where you can get the token?

Go to [Cài đặt] menu at the bottom right, and you will see below screen:

![](https://i.gyazo.com/7633c7ef585f3a10c75578d71856728b.png)

Take the red part, this is your token (or api key).
Be remember : *Keep this token safe*, or other people will be able to act as your behave at kipalog :(.


## Usage
Currently we provide 5 api endpoint:
- POST /api/v1/post : to create a post
- POST /api/v1/post/preview : to get a html preview of a post by kipalog-markdown engine
- GET  /api/v1/post/hot : get 30 recent hot post
- GET  /api/v1/post/newest : get 30 recent newest post
- POST /api/v1/post/bytag : get 30 recent post of a tag

Each request need to attach above token, for example as below:

```bash
curl -H "Accept-Charset: application/json" -H "X-Kipalog-Token: 50apc5dac3vwjvbrr8631s42j1veif" http://localhost:3000/api/v1/post/newest | jq
```

### POST `/api/v1/post`
What: To create a new post at kipalog
Limit: 20 request / 1 days

#### Request
`METHOD`: POST, an object typed **JSON**, demo:
```json
{
  "title" : "Khởi nghiệp? Xem film này đi!",
  "content": "`console.log('Joy (2015)');`",
  "status": "published",
  "tag": "startup, film"
}
```
| key | | value | description |
|---|---|---|---|
| title | required | string | title of a post |
| content | required | string | markdown content |
| status | required | string ("draft" or "published") | status of post |
| tag | option | string (list of string, split by comma) | list of tags |

#### Response
a **JSON** object
```json
{
  "content":"",
  "status": 422,
  "cause": "unprocessable entity"
}
```
| key | value | description |
|---|---|---|
| content | string | normaly empty |
| status | int | http status code |
| cause | string | failed reason if fail |

### POST `/api/v1/post/preview`
What: To preview a post at kipalog using kipalog markdown engine
Limit: 100 request / 5 minutes

#### Request
**JSON**

```json
{
  "content": "**content of a post (markdown)**",
}
```
| key | | value | description |
|---|---|---|---|
| content | required | string | content of need-rendered post |

#### Response
**JSON**
```json
{
  "content":"<html></html>",
  "status": 422,
  "cause": "unprocessable entity"
}
```

| key | value | description |
|---|---|---|
| content | string | rendered html of posted content |
| status | int | http status code |
| cause | string | failed reason if fail |

### GET `/api/v1/post/hot`
What: Get 30 recent hot post [only publiced post]
Limit: 300 request / 5 minutes

#### Request
N/A

#### Response
**JSON**

```json
{
  "status": 200,
  "cause": "",
  "content": [
    {
      "title": "Khởi nghiệp? Xem film này đi!",
      "content": "`console.log('Joy (2015)');`",
    }, {
      "title": "Preview",
      "content": "**content of a post (markdown)**",
    }
  ]
}
```
Response a array of post object.
*Get 30 recent hot post*

A json object of post:

| key | value | description |
|---|---|---|
| status | int | http status code |
| cause | string | failed reason if fail |
| content | array | array of 30 recent hot post object |

**Posts object**

| key | value | description |
|---|---|---|
| title | string | post title |
| content | string | Markdown content |


### GET `/api/v1/post/newest`
What: Get 30 recent newest post
Limit: 300 request / 5 minutes

#### Request
N/A

#### Response
**JSON**
List of 30 recent newest post. 

```json
{
  "status": 200,
  "cause": "",
  "content": [
    {
      "title": "Khởi nghiệp? Xem film này đi!",
      "content": "`console.log('Joy (2015)');`",
    }, 
    {
      "title": "Preview",
      "content": "**content of a post (markdown)**",
    }
  ]
}
```

### POST `/api/v1/post/bytag`
What: Get 30 recent post by tag
Limit: 300 request / 5 minutes

#### Request
```
{
  "tag_name": "til"
}
```

| key | | value | description |
|---|---|---|---|
| tag_name | required | string | a tag |

#### Response
**JSON**


```json
{
  "status": 200,
  "cause": "",
  "content": [
    {
      "title": "Khởi nghiệp? Xem film này đi!",
      "content": "`console.log('Joy (2015)');`",
    }, 
    {
      "title": "Preview",
      "content": "**content of a post (markdown)**",
    }
  ]
}
```
