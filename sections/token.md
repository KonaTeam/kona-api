Token
========

Making requests to the API end-points requires adding a user-specific authentication token as part of the HTTP Authorization header.  These tokens can be retrieved and reset from the below end-points.

Get token
------------

In order to authenticate to this end-point the HTTP Authorization header of your request needs to be in the following format.  `Authorization: Kona app_key:email:password`

* `GET /token` will retrieve a user's authentication token

Upon success, `200 OK` will be returend along with a response that looks like the following:

```
{
  "tokens": [{
    "auth_token": 'Th3Us3rsT0k3n'
  }]
}
```

Reset token
------------

* `PUT /token/reset` will regenerage a user's authentication token

Upon success, `200 OK` will be returend along with a response containing the token (See the **Get token** end-point for more info).