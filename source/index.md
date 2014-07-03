---
title: FollowUp API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Sign Up for a FollowUp Account</a>

includes:
  - errors

search: true
---

# API Reference

Welcome to the FollowUp API documentation!

You can use our API to access, create and update your reminders.

JSON will be returned in all responses from the API including errors. We currently have code examples using curl and will provide examples for ruby once we push our gem.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: myaccesstoken"
```

> Make sure to replace `myaccesstoken` with your API key.

FollowUp uses Oauth access tokens to allow access to the API. You can register a new FollowUp API application [here](https://followup.cc/api_applications/new).

FollowUp expects for the access token key to be included in all API requests that looks like the following:

`Authorization: myaccesstoken`

<aside class="notice">
You must replace `myaccesstoken` with your access token.
</aside>

# Reminders

## Get All Reminders

```shell
curl "http://followup.cc/api/v1/reminders.json"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@gotit.io",
    "action_method": "to",
    "send_reminder_to": "stuartchaney22@gmail.com",
    "in_reply_to": nil,
    "responded_to": false,
    "is_recurring": false,
    "repeat_interval": nil,
    "header_created_at": 1391326015,
    "was_scheduled_at": 1403978619,
    "starts_at": 1403978431,
    "cancelled_at": 1403979558,
    "expired_at": 1403979574,
    "user_email_id": 34,
    "user_email": "stuartchaney22@gmail.com"
  },
  {
    "id": 1873,
    "subject": "And another test reminder",
    "action_address": "1dayfollowup.cc",
    "action_method": "to",
    "send_reminder_to": "stuartchaney22@gmail.com",
    "in_reply_to": nil,
    "responded_to": false,
    "is_recurring": nil,
    "repeat_interval": nil,
    "header_created_at": 1396394129,
    "was_scheduled_at": 1396394157,
    "starts_at": 1396394189,
    "cancelled_at": 1403980210,
    "expired_at": 1396394197,
    "user_email_id": 34,
    "user_email": "stuartchaney22@gmail.com"
   }
]
```

This endpoint retrieves all reminders associated with your account.

### HTTP Request

`GET https://followup.cc/api/v1/reminders.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
email | false | If set the result will only include reminders for the given email.
scope | false | Pass a given scope to filter your reminders.

## Get a Specific Reminder

```shell
curl "https://followup.cc/api/v1/reminders/<ID>.json?access_token=myaccesstoken"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "subject": "Here's a test reminder",
  "action_address": "1minute@gotit.io",
  "action_method": "to",
  "send_reminder_to": "stuartchaney22@gmail.com",
  "in_reply_to": nil,
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": nil,
  "header_created_at": 1391326015,
  "was_scheduled_at": 1403978619,
  "starts_at": 1403978431,
  "cancelled_at": 1403979558,
  "expired_at": 1403979574,
  "user_email_id": 34,
  "user_email": "stuartchaney22@gmail.com"
}
```

This endpoint retrieves a specific reminder.

### HTTP Request

`GET https://followup.cc/api/v1/reminders/<ID>.json?access_token=myaccesstoken`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the reminder to retrieve

## Create a Reminder

```shell
curl "http://followup.cc/api/v1/reminders.json"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@gotit.io",
    "action_method": "to",
    "send_reminder_to": "stuartchaney22@gmail.com",
    "in_reply_to": nil,
    "responded_to": false,
    "is_recurring": false,
    "repeat_interval": nil,
    "header_created_at": 1391326015,
    "was_scheduled_at": 1403978619,
    "starts_at": 1403978431,
    "cancelled_at": 1403979558,
    "expired_at": 1403979574,
    "user_email_id": 34,
    "user_email": "stuartchaney22@gmail.com"
  }
]
```

This endpoint creates a reminder associated with your account.

### HTTP Request

`POST https://followup.cc/api/v1/reminders.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
to | true | The action to set a reminder with followup eg `1day@followup.cc`.
subject | true | The subject of the email.
body | true | The body of the email.
from | true | Which email you want to send the reminder from.

## Reschedule a Reminder

```shell
curl "http://followup.cc/api/v1/reminders/1/reschedule.json"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@gotit.io",
    "action_method": "to",
    "send_reminder_to": "stuartchaney22@gmail.com",
    "in_reply_to": nil,
    "responded_to": false,
    "is_recurring": false,
    "repeat_interval": nil,
    "header_created_at": 1391326015,
    "was_scheduled_at": 1403978619,
    "starts_at": 1403978431,
    "cancelled_at": 1403979558,
    "expired_at": 1403979574,
    "user_email_id": 34,
    "user_email": "stuartchaney22@gmail.com"
  }
]
```

This endpoint creates a reminder associated with your account.

### HTTP Request

`PUT https://followup.cc/api/v1/reminders/<ID>/reschedule.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
starts_at | true | The time the reminder will send as a UTC Unix timestamp.
