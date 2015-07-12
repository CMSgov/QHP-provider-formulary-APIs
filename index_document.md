Index document
==============

To support discovery of the machine-readable issuer JSON files for plans,
providers, and formularies, issuers MUST publish an index document, in JSON
format, to a **well-known URL** on their web site.

The path of the index document SHALL BE `/cms-data-index.json`.

Assume an issuer has a public web site at the domain name `example-issuer.com`.
A fully-qualified well-known URL therefore would be:

```
http://example-issuer.com/cms-data-index.json
```

This standard does not provide guidance on the use of `https` vs. `http` for
a scheme: either or both are valid for the purposes of the index document's URL.

This standard does not provide guidance on the MIME media type the docuemnt
shall be serve as. According to RFC 4627, the media type for JSON text is
`application/json`.

Schema
------

The schema of the index document is as follows:

| Field | Description | Required? |
| ----- | ----------- | --------- |
| provider_urls | An array of URLs of JSON files that conform to the provider schema, minimum of 1 required | Required |
| formulary_urls | An array of URLs of JSON files that conform to the formulary schema, minimum of 1 required | Required |
| provider_urls | An array of URLs of JSON files that conform to the provider schema, minimum of 1 required | Required |

### Example

``` json
{
    "provider_urls": [
        "http://example-issuer.com/data/providers-1.json",
        "http://example-issuer.com/data/providers-2.json"
    ],
    "formulary_urls": [
        "http://example-issuer.com/data/formularies-1.json",
        "http://example-issuer.com/data/formularies-2.json",
        "http://example-issuer.com/data/formularies-3.json"
    ],
    "plan_urls": [
        "http://example-issuer.com/data/plans.json"
    ]
}
```
