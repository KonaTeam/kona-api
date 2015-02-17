Templates
========

A templates is a specialized space within Kona that can be used to create additional spaces.  This endpoint is
specifically for retrieving templates available for a user and optionally limited by account and other criteria.
Template management is handled through the [spaces](spaces.md) endpoint.

Schema  <a name='schema'></a>
------------
```
{
  "templates": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "account_id", integer, readonly,
    "details": string, readonly
    "overview": string, readonly
    "updated_at": datetime, readonly,
    "picture_thumb": string, readonly,
    "category": integer, (personal: 0, work: 1 [default]), readonly
    "tags": array of string, readonly,
    "template_date_type": string, readonly ('start_date', 'end_date', 'none')
  }]
}
```


Get templates
------------
`GET /templates` will return all available templates.

### Filter parameters
`GET /templates?account_ids=:ids` where :ids is an account_id or comma delimited list of account ids. See [accounts](accounts.md)

`GET /templates??tag=:tag` where :tag is a tag.

`GET /templates?add_on_type=:type` where :type is an add on type. See [add ons in accounts](accounts.md#addons)

`GET /templates/:id` will return the specified template. See [get response](responses.md#get).
