Developer Documentation
=======================

Learn how to describe what providers and drugs are covered by a particular health plan.

JSON
----

All information must be described in the JSON file format. JSON is a lightweight and simple way to represent machine-readable data. It is quickly becoming the de facto standard for shuttling data across the internet, fueled primarily by the rise of mobile and APIs. Modern programming languages can interpret and produce JSON out of the box.

[Learn about JSON >](http://json.org/)


Public Discoverability
----------------------

Organizations must post their `index.json`, `plans.json`, `providers.json`, and `drugs.json` files on a website, accessible to the public. 

The JSON URLs listed above *must* be provided over HTTPS to ensure the integrity of the data.

Data types
----------

All values in the JSON are strings, unless otherwise noted in the `Definition` field. 

Dates should be strings, in ISO 8601 format (e.g. YYYY-MM-DD).

Health Plans - plans.json
-------------------------

### Description

`plans.json` contains a list of health plans and their corresponding network of providers and formularies.


### Schema

| Field               | Label                          | Definition                                                                                                           | Required |
| -----               | -----                          | ----------                                                                                                           | -------- |
| **plan_id_type**    | ID Type                        | Type of Plan ID. For all Marketplace plans this should be: `HIOS-PLAN-ID`                                                  | Yes   |
| **plan_id**         | Unique Identifier              | The 14-character, HIOS-generated Plan ID number. (Plan IDs must be unique, even across different markets.)           | Yes   |
| **marketing_name**  | Marketing Name                 | The name of the plan as it is displayed on HealthCare.gov                                                            | Yes   |
| **summary_url**     | URL for Plan Information       | The URL that goes directly to the summary of benefits and coverage for the specific standard plan or plan variation. | Yes   |
| **marketing_url**   | URL for Plan Information       | The URL that goes directly to the plan brochure for the specific standard plan or plan variation.                    | Yes       |
| **formulary_url**  | URL for Formulary              | The URL that goes directly to the formulary brochure for the specific standard plan or plan variation.                    | No       |
| **plan_contact**    | Contact Email Address for Plan | An email address for developers/public to report mistakes in the network and formulary data.                         | Yes   |
| **network**         | Network                        | Array of networks                                                                                                    | Yes   |
| **formulary**       | Formulary                    | The formulary associated with this plan.                                         | Yes   |
| **benefits**       | Benefits                    | Array of benefits                                                                                                  | No   |
| **last_updated_on** | Last Updated On                | ISO 8601 format (e.g. YYYY-MM-DD) | Yes   |


#### Network sub-type

This type defines a network within a plan. The values should be something that is meaningful to an issuer, there is no taxonomy of network tier names. This value will be used later in the `providers.json` file to connect a provider to a specific plan and network tier within that plan.

| Field              | Label        | Definition                                                                                             | Required |
| -----              | -----        | ----------                                                                                             | -------- |
| ***network_tier*** | Network Tier | Tier name for network (Example Values: `PREFERRED`, `NON-PREFERRED`, etc. Values should be all uppercase. ) | Yes   |

#### Formulary sub-type

This type defines a formulary within a plan. The values should be something that is meaningful to an issuer, there is no taxonomy of formulary tier names. This value will be used later in the `drugs.json` file to connect a provider to a specific plan and network tier within that plan.

| Field              | Label        | Definition                                                                                                                                                                                                                                            | Required |
| -----              | -----        | ----------                                                                                                                                                                                                                                            | -------- |
| ***drug_tier***    | Drug Tier    | Tier for formulary - (Example Values: `GENERIC`, `PREFERRED-GENERIC`, `NON-PREFERRED-GENERIC`, `SPECIALTY`, `BRAND`, `PREFERRED-BRAND`, `NON-PREFERRED-BRAND`, `ZERO-COST-SHARE-PREVENTIVE`, `MEDICAL-SERVICE`, etc. Values should be all uppercase.) | Yes   |
| ***mail_order***   | Mail Order   | Does the formulary cover mail order? - (Values: `true` or `false`)                                                                                                                                                                                    | Yes   |
| ***cost_sharing*** | Cost Sharing | Array of cost sharing values (see "Cost sharing sub-type" below)                                                                                                                                                                                      | No   |

#### Cost sharing sub-type

| Field                | Label              | Definition                                                                                                                                                                                         | Required |
| -----                | -----              | ----------                                                                                                                                                                                         | -------- |
| **pharmacy_type**    | Pharmacy Type      | Pharmacy type (Example Values: `1-MONTH-IN-RETAIL`, `1-MONTH-OUT-RETAIL`, `1-MONTH-IN-MAIL`, `1-MONTH-OUT-MAIL`, `3-MONTH-IN-RETAIL`, `3-MONTH-OUT-RETAIL`, `3-MONTH-IN-MAIL`, `3-MONTH-OUT-MAIL`) | Yes   |
| **copay_amount**     | Copay amount       | Amount of copay, in $ (number)                                                                                                                                                                     | Yes   |
| **copay_opt**        | Copay option       | Qualifier of copay amount (Values: `AFTER-DEDUCTIBLE`, `BEFORE-DEDUCTIBLE`, `NO-CHARGE`, `NO-CHARGE-AFTER-DEDUCTIBLE`                                                                              | Yes   |
| **coinsurance_rate** | Coinsurance rate   | Rate of coinsurance (float, 0.0 to 1.0)                                                                                                                                                            | Yes   |
| **coinsurance_opt**  | Coinsurance option | Qualifier for coinsurance rate (Values: `AFTER-DEDUCTIBLE`, `NO-CHARGE`, `NO-CHARGE-AFTER-DEDUCTIBLE`)                                                                                             | Yes   |


#### Benefits sub-type

The **Benefits** sub-type is an No section and will be shaped depending on what industry and consumers find valuable. 

For example, many health plans are offering telemedicine as an additional health benefit and that can be highlighted by adding a `telemedicine` entry.

| Field            | Label               | Definition                                                                            | Required |
| -----            | -----               | ----------                                                                            | -------- |
| **telemedicine** | Offers Telemedicine | Does the plan cover telemedicine? Boolean (values should be either `true` or `false`) | No   |


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
        "formulary": {
          "drug_tier": "BASIC",
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
        "last_updated_on": "2015-03-17"
    }
]
```

Providers - providers.json
--------------------------

### Description

`providers.json` contains a list of providers and the plans that cover their services.

If a provider has more than one NPI number, please create seperate entries for each NPI number. If there is no NPI number, set the value to null (`{"npi": null}`)

### Schema

| Field               | Label                | Definition                                                                                                              | Required |
| -----               | -----                | ----------                                                                                                              | -------- |
| **npi**             | National Provider ID | The 10-digit National Provider Identifier (NPI) is a unique identification number for covered health care providers              | Yes   |
| **type**            | Type                 | Specify if `INDIVIDUAL` or `FACILITY`                                                                                   | Yes   |
| **plans**           | Plans                | Array of plans that cover this provider (see "Plans sub-type" below)                                                    | Yes   |
| **last_updated_on** | Last Updated On      | Date of when the record for this provider has been last updated or refreshed - ISO 8601 format (e.g. YYYY-MM-DD) | Yes   |

If the entry is for an `INDIVIDUAL` then the following fields should be present:

| Field           | Label              | Definition                                                        | Required |
| -----           | -----              | ----------                                                        | -------- |
| **name**        | Name               | -                                                                 | Yes   |
| ***prefix***    | Prefix             | One of `Mr.`, `Mrs.`, `Miss`, `Ms.`, `Dr.`                        | No |
| ***first***     | First Name         | Full first name                                                   | Yes   |
| ***middle***    | Middle Name        | Full middle name                                                  | No |
| ***last***      | Last Name          | Full last name                                                    | Yes   |
| ***suffix***    | Suffix             | One of `Jr.`, `Sr.`, `II`, `III`, `III`, `IV`                     | No |
| **addresses**   | Address            | List of addresses for this provider                               | Yes   |
| ***address***   | Street Address     | -                                                                 | Yes   |
| ***address_2*** | Street Address 2   | -                                                                 | No |
| ***city***      | City               | -                                                                 | Yes   |
| ***state***     | State Abbreviation | Two letter state abbreviation (FL, IA, etc.)                      | Yes   |
| ***zip***       | Zip Code           | Five digit zip code, represented as a string                      | Yes   |
| ***phone***     | Phone Number       | Phone number for this address, represented as a string of numbers | Yes |
| **speciality**  | Specialty Type     | An array of speciality types. Free form text field.               | Yes |
| **accepting**   | Accepting Patients | Is the provider accepting new patients? One of three values: `accepting`, `not accepting`, `accepting in some locations` | No |
| **gender**      | Gender             | Values: `Male`, `Female`, `Other`                                 | Yes |
| **languages**   | Languages Spoken   | An array of the languages spoken                                  | Yes |

If the entry is for a `FACILITY` then the following fields should be present:

| Field             | Label              | Definition                                        | Required |
| -----             | -----              | ----------                                        | -------- |
| **facility_name** | Facility Name      | -                                                 | Yes |
| **facility_type** | Facility Type      | An array of facility types. Free-form text field. | Yes |
| **addresses**     | Address            | List of addresses for this facility (Limit 1)     | Yes |
| ***address***     | Street Address     | -                                                 | Yes |
| ***address_2***   | Street Address 2   | -                                                 | No  |
| ***city***        | City               | -                                                 | Yes |
| ***state***       | State Abbreviation | Two letter state abbreviation (FL, IA, etc.)      | Yes |
| ***zip***         | Zip Code           | Five digit zip code, represented as a string      | Yes |
| ***phone***       | Phone Number       | Phone number for this address, string             | Yes  |


#### Plans sub-type

| Field              | Label             | Definition                                                                                                 | Required |
| -----              | -----             | ----------                                                                                                 | -------- |
| ***plan_id_type*** | ID Type           | Type of Plan ID. For all Marketplace plans this should be: `HIOS-PLAN-ID`                                  | Yes   |
| ***plan_id***      | Unique Identifier | The plan ID that was used in the plans.json as the `plan_id` value. For a Marketplace plan, this must be the 14-digit HIOS plan id. | Yes   |
| ***network_tier*** | Network Tier      | Tier for network (Example Values: `PREFERRED`, `NON-PREFERRED`, etc. Values should be all uppercase.) Must match a network tier defined in the corresponding plan record in a `plans.json` file. | Yes   |


### Example

```
[
    {
        "npi": "1234567893",
        "type": "INDIVIDUAL",
        "name": {
            "first": "Sarah",
            "middle": "Maya",
            "last": "Ngyuen",
            "suffix": "Jr."
        },
        "addresses": [
          {
            "address": "123 Main St",
            "address_2": "Suite 120",
            "city": "Little Rock",
            "state": "AR",
            "zip": "72201",
            "phone": "2025551212"
          },
          {
            "address": "675 South St",
            "city": "Little Rock",
            "state": "AR",
            "zip": "72201",
            "phone": "2025551212"
          },
        ],
        "specialty": ["Ophthalmology", "Endocrinology"],
        "accepting": "accepting",
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
        "gender": "Female",
        "last_updated_on": "2015-03-17"
    },
    {
        "npi": "1234567893",
        "type": "FACILITY",
        "facility_name": "Main Street Hospital",
        "facility_type": ["Hospital", "Dialysis"],
        "addresses": [
          {
            "address": "123 Main St",
            "address_2": "Suite 120",
            "city": "Little Rock",
            "state": "AR",
            "zip": "72201",
            "phone": "2025551212"
          }
        ],
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
| **rxnorm_id** | Drug Identifier | RxCUI (Specific drug identifier from RXNORM)                     | Yes   |
| **drug_name** | Drug Name       | Name of Drug                                                     | Yes   |
| **plans**     | Plans           | Array of plans that cover this drug (see "Plans sub-type" below) | Yes   |


#### Plans sub-type

| Field                     | Label                        | Definition                                                                                                                                                                                                                                           | Required |
| -----                     | -----                        | ----------                                                                                                                                                                                                                                           | -------- |
| ***plan_id_type*** | ID Type           | Type of Plan ID. For all Marketplace plans this should be: `HIOS-PLAN-ID`                                  | Yes   |
| ***plan_id***      | Unique Identifier | The plan ID that was used in the plans.json as the `plan_id` value. For a Marketplace plan, this must be the 14-digit HIOS plan id. | Yes   |
| ***drug_tier***           | Drug Tier                    | Tier for formulary (Example Values: `GENERIC`, `PREFERRED-GENERIC`, `NON-PREFERRED-GENERIC`, `SPECIALTY`, `BRAND`, `PREFERRED-BRAND`, `NON-PREFERRED-BRAND`, `ZERO-COST-SHARE-PREVENTIVE`, `MEDICAL-SERVICE`, etc. Values should be all uppercase. ) | Yes   |
| ***prior_authorization*** | Prior Authorization Required | Is prior authorization required? - (boolean value: `true` or `false`)                                                                                                                                                                                       | No   |
| ***step_therapy***        | Step Therapy Required        | Is step therapy required? - (boolean value: `true` or `false`)                                                                                                                                                                                              | No   |
| ***quantity_limit***      | Quantity Limit               | Is there a quantity limit for this drug? - (boolean value: `true` or `false`)                                                                                                                                                                               | No   |



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
