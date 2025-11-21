---
title: Pillar Two Service Guide (SDES) Banner
weight: 1
---

# Pillar Two Service Guide (SDES) H1


## Overview

This guide explains the taxes applicable under the Pillar 2 agreement and how to use the SDES File Upload API for sharing documentation and ensuring compliance.

Contents:

* What is the Pillar 2 agreement?
* How to use HMRC APIs
* Registering to use HMRC APIs
* Getting approvals for live transfers
* How to prepare your files for transfer
* Endpoints and notifications
* Terms of use
* Getting help


## What is the Pillar 2 agreement?

The Pillar 2 agreement ensures that the largest corporate groups are subject to a global minimum corporation tax rate of 15%. In response, the UK Government has introduced two new taxes:

* Domestic Top-up Tax (DTT)
* Multinational Top-up Tax (MTT)

These taxes apply to accounting periods beginning on or after 31 December 2023.

For more information on MTT and DTT, please consult the [HMRC summary page](https://www.gov.uk/government/collections/multinational-top-up-tax-and-domestic-top-up-tax). 

To meet administrative obligations for these taxes, groups must file a GloBE Information Return (GIR): 

* annually and, 
* in an xml format

The UK participates in the exchange of GIR information with willing partner jurisdictions so UK filing members who submit their GIR to HMRC should not need to file a GIR with other participating tax authorities. 

**Related documentation:**

* <a href="/guides/pillar-two-service-guide/downloads/notification-schema.json" download>Callback Notification Schema</a>
* [OECDPillar2 GloBE Information Return XML Schema](https://www.oecd.org/content/dam/oecd/en/publications/reports/2025/01/globe-information-return-pillar-two-xml-schema_3980638f/c594935a-en.pdf)
* [OECD Pillar2 GloBE Model Rules](https://www.oecd.org/en/publications/tax-challenges-arising-from-digitalisation-of-the-economy-global-anti-base-erosion-model-rules-pillar-two_782bac33-en.html)

## How to use HMRC APIs

GloBE Information Returns (GIRs) must be submitted to HMRC using the Secure Data Exchange Service (SDES).

You can test the File Upload API integration with your software in the sandbox environment at any time.

To transfer live files you will need:

* **Service Reference Number (SRN)** to identify your organisation in the system transfer
* **an information type value** to connect your system transfer to HMRC
* **a submission URL** generated through the Secure Data Exchange File Upload API
* **the GIR XML file** to upload and validate through the Secure Data Exchange File Upload API

To get this information and production credentials to transfer files, you must register for the following services.

## Registering to use HMRC APIs

Follow these steps to get access to use HMRC APIs

1. **Register for the Secure Data Exchange Service (SDES)**. Email [OECDPillar2TPSnew@hmrc.gov.uk](mailto:OECDPillar2TPSnew@hmrc.gov.uk) to request an invitation code to register for SDES. Follow the instructions emailed to you with the invitation code.

2. **Register for the Developer Hub**. You must register with the HMRC Developer Hub to access and set up automated data exchanges with SDES. The SDES ‘File Upload’ API can be found in the ‘Other’ category in ‘API documentation’. [Register with the HMRC Developer Hub](https://developer.service.hmrc.gov.uk/developer/registration)

Production credentials for live transfers will only be issued once registration for these services is complete and the approval process has been successfully followed.

## Getting approval for live transfers

When you’re ready to request approval to take your software with the SDES File Upload API live, follow these steps:

1. Sign in to your Developer Hub account
2. Go to Manage Applications
3. Select ‘Get production credentials’ from the left-hand menu
4. Follow the onscreen instructions to submit your application
5. The Software Development Support Team (SDST) will review your application
6. Once approved, go to ‘Manage Applications’ and select ‘Production applications’

For help with production credentials, contact HMRC Software Developer Support Team (SDST) at [SDSTeam@hmrc.gov.uk](mailto:SDSTeam@hmrc.gov.uk). 

## How to prepare your files for transfer

The File Upload API supports the following journeys:

* Automated transfer of bulk data files from one end system to another
* Reporting file delivery
* Reporting file delivery failures

You must follow the formats and instructions below to successfully transfer a file. Files that do not follow this format may be uploaded to the service, but will fail at the validation stage.

### Accepted file formats
All files must be submitted in a .xml format.

Alternative formats may upload to SDES but will fail at the validation stage.

### File validation
Files are checked to ensure they contain the expected data and structure. We recommend reading the OECD Schema and Business/Model Rules and UK specific Business/ Schema, part of the Pillar 2 API Service Guide. 

## Endpoints and notifications
Notifications inform users when a file’s status changes, if they are registered for SDES and linked to an organisation’s transfer. These alerts are sent by email by default or can be set up as a callback notification.

### Endpoints
The File Upload API must include the following HTTPS RESTful endpoints to send notifications to users:

* URL → Any Rest complaint URL e.g. https://mydomain.com/sdes-callback
* HTTP Method → POST
* Content-Type → application/json
* Optional Header → The customer can optionally specify a bearer token for authorisation purposes
* Request Body must accept messages compliant with the schema specified below


### Callback Notification
Callback notifications tell Secure Data Exchange (SDES) users when a file status changes. The following statuses can be received by email or API notification:

* FileReceived - The file has passed integrity checks and is stored in SDES
* FileProcessingFailure - The file transfer has failed for the listed reason

### FileReceived
This notification tells the user that a file uploaded to or pulled by SDES has been virus-scanned and passed integrity checks. It does not confirm delivery to a named recipient.

```json
{
    "notification": "FileReceived",
    "filename": "32f2c4f7-c635-45e0-bee2-0bdd97a4a70d.zip",
    "checksumAlgorithm": "md5",
    "checksum": "894bed34007114b82fa39e05197f9eec",
    "correlationID": "32f2c4f7-c635-45e0-bee2-0bdd97a4a70d",
    "dateTime": "2020-11-09T16:48:21.659Z",
    "properties": [
                    {
                    "name": "name1",
                    "value": "value1"
                    },
                    {
                    "name": "name2",
                    "value": "value2"
                    }
    ]
  }
```
**FileProcessingFailure**

Notifies the user when the file transfer has failed, including the reason for failure.

```json
  {
      "notification": "FileProcessingFailure",
      "filename": "32f2c4f7-c635-45e0-bee2-0bdd97a4a70d.zip",
      "checksumAlgorithm": "md5",
      "checksum": "894bed34007114b82fa39e05197f9eec",
      "correlationID": "32f2c4f7-c635-45e0-bee2-0bdd97a4a70d",
      "dateTime": "2020-11-09T16:48:21.659Z",
      "failureReason":"Virus Detected",
      "actionRequired":"ADDRESS-FAILURE-THEN-RETRY"
  }
```

**Related documents:**

* <a href="/guides/pillar-two-service-guide/downloads/notification-schema.json" download>Callback Notification Schema</a>
* [Pillars 2 API Service Guide](https://developer.service.hmrc.gov.uk/guides/pillar2-service-guide/)
* [UK Validation Schema]()


## Terms of use
All organisations and their nominated personnel who are using HMRC’s SDES File Upload API are subject to the following terms of use:

* [SDES terms and conditions]()
* [Developer Hub terms and conditions]()
* [Developer Hub terms of use]()

## Getting help

**Help accessing and using the Secure Data Exchange Service**

For help registering with the Secure Data Exchange (SDES), or for assistance uploading or downloading files, contact the dedicated support team.

Monday - Friday 9 am - 5.30 pm (excluding Bank Holidays)

Email: [sdessupport@hmrc.gov.uk](mailto:sdessupport@hmrc.gov.uk)

Telephone: 03000 597222

**Help accessing and using the HMRC Developer Hub**

For help accessing your Developer Hub account, or for assistance applying for production credentials, contact the Software Development Support team at [SDSTeam@hmrc.gov.uk](mailto:SDSTeam@hmrc.gov.uk).

***
<u>**Version details**</u>

Version 1.0 issued: November 2025

