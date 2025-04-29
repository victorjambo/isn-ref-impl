ℹ️ See [EoT Home](https://github.com/ecosystem-of-trust) for background on the Ecosystem or Trust (EoT), terms of reference and other artefacts and important links. See [Border Trade Demonstrators](https://github.com/border-trade-demonstrators) for background information on BTDs and the [dasbhoard](https://github.com/border-trade-demonstrators/dashboard) for details on the different BTDs. Please note this page may link to closed and private repositories which you will need to be invited into.

# BTD 1 - PHA SPS import notifications

## Contents

- [About](https://github.com/border-trade-demonstrators/btd-1#about)
- [Use case](https://github.com/border-trade-demonstrators/btd-1#use-case)
- [Payload definitions](https://github.com/border-trade-demonstrators/btd-1#payload-definitions)
- [Required capabilities](https://github.com/border-trade-demonstrators/btd-1#required-capabilities)
- [Mapping to report content and recommendations](https://github.com/border-trade-demonstrators/btd-1#mapping-to-eot-report-content-and-recommendations)

## About

PHAs will undertake a number of checks at the border (e.g. identity and physical checks). The ability to receive supplementary notification data via signals in a timely manner, particularly for RoRo loads which move quickly, will enable more effective goods control and interventions. 

## Use case

PHA will request a selection of fields (see below) from an EoT System of Record which will be posted on to ISN messaging infrastructure from the EoT operators ISN site via either multipart form data or JSON authenticated via Oauth 2 Bearer token.

PHA may receive signal information via a number of options including:
- dashboard
- vanilla JSON feed
- Server Sent Event (SSE) push (coming soon)
- RSS or feed (potentially if required)

Signals will be prioritised according to a local model per PHA/FSA/Defra requirements and will have an expiry date to ensure information accuracy.

PHA will close a feedback loop by transmitting information to selected BTD participants (initially just CO) indicating whether the information was useful, not useful and what the result of the intervention was - this information will be **anonymised**. This feedback loop will work as part of the evaluation.

Two payloads are accepted and the [signal payloads](https://github.com/information-sharing-networks/signals#example-3---a-signal-and-its-metadata-which-is-associated-to-a-payload-of-information-in-a-given-domain) fields are outlined below.

# Payload definitions
## Pre-notification Payload 
The technical definition can be found [here](https://github.com/border-trade-demonstrators/btd-1/blob/main/isn-btd-1.edn) and a description of the fields is below.

| Field name | Description | Data type | Optionality | Notes |
| --- | --- | --- | --- | --- |
CHED Numbers|A set of CHED identifiers|CHED-P or CHED-D|Optional||
cnCodes|Classification of the goods reference |String |Optional ||
Commodity Description|Plain text description of goods |String|Required|
Country Of Origin|Country goods/sample originated from|ISO3166 (E.G. GB)|Required||
Exporter EORI|Exporter registration number |String|Optional ||
Importer EORI|Importer registration number |String|Optional ||
Location|Location of where goods are loaded|String |Optional ||
Mode|The goods movement mode|Enumeration (only RORO supported in this pilot)|Required||
Unit Identification|A list of identifiers and identifier types (as key/value pairs) for an incoming unit.|May be multiples from a set of identifiers (e.g. container number trailer registration number/VRN/ TRN etc)|Required||
Trailer registration number|Registration number of the trailer|String (e.g. WGM033P)|Required||

Note for the purpose of this pilot signal we have agreed to use the metadata "start" item is being used to hold an ETA for the movement.

Here is example of the JSON that would be used to publish a pre-notification signal to the API

``` json
{
  "h": "event",
  "name": "chicken and beef (movement ref: 955R)",
  "start": "2024-08-27T09:00:00Z",
  "summary": "moving to PortA with ETA 2024-08-28T09:00:00Z",
  "category": [
    "pre-notification",
    "isn@btd-1.info-sharing.network"
  ],
  "payload": {
    "chedNumbers": [
      "CHEDP.GB.2024.1234567"
    ],
    "cnCodes": [
      "1602"
    ],
    "commodityDescription": "Other prepared or preserved meat",
    "countryOfOrigin": "PL",
    "exporterEORI": "PL12300000004358Z",
    "importerEORI": "GB123012792073",
    "location": "Rolpek 2 (50.9457, 16.3523)",
    "mode": "RORO",
    "unitIdentification": {
      "trailerRegistrationNumber": "WGM1234P"
    }
  }
}
```

### Dispatch Payload

Optionally, a dispatch signal payload is sent as an update when the vehicle has set off to the port. The signal will carry additional information to support the identification of the unit such as trailer number, expected/actual time of departure to the port of exit. As this information which is more likely to be avaiable closer to the unit being ready and loaded.

The dispatch signal should be linked to the original pre-notificaton signal using the original correlationId.

Optionally, the fields defined in the pre-notification signal can be include with this payload (in this case the original data will be superceeded with the supplied data).

| Field name | Description | Data type | Optionality | Notes |
| --- | --- | --- | --- | --- |
Seal numbers|Serial number or reference of seals on unit|String |optional ||
Destination Plant|Place of destination of goods |String |optoional ||
Planned departure time|Time of expected departure from loading location |Date Time ISO 8601|Required||
Actual departure time|Confirmed time of actual detarture of goods from loading location |Date Time ISO 8601|Optional||
Port of Exit|Port where the goods are exiting |String (e.g. Calais)|Required||
Port of Entry|Port where the goods are entering in UK|String (e.g. Dover)|Required||

Here is example of the JSON that would be used to publish a dispatch signal to the API

``` json
    {
      "h": "event",
      "name": "chicken and beef (movement ref: PR0X)",
      "summary": "movement despatched",
      "category": [
        "despatch",
        "isn@btd-1.info-sharing.network"
      ],
      "correlation-id": "ae761d05-a10f-4507-a091-290c987d8b5e",
      "payload": {
        "sealNumbers": ["ABC00001", "ABC00002"],
        "destinationPlant": "Birmingham Central",
        "plannedDepartureTime": "2024-08-28T07:00:00.367010Z",
        "actualDepartureTime": "2024-08-29T07:56:16.367010Z",
        "portOfExit": "Calais",
        "portOfEntry": "Dover"
      }
    }
```


## Required capabilities

Participants in this border trade demonstrator pilot need to meet the following requirements.  See the [use cases](https://github.com/border-trade-demonstrators/btd-1#use-cases) above and the [deliverables]

- Pre-notification via selection of fields from underlying EoT or similar system of record transmitted as signals
- Collaboration Agreement must be signed by interested parties

## Mapping to EoT report content and recommendations

- [Report section 5](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#measuring-the-value-of-an-eot-model): Overview of the UK biosecurity regimes - pre-notification
- [Report section 4](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#the-ecosystem-of-trust-model): usecases
- [Report section 6 B](https://www.gov.uk/government/publications/the-ecosystem-of-trust-evaluation-report-2023/the-ecosystem-of-trust-evaluation-report-august-2023-html#recommendations-for-how-we-address-the-challenges-to-eot-adoption): Signals, Collab Agreement
