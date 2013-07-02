Kona API
========

This is the official API documentation for Kona (kona.com). This is a REST API that uses JSON for serialization and
tokens for authentication. The API conforms to the ID based [JSON API](http://jsonapi.org/) specification.


Making a request
----------------------

All API end-points start with `https://konaservername.com/api/` and are **SSL only**.  If we change the API such that it is no longer
backward-compatible then we will release a new version.  Newer versions of the API will be accessible by setting the
specific version in an accept header.

Making a request to create a space in curl would look like this:

```shell
curl \
-d  '{ "spaces": [{ "name": "My New Space" }] }' \
-H "Content-Type: application/json" \
-H 'Authorization: Kona app_key:authentication_token' \
https://konaservername/api/spaces 
```

The `app_key` is the code generated from your Account Management page that authorizes an application to make requests
to the API.  The `authentication_token` is a user specific token that allows your client to make requests on a users behalf.
Both the `app_key` and `authentication_token` must be sent in the HTTP Authorization header with all requests to the API
with the format above.

See [Token](https://github.com/KonaTeam/kona-api/blob/master/sections/token.md) for more information on how to retrieve a user's authentication token.

See [responses](https://github.com/KonaTeam/kona-api/blob/master/sections/responses.md) for more information on what to
expect returned for requests.

Pagination
----------------------

All GET requests that return multiple results support pagination.  By default, we send back a maximum of 25 items per page.
You may override the default by passing `{ per_page: YOUR_NUMBER, page: CURRENT_PAGE }` as root level nodes in the request.
We plan on adding next and prev urls to the response in order to make paging simpler on the client.

Available API sections
----------------------

* [Token](https://github.com/KonaTeam/kona-api/blob/master/sections/token.md)
* [Spaces](https://github.com/KonaTeam/kona-api/blob/master/sections/spaces.md)

Help us make it delightful
----------------------

We want to help you succeed using the Kona API. If you have a specific feature request or if you found a bug, please
use GitHub Issues to report.
