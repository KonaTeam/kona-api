External Objects
========

Integrating applications can link conversations, tasks and calendar events to their own records and/or resources by using the attribute `external_objects`. This would help applications be redirected back to their own resources from an associated Kona conversation, task or calendar event.

Schema  <a name='schema'></a>
------------

Add the following attribute to a conversation, calendar event or task 
```
    "external_object": {
        "id": string, required,
        "name": string, required,
        "url": string, optional
    }
```

For example, when you create a new conversation with an external object, it would look something like this: 
```
{
  "conversations": [{
    "name": "Statewide Radios Opportunity Planning",
    "space_id": 9,
    "stakeholder": 2,
    "initial_comment": "This is a lucrative opportunity for us. Let's start the capture process immediately",
    "external_object": {
        "id": "AD8EDAB4-8725-4FA1-8347-DE091D600960",
        "name": "STATEWIDE TWO WAY RADIOS",
        "url": "https://iq.govwin.com/neo/opportunity/view/117242"
    }
  }]
}
```

When you delete an activity, its corresponding external object is also deleted. 

You can also update and/or add an external object to an existing activity by sending out an update request and including an `external_object` attribute to the payload. To remove the external object of an activity, pass an empty object to the `external_object` attribute e.g. `"external_object": {}`.