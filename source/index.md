---
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

## Reminder Object

A reminder object has the following fields:

Attribute | Type | Description
--------- | ------- | -----------
id | integer | The id of the reminder
subject | string | The subject of the email you sent to followup
action_address | string | The followup email address used to set the reminder. eg `1day@followup.cc`
action_method | string | The method followup recived your reminder email with. eg `"to", "cc", "bcc"`
send_reminder_to | string | Which of your emails the reminder will be sent to
responded_to | boolean | Whether the reminder has been responded to or not
is_recurring | boolean | Whether the reminder is to be repeated or not
repeat_interval | string | String representing the interval period of when the reminder should be repeated
header_created_at | timestamp | UTC Unix timestamp representation of when the reminders header was created at
was_scheduled_at | timestamp | UTC Unix timestamp representation of when the reminder was scheduled
starts_at | timestamp | UTC Unix timestamp representation of when the reminder starts
cancelled_at | timestamp | UTC Unix timestamp representation of when the reminder was cancelled
expired_at | timestamp | UTC Unix timestamp representation of when the reminder expired
user_email | hash | Hash of the user email who the reminder belongs to 


## Get All Reminders

```shell
$ curl "https://followup.cc/api/v1/reminders.json?access_token=myaccesstoken" \
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "type": "reminder.list",
  "total_count": 5,
  "pagination": {
    "next_page_number": null,
    "next_page_url": null,
    "total_pages": 1
  },
  "reminders": [
    {
      "id": 1,
      "subject": "Boom?",
      "action_address": "1minute@followup.cc",
      "action_method": "to",
      "send_reminder_to": "stuartchaney22@gmail.com",
      "responded_to": false,
      "is_recurring": false,
      "repeat_interval": null,
      "header_created_at": 1391326015,
      "was_scheduled_at": 1404685631,
      "starts_at": 1404685589,
      "cancelled_at": null,
      "expired_at": 1404685634,
      "user_email": {
        "id": 34,
        "email": "stuartchaney22@gmail.com",
        "is_primary": true,
        "user_id": 42,
        "is_confirmed": true,
        "created_at": 1393200248
      }
    }
  ]
}
```

This endpoint retrieves all reminders associated with your account.

### HTTP Request

`GET https://followup.cc/api/v1/reminders.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
email | false | If set the result will only include reminders for the given email.
scope | false | Pass a given scope to filter your reminders.

#### Scope Parameters

Scope paramaters filter your reminders and can also be chained together by passing multiple scope paramters in an array.


Parameter | Description
--------- | -------
past | Filter by past reminders
future | Filter by future reminders
recent | Filter by reminders from the past month
expired | Filter by expired reminders
cancelled | Filter by cancelled reminders
uncancelled | Filter by reminders that have not been cancelled
unscheduled | Filter by reminders that are yet to be scheduled
active | Filter by active reminders that are yet to be sent
responses | Filter by reminders that have been responded to (*Must have response detection enabled for your account*)
recurring | Filter by recurring reminders
non_recurring | Filter by reminders that are only sent once
was_scheduled | Filter by reminders that have been scheduled
this_month | Filter by reminders from this month


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
  "action_address": "1minute@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stuartchaney22@gmail.com",
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
curl "https://followup.cc/api/v1/reminders.json"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@followup.cc",
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
curl "https://followup.cc/api/v1/reminders/1/reschedule.json"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@followup.cc",
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


## Update a Reminder's Recipient

```shell
curl "https://followup.cc/api/v1/reminders/1/update_recipient.json"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@followup.cc",
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

This endpoint updates the recipient of the reminder.

### HTTP Request

`PUT https://followup.cc/api/v1/reminders/<ID>/update_recipient.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
email | true | The email address of the new recipient.


## Send the Reminder Now

```shell
 $ curl "https://followup.cc/api/v1/reminders/1/send_now.json?access_token=myaccesstoken" \
 -H 'Accept:application/json' \
 -H 'Content-Type:application/json' -d'
 {
   "cancel": true
 }'
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@followup.cc",
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

This endpoint sends the reminder straight away and also has the option
to cancel it too.

### HTTP Request

`PUT https://followup.cc/api/v1/reminders/<ID>/send_now.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cancel | false | a boolean field which indicates whether you would like to cancel the reminder at the time of sending.

## Snooze a Reminder

```shell
curl
"https://followup.cc/api/v1/reminders/1/snooze.json?seconds=60&access_token=blah"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@followup.cc",
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

This endpoint snoozes a reminder for a given time period.

### HTTP Request

`PUT https://followup.cc/api/v1/reminders/<ID>/snooze.json?seconds=60&access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
seconds | 0 | an integer representing how many seconds you would like the reminder snoozed for.

## Cancel a Reminder

```shell
curl
"https://followup.cc/api/v1/reminders/1.json?access_token=blah"
  -H "Authorization: myaccesstoken"
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "subject": "Here's a test reminder",
    "action_address": "1minute@followup.cc",
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

This endpoint cancels the remidner.

### HTTP Request

`PUT https://followup.cc/api/v1/reminders/<ID>.json?access_token=myaccesstoken`


# Users

## User Object

A user object has the following fields:

Attribute | Type | Description
--------- | ------- | -----------
id | integer | Id of the user
email | string | Email address of the user
first_name | string | First name of the user
last_name | string | Last name of the user
timezone | string | Timezone of the user
send_confirmation | boolean | Whether or not to send confirmation emails to the user upon creation of a reminder
store_message_body | boolean | Whether or not to send the body of the reminder in the reminder email for the user
communications | boolean | Whether or not to send announcements and newsletters to the user
reply_to_customization | boolean | Whether or not to set the reply-to the reminder's recipients instead of no-reply@followup.cc
include_attachments_customization | boolean | Whether or not to include any attachments sent in the reminder
snooze_bar_size_customization | string | What size the user's snooze bar is set to. 
day_start_hour | integer | Represents what time your day starts at
phone_number | string | User's phone number
is_confirmed | boolean | Whether the user is confirmed or not
created_at | timestamp | UTC Unix timestamp representation of when theuser was created


## Get the current user

```shell
$ curl "https://followup.cc/api/v1/users/current.json?access_token=myaccesstoken" \
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "id": 42,
  "email": "stuartchaney22@gmail.com",
  "first_name": null,
  "last_name": null,
  "timezone": "Pacific Time (US & Canada)",
  "send_confirmation": true,
  "store_message_body": false,
  "communications": true,
  "reply_to_customization": false,
  "include_attachments_customization": true,
  "snooze_bar_size_customization": "full",
  "day_start_hour": 7,
  "phone_number": null,
  "is_confirmed": true,
  "created_at": 1393200247
}
```

This endpoint retrieves all of the your user details.

### HTTP Request

`GET https://followup.cc/api/v1/users/current.json?access_token=myaccesstoken`


# User Emails 

## User Email Object

A user email object has the following fields:

Attribute | Type | Description
--------- | ------- | -----------
id | integer | Id of the user email
email | string | Email address of the user_email
is_primary | boolean | Whether the user_email is the primary user_email for the user
is_confirmed | string | Whether the user_email has been confirmed
created_at | timestamp | UTC Unix timestamp representation of when the user_email was created

## Get All User Emails

```shell
$ curl "https://followup.cc/api/v1/user_emails.json?access_token=myaccesstoken" \
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "type": "user_email.list",
  "total_count": 2,
  "pagination": {
    "next_page_number": null,
    "next_page_url": null,
    "total_pages": 1
  },
  "user_emails": [
    {
      "id": 34,
      "email": "stuartchaney22@gmail.com",
      "is_primary": true,
      "user_id": 42,
      "is_confirmed": true,
      "created_at": 1393200248
    }
  ]
}
```

This endpoint retrieves all of the your user details.

### HTTP Request

`GET https://followup.cc/api/v1/user_emails.json?access_token=myaccesstoken`

## Get a Specific User Email

```shell
$ curl "https://followup.cc/api/v1/user_emails/1.json?access_token=myaccesstoken" \
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
 {
    "id": 34,
    "email": "stuartchaney22@gmail.com",
    "is_primary": true,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200248
  }
```

This endpoint retrieves details of a specific user email.

### HTTP Request

`GET https://followup.cc/api/v1/user_emails/1.json?access_token=myaccesstoken`
