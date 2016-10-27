Tasks
========

This API allows you to create, read, update, delete, and manage tasks in spaces.

Schema  <a name='schema'></a>
------------
```
{
  "tasks": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "location": string,
    "details": string,
    "end_at": datetime,
    "space_id": integer,
    "creator_id": integer,
    "participant_ids": integer array, writeonly,
    "editor_ids": integer array, writeonly,
    "status": integer, {pending: 0, in_progress: 1 [default], complete: 2, group_closed: 3, group_reopened: 4},
    "visibility": integer, {participants_only: 1, public: 2 [default]},
    "stakeholder": integer, {just_me: 0, group: 1 [default], everyone: 2},
    "default_role": integer, {editor: 1, viewer: 2 [default], collaboration_complete: 3},
    "tags": string array, readonly,
    "group_complete: boolean, readonly,
    "completed_at": datetime, readonly,
    "completed_by_id": integer, readonly
  }]
}
```

recurring tasks support these attributes as well:
```
    "series_id": integer, readonly, The original task id for a recurring task.
    "num_in_series": integer, readonly, The recurrence sequence for a recurring task.
    "period": integer: {none: 0, day: 1, week: 2, month: 3, year: 4, weekday: 5},
    "frequency": integer, {none: 0, every: 1, every_other: 2, every_third: 3},
    "sel_days": integer, {Sunday: 0, etc.},
    "series_end_at": datetime
```



Get tasks
------------
`GET /tasks` will return all active tasks.

### Sort parameters
`GET /tasks?order_by=created_at` to sort the result by create date from the latest to the oldest.

`GET /tasks?order_by=updated_at` to sort the result from the latest to the oldest based on when the calendar event's metadata (i.e name, notes, start date etc.) was last updated.

### Filter parameters
`GET /tasks/:id` will return the specified task. See [get response](responses.md#get).

`GET /tasks/:id?num_in_series=:num` will return [num] instance of the recurring task [id]. See [get response](responses.md#get).

Create task
-----------
`POST /tasks` will return the created task based on the JSON request sent. See [create response](responses.md#create).

To create a task for the "Everyone" group, set stakeholder to 2.

Update task
---------------
`PUT /tasks/:id` will update the task from the JSON parameters sent. Only attributes in the payload will be updated (patch). See [update response](responses.md#update).


Delete task
---------------
`DELETE /tasks/:id` will delete the task from the id sent. See [delete response](responses.md#delete)

External Objects
---------------
You can link external resources to tasks by using the `external_objects` attribute. See the page on [external objects](external_objects.md) for more info. 

