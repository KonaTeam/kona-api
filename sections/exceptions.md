Exceptions
=====

A JSON object is returned when an error is encountered during an API call. It has the following schema:

```json
{
  "problemType":"URLLinkToExceptionsPage",
  "title":"ErrorTitle",
  "details":  "ProblemsDetails"
}
```
Here are some of the errors you might encounter, and possible explanation & cause of the error to help you out.

Unauthenticated <a name='unauthenticated'><a>
------------

Each API call needs to be authenticated. Something went wrong during this process and we aborted the process. Refer to the ```"details"``` key for the specificss of the error. Some possible reasons why you encountered this exception are (but not limited to):

  - Your access token might have expired and need to be refreshed. See [Authentication](authentication.md) section.
  - The access token included in the API call or your credentials are incorrect/invalid.

Unprocessable Entity <a name='unprocessable_entity'><a>
------------

You get this exception if your input violates a business rule. This entirely depends on the resource you are working with. You must refer to the ```"details"``` component to know more. You can also refer to our knowledgebase articles in our [support site](http://support.kona.com) to know more about the resources you are working with. Feel free to [email us](mailto:support@kona.com) if you have any questions.

Examples of when you get this error:

   - Trying to create a space from a template that requires a date and you did not provide one.
   - Attempting to create a group in a space you are not a member off.

Not Found <a name='not_found'><a>
------------

You tried to access and/or work with a particular resource that either you don't have access to or does not exist. Examples are you are trying to access a space that you don't have access to or get a particular task that does not exist. Check the ID of the resource you are trying to access.

Forbidden <a name='render_forbidden'><a>
------------

You get this error if you are trying to work on a resource you do not have permission to. Examples include updating a space group when you are not the owner of the space.

Server Error <a name='server_error'><a>
------------

We return this error when our servers encountered an unexpected technical problem.
