## Overview Section

The Account Information API is used to inform the account owner of the Balance and Transaction entries booked to the account for a given date or time-range on a specific day.

The information returned by these APIs can be requested as either a standardised response as per ISO 20022 Cash Management (camt) messaging standards or an HSBC proprietary JSON format:

camt.052.001.02 - Bank to Customer Account Report - used for balances and intraday transaction reporting for current day transactions
camt.053.001.02 - Bank to Customer Statement - used for reporting of historical date transactions
HSBC JSON - covers balances and transactions for current date and historical periods
This document provides the guidelines required to enable organisations to implement the Account Information responses into their systems and processes.


## Getting Started


## Terms and Condiditions


1. Components of the Agreement. The Developer Agreement (the “Developer Agreement”) is comprised of the following:
these Developer Terms, including the Data Protection Appendix and the Developer Policy;
our Terms and Conditions of Use;
our Branding Guidelines; and
the Documentation.
2. Precedence. If there is an irreconcilable conflict between these Developer Terms and any other document(s) comprising the Developer Agreement, these Developer Terms shall govern. Please note that if you use any of the Spotify Widget (defined below), you will be bound by the separate Spotify Widget Terms.
3. Acceptance of Developer Agreement. We invite you to review, download and use our Spotify Platform. Please note that this invitation is subject to your review of and agreement with the Developer Agreement. You are not required to agree to the Developer Agreement. However, if you reject the Developer Agreement, you do not have any right to use the Spotify Platform. If you use or otherwise access the Spotify Platform, you will be deemed to have accepted the Developer Agreement and entered into a legally binding contract with Spotify AB (“Spotify”, “we”, and sometimes “us”). You may not use the Spotify Platform for any purpose that is not expressly authorized in the Developer Agreement.
4. Acceptance on Behalf of an Organization. If you are developing on behalf of an organization, you agree to the terms of the Developer Agreement for that organization and promise that you have authority to bind that organization and its parents, subsidiaries, and sister companies to the Developer Agreement. In that case, “you” and “your” will refer to that organization, its parents, subsidiaries, and sister companies.
5. Language. In the event that the Developer Agreement, or any part thereof, is translated into other languages and there is a discrepancy between versions in different languages, the English language version shall prevail to the extent that such discrepancy is the result of an error in translation.
6. Independent Contractors. There is no joint venture, partnership, agency, or fiduciary relationship existing between you and Spotify, and the parties do not intend to create any such relationship by the Developer Agreement.
7. Capitalized terms not otherwise defined herein have the meaning given in our other terms and policies, including our Terms and Conditions of Use. The term “including” means “including without limitation.”
8. If you are an Australian consumer or small business (as defined by Australian Consumer Law): These terms do not exclude or limit any statutory rights or remedies you may have under Australian Consumer Law. However, to the extent permitted by the Australian Consumer Law, Spotify’s liability for any breach of the consumer guarantees is limited to (as elected by Spotify) the resupply of the goods or services to which the breach relates or the payment of the cost of replacing the goods or resupplying the services.


## Authentication



## Explanations of Request-Response Cycles


## Links to SDKs and Code Libraries


## Error messages
   ### Error format
   API errors in two ways: standard HTTP response codes and human-readable messages in JSON format.
   
   ### Error – HTTP Response
   ```
   HTTP/1.1 405 Method Not Allowed
   Server: nginx
   Content-Type: application/problem+json; charset=utf-8
   Content-Length: 253
   X-Request-Id: a1efb240-f8d8-40fe-a680-c3a5619a42e9
   Link: <https://us2.api.mailchimp.com/schema/3.0/ProblemDetailDocument.json>; rel="describedBy"
   Date: Thu, 17 Sep 2015 19:02:28 GMT
   Connection: keep-alive
   Set-Cookie: _AVESTA_ENVIRONMENT=prod; path=/
   ```
   
   ### Error – JSON
   ```
   {
    "type": "https://mailchimp.com/developer/marketing/docs/errors/",
    "title": "Method Not Allowed",
    "status": 405,
    "detail": "The requested method and resource are not compatible. See the Allow header for this resource's available methods.",
    "instance": "3b4dcb40-0b6b-4820-bfaa-41267b3826ea"
    }
    ```




## Change log
Change log and release history:

| Version | Release | Date  | Status | Description |
| ------- | --- | ---- |------------- | ----------- |
| V1 | December | 2018 |Deprecated | Account Balance API|
| V2 | June | 2019 | Live | Bug fixes and added Account Transaction API |
