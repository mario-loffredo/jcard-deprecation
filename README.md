# jCard Deprecation in RDAP

This project aims at deprecating jCard in RDAP.

# Table of contents
1. [Motivations](#motivations)
2. [Converting jCard into JSCard](#converting-jcard-into-jscontact)


<a name="motivations"></a>
## 1. Motivations

According to the feedback from [RDAP Pilot WG](https://www.icann.org/en/system/files/files/rdap-pilot-report-25apr19-en.pdf), the most frequent concern about RDAP implementation at both server and client side is currently related to the handling of [jCard](https://tools.ietf.org/html/rfc7095). In fact, it is considered:

*	unintuitive;
*	very complicated for both consumers and servers;
*	uncompliant with REST API standards;
*	affecting performance.

Such a feeling is not limited to RDAP implementers but is also shared by most of APIs producers and consumers dealing with jCard.
The JSContact specification [RFC9553](https://www.rfc-editor.org/rfc/rfc9553.html) includes a contact representation that is able to represent the same information as jCard more efficiently. In particular, it meets the requirements from RDAP implementers about representing multilingual information and unstructured data. Therefore, jCard might be deprecated and replaced by the contact card defined by JSContact in the RDAP responses.


<a name="converting-jcard-into-jscontact"></a>
## 2. Converting jCard into JSContact

While the jCard element in the RDAP response is named "vcardArray", its JSContact counterpart is called "jscontact_card" to be compliant with the extension identifier defined in the rdapConformance array. Here in the following examples of mapping between &quot;vcardArray&quot; and &quot;jscontact_card&quot; according to the policy defined in [JSContact: Converting from and to vCard](https://datatracker.ietf.org/doc/draft-ietf-calext-jscontact-vcard/) are shown.

### Example of an entity lookup response

```
   {
      "rdapConformance": [
         "rdap_level_0",
         "jscontact"
      ],
      "objectClassName": "entity",
      "handle":"XXXX",
      "jscontact_card":{
        "@type": "Card",
        "version": "1.0",
        "uid": "74b64df3-2d60-56b4-9df3-8594886f4456",
        "language": "en",
        "name": {
          "full": "Joe User",
          "components": [
            { "kind": "surname", "value": "User" },
            { "kind": "given", "value": "Joe" }
          ]
        },
        "organizations": {
          "org": {
            "name": "Example"
          }
        },
         "addresses": {
           "addr": {
             "components": [
               { "kind": "name", "value": "4321 Rue Somewhere" },
               { "kind": "extension", "value": "Suite 1234" },
               { "kind": "locality", "value": "Quebec" },
               { "kind": "region", "value": "QC" },
               { "kind": "postcode", "value": "G1V 2M2" },
               { "kind": "country", "value": "Canada" }
             ],
             "countryCode": "CA",
             "coordinates": "geo:46.772673,-71.282945"
           },
           "addresses-1": {
             "full": "123 Maple Ave Vancouver BC 1239"
             "contexts": { "private": true }
           }
         },
        "phones": {
          "voice": {
            "features": { "voice": true },
            "number": "tel:+1-555-555-1234"
          },
          "fax": {
            "features": { "fax": true },
            "number": "tel:+1-555-555-4321"
          }
        },
        "emails": {
          "email": {
            "address": "joe.user@example.com"
          }
        },
        "links": {
          "url": {
            "uri": "https://www.example.com"
          },
          "contact-uri": {
            "kind": "contact",
            "uri": "mailto:contact@example.com"
          }
        }
      },
      "roles":[ "registrar" ],
      "publicIds":[
        {
          "type":"IANA Registrar ID",
          "identifier":"1"
        }
      ],
      "links":[
        {
          "value":"https://example.com/entity/XXXX",
          "rel":"self",
          "href":"https://example.com/entity/XXXX",
          "type" : "application/rdap+json"
        }
      ],
      "events":[
        {
          "eventAction":"registration",
          "eventDate":"1990-12-31T23:59:59Z"
        }
      ],
      "asEventActor":[
        {
          "eventAction":"last changed",
          "eventDate":"1991-12-31T23:59:59Z"
        }
      ]
   }
```

### Elided example of an entity lookup response including multilingual information

```
...
"jscontact_card": {
  "@type": "Card",
  "version": "1.0",
  "uid": "7812cafe-336e-5969-988b-ad68f78ae90f",
  "language": "en",
  "name": {
    "full": "Vasya Pupkin"
  },
  "organizations": {
    "org": {
      "name": "My Company"
    }
  },
  "addresses": {
    "addr": {
      "components": [
        { "kind": "name", "value": "1 Street" },
        { "kind": "postOfficeBox", "value": "01001" },
        { "kind": "locality", "value": "Kyiv" }
      ],
      "countryCode": "UA"
    }
  },
  "localizations": {
    "ua": {
      "addresses": {
        "addr": {
          "components": [
           { "kind": "name", "value": "1, Улица" },
           { "kind": "postOfficeBox", "value": "01001" },
           { "kind": "locality", "value": "Киев" }
          ],
          "countryCode": "UA"
        }
      },
      "name": {
        "full": "Вася Пупкин"
      },
      "organizations": {
        "org": {
          "name": "Моя Компания"
        }
      }
    }
  }
}
...
```