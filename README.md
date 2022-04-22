# Project E-commerce API

This repository contains the full code and documentation for [transmit](https://transmit-chat-app.herokuapp.com/chats)’s API. This REST API allows users to receive and send messages through [transmit](https://transmit-chat-app.herokuapp.com/chats) account.

#### Contents

- [Overview](#1-overview)
- [Authentication](#2-authentication)
- [Resources](#3-resources)
  - [Users](#31-users)
  - [Products](#32-products)
- [Architecture Diagram](#4-architecture)
- [Entity Relationship Diagram](#5-erd)

## 1. Overview

This API is a JSON-based OAuth2 bearer token API. The token is generated using JSON Web Token (JWT).

## 2. Authentication

In order to receive and send messages, users will need an access token. An access token grants limited access to a user’s account.

## 3. Resources

The API is RESTful and arranged around resources. All requests must be made with an integration token.

Typically, the first request you make should be to acquire user details. This will confirm that your access token is valid, and give you a user id that you will need for subsequent requests.

### 3.1. Users

#### Getting the authenticated user’s details

Returns details of the user who has granted permission to the application.

Example request:

```
POST /login/
Host: localhost
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2NDQ2NDA4MzEsImV4cCI6MTY0NTI0NTYzMX0.miNjUnu6GssoM2yGP8CLH2ecNMPVpk06-vMfIQBau88
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is a User object id within a data envelope.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
 "user": "621a0a5b4280f7543ca2b871"
}
```

Where a full User object is:

| Field      | Type    | Description                                          |
| ---------- | ------- | ---------------------------------------------------- |
| id         | string  | A unique identifier for the user.                    |
| email      | string  | The user’s email (unique).                           |
| password   | string  | The user’s password,hashed by bcrypt.                |
| pic        | string  | The user’s profile picture (uploaded to cloudinary). |
| isAdmin    | boolean | The user’s status in group chat                      |
| timestamps | date    | The user’s sign up date                              |

Possible errors:

| Error code       | Description                                       |
| ---------------- | ------------------------------------------------- |
| 401 Unauthorized | The `accessToken` is invalid or has been revoked. |

### 3.2. Chats

#### Create, Access, Fetch, Rename Group, Add User, Remove

The users that are logged in can access one on one chats or group chats (using POST method), fetch chats (using GET method), create group (using POST method), rename group(using PUT method), add user to group (using PUT method) and remove user from group (using PUT method).

```
GET /
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

Where a chat object is:

| Field       | Type    | Description                       |
| ----------- | ------- | --------------------------------- |
| id          | string  | A unique identifier for the chat. |
| chatName    | string  | The group chat's name             |
| isGroupChat | boolean | The chat's satus.                 |
| users       | object  | The data from user object.        |

```
POST /group
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

### 3.3. Messages

#### Send and Fetch

The users that are logged in can send messages on one on one chats or group chats (using POST method) and fetch all messages (using GET method).

```
POST /
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

Where a chat object is:

| Field       | Type   | Description                   |
| ----------- | ------ | ----------------------------- |
| sender      | object | The user object.              |
| content     | string | The message.                  |
| chat        | object | The chat object               |
| timepstamps | date   | The time the message is sent. |

```
GET /:chatId
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
