Groups
========

This API allows you to create, read, update, delete, and manage membership to groups within a space.

Schema  <a name='schema'><a>
------------
```
{
  "groups": [{
    "id": integer, readonly,
    "space_id" : integer, required on create, readonly otherwise,
    "name": string, required,
    "updated_at": datetime, readonly,
    "href": string, readonly,
    "user_ids": array of string
  }]
}
```


Get groups
------------
`GET /groups` will return all active groups.

`GET /groups?space_id=:space_id` will return all active groups for :space_id.

`GET /groups/:id` will return the specified group. See [get response](responses.md#get).


Create group
-----------
`POST /groups` will return the created group based on the JSON request sent. See [create response](responses.md#create).


Update group
---------------
`PUT /groups/:id` will update the group from the JSON parameters sent. Only attributes in the payload will be updated (patch).
The collection of user_ids will be used for all group membership. To add or remove members, pass up the new complete list of user_ids.
See [update response](responses.md#update).


Delete group
---------------
`DELETE /groups/:id` will delete the group from the id sent. See [delete response](responses.md#delete)


