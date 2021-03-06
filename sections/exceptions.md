Exceptions
=====

A JSON object is returned when an error is encountered during an API call. It has the following schema:

```json
{
  "problemType": "URLLinkToExceptionsPage",
  "title": "ErrorTitle",
  "details":  "ProblemsDetails"
}
```
Here are some of the errors you might encounter, and possible explanation & cause of the error to help you out.

Unauthenticated <a name='unauthenticated'><a>
------------

Each API call needs to be authenticated. Something went wrong during this process and we aborted the process. Refer to the ```"details"``` key for the specificss of the error. Some possible reasons why you encountered this exception are (but not limited to):

  - Your access token might have expired and need to be refreshed. See [Authentication](authentication.md) section.
  - The access token included in the API call or your credentials are incorrect/invalid.

Expired Password <a name='expired'><a>
------------

Your password has already expired and needs to be changed. Go to [Kona web](https://www.kona.com/users/sign_in) to update your password and try again in your API call.

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

Bad Request <a name='bad_request'><a>
--------------

We return a 400 Bad Request error when we encounter invalid request parameters. This may depend on the resource you are working with. You must refer to the ```"details"``` component to know more. You can also refer to our knowledge base articles in our [support site](http://support.kona.com) to know more about the resources you are working with. Feel free to [email us](mailto:support@kona.com) if you have any questions. The error response will include details about the invalid parameter(s).

We also return a 400 Bad Request error when we encounter a problem parsing the JSON object in your request. By default, we respond in raw text format with the JSON string included.

If you want the response as a JSON object, include "Accept: application/json" in the header of your request.

Authentication Error
--------------

In conforming with the [OAuth 2.0 Framework](https://tools.ietf.org/html/rfc6749), authentication error responses have the following JSON schema:

```json
{
  "error": "ErrorTitle",
  "error_description":  "ErrorDescription"
}
```
Check out the value of `error_description` to get some explanation on the details of the error encountered during the authentication process.