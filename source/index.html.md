---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to my API documentation demo site.

<aside class="notice">
Some clients have API schemas that will become public, others have gated API documention. Even clients that intend their API schema to be made public may not want to do so until the full review cycle is complete. This showcase represents my solution to both challenges.
</aside>


# Client

## Cloud RF-API

[Cloud RF](https://docs.cloudrf.com/) provides its customers with a dashboard SaaS and direct access to the API that underpins it. My task was to take a non-spec-compliant API schema (in YAML format) and:

- improve the endpoint and schema descriptions
- ensure the API schema complies with Open API 3.0
- make calls to the endpoints and add descriptions of the responses/payloads to the schema
- start high-level docs

As is typical with such a project, I was able to identify inconsistencies in logic such as:
- default value that lay outside of min/max value
- integer that should be a float


<aside class="notice">
Cloud RF's api creates electromagnetic and geospacial modelling to assist with planning radio transmitter and antena properties).
</aside>


### Tools Used

Parameter | Description
--------- | -----------
Pandoc | I have updated the YAML file and written pages in Markdown for a Pandoc-generated static site
Stoplight | I used Stoplight locally to assist with error identification and ensuring compliance with spec
GitHub | The main communications channel via PRs
GitBash | For version control via GitHub


#### API Schema Extract

##### Introduction
The Cloud-RF API enables users to model and test radio propagation for any radio, anywhere. Authenticate by passing your API2.0 key in the request header as `key`. Set up a [CloudRF.com](https://cloudrf.com/my-account) account to generate this key. All data is transferred as JSON. For detailed documentation, visit [docs.cloudrf.com](https://docs.cloudrf.com).

##### Base URLs:

* <a href="https://api.cloudrf.com">https://api.cloudrf.com</a>

<a href="https://cloudrf.com/terms-and-conditions/">

Terms of service</a>

Email: <a href="mailto:support@cloudrf.com">Support</a>

##### Authentication

* API Key (ApiKeyAuth)
    - Parameter Name: **key**, in: header. An APIKey passed in the request headers.

<h1 id="cloud-rf-api-create">Create</h1>

This set of endpoints allows the user to create new links, site heatmaps, routes, and networks.

###### Create a point-to-multipoint heatmap

<a id="opIdarea"></a>

> Code samples

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'key' => 'API_KEY'
}

result = RestClient.post 'https://api.cloudrf.com/area',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'key': 'API_KEY'
}

r = requests.post('https://api.cloudrf.com/area', headers = headers)

print(r.json())

```

```http
POST https://api.cloudrf.com/area HTTP/1.1
Host: api.cloudrf.com
Content-Type: application/json
Accept: application/json

```

`POST /area`

This endpoint returns an omni-directional coverage plot (point-to-multipoint) as an image, rendered with a color schema. This function assumes the same receiver height at all locations out to fixed radius (up to a 300 km maximum). Due to its recursive processing, this is the slowest of all the API calls. Speed can be improved significantly by adjusting the resolution "res" parameter upwards so 4 (m) is faster than 2 (m). A standard request requires transmitter, receiver, antenna, and output objects; providing model and environment enhances accuracy.

> Body parameter

```json
{
  "site": "A1",
  "network": "Testing",
  "transmitter": {
    "lat": 38.916,
    "lon": 1.448,
    "alt": 2,
    "frq": 868,
    "txw": 0.1,
    "bwi": 1
  },
  "receiver": {
    "lat": 0,
    "lon": 0,
    "alt": 2,
    "rxg": 3,
    "rxs": -100
  },
  "antenna": {
    "txg": 2.15,
    "txl": 1,
    "ant": 0,
    "azi": 1,
    "tlt": 10,
    "hbw": 2,
    "vbw": 2,
    "pol": "h"
  },
  "output": {
    "units": "metric",
    "col": "RAINBOW.dBm",
    "out": 2,
    "ber": 2,
    "mod": 7,
    "nf": -100,
    "res": 10,
    "rad": 5
  }
}
```

<h3 id="create-a-point-to-multipoint-heatmap-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|The request payload providing the site properties.|
|» site|body|string|false|Site name.|
|» network|body|string|false|Network name/group.|
|» transmitter|body|[Transmitter](#schematransmitter)|true|none|
|»» lat|body|number(float)|false|Latitude of transmitter in decimal degrees.|
|»» lon|body|number(float)|false|Longitude of transmitter in decimal degrees.|
|»» alt|body|number(float)|false|Altitude above ground level in meters OR feet.|
|»» frq|body|number(float)|false|Center frequency in megahertz.|
|»» txw|body|number(float)|false|Transmitter power in watts before the antenna.|
|»» bwi|body|number(float)|false|Bandwidth in MHz. 1MHz has a noise floor of -114dBm; 10MHz = -104dBm; 20MHz = -101dBm.|
|» receiver|body|[Receiver](#schemareceiver)|true|none|
|»» lat|body|number(float)|false|Latitude in decimal degrees.|
|»» lon|body|number(float)|false|Longitude in decimal degrees.|
|»» alt|body|number(float)|false|Altitude above ground level in meters OR feet.|
|»» rxg|body|number(float)|false|Receiver antenna gain in dBi.|
|»» rxs|body|number(float)|false|Receiver sensitivity/threshold in measured units defined by 'out' parameter.|
|» antenna|body|[Antenna](#schemaantenna)|true|none|
|»» txg|body|number(float)|false|Transmitter antenna gain in dBi.|
|»» txl|body|number(float)|false|Feeder loss in dB.|
|»» ant|body|integer(int32)|false|Antenna pattern code: 0=allows custom options (see: hbw; vbw; fbr) 1=Vertical dipole (omni-directional).|
|»» azi|body|number(float)|false|Antenna azimuth in degrees north.|
|»» tlt|body|number(float)|false|Antenna tilt in degrees below the horizon (inverted).|
|»» hbw|body|number(float)|false|Custom antenna horizontal beamwidth in degrees; for use only with ant=0.|
|»» vbw|body|number(float)|false|Custom antenna vertical beamwidth in degrees; for use only with ant=0.|
|»» pol|body|string|false|Antenna polarization.|
|» model|body|[Model](#schemamodel)|false|none|
|»» pm|body|integer(int32)|false|Propagation model: 1=Irregular Terrain Model; 2=Line of Sight (LOS); 3=Hata; 4=ECC33; 5=SUI Microwave; 6=COST231; 7=Free space path loss; 9=Ericsson9999; 10=Plane earth loss; 11=Egli.|
|»» pe|body|integer(int32)|false|Propagation model subtype/environment: 1=Conservative/Urban; 2=Average/Suburban; 3=Optimistic/rural.|
|»» cli|body|integer(int32)|false|Radio climate for ITM model: (1). 1=Equatorial (Congo); 2=Continental Subtropical (Sudan); 3=Maritime Subtropical (West coast of Africa); 4=Desert (Sahara); 5=Continental Temperate; 6=Maritime Temperate and Over Land (UK and West coasts of US & EU); 7=Maritime Temperate, Over Sea.|
|»» ked|body|number|false|Include knife edge diffraction for enhancing basic empirical models (already in ITM). Switch on/off; 1=INCLUDE, 0=OFF, i.e., if diffraction is OFF (0) then the model applies more conservative 'line of sight'.|
|»» rel|body|number(float)|false|ITM model required reliability as %|
|»» ter|body|integer(int32)|false|Terrain code for ITM model (1): 1=Water; 2=Wet ground; 3=Farmland; 4=Forest/Average; 5=Mountain/Sand; 6=City/Poor ground.|
|» environment|body|[Environment](#schemaenvironment)|false|none|
|»» clm|body|integer(int32)|false|Clutter mode: 0=None/DSM only; 1=System & custom clutter; 2=Custom clutter only.|
|»» cll|body|integer(int32)|false|Clutter loss: 0=None/DSM only; 1=Hard/LOS mode; 2=Soft/NLOS mode.|
|»» mat|body|number(float)|false|Clutter attenuation override in dB/m based on a tree block OR hollow building; light foliage=0.1dB/m; brick=1.0dB/m; concrete=5dB/m.|
|» output|body|[Output](#schemaoutput)|true|none|
|»» units|body|string|false|Define distance units applied; either meters/kilometers (metric) or feet/miles (imperial).|
|»» col|body|string|false|Color schema code OR filename string. Codes: 1=Cellular (5); 2=Red; 3=Green; 4=Blue; 5=Microwave(7); 7=Custom RGB; 8=Automatic by frequency; 9=Greyscale/GIS; 10=Rainbow(24); 11=Green/Blue/Red; 13=Sub noise floor (10); 14=TV broadcasting (4); 15=Red threshold; 16=Green threshold; 17=Blue threshold. Filename examples: RAINBOW.dBm; CUSTOMSCHEMA.dBm.|
|»» out|body|integer(int32)|false|Measured units: 1=dB; 2=dBm; 3=dBuV; 4=SNR (4 supports nf option).|
|»» ber|body|integer(int32)|false|Bit error rate: 1=0.1; 2=0.01; 3=0.001; 4=0.0001; 5=0.00001; 6=0.000001; >6=Lora; 7=SF7; 8=SF8; 9=SF9; 10=SF10; 11=SF11; 12=SF12.|
|»» mod|body|integer(int32)|false|Modulation: 1=4QAM; 2=16QAM; 3=64QAM; 4=256QAM; 5=1024QAM; 6=BPSK; 7=QPSK; 8=8PSK; 9=16PSK; 10=32PSK; 11=LoRa.|
|»» nf|body|number(float)|false|Noise floor in dBm for use with out=4/SNR.|
|»» res|body|number(float)|false|Resolution in meters for output.|
|»» rad|body|number(float)|false|Radius in kilometers for output.|

#### Enumerated Values

|Parameter|Value|
|---|---|
|»» ant|0|
|»» ant|1|
|»» pol|h|
|»» pol|v|
|»» ked|0|
|»» ked|1|
|»» units|metric|
|»» units|imperial|

> Example responses

> 200 Response

```json
{
  "kmz": "https://api.cloudrf.com/archive/export?sid=YnJETGFWbjZsNDBlNVRQY1phUFFsdz09&fmt=kmz",
  "PNG_WGS84": "https://api.cloudrf.com/users/40843/0722102238_Testing_A1.4326.png",
  "PNG_Mercator": "https://api.cloudrf.com/users/40843/0722102238_Testing_A1.3857.png",
  "bounds": [
    [
      38.97373,
      1.505727,
      38.85827,
      1.390273
    ]
  ],
  "id": "3930916",
  "sid": "M2ZSaUZuVlVDRno3TTJDTGVqRGJlUT09",
  "area": "0 km2",
  "coverage": "0%",
  "key": [
    [
      {
        "l": "-20dBm",
        "r": 255,
        "g": 19,
        "b": 0
      },
      {
        "l": "-24dBm",
        "r": 255,
        "g": 61,
        "b": 0
      },
      {
        "l": "-28dBm",
        "r": 255,
        "g": 102,
        "b": 0
      },
      {
        "l": "-32dBm",
        "r": 255,
        "g": 144,
        "b": 0
      },
      {
        "l": "-36dBm",
        "r": 255,
        "g": 185,
        "b": 0
      },
      {
        "l": "-40dBm",
        "r": 255,
        "g": 227,
        "b": 0
      },
      {
        "l": "-44dBm",
        "r": 242,
        "g": 255,
        "b": 0
      },
      {
        "l": "-48dBm",
        "r": 200,
        "g": 255,
        "b": 0
      },
      {
        "l": "-52dBm",
        "r": 159,
        "g": 255,
        "b": 0
      },
      {
        "l": "-56dBm",
        "r": 117,
        "g": 255,
        "b": 0
      },
      {
        "l": "-60dBm",
        "r": 76,
        "g": 255,
        "b": 0
      },
      {
        "l": "-64dBm",
        "r": 34,
        "g": 255,
        "b": 0
      },
      {
        "l": "-68dBm",
        "r": 0,
        "g": 255,
        "b": 7
      },
      {
        "l": "-72dBm",
        "r": 0,
        "g": 255,
        "b": 49
      },
      {
        "l": "-76dBm",
        "r": 0,
        "g": 255,
        "b": 90
      },
      {
        "l": "-80dBm",
        "r": 0,
        "g": 255,
        "b": 132
      },
      {
        "l": "-84dBm",
        "r": 0,
        "g": 255,
        "b": 173
      },
      {
        "l": "-88dBm",
        "r": 0,
        "g": 255,
        "b": 215
      },
      {
        "l": "-92dBm",
        "r": 0,
        "g": 254,
        "b": 255
      },
      {
        "l": "-96dBm",
        "r": 0,
        "g": 212,
        "b": 255
      },
      {
        "l": "-100dBm",
        "r": 0,
        "g": 171,
        "b": 255
      },
      {
        "l": "-104dBm",
        "r": 0,
        "g": 129,
        "b": 255
      },
      {
        "l": "-108dBm",
        "r": 0,
        "g": 88,
        "b": 255
      },
      {
        "l": "-112dBm",
        "r": 0,
        "g": 46,
        "b": 255
      }
    ]
  ],
  "elapsed": 3572,
  "balance": 1
}
```


## SMTP2GO

SMTP2GO provides their customers with a dashboard to manage their email campaigns. They also allow users to interact directly with the API that underpins it.

My task was to take a non-spec-compliant API schema (in YAML format) and:

- improve the endpoint and schema descriptions
- ensure the API schema complies with Open API 3.0
- provide high-level docs

As is typical with such a project, I was able to identify inconsistencies in logic such as:
- strings that should be datetime
- boolean with no type definition to let user know whether to pass 0/1 or True/False


### Tools Used

Parameter | Description
--------- | -----------
Redocly | Consumed the YAML and added Markdown pages for high-level overview material using [Redoc.ly](https://redoc.ly/)
Stoplight | I used Stoplight locally to assist with error identification and ensuring compliance with spec
Slack | The main communications channel to chat to the dev assigned to answer all my questions
GitBash | For version control via GitHub
Asana | For task management


#### API Schema Extract

##### Quick Start

Get started with SMTP2GO's simple REST API.

<aside class="notice">

These docs support users who wish to integrate with the API. Should you wish to use SMTP, [that is also supported](https://www.smtp2go.com/setup/).

</aside>

#### Prerequisites

To make an API call to SMTP2GO, first:

- [create an account](https://www.smtp2go.com/pricing/)
- [generate a unique API Key](#authenticate)

##### Authenticate

Once you have registered for an account, log in to your [control panel](https://app.smtp2go.com/login/).

<aside class="notice">
Generate an API Key from:

> Control Panel > Settings > API Keys

</aside>

Your API key must be provided in the `X-Smtp2go-Api-Key` header **and** in the body `api_key`.

If you are new to using API keys, please consider [API key security](#API-key-security).

#### Choose your Region

Now that you are ready to authenticate, you can:

- [choose your region](#base-urls)
- pass the API key in the request headers and in the body

Your options include:

  - Regionless
  - US Region
  - EU Region

Your choice of region determines the URL to which you make your API call. For example, to make a request to add senders to your allowed list using the US region, use the path:


```
https://us-api.smtp2go.com/v3/allowed_senders/add

```


## SMTP2GO Public API

SMTP2GO Public API Schema v3.0. Integrate with SMTP2GO Systems.

<aside class="notice">

Scroll down for code samples, example requests, and responses.

</aside>

### Base URLs:

* <a href="https://api.smtp2go.com/v3">https://api.smtp2go.com/v3</a>

* <a href="https://us-api.smtp2go.com/v3">https://us-api.smtp2go.com/v3</a>

* <a href="https://eu-api.smtp2go.com/v3">https://eu-api.smtp2go.com/v3</a>

Email: <a href="mailto:ticket@smtp2go.com">SMTP2GO Support</a> Web: <a href="https://support.smtp2go.com">SMTP2GO Support</a>
License: <a href="https://support.smtp2go.com/">Apache 2.0</a>

### Authentication

* API Key (APIKeyHeader)
    - Parameter Name: **X-Smtp2go-Api-Key**, in: header and in the body `api_key`.

<h2 id="smtp2go-public-api-v3-0-0-activity">Activity</h2>

A POST command allows you to search the activity stream. Select individual field/s to search, such as `start_date`, or utilise the `search` field to search across all fields.

## postActivitySearch

<a id="opIdpostActivitySearch"></a>

> Code samples

```shell
# You can also use wget
curl -X POST https://api.smtp2go.com/v3/activity/search \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'X-Smtp2go-Api-Key: API_KEY'

```

```http
POST https://api.smtp2go.com/v3/activity/search HTTP/1.1
Host: api.smtp2go.com
Content-Type: application/json
Accept: application/json

```

```javascript
const inputBody = '{
  "api_key": "api-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "start_date": "2021-04-20T00:00:00",
  "end_date": "2021-04-21T00:00:00",
  "search": "string",
  "search_subject": "string",
  "search_sender": "string",
  "search_recipient": "string",
  "search_usernames": "string",
  "limit": 0,
  "continue_token": "string",
  "only_latest": false,
  "event_types": [
    "processed"
  ]
}';
const headers = {
  'Content-Type':'application/json',
  'Accept':'application/json',
  'X-Smtp2go-Api-Key':'API_KEY'
};

fetch('https://api.smtp2go.com/v3/activity/search',
{
  method: 'POST',
  body: inputBody,
  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Content-Type' => 'application/json',
  'Accept' => 'application/json',
  'X-Smtp2go-Api-Key' => 'API_KEY'
}

result = RestClient.post 'https://api.smtp2go.com/v3/activity/search',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'X-Smtp2go-Api-Key': 'API_KEY'
}

r = requests.post('https://api.smtp2go.com/v3/activity/search', headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Content-Type' => 'application/json',
    'Accept' => 'application/json',
    'X-Smtp2go-Api-Key' => 'API_KEY',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('POST','https://api.smtp2go.com/v3/activity/search', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.smtp2go.com/v3/activity/search");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Content-Type": []string{"application/json"},
        "Accept": []string{"application/json"},
        "X-Smtp2go-Api-Key": []string{"API_KEY"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("POST", "https://api.smtp2go.com/v3/activity/search", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

## API key security

It is important to keep your API key secure. Publicly exposing your key can compromise your account, which could result in unexpected charges. To keep your API keys secure, follow these best practices:

- Store API keys as environment variables

<aside class="warning">
Do not embed API keys directly in code. API keys that are embedded in code can be accidentally exposed outside your circle of trust.
</aside>

- If you store API keys in files, store those files outside your application's source tree.

<aside class="notice">
This helps to ensure that your keys do not end up in your source code control system. This is particularly important if you use a public source code management system such as GitHub.
</aside>

