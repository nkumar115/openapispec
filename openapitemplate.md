## Overview Section

The Account Information API is used to inform the account owner of the Balance and Transaction entries booked to the account for a given date or time-range on a specific day.

The information returned by these APIs can be requested as either a standardised response as per ISO 20022 Cash Management (camt) messaging standards or an HSBC proprietary JSON format:

camt.052.001.02 - Bank to Customer Account Report - used for balances and intraday transaction reporting for current day transactions
camt.053.001.02 - Bank to Customer Statement - used for reporting of historical date transactions
HSBC JSON - covers balances and transactions for current date and historical periods
This document provides the guidelines required to enable organisations to implement the Account Information responses into their systems and processes.


## Getting Started


## Terms and Condiditions


## Authentication



## Explanations of Request-Response Cycles


## Links to SDKs and Code Libraries


## Error messages


## Change log
Change log and release history:

| Version | Release | Date  | Status | Description |
| ------- | --- | ---- |------------- | ----------- |
| V1 | December | 2018 |Deprecated | Account Balance API|
| -------- | --- | ---- | ---------- | ----------- |
| V2 | June | 2019 | Live | Bug fixes and added Account Transaction API |
