Risks
========

This API allows you to create, read, update, delete, and manage risks in spaces.
TODO: Disclaimer about needing Risk add-on.

Schema  <a name='schema'></a>
------------
```
{
  "risks": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "location": string,
    "details": string,
    "space_id": integer,
    "creator_id": integer, readonly,
    "participants": [{"user_id": integer, "role": integer }],
    "groups": [{"group_id": integer, "role": integer }],
    "status": integer, {pending: 0, in_progress: 1 [default], complete: 2, group_closed: 3, group_reopened: 4},
    "visibility": integer, {participants_only: 1, public: 2 [default]},
    "stakeholder": integer, {just_me: 0, group: 1 [default], everyone: 2},
    "default_role": integer, {editor: 1, viewer: 2 [default], collaboration_complete: 3}
  }]
}
```

Get risks
------------
`GET /risks` will return all active risks.

`GET /risks/:id` will return the specified risk. See [get response](responses.md#get).

Create risk
-----------
`POST /risks` will return the created risk based on the JSON request sent. See [create response](responses.md#create).

To create a risk for the "Everyone" group, set stakeholder to 2. For example, this will create a risk for space 9 that is assigned to Everyone and is public.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "stakeholder": 2
  }]
}
```

This will create a risk for space 9 that is assigned to group 11 and is private.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "groups": [{"group_id": 11, "role": 1 }],
    "visibility": 1
  }]
}
```

This will create a risk for space 9 with user 14 and group 11 as editors, user 15 and group 12 as viewers and is private.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "participants": [{"user_id": 14, "role": 1},{"user_id":15, "role": 2}],
    "groups": [{"group_id": 11, "role": 1 },{"group_id": 12, "role": 2 }],
    "visibility": 1
  }]
}
```

Delete risk
---------------
`DELETE /risks/:id` will delete the risk from the id sent. See [delete response](responses.md#delete)


