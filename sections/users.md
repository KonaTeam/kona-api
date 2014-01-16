Users
========

This API allows you to read users you are connected with in Kona.

When getting or creating a space, it may be useful to receive a list of users that are in that space.  You can do this by passing `"include": "users"` as a root level JSON node in the request.

Schema  <a name='schema'><a>
------------
```
{
  "users": [{
    "id": integer, readonly,
    "email": string, required,
    "name": string,
    "picture_thumb" :string, readonly,
    "updated_at": datetime, readonly,
    "href": string, readonly
  }]
}
```


Get users
------------
`GET /users` will return all users you are connected with.  You may specify 'like=[string]' to match limited users.

`GET /users/:id` will return the specified user.

`GET /users/self` will return the current user which is useful after authenticating.<a name='self'></a>

See [get response](responses.md#get).