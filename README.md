Developer Documentation
=======================

Learn how to describe what providers and drugs are covered by a particular health plan.

JSON
----

All information must be described in the JSON file format. JSON is a lightweight
and simple way to represent machine-readable data. It is quickly becoming the de
facto standard for shuttling data across the internet, fueled primarily by the
rise of mobile and APIs. Modern programming languages can interpret and produce
JSON out of the box.

[Learn about JSON >](#)


Public Discoverability
----------------------

Organizations must post their `plans.json`, `providers.json`, and `drugs.json` files on their websites, accessible to the public. 

The path to the URLs will be submitted via HIOS to CMS. 

The JSON URLs listed above *must* be provided over HTTPS to ensure the integrity of the data.


Health Plans - plans.json
-------------------------

### Description

`plans.json` contains a list of health plans and their corresponding network of providers and formularies.


### Schema

| Field               | Label                          | Definition                                                                                                           | Required |
| -----               | -----                          | ----------                                                                                                           | -------- |
| **plan_id_type**    | ID Type                        | Type of Plan ID. The preferred is the HIOS Plan ID - `HIOS-PLAN-ID`                                                  | Always   |
| **plan_id**         | Unique Identifier              | The 14-character, HIOS-generated Plan ID number. (Plan IDs must be unique, even across different markets.)           | Always   |
| **marketing_name**  | Marketing Name                 | The name of the plan as it is displayed on HealthCare.gov                                                            | Always   |
| **summary_url**     | URL for Plan Information       | The URL that goes directly to the summary of benefits and coverage for the specific standard plan or plan variation. | Always   |
| **marketing_url**   | URL for Plan Information       | The URL that goes directly to the plan brochure for the specific standard plan or plan variation.                    | No       |
| **formulary_url**  | URL for Formulary              | The URL that goes directly to the formulary brochure for the specific standard plan or plan variation.                    | No       |
| **plan_contact**    | Contact Email Address for Plan | An email address for developers/public to report mistakes in the network and formulary data.                         | Always   |
| **network**         | Network                        | Array of networks                                                                                                    | Always   |
| **formulary**       | Formularies                    | Array of drug lists                                                                                                  | Always   |
| **benefits**       | Benefits                    | Array of benefits                                                                                                  | No   |
| **last_updated_on** | Last Updated On                | ISO 8601 format (e.g. YYYY-MM-DD) | Always   |


#### Network sub-type

| Field              | Label        | Definition                                                                                             | Required |
| -----              | -----        | ----------                                                                                             | -------- |
| ***network_tier*** | Network Tier | Tier for network (Example Values: `PREFERRED`, `NON-PREFERRED`, etc. Values should be all uppercase. ) | Always   |


#### Formulary sub-type

| Field              | Label        | Definition                                                                                                                                                                                                                                            | Required |
| -----              | -----        | ----------                                                                                                                                                                                                                                            | -------- |
| ***drug_tier***    | Drug Tier    | Tier for formulary - (Example Values: `GENERIC`, `PREFERRED-GENERIC`, `NON-PREFERRED-GENERIC`, `SPECIALTY`, `BRAND`, `PREFERRED-BRAND`, `NON-PREFERRED-BRAND`, `ZERO-COST-SHARE-PREVENTIVE`, `MEDICAL-SERVICE`, etc. Values should be all uppercase.) | Always   |
| ***mail_order***   | Mail Order   | Does the formulary cover mail order? - (Values: `true` or `false`)                                                                                                                                                                                    | Always   |
| ***cost_sharing*** | Cost Sharing | Array of cost sharing values (see "Cost sharing sub-type" below)                                                                                                                                                                                      | Always   |


#### Cost sharing sub-type

| Field                | Label              | Definition                                                                                                                                                                                         | Required |
| -----                | -----              | ----------                                                                                                                                                                                         | -------- |
| **pharmacy_type**    | Pharmacy Type      | Pharmacy type (Example Values: `1-MONTH-IN-RETAIL`, `1-MONTH-OUT-RETAIL`, `1-MONTH-IN-MAIL`, `1-MONTH-OUT-MAIL`, `3-MONTH-IN-RETAIL`, `3-MONTH-OUT-RETAIL`, `3-MONTH-IN-MAIL`, `3-MONTH-OUT-MAIL`) | Always   |
| **copay_amount**     | Copay amount       | Amount of copay, in $ (number)                                                                                                                                                                     | Always   |
| **copay_opt**        | Copay option       | Qualifier of copay amount (Values: `AFTER-DEDUCTIBLE`, `BEFORE-DEDUCTIBLE`, `NO-CHARGE`, `NO-CHARGE-AFTER-DEDUCTIBLE`                                                                              | Always   |
| **coinsurance_rate** | Coinsurance rate   | Rate of coinsurance (float, 0.0 to 1.0)                                                                                                                                                            | Always   |
| **coinsurance_opt**  | Coinsurance option | Qualifier for coinsurance rate (Values: `AFTER-DEDUCTIBLE`, `NO-CHARGE`, `NO-CHARGE-AFTER-DEDUCTIBLE`)                                                                                             | Always   |


#### Benefits sub-type

The **Benefits** sub-type is an optional section and will be shaped depending on what industry and consumers find valuable. 

For example, many health plans are offering telemedicine as an additional health benefit and that can be highlighted by adding a `telemedicine` entry.

| Field                | Label              | Definition                                                                                                                                                                                         | Required |
| -----                | -----              | ----------                                                                                                                                                                                         | -------- |
| **telemedicine**    | Offers Telemedicine     | Does the plan cover telemedicine? - (Values: `true` or `false`)    | No   |


### Example 

```
[
    {
        "plan_id_type": "HIOS-PLAN-ID",
        "plan_id": "12345XX9876543",
        "marketing_name": "Sample Gold Health Plan",
        "summary_url": "http://url/to/summary/benefits/coverage",
        "marketing_url": "http://url/to/health/plan/information",
        "formulary_url": "http://url/to/formulary/information",
        "plan_contact": "email@address.com",
        "network": [
            {
                "network_tier": "PREFERRED"
            },
            {
                "network_tier": "NON-PREFERRED"
            }
        ],
        "formulary": [
            {
                "drug_tier": "GENERIC",
                "mail_order": true,
                "cost_sharing": [
                    {
                        "pharmacy_type": "1-MONTH-IN-RETAIL",
                        "copay_amount": 20.0,
                        "copay_opt": "AFTER-DEDUCTIBLE",
                        "coinsurance_rate": 0.10,
                        "coinsurance_opt": "BEFORE-DEDUCTIBLE"
                    },
                    {
                        "pharmacy_type": "1-MONTH-IN-MAIL",
                        "copay_amount": 0.0,
                        "copay_opt": "NO-CHARGE",
                        "coinsurance_rate": 0.20,
                        "coinsurance_opt": null
                    }
                ]
            },
            {
                "drug_tier": "BRAND",
                "mail_order": true,
                "cost_sharing": [
                    {
                        "pharmacy_type": "1-MONTH-IN-RETAIL",
                        "copay_amount": 15.0,
                        "copay_opt": null,
                        "coinsurance_rate": 0.0,
                        "coinsurance_opt": null
                    },
                    {
                        "pharmacy_type": "1-MONTH-IN-MAIL",
                        "copay_amount": 20.0,
                        "copay_opt": "AFTER-DEDUCTIBLE",
                        "coinsurance_rate": 0.10,
                        "coinsurance_opt": "BEFORE-DEDUCTIBLE"
                    }
                ]
            }
        ],
        "last_updated_on": "2015-03-17"
    }
]
```

Providers - providers.json
--------------------------

### Description

`providers.json` contains a list of providers and the plans that cover their services.

### Schema

| Field     | Label                | Definition                                                                                                 | Required |
| -----     | -----                | ----------                                                                                                 | -------- |
| **npi**   | National Provider ID | The National Provider Identifier (NPI) is a unique identification number for covered health care providers | Always   |
| **type**  | Type                 | Specify if `INDIVIDUAL` or `FACILITY`                                                                      | Always   |
| **plans** | Plans                | Array of plans that cover this provider (see "Plans sub-type" below)                                       | Always   |
| **last_updated_on** | Last Updated On                | Date of when the record for this provider has been last updated or refreshed - ISO 8601 format (e.g. YYYY-MM-DD)                | Always   |

If the entry is for an `INDIVIDUAL` then the following fields should be present:

| Field           | Label              | Definition                                                        | Required |
| -----           | -----              | ----------                                                        | -------- |
| **name**        | Name               | -                                                                 | Always   |
| ***prefix***    | Prefix             | -                                                                 | No       |
| ***first***     | First Name         | -                                                                 | Always   |
| ***middle***    | Middle Name        | -                                                                 | No       |
| ***last***      | Last Name          | -                                                                 | Always   |
| ***suffix***    | Suffix             | -                                                                 | No       |
| **address**     | Address            | -                                                                 | Always   |
| ***address***   | Street Address     | -                                                                 | Always   |
| ***address_2*** | Street Address 2   | -                                                                 | No       |
| ***city***      | City               | -                                                                 | Always   |
| ***state***     | State Abbreviation | -                                                                 | Always   |
| ***zip***       | Zip Code           | -                                                                 | Always   |
| **phone**       | Phone Number       | -                                                                 | Always   |
| **specialty**   | Specialty Type     | An array of speciality types.                                     | Always   |
| **accepting**   | Accepting Patients | Is the provider accepting patients? - (Values: `true` or `false`) | Always   |
| **gender**      | Gender             | Values: `Male`, `Female`, `Other`                                 | Optional   |
| **languages**   | Languages Spoken   | An array of the languages spoken                                  | Optional   |

If the entry is for a `FACILITY` then the following fields should be present:

| Field             | Label              | Definition | Required |
| -----             | -----              | ---------- | -------- |
| **facility_name** | Facility Name      | -          | Always   |
| **facility_type** | Facility Type      | An array of facility types. | Always   |
| **address**       | Address            | -          | Always   |
| ***address***     | Street Address     | -          | Always   |
| ***address_2***   | Street Address 2   | -          | No       |
| ***city***        | City               | -          | Always   |
| ***state***       | State Abbreviation | -          | Always   |
| ***zip***         | Zip Code           | -          | Always   |
| **phone**         | Phone Number       | -          | Always   |


#### Plans sub-type

| Field              | Label             | Definition                                                                                                 | Required |
| -----              | -----             | ----------                                                                                                 | -------- |
| ***plan_id_type*** | ID Type           | Type of Plan ID. The preferred is the HIOS Plan ID - `HIOS-PLAN-ID`                                        | Always   |
| ***plan_id***      | Unique Identifier | The 14-character, HIOS-generated Plan ID number. (Plan IDs must be unique, even across different markets.) | Always   |
| ***network_tier*** | Network Tier      | Tier for network (Example Values: `PREFERRED`, `NON-PREFERRED`, etc. Values should be all uppercase. )     | Always   |


### Example

```
[
    {
        "npi": "1234567890123456",
        "type": "INDIVIDUAL",
        "name": {
            "first": "Sarah",
            "middle": "Maya",
            "last": "Ngyuen",
            "suffix": "Jr."
        },
        "address": {
            "address": "123 Main Street",
            "address_2": "Suite 120",
            "city": "Little Rock",
            "state": "AR",
            "zip": "72201"
        },
        "phone": "2025551212",
        "specialty": ["Ophthalmology"],
        "accepting": true,
        "plans": [
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "12345XX9876543",
                "network_tier": "PREFERRED"
            },
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "12345XX9876543",
                "network_tier": "NON-PREFERRED"
            }
        ],
        "languages": ["English", "Spanish", "Mandarin"],
        "last_updated_on": "2015-03-17"
    },
    {
        "npi": "1234567890123949",
        "type": "FACILITY",
        "facility_name": "Main Street Hospital",
        "facility_type": ["Hospital"],
        "address": {
            "address": "123 Main Street",
            "address_2": "Suite 120",
            "city": "Little Rock",
            "state": "AR",
            "zip": "72201"
        },
        "phone": "2025551212",
        "plans": [
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "12345XX9876543",
                "network_tier": "PREFERRED"
            },
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "12345XX9876543",
                "network_tier": "NON-PREFERRED"
            }
        ],
        "last_updated_on": "2015-03-17"
    }
]
```

Drugs - drugs.json
------------------------------

### Description

`drugs.json` contains a list of drugs and the plans that cover them.

### Schema

| Field         | Label           | Definition                                                       | Required |
| -----         | -----           | ----------                                                       | -------- |
| **rxnorm_id** | Drug Identifier | RxCUI (Specific drug identifier from RXNORM)                     | Always   |
| **drug_name** | Drug Name       | Name of Drug                                                     | Always   |
| **plans**     | Plans           | Array of plans that cover this drug (see "Plans sub-type" below) | Always   |


#### Plans sub-type

| Field                     | Label                        | Definition                                                                                                                                                                                                                                           | Required |
| -----                     | -----                        | ----------                                                                                                                                                                                                                                           | -------- |
| ***plan_id_type***        | ID Type                      | Type of Plan ID. The preferred is the HIOS Plan ID - `HIOS-PLAN-ID`                                                                                                                                                                                  | Always   |
| ***plan_id***             | Unique Identifier            | The 14-character, HIOS-generated Plan ID number. (Plan IDs must be unique, even across different markets.)                                                                                                                                           | Always   |
| ***drug_tier***           | Drug Tier                    | Tier for formulary (Example Values: `GENERIC`, `PREFERRED-GENERIC`, `NON-PREFERRED-GENERIC`, `SPECIALTY`, `BRAND`, `PREFERRED-BRAND`, `NON-PREFERRED-BRAND`, `ZERO-COST-SHARE-PREVENTIVE`, `MEDICAL-SERVICE`, etc. Values should be all uppercase. ) | Always   |
| ***prior_authorization*** | Prior Authorization Required | Is prior authorization required? - (Values: `true` or `false`)                                                                                                                                                                                       | Always   |
| ***step_therapy***        | Step Therapy Required        | Is step therapy required? - (Values: `true` or `false`)                                                                                                                                                                                              | Always   |
| ***quantity_limit***      | Quantity Limit               | Is there a quantity limit for this drug? - (Values: `true` or `false`)                                                                                                                                                                               | Always   |



### Example

```
[
    {
        "rxnorm_id": "209459",
        "drug_name": "Acetaminophen 500 MG Oral Tablet [Tylenol]",
        "plans": [
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "12345XX9876543",
                "drug_tier": "GENERIC",
                "prior_authorization": false,
                "step_therapy": false,
                "quantity_limit": false
            },
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "56748XXX123933",
                "drug_tier": "GENERIC",
                "prior_authorization": false,
                "step_therapy": false,
                "quantity_limit": false
            }
        ]
    },
    {
        "rxnorm_id": "248656",
        "drug_name": "Azithromycin 500 MG Oral Tablet [Zithromax]",
        "plans": [
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "12345XX9876543",
                "drug_tier": "GENERIC",
                "prior_authorization": false,
                "step_therapy": false,
                "quantity_limit": true
            },
            {
                "plan_id_type": "HIOS-PLAN-ID",
                "plan_id": "56748XXX123933",
                "drug_tier": "GENERIC",
                "prior_authorization": false,
                "step_therapy": false,
                "quantity_limit": false
            }
        ]
    }
]
```
