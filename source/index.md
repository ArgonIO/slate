---
language_tabs:
  - shell

toc_footers:
  - <a href='https://followup.cc'>Sign Up for a FollowUp Account</a>

includes:
  - errors

search: true
---

# API Reference

Welcome to the FollowUp API documentation!

You can use our API to access, create and update your reminders.

JSON will be returned in all responses from the API including errors. We currently have code examples using curl and will provide examples for ruby once we push our gem.

# Authentication

> To retreive an access token using an API key, use this code:

```shell
$ curl -i https://api.followup.cc/users/current.json?api_key=myapikey \
-H 'Accept:application/json'
```

> To retreive an access token using oauth, use this code:

```shell
$ curl -i https://followup.cc/oauth/token \
-F grant_type=password \
-F client_id="<yourclientid>" \
-F client_secret="<yourclientsecret>" \
-F username=<youremailaddress>\
-F password=<yourpassword>
```

> The above command returns JSON structured like this:

```json
{
  "access_token":"<youraccesstoken>",
  "token_type":"bearer",
  "expires_in":7200,
  "refresh_token":"<yourrefreshtoken>"
}
```
> To retreive a new access token after it has expired, use this code:

```shell
$ curl -i http://followup.cc/oauth/token \
-F grant_type=refresh_token \
-F client_id="<yourclientid>" \
-F client_secret="<yourclientsecret>" \
-F refresh_token="<yourrefreshtoken>"
```

> The above command returns JSON structured like this:

```json
{
  "access_token":"<yournewaccesstoken>",
  "token_type":"bearer",
  "expires_in":7200,
  "refresh_token":"<yournewrefreshtoken>"
}
```

FollowUp provides two methods of accesing the API, via an API key or Oauth access token.

### API Keys

We recommend this option if you only need to access your own account.

API keys are available [here](https://followup.cc/api).

FollowUp expects for the api key to be included in all API requests that looks like the following:

`https://api.followup.cc/v1/users/current.json?api_key=myapikey`


### Oauth

We recommend this option if your application needs to access other user's accounts.

FollowUp expects for the access token key to be included in all API requests that looks like the following:

`https://api.followup.cc/v1/users/current.json?access_token=myaccesstoken`

Access tokens expire 2 hours after creation. New Access tokens can be obtained using refresh tokens which is detailed in the CURL example to the right.

<aside class="notice">
You must replace `myaccesstoken` and `myapikey` with your access token
or api key.
</aside>

# Reminders

## Reminder object

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
to_addresses | array | List of addresses present in 'to' field, when sending an email
cc_addresses | array | List of addresses present in 'cc' field
body_html | string | Content of the email, represented in html structure
body_text | string | Content of the email, without html tags
attachments | array | List of attachments. In order to download them, use links from reminder object, with current access token in params.


## Get all reminders

```shell
$ curl "https://api.followup.cc/v1/reminders.json?access_token=myaccesstoken&scope[]=past&scope[]=cancelled&email=stu@argon.io" \
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
      "subject": "This is a test reminder",
      "action_address": "1day@followup.cc",
      "action_method": "to",
      "send_reminder_to": "stu@argon.io",
      "responded_to": false,
      "is_recurring": false,
      "repeat_interval": null,
      "header_created_at": 1404709308,
      "was_scheduled_at": 1404709358,
      "starts_at": 1404795791,
      "cancelled_at": null,
      "expired_at": null,
      "body_html": "<div dir=\"ltr\">message example</div>\r\n",
      "body_text": "message example\r\n",
      "to_addresses": ["1min@followup.cc"],
      "cc_addresses": ["hello@example.cc"],
      "attachments": [
        {
          "file_name": "panda.jpg",
          "file_url": "https://api.followup.cc/v1/attachments/411861/panda.jpg",
          "content_type": "image/jpeg"
        }
      ],
      "user_email": {
        "id": 34,
        "email": "stu@argon.io",
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

`GET https://api.followup.cc/v1/reminders.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
email | string | false | If set the result will only include reminders for the given email.
scope | array | false | Pass a given scope to filter your reminders.

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


## Get a specific reminder

```shell
$ curl "https://api.followup.cc/v1/reminders/1.json?access_token=myaccesstoken"
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "subject": "This is a test reminder",
  "action_address": "1day@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stu@argon.io",
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": null,
  "header_created_at": 1404709308,
  "was_scheduled_at": 1404709358,
  "starts_at": 1404795791,
  "cancelled_at": null,
  "expired_at": null,
  "user_email": {
    "id": 34,
    "email": "stu@argon.io",
    "is_primary": true,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200248
  }
}
```

This endpoint retrieves a specific reminder.

### HTTP Request

`GET https://api.followup.cc/v1/reminders/1.json?access_token=myaccesstoken`

### URL Parameters

Parameter | Required | Description
--------- | ---------| -----------
ID | true | The ID of the reminder to retrieve

## Create a reminder

```shell
curl
"https://api.followup.cc/v1/reminders.json?access_token=myaccesstoken" \
  -X POST \
  -H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "status": 201,
  "header": {
    "Content-Type": "text/json",
    "Content-Length": "15"
  },
  "chunked": false,
  "writer": {},
  "block": null,
  "length": 15,
  "body": [
    "Reminder queued"
  ]
}
```

This endpoint creates a reminder associated with your account.

### HTTP Request

`POST https://api.followup.cc/v1/reminders.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Type |  Required | Description
--------- | ------- | ---------| -----------
to | string | true | The action to set a reminder with followup eg `1day@followup.cc`. 
subject | string | true | The subject of the email.
body | string | true | The body of the email.
from | string | true | Which email you want to send the reminder from.

## Reschedule a reminder

```shell
$ curl "https://api.followup.cc/v1/reminders/1/reschedule.json?access_token=myaccesstoken" \
-X PUT \
-H 'Accept:application/json' \
-H 'Content-Type:application/json' -d'
{
  "starts_at": 1404706692
}'
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "subject": "This is a test reminder",
  "action_address": "1day@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stu@argon.io",
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": null,
  "header_created_at": 1404709308,
  "was_scheduled_at": 1404709358,
  "starts_at": 1404706692,
  "cancelled_at": null,
  "expired_at": null,
  "user_email": {
    "id": 34,
    "email": "stu@argon.io",
    "is_primary": true,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200248
  }
}
```

This endpoint creates a reminder associated with your account.

### HTTP Request

`PUT https://api.followup.cc/v1/reminders/1/reschedule.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Type |  Required |Description
--------- | ------- | ---------| -----------
starts_at | timestamp | true | The time the reminder will send as a UTC Unix timestamp.


## Update a reminder's recipient

```shell
$ curl "https://api.followup.cc/v1/reminders/1/update_recipient.json?access_token=myaccesstoken" \
-X PUT \
-H 'Accept:application/json' \
-H 'Content-Type:application/json' -d'
{
  "email": stuartdoe@gmail.com
}'
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "subject": "This is a test reminder",
  "action_address": "1day@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stuartdoe@gmail",
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": null,
  "header_created_at": 1404709308,
  "was_scheduled_at": 1404709358,
  "starts_at": 1404795791,
  "cancelled_at": null,
  "expired_at": null,
  "user_email": {
    "id": 35,
    "email": "stuartdoe@gmail.com",
    "is_primary": false,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200398
  }
}
```

This endpoint updates the recipient of the reminder. 

### HTTP Request

`PUT https://api.followup.cc/v1/reminders/1/update_recipient.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
email | true | The email address of the new recipient. This email address must be an email address from your account.


## Send the reminder now

```shell
 $ curl "https://api.followup.cc/v1/reminders/1/send_now.json?access_token=myaccesstoken" \
 -X PUT \
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
  "subject": "This is a test reminder",
  "action_address": "1day@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stu@argon.io",
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": null,
  "header_created_at": 1404709308,
  "was_scheduled_at": 1404709358,
  "starts_at": 1404795791,
  "cancelled_at": null,
  "expired_at": null,
  "user_email": {
    "id": 34,
    "email": "stu@argon.io",
    "is_primary": true,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200248
  }
}
```

This endpoint sends the reminder straight away and also has the option
to cancel it too.

### HTTP Request

`PUT https://api.followup.cc/v1/reminders/1/send_now.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
cancel | false | a boolean field which indicates whether you would like to cancel the reminder at the time of sending.

## Snooze a reminder

```shell
$ curl "https://api.followup.cc/v1/reminders/1/send_now.json?access_token=myaccesstoken" \
 -X PUT \
 -H 'Accept:application/json' \
 -H 'Content-Type:application/json' -d'
 {
   "seconds": 1800
 }'
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "subject": "This is a test reminder",
  "action_address": "1day@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stu@argon.io",
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": null,
  "header_created_at": 1404709308,
  "was_scheduled_at": 1404709358,
  "starts_at": 1404762736,
  "cancelled_at": null,
  "expired_at": null,
  "user_email": {
    "id": 34,
    "email": "stu@argon.io",
    "is_primary": true,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200248
  }
}
```

This endpoint snoozes a reminder for a given time period. The current reminder will be expired and a new reminder will be created and returned.

### HTTP Request

`PUT https://api.followup.cc/v1/reminders/1/snooze.json?access_token=myaccesstoken`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
seconds | 0 | an integer representing how many seconds you would like the reminder snoozed for.

## Cancel a reminder

```shell
$ curl "https://api.followup.cc/v1/reminders/1.json?access_token=myaccesstoken" \
 -X DELETE \
 -H 'Accept:application/json' \
 -H 'Content-Type:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "id": 1,
  "subject": "This is a test reminder",
  "action_address": "1day@followup.cc",
  "action_method": "to",
  "send_reminder_to": "stu@argon.io",
  "responded_to": false,
  "is_recurring": false,
  "repeat_interval": null,
  "header_created_at": 1404709308,
  "was_scheduled_at": 1404709358,
  "starts_at": 1404795791,
  "cancelled_at": 1404762901,
  "expired_at": null,
  "user_email": {
    "id": 34,
    "email": "stu@argon.io",
    "is_primary": true,
    "user_id": 42,
    "is_confirmed": true,
    "created_at": 1393200248
  }
}
```

This endpoint cancels the remidner.

### HTTP Request

`PUT https://api.followup.cc/v1/reminders/1.json?access_token=myaccesstoken`


# Users

## User object

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
created_at | timestamp | UTC Unix timestamp representation of when the user was created


## Get the current user

```shell
$ curl "https://api.followup.cc/v1/users/current.json?access_token=myaccesstoken" \
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "id": 42,
  "email": "stu@argon.io",
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

`GET https://api.followup.cc/v1/users/current.json?access_token=myaccesstoken`


# User Emails 

## User email object

A user email object has the following fields:

Attribute | Type | Description
--------- | ------- | -----------
id | integer | Id of the user email
email | string | Email address of the user_email
is_primary | boolean | Whether the user_email is the primary user_email for the user
is_confirmed | string | Whether the user_email has been confirmed
created_at | timestamp | UTC Unix timestamp representation of when the user_email was created

## Get all user emails

```shell
$ curl "https://api.followup.cc/v1/user_emails.json?access_token=myaccesstoken" \
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
      "email": "stu@argon.io",
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

`GET https://api.followup.cc/v1/user_emails.json?access_token=myaccesstoken`

## Get a specific user email

```shell
$ curl "https://api.followup.cc/v1/user_emails/1.json?access_token=myaccesstoken" \
-H 'Accept:application/json'
```

> The above command returns JSON structured like this:

```json
{
  "id": 34,
  "email": "stu@argon.io",
  "is_primary": true,
  "user_id": 42,
  "is_confirmed": true,
  "created_at": 1393200248
}
```

This endpoint retrieves details of a specific user email.

### HTTP Request

`GET https://api.followup.cc/v1/user_emails/1.json?access_token=myaccesstoken`
