Spaces
========

Get spaces
------------

* `GET /spaces` will return all active spaces.

```json
{
  "spaces":[{
    "id":1,
    "name":"Just for Me",
    "details":"This space allows you to capture reminders that may not fit neatly into a space of their own. For example: 'Remember the dry cleaning', 'Schedule my dentist appointment', or 'Need to buy a gift for the birthday party Brian is attending'.",
    "updated_at":1372091715000,
    "picture_thumb":"/kona/images/defaultproject.png"
    "href":"https://api.kona.com/spaces/1"
  },{
    "id":2,
    "name":"Neightborhood HOA",
    "details":"Where we discuss our nieghborhood",
    "overview":"Please create a new conversation for each HOA issue to be discussed."
    "updated_at":1372091733000,
    "picture_thumb":"/kona/images/defaultproject.png"
    "href":"https://api.kona.com/spaces/2"
  }]
}
```

Get space
-----------

* `GET /spaces/2` will return the specified space.

```json
{
  "spaces": [{
    "id": 2,
    "name":"Neightborhood HOA",
    "details":"Where we discuss our nieghborhood",
    "overview":"Please create a new conversation for each HOA issue to be discussed."
    "updated_at":1372091733000,
    "picture_thumb":"/kona/images/defaultproject.png"
    "href":"https://api.kona.com/spaces/2"
  }]
}
```

Create space
-----------

* `POST /spaces` will return the created space based on the JSON request sent.

```json
{
  "spaces": [{
    "name": "Scout Troop 703",
    "color": "#b373b3",
    "details": "This is the official space for Scout Troop 703",
    "overview": "Anything goes but keep it clean!"
  }]
}
```

Upon success, `201 Created` will be returend with the location of the new space in the `Location` header along with the current JSON representation of the space (See the **Get space** endpoint for more info).


Update space
---------------

* `PUT /spaces/1.json` will update the space from the JSON parameters sent.

```json
{
  "name": "New Space Name",
  "details": "And some new deets",
  "overview": "And some guidance on how to use this space"
}
```

Upon success, `200 OK` will be returend along with the current JSON representation of the space (See the **Get space** endpoint for more info). If the user does not have access to update the space, you'll receive `403 Forbidden`.


Delete space
---------------

* `DELETE /spaces/1.json` will delete the space from the id sent.

Upon success, `204 No Content` will be returend. If the user does not have access to delete the space, you'll receive `403 Forbidden`.
