Accounts
========

Accounts organize spaces for organizations.  This endpoint allows retrieval of accounts accessible for a user.

Schema  <a name='schema'></a>
------------
```
{
  "accounts": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "account_type": integer, (personal: 0, pro: 1, corporate: 2, partner: 3}, readonly
  }]
}
```


Get accounts
------------
`GET /accounts` will return all available accounts.

### Filter parameters
`GET /accounts/:id` will return the specified account. See [get response](responses.md#get).

`GET /accounts/current` will return the account associated with the current credentials if you are a member of the account.

<a name='addons'></a>Passing `include=account_add_ons` will cause the response to include the account add ons for each account.
```
  "account_add_ons": [{
    "id": integer, readonly,
    "add_on_type": integer, (risk: 0), readonly,
    "user_limit": integer, readonly
  }]
```
