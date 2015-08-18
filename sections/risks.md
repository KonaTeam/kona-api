Risks
========

This API allows you to create, read, delete, and manage risks in spaces.
TODO: Disclaimer about needing Risk add-on.

Schema  <a name='schema'></a>
------------
```
{
  "risks": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "details": string,
    "space_id": integer,
    "creator_id": integer, readonly,
    "participants": [{"user_id": integer, "role": integer }],
    "groups": [{"group_id": integer, "role": integer }],
    "status": integer, {pending: 0, in_progress: 1 [default], complete: 2, group_closed: 3, group_reopened: 4},
    "visibility": integer, {participants_only: 1, public: 2 [default]},
    "stakeholder": integer, {just_me: 0, group: 1 [default], everyone: 2},
    "default_role": integer, {editor: 1, viewer: 2 [default], collaboration_complete: 3},
    "tags": string array, readonly,
    "risk_type": integer, {threat: 0, opportunity: 1, calendar_event: 2, window: 3},
    "risk_id": string,
    "mitigation": boolean,
    "mitigation_text", string, readonly except on create,
    "initial_comment", string, create only,
    "prevent_splitting", boolean, readonly,
    "poll_results": [{ "user_id": integer, "poll_question_index": integer, "poll_answer_index": integer, "custom_value": string}], readonly
    "risk_window": {"prevent_splitting": boolean, "min_start": datetime, "likely_start": datetime, "max_start": datetime, "min_finish": datetime, "likely_finish": datetime, "max_finish": datetime}, readonly,
    "risk_calendar_events": [{"enabled": boolean, "description": string, "probability": integer, "min": integer, "most_likely": integer, "max": integer}], readonly
  }]
}
```

Get risks
------------
`GET /risks` will return all active risks.

### Filter parameters
`GET /risks?space_id=:space_id` will return all active risks for :space_id.

`GET /risks/:id` will return the specified risk. See [get response](responses.md#get).

Create risk
-----------
`POST /risks` will return the created risk based on the JSON request sent. See [create response](responses.md#create).

### Threats and Opportunities ###

For threats and opportunities, the new_poll_results array is expected to be passed up to contain the creator's risk consisting of
poll_question_index & poll_answer_index which correlate to the details in the risk matrix. The risk score is also
provided.

Every risk has a probability
and is always poll_question_index: 0. The subsequent indexes correspond to the impacts listed in the risk matrix; e.g.
if the first impact is Schedule, then poll_question_index: 1 corresponds to that. The poll_question_index series
continues for mitigation questions regardless if mitigation is provided. See example below.

The poll_answer_index values are similarly derived from the risk matrix. Probability values correspond to
ProbabilityImpacts 0=first RiskProbabilityImpact in the risk matrix, etc.
Impact values correspond to ImpactCell values *but in reverse order*.
"Negligible" is a special value that is added to the end of each list of values.
Note: Future versions of the API will support the string value.

The creator's score is also *required* to be passed up with a poll_question_index corresponding to to the last impact's
index +1. The poll_answer_index corresponds to the Threshold; e.g. 0 == the first threshold, etc.
The custom_value that is the score.
A mitigation score should follow if mitigation is provided following the same pattern

Example providing risk and score for a matrix that defines a schedule and then cost impacts:
```
"new_poll_results": [
   { "poll_question_index": 0, "poll_answer_index": 1},  # probability "High" (from Probability Impacts)
   { "poll_question_index": 1, "poll_answer_index": 0},  # schedule "Very High"  (5=Negligible since 5 impacts in matrix, other values from Impact Cells)
   { "poll_question_index": 2, "poll_answer_index": 2},  # cost "Medium" (5=Negligible since 5 impacts in matrix, other values from Impact Cells)
   { "poll_question_index": 6, "poll_answer_index": 0, "custom_value": 10}  # score - note poll_question_index still 'leaves room' for mitigation
   ]
```

Example providing risk, mitigation and scores for a matrix that defines a schedule and then cost impacts:
```
"new_poll_results": [
   { "poll_question_index": 0, "poll_answer_index": 1},  # probability "High" (from Probability Impacts)
   { "poll_question_index": 1, "poll_answer_index": 0},  # schedule "Very High"  (5=Negligible since 5 impacts in matrix, other values from Impact Cells)
   { "poll_question_index": 2, "poll_answer_index": 2},  # cost "Medium" (5=Negligible since 5 impacts in matrix, other values from Impact Cells)
   { "poll_question_index": 3, "poll_answer_index": 0},  # mitigation probability "Very High"
   { "poll_question_index": 4, "poll_answer_index": 5},  # mitigation schedule "Negligible"
   { "poll_question_index": 5, "poll_answer_index": 4},  # mitigation cost "Very Low"
   { "poll_question_index": 6, "poll_answer_index": 1, "custom_value": 10}  # score corresponding to the second Threshold
   { "poll_question_index": 7, "poll_answer_index": 0, "custom_value": 0}   # mitigation score corresponding to the first Threshold
   ]
```


To create a risk for the "Everyone" group, set stakeholder to 2. For example, this will create a risk for space 9 that
is assigned to Everyone and is public with an initial comment.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "stakeholder": 2,
    "risk_type": 0,
    "initial_comment": "mitigation description goes here",
    "new_poll_results": [ ... creator responses excluded for brevity ...]
  }]
}
```

This will create a risk for space 9 that is assigned to group 11 and is private *with* mitigation.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "groups": [{"group_id": 11, "role": 1 }],
    "visibility": 1,
    "risk_type": 0,
    "mitigation": 1,
    "new_poll_results": [ ... creator responses and mitigation excluded for brevity ...]
  }]
}
```

This will create a risk for space 9 with user 14 and group 11 as editors, user 15 and group 12 as viewers and is private.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "participants": [{"user_id": 14, "role": 1},{"user_id":15, "role": 2}],
    "groups": [{"group_id": 11, "role": 1 },{"group_id": 12, "role": 2 }],
    "visibility": 1,
    "risk_type": 0,
    "new_poll_results": [ ... creator responses excluded for brevity ...]
  }]
}
```

### Risk Calendar Events ###
For risk calendar events the **prevent_splitting** attribute is passed directly with the risk object, while the calendar event information is passed as an array under the risk_calendar_events_attibutes node.  Example:

```
{
  "risk_calendar_event_attributes": [
    { "enabled": true, 
      "description": "January", 
      "probability": 70, 
      "min": 10, 
      "most_likely": 50, 
      "max": 60
    },
    { "enabled": true, 
      "description": "February", 
      "probability": 75, 
      "min": 15, 
      "most_likely": 25, 
      "max": 35
    },
    etc...
  ]
}
```


### Risk Windows ###
For risk windows the window information is passed under the risk_window_attibutes node.  Example:

```
{
  "risk_window_attributes": {
    "prevent_splitting": false, 
    "min_start": "2015-08-17 00:00:00 -0000",
    "likely_start": "2015-08-20 00:00:00 -0000",
    "max_start": "2015-08-23 00:00:00 -0000",
    "min_finish": "2015-08-22 00:00:00 -0000",
    "likely_finish": "2015-08-25 00:00:00 -0000",
    "max_finish": "2015-08-29 00:00:00 -0000"
  }
}
```

**Note:** Kona currently does not support batch creation of risks. Make multiple API calls when creating a number of risks, with one risk per call - which should also allow you to monitor if a particular risk has been created successfully or not. 

Delete risk
---------------
`DELETE /risks/:id` will delete the risk from the id sent. See [delete response](responses.md#delete)


