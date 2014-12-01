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
    "priority": integer, (normal: 0 [default], important: 1)
  }]
}
```


Create comment
-----------
`POST /comments` will return the created comment based on the JSON request sent. See [create response](responses.md#create).   
Comments are always associated with another resource. To create a comment, space from a template, include the following attributes in your JSON request

```
  "commentable_type": string, ('task', 'calendar_event', 'attachment'),   
  "commentable_id": integer   
```

