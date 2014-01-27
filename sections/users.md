Users
========

This API allows you to read users you are connected with in Kona.

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

`GET /userinfo` will return the current user which is useful after authenticating.<a name='userinfo'></a>

See [get response](responses.md#get).