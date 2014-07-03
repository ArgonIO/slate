# Errors

<aside class="notice">This error section is stored in a separate file in `includes/_errors.md`. Slate allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside>

The Reminder API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- The API request is malformed.
401 | Unauthorized -- Your access token is wrong
403 | Forbidden -- The reminder requested is hidden for administrators only
404 | Not Found -- The specified reminder could not be found
405 | Method Not Allowed -- You tried to access a reminder with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
410 | Gone -- The reminder requested has been removed from our servers
418 | I'm a teapot
429 | Too Many Requests -- You're requesting too many reminders! Slown down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
