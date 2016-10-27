Calendar Events
========

This API allows you to create, read, update, delete, and manage calendar events in spaces.

Schema  <a name='schema'></a>
------------
```
{
  "calendar_events": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "location": string,
    "details": string,
    "all_day": boolean,
    "start_at": datetime, required, defaults to current datetime,
    "end_at": datetime, required, defaults to current datetime,
    "space_id": integer,
    "creator_id": integer,
    "participant_ids": integer array, writeonly,
    "editor_ids": integer array, writeonly,
    "status": integer, {pending: 0, in_progress: 1 [default], complete: 2, group_closed: 3, group_reopened: 4},
    "visibility": integer, {participants_only: 1, public: 2 [default]},
    "stakeholder": integer, {just_me: 0, group: 1 [default], everyone: 2},
    "default_role": integer, {editor: 1 [default], viewer: 2, collaboration_complete: 3},
    "tags": string array, readonly,
    "ics_feed_id": integer, readonly
  }]
}
```

recurring events support these attributes as well:
```
    "series_id": integer, readonly, The original event id for a recurring event.
    "num_in_series": integer, readonly, The recurrence sequence for a recurring event.
    "period": integer: {none: 0, day: 1, week: 2, month: 3, year: 4, weekday: 5},
    "frequency": integer, {none: 0, every: 1, every_other: 2, every_third: 3},
    "sel_days": integer, {Sunday: 0, etc.},
    "series_end_at": datetime
```

Get events
------------
`GET /calendar_events` will return all active events for the current month.

### Filter parameters
`GET /calendar_events?date_range_start=:start&date_range_end=:end` will limit the events to the time window. Note: Paging is still in effect.

`GET /calendar_events/:id` will return the specified event. See [get response](responses.md#get).

`GET /calendar_events/:id?num_in_series=:num` will return [num] instance of the recurring event [id]. See [get response](responses.md#get).

Create events
-----------
`POST /calendar_events` will return the created event based on the JSON request sent. See [create response](responses.md#create).

To create an event for the "Everyone" group, set stakeholder to 2.

Update event
---------------
`PUT /calendar_events/:id` will update the event from the JSON parameters sent. Only attributes in the payload will be updated (patch). See [update response](responses.md#update).


Delete event
---------------
`DELETE /calendar_events/:id` will delete the event from the id sent. See [delete response](responses.md#delete)

External Objects
---------------
You can link external resources to calendar events by using the `external_objects` attribute. See the page on [external objects](external_objects.md) for more info. 


