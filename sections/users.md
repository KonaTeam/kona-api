Users
========

This API allows you to read users you are connected with in Kona.

Schema  <a name='schema'></a>
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


Get users <a name='get'></a>
------------
`GET /users` will return all users you are connected with.

### Filter parameters
`GET /users?like=:pattern` will return users whose name or email starts with pattern.

`GET /users?space_id=:space_id` will return users who are members of the given space.

`GET /users/:id` will return the specified user.

`GET /userinfo` will return the current user which is useful after authenticating.<a name='userinfo'></a> It will also include information of the current account and the total unread count of conversations as of the time the API was called. Its response has the following schema
```
  {
  "users": [{
    "id": integer, readonly,
    "email": string, required,
    "name": string,
    "picture_thumb" :string, readonly,
    "updated_at": datetime, readonly,
    "href": string, readonly,
    "current_account": {
        "id": integer,
        "member": boolean
      }
    "unread_conversation_count": integer
    }]
}
```

See [get response](responses.md#get).

External Objects
---------------
You can link external resources to users as well by using the `external_objects` attribute. See the page on [external objects](external_objects.md) for more info. 
