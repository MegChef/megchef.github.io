---
title: "REST API Examples (With Screenshots)"
weight: 4
---

## REST API Examples

Below are real executions of the endpoints using cURL along with screenshots.

---

## Register User

```bash
curl -X POST http://localhost:3000/api/users/register \
  -H "Content-Type: application/json" \
  -d '{"username":"root","password":"sillypassword"}'
```

## Login

```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"alice","password":"password123"}'
```

## Send friend request

```bash
curl -X POST http://localhost:3000/api/friends/request/OTHER_USER_ID \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## View friend request

```bash
curl -X GET http://localhost:3000/api/friends/requests \
  -H "Authorization: Bearer <bob_token>"
```

## Accept friend request as bob

```bash
curl -X POST http://localhost:3000/api/friends/accept/REQUEST_ID \
  -H "Authorization: Bearer <bob_token>"
```

## View friends

```bash
curl -X GET http://localhost:3000/api/friends \
  -H "Authorization: Bearer <bob_token>"
```

## Remove a friend

```bash
curl -X DELETE http://localhost:3000/api/friends/USER_ID \
  -H "Authorization: Bearer <alice_token>"
```

## Reject friend request

```bash
curl -X POST http://localhost:3000/api/friends/reject/REQUEST_ID \
  -H "Authorization: Bearer <bob_TOKEN>"
```

## Delete own profile

```bash
curl -X DELETE http://localhost:3000/api/users/me \
  -H "Authorization: Bearer <USER_TOKEN>"
```

## Change password

```bash
curl -X POST http://localhost:3000/api/users/me/password \
  -H "Authorization: Bearer <USER_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"password":"newpassword123"}'
```

## User logs out

```bash
curl -X POST http://localhost:3000/api/auth/logout \
  -H "Authorization: Bearer <USER_TOKEN>"
```

## View users (admin only):

```bash
curl -X GET http://localhost:3000/api/users \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Admin deletes user

```bash
curl -X DELETE http://localhost:3000/api/users/USER_ID \
  -H "Authorization: Bearer <ADMIN_TOKEN>"
```
