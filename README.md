Kona API
========

This is the official API documentation for Kona (kona.com). This is a REST API that uses JSON for serialization and
tokens for authentication. The API conforms to the ID based [JSON API](http://jsonapi.org/) specification.


Making a request
----------------------

All API end-points start with `https://io.kona.com/api/` and are **SSL only**.  If we change the API such that it is no longer
backward-compatible then we will release a new version.  Newer versions of the API will be accessible by setting the
specific version in an accept header. Note: Do not rely on ordinal position for response parsing; response[3].
Rather, use the JSON keys provided; response['name'].

Making a request to create a space in curl would look like this:

```shell
curl \
  -d  '{ "spaces": [{ "name": "My New Space" }] }' \
  -H "Content-Type: application/json" \
  -H 'Authorization: Bearer access_token' \
  https://io.kona.com/api/spaces
```

The `access_token` is the user's OAuth access token.
See [Authentication](sections/authentication.md) for more information on how to retreive an access token.

See [responses](sections/responses.md) for more information on what is
returned for requests.

Pagination
----------------------

All GET requests that return multiple results support pagination.  By default, we send back a maximum of 25 items per page.
You may override the default by passing `{ per_page: YOUR_NUMBER, page: CURRENT_PAGE }` as root level nodes in the request.
We plan on adding next and prev urls to the response in order to make paging simpler on the client.

Available API sections
----------------------

* [Authentication](sections/authentication.md)
* [Spaces](sections/spaces.md)
* [Users](sections/users.md)
* [Tasks](sections/tasks.md)
* [Calendar Events](sections/calendar_events.md)

Help us make it delightful
----------------------

We want to help you succeed using the Kona API. If you have a specific feature request or if you found a bug, please
use GitHub Issues to report.
