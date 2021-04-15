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
 ...      
 "vcardArray":[
   "vcard",
   [
     ["version", {}, "text", "4.0"],
     ["fn", {}, "text", "Joe User"],
     ["n", {}, "text",
       ["User", "Joe", "", "", ["ing. jr", "M.Sc."]]
     ],
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
     ["adr",
       {
         "type":"home",
         "label":"123 Maple Ave\nSuite 90001\nVancouver\nBC\n1239\n"
       },
       "text",
       [
         "", "", "", "", "", "", ""
       ]
     ],
     ["tel",
       {
         "type":["work", "voice"],
         "pref":"1"
       },
       "uri",
       "tel:+1-555-555-1234;ext=102"
     ],
     ["tel",
       { "type":["work", "cell", "voice", "video", "text"] },
       "uri",
       "tel:+1-555-555-4321"
     ],
     ["email",
       { "type":"work" },
       "text",
       "joe.user@example.com"
     ],
     ["geo", {
       "type":"work"}, 
       "uri", 
       "geo:46.772673,-71.282945"
     ],
     ["key",
       { "type":"work" },
       "uri",
       "http://www.example.com/joe.user/joe.asc"
     ],
     ["tz", 
       {},
       "utc-offset", 
       "-05:00"
     ],
     ["url", 
       { "type":"home" },
       "uri", 
       "http://example.org"
     ]
   ]
 ],
 ...      

```

```
      ...      
      "jscard":{
        "uid": "XXXX",
        "fullName": { "value": "Joe User" },
        "name": [
          { "value":"Joe", "type": "personal" },
          { "value":"User", "type": "surname" },
          { "value":"ing. jr", "type": "suffix" },
          { "value":"M.Sc.", "type": "suffix" }
        ],
        "kind": "individual",
        "preferredContactLanguages": {
          "fr": { "pref": 1 },
          "en": { "pref": 2 }
        },
        "organizations": {
          "org": {
            "name": { "value": "Example" }
          }
        },
        "titles": {
          "title-1": {
            "title": { "value": "Research Scientist" }
          },
          "title-2": {
            "title": { "value": "Project Lead" }
          }
        },
        "addresses": {
          "int": {
            "contexts": { "work": true },
            "extension": "Suite 1234",
            "street": "4321 Rue Somewhere",
            "locality": "Quebec",
            "region": "QC",
            "postcode": "G1V 2M2",
            "country": "Canada",
            "coordinates": "geo:46.772673,-71.282945",
            "timeZone": "Canada/Eastern"
          },
          "home": {
            "contexts": { "private": true },
            "fullAddress": {
              "value": "123 Maple Ave\nSuite 90001\nVancouver\nBC\n1239\n"
            }
          }
        },
        "phones": {
          "voice" : {
            "contexts": { "work": true },
            "features": { "voice": true },
            "label": "cell,video,text",
            "pref": 1,
            "phone": "tel:+1-555-555-1234;ext=102"
          }
        },
        "emails": {
          "email": {
            "contexts": { "work": true },
            "email": "joe.user@example.com"
          }
        },
        "online": {
          "key": {
            "contexts": { "work": true },
            "type": "uri",
            "label": "key",
            "resource": "http://www.example.com/joe.user/joe.asc"
          },
          "url": {
            "contexts": { "private": true },
            "type": "uri",
            "label": "url",
            "resource": "http://example.org"
          }
        }
      },
      ...      
```

### Multilingual Information

```
...
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
],
...
```

```
...
"jscard":{
  "uid": "XXXX",
  "fullName": {
    "value": "大久保 正仁",
    "language": "ja",
    "localizations": {
      "en": "Okubo Masahito"
    }
  },
  "titles": {
    "a-title": {
      "value": "事務局長",
      "language": "ja",
      "localizations": {
        "en": "Secretary General"
      }
    }
  },
  "kind": "individual",
  "preferredContactLanguages": {
    "ja": [{ "pref": 1 }],
    "en": [{ "pref": 2 }]
  }
},
...
```

### Unstructured Data

```
...
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
    [ "kind", {}, "text", "org" ],
    [ "adr",
       { "label": "8F., No.172-1, Sec.2, Ji-Lung Rd," },
       "text",
       [ "", "", "", "", "", "", "" ]
    ]
],
...
```

```
...
"jscard":{
  "uid": "XXXX",
  "fullName": {
    "value": "Taiwan Fixed Network CO.,LTD.",
    "language": "en",
    "localizations": {
      "zh-Hant-TW": "台灣固網股份有限公司"
    }
  },
  "kind": "org",
  "addresses": {
    "my-address": {
      "fullAddress": {
        "value": "8F., No.172-1, Sec.2, Ji-Lung Rd,"
      }
    }
  }
},
...
```