Accounts
========

Accounts organize spaces for organizations.  This endpoint allows retrieval of accounts accessible for a user.

Schema  <a name='schema'><a>
------------
```
{
  "accounts": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "account_type": integer, (0=personal, 1=pro, 2=corporate, 3=partner}, readonly
  }]
}
```


Get accounts
------------
`GET /accounts` will return all available accounts.

`GET /accounts/:id` will return the specified template. See [get response](responses.md#get).

Passing `include=account_add_ons` will cause the response to include the account add ons for each account:
------------
```
{
  "account_add_ons": [{
    "id": integer, readonly,
    "name": string, readonly,
    "add_on_type": integer, (0=risk), readonly,
    "user_limit": integer, readonly
  }]
}
```