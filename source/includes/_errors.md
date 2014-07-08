# Errors

## Error object
The API returns an error list comprised of the following:

Field | Description
--------- | -------
type | error.list
errors | an array of errors

### ERROR OBJECT ATTRIBUTES

> Erorr lists returns JSON structured like this:

```json
{
  "type": "error_list",
  "errors": [
    {
      "status": "missing_param",
      "message": "seconds is missing",
      "field": "seconds"
    }
  ]
}
```

Each Error Object has the following attributes

Field |	Description
-------| ---------
code |	A string indicating the kind of error, used to further qualify the HTTP response code
message |	Optional. Human readable description of the error
field	| Optional. Used to identify a particular field or query parameter that was in error.


## Error codes
Error Code | Meaning
---------- | -------
400 | Bad Request -- The API request is malformed.
401 | Unauthorized -- Your access token is wrong
403 | Forbidden -- The reminder requested is hidden for administrators only
404 | Not Found -- The specified reminder could not be found
405 | Method Not Allowed -- You tried to access a reminder with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
410 | Gone -- The reminder requested has been removed from our servers
429 | Too Many Requests -- You're requesting too many reminders! Slown down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
