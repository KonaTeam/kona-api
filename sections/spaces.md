Spaces
========

Most resources in Kona are organized into spaces.  This API allows you to create, read, update, delete, and manage membership to spaces.
Templates are specialized spaces within Kona that can be used to create additional spaces.

Schema  <a name='schema'></a>
------------
```
{
  "spaces": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "account_id", integer, readonly except on create,
    "details": string,
    "welcome_message": string,
    "updated_at": datetime, readonly,
    "picture_thumb": string, readonly,
    "category": integer, (personal: 0, work: 1 [default]),
    "space_type": integer, {standard: 0, default: 1, account_default: 2, open: 3}, readonly,
    "template": boolean, readonly,
    "published": boolean, readonly
  }]
}
```


Get spaces
------------
`GET /spaces` will return all active spaces. Spaces hidden for the current user are excluded by default.

### Filter parameters
`GET /spaces?add_on_type=:type` will filter spaces for the given type.

`GET /spaces?hidden=true` will return hidden spaces.

`GET /spaces/:id` will return the specified space. See [get response](responses.md#get).

<a name='participants'></a>Passing `include=participants` will cause the response to include the participants of the spaces.
```
  "participants": [{
    "user_id": integer,
    "role": integer, {owner: 1, participant: 2},
    "status": integer, {pending: 0, accept: 1, reject: 2}
    "updated_at": datetime
  }]
```

<a name='addons'></a>Passing `include=space_account_add_ons` will cause the response to include the account add ons for the spaces. See [Account add-ons](accounts.md#addons).
```
  "space_account_add_ons": [{
    "account_add_on_id": integer,
    "meta": text
  }]
```


Create space
-----------
`POST /spaces` will return the created space based on the JSON request sent. See [create response](responses.md#create).
The `space_account_add_ons` array node is supported for create.
To create a space from a template, include the following attributes in your JSON request
```
  "account_id": integer, (1=My Account [default]),
  "template_space_id": integer,
  "subscription_anchor_date": string, "YYYY-MM-DD" format, required when "template_date_type" is not 'none'
  ```

Update space
---------------
`PUT /spaces/:id` will update the space from the JSON parameters sent. Only attributes in the payload will be updated (patch). See [update response](responses.md#update).
The `space_account_add_ons` array node is supported for update.


Delete space
---------------
`DELETE /spaces/:id` will delete the space from the id sent. See [delete response](responses.md#delete)


Invite to space
---------------
`POST /spaces/:id/participants` will allow people to be invited to a space.

```
{
  "participants": [{"email": "frank@example.com"}, {"email": "pete@example.com"}],
  "message": "Welcome to the space!",
  "auto-accept" : false // automatically accepts the invite on behalf of the user when set to true. defaults to false
  "suppress_owner_notification": false // when set to true, it does not notify the space owner when the invite API has been sent out. defaults to false
}
```

Upon success, `200 OK` will be returned along with a response with the following schema:

```
{
  "participants":[{
    "id": integer,
    "email": string,
    "name": string,
    "status": string (created | existing | resent | failed),
    "invitation_sent": boolean
  }]
}
```

When you put in multiple email addresses in the request, make sure to check the `status` and/or `invitation_sent` attributes of each email address in the response to see if there are any validation errors encountered in a particular email address. For example, if you call the API with 3 email addresses in it and the third is not a valid email address, the hash for that particular email would look like this.

```json
{
  "participants":[{
    "email": "John+Doe",
    "status": "failed",
    "status_reason": "User Email must be a valid email address",
    "invitation_sent": false
  }]
}
```

If all the email addresses in the request have failed our validation, we return an 422 exception.

In some cases, you would want existing Kona users to be included automatically in a space, bypassing the invitation phase. To do so, add the `auto_accept` attribute in your POST request e.g. 

```json
{
  "participants": [{"email": "frank@example.com"}, {"email": "pete@example.com"}],
  "message": "Welcome to the space!",
  "auto_accept": true
}
```

By default, the space owner is notified on their global feed notification whenever the API is called. To suppress this, use the `suppress_owner_notification` attribute.

Remove from space
---------------
`DELETE /spaces/:space_id/participants/:user_id` will allow users to be removed from a space.

`DELETE /spaces/:space_id/participants/:email` will allow users to be removed from a space.

Upon success, `204 No Content` will be returned.  If the user does not have permission to revoke a users space membership, you"ll receive `403 Forbidden`.
