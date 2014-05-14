Risks
========

This API allows you to create, read, update, delete, and manage risks in spaces.
TODO: Disclaimer about needing Risk add-on.

Schema  <a name='schema'></a>
------------
```
{
  "risks": [{
    "id": integer, readonly,
    "name": string, required,
    "href": string, readonly,
    "location": string,
    "details": string,
    "space_id": integer,
    "creator_id": integer, readonly,
    "participants": [{"user_id": integer, "role": integer }],
    "groups": [{"group_id": integer, "role": integer }],
    "status": integer, {pending: 0, in_progress: 1 [default], complete: 2, group_closed: 3, group_reopened: 4},
    "visibility": integer, {participants_only: 1, public: 2 [default]},
    "stakeholder": integer, {just_me: 0, group: 1 [default], everyone: 2},
    "default_role": integer, {editor: 1, viewer: 2 [default], collaboration_complete: 3},
    "risk_type": integer, {thread: 0, opportunity: 1},
    "risk_id": integer
  }]
}
```

Get risks
------------
`GET /risks` will return all active risks.

`GET /risks/:id` will return the specified risk. See [get response](responses.md#get).

Create risk
-----------
`POST /risks` will return the created risk based on the JSON request sent. See [create response](responses.md#create).

The new_poll_results array is expected to be passed up to contain the creator's risk consisting of
poll_question_index & poll_answer_index which correlate to the details in the risk matrix. The risk score is also
provided.

Every risk has a probability
and is always poll_question_index: 0. The subsequent indexes correspond to the impacts listed in the risk matrix; e.g.
if the first impact is Schedule, then poll_question_index: 1 corresponds to that. The poll_quesition_index series
continues for mitigation questions regardless if mitigation is provided. See example below.

The poll_answer_index values are similarly derived from the risk matrix. Probability values correspond to
ProbabilityImpacts 0=first RiskProbabilityImpact in matrix, etc.
Impact values correspond to ImpactCell values. However 0 is reserved for "Negligible".

The creator's score is also expected to be passed up with a poll_question_index corresponding to to the last impact's
index +1. This will have a "custom_value" that is the score. A mitigation score should follow if mitigation is provided.

Example providing risk and score for a matrix that defines a schedule and then cost impacts:
```
"new_poll_results": [
   { "poll_question_index": 0, "poll_answer_index": 2},  # probability "Medium" (from Probability Impacts)
   { "poll_question_index": 1, "poll_answer_index": 3},  # schedule "Medium"  (0=Negligible, other values from Impact Cells)
   { "poll_question_index": 2, "poll_answer_index": 1},  # cost "Low" (0=Negligible, other values from Impact Cells)
   { "poll_question_index": 6, "poll_answer_index": 0, "custom_value": 10}  # score - note poll_question_index still 'leaves room' for mitigation
   ]
```

Example providing risk, mitigation and scores for a matrix that defines a schedule and then cost impacts:
```
"new_poll_results": [
   { "poll_question_index": 0, "poll_answer_index": 2},  # probability "Medium" (from Probability Impacts)
   { "poll_question_index": 1, "poll_answer_index": 3},  # schedule "Medium"  (0=Negligible, other values from Impact Cells)
   { "poll_question_index": 2, "poll_answer_index": 1},  # cost "Low" (0=Negligible, other values from Impact Cells)
   { "poll_question_index": 3, "poll_answer_index": 0},  # mitigation probability "Very High"
   { "poll_question_index": 4, "poll_answer_index": 0},  # mitigation schedule "Negligible"
   { "poll_question_index": 5, "poll_answer_index": 0},  # mitigation cost "Negligible"
   { "poll_question_index": 6, "poll_answer_index": 0, "custom_value": 10}  # score
   { "poll_question_index": 7, "poll_answer_index": 0, "custom_value": 0}   # mitigation score
   ]
```


To create a risk for the "Everyone" group, set stakeholder to 2. For example, this will create a risk for space 9 that is assigned to Everyone and is public.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "stakeholder": 2,
    "risk_type": 0,
    "new_poll_results": [ ... ]
  }]
}
```

This will create a risk for space 9 that is assigned to group 11 and is private.
```
{
  "risks": [{
    "name": "new risk",
    "space_id": 9,
    "groups": [{"group_id": 11, "role": 1 }],
    "visibility": 1,
    "risk_type": 0,
    "new_poll_results": [ ... ]
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
    "new_poll_results": [ ... ]
  }]
}
```

Delete risk
---------------
`DELETE /risks/:id` will delete the risk from the id sent. See [delete response](responses.md#delete)


