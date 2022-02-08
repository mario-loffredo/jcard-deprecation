# jCard Deprecation in RDAP

This project aims at deprecating jCard in RDAP.

# Table of contents
1. [Motivations](#motivations)
2. [Converting jCard into JSCard](#converting-jcard-into-jscard)


<a name="motivations"></a>
## 1. Motivations

According to the feedback from [RDAP Pilot WG](https://www.icann.org/en/system/files/files/rdap-pilot-report-25apr19-en.pdf), the most frequent concern about RDAP implementation at both server and client side is currently related to the handling of [jCard](https://tools.ietf.org/html/rfc7095). In fact, it is considered:

*	unintuitive;
*	very complicated for both consumers and servers;
*	uncompliant with REST API standards;
*	affecting performance.

Such a feeling is not limited to RDAP implementers but is also shared by most of APIs producers and consumers dealing with jCard.
[JSContact](https://datatracker.ietf.org/doc/draft-ietf-jmap-jscontact) includes a contact representation that is able to represent the same information as jCard more efficiently. In particular, it meets the requirements from RDAP implementers about representing multilingual information and unstructured data. Therefore, jCard might be deprecated and replaced by the contact card defined by JSContact in the RDAP responses.


<a name="converting-jcard-into-jscard"></a>
## 2. Converting jCard into JSCard

While the jCard element in the RDAP response is named "vcardArray", its JSCard counterpart could be called "jscard". Here in the following examples of mapping between &quot;vcardArray&quot; and &quot;jscard&quot; according to the policy defined in [JSContact: Converting from and to vCard](https://datatracker.ietf.org/doc/draft-loffredo-jmap-jscontact-vcard/) are shown.

### Example of an entity lookup response

```
{
  "rdapConformance": [
     "rdap_level_0",
     "jscard_0"
  ],
  "objectClassName" : "entity",
  "handle":"XXXX",
  "jscard":{
    "@type": "Card",
    "uid": "XXXX",
    "fullName": "Joe User" ,
    "name": {
      "components": [
        {
          "@type": "NameComponent",
          "type": "surname",
          "value": "User"
        },
        {
          "@type": "NameComponent",
          "type": "personal",
          "value": "Joe"
        },
        {
          "@type": "NameComponent",
          "type": "suffix",
          "value": "ing. jr"
        },
        {
          "@type": "NameComponent",
          "type": "suffix",
          "value": "M.Sc."
        }
      ]
    },
    "kind": "individual",
    "preferredContactLanguages": {
      "fr": {
        "@type": "ContactLanguage",
        "pref": 1
      },
      "en": {
        "@type": "ContactLanguage",
        "pref": 2
      }
    },
    "organizations": {
      "org": {
        "@type": "Organization",
        "name": "Example"
      }
    },
    "titles": {
      "title": {
        "@type": "Title",
        "title": "Research Scientist"
      },
      "role": {
        "@type": "Title",
        "title": "Project Lead"
      }
    },
    "addresses": {
      "addr": {
        "@type": "Address",
        "contexts": {
          "work": true
        },
        "street": [
          {
            "@type": "StreetComponent",
            "type": "name",
            "value": "4321 Rue Somewhere"
          },
          {
            "@type": "StreetComponent",
            "type": "extension",
            "value": "Suite 1234"
          }
        ],
        "locality": "Quebec",
        "region": "QC",
        "postcode": "G1V 2M2",
        "country": "Canada",
        "coordinates": "geo:46.772673,-71.282945",
        "timeZone": "Etc/GMT+5"
      },
      "home": {
        "@type": "Address",
        "contexts": {
          "private": true
        },
        "fullAddress": "123 Maple Ave\nSuite 90001\nVancouver\nBC\n1239\n"
      }
    },
    "phones": {
      "voice" : {
        "@type": "Phone",
        "contexts": {
          "work": true
        },
        "features": {
           "voice": true,
           "cell": true,
           "video": true,
           "text": true
        },
        "pref": 1,
        "phone": "tel:+1-555-555-1234;ext=102"
      }
    },
    "emails": {
      "email": {
        "@type": "EmailAddress",
        "contexts": {
          "work": true
        },
        "email": "joe.user@example.com"
      }
    },
    "online": {
      "key": {
        "@type" : "Resource",
        "contexts": {
          "work": true
        },
        "type": "uri",
        "label": "key",
        "resource": "http://www.example.com/joe.user/joe.asc"
      },
      "url": {
        "@type" : "Resource",
        "contexts": {
          "private": true
        },
        "type": "uri",
        "label": "url",
        "resource": "http://example.org"
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
  "remarks":[
    {
      "description":[
        "She sells sea shells down by the sea shore.",
        "Originally written by Terry Sullivan."
      ]
    }
  ],
  "links":[
    {
      "value":"http://example.com/entity/XXXX",
      "rel":"self",
      "href":"http://example.com/entity/XXXX",
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
    "jscard": {
      "@type" : "Card",
      "uid" : "7e0636f5-e48f-4a32-ab96-b57e9c07c7aa",
      "fullName" : "Vasya Pupkin",
      "organizations" : {
        "org" : {
          "@type" : "Organization",
          "name" : "My Company"
        }
      },
      "addresses" : {
        "addr" : {
          "@type" : "Address",
          "street" : [ {
            "@type" : "StreetComponent",
            "type" : "name",
            "value" : "1 Street"
          }, {
            "@type" : "StreetComponent",
            "type" : "postOfficeBox",
            "value" : "01001"
          } ],
          "locality" : "Kyiv",
          "countryCode" : "UA"
        }
      },
      "localizations" : {
        "uk" : {
          "/jscard/addresses/addr" : {
            "@type" : "Address",
            "street" : [ {
              "@type" : "StreetComponent",
              "type" : "name",
              "value" : "1, Улица"
            }, {
              "@type" : "StreetComponent",
              "type" : "postOfficeBox",
              "value" : "01001"
            } ],
            "locality" : "Киев",
            "countryCode" : "UA"
          },
          "/jscard/fullName" : "Вася Пупкин",
          "/jscard/organizations/org" : {
            "@type" : "Organization",
            "name" : "Моя Компания"
          }
        }
      }
    }
...
```