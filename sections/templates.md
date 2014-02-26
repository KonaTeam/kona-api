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
    "details": string, readonly
    "overview": string, readonly
    "updated_at": datetime, readonly,
    "picture_thumb": string, readonly,
    "category": integer, (0=personal, 1=work [default]), readonly
    "tags": array of string, readonly,
    "template_date_type": string, readonly ('start_date', 'end_date', 'none')
  }]
}
```


Get templates
------------
`GET /templates` will return all available templates. Results may be restricted by specifying the following optional parameters:
* `account_ids=#` where # is an account_id or comma delimited list of account ids. See [accounts](accounts.md)
* `tag=[tag]` where [tag] is a tag.
* `add_on_type=[type]` where [type] is an add on type. See [add ons in accounts](accounts.md#addons)

`GET /templates/:id` will return the specified template. See [get response](responses.md#get).
