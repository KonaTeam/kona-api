Conversations
========

This API allows you to create, read, delete, and manage conversations in spaces.

Schema  <a name='schema'></a>
------------
```
{
  "conversations": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "details": string,
    "space_id": integer,
    "creator_id": integer, readonly,
    "participants": [{"user_id": integer, "role": integer }],
    "groups": [{"group_id": integer, "role": integer }],
    "stakeholder": integer, {just_me: 0, group: 1 [default], everyone: 2},
    "default_role": integer, {editor: 1, viewer: 2 [default], collaboration_complete: 3},
    "tags": string array, readonly,
    "initial_comment", string, create only
  }]
}
```

Note: Conversations are always visible to the participants only.

Get conversations
------------
`GET /conversations` will return all active conversations.

### Sort parameters
`GET /conversations?order_by=created_at` to sort the result by create date from the latest to the oldest.

`GET /conversations?order_by=updated_at` to sort the result from the latest to the oldest based on when the calendar event's metadata (i.e name, notes, start date etc.) was last updated.

### Filter parameters
`GET /conversations?space_id=:space_id` will return all active conversations for :space_id.

`GET /conversations/:id` will return the specified conversation. See [get response](responses.md#get).

Create conversation
-----------
`POST /conversations` will return the created conversation based on the JSON request sent. See [create response](responses.md#create).


To create a conversation for the "Everyone" group, set stakeholder to 2. For example, this will create a conversation for space 9 that
is assigned to Everyone and is public with an initial comment.
```
{
  "conversations": [{
    "name": "new conversation",
    "space_id": 9,
    "stakeholder": 2,
    "initial_comment": "mitigation description goes here"
  }]
}
```

This will create a conversation for space 9 that is assigned to group 11.
```
{
  "conversations": [{
    "name": "new conversation",
    "space_id": 9,
    "groups": [{"group_id": 11, "role": 1 }]
  }]
}
```

This will create a conversation for space 9 with user 14 and group 11 as editors, user 15 and group 12 as viewers.
```
{
  "conversations": [{
    "name": "new conversation",
    "space_id": 9,
    "participants": [{"user_id": 14, "role": 1},{"user_id":15, "role": 2}],
    "groups": [{"group_id": 11, "role": 1 },{"group_id": 12, "role": 2 }]
  }]
}
```

Delete conversation
---------------
`DELETE /conversations/:id` will delete the conversation from the id sent. See [delete response](responses.md#delete)

External Objects
---------------
You can link external resources to conversations by using the `external_objects` attribute. See the page on [external objects](external_objects.md) for more info. 