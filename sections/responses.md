Responses
========

All resources are represented as JSON objects and are wrapped in an array. Each entity has a unique integer identifier "id".
All DateTime values are represented as Unix epoch.

Get resource  <a name='get'><a>
------------
Upon success, `200 OK` will be returned along with the current JSON representation of the resource.

Create resource  <a name='create'><a>
-----------
Create is handled with a POST to the resource's path.
Upon success, `201 Created` will be returned with the location of the new resource in the `Location` header along with
the current JSON representation of the resource. If an error such as a business rule violation occurs (i.e. exceeded the user's space limit),  the details  of the error can be found in the response in JSON format.


Update resource  <a name='update'><a>
---------------
Update is handled with a PUT to the resource's path.
Upon success, `200 OK` will be returned along with the current JSON representation of the resource. If the user does
not have access to update the resource, you will receive `403 Forbidden`.


Delete resource  <a name='delete'><a>
---------------
Delete is handled with a DELETE to the resource's path.
Upon success, `204 No Content` will be returned. If the user does not have access to delete the resource, you will
receive `403 Forbidden`.

