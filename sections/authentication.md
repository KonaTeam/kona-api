# Authentication

The Kona APIs use the OAuth 2.0 protocol for authentication and authorization. OAuth 2.0 is a relatively simple protocol.
To begin, you register your application with Kona via _Account Management_, _Manage client applications_. With the details
from the client applications view, your client application requests an access token from Kona on behalf of the user.
Once the user approves the request, the access token is given back to your client application. Subsequent API requests
will pass the access token.

Client libraries exist for various platforms. This documentation will demonstrate using the [OAuth2 gem](https://github.com/intridea/oauth2) in Ruby.
You'll need to `gem install oauth2` if you want to follow the Ruby examples.
Curl examples are provided. However, using a client library can simplify interacting with an OAuth provider.

The authorization endpoint is `https://io.kona.com/oauth/authorize` and the access token endpoint is `https://io.kona.com/oauth/token`. [References](authentication.md#references) are linked at the bottom to help learn the basics of OAuth.

## Getting an access token
Using the client id, client secret and redirect uri from _Manage client applications_:

### Ruby
```ruby
# Step 1, get an Authentication Code
require 'oauth2'
client_id = 'theid'
client_secret = 'thesecret'
redirect_uri = 'http://myserver/oauth2/callback' # this must match the uri registered in Manage client applications

client = OAuth2::Client.new(client_id, client_secret, site: 'https://io.kona.com')
url = client.auth_code.authorize_url(:redirect_uri => redirect_uri)

# This returns a url you will direct the user to. From this page, the Kona user will approve your client application's
# access. Once approved, the user is redirected to redirect_uri with a 'code' url parameter. This code is short lived
# and is used to request an access token:

# Step 2, use Authentication Code to get access token
code = 'thecodefromthecallback'
token = client.auth_code.get_token( code, redirect_uri: redirect_uri)

access_token = token.token
refresh_token = token.refresh_token

# Step 3, save access token and refresh token in a secure storage.

# When the access token expires, the refresh token can be used to issue a new token. Save the new token. The refresh
# token will not change.
access_token = token.refresh!.token

```

### Curl
```shell
# Step 1, get an Authentication Code
export CLIENT_ID="theid"
export CLIENT_SECRET="thesecret"
export REDIRECT_URI="http://myserver/oauth2/callback"
open https://io.kona.com/oauth/authorize\?response_type\=code\&client_id\=$CLIENT_ID\&redirect_uri\=$REDIRECT_URI

# Once approved, grab the 'code' url parameter returned and use it to POST to get the access token

# Step 2, use Authentication Code to get an access token
export CODE='thecode'
curl -i -X POST https://io.kona.com/oauth/token\?code\=$CODE\&client_secret\=$CLIENT_SECRET\&client_id\=$CLIENT_ID\&grant_type\=authorization_code

# Which should return something like:
HTTP/1.1 200
Server: nginx/1.4.2
Content-Type: application/json; charset=utf-8
Content-Length: 208
Connection: keep-alive
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Status: 200
x-api-version: web
X-Rack-Cache: invalidate, pass
X-Request-Id: 68fcaa4769f6da9aacbf43c9840f9f57
X-Runtime: 0.022264
X-UA-Compatible: IE=Edge,chrome=1

{"access_token":"theaccesstoken","token_type":"bearer","expires_in":604800,"refresh_token":"therefreshtoken"}

# Step 3, save access token and refresh token in a secure storage.

# When the access token expires, the refresh token can be used to issue a new token. Save the new token. The refresh
# token will not change.
export REFRESH_TOKEN='therefreshtoken'
curl -i -X POST https://io.kona.com/oauth/token\?refresh_token\=$REFRESH_TOKEN\&client_secret\=$CLIENT_SECRET\&client_id\=$CLIENT_ID\&grant_type\=refresh_token

{"access_token":"thenewaccesstoken","token_type":"bearer","expires_in":604800,"refresh_token":"therefreshtoken"}

```

## All API requests pass the access token for authentication

### Ruby
```ruby
require 'oauth2'
client_id = 'theid'
client_secret = 'thesecret'
access_token, refesh_token = store.access_token, store.refresh_token # assuming 'store' stores tokens

client = OAuth2::Client.new(client_id, client_secret, site: 'https://io.kona.com')
token = OAuth2::AccessToken.new(client, access_token, {refresh_token: refresh_token})
response = token.get('/api/spaces')

```

### Curl
```shell
export CLIENT_ID="theid"
export CLIENT_SECRET="thesecret"
export ACCESS_TOKEN="theaccesstoken" # retrieved from some secure store
curl -H "Content-Type: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" https://io.kona.com/api/spaces

```

## References <a name='references'><a>

[Google OAuth 2.0] (https://developers.google.com/accounts/docs/OAuth2)

[lockitron OAuth 2.0] (https://api.lockitron.com/v1/getting_started/authenticating_with_oauth)
