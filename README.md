# nl-covid19-coronacheck-app-coronatestprovider-portal

Version 0.1 - 7th May 2021


In the CoronaCheck project we are providing sample test data to Test Providers that we advice the Test Providers contain within their system. Combined with the Test Provider Test Portal (at https://aaaa), this enables Test Providers to conduct an end-to-end test of their endpoints.

## Test Portal
VWS provides a web front-end through which Test Providers can test their endpoints. Data entered through this web front-end will be sent to a given endpoint and show the response it receives from it.

On the web front-end the user will need to enter:
The TOKEN element of the retrieval code (the Y-part from XXX-YYYYYYYYYYYYY-ZV).
The end-point that the test service needs to address

After the test service has been called, the exact return values will be shown to the user.

A standardized test set has been provided. It is up to the Test Provider to ensure that this data is recorded in their database, such that this VWS web front-end can be used against an existing and known test set.

This Test Portal can be found here: 

### User Interface
The Test Portal contains the following fields:

```
[ URL                                              ]
[ Token                                          ^ ]
[ Verification code                                ] (optional)
```

After providing this information and hitting `Submit` the portal executes the following:

 * It sends a `POST` request to the provided URL
 * The Tokens that a user can select are presented as a drop-down. These are read from the accompanying test data set, that should be included in the Test Providers database.
 * This post request contains the following headers:
   - `Authorization: Bearer <Token>`
   - `CoronaCheck-Protocol-Version: 2.0`
 * And, if a Verification code was provided, the following JSON object is sent as data
   - `{ "verificationCode": "<Verification code>"}`

The values returned from this call are then presented to the user. Next to the returned values, the expected return values are also shown and it is clearly indicated which do not match the expected outcomes.

## Test Data
The data that the Test Provider should maintain in their system can be found [here](default-test-cases.csv) in this repository. This data has been provided as a means to conduct end-to-end testing via the above mentioned portal. The data is defined as follows:

| field name               | type               | description                                                      |
|--------------------------|--------------------|------------------------------------------------------------------|
| token                    | string             | the token that is going to be used to retrieve the test data     |
| protocolVersion          | string             | the version number of the interface                              |
| providerIdentifier       | string             | the three-letter code for the test provider                      |
| unique                   | string             | the unique number of the test                                    |
| sampleDate               | string (date/time) | the date and time of when the sample was taken                   |
| testType                 | string             | the type of the test that was taken                              |
| isSpecimen               | boolean            | if this data refers to a specimen (always TRUE)                  |
| negativeResult           | boolean            | if the result of the test was negative (always TRUE)             |
| namePrefix               | string             | any letters, or titles that go in front of the persons' name     |
| firstName                | string             | the full first name of the person                                |
| nameInfix                | string             | any letters or words that go in between first and last name      |
| lastName                 | string             | the full last name of the person                                 |
| namePostfix              | string             | any letters or titles that go after the persons' last name       |
| dateOfBirth              | string (date)      | the date of birth of the person                                  |
| expectedReturnCode       | int                | the HTTP return code that is expected                            |
| expectedStatus           | string             | the status code that is expected                                 |
| expectedSampleDate       | string (date/time) | the expected date and time when the test sample was taken        |
| expectedNegativeResult   | boolean            | the expected value if the test result was negative (always TRUE) |
| expectedTestType         | string             | the expected test type                                           |
| expectedIsSpecimen       | boolean            | the expected specimen indicator (always TRUE)                    |
| expectedFirstNameInitial | character          | the expected first name initial                                  |
| expectedLastNameInitial  | character          | the expected last name initial                                   |
| expectedBirthDay         | int                | the expected day-of-month of the persons' birth date             |
| expectedBirthMonth       | int                | the expected month of the persons' birth date                    |
| testTitle                | string             | the name of the specific test record                             |

How this data is represented in the Test Provider database is up to the implementor of that database.