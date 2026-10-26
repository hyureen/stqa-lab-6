# API Findings: JSONPlaceholder

Testers (Group 5): ELisa, Nurfarhana, Ammar, Ikmal, Shervona, Nureen
Date: 26 October 2026

Record what the server actually returned for each route. A value
copied from the documentation without being checked against a real
request is not a finding.

## GET /posts/1

| Field        | Value                           |
| ------------ | ------------------------------- |
| Status code  | 200                             |
| Content-Type | application/json; charset=utf-8 |

Response body keys and their types:

| Key    | Type | Example value                                                                |
| ------ | ---- | ---------------------------------------------------------------------------- |
| userId | int  | 1                                                                            |
| id     | int  | 1                                                                            |
| title  | str  | "sunt aut facere repellat provident occaecati excepturi optio reprehenderit" |
| body   | str  | "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum..."      |

## GET /posts

| Field        | Value                           |
| ------------ | ------------------------------- |
| Status code  | 200                             |
| Content-Type | application/json; charset=utf-8 |

Shape of the response body (object or array, and the type of each
element):

A JSON array of 100 elements. Each element is an object with the same
four keys as GET /posts/1: `userId` (int), `id` (int), `title` (str),
`body` (str). The first element is post id 1.

## POST /posts

Request body sent:

```json
{
  "title": "foo",
  "body": "bar",
  "userId": 1
}
```

| Field        | Value                           |
| ------------ | ------------------------------- |
| Status code  | 201                             |
| Content-Type | application/json; charset=utf-8 |

Response body keys and their types:

| Key    | Type | Example value             |
| ------ | ---- | ------------------------- |
| title  | str  | "foo"                     |
| body   | str  | "bar"                     |
| userId | int  | 1                         |
| id     | int  | 101 (added by the server) |

## DELETE /posts/1

| Field        | Value                           |
| ------------ | ------------------------------- |
| Status code  | 200                             |
| Content-Type | application/json; charset=utf-8 |

Response body:

`{}` (an empty JSON object)

## Notes

- Every route returned `Content-Type: application/json; charset=utf-8`,
  not plain `application/json` as in Part 1. A test that asserts only
  `application/json` with `==` fails against this server.
- POST /posts returned 201 and a new id (101), but a follow-up
  GET /posts/101 returned 404. The server accepted the request without
  actually saving the post.
- DELETE /posts/1 returned 200, but a follow-up GET /posts/1 still
  returned 200. The server accepted the request without actually
  deleting the post.
- Conclusion: JSONPlaceholder returns success codes for POST and
  DELETE but does not change any data on the server. A test that
  checks only the status code would pass even though the operation
  never happened. To confirm a write, the resource must be read back
  afterwards.
