Spaces
========

Most resources in Kona are organized into spaces.  This API allows you to create, read, update, delete, and manage membership to spaces.

When getting or creating a space, it may be useful to receive a list of users that are in that space.  You can do this
by passing `"include": "users"` as a root level JSON node in the request. See [Users schema](users.md#schema).

Schema  <a name='schema'><a>
------------
```
{
  "spaces": [{
    "id": integer, readonly,
    "name": string, required,
    "details": string,
    "welcome_message": string,
    "category": integer, (0=personal, 1=work [default]),
    "updated_at": datetime, readonly,
    "picture_thumb": string, readonly,
    "href": string, readonly
  }]
}
```


Get spaces
------------
`GET /spaces` will return all active spaces.

`GET /spaces/:id` will return the specified space. See [get response](responses.md#get).


Create space
-----------
`POST /spaces` will return the created space based on the JSON request sent. See [create response](responses.md#create).


Update space
---------------
`PUT /spaces/:id` will update the space from the JSON parameters sent. Only attributes in the payload will be updated (patch). See [update response](responses.md#update).


Delete space
---------------
`DELETE /spaces/:id` will delete the space from the id sent. See [delete response](responses.md#delete)


Invite to space
---------------
`POST /spaces/:id/participants` will allow people to be invited to a space.

```json
{
  "participants": [{"email": "frank@example.com"}, {"email": "pete@example.com"}],
  "message": "Welcome to the space!"
}
```

Upon success, `200 OK` will be returned along with a response with the following schema:

```json
{
  "users":[{
    "id": integer,
    "email": string,
    "name": string,
    "status": string (created | existing | resent),
    "invitation_sent": boolean
  }]
}
```

Remove from space
---------------
`DELETE /spaces/:space_id/participants/:user_id` will allow users to be removed from a space.

`DELETE /spaces/:space_id/participants/:email` will allow users to be removed from a space.

Upon success, `204 No Content` will be returned.  If the user does not have permission to revoke a users space membership, you"ll receive `403 Forbidden`.
