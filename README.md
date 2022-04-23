# Transmit (Chat App) API :speech_balloon:

This repository contains the full code and documentation for [transmit](https://transmit-chat-app.herokuapp.com)’s API. This REST API allows users to receive and send messages through [transmit](https://transmit-chat-app.herokuapp.com) account. The frontend side can be accessed [here](https://github.com/Sahlan009/FP_Frontend).

#### Contents :bookmark:

- [Overview](#1-overview)
- [Authentication](#2-authentication)
- [Resources](#3-resources)
  - [Users](#31-users)
  - [Chats](#32-chats)
  - [Messages](#33-messages)

## 1. Overview :bulb:

This API is a JSON-based OAuth2 bearer token API. The token is generated using JSON Web Token (JWT).

## 2. Authentication :closed_lock_with_key:

In order to receive and send messages, users will need an access token. An access token grants limited access to a user’s account.

## 3. Resources :open_file_folder:

The API is RESTful and arranged around resources. All requests must be made with an integration token.

Typically, the first request you make should be to acquire user details. This will confirm that your access token is valid, and give you a user id that you will need for subsequent requests.

### 3.1. Users

#### User Registration

Register user to get an access token.

Example request:

```
POST /api/user
{
    "name": "Shinichi Kudo",
    "email": "kudos@example.com",
    "password": "bakerstreet221b"
}
```

The response is a User object (id, name, email, pic, token) within a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "_id": "624fce32a08e9443f1ab833e",
    "name": "Shinichi Kudo",
    "email": "kudos@example.com",
    "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjYyNGZjZTMyYTA4ZTk0NDNmMWFiODMzZSIsImlhdCI6MTY1MDY2ODAzMiwiZXhwIjoxNjUzMjYwMDMyfQ.d-mpYlxkQIY5Wmp2VL5gWjxNp1B6_sugVd2D-bJWofg"
}
```

Where a full User object is:

| Field      | Type    | Description                                                         |
| ---------- | ------- | ------------------------------------------------------------------- |
| id         | string  | A unique identifier for the user.                                   |
| email      | string  | The user’s email (unique).                                          |
| password   | string  | The user’s password, hashed by bcrypt.                              |
| pic        | string  | The user’s profile picture (link from uploaded file in cloudinary). |
| isAdmin    | boolean | The user’s status in group chat                                     |
| timestamps | date    | The created at and updated at time                                  |

#### Getting the authenticated user’s details

Returns details of the user who has granted permission to the application.

Example request:

```
POST /api/user/login
{
    "email": "kudos@example.com",
    "password": "bakerstreet221b"
}
```

The response is a User object (id, name, email, pic, token) within a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "_id": "624fce32a08e9443f1ab833e",
    "name": "Shinichi Kudo",
    "email": "kudos@example.com",
    "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjYyNGZjZTMyYTA4ZTk0NDNmMWFiODMzZSIsImlhdCI6MTY1MDY2ODAzMiwiZXhwIjoxNjUzMjYwMDMyfQ.d-mpYlxkQIY5Wmp2VL5gWjxNp1B6_sugVd2D-bJWofg"
}
```

Where a full User object is:

| Field      | Type    | Description                                                         |
| ---------- | ------- | ------------------------------------------------------------------- |
| id         | string  | A unique identifier for the user.                                   |
| email      | string  | The user’s email (unique).                                          |
| password   | string  | The user’s password, hashed by bcrypt.                              |
| pic        | string  | The user’s profile picture (link from uploaded file in cloudinary). |
| isAdmin    | boolean | The user’s status in group chat                                     |
| timestamps | date    | The created at and updated at time                                  |

Possible errors:

| Error code       | Description                                       |
| ---------------- | ------------------------------------------------- |
| 401 Unauthorized | The `accessToken` is invalid or has been revoked. |

#### Search User

The users that are logged in can search other users using GET method and query paramter.

```
GET /api/user?search=w
Host: localhost
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2NDQ2NDA4MzEsImV4cCI6MTY0NTI0NTYzMX0.miNjUnu6GssoM2yGP8CLH2ecNMPVpk06-vMfIQBau88
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is a list of other users that contain the inputted letter or words in their names or emails. The response is wrapped in a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8


    {
        "isAdmin": false,
        "_id": "624ff6a5b93668500dd99e3b",
        "name": "William Moriarty",
        "email": "willmoriarty@example.com",
        "password": "$2a$10$He7NWX0/3KpUbH05XrRXt.w7hzUeMv7PcULBjzinw25a/HZ7qE5Si",
        "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
        "createdAt": "2022-04-08T08:47:33.067Z",
        "updatedAt": "2022-04-08T08:47:33.067Z",
        "__v": 0
    },
    {
        "isAdmin": false,
        "_id": "624ff9c4b93668500dd99e41",
        "name": "Pawaret Sermrittirong",
        "email": "pang@gifted.com",
        "password": "$2a$10$DuPBT9j.DsWTTf68fO70LetE1SEbWnZxxBFa/aYZWL0Lal.7nfni.",
        "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg",
        "createdAt": "2022-04-08T09:00:52.902Z",
        "updatedAt": "2022-04-08T09:00:52.902Z",
        "__v": 0
    },
    {
        "isAdmin": false,
        "_id": "624ffa1bb93668500dd99e44",
        "name": "Wasuthorn Worachotmethee",
        "email": "wave@gifted.com",
        "password": "$2a$10$8YkSZ3WFZsuXOVUJUCpktenNrpdgGnVsnPb.3tVKXHt6AEtcwslnS",
        "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
        "createdAt": "2022-04-08T09:02:19.446Z",
        "updatedAt": "2022-04-08T09:02:19.446Z",
        "__v": 0
    },
    {
        "isAdmin": false,
        "_id": "625049493175d81628338aea",
        "name": "Wichai Saingern",
        "email": "ohm@gifted.com",
        "password": "$2a$10$lB90DDij2ResC8aIAe3xrOm7QREPWjhyoZi9m/vF3z9g1q6Mn8vn.",
        "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649428806/xwz3saif6rmheu6oc9zn.jpg",
        "createdAt": "2022-04-08T14:40:09.050Z",
        "updatedAt": "2022-04-08T14:40:09.050Z",
        "__v": 0
    }
]
```

### 3.2. Chats

#### Create Chats

The users that are logged in can create and access one on one chats or group chats (using POST method).

```
POST /api/chat
{
    "userId":"624ffa1bb93668500dd99e44"
}
```

The response is a chat object. This endpoint will return all chats when the user is logged in.

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "_id": "625acb3cfe85dbb000392ae6",
        "chatName": "sender",
        "isGroupChat": false,
        "users": [
            {
                "isAdmin": false,
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T05:54:58.722Z",
                "updatedAt": "2022-04-08T05:54:58.722Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ffa1bb93668500dd99e44",
                "name": "Wasuthorn Worachotmethee",
                "email": "wave@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
                "createdAt": "2022-04-08T09:02:19.446Z",
                "updatedAt": "2022-04-08T09:02:19.446Z",
                "__v": 0
            }
        ],
        "createdAt": "2022-04-16T13:57:16.678Z",
        "updatedAt": "2022-04-16T13:58:06.195Z",
        "__v": 0,
        "latestMessage": {
            "_id": "625acb6efe85dbb000392aed",
            "sender": {
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
            },
            "content": "Mic check 1 2",
            "chat": "625acb3cfe85dbb000392ae6",
            "createdAt": "2022-04-16T13:58:06.116Z",
            "updatedAt": "2022-04-16T13:58:06.116Z",
            "__v": 0
            }
    }
]
```

Where a chat object is:

| Field         | Type    | Description                        |
| ------------- | ------- | ---------------------------------- |
| id            | string  | A unique identifier for the chat.  |
| chatName      | string  | The group chat's name              |
| isGroupChat   | boolean | The chat's satus.                  |
| users         | object  | The data from user object.         |
| latestMessage | object  | The data from message object.      |
| groupAdmin    | object  | The data from user object.         |
| timestamps    | date    | The created at and updated at time |

#### Fetch Chats

The users that are logged in can fetch chats (using GET method).

```
GET /api/chat
```

The response is a list of chats. The response array is wrapped in a data envelope. This endpoint will return all chats when the user is logged in.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "_id": "625cf17f31a235b1a95d4a47",
        "chatName": "Test Group Chat",
        "isGroupChat": true,
        "users": [
            {
                "isAdmin": false,
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T05:54:58.722Z",
                "updatedAt": "2022-04-08T05:54:58.722Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ff9c4b93668500dd99e41",
                "name": "Pawaret Sermrittirong",
                "email": "pang@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg",
                "createdAt": "2022-04-08T09:00:52.902Z",
                "updatedAt": "2022-04-08T09:00:52.902Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624da1df02019b2af2112076",
                "name": "Enola Holmes",
                "email": "enola@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-06T14:21:19.424Z",
                "updatedAt": "2022-04-06T14:21:19.424Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ff7f2b93668500dd99e3e",
                "name": "Yuuji Itadori",
                "email": "yuuji@example.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649407911/jj35puyk2iunpumkr178.jpg",
                "createdAt": "2022-04-08T08:53:06.560Z",
                "updatedAt": "2022-04-08T08:53:06.560Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ffa1bb93668500dd99e44",
                "name": "Wasuthorn Worachotmethee",
                "email": "wave@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
                "createdAt": "2022-04-08T09:02:19.446Z",
                "updatedAt": "2022-04-08T09:02:19.446Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "625049493175d81628338aea",
                "name": "Wichai Saingern",
                "email": "ohm@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649428806/xwz3saif6rmheu6oc9zn.jpg",
                "createdAt": "2022-04-08T14:40:09.050Z",
                "updatedAt": "2022-04-08T14:40:09.050Z",
                "__v": 0
            },
            {
                "_id": "625cf10731a235b1a95d49e3",
                "name": "Admin",
                "email": "admin@example.com",
                "pic": "http://res.cloudinary.com/dujxgzag1/image/upload/v1650258179/ze9zonbkx2hytquvozvy.jpg",
                "isAdmin": false,
                "createdAt": "2022-04-18T05:03:03.417Z",
                "updatedAt": "2022-04-18T05:03:03.417Z",
                "__v": 0
            }
        ],
        "groupAdmin": {
            "_id": "625cf10731a235b1a95d49e3",
            "name": "Admin",
            "email": "admin@example.com",
            "pic": "http://res.cloudinary.com/dujxgzag1/image/upload/v1650258179/ze9zonbkx2hytquvozvy.jpg",
            "isAdmin": false,
            "createdAt": "2022-04-18T05:03:03.417Z",
            "updatedAt": "2022-04-18T05:03:03.417Z",
            "__v": 0
        },
        "createdAt": "2022-04-18T05:05:03.546Z",
        "updatedAt": "2022-04-22T14:42:24.979Z",
        "__v": 0,
        "latestMessage": {
            "_id": "6262becf0bacb548ce387d6a",
            "sender": {
                "_id": "624ffa1bb93668500dd99e44",
                "name": "Wasuthorn Worachotmethee",
                "email": "wave@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg"
            },
            "content": "Check",
            "chat": "625cf17f31a235b1a95d4a47",
            "createdAt": "2022-04-22T14:42:23.985Z",
            "updatedAt": "2022-04-22T14:42:23.985Z",
            "__v": 0
        }
    },
]
```

#### Create a Group Chat

The users that are logged in can create a group (using POST method).

```
POST /api/chat/group
{
    "name":"Ad Astra Sapientiam",
    "users": "[\"624ff9c4b93668500dd99e41\", \"624ffa1bb93668500dd99e44\", \"625049493175d81628338aea\"]"
}
```

This endpoint will create a group chat when the user is logged in with the user as the admin. The response array is wrapped in a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
[
    {
    "_id": "62632470cc8748cabc3ae9a7",
    "chatName": "Ad Astra Sapientiam",
    "isGroupChat": true,
    "users": [
        {
            "isAdmin": false,
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg",
            "createdAt": "2022-04-08T09:00:52.902Z",
            "updatedAt": "2022-04-08T09:00:52.902Z",
            "__v": 0
        },
        {
            "isAdmin": false,
            "_id": "624ffa1bb93668500dd99e44",
            "name": "Wasuthorn Worachotmethee",
            "email": "wave@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
            "createdAt": "2022-04-08T09:02:19.446Z",
            "updatedAt": "2022-04-08T09:02:19.446Z",
            "__v": 0
        },
        {
            "isAdmin": false,
            "_id": "625049493175d81628338aea",
            "name": "Wichai Saingern",
            "email": "ohm@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649428806/xwz3saif6rmheu6oc9zn.jpg",
            "createdAt": "2022-04-08T14:40:09.050Z",
            "updatedAt": "2022-04-08T14:40:09.050Z",
            "__v": 0
        },
        {
            "isAdmin": true,
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
            "createdAt": "2022-04-08T05:54:58.722Z",
            "updatedAt": "2022-04-08T05:54:58.722Z",
            "__v": 0
        }
    ],
    "groupAdmin": {
        "isAdmin": true,
        "_id": "624fce32a08e9443f1ab833e",
        "name": "Shinichi Kudo",
        "email": "kudos@example.com",
        "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
        "createdAt": "2022-04-08T05:54:58.722Z",
        "updatedAt": "2022-04-08T05:54:58.722Z",
        "__v": 0
    },
    "createdAt": "2022-04-22T21:56:00.562Z",
    "updatedAt": "2022-04-22T21:56:00.562Z",
    "__v": 0
}
]
```

Possible errors:

| Error code       | Description                                                                  |
| ---------------- | ---------------------------------------------------------------------------- |
| 401 Unauthorized | The `accessToken` is invalid, lacks the `isAdmin` scope or has been revoked. |

#### Rename a Group Chat

The users that are logged in can rename a group (using PUT method).

```
PUT /api/chat/rename
{
    "chatId": "62504a3c3175d81628338aef",
    "chatName": "Too Cool For School"
}
```

This endpoint will rename a group chat. The response array is wrapped in a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
[
   {
        "_id": "62504a3c3175d81628338aef",
        "chatName": "Too Cool For School",
        "isGroupChat": true,
        "users": [
            {
                "isAdmin": false,
                "_id": "624ff9c4b93668500dd99e41",
                "name": "Pawaret Sermrittirong",
                "email": "pang@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg",
                "createdAt": "2022-04-08T09:00:52.902Z",
                "updatedAt": "2022-04-08T09:00:52.902Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ffa1bb93668500dd99e44",
                "name": "Wasuthorn Worachotmethee",
                "email": "wave@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
                "createdAt": "2022-04-08T09:02:19.446Z",
                "updatedAt": "2022-04-08T09:02:19.446Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "625049493175d81628338aea",
                "name": "Wichai Saingern",
                "email": "ohm@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649428806/xwz3saif6rmheu6oc9zn.jpg",
                "createdAt": "2022-04-08T14:40:09.050Z",
                "updatedAt": "2022-04-08T14:40:09.050Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T05:54:58.722Z",
                "updatedAt": "2022-04-08T05:54:58.722Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ff6a5b93668500dd99e3b",
                "name": "William Moriarty",
                "email": "willmoriarty@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T08:47:33.067Z",
                "updatedAt": "2022-04-08T08:47:33.067Z",
                "__v": 0
            }
        ],
        "groupAdmin": {
            "isAdmin": false,
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
            "createdAt": "2022-04-08T05:54:58.722Z",
            "updatedAt": "2022-04-08T05:54:58.722Z",
            "__v": 0
        },
        "createdAt": "2022-04-08T14:44:12.848Z",
        "updatedAt": "2022-04-22T22:46:41.848Z",
        "__v": 0,
        "latestMessage": "625cef9931a235b1a95d49cb"
    }
]
```

#### Add a User to a Group Chat

The users that are admins can add other users to a group (using PUT method).

```
PUT /api/chat/groupadd
{
    "chatId": "62504a3c3175d81628338aef",
    "userId": "624ff6a5b93668500dd99e3b"
}
```

This endpoint will add other users to a group chat. The response array is wrapped in a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
[
   {
        "_id": "62504a3c3175d81628338aef",
        "chatName": "Too Cool For School",
        "isGroupChat": true,
        "users": [
            {
                "isAdmin": false,
                "_id": "624ff9c4b93668500dd99e41",
                "name": "Pawaret Sermrittirong",
                "email": "pang@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9{
        "_id": "62504a3c3175d81628338aef",
        "chatName": "Too Cool For School",
        "isGroupChat": true,
        "users": [
            {
                "isAdmin": false,
                "_id": "624ff9c4b93668500dd99e41",
                "name": "Pawaret Sermrittirong",
                "email": "pang@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg",
                "createdAt": "2022-04-08T09:00:52.902Z",
                "updatedAt": "2022-04-08T09:00:52.902Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ffa1bb93668500dd99e44",
                "name": "Wasuthorn Worachotmethee",
                "email": "wave@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
                "createdAt": "2022-04-08T09:02:19.446Z",
                "updatedAt": "2022-04-08T09:02:19.446Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "625049493175d81628338aea",
                "name": "Wichai Saingern",
                "email": "ohm@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649428806/xwz3saif6rmheu6oc9zn.jpg",
                "createdAt": "2022-04-08T14:40:09.050Z",
                "updatedAt": "2022-04-08T14:40:09.050Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T05:54:58.722Z",
                "updatedAt": "2022-04-08T05:54:58.722Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ff6a5b93668500dd99e3b",
                "name": "William Moriarty",
                "email": "willmoriarty@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T08:47:33.067Z",
                "updatedAt": "2022-04-08T08:47:33.067Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ff6a5b93668500dd99e3b",
                "name": "William Moriarty",
                "email": "willmoriarty@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T08:47:33.067Z",
                "updatedAt": "2022-04-08T08:47:33.067Z",
                "__v": 0
            }
        ],
        "groupAdmin": {
            "isAdmin": false,
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
            "createdAt": "2022-04-08T05:54:58.722Z",
            "updatedAt": "2022-04-08T05:54:58.722Z",
            "__v": 0
        },
        "createdAt": "2022-04-08T14:44:12.848Z",
        "updatedAt": "2022-04-22T22:49:21.847Z",
        "__v": 0,
        "latestMessage": "625cef9931a235b1a95d49cb"
    }
]
```

#### Remove a User from a Group Chat

The users that are admins can remove other users from a group (using PUT method).

```
PUT /api/chat/groupremove
{
    "chatId": "62504a3c3175d81628338aef",
    "userId": "624ff6a5b93668500dd99e3b"
}
```

This endpoint will add other users to a group chat. The response array is wrapped in a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
[
   {
        "_id": "62504a3c3175d81628338aef",
        "chatName": "Too Cool For School",
        "isGroupChat": true,
        "users": [
            {
                "isAdmin": false,
                "_id": "624ff9c4b93668500dd99e41",
                "name": "Pawaret Sermrittirong",
                "email": "pang@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg",
                "createdAt": "2022-04-08T09:00:52.902Z",
                "updatedAt": "2022-04-08T09:00:52.902Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624ffa1bb93668500dd99e44",
                "name": "Wasuthorn Worachotmethee",
                "email": "wave@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408534/pzaqfjg1hphosobx1bdj.jpg",
                "createdAt": "2022-04-08T09:02:19.446Z",
                "updatedAt": "2022-04-08T09:02:19.446Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "625049493175d81628338aea",
                "name": "Wichai Saingern",
                "email": "ohm@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649428806/xwz3saif6rmheu6oc9zn.jpg",
                "createdAt": "2022-04-08T14:40:09.050Z",
                "updatedAt": "2022-04-08T14:40:09.050Z",
                "__v": 0
            },
            {
                "isAdmin": false,
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
                "createdAt": "2022-04-08T05:54:58.722Z",
                "updatedAt": "2022-04-08T05:54:58.722Z",
                "__v": 0
            }
        ],
        "groupAdmin": {
            "isAdmin": false,
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg",
            "createdAt": "2022-04-08T05:54:58.722Z",
            "updatedAt": "2022-04-08T05:54:58.722Z",
            "__v": 0
        },
        "createdAt": "2022-04-08T14:44:12.848Z",
        "updatedAt": "2022-04-22T22:52:08.925Z",
        "__v": 0,
        "latestMessage": "625cef9931a235b1a95d49cb"
    }
]
```

### 3.3. Messages

#### Send Messages

The users that are logged in can send messages on one on one chats or group chats (using POST method).

```
POST /api/message
{
    "content": "Coffe or cake?",
    "chatId": "625ac3b66d3550322baa5d07"
}
```

This endpoint will send a message from the logged in user to another user. The response array is wrapped in a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
    "sender": {
        "_id": "624fce32a08e9443f1ab833e",
        "name": "Shinichi Kudo",
        "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
    },
    "content": "Coffe or cake?",
    "chat": {
        "_id": "625ac3b66d3550322baa5d07",
        "chatName": "sender",
        "isGroupChat": false,
        "users": [
            {
                "_id": "624fce32a08e9443f1ab833e",
                "name": "Shinichi Kudo",
                "email": "kudos@example.com",
                "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
            },
            {
                "_id": "624ff9c4b93668500dd99e41",
                "name": "Pawaret Sermrittirong",
                "email": "pang@gifted.com",
                "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
            }
        ],
        "createdAt": "2022-04-16T13:25:10.591Z",
        "updatedAt": "2022-04-18T13:29:14.362Z",
        "__v": 0,
        "latestMessage": "625d67aa31a235b1a95d4aac"
    },
    "_id": "62632550cc8748cabc3ae9b1",
    "createdAt": "2022-04-22T21:59:44.043Z",
    "updatedAt": "2022-04-22T21:59:44.043Z",
    "__v": 0
    }
]
```

Where a message object is:

| Field       | Type   | Description                   |
| ----------- | ------ | ----------------------------- |
| sender      | object | The data from user object.    |
| content     | string | The message.                  |
| chat        | object | The data from chat object.    |
| timepstamps | date   | The time the message is sent. |

#### Fetch Messages

The users that are logged in can fetch all messages (using GET method).

```
GET /api/message/:chatId
```

The response is a list of messages in a chat. The response array is wrapped in a data envelope. This endpoint will return all messages when the user is logged in.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "_id": "625ae096fe85dbb000392b69",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Coffee or cake?",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T15:28:22.402Z",
        "updatedAt": "2022-04-16T15:28:22.402Z",
        "__v": 0
    },
    {
        "_id": "625ae0bffe85dbb000392b7b",
        "sender": {
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
        },
        "content": "Coffee",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T15:29:03.393Z",
        "updatedAt": "2022-04-16T15:29:03.393Z",
        "__v": 0
    },
    {
        "_id": "625aeb0703d7ff68646ed501",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Nice choice",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T16:12:55.551Z",
        "updatedAt": "2022-04-16T16:12:55.551Z",
        "__v": 0
    },
    {
        "_id": "625aeeb903d7ff68646ed529",
        "sender": {
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
        },
        "content": "Why so sudden?",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T16:28:41.584Z",
        "updatedAt": "2022-04-16T16:28:41.584Z",
        "__v": 0
    },
    {
        "_id": "625aef1c03d7ff68646ed53b",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Just wondering",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T16:30:20.541Z",
        "updatedAt": "2022-04-16T16:30:20.541Z",
        "__v": 0
    },
    {
        "_id": "625af040366a0d284fdf34f8",
        "sender": {
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
        },
        "content": "The new netflix movie right?",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T16:35:12.220Z",
        "updatedAt": "2022-04-16T16:35:12.220Z",
        "__v": 0
    },
    {
        "_id": "625af18e3b9300ce5eeb47ac",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Right!",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T16:40:46.931Z",
        "updatedAt": "2022-04-16T16:40:46.931Z",
        "__v": 0
    },
    {
        "_id": "625af1bd3b9300ce5eeb47c5",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "I think there's something wrong with the code",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-16T16:41:33.775Z",
        "updatedAt": "2022-04-16T16:41:33.775Z",
        "__v": 0
    },
    {
        "_id": "625cef6131a235b1a95d4984",
        "sender": {
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
        },
        "content": "Check",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-18T04:56:01.641Z",
        "updatedAt": "2022-04-18T04:56:01.641Z",
        "__v": 0
    },
    {
        "_id": "625cef6831a235b1a95d498b",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Received well",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-18T04:56:08.498Z",
        "updatedAt": "2022-04-18T04:56:08.498Z",
        "__v": 0
    },
    {
        "_id": "625d665a31a235b1a95d4a8e",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Hey",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-18T13:23:38.144Z",
        "updatedAt": "2022-04-18T13:23:38.144Z",
        "__v": 0
    },
    {
        "_id": "625d675231a235b1a95d4a9b",
        "sender": {
            "_id": "624ff9c4b93668500dd99e41",
            "name": "Pawaret Sermrittirong",
            "email": "pang@gifted.com",
            "pic": "http://res.cloudinary.com/dwusgfjtk/image/upload/v1649408452/ilenigem9dal1grjjggf.jpg"
        },
        "content": "Guess what",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-18T13:27:46.523Z",
        "updatedAt": "2022-04-18T13:27:46.523Z",
        "__v": 0
    },
    {
        "_id": "625d67aa31a235b1a95d4aac",
        "sender": {
            "_id": "624fce32a08e9443f1ab833e",
            "name": "Shinichi Kudo",
            "email": "kudos@example.com",
            "pic": "https://icon-library.com/images/anonymous-avatar-icon/anonymous-avatar-icon-25.jpg"
        },
        "content": "What?",
        "chat": {
            "_id": "625ac3b66d3550322baa5d07",
            "chatName": "sender",
            "isGroupChat": false,
            "users": [
                "624fce32a08e9443f1ab833e",
                "624ff9c4b93668500dd99e41"
            ],
            "createdAt": "2022-04-16T13:25:10.591Z",
            "updatedAt": "2022-04-18T13:29:14.362Z",
            "__v": 0,
            "latestMessage": "625d67aa31a235b1a95d4aac"
        },
        "createdAt": "2022-04-18T13:29:14.288Z",
        "updatedAt": "2022-04-18T13:29:14.288Z",
        "__v": 0
    }
]
```

Where a chat object is:

| Field       | Type    | Description                       |
| ----------- | ------- | --------------------------------- |
| id          | string  | A unique identifier for the chat. |
| chatName    | string  | The group chat's name             |
| isGroupChat | boolean | The chat's satus.                 |
| users       | object  | The data from user object.        |

Possible errors:

| Error code       | Description                                                                  |
| ---------------- | ---------------------------------------------------------------------------- |
| 401 Unauthorized | The `accessToken` is invalid, lacks the `isAdmin` scope or has been revoked. |
