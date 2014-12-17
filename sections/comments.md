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


Create comment
-----------
`POST /comments` will return the created comment based on the JSON request sent. See [create response](responses.md#create).   
Comments are always associated with another resource. To create a comment, space from a template, include the following attributes in your JSON request

```
  "resource_type": string, ('task', 'calendar_event', 'attachment', 'conversation'),
  "resource_id": integer
```

You can get the ID of the particular resource in a number of ways. You can go to Kona on your browser and pick out the ID
from the resource's url. For example, given you have a task whose url is `https://kona.com/#!/projects/10/tasks/1`, the resource ID
for the task is 1. You can get the url after selecting the resource and inspecting the browser's address bar or go to the
dropdown menu of the resource and select "Copy URL".

Alternatively, you can use the other REST APIs to search for the particular resource and get its ID key.

Make sure to escape double-quotes and use the `<br>` tag to add a new line to the content.

