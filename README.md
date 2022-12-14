# BlogHost

This is an api for a blog app

---

## Requirements

Users should have a first_name, last_name, email, password,

A user should be able to sign up and sign in into the blog app

Use JWT as authentication strategy and expire the token after 1 hour

A blog can be in two states; draft and published

Logged in and not logged in users should be able to get a list of published blogs created

Logged in and not logged in users should be able to to get a published blog

Logged in users should be able to create a blog.

When a blog is created, it is in draft state

The owner of the blog should be able to update the state of the blog to published

The owner of a blog should be able to edit the blog in draft or published state

The owner of the blog should be able to delete the blog in draft or published state

The owner of the blog should be able to get a list of their blogs.

The endpoint should be paginated

It should be filterable by state

Blogs created should have title, description, tags, author, timestamp, state, read_count, reading_time and body.

The list of blogs endpoint that can be accessed by both logged in and not logged in users should be paginated:

default it to 20 blogs per page.

It should also be searchable by author, title and tags.

It should also be orderable by read_count, reading_time and timestamp

When a single blog is requested, the api should return the user information (the author) with the blog. The read_count of the blog too should be updated by 1

Come up with any algorithm for calculating the reading_time of the blog.

---

## Development

## Prerequisites

- Nodejs
- Mongodb

---

## Base URL

- somehostsite.com

## Models

---

### User

| field      | data_type | constraints      |
| ---------- | --------- | ---------------- |
| id         | string    | required         |
| username   | string    | required, unique |
| first_name | string    | required         |
| last_name  | string    | required         |
| email      | string    | required, unique |
| password   | string    | required         |

### Blog

| field        | data_type  | constraints                                              |
| ------------ | ---------- | -------------------------------------------------------- |
| title        | string     | required, unique                                         |
| description  | string     | optional                                                 |
| author       | ref - User |                                                          |
| owner        | string     |                                                          |
| state        | string     | required, default: 'draft', enum: ['draft', 'published'] |
| read_count   | Number     | default: 0                                               |
| reading_time | Number     |                                                          |
| tags         | array      | optional                                                 |
| body         | string     | required                                                 |

## APIs

---

### Creating a user

- Route: /bloghostt/signup
- Method: POST

:point_down: Body

```json
{
  "first_name": "Fawaz",
  "last_name": "Aro",
  "username": "sam",
  "email": "arofawaz123@gmail.com",
  "password": "FAWAZ"
}
```

:point_down: Response

```json
{
  "_id": { "$oid": "6364ab2ac194766c15931e95" },
  "first_name": "Fawaz",
  "last_name": "Aro",
  "username": "fawaz",
  "email": "arofawaz123@gmail.com",
  "createdAt": { "$date": { "$numberLong": "1667541802899" } },
  "updatedAt": { "$date": { "$numberLong": "1667541802899" } },
  "__v": { "$numberInt": "0" }
}
```

---

### Login User

- Route: /bloghostt/login
- Method: POST
- Body:

```
{
  "email": "email",
  "password": "password",
}
```

- Responses

Success

```
{
    message: 'Login successful',
}
```

---

### Create Blog

- Route: /bloghostt/blog
- Method: POST
- Header
  - Authorization: cookie {token}

:point_down: Body

```json
{
  "title": "List",
  "description": "PoliticsVerse  @PoliticsVerse ?? 6h LiCompanies who hav",

  "tags": ["Airlines", "America"],
  "state": "published",
  "body": "Skip to comments.\r\nList twiter ^Posted on 11/05/2022 1:45:38 PM PDT by janetjanet998\r\nPoliticsVerse @PoliticsVerse ?? 6h List of Companies who have suspended Twitter Ads: \r\nGeneral Mills CVS United Ai??? [+1616 chars]"
}
```

:point_down: Response

```json
{
  "status": "true",
  "data": {
    "title": "List",
    "description": "PoliticsVerse  @PoliticsVerse ?? 6h LiCompanies who hav",
    "author": "6367c296ba7522bd8561e4f6",
    "state": "draft",
    "read_count": 0,
    "tags": ["Airlines", "America"],
    "body": "Skip to comments.\r\nList twiter ^Posted on 11/05/ 2022 1:45:38 PM PDT by janetjanet998\r\nPoliticsVerse @PoliticsVerse ?? 6h List of Companies who have suspended Twitter Ads: \r\nGeneral Mills CVS United Ai??? [+1616 chars]",
    "_id": "6367cc2271c384885108032f",
    "createdAt": "2022-11-06T15:00:50.202Z",
    "updatedAt": "2022-11-06T15:00:50.202Z",
    "reading_time": 2
  }
}
```

## Getting post all blogs by a logged in user

- Route: /blogs/user
- Method: get

-- This method automatically get the id by a logged in user an get all post by the specific user (draft and public)

```
````

## Update the contents of a Blog

- Route: /bloghostt/blog/:id
- Method: PATCH
- Header
- Authorization: cookie{token}
- Requirement: User must be logged

:point_down: Body

```json
{
  "state": "published",
  "tags": ["memoirs", "expose"],
  "body": "A very fun article that is long enough to be fun, and short enough to be ..fun! A sailor went to sea to see what he could see but all that he could see was the bottom of the deep blue sea."
}
```

:point_down: Response

```json
{
  "status": "true",
  "data": {
    "_id": "6367cc2271c384885108032f",
    "title": "List",
    "description": "PoliticsVerse  @PoliticsVerse ?? 6h LiCompanies who hav",
    "author": "6367c296ba7522bd8561e4f6",
    "state": "published",
    "read_count": 0,
    "tags": ["memoirs", "expose"],
    "body": "A very fun article that is long enough to be fun, and short enough to be ..fun! A sailor went to sea to see what he could see but all that he could see was the bottom of the deep blue sea.",
    "createdAt": "2022-11-06T15:00:50.202Z",
    "updatedAt": "2022-11-06T16:22:29.326Z",
    "reading_time": 1
  }
}
```

### Get all Blogs

- Route: /bloghostt/blogs
- Method: get

:point_down: Body

-Get all published blogs
-filter by tags  
-filter by title

:point_down: Response

```json
{
  "status": "true",
  "data": {
    "_id": "6367cc2271c384885108032f",
    "title": "The Adventures of John",
    "description": "Fun times as Johnny",
    "author": "6367c296ba7522bd8561e4f6",
    "state": "published",
    "read_count": 0,
    "tags": ["memoirs", "expose"],
    "body": "A very fun article that is long enough to be fun, and short enough to be ..fun! A sailor went to sea to see what he could see but all that he could see was the bottom of the deep blue sea.",
    "createdAt": "2022-11-06T15:00:50.202Z",
    "updatedAt": "2022-11-06T16:22:29.326Z",
    "reading_time": 1
  }
}
```

## Delete Blog

- Route: bloghostt/blog/:id
- Method: Delete
- Requirement: User must be logged in

- Delete the blog with the specified id, and returns the deleted blog

---

## Contributor

- Fawaz Aro
