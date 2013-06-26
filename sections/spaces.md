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
    "id": 5,
    "name": "Evil League of Evil Network",
    "details": "Dedicate to lies, injustice and the criminal way",
    "updated_at": 1372091746000,
    "picture_thumb": "/kona/images/defaultproject.png",
    "href": "http://localhost:3000/api/spaces/5",
  }]
}
```

Creating a space
-----------

* `POST /spaces` will return the created space based on the JSON request sent.

```json
{
  spaces: [{
    name: "Scout Troop 703",
    color: "#b373b3",
    details: "This is the official space for Scout Troop 703"
  }]
}
```

Upon success, `201 Created` will be returend with the location of the new space in the `Location` header along with the current JSON representation of the space (See the **Get space** endpoint for more info).


Update space
---------------

* `PUT /spaces/1.json` will update the space from the JSON parameters sent.

```json
{
  "name": "This is a new name for the project!",
  "description": "And a new description..."
}
```

Upon success, `200 OK` will be returend along with the current JSON representation of the space (See the **Get space** endpoint for more info). If the user does not have access to update the space, you'll see `403 Forbidden`.
