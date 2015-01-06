Comments
========

This API allows you to create comments.

Schema  <a name='schema'></a>
------------
```
{
  "comments": [{
    "id": integer, readonly,
    "content": string, required,
    "href": string, readonly,
    "priority": integer, (normal: 0 [default], important: 1),
    "created_at": datetime, readonly
  }]
}
```

Get comments
-----------
`GET /comments?resource_type=:resource_type&resource_id=:resource_id` will return paged comments starting from most recent. See [resource identity](comments.md#resourceid).

`GET /comments/:id` will return the specified comment. See [get response](responses.md#get).

Create comment
-----------
`POST /comments` will return the created comment based on the JSON request sent. See [create response](responses.md#create).   
Comments are always associated with another resource. To create a comment, space from a template, include the following attributes in your JSON request

<a name='resourceid'></a>
```
  "resource_type": string, ('task', 'calendar_event', 'attachment', 'conversation'),
  "resource_id": integer
```

