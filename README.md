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

While the jCard element in the RDAP response is named "vcardArray", its JSCard counterpart could be called "jscard". Here in the following three examples of mapping between &quot;vcardArray&quot; and &quot;jscard&quot; according to the policy defined in [JSContact: Converting from and to vCard](https://datatracker.ietf.org/doc/draft-loffredo-jmap-jscontact-vcard/) are shown.

### Use case taken from RFC7483

```
"vcardArray":[
  "vcard",
  [
    ["version", {}, "text", "4.0"],
    ["fn", {}, "text", "Joe User"],
    ["kind", {}, "text", "individual"],
    ["lang", {
      "pref":"1"
    }, "language-tag", "fr"],
    ["lang", {
      "pref":"2"
    }, "language-tag", "en"],
    ["org", {
       "type":"work"
    }, "text", "Example"],
    ["title", {}, "text", "Research Scientist"],
    ["role", {}, "text", "Project Lead"],
    ["adr",
      { "type":"work" },
      "text",
      [
        "",
        "Suite 1234",
        "4321 Rue Somewhere",
        "Quebec",
        "QC",
        "G1V 2M2",
        "Canada"
      ]
    ],
    ["tel",
      { "type":["work", "voice"], "pref":"1" },
      "uri", "tel:+1-555-555-1234;ext=102"
    ],
    ["email",
      { "type":"work" },
      "text", "joe.user@example.com"
    ]
  ]
]

"jscard":{
  "uid": <handle value>,
  "fullName": { "value": "Joe User" },
  "kind": "individual",
  "preferredContactLanguages": {
    "fr": { "preference": 1 },
    "en": { "preference": 2 }
  },
  "organization": [ { "value": "Example" } ],
  "jobTitle": [ { "value": "Research Scientist" } ],
  "role": [ { "value": "Project Lead" } ],
  "addresses": [
                 {
                   "context": "work",
                   "extension": "Suite 1234",
                   "street": "4321 Rue Somewhere",
                   "locality": "Quebec",
                   "region": "QC",
                   "postcode": "G1V 2M2",
                   "country": "Canada"
                 }
  ],
  "phones": [
              { "type": "voice",
                "context": "work",
                "isPreferred": true,
                "value": "tel:+1-555-555-1234;ext=102"
              }
  ],
  "emails": [
              {
                "context": "work",
                "value": "joe.user@example.com"
              }
  ]
 }
```

### Multilingual Information

```
"vcardArray": [
  "vcard",
  [
    [ "version", {}, "text", "4.0" ],
    [ "fn",
      { "language": "ja", "altid": "1" },
      "text",
      "大久保 正仁"
    ],
    [ "fn",
      { "language": "en", "altid": "1" },
      "text",
      "Okubo Masahito"
    ],
    [ "title",
      { "language": "ja", "altid": "1" },
      "text",
      "事務局長"
    ],
    [ "title",
      { "language": "en", "altid": "1" },
      "text",
      "Secretary General"
    ],
    [ "kind", {}, "text", "individual" ],
    [ "lang", { "pref": "1" }, "language-tag", "ja" ],
    [ "lang", { "pref": "2" }, "language-tag", "en" ]
  ]
]

"jscard":{
  "uid": <handle value>,
  "fullName": {
    "value": "大久保 正仁",
    "language": "ja",
    "localizations": {
      "en": "Okubo Masahito"
    }
  },
  "jobTitle": [
                {
                  "value": "事務局長",
                  "language": "ja",
                  "localizations": {
                    "en": "Secretary General"
                  }
                }
  ],
  "kind": "individual",
  "preferredContactLanguages": {
     "ja": { "preference": 1 },
     "en": { "preference": 2 }
  }
}
```

### Unstructured Data

```
"vcardArray": [
  "vcard",
  [
    [ "version", {}, "text", "4.0" ],
    [ "fn",
       { "altid": "1", "language": "zh-Hant-TW" },
       "text",
       "台灣固網股份有限公司"
    ],
    [ "fn",
       { "altid": "1", "language": "en" },
       "text",
       "Taiwan Fixed Network CO.,LTD."
    ],
    [ "kind", {}, "text", "individual" ],
    [ "adr",
       { "label": "8F., No.172-1, Sec.2, Ji-Lung Rd," },
       "text",
       [ "", "", "", "", "", "", "" ]
    ],
    [ "tel", { "type": "voice" }, "text", "" ],
    [ "tel", { "type": "fax" }, "text", "" ],
    [ "email", {}, "text", "" ]
  ]
]

"jscard":{
  "uid": <handle value>,
  "fullName": {
    "value": "Taiwan Fixed Network CO.,LTD.",
    "language": "en",
    "localizations": {
      "zh-Hant-TW": "台灣固網股份有限公司"
    }
  },
  "kind": "individual",
  "addresses": [
                 {
                   "fullAddress": {
                    "value": "8F., No.172-1, Sec.2, Ji-Lung Rd,"
                   }
                 }
  ],
  "phones": [
              {
                "type": "voice",
                "value": ""
              },
              {
                "type": "fax",
                "value": ""
              }
  ],
  "emails": [
              {
                "value": ""
              }
  ]
}
```