
# [Delivery Hero - PANDORA] Test for Senior Software Engineer

With this test, we want to create an environment where you can demonstrate your engineering skills by creating a production-ready solution for a given challenge.  We recommend you to do not disclose the implementation fo this challenge.

## Challenge
Given a context of a microservice architecture, you have to implement a payment processor service that has to accept  5.000 order requests per minute, however, in order to process the payment you have to call a third-party to complete the financial transaction; This third-party responds each request in an average of 896ms and can only accept 3 simultaneous request. We encourage you to over-engineer your solution to show your potential, however, you have to attend the following criteria:

* The data **must** be stored
* The service **must** be fault tolerant
* All orders **must** be processed
* The code **must** be tested, and we should be able to run those tests
* The project **must** have a readme file containing at least:
	- An explanation of the architecture decisions
	- If using libraries/frameworks/microframeworks, an explanation why you did choose each of them and the possible alternatives.
	- How to run the tests and the projects
* The project **must** run using docker-compose to provision the infrastructure
* You are free to choose your infrastructure/services, however, they **must** be open source and run over docker.
* To delivery the test we require you to create a private repository on Bitbucket and give us reading access to the email danilo.freitas@foodora.com . The name of the repository has to be like the following: firstname-lastname-pandora-snr-sfw-eng-MM-YYYY.


##### You have to implement the following endpoint to accept orders:

```
HOST: http://localhost/
# Software Engineer Test

## Order [/order]

### Place Order [POST]

+ Request (application/json)

        {
            "country": "MY",
            "curstomer": "972a18f7-aa4d-42a7-9224-d4c6dfd1657d",
            "delivery_address": "938027b4-65c3-4d9b-a621-fb222d646749"
            "payment_method": "429cb853-d6cf-4020-8ee6-29369dcac4a1",
            "vendor": "6d8bb387-feef-4265-8b5d-9ae9f307fed4",
            "dishes": [
                {
                    
                    "dish": "c51a023e-d00f-4699-ab84-0759c8f953c9",
                    "quantity": 2,
                    "price": 3276
                    "discount": 328
                },{
                    "dish": "22173f3f-c4ee-44b7-935d-b580de313e69",
                    "quantity": 1,
                    "price": 1021
                    "discount": 103
                }
            ],
            "voucher": "ENG1000GRAU"
        }

+ Response 201 (application/json)

    + Body

        {
            "order": "d9085704-6d89-4bc0-990c-a1359d652123"
            "country": "MY",
            "curstomer": "972a18f7-aa4d-42a7-9224-d4c6dfd1657d",
            "delivery_address": "938027b4-65c3-4d9b-a621-fb222d646749"
            "payment_method": "429cb853-d6cf-4020-8ee6-29369dcac4a1",
            "vendor": "6d8bb387-feef-4265-8b5d-9ae9f307fed4",
            "dishes": [
                {
                    "dish": "c51a023e-d00f-4699-ab84-0759c8f953c9",
                    "quantity": 2,
                    "price": 3276 //in cents
                    "discount": 328,
                },{
                    "dish": "22173f3f-c4ee-44b7-935d-b580de313e69",
                    "quantity": 1,
                    "price": 1021
                    "discount": 103
                }
            ],
            "total": 4287,
            "shipping": 300,
            "discount": 431,
            "grand_total": 4266,
            "voucher": "ENG1000GRAU",
            "status": "processed"
        }
        
+ Response 400 (application/json)

    + Body

        {
            "error" {
                "code": 1001,
                "message": "grand total mismatch"
            },
            "order": "d9085704-6d89-4bc0-990c-a1359d652123"
            "status": "failed"
        }
```

##### You have to call the following endpoint in order to process the payment:

```
HOST: http://payment.thirdparty/

# Third party payment service
You have to assume that you would not have it implemented, however this service should be either mocked or covered by tests

## Payment [/pay]

### Process Payment [POST]

+ Request (application/json)

        {
            "country": "MY",
            "curstomer": "972a18f7-aa4d-42a7-9224-d4c6dfd1657d",
            "payment_method": "429cb853-d6cf-4020-8ee6-29369dcac4a1",
            "order": "d9085704-6d89-4bc0-990c-a1359d652123",
            "vendor": "6d8bb387-feef-4265-8b5d-9ae9f307fed4",
            "grand_total": 4266
        }

+ Response 201 (application/json)

    + Body

        {
            "transaction": "71d13d76-be40-45ca-b393-905f4f776bbe"
            "status": "payd"
        }
        
+ Response 400 (application/json)

    + Body

        {
            "error" {
                "code": 1551,
                "message": "no enough funds"
            },
            "order": "d9085704-6d89-4bc0-990c-a1359d652123"
            "status": "failed"
        }

+ Response 500 (application/json)
```
