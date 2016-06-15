Comments
========

This API allows you to create comments.

Schema  <a name='schema'></a>
------------
```
{
  "comments": [{
    "id": integer, readonly,
    "creator_id": integer, readonly,
    "content": string, required,
    "href": string, readonly,
    "priority": integer, (normal: 0 [default], important: 1),
    "created_at": datetime, readonly
  }]
}
```

Get comments
-----------
### Query parameters
You need to specify the activity or resource type and ID fetch the comments made to it.

`GET /comments?resource_type=:resource_type&resource_id=:resource_id` will return paged comments starting from most recent. See [resource identity](comments.md#resourceid).

`GET /comments/:id` will return the specified comment. See [get response](responses.md#get).

Create comment <a name='post'></a>
-----------
`POST /comments` will return the created comment based on the JSON request sent. See [create response](responses.md#create).   
Comments are always associated with another resource. To create a comment, space from a template, include the following attributes in your JSON request

<a name='resourceid'></a>
```
  "resource_type": string, ('task', 'calendar_event', 'attachment', 'conversation'),
  "resource_id": integer
```

You can "@mention" people or groups by encoding the appropriate ID(s) e.g. 
`Hi @user:123, I agree. @group:456 should take note.`

This will translated and return the content to the appropriate markup for @mention notification. 

`Hi Joe User, I agree. Developers should take note.` 

You can get the ID of a particular resource by using their corresponding REST APIs to search for it and return its ID key.

You can also use the person's email address instead of their user ID to mention them e.g. 

`Hi @user:joeuser@example.com, I agree.` 

We will try to map the given email address to a participant of the specified activity/resource - otherwise, we return a [Not Found exception](exceptions.md#not_found).  

Make sure to escape double-quotes and use the `<br>` tag to add a new line to the content.

