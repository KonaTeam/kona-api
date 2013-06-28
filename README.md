Kona API
========

This is the official API documentation for Kona (kona.com). This is a REST API that uses JSON for serialization and tokens for authentication.


Making a request
----------------------
All API end-points start with `https://test.kona.com/api/` and are **SSL only**.  If we change the API in back-incompatible ways then we will release a new version.  Newer versions of the API will be accessible by setting the specific version in an accept header.

Making a request to create a space in curl would look like this:

```shell
curl \
-d  '{ "spaces": [{ "name": "My New Space" }] }' \
-H "Content-Type: application/json" \
-H 'Authorization: Kona app_key:authentication_token' \
http://localhost:3000/api/spaces 
```

The `app_key` is the code generated from your Account Management page that authorizes an application to make requests to the API.  The `authentication_token` is a user specific token that allows your client to make requests on a users behalf.

Available API sections
----------------------
* [Projects](https://github.com/KonaTeam/kona-api/blob/master/sections/spaces.md)

Help us make it delightful
----------------------

We want to help you succeed using the Kona API. If you have a specific feature request or if you found a bug, please use GitHub issues to report.
