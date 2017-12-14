The ThePersuit API opens up the opportunity for developers to:

* Offer a group payments option in their applications
* Offer a pre-sales commerce option in their applications.
* Develop a crowdfunding application (i.e., single project model like Lockitron or multi-project model like Kickstarter).
* Develop a social fundraising application.
* Enable the collaboration around these styles of commerce.

What we think is cool about the ThePersuit API:

* Out of the box the ThePersuit API works with Stripe. (Check out our note on the transition from Balanced Payments)
* Support for multiple currencies and international languages 
* The API works with credit cards.
* The API provides collaboration tools such as commenting/updates, nested comments, messaging, notifications, and tracking of those that have paid and those that haven't.
* We also provide a tool to tokenize the sensitive information you collect (credit card and bank account numbers), so you don't have to worry about PCI compliance.


# Menu 
* [Introduction](Introduction)
* Security
* Use-Cases
* Getting Started
* User Resources
* Users
* User Campaigns
* User Cards
* User Banks
* User Payments
* Campaign Resources
* Campaigns
* Campaign Payments
* Rejected Payments
* Refunds
* Campaign Settlements
* Campaign Comments
* API Examples
* Tokenizing Sensitive User Information
* Resource Definitions
* User Definition
* Card Definition
* Bank Definition
* Campaign Definition
* Payment Definition
* Settlement Definition
* Pagination
* FAQs
* Terms of Use

# Introduction

 ThePersuit API is a layer of abstraction for campaign managment and a range of payment processors tailored for charitable campaigns.  Currently, we support a range of paument processors and locations including:
 *  **Stripe**, which is well-suited for single-project sites like Lockitron, or multi-project services such as Kickstarter
 * Credit cards Mastercard/Visa 
 * Crypto Currencies - comming soon



The API defines 3 main objects: 
* campaign - including social media interaction and tracking 
* user/profile management 
* payment 

At a very high-level, a campaign represents a monetary goal that one or more users try to achieve through payments. The campaign can define multiple payment policies such as fixed payments, e.g, tickets, or variable payments, e.g., donations.


The campaign can be for an individual or group. There will be one legal owner > 18 as the campaign admin. Once the campaign goal is achieved, the money is sent to the campaign admin according to the policies defined when the campaign was created.

Here is a full definition for each one of those objects: campaign, user, payment.

## Security 

**ThePersuit** is fully PCI compliant and security. ThePersuit 

* forces HTTPS for all services, including our public website. 
* All data is stored in a Payment Industry Data Security Standard (PCI DSS) Compliant environment
* We avoid storing unnecessary data with card details removed once cleared by the payment companies


If you believe you've found any security issues, please email us at security@ThePersuit.com. Though usually faster, we guarantee a direct response within 24 hours. We ask that you do not disclose a security bug publicly until it has been addressed by our team.

## Use Cases

The ThePersuit API platform  can be used in a:

* single-project environment (i.e. app.net/lockitron.com), 
* a multi-project environment (a la Kickstarter), or for simple Group Payments. This includes things like group vacation bookings/rentals, wedding gifts, bachelor parties, and the list goes on. We know we're only scratching the surface of what can be done with this API, and we're sure our users will find countless other creative and clever ways to utilize our API.


## Getting Started 

### Sandbox Environment

To help you build your application, we provide a sandbox environment that you can use for testing. Use the base URI below:

https://api-sandbox.ThePersuit.com/v1
The sandbox environment will be configured with a free Stripe account. If you need support for a different payment processor, please let us know.

### Authentication 

We are big fans of simplicity. Therefore, we support Basic Authentication over SSL. You can simply use curl to test our API in 2 seconds.

$ curl -u API_KEY:API_SECRET https://api-sandbox.ThePersuit.com/v1
If the credentials provided were invalid, the API will respond with

`
401 => Unauthorized
Data Formats
`

Currently, we only support JSON data formats. If you think we should support other data formats, please let us know what and why support.api@ThePersuit.com.

### Getting involved 

We need your help and we would love to get you involved. Feel free to create github issues, give feedback and comment, submit pull requests, or review specs. You can even ask for new features and vote on the ones you like to be a higher priority.

When is feature X going to be in production?

Any proposed spec change will be in a separate "feature" branch. Once we have a consensus from the community on the spec change, it'll be merged into dev branch, which means that the ThePersuit team will start working on it. Once it is fully implemented and ready to go to production, it'll get merged to the master branch.

Using Metadata

All resources contain a metadata field for storing key-value pairs of extra data. Store as many of these key-value pairs as you wish.

Some common uses of this field include storing extra user data, such as address fields or profile image urls, or storing extra campaign data, such as a campaign description field or campaign image url.

Important Note: Updating the metadata field completely overwrites its contents. Be sure to include the entirety of the data you wish to store when making an update, including key-value pairs that did not change.

## User Resources

| Path        | HTTP Methods  | Description  |
| ------------- |:-------------|:-----|
| /users	| GET POST	| List of users | 
| 	/users/:id	| 	GET PUT	A specific user	| 
| 	/users/:id/verification	| 	POST	Verify a user to receive payments	| 
| 	/users/:id/campaigns	| 	GET	All campaigns this user is admin of	| 
| 	/users/:id/cards	| 	GET POST	User credit cards	| 
| 	/users/:id/cards/:id	| 	GET PUT DELETE	A specific credit card	| 
| 	/users/:id/banks	| 	GET POST	User bank accounts	| 
| 	/users/:id/banks/:id	| 	GET PUT DELETE	A specific bank account	| 
| 	/users/:id/payments	| 	GET	A list of Users payments	| 



## Users

### Create User 

The minimum amount of information needed to create a user is a valid email address.

Keep in mind that the metadata field is a great place to store references to other user assets, such as a profile image.

POST /users
Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users \
    -d'
    {
       "user" : {
          "firstname" : "foo",
          "lastname" : "bar",
          "email" : "user@example.com",
          "metadata" : { "img" : "http://www.example.com/path-to-profile-image" }
       }
    }'

Response Body

    {
        "user": {
            "id": "USREC5",
            "email": "foo.bar@gmail.com",
            "firstname": "Foo",
            "lastname": "Bar",
            "is_verified": 0,
            "creation_date": "2011-07-02T14:20:48Z",
            "modification_date": "2011-09-02T14:20:48Z",
            "uri": "/v1/users/USREC5",
            "cards_uri": "/v1/users/USREC5/cards",
            "banks_uri": "/v1/users/USREC5/banks",
            "campaigns_uri": "/v1/users/USREC5/campaigns",
            "payments_uri": "/v1/users/USREC5/payments",
            "metadata" : { "img" : "http://www.example.com/path-to-profile-image" }
        }
    }

Response Codes

    201 => Created
    400 => Bad Request
    Verify User

In order for a user to receive money raised from a campaign, the user's identity needs to be verified. A user is verified by POSTing verification data to the /users/:id/verification.

The verification data is as follows:

* **name** - should be the real name of the user
* **dob** - the date of birth of the user in the format YYYY-MM
* **phone_number** - the user's phone number
* **street_address** - the user's street address
* **postal** code - the user's postal code

In some cases, if the user cannot be fully verified with this information, we may return a 449 response indicating that we need more info. In these cases you should re-post the data with the additional ssn field containing the user's social security number for additional verification.

Once the verification has succeeded, is_verified on the user object will be set to 1 to reflect this change.

    POST /users/:id/verification

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USREC5/verification \
    -d'
    {
       "verification" : {
          "name" : "Khaled Hussein",
          "dob" : "1984-07",
          "phone_number" : "(000) 000-0000",
          "street_address" : "324 awesome address, awesome city, CA",
          "postal_code" : "12345"
       }
    }'

Response Body

    {}

Response Codes

    200 => OK
    400 => Bad Data, or Could not Verify admin information
    449 => Retry with SSN
    Get User

    GET  /users/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USREC5

Response Body

    {
        "user": {
            "id": "USREC5",
            "email": "foo.bar@gmail.com",
            "firstname": "Foo",
            "lastname": "Bar",
            "is_verified": 0,
            "creation_date": "2011-07-02T14:20:48Z",
            "modification_date": "2011-09-02T14:20:48Z",
            "uri": "/v1/users/USREC5",
            "cards_uri": "/v1/users/USREC5/cards",
            "banks_uri": "/v1/users/USREC5/banks",
            "campaigns_uri": "/v1/users/USREC5/campaigns",
            "payments_uri": "/v1/users/USREC5/payments",
            "metadata" : { "img" : "http://www.example.com/path-to-profile-image" }
        }
    }

Response Codes

    200 => OK

List Users

    GET /users

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users

Response Body

    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        },
        "users": [
            {
                "id": "USREC5",
                "email": "foo.bar@gmail.com",
                "firstname": "Foo",
                "lastname": "Bar",
                "is_verified": 0,
                "creation_date": "2011-07-02T14:20:48Z",
                "modification_date": "2011-09-02T14:20:48Z",
                "uri": "/v1/users/USREC5",
                "cards_uri": "/v1/users/USREC5/cards",
                "banks_uri": "/v1/users/USREC5/banks",
                "campaigns_uri": "/v1/users/USREC5/campaigns",
                "payments_uri": "/v1/users/USREC5/payments",
                "metadata" : { "img" : "http://www.example.com/path-to-profile-image" }
            },
            .
            .
            .
        ]
    }

Response Codes

    200 => OK
    Update User

Currently, this request supports partial PUTs. For example, you can do a request to update a single attribute without having to send the full user object.

    PUT /users/:id
    Example Request

    $ curl -X PUT -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USREC5 \
    -d'
    {
        "user": {
            "lastname": "new last name"
        }
    }'

Response Body

    {
        "user": {
            "id": "USREC5",
            "email": "foo.bar@gmail.com",
            "firstname": "Foo",
            "lastname": "new last name",
            "is_verified": 0,
            "creation_date": "2011-07-02T14:20:48Z",
            "modification_date": "2011-09-02T14:20:48Z",
            "uri": "/v1/users/USREC5",
            "cards_uri": "/v1/users/USREC5/cards",
            "banks_uri": "/v1/users/USREC5/banks",
            "campaigns_uri": "/v1/users/USREC5/campaigns",
            "payments_uri": "/v1/users/USREC5/payments",
            "metadata" : { "img" : "http://www.example.com/path-to-profile-image" }
        }
    }

Response Codes

    200 => OK
    User Campaigns

#### Get User Campaigns

This resource returns all the campaigns that the user has created as well as the campaigns that he paid for.

    GET /users/:id/campaigns

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USREC5/campaigns
    Response Body

    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        },
        "campaigns": [
            {
                "id": "CMPBDA",
                "title": "Campaign Title",
                "ThePersuit_amount": 100,
                "min_payment_amount": 0,
                "fixed_payment_amount": 0,
                "expiration_date": "2000-01-02T01:02:03Z",
                "is_ThePersuited": 0,
                "is_paid": 0,
                "is_expired": 0,
                "needs_bank": 0,
                "uri": "/v1/campaigns/CMPBDA",
                "payments_uri": "/v1/campaigns/CMPBDA/payments",
                "settlements_uri": "/v1/campaigns/CMPBDA/settlements",
                "admin": { "id": "USREC5", "uri": "/v1/users/USREC5", ... },
                "first_contributor": null,
                "ThePersuiter": null,
                "stats": {
                    "ThePersuit_percent": 0,
                    "raised_amount": 0,
                    "unique_contributors": 0,
                    "number_of_contributions": 0
                },
                "creation_date": "2011-07-02T14:20:48Z",
                "modification_date": "2011-09-02T14:20:48Z",
                "metadata": { }
            },
            .
            .
            .
        ]
    }

Response Codes

    200 => OK
    User Cards

Create User Card

    POST /users/:id/cards
    Example Request

    $ curl -X POST -u key:secret -H Content-Type:application/json \
    https://api-sandbox.ThePersuit.com/v1/users/USR50A/cards \
    -d'
    {
        "card": {
            "number": "4111111111111111",
            "expiration_month": "01",
            "expiration_year": "2023",
            "security_code": "123",
            "postal_code": "12345"
        }
    }'

Response Body

    {
        "card": {
            "id" : "CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0",
            "last_four" : "1111",
            "expiration_year" : 2023,
            "expiration_month" : "01",
            "user": { "id" : "USR50A", "uri" : "/v1/users/USR50A", ... },
            "uri" : "/v1/users/USR50A/cards/CCP6D6",
            "card_type" : "VISA card",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {}
        }
    }

Response Codes

    201 => Created

#### Get User Card

    GET /users/:id/cards/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR50A/cards/CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0

Response Body

    {
        "card": {
            "id" : "CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0",
            "last_four" : "1111",
            "expiration_year" : 2023,
            "expiration_month" : "01",
            "user": { "id" : "USR50A", "uri" : "/v1/users/USR50A", ... },
            "uri" : "/v1/users/USR50A/cards/CCP6D6",
            "card_type" : "VISA card",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {}
        }
    }

Response Codes

    200 => OK

#### List User Cards

    GET /users/:id/cards

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR50A/cards

Response Body

    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        },
        "cards" : [
              {
                 "id" : "CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0",
                 "last_four" : "1111",
                 "expiration_year" : 2023,
                 "expiration_month" : "01",
                 "user": { "id" : "USR50A", "uri" : "/v1/users/USR50A", ... },
                 "uri" : "/v1/users/USR50A/cards/CCP6D6",
                 "card_type" : "VISA card",
                 "creation_date" : "2012-08-23T07:42:46.134467000Z",
                 "modification_date" : "2012-09-23T07:42:46.134467000Z",
                 "metadata" : {}
              },
              .
              .
              .
        ]
    }

Response Codes

    200 => OK

#### Update User Card

Card information cannot be updated once it is set. You can however, modify the metadata of a Card. That is the only thing modifiable with this request. Other fields submitted will be ignored.

    PUT /users/:id/cards/:id

Example Request

    $ curl -X PUT -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR50A/cards/CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0 \
    -d'
    {
        "card": {
            "metadata" : {
                "key1" : "value1"
            }
        }
    }'

Response Body

    {
        "card": {
            "id" : "CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0",
            "last_four" : "1111",
            "expiration_year" : 2023,
            "expiration_month" : "01",
            "user": { "id" : "USR50A", "uri" : "/v1/users/USR50A", ... },
            "uri" : "/v1/users/USR50A/cards/CCP6D6",
            "card_type" : "VISA card",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {
                "key1" : "value1"
            }
        }
    }

Response Codes

    200 => OK

### Delete User Card

DELETE /users/:id/cards/:id

Example Request

    $ curl -X DELETE -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR50A/cards/CCP6D6E7E7C0C5C11E2BD7001E2CFE628C0

Response Codes

    200 => OK

####User Banks

Note that the bank_code field is also referred to as a "routing number" in the USA.

#### Create User Bank

    POST /users/:id/banks

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks \
    -d'
    {
        "bank": {
            "account_number" : "1234567890",
            "name" : "John Smith",
            "bank_code" : "321174851"
        }
    }

Response Body

    {
        "bank": {
            "id" : "BAP688",
            "account_number_last_four" : "7890",
            "bank_code_last_four" : "4851",
            "name" : "John Smith",
            "is_default" : 0,
            "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
            "uri" : "/v1/users/USR54B/banks/BAP688",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {}
        }
    }

Response Codes

    201 => Created

### Make User Bank Default

Before a bank account will be sent money, it must be marked as the default bank account for that user. To make a bank account the default, make the following request:

    POST /users/:id/banks/default

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks/default \
    -d'
    {
        "bank": { "id" : "BAP688" }
    }'
    Response Body
    
    {
        "bank": {
            "id" : "BAP688",
            "account_number_last_four" : "7890",
            "bank_code_last_four" : "4851",
            "name" : "John Smith",
            "is_default" : 1,
            "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
            "uri" : "/v1/users/USR54B/banks/BAP688",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {}
        }
    }

Response Codes

    200 => OK

Get User's Default Bank

To get the current default bank for a user, you can simply request:

    GET /users/:id/banks/default

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks/default

Response Body

    {
        "bank": {
            "id" : "BAP688",
            "account_number_last_four" : "7890",
            "bank_code_last_four" : "4851",
            "name" : "John Smith",
            "is_default" : 1,
            "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
            "uri" : "/v1/users/USR54B/banks/BAP688",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {}
        }
    }

Response Codes

    200 => OK

#### Get User Bank

    GET /users/:id/banks/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks/BAP688

Response Body

    {
        "bank" : {
            "id" : "BAP688",
            "account_number_last_four" : "7890",
            "bank_code_last_four" : "4851",
            "name" : "John Smith",
            "is_default" : 0,
            "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
            "uri" : "/v1/users/USR54B/banks/BAP688",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {}
        }
    }

Response Codes

    200 => OK

#### List User Banks

This resource lists the bank accounts associated with this user.

    GET /users/:id/banks

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks

Response Body

    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        },
        "banks": [
            {
                "id" : "BAP688",
                "account_number_last_four" : "7890",
                "bank_code_last_four" : "4851",
                "name" : "John Smith",
                "is_default" : 0,
                "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
                "uri" : "/v1/users/USR54B/banks/BAP688",
                "creation_date" : "2012-08-23T07:42:46.134467000Z",
                "modification_date" : "2012-09-23T07:42:46.134467000Z",
                "metadata" : {}
            },
            .
            .
            .
        ]
    }

Response Codes

    200 => OK

####  Update User Bank

Bank information cannot be updated once it is set. You can however, modify the metadata of a bank account. That is the only thing modifiable with this request. Other fields submitted will be ignored.

    PUT /users/:id/banks/:id

Example Request

    $ curl -X PUT -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks/BAP688 \
    -d'
    {
        "bank" : {
            "metadata" : {
                "key1" : "value1"
            }
        }
    }

Response Body

    {
        "bank" : {
            "id" : "BAP688",
            "account_number_last_four" : "7890",
            "bank_code_last_four" : "4851",
            "name" : "John Smith",
            "is_default" : 0,
            "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
            "uri" : "/v1/users/USR54B/banks/BAP688",
            "creation_date" : "2012-08-23T07:42:46.134467000Z",
            "modification_date" : "2012-09-23T07:42:46.134467000Z",
            "metadata" : {
                "key1" : "value1"
            }
        }
    }

Response Codes

    200 => OK

Delete User Bank

DELETE /users/:id/banks/:id
Example Request

    $ curl -X DELETE -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/banks/BAP688

Response Codes

    200 => OK

User Payments

List User Payments

    GET /users/:id/payments

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/users/USR54B/payments

Response Body

    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        },
        "payments": [
            {
              "id" : "CON234",
              "status" : "charged",
              "amount" : 2000,
              "user_fee_amount" : 40,
              "admin_fee_amount" : 40,
              "uri" : "/v1/campaigns/CMP96B/payments/CON234",
              "campaign" : { "id": "CMP96B", "uri" : "/v1/campaigns/CMP96B", ... },
              "card" : { "id" : "CCPC41", "uri" : "/v1/users/USR521/cards/CCPC42", ... },
              "user": { "id" : "USR54B", "uri" : "/v1/users/USR54B", ... },
              "creation_date" : "2012-10-20T15:45:13Z",
              "modification_date" : "2012-10-20T15:45:47Z",
              "metadata" : {}
            },
            .
            .
            .
        ]
    }

Response Codes

    200 => OK

# Campaign Resources

* [Campaigns](Campains)
* [Campaign Payments](Campaign Payments)
* [Rejected Payments](Rejected Payments)
* [Refunds](Refunds)
* [Campaign Settlements](Campaign Settlements)
* [Campaign Comments](Campaign Comments])

| 	Path	| 	HTTP Methods	| 	Description	| 
| 	:------------	| 	:------------	| 	:------------	| 
| 	/campaigns	| 	GET POST	| 	List of campaigns	| 
| 	/campaigns/:id	| 	GET PUT	| 	A specific campaign	| 
| 	/campaigns/:id/payments	| 	GET POSTPUT	| 	Campaign Payments	| 
| 	/campaigns/:id/rejected_payments	| 	GET	| 	Rejected Payments	| 
| 	/campaigns/:id/payments/:id	| 	GET	| 	Details about a specific payment	| 
| 	/campaigns/:id/payments/:id/refund	| 	POST	| 	Refunding a specific payment	| 
| 	/campaigns/:id/settlements	| 	GET	| 	Campaign Settlements	| 
| 	/campaigns/:id/settlements/:id	| 	GET	| 	Campaign Settlement	| 
| 	/campaigns/:id/settlements/:id/bank	| 	POST	| 	Update a Campaign Settlement Bank	| 
| 	/campaigns/:id/comments	| 	GET POST	| 	Campaign Comments	| 
| 	/campaigns/:id/comments/:id	| 	GET PUTDELETE	| 	Details about (or delete) a specific comment	| 



## Create Campaign

A campaign needs to be associated to a user. We refer to this user as the "campaign admin". Campaign admins can create campaigns without having to be verified. However, they need to be verified in order to set up their bank account details and then be able to receive the money collected in their campaign.

The metadata field is a great place to store references to other campaign assets, such as a campaign image or description.

    POST /campaigns

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns \
    -d'
    {
        "campaign": {
            "user_id":"USREC5",
            "title":"Campaign Title",
            "expiration_date":"2000-01-02T01:02:03Z",
            "ThePersuit_amount":100,
            "metadata" : { "img" : "http://www.example.com/path-to-campaign-image" }
        }
    }'

Response Body

    {
        "campaign": {
            "id": "CMPBDA",
            "title": "Campaign Title",
            "ThePersuit_amount": 100,
            "min_payment_amount": 0,
            "fixed_payment_amount": 0,
            "expiration_date": "2000-01-02T01:02:03Z",
            "is_ThePersuited": 0,
            "is_paid": 0,
            "is_expired": 0,
            "needs_bank": 0,
            "uri": "/v1/campaigns/CMPBDA",
            "payments_uri": "/v1/campaigns/CMPBDA/payments",
            "settlements_uri": "/v1/campaigns/CMPBDA/settlements",
            "admin": { "id": "USREC5", "uri": "/v1/users/USREC5", ... },
            "first_contributor": null,
            "ThePersuiter": null,
            "stats": {
                "ThePersuit_percent": 0,
                "raised_amount": 0,
                "unique_contributors": 0,
                "number_of_contributions": 0
            },
            "creation_date": "2011-07-02T14:20:48Z",
            "modification_date": "2011-09-02T14:20:48Z",
            "metadata" : { "img" : "http://www.example.com/path-to-campaign-image" }
        }
    }

Response Codes

    201 => Created

Get Campaign

    GET  /campaigns/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPBDA
    Response Body
    
    {
        "campaign": {
            "id": "CMPBDA",
            "title": "Campaign Title",
            "ThePersuit_amount": 100,
            "min_payment_amount": 0,
            "fixed_payment_amount": 0,
            "expiration_date": "2000-01-02T01:02:03Z",
            "is_ThePersuited": 0,
            "is_paid": 0,
            "is_expired": 0,
            "needs_bank": 0,
            "uri": "/v1/campaigns/CMPBDA",
            "payments_uri": "/v1/campaigns/CMPBDA/payments",
            "settlements_uri": "/v1/campaigns/CMPBDA/settlements",
            "admin": { "id": "USREC5", "uri": "/v1/users/USREC5", ... },
            "first_contributor": null,
            "ThePersuiter": null,
            "stats": {
                "ThePersuit_percent": 0,
                "raised_amount": 0,
                "unique_contributors": 0,
                "number_of_contributions": 0
            },
            "creation_date": "2011-07-02T14:20:48Z",
            "modification_date": "2011-09-02T14:20:48Z",
            "metadata" : { "img" : "http://www.example.com/path-to-campaign-image" }
        }
    }

Response Codes

    200 => OK

List campaigns

    GET /campaigns

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns
    Response Body
    
    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        },
        "campaigns": [
            {
                "id": "CMPBDA",
                "title": "Campaign Title",
                "ThePersuit_amount": 100,
                "min_payment_amount": 0,
                "fixed_payment_amount": 0,
                "expiration_date": "2000-01-02T01:02:03Z",
                "is_ThePersuited": 0,
                "is_paid": 0,
                "is_expired": 0,
                "needs_bank": 0,
                "uri": "/v1/campaigns/CMPBDA",
                "payments_uri": "/v1/campaigns/CMPBDA/payments",
                "settlements_uri": "/v1/campaigns/CMPBDA/settlements",
                "admin": { "id": "USREC5", "uri": "/v1/users/USREC5", ... },
                "first_contributor": null,
                "ThePersuiter": null,
                "stats": {
                    "ThePersuit_percent": 0,
                    "raised_amount": 0,
                    "unique_contributors": 0,
                    "number_of_contributions": 0
                },
                "creation_date": "2011-07-02T14:20:48Z",
                "modification_date": "2011-09-02T14:20:48Z",
                "metadata" : { "img" : "http://www.example.com/path-to-campaign-image" }
            },
            .
            .
            .
        ]
    }

Response Codes

    200 => OK

### Update Campaign

Currently, this request supports partial PUTs. For example, you can do a request to update a single attribute without having to send the full campaign object.

    PUT /campaigns/:id

Example Request

    $ curl -X PUT -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPBDA \
    -d'
    {
        "campaign": {
            "title":"A Different Campaign Title"
        }
    }'
    Response Body
    
    {
        "campaign": {
            "id": "CMPBDA",
            "title": "A Different Campaign Title",
            "ThePersuit_amount": 100,
            "min_payment_amount": 0,
            "fixed_payment_amount": 0,
            "expiration_date": "2000-01-02T01:02:03Z",
            "is_ThePersuited": 0,
            "is_paid": 0,
            "is_expired": 0,
            "needs_bank": 0,
            "uri": "/v1/campaigns/CMPBDA",
            "payments_uri": "/v1/campaigns/CMPBDA/payments",
            "settlements_uri": "/v1/campaigns/CMPBDA/settlements",
            "admin": { "id": "USREC5", "uri": "/v1/users/USREC5", ... },
            "first_contributor": null,
            "ThePersuiter": null,
            "stats": {
                "ThePersuit_percent": 0,
                "raised_amount": 0,
                "unique_contributors": 0,
                "number_of_contributions": 0
            },
            "creation_date": "2011-07-02T14:20:48Z",
            "modification_date": "2011-09-02T14:20:48Z",
            "metadata" : { "img" : "http://www.example.com/path-to-campaign-image" }
        }
    }

Response Codes

    200 => OK

Campaign Payments

### Create campaign payment

Before a user is able to contribute, they need to have a Credit Card associated to them. All amounts and prices used in this API are always in cents.

When creating a payment, the amount field determines how much money is going to the campaign. The user_fee_amount accepts a value that will be charged to the paying user, on top of the amount, and the admin_fee_amount will be taken out of the money that goes to the campaign admin when the campaign ThePersuits.

For example, if a user wants to pay $20.00 to a campaign, and you want to add a 2% fee to the user, you would send amount as 2000 and user_fee_amount as 40 (2% of the $20.00). The users credit card would then be charged $20.40. In the same scenario, if you wanted to charge 2% from the admin when the campaign ThePersuits, you would set admin_fee_amount to 40 and on ThePersuit the admin will only receive $19.60 from the $20.00 payment.

    POST /campaigns/:id/payments

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPBDA/payments \
    -d'
    {
        "payment": {
            "amount" : 2000,
            "user_fee_amount" : 40,
            "admin_fee_amount" : 40,
            "user_id": "USR521",
            "card_id": "CCPC41"
        }
    }'

Response Body

    {
       "payment" : {
          "id" : "CON233",
          "status" : "charged",
          "amount" : 2000,
          "user_fee_amount" : 40,
          "admin_fee_amount" : 40,
          "uri" : "/v1/campaigns/CMP96B/payments/CON233",
          "campaign" : { "id": "CMP96B", "uri" : "/v1/campaigns/CMP96B", ... },
          "card" : { "id" : "CCPC41", "uri" : "/v1/users/USR521/cards/CCPC41", ... },
          "user": { "id" : "USR521", "uri" : "/v1/users/USR521", ... },
          "creation_date" : "2012-10-20T15:45:13Z",
          "modification_date" : "2012-10-20T15:45:47Z",
          "metadata" : {}
       }
    }

Response Codes

    201 => Created

### Get campaign payment

    GET /campaigns/:id/payments/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMP96B/payments/CON233
    Response Body
    
    {
       "payment" : {
          "id" : "CON233",
          "status" : "charged",
          "amount" : 2000,
          "user_fee_amount" : 40,
          "admin_fee_amount" : 40,
          "uri" : "/v1/campaigns/CMP96B/payments/CON233",
          "campaign" : { "id": "CMP96B", "uri" : "/v1/campaigns/CMP96B", ... },
          "card" : { "id" : "CCPC41", "uri" : "/v1/users/USR521/cards/CCPC41", ... },
          "user": { "id" : "USR521", "uri" : "/v1/users/USR521", ... },
          "creation_date" : "2012-10-20T15:45:13Z",
          "modification_date" : "2012-10-20T15:45:47Z",
          "metadata" : {}
       }
    }

Response Codes

    200 => OK

### Update campaign payment

You may update the credit card for payments. Note that you may only do this for payments with a status of "rejected".

    PUT /campaigns/:id/payments/:id

Example Request

    $ curl -X PUT -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMP96B/payments/CON233 \
    -d'
    {
        "payment": {
            "card_id": "CCPC42"
        }
    }'
    Response Body
    
    {
       "payment" : {
          "id" : "CON234",
          "status" : "charged",
          "amount" : 2000,
          "user_fee_amount" : 40,
          "admin_fee_amount" : 40,
          "uri" : "/v1/campaigns/CMP96B/payments/CON234",
          "campaign" : { "id": "CMP96B", "uri" : "/v1/campaigns/CMP96B", ... },
          "card" : { "id" : "CCPC41", "uri" : "/v1/users/USR521/cards/CCPC42", ... },
          "user": { "id" : "USR521", "uri" : "/v1/users/USR521", ... },
          "creation_date" : "2012-10-20T15:45:13Z",
          "modification_date" : "2012-10-20T15:45:47Z",
          "metadata" : {}
       }
    }

Response Codes

    200 => OK

### List campaign payments

    GET /campaigns/:id/payments

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMP96B/payments
    Response Body
    
    {
       "pagination" : {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
       },
       "payments" : [
          {
              "id" : "CON233",
              "status" : "charged",
              "amount" : 2000,
              "user_fee_amount" : 40,
              "admin_fee_amount" : 40,
              "uri" : "/v1/campaigns/CMP96B/payments/CON233",
              "campaign" : { "id": "CMP96B", "uri" : "/v1/campaigns/CMP96B", ... },
              "card" : { "id" : "CCPC41", "uri" : "/v1/users/USR521/cards/CCPC41", ... },
              "user": { "id" : "USR521", "uri" : "/v1/users/USR521", ... },
              "creation_date" : "2012-10-20T15:45:13Z",
              "modification_date" : "2012-10-20T15:45:47Z",
              "metadata" : {}
          },
          .
          .
          .
       ]
    }

Response Codes

    200 => OK

Get rejected payments

    GET /campaigns/:id/rejected_payments

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMP96B/rejected_payments
    Response Body
    
    {
        "pagination" : {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 1,
            "total_entries": 3,
            "per_page": 50
        }
        "payments" : [
          {
              "id" : "CON234",
              "status" : rejected",
              "amount" : 2000,
              "user_fee_amount" : 40,
              "admin_fee_amount" : 40,
              "uri" : "/v1/campaigns/CMP96B/payments/CON234",
              "campaign" : { "id": "CMP96B", "uri" : "/v1/campaigns/CMP96B", ... },
              "card" : { "id" : "CCPC41", "uri" : "/v1/users/USR521/cards/CCPC41", ... },
              "user": { "id" : "USR521", "uri" : "/v1/users/USR521", ... },
              "creation_date" : "2012-10-20T15:45:13Z",
              "modification_date" : "2012-10-20T15:45:47Z",
              "metadata" : {}
          },
          .
          .
          .
       ]
    }

Response Codes

    200 => OK

### Refunds

Refund a payment

In order to refund a payment, simply POST with an empty body to the payment's refund subresource.

    POST /campaigns/:id/payments/:id/refund

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMP96B/payments/CON233/refund

Response Codes

    200 => OK

### Campaign Settlements

Campaign Settlements represent a disbursement of funds for a ThePersuited campaign. A campaign settlement will show you the campaign, bank, and user that the settlement belongs to. It will also show you the admin_amount, which is the amount of money being sent to the admin's bank account. The escrow_amount is how much money from the campaign is going into your escrow account (it represents fees charged to the admin and payers). Possible statuses for a campaign settlement are:

pending - this means that the funds are being transfered to the bank account specified in the settlement.
rejected - this means that the settlement was rejected and could not be sent to the bank account in question. In these cases, the bank account can be updated, as specified in the update campaign settlement section.
re-sent pending - this means that the settlement previously failed, but the bank account has been updated, and the funds are being transfered to the new bank account.
cleared - this means that the funds have cleared the bank account and the settlement has completed successfully.

### Get Campaign Settlement

    GET /campaigns/:id/settlements/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/settlements/SMTD88
    Response Body
    
    {
        "settlement" : {
            "id" : "SMTD88",
            "status" : "pending",
            "admin_amount" : 1960,
            "escrow_amount" : 40,
            "bank" : { "id" : "BAPCA3", "uri" : "/v1/users/USRC77/banks/BAPCA3", ... },
            "campaign" : { "id" : "CMPCCC", "uri" : "/v1/campaigns/CMPCCC", ... },
            "user" : { "id" : "USRC7B", "uri" : "/v1/users/USRC7B", ... },
            "uri": "/v1/campaigns/CMPCCC/settlements/SMTD88",
            "creation_date" : "2012-10-29T15:34:48.177091000Z",
            "modification_date" : "2012-10-29T15:34:48.177091000Z"
        }
    }

### List Campaign Settlements

    GET /campaigns/:id/settlements

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/settlements

Response Body

    {
        "pagination" : {
                "page": 1,
                "entries_on_this_page": 3,
                "total_pages": 1,
                "total_entries": 3,
                "per_page": 50
            }
        "settlements" : [
            {
                "id" : "SMTD88",
                "status" : "pending",
                "admin_amount" : 1960,
                "escrow_amount" : 40,
                "bank" : { "id" : "BAPCA3", "uri" : "/v1/users/USRC77/banks/BAPCA3", ... },
                "campaign" : { "id" : "CMPCCC", "uri" : "/v1/campaigns/CMPCCC", ... },
                "user" : { "id" : "USRC7B", "uri" : "/v1/users/USRC7B", ... },
                "uri": "/v1/campaigns/CMPCCC/settlements/SMTD88",
                "creation_date" : "2012-10-29T15:34:48.177091000Z",
                "modification_date" : "2012-10-29T15:34:48.177091000Z"
            },
            .
            .
            .
        ]
    }

### Update Campaign Settlement Bank

A Campaign Settlement can only be updated if the status is rejected. In this instance, a bank object can be sent with the id of a new bank account to re-attempt the settlement with.

    POST /campaigns/:id/settlements/:id/bank

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/settlements/SMTD88/bank \
    -d'
    {
        "bank" : { "id" : "BAPCA4" }
    }'

### Campaign Comments

#### Create Campaign Comment

To create a comment, POST to /campaigns/:id/comments. The only required fields are the user_id of the comment author and the body of the comment. The title, parent_id, and score fields are optional. The parent_id is the id of the parent of this comment, i.e., the comment that this comment is a reply to. This only matters if you want to support nested comments. You may provide a parent_id of null for top-level comments. The purpose of the score field is to provide support for voting on comments.

    POST /campaigns/:id/comments

Example Request

    $ curl -X POST -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/comments
    -d'
    {
        "comment" : {
            "user_id' : "USR123",
            "title" : "Optional Title",
            "body" : "Comment Body",
            "score" : 1,
            "parent_id" : null
        }
    }'

Response

    {
        "comment" : {
            "id" : "CMT123",
            "user_id' : "USR123",
            "campaign_id" : "CMPCCC",
            "title" : "Optional Title",
            "body" : "Comment Body",
            "score" : 1,
            "parent_id" : null,
            "creation_date" : "2012-10-01T00:00:00Z",
            "modification_date" : "2012-10-01T00:00:00Z",
            "metadata" : { }
        }
    }

Response Codes

    200 => OK

List Campaign Comments

    GET /campaigns/:id/comments

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/comments

Response Body

    {
        "comments" : [
            {
                "id" : "CMT123",
                "user_id' : "USR123",
                "campaign_id" : "CMPCCC",
                "title" : "Optional Title",
                "body" : "Comment Body",
                "score" : 1,
                "parent_id" : null,
                "creation_date" : "2012-10-01T00:00:00Z",
                "modification_date" : "2012-10-01T00:00:00Z",
                "metadata" : { }
            },
            .
            .
            .
        ]
    }

Response Codes

    200 => OK

Get campaign comment

    GET /campaigns/:id/comments/:id

Example Request

    $ curl -X GET -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/comments/CMT123

Response Body

    {
        "comment" : {
            "id" : "CMT123",
            "user_id' : "USR123",
            "campaign_id" : "CMPCCC",
            "title" : "Optional Title",
            "body" : "Comment Body",
            "score" : 1,
            "parent_id" : null,
            "creation_date" : "2012-10-01T00:00:00Z",
            "modification_date" : "2012-10-01T00:00:00Z",
            "metadata" : { }
        }
    }

Response Codes

    200 => OK

Update campaign comment

Currently you can only alter the score and the metadata of a comment.

    PUT /campaigns/:id/comments/:id

Example Request

    $ curl -X PUT -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/comments/CMT123 \
    -d'
    {
        "comment" : {
            "score" : 2,
            "metadata" : {
                "key" : "value",
            }
        }
    }'

Response Body

    {
        "comment" : {
            "id" : "CMT123",
            "user_id' : "USR123",
            "campaign_id" : "CMPCCC",
            "title" : "Optional Title",
            "body" : "Comment Body",
            "score" : 1,
            "parent_id" : null,
            "creation_date" : "2012-10-01T00:00:00Z",
            "modification_date" : "2012-10-01T00:00:00Z",
            "metadata" : {
                "key" : "value",
            }
        }
    }

Response Codes

    200 => OK

Delete a comment

DELETE /campaigns/:id/comments/:id
Example Request

    $ curl -X DELETE -H Content-Type:application/json -u key:secret \
    https://api-sandbox.ThePersuit.com/v1/campaigns/CMPCCC/comments/CMT123

Response Codes

    200 => OK

# API Examples

## Create a User

Generally the first thing you want to do is create some users. Let's create two users, an admin, and a contributor.

## Create the admin user

    $ curl -X POST -u key:secret  -H Content-Type:application/json \
        https://api-sandbox.ThePersuit.com/v1/users \
        -d'{
            "user":{
                "email"    : "user@gmail.com"
            }
        }'

 Response

    {
        "user" : {
            "banks_uri": "/v1/users/USR38/banks",
            "campaigns_uri": "/v1/users/USR38/campaigns",
            "cards_uri": "/v1/users/USR38/cards",
            "creation_date": "2013-03-19T03:29:34.605286000Z",
            "email": "user@gmail.com",
            "firstname": null,
            "id": "USR38",
            "is_verified": 0,
            "lastname": null,
            "metadata": {},
            "modification_date": "2013-03-19T03:29:34.605286000Z",
            "payments_uri": "/v1/users/USR38/payments",
            "uri": "/v1/users/USR38"
        }
     }

### Create the paying user

     $ curl -X POST -u key:secret  -H Content-Type:application/json \
         https://api-sandbox.ThePersuit.com/v1/users \
         -d'{
             "user":{
                 "email" : "payer@gmail.com"
             }
         }'

 Response

     {
         "user" : {
            "banks_uri": "/v1/users/USR55/banks",
            "campaigns_uri": "/v1/users/USR55/campaigns",
            "cards_uri": "/v1/users/USR55/cards",
            "creation_date": "2013-03-19T03:29:34.605286000Z",
            "email": "payer@gmail.com",
            "firstname": null,
            "id": "USR55",
            "is_verified": 0,
            "lastname": null,
            "metadata": {},
            "modification_date": "2013-03-19T03:29:34.605286000Z",
            "payments_uri": "/v1/users/USR55/payments",
            "uri": "/v1/users/USR55"
         }
      }

### Create a campaign

Once you've got some users, you can create a campaign for any of them. For this example, we'll make the first user created above the admin user, and use the second user to make a payment on the campaign.

    $ curl -X POST -u key:secret  -H Content-Type:application/json \
        https://api-sandbox.ThePersuit.com/v1/campaigns \
        -d'{
            "campaign" : {
                "user_id" : "USR38",
                "expiration_date" : "2012-10-31T12:00:00Z",
                "title" : "Halloween Awesome Fest",
                "ThePersuit_amount" : 300000
            }
        }'

Response

    {
        "campaign" : {
            "admin": {
                "banks_uri": "/v1/users/USR38/banks",
                "campaigns_uri": "/v1/users/USR38/campaigns",
                "cards_uri": "/v1/users/USR38/cards",
                "creation_date": "2013-03-19T03:29:34.605286000Z",
                "email": "user@gmail.com",
                "firstname": null,
                "id": "USR38",
                "is_verified": 0,
                "lastname": null,
                "metadata": {},
                "modification_date": "2013-03-19T03:29:34.605286000Z",
                "payments_uri": "/v1/users/USR38/payments",
                "uri": "/v1/users/USR38"
            },
            "creation_date": "2013-03-19T03:34:13.518139000Z",
            "expiration_date": "2012-10-31T12:00:00Z",
            "first_contributor": null,
            "fixed_payment_amount": 0,
            "id": "CMPDE8",
            "is_expired": 0,
            "needs_bank": 0,
            "is_paid": 0,
            "is_ThePersuited": 0,
            "metadata": {},
            "min_payment_amount": 0,
            "modification_date": "2013-03-19T03:34:13.518139000Z",
            "payments_uri": "/v1/campaigns/CMPDE8/payments",
            "settlements_uri": "/v1/campaigns/CMPDE8/settlements",
            "stats": {
                "number_of_contributions": 0,
                "raised_amount": 0,
                "ThePersuit_percent": 0,
                "unique_contributors": 0
            },
            "ThePersuit_amount": 300000,
            "ThePersuiter": null,
            "title": "Halloween Awesome Fest",
            "uri": "/v1/campaigns/CMPDE8"
        }
    }

### Create a card

Now, we'll create a credit card for the paying user.

    $ curl -X POST -u key:secret  -H Content-Type:application/json \
        https://api-sandbox.ThePersuit.com/v1/users/USR55/cards \
        -d'{
            "card" : {
                "number" : "4111111111111111",
                "expiration_month" : "07",
                "expiration_year" : "2023",
                "security_code" : "123"
            }
        }'

 Response

    {
        "card" : {
            "card_type": "VISA card",
            "creation_date": "2013-03-19T03:36:21.682748000Z",
            "expiration_month": "07",
            "expiration_year": 2023,
            "id": "CCP2A",
            "last_four": "1111",
            "metadata": {},
            "modification_date": "2013-03-19T03:36:21.682748000Z",
            "uri": "/v1/users/USR55/cards/CCP2AE",
            "user": {
                "banks_uri": "/v1/users/USR55/banks",
                "campaigns_uri": "/v1/users/USR55/campaigns",
                "cards_uri": "/v1/users/USR55/cards",
                "creation_date": "2013-03-19T03:29:34.605286000Z",
                "email": "payer@gmail.com",
                "firstname": null,
                "id": "USR55",
                "is_verified": 0,
                "lastname": null,
                "metadata": {},
                "modification_date": "2013-03-19T03:29:34.605286000Z",
                "payments_uri": "/v1/users/USR55/payments",
                "uri": "/v1/users/USR55"
            }
        }
     }

### Create a payment

Now we'll create a payment by the paying user to the campaign we created.

    $ curl -X POST -u key:secret  -H Content-Type:application/json \
        https://api-sandbox.ThePersuit.com/v1/campaigns/CMP542/payments \
        -d'{
            "payment" : {
                "user_id" : "USR55",
                "amount" : "3000",
                "user_fee_amount" : "100",
                "admin_fee_amount" : "40",
                "card_id" : "CCP2AE"
            }
        }'

 Response
{
    "payment" : {
        "admin_fee_amount": 40,
        "amount": 3000,
        "campaign" : {
            "admin": {
                "banks_uri": "/v1/users/USR38/banks",
                "campaigns_uri": "/v1/users/USR38/campaigns",
                "cards_uri": "/v1/users/USR38/cards",
                "creation_date": "2013-03-19T03:29:34.605286000Z",
                "email": "user@gmail.com",
                "firstname": null,
                "id": "USR38",
                "is_verified": 0,
                "lastname": null,
                "metadata": {},
                "modification_date": "2013-03-19T03:29:34.605286000Z",
                "payments_uri": "/v1/users/USR38/payments",
                "uri": "/v1/users/USR38"
            },
            "creation_date": "2013-03-19T03:34:13.518139000Z",
            "expiration_date": "2012-10-31T12:00:00Z",
            "first_contributor": null,
            "fixed_payment_amount": 0,
            "id": "CMPDE8",
            "is_expired": 0,
            "needs_bank": 0,
            "is_paid": 0,
            "is_ThePersuited": 0,
            "metadata": {},
            "min_payment_amount": 0,
            "modification_date": "2013-03-19T03:34:13.518139000Z",
            "payments_uri": "/v1/campaigns/CMPDE8/payments",
            "settlements_uri": "/v1/campaigns/CMPDE8/settlements",
            "stats": {
                "number_of_contributions": 1,
                "raised_amount": 3000,
                "ThePersuit_percent": 1,
                "unique_contributors": 1
            },
            "ThePersuit_amount": 300000,
            "ThePersuiter": null,
            "title": "Halloween Awesome Fest",
            "uri": "/v1/campaigns/CMPDE8"
        },
        "card": {
            "card_type": "VISA card",
            "creation_date": "2013-03-19T03:36:21.682748000Z",
            "expiration_month": "07",
            "expiration_year": 2023,
            "id": "CCP2A",
            "last_four": "1111",
            "metadata": {},
            "modification_date": "2013-03-19T03:36:21.682748000Z",
            "uri": "/v1/users/USR55/cards/CCP2AE",
            "user": {
                "banks_uri": "/v1/users/USR55/banks",
                "campaigns_uri": "/v1/users/USR55/campaigns",
                "cards_uri": "/v1/users/USR55/cards",
                "creation_date": "2013-03-19T03:29:34.605286000Z",
                "email": "payer@gmail.com",
                "firstname": null,
                "id": "USR55",
                "is_verified": 0,
                "lastname": null,
                "metadata": {},
                "modification_date": "2013-03-19T03:29:34.605286000Z",
                "payments_uri": "/v1/users/USR55/payments",
                "uri": "/v1/users/USR55"
            }
        },
        "creation_date": "2013-03-19T03:38:38.736326000Z",
        "id": "CON7C9",
        "metadata": {},
        "modification_date": "2013-03-19T03:38:41Z",
        "status": "authorized",
        "uri": "/v1/campaigns/CMPDE8/payments/CON7C9",
        "user": {
            "banks_uri": "/v1/users/USR55/banks",
            "campaigns_uri": "/v1/users/USR55/campaigns",
            "cards_uri": "/v1/users/USR55/cards",
            "creation_date": "2013-03-19T03:29:34.605286000Z",
            "email": "payer@gmail.com",
            "firstname": null,
            "id": "USR55",
            "is_verified": 0,
            "lastname": null,
            "metadata": {},
            "modification_date": "2013-03-19T03:29:34.605286000Z",
            "payments_uri": "/v1/users/USR55/payments",
            "uri": "/v1/users/USR55"
        },
        "user_fee_amount": 100
 }
This payment will charge $21.00 to the user's credit card ($20.00 from the amount field, plus $1.00 from the user_fee_amount field), and the admin of the campaign will receive $18.00 from the payment ($20.00 from amount minus the $2.00 set in the admin_fee_amount field).

Now you have created 2 users, a campaign under one user, and a payment to the campaign by another user!

Tokenizing Sensitive User Information

If you want to store sensitive financial information from your users (like credit card and bank account numbers), you are required to be compliant with the Payment Card Industry Data Security Standard (PCI DSS). ThePersuit handles this for you by providing a PCI-compliant javascript library, crowdThePersuit.js, which is easy to implement on your website. Sensitive payment information can then be securely collected without ever touching your servers, keeping you completely outside of PCI and regulatory scope.

To implement crowdThePersuit.js, follow the steps below. You can also check out this example implementation.

Include the Library

To use crowdThePersuit.js, simply include the following script tag on any page where you will be collecting credit card or bank account information:

    <script type="text/javascript" src="https://api.ThePersuit.com/v1/js/crowdThePersuit.js"></script>

Initialize the crowdThePersuit object

In a separate script tag, initialize the crowdThePersuit object. NOTE: this defaults to using the sandbox api when no parameter is passed to the init method:

    <script type="text/javascript">
        crowdThePersuit.init();
    </script>

Once you are ready to go to production, pass 'production' as a parameter:

    <script type="text/javascript">
        crowdThePersuit.init('production');
    </script>

Make sure to create users first

Credit cards and bank accounts must be associated with existing user objects created through our API. This means that you need to collect user account information in a step before collecting credit card or bank account information. See creating users for more information. You'll need the resulting user id to create credit cards and bank accounts for that user.

### Create a Credit Card

Once you have a user, the next step is to collect her credit card information and pass it to the crowdThePersuit.card.create function along with her user id, as well as a function to handle the response (more on that in a second).

Example:

    "cardData": {
        "number":"4111111111111111",
        "expiration_month":"03",
        "expiration_year":2023,
        "security_code":123
    }

crowdThePersuit.card.create(user_id, cardData, responseHandler);
The user id should be the hash returned by the user creation API call, e.g. "USR1E0A9BCE5F6111E28F485D097AC0CAB6"

The required credit card data fields are:

number: card number, as a string, without any separators, should be 16 digits in length, e.g. "4111111111111111"
expiration_month: two digit number, as a string, representing the card's expiration month, e.g. "03"
expiration_year: four digit number, as a string, representing the card's expiration year, e.g. "2013"
security_code: three or four digit number, as a string, e.g. "123"
If successful, the response object passed to the responseHandler function takes the form:

    {
        "card": {
            "card_type": "VISA card",
            "creation_date": "2013-03-19T03:48:59.685761000Z",
            "expiration_month": "03",
            "expiration_year": 2023,
            "id": "CCPEEBABE72904711E2AB7EDDC43A854B4C",
            "last_four": "1111",
            "metadata": {},
            "modification_date": "2013-03-19T03:48:59.685761000Z",
            "uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A/cards/CCPEEBABE72904711E2AB7EDDC43A854B4C",
            "user": {
                "banks_uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A/banks",
                "campaigns_uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A/campaigns",
                "cards_uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A/cards",
                "creation_date": "2013-02-18T00:09:39Z",
                "email": "mark@ting.com",
                "firstname": "John",
                "id": "USR7C7CE3AC795F11E2901DABD956AC4F1A",
                "is_verified": 0,
                "lastname": "Smith",
                "metadata": {},
                "modification_date": "2013-03-19T00:03:56Z",
                "paid_campaigns_uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A/paid_campaigns",
                "payments_uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A/payments",
                "uri": "/v1/users/USR7C7CE3AC795F11E2901DABD956AC4F1A"
            }
        },
        "request_id":"REQ5735A0D8601F11E2998D00347AC0CAB6",
        "status":201
    }

If not successful, the response object passed to the responseHandler function takes the form:

    {
        "error": ... // A string or an object describing the error(s)
        "request_id": ... //May or may not be present depending on whether the error was found before or after submitting to the API
        "status": ... // HTTP status code
    }

Handling the Response

When creating a card or a bank account, you must pass in your own callback function to handle the response from our API. The status property included with the response provides a handy hook to help you decide what to do.

Here is a basic outline of what your response handler could look like:

    function responseHandler(response) {
        switch (response.status) {
           case 201:
               // The card or bank was created successfully!
               // Submit the data contained in response.card or response.bank to your server for saving
               // For bank accounts, remember to set the new account as the default in addition to saving it to your database
               break;
           case 400:
               // missing field - check response.error for details
               break;
           case 404:
               // your user_id is incorrect (no user was found)
               break;
           default:
               // Some other error ocurred, check response.error for details
       }
    }

### Create a Bank Account

Creating bank accounts is just like creating credit cards. First gather up the bank account information, then pass it to crowdThePersuit.bank.create along with a user id and a response handler...but keep in mind one important extra step: When you go to save the tokenized bank account to your server (most likely through a request made in your callback function), you need to make an additional API call to set this new bank account as the 'default' bank account. See setting the default bank account for more information.

Example:

    "bankData": {
        "name" : "John Smith",
        "account_number" : "1234567890",
        "bank_code" : "321174851"
    }

crowdThePersuit.bank.create(user_id, bankData, responseHandler);
The user id should be the hash returned by the user creation API call, e.g. "USR1E0A9BCE5F6111E28F485D097AC0CAB6"

The required credit card data fields are:

name: card number, as a string, without any separators, should be 16 digits in length, e.g. "4111111111111111"
account_number: arbitrary length number
bank_code: nine digit number, as a string, representing the bank code (also known as routing number), e.g. "321174851"
If successful, the response object passed to the responseHandler function takes the form:

    {
        "bank": {
            "account_number_last_four": "7890",
            "bank_code_last_four": "1234",
            "creation_date": "2013-03-19T03:46:45.557777000Z",
            "id": "BAP9EC85D70904711E2AB7EDDC43A854B4C",
            "is_default": 0,
            "metadata": {},
            "modification_date": "2013-03-19T03:46:45.557777000Z",
            "name": "John Smith",
            "uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083/banks/BAP9EC85D70904711E2AB7EDDC43A854B4C",
            "user": {
                "banks_uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083/banks",
                "campaigns_uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083/campaigns",
                "cards_uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083/cards",
                "creation_date": "2013-02-19T18:11:37Z",
                "email": "happyness@sauce.com",
                "firstname": "marc",
                "id": "USRCD2689FA7ABF11E2863ADF5C03071083",
                "is_verified": 1,
                "lastname": null,
                "metadata": {},
                "modification_date": "2013-03-04T06:14:25.991725000Z",
                "paid_campaigns_uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083/paid_campaigns",
                "payments_uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083/payments",
                "uri": "/v1/users/USRCD2689FA7ABF11E2863ADF5C03071083"
            }
        },
        "request_id":"REQ5743854A601F11E2B79201347AC0CAB6",
        "status":201
    }

If not successful, the response object passed to the responseHandler function takes the form:

    {
        "error": ... // A string or an object describing the error(s)
        "request_id": ... //May or may not be present depending on whether the error was found before or after submitting to the API
        "status": ... // HTTP status code
    }

Client-side Validation Helpers

These handy functions let you validate user input on the client side, leading to a better overall experience.

Validating Card Number

Checks that the number is formatted correctly and passes the Luhn check. Note that spaces and other punctuation are ignored.

crowdThePersuit.card.isCardNumberValid('4111111111111111');      // true
crowdThePersuit.card.isCardNumberValid('4111-1111-1111-1111');   // true
crowdThePersuit.card.isCardNumberValid('4111 1111 1111 1111');   // true
crowdThePersuit.card.isCardNumberValid('123456');                // false, too short
crowdThePersuit.card.isCardNumberValid('4242-1111-1111-1111');   // false, doesn't pass Luhn check
Determining Card Brand

Returns the card brand based on the card number

crowdThePersuit.card.cardType('5105105105105100');   // Mastercard
crowdThePersuit.card.cardType('4111111111111111');   // VISA
crowdThePersuit.card.cardType('341111111111111');    // American Express
crowdThePersuit.card.cardType(0)                     // null
Validating Security Code (CSC)

Checks that the security code (also known as CSC or CVC)is properly formatted based on card brand

crowdThePersuit.card.isSecurityCodeValid('4111111111111111', 999)   // true, VISA has a 3 digit security code
crowdThePersuit.card.isSecurityCodeValid('4111111111111111', 9999)  // false
crowdThePersuit.card.isSecurityCodeValid('341111111111111', 999)    // false, American Express has a 4 digist security code
Validating Card Expiration

Checks if the expiration date is properly formatted and in the future

    crowdThePersuit.card.isExpirationValid('01', '2020');    // true
    crowdThePersuit.card.isExpirationValid(1, 2010);         // false
    General Card Validation

Runs the full set of validation functions, returns an error object that can contain multiple errors

    crowdThePersuit.card.validate({
        card_number:'4111111111111112',
        expiration_month: '01',
        expiration_year: '2010',
        security_code: '123'
    });

Returns:

    {
        card_number:'"4111111111111112" is not a valid credit card number',
        expiration:'"01-2010" is not a valid credit card expiration date'
    }

Validating a USA Bank Code (Routing Number)

Use ONLY for USA-based bank accounts. Checks against the MICR Routing Number Format

crowdThePersuit.bank.validateUSARoutingNumber('321174851') // passes
crowdThePersuit.bank.validateUSARoutingNumber('021000021') // passes
crowdThePersuit.bank.validateUSARoutingNumber('123457890') // fails
Tokenization Examples

Here we include two sample forms, one to collect credit card information and one to collect bank account information.

Included also is sample javascript to handle the data captured in the forms using crowdThePersuit.js. We use jQuery for convenience, but keep in mind that jQuery is not required when using crowdThePersuit.js.

Sample Credit Card Form

    <form action="#" method="POST" "id="cardForm">
        <input type="hidden" name="user_id">
        <fieldset>
            <label>Card Number</label>
            <input type="text" name="card_number" autocomplete="off">
        </fieldset>
        <fieldset>
            <label>Expiration</label>
            <select name="expiration_month" style="width:50px">
                <option value="01" selected>01</option>
                <option value="02">02</option>
                <option value="03">03</option>
                <option value="04">04</option>
                <option value="05">05</option>
                <option value="06">06</option>
                <option value="07">07</option>
                <option value="08">08</option>
                <option value="09">09</option>
                <option value="10">10</option>
                <option value="11">11</option>
                <option value="12">12</option>
                </select>
                /
            <select name="expiration_year" style="width:75px">
                <option value="2013" selected>2013</option>
                <option value="2014">2014</option>
                <option value="2015">2015</option>
                <option value="2016">2016</option>
                <option value="2017">2017</option>
                <option value="2018">2018</option>
                <option value="2019">2019</option>
                <option value="2020">2020</option>
            </select>
        </fieldset>
        <fieldset>
            <label>Security Code</label>
            <input type="text" name="security_code" autocomplete="off">
        </fieldset>
        <button type="submit">submit</button>
    </form>

Sample Bank Account Form

    <form action="#" method="POST" id="bankForm">
        <input type="hidden" name="user_id">
        <fieldset>
            <label>Bank Name</label>
            <input type="text" name="name">
        </fieldset>
        <fieldset>
            <label>Account Number</label>
            <input type="text" name="account_number" autocomplete="off">
        </fieldset>
        <fieldset>
            <label>Bank Code (Routing Number in USA)</label>
            <input type="text" name="bank_code">
        </fieldset>
    
        <button type="submit">submit</button>
    </form>

Sample Javascript

    <script type="text/javascript">
        (function() {
    
            //Initialize the crowdThePersuit object
            crowdThePersuit.init();
    
            var responseHandler = function(response) {
                switch (response.status) {
                   case 201:
                       // The card or bank was created successfully!
                       // Submit the data contained in response.card or repsonse.bank to your server for saving
                       // For bank accounts, remember to set the new account as the default in addition to saving it to your database
                       break;
                   case 400:
                       // missing field - check response.error for details
                       break;
                   case 404:
                       // your user_id is incorrect (no user was found)
                       break;
                   default:
                       // Some other error ocurred, check response.error for details
                }
            }
    
            var tokenizeCard = function(e) {
                e.preventDefault();
    
                var $form = $('form#cardForm');
                var cardData = {
                    number: $form.find('[name="card_number"]').val(),
                    expiration_month: $form.find('[name="expiration_month"]').val(),
                    expiration_year: $form.find('[name="expiration_year"]').val(),
                    security_code: $form.find('[name="security_code"]').val()
                };
                var user_id = $form.find('[name="user_id"]').val();
    
                crowdThePersuit.card.create(user_id, cardData, callback);
            };
            $('#cardForm').submit(tokenizeCard);
    
            var tokenizeBankAccount = function(e) {
                e.preventDefault();
    
                var $form = $('form#bankForm');
                var bankAccountData = {
                    name: $form.find('[name="name"]').val(),
                    account_number: $form.find('[name="account_number"]').val(),
                    bank_code: $form.find('[name="bank_code"]').val()
                };
                var user_id = $form.find('[name="user_id"]').val();
    
                crowdThePersuit.bank.create(user_id, bankAccountData, callback);
            };
            $('#bankForm').submit(tokenizeBankAccount);
        })();
    </script>

## Resource Definitions

This section outlines the full definition of our resources.

### User Definition

| 	Attribute	| 	Type	| 	Required	| 	Description	| 
| 	:------------	| 	:------------	| 	:------------	| 	:------------	| 
| 	id	| 	string	| 	Auto generated and read-only	| 	A unique identifier for the user	| 
| 	email	| 	string	| 	Yes	| 	Email address	| 
| 	firstname	| 	string	| 	Yes	| 	The first name of the user	| 
| 	lastname	| 	string	| 	Yes	| 	The last name of the user	| 
| 	is_verified	| 	integer	| 	Auto generated and read-only	| 	Whether this user is verified to receive money	| 
| 	creation_date	| 	string	| 	Auto generated and read-only	| 	It is ISO8601 DateTime format	| 
| 	modification_date	| 	string	| 	Auto generated and read-only	| 	It is ISO8601 DateTime format	| 
| 	uri	| 	string	| 	Auto generated and read-only	| 	The uri for this user resource	| 
| 	cards_uri	| 	string	| 	Auto generated and read-only	| 	The uri for listing user's cards	| 
| 	banks_uri	| 	string	| 	Auto generated and read-only	| 	The uri for listing user's banks	| 
| 	campaigns_uri	| 	string	| 	Auto generated and read-only	| 	The uri for listing user's campaigns	| 
| 	payments_uri	| 	string	| 	Auto generated and read-only	| 	The uri for the user's payments	| 
| 	metadata	| 	JSON object	| 	No	| 	Key-Value pair for any extra data the API consumer wants to store. For example, a reference to the URL of a user's profile image.	| 

## Card Definition							
| 	Attribute	| 	Type	| 	Required	| 	Description	| 	
| 	:------------	| 	:------------	| 	:------------	| 	:------------	| 
| 	id	| 	string	| 	Auto generated and read-only	| 	A unique identifier for the card	| 
| 	last_four	| 	string	| 	Read-Only	| 	Last four digits of credit card number	| 
| 	expiration_month	| 	string	| 	Yes	| 	Expiration month (string representing 2 digits)	| 
| 	expiration_year	| 	integer	| 	Yes	| 	Expiration year (4 digit number)	| 
| 	card_type	| 	string	| 	Read-Only	| 	Identifies the card's brand (VISA, Mastercard, etc)	| 
| 	user	| 	JSON object	| 	Read-Only	| 	The user object that owns this card	| 
| 	uri	| 	string	| 	Auto generated and read-only	| 	The uri for this card resource	| 
| 	creation_date	| 	string	| 	Auto generated and read-only	| 	It is ISO8601 DateTime format	| 
| 	modification_date	| 	string	| 	Auto generated and read-only	| 	It is ISO8601 DateTime format	| 
| 	metadata	| 	JSON object	| 	No	| 	Key-Value pair for any extra data the API consumer wants to store.	| 


### Bank Definition								
| 	Attribute	| 	Type	| 	Required	| 	Description	| 
| 	:------------	| 	:------------	| 	:------------	| 	:------------	| 
| 	id	| 	string	| 	Auto generated and read-only	| 	A unique identifier for the bank	| 
| 	account_number_last_four	| 	string	| 	Read-Only	| 	Last four digits of account nummber	| 
| 	bank_code_last_four	| 	string	| 	Read-Only	| 	Last four digits of bank routing nummber	| 
| 	name	| 	String	| 	No	| 	The name of this bank account	| 
| 	is_default	| 	Integer (1 or 0)	| 	Read-only	| 	Tells you if this bank is currently the default for the user	| 
| 	user	| 	JSON object	| 	Read-Only	| 	The user object that owns this bank	| 
| 	uri	| 	string	| 	Auto generated and read-only	| 	The uri for this bank resource	| 
| 	creation_date	| 	string	| 	Auto generated and read-only	| 	It is ISO8601 DateTime format	| 
| 	modification_date	| 	string	| 	Auto generated and read-only	| 	It is ISO8601 DateTime format	| 
| 	metadata	| 	JSON object	| 	No	| 	Key-Value pair for any extra data the API consumer wants to store.	| 



## Pagination 

GET /users

    {
        "pagination": {
            "page": 1,
            "entries_on_this_page": 3,
            "total_pages": 2,
            "total_entries": 100,
            "per_page": 50
        },
        "users": [
            {
                "id": "USREC5",
                "email": "foo.bar@gmail.com",
                "firstname": "Foo",
                "lastname": "Bar",
                "is_verified": 0,
                "creation_date": "2011-07-02T14:20:48Z",
                "modification_date": "2011-09-02T14:20:48Z",
                "uri": "/v1/users/USREC5",
                "cards_uri": "/v1/users/USREC5/cards",
                "banks_uri": "/v1/users/USREC5/banks",
                "campaigns_uri": "/v1/users/USREC5/campaigns",
                "payments_uri": "/v1/users/USREC5/payments",
                "metadata" : { "img" : "http://www.example.com/path-to-profile-image" }
            },
            .
            .
            .
        ]
    }

The default limits can be changed with per_page and page request parameters. For example:

GET /users?page=2&per_page=10
Frequently Asked Questions
Q: Can I create ThePersuit.com or ThePersuit/Open camapigns through the ThePersuit API?

A: Campaigns created through the ThePersuit API exist independently of ThePersuit.com and ThePersuit/Open campaigns.

If you're interested in allowing users to create ThePersuit.com campaigns from your website or application, you may find that the beta version of our ThePersuit Button fits your needs.

Q: Can I use the ThePersuit API outside of the United States?

A: While contributions to campaigns created through the API can be made with any major credit card (VISA, MasterCard, American Express, and Discover), all payouts of funds collected occur to US-based bank accounts.

Q: What are the costs associated with using the ThePersuit API?

A: The fee to use the ThePersuit API is 1% of funds collected. You'll only be charged for campaigns that hit their ThePersuit amount. In addition to the ThePersuit API fee, Stripe fees also apply.

Q: Does the ThePersuit API allow me to do white label crowdfunding?

A: The ThePersuit API provides crowdfunding logic on top of payments processing handled by Stripe. The ThePersuit API does not provide auth, login, UI/UX, etc. - you're in full control of the look, feel, and interactions within the application.

Q: Can I interact with the ThePersuit/Open campaigns via the ThePersuit API?

A: The ThePersuit API and ThePersuit/Open exist independently of each other. Accordingly, campaigns created via ThePersuit/Open cannot be modified or accessed via the ThePersuit API.

What does exist for ThePersuit/Open are campaign and payment endpoints that return a JSON array of their respective data. These endpoints can be found on the campaign edit page in your ThePersuit/Open admin panel and are often used for exporting campaign / payment data to fulfillment tools like BackerKit and CMS platforms like Salesforce.


































