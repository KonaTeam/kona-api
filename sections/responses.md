Responses
========

All resources are represented as JSON objects and are wrapped in an array.

Get resource
------------
Upon success, `200 OK` will be returned along with the currnet JSON representation of the resource.

Create resource
-----------
Create is handled with a POST to the resource's path.
Upon success, `201 Created` will be returend with the location of the new resource in the `Location` header along with the current JSON representation of the resource.


Update resource
---------------
Update is handled with a PUT to the resource's path.
Upon success, `200 OK` will be returend along with the current JSON representation of the resource. If the user does not have access to update the resource, you"ll receive `403 Forbidden`.


Delete space
---------------
Delete is hanled with a DELETE to the resource's path.
Upon success, `204 No Content` will be returend. If the user does not have access to delete the resource, you"ll receive `403 Forbidden`.

