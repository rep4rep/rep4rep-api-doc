---
title: Rep4Rep API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell

toc_footers:
  - <a href='https://github.com/rep4rep/rep4rep-bot/blob/main/api.js'>NodeJS Example API Wrapper</a>
  - <a href='https://rep4rep.com'>rep4rep.com</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Rep4Rep API
---

# Authentication

> Example request:

```shell
curl "https://rep4rep.com/pub-api/user" \
  --get \
  --data "apiToken=API_TOKEN_HERE"
```

> Make sure to replace `API_TOKEN_HERE` with your API key.


Rep4Rep uses API keys to allow access to its API. You can create & manage your api token over on the setttings page. [rep4rep settings](https://rep4rep.com/user/settings/).

Rep4Rep expects the API key to be included in all API requests as a **parameter** named `apiToken`, like so:

`https://rep4rep.com/pub-api/user?apiToken=API_TOKEN_HERE`

<aside class="notice">
You must replace <code>API_TOKEN_HERE</code> with your own Api Token.
</aside>


# User

## Get User

```shell
curl "https://rep4rep.com/pub-api/user" \
  --get \
  --data "apiToken=API_TOKEN_HERE"
```

> Example successful response:

```json
{
  "uid": 1,
  "username": "user",
  "email": "user@rep4rep.com",
  "points": 500,
  "pendingPoints": 0,
  "inGroup": true
}
```

This endpoint retrieves your own user information.

### HTTP Request

`GET https://rep4rep.com/pub-api/user`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apiToken | yes | Your Api Token.

# Steamprofiles

## Get Steamprofiles

```shell
curl "https://rep4rep.com/pub-api/user/steamprofiles" \
  --get \
  --data "apiToken=API_TOKEN_HERE"
```

> Example successful response:

```json
[
  {
    "id": "60f819ddc9250",
    "steamId": "76561190000000000",
    "personaName": "user 1",
    "profileUrl": "https://steamcommunity.com/id/user1/",
    "avatar": "https://avatars.akamai.steamstatic.com/fef49e7fa7e1997310d705b2a6158ff8dc1cdfeb_full.jpg",
    "communityVisibilityState": 3,
    "profileState": 1,
    "commentPermission": 1,
    "canReceiveComment": true
  },
  {
    "id": "66acf813b9d2d",
    "steamId": "76561190000000000",
    "personaName": "user 2",
    "profileUrl": "https://steamcommunity.com/id/user2/",
    "avatar": "https://avatars.akamai.steamstatic.com/fef49e7fa7e1997310d705b2a6158ff8dc1cdfeb_full.jpg",
    "communityVisibilityState": 3,
    "profileState": 1,
    "commentPermission": 1,
    "canReceiveComment": true
  }
]
```

This endpoint retrieves your added steamprofiles and the internal rep4rep steamprofile ID for the steamprofile.

**Important:**
`id` is your internal rep4rep steam ID which is used for other requests.


### HTTP Request

`GET https://rep4rep.com/pub-api/user/steamprofiles`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apiToken | yes | Your Api Token.

## Add Steamprofile

```shell
curl "https://rep4rep.com/pub-api/user/steamprofiles/add" \
  -X POST \
  -d "apiToken=API_TOKEN_HERE" \
  -d "steamProfile=STEAM_PROFILE_ID"
```

> Example successful response:

```json
{
  "success": "Your steam profile was added!"
}
```

This endpoint adds a steamprofile to your rep4rep account.

### HTTP Request

`POST https://rep4rep.com/pub-api/user/steamprofiles/add`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apiToken | yes | Your Api Token.
steamProfile | yes | Steamprofile URL, SteamID64 or Custom ID.


# Tasks

## Get Available Tasks

```shell
curl "https://rep4rep.com/pub-api/tasks" \
  --get \
  --data "apiToken=API_TOKEN_HERE" \
  --data "steamProfile=INTERNAL_REP4REP_PROFILE_ID"
```

> Example successful response:

```json
[
  {
    "taskId": "6154d6727812c",
    "targetSteamProfileId": "76561190000000000",
    "targetSteamProfileName": "user 1",
    "requiredCommentId": 4455,
    "requiredCommentText": "hello"
  },
  {
    "taskId": "660dfdbd1343c",
    "targetSteamProfileId": "76561190000000000",
    "targetSteamProfileName": "user 2",
    "requiredCommentId": 5566,
    "requiredCommentText": "okay"
  },
  ...
]
```

This endpoint returns the available tasks for a given steamprofile.


### HTTP Request

`GET https://rep4rep.com/pub-api/tasks`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apiToken | yes | Your Api Token.
steamProfile | yes | Internal Rep4Rep Steamprofile ID

**To get your internal rep4rep steamprofile ID use [get-steamprofiles](#get-steamprofiles)**


## Update Task as Completed

```shell
curl "https://rep4rep.com/pub-api/tasks/complete" \
  -X POST \
  -d "apiToken=API_TOKEN_HERE" \
  -d "taskId=TASK_ID" \
  -d "commentId=COMMENT_ID" \
  -d "authorSteamProfileId=AUTHOR_INTERNAL_R4R_STEAM_ID"
```

> Example successful response:

```json
{
  "success": "Task marked as complete and will be verified shortly."
}
```

This endpoint marks a task as completed and will be awaiting verification automatically after.

You should call this after a comment has been posted successfully.

### HTTP Request

`POST https://rep4rep.com/pub-api/tasks/complete`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apiToken | yes | Your Api Token.
taskId | yes | Task ID.
commentId | yes | Comment ID.
authorSteamProfileId | yes | Your internal Rep4Rep Steamprofile ID that the comment was posted from.

**To get your internal rep4rep steamprofile ID use [get-steamprofiles](#get-steamprofiles)**
