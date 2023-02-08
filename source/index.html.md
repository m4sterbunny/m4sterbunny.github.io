---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://www.fiverr.com/harrieb'>Fiver Pro Profile</a>
  - <a href='https://www.upwork.com/freelancers/~0111e0b1f1b6c5d8d4'>Upwork Profile</a>

headingLevel: 3

search: true

code_clipboard: true
---

# API Showcase

Welcome to my API documentation demo site.

<aside class="notice">
Some clients have API schemas that will become public, others have gated API documentation. Even clients that intend their API schema to be made public may not want to do so until the full review cycle is complete. This showcase represents my solution to such challenges.
</aside>

Note that different styles may be applied to the written English according to client preferences. Expect:

- UK *and* US English
- A stop to end descriptions.
- No stop
- Oxford comma/none


# Showcase 1: Cloud RF

####  About Cloud RF

[Cloud RF](https://docs.cloudrf.com/) provides its customers with a dashboard SaaS and direct access to the API that underpins it. My task was to take a non-spec-compliant API schema (in YAML format) and:

- improve the endpoint and schema descriptions
- ensure the API schema complies with Open API 3.0
- make calls to the endpoints and add descriptions of the responses/payloads to the schema
- start high-level docs

As is typical with such a project, I was able to identify inconsistencies in logic such as:

- default values that lay outside of min/max values
- integer that should be a float


<aside class="notice">
Cloud RF's API creates electromagnetic and geospacial modelling to assist with planning radio transmitter and antenna properties.
</aside>


### Tools Used

Parameter | Description
--------- | -----------
Pandoc | I updated the YAML file and written pages in Markdown for a Pandoc-generated static site
Stoplight | I used Stoplight locally to assist with error identification and ensuring compliance with spec
GitHub | The main communications channel via PRs
GitBash | For version control via GitHub


### API Schema Extract

### Introduction
The Cloud-RF API enables users to model and test radio propagation for any radio, anywhere. Authenticate by passing your API2.0 key in the request header as `key`. Set up a [CloudRF.com](https://cloudrf.com/my-account) account to generate this key. All data is transferred as JSON. For detailed documentation, visit [docs.cloudrf.com](https://docs.cloudrf.com).

### Base URLs:

* <a href="https://api.cloudrf.com">https://api.cloudrf.com</a>

<a href="https://cloudrf.com/terms-and-conditions/">

Terms of service</a>

Email: <a href="mailto:support@cloudrf.com">Support</a>

### Authentication

An APIKey passed in the request headers:

* ApiKeyAuth
    - Pass parameter name: **key**, in: header.

#### Endpoint Example

Create a point-to-multipoint heatmap

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

#### Cloud RF API
POST https://api.cloudrf.com/area
Host: api.cloudrf.com
Content-Type: application/json


`POST /area`

This endpoint returns an omni-directional coverage plot (point-to-multipoint) as an image, rendered with a color schema. This function assumes the same receiver height at all locations out to fixed radius (up to a 300 km maximum). Due to its recursive processing, this is the slowest of all the API calls. Speed can be improved significantly by adjusting the resolution "res" parameter upwards so 4 (m) is faster than 2 (m). A standard request requires transmitter, receiver, antenna, and output objects; providing model and environment enhances accuracy.

> Example Body Parameter

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
<h4 id="tocS_Transmitter">Transmitter</h4>

<a id="schematransmitter"></a>
<a id="schema_Transmitter"></a>
<a id="tocStransmitter"></a>
<a id="tocstransmitter"></a>

```json
{
  "lat": 38.916,
  "lon": 1.448,
  "alt": 1,
  "frq": 868,
  "txw": 0.1,
  "bwi": 1
}

```

#### Transmitter Object

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|lat|number(float)|false|none|Latitude of transmitter in decimal degrees.|
|lon|number(float)|false|none|Longitude of transmitter in decimal degrees.|
|alt|number(float)|false|none|Altitude above ground level in meters OR feet.|
|frq|number(float)|false|none|Center frequency in megahertz.|
|txw|number(float)|false|none|Transmitter power in watts before the antenna.|
|bwi|number(float)|false|none|Bandwidth in MHz. 1MHz has a noise floor of -114dBm; 10MHz = -104dBm; 20MHz = -101dBm.|

<h4 id="tocS_Antenna">Antenna</h4>

<a id="schemaantenna"></a>
<a id="schema_Antenna"></a>
<a id="tocSantenna"></a>
<a id="tocsantenna"></a>

```json
{
  "txg": 2.15,
  "txl": 1,
  "ant": 0,
  "azi": 1,
  "tlt": 10,
  "hbw": 2,
  "vbw": 2,
  "pol": "h"
}

```

#### Antenna Object

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|txg|number(float)|false|none|Transmitter antenna gain in dBi.|
|txl|number(float)|false|none|Feeder loss in dB.|
|ant|integer(int32)|false|none|Antenna pattern code: 0=allows custom options (see: hbw; vbw; fbr) 1=Vertical dipole (omni-directional).|
|azi|number(float)|false|none|Antenna azimuth in degrees north.|
|tlt|number(float)|false|none|Antenna tilt in degrees below the horizon (inverted).|
|hbw|number(float)|false|none|Custom antenna horizontal beamwidth in degrees; for use only with ant=0.|
|vbw|number(float)|false|none|Custom antenna vertical beamwidth in degrees; for use only with ant=0.|
|pol|string|false|none|Antenna polarization.|


<h4 id="tocS_Receiver">Receiver</h4>

<a id="schemareceiver"></a>
<a id="schema_Receiver"></a>
<a id="tocSreceiver"></a>
<a id="tocsreceiver"></a>

```json
{
  "lat": 38.906986,
  "lon": 1.421416,
  "alt": 0.1,
  "rxg": 3,
  "rxs": -100
}

```

#### Receiver Object

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|lat|number(float)|false|none|Latitude in decimal degrees.|
|lon|number(float)|false|none|Longitude in decimal degrees.|
|alt|number(float)|false|none|Altitude above ground level in meters OR feet.|
|rxg|number(float)|false|none|Receiver antenna gain in dBi.|
|rxs|number(float)|false|none|Receiver sensitivity/threshold in measured units defined by 'out' parameter.|

<h4 id="tocS_Model">Model</h4>

<a id="schemamodel"></a>
<a id="schema_Model"></a>
<a id="tocSmodel"></a>
<a id="tocsmodel"></a>

```json
{
  "pm": 1,
  "pe": 2,
  "cli": 7,
  "ked": 1,
  "rel": 55,
  "ter": 3
}

```

#### Model Object

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|pm|integer(int32)|false|none|Propagation model: 1=Irregular Terrain Model; 2=Line of Sight (LOS); 3=Hata; 4=ECC33; 5=SUI Microwave; 6=COST231; 7=Free space path loss; 9=Ericsson9999; 10=Plane earth loss; 11=Egli.|
|pe|integer(int32)|false|none|Propagation model subtype/environment: 1=Conservative/Urban; 2=Average/Suburban; 3=Optimistic/rural.|
|cli|integer(int32)|false|none|Radio climate for ITM model: (1). 1=Equatorial (Congo); 2=Continental Subtropical (Sudan); 3=Maritime Subtropical (West coast of Africa); 4=Desert (Sahara); 5=Continental Temperate; 6=Maritime Temperate and Over Land (UK and West coasts of US & EU); 7=Maritime Temperate, Over Sea.|
|ked|number|false|none|Include knife edge diffraction for enhancing basic empirical models (already in ITM). Switch on/off; 1=INCLUDE, 0=OFF, i.e., if diffraction is OFF (0) then the model applies more conservative 'line of sight'.|
|rel|number(float)|false|none|ITM model required reliability as %|
|ter|integer(int32)|false|none|Terrain code for ITM model (1): 1=Water; 2=Wet ground; 3=Farmland; 4=Forest/Average; 5=Mountain/Sand; 6=City/Poor ground.|


<h4 id="tocS_Environment">Environment</h4>

<a id="schemaenvironment"></a>
<a id="schema_Environment"></a>
<a id="tocSenvironment"></a>
<a id="tocsenvironment"></a>

```json
{
  "clm": 1,
  "cll": 1,
  "mat": 1
}

```

#### Environment Object

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|clm|integer(int32)|false|none|Clutter mode: 0=None/DSM only; 1=System & custom clutter; 2=Custom clutter only.|
|cll|integer(int32)|false|none|Clutter loss: 0=None/DSM only; 1=Hard/LOS mode; 2=Soft/NLOS mode.|
|mat|number(float)|false|none|Clutter attenuation override in dB/m based on a tree block OR hollow building; light foliage=0.1dB/m; brick=1.0dB/m; concrete=5dB/m.|

<h4 id="tocS_Output">Output</h4>

<a id="schemaoutput"></a>
<a id="schema_Output"></a>
<a id="tocSoutput"></a>
<a id="tocsoutput"></a>

```json
{
  "units": "metric",
  "col": "RAINBOW.dBm",
  "out": 2,
  "ber": 2,
  "mod": 7,
  "nf": -100,
  "res": 10,
  "rad": 5
}

```

#### Output Object

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|units|string|false|none|Define distance units applied; either meters/kilometers (metric) or feet/miles (imperial).|
|col|string|false|none|Color schema code OR filename string. Codes: 1=Cellular (5); 2=Red; 3=Green; 4=Blue; 5=Microwave(7); 7=Custom RGB; 8=Automatic by frequency; 9=Greyscale/GIS; 10=Rainbow(24); 11=Green/Blue/Red; 13=Sub noise floor (10); 14=TV broadcasting (4); 15=Red threshold; 16=Green threshold; 17=Blue threshold. Filename examples: RAINBOW.dBm; CUSTOMSCHEMA.dBm.|
|out|integer(int32)|false|none|Measured units: 1=dB; 2=dBm; 3=dBuV; 4=SNR (4 supports nf option).|
|ber|integer(int32)|false|none|Bit error rate: 1=0.1; 2=0.01; 3=0.001; 4=0.0001; 5=0.00001; 6=0.000001; >6=Lora; 7=SF7; 8=SF8; 9=SF9; 10=SF10; 11=SF11; 12=SF12.|
|mod|integer(int32)|false|none|Modulation: 1=4QAM; 2=16QAM; 3=64QAM; 4=256QAM; 5=1024QAM; 6=BPSK; 7=QPSK; 8=8PSK; 9=16PSK; 10=32PSK; 11=LoRa.|
|nf|number(float)|false|none|Noise floor in dBm for use with out=4/SNR.|
|res|number(float)|false|none|Resolution in meters for output.|
|rad|number(float)|false|none|Radius in kilometers for output.|


# Showcase 2: SMTP2GO

### About SMTP2GO

SMTP2GO provides their customers with a dashboard to manage their email campaigns. They also allow users to interact directly with the API that underpins it.

My task was to take a non-spec-compliant API schema (in YAML format) and:

- improve the endpoint and schema descriptions
- ensure the API schema complies with Open API 3.0
- provide high-level docs

As is typical with such a project, I was able to identify inconsistencies in logic such as:
- strings that should be datetime
- boolean with no example to let user know whether to pass 0/1 or True/False
- allOf that should be anyOf


### Tools Used

Parameter | Description
--------- | -----------
Slate | Cleaned up the YAML, converted it to JSON, used Widdershins to convert the schema to Markdown and added Markdown pages for high-level overview material in Slate
Stoplight | I used Stoplight locally to assist with error identification and ensuring compliance with spec
Slack | The main communications channel to chat to the developer assigned to answer all my questions
GitBash | For version control via GitHub
Asana | For task management


### API Overview Extract

### Quick Start

Get started with SMTP2GO's simple REST API.

These docs support users who wish to integrate with the API. Should you wish to use SMTP, [that is also supported](https://www.smtp2go.com/setup/).

#### Prerequisites

To make an API call to SMTP2GO, first:

- [create an account](https://www.smtp2go.com/pricing/)
- [generate a unique API Key](#authenticate)

#### Authenticate

Once you have registered for an account, log in to your [control panel](https://app.smtp2go.com/login/).

<aside class="notice">
Generate an API Key from:

> Control Panel > Settings > API Keys

</aside>

Your API key must be provided in the `X-Smtp2go-Api-Key` header **and** in the body `api_key`.

If you are new to using API keys, please consider [API key security](#api-key-security).

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

### API Schema Extract

#### SMTP2GO Public API

SMTP2GO Public API Schema v3.0. Integrate with SMTP2GO Systems.

<aside class="notice">

Scroll down for code samples, example requests, and responses.

</aside>

#### Base URLs:

* <a href="https://api.smtp2go.com/v3">https://api.smtp2go.com/v3</a>

* <a href="https://us-api.smtp2go.com/v3">https://us-api.smtp2go.com/v3</a>

* <a href="https://eu-api.smtp2go.com/v3">https://eu-api.smtp2go.com/v3</a>

Email: <a href="mailto:ticket@smtp2go.com">SMTP2GO Support</a> Web: <a href="https://support.smtp2go.com">SMTP2GO Support</a>
License: <a href="https://support.smtp2go.com/">Apache 2.0</a>

### Authentication

* API Key (APIKeyHeader)
    - Parameter Name: **X-Smtp2go-Api-Key**, in: header and in the body `api_key`.

<h4 id="smtp2go-public-api-v3-0-0-activity">Activity</h4>

A POST command allows you to search the activity stream. Select individual field/s to search, such as `start_date`, or utilise the `search` field to search across all fields.

#### postActivitySearch

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


`POST /activity/search`

*Search events*

This endpoint searches the activity stream for events.

> Body parameter

```json
{
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
}
```

<h3 id="postactivitysearch-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|object|false|The request payload carrying the search criteria|
|» api_key|body|APIKeyField|true|API Key from [SMTP2GO App](https://app.smtp2go.com/login/) > Settings > API Keys|
|» start_date|body|string|false|ISO-8601-formatted datetime, which defaults to the current date at midnight (the range is inclusive of start_date and exclusive of end_date).|
|» end_date|body|string|false|ISO-8601-formatted datetime, which defaults to now|
|» search|body|string|false|A string used to search across all fields|
|» search_subject|body|string|false|A string used to search the subject field|
|» search_sender|body|string|false|A string used to search the sender field|
|» search_recipient|body|string|false|A string used to search the recipients field|
|» search_usernames|body|string|false|A string used to search the username field|
|» limit|body|integer|false|An integer to restrict the number of events returned per page|
|» continue_token|body|string|false|A string containing a token used to fetch the next page of results based on the current search|
|» only_latest|body|boolean|false|Pass true here to display only the latest event for each email|
|» event_types|body|any|false|none|
|»» *anonymous*|body|[string]|false|none|
|»» *anonymous*|body|any|false|A list of event types to filter on|


### API key security

It is important to keep your API key secure. Publicly exposing your key can compromise your account, which could result in unexpected charges. To keep your API keys secure, follow these best practices:

- Store API keys as environment variables

<aside class="warning">
Do not embed API keys directly in code. API keys that are embedded in code can be accidentally exposed outside your circle of trust.
</aside>

- If you store API keys in files, store those files outside your application's source tree.

<aside class="notice">
This helps to ensure that your keys do not end up in your source code control system. This is particularly important if you use a public source code management system such as GitHub.
</aside>

# Showcase 3: Faria

### About Faria

[Faria](https://www.faria.org/) provides its customers with a dashboard SaaS to manage vital aspects of School Administration from applications through to lessons and class management. Faria's many products are supported by unique API sets.

My task was to:

- identify a platform that could support multi-level user access including an SSO
- assist with implementing the platform
- advocate for a "docs as code" philosophy
- create high-level user documentation based on often out-of-date, and conflicting PDF guides
- improve the endpoint and schema descriptions for 5 APIs that support various dashboards
- ensure the API schemas comply with Open API 3.0


<aside class="notice">
Faria's project was particularly challenging due to the access control requirements. Some APIs are going to be available to clients, while others remain private to Faria developers. Faria's CTO wanted a single portal to manage this control pattern.

Also, the API team were not familiar with exposing their API documentation outside of their local teams.
</aside>


### Tools Used

Parameter | Description
--------- | -----------
Redocly | I cleaned the API YAML files and provided them to the Redocly build for consumption. I also populated several high-level overview pages for partners, developers, and external clients; written in Markdown
Rswag | I identified an automated tool that allowed the dev teams to extract a Swagger-compliant YAML directly from their Ruby code-base
Spectral | I proposed that Spectral be setup with webhooks to automatically verify adherence to spec as a new YAML file is generated
GitHub | I joined the Faria.org GitHub organisation to enable me to have access to the various repos including the documentation site
GitBash | For version control via GitHub


### API Schema Extract

This API set Orchestrates Faria Services Manager and School Administration with the ManageBac GUI.

Base URL:

* <a href="https://api.devel.managebac.com">https://api.devel.managebac.com</a>

Email: <a href="mailto:hello@managebac.com">Faria International School</a> Web: <a href="https://www.faria.org/support">Faria International School</a>

#### Authentication

* API Key (authTokenQuery)
    - Parameter Name: **auth_token**, in: query. Your V2 API authentication token may be passed in the parameters.

* API Key (authTokenHeader)
    - Parameter Name: **auth-token**, in: header. Your V2 API authentication token may be passed in the header.

To protect your key, use [environment variables](#api-key-security).


#### Get all Year Groups

<a id="opIdlistYearGroups"></a>

> Code samples

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json'
}

result = RestClient.get 'https://api.devel.managebac.com/v2/year-groups',
  params: {
  }, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json'
}

r = requests.get('https://api.devel.managebac.com/v2/year-groups', headers = headers)

print(r.json())

```

```http
GET https://api.devel.managebac.com/v2/year-groups HTTP/1.1
Host: api.devel.managebac.com
Accept: application/json

```

`GET /v2/year-groups`

This endpoint retrieves the basic information of all Year Groups. It returns the group name, program, grade level and student IDs for members.<b> Note:</b> <code>/v2/ib-groups</code> endpoints have been deprecated in favor of <code>/v2/year-groups</code> endpoints and will be removed completely by the end of 2021.

<h3 id="get-all-year-groups-parameters"> Year Group Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|modified_since|query|string|false|A timestamp to filter results by modification date.|
|page|query|string|false|An integer defining which page to display.|
|per_page|query|string|false|An integer defining the number of records to display per page.|

> Example responses

> 200 Response

```json
{
  "year_groups": [
    {
      "id": 10907398,
      "name": "IB Diploma Group of 2019 (Grade 12)",
      "program": "IB Diploma",
      "grade": "Grade 12",
      "grade_number": 13,
      "student_ids": [
        10193652
      ]
    },
    {
      "id": 11076103,
      "name": "IB MYP Group of 2019 (Year 4)",
      "program": "IB Middle Years",
      "grade": "Year 4",
      "grade_number": 10,
      "student_ids": [
        12459565,
        11456849,
        12068809
      ]
    }
  ],
  "meta": {
    "current_page": 1,
    "total_pages": 1,
    "total_count": 2,
    "per_page": 100
  }
}
```

Within the Faria team, I was the most experienced in terms of API documentation. This meant not only did I have to identify the tooling for the developers to use, I also had to advocate for best practice. I supported the various development teams (there are different teams working on each Faria product/API):

  - By writing up how the first individual implemented a new tool and sharing code samples (e.g. for Rswag and Spectral)
  - By advocating for CICD practices; now when developers make upstream changes to the API a build is triggered downstream to update the API schema on the documentation site
  - By becoming the in-house "expert" on Redoc.ly (there is a limit to this, I am not a developer! I can, however, customize the build).

# Showcase 4: Qredo

### About Qredo

Qredo is a startup in the Crypto wallet space. As documentation lead I was responsible for the [developer portal](https://developers.qredo.com/): its information heirachy and content.

In terms of the API specifications, my task was to take non-spec-compliant API schemas (in YAML format) and:

- improve the endpoint and schema descriptions
- ensure the API schema complies with Open API 3.0
- provide high-level docs

As is typical with such a project, I was able to identify inconsistencies in logic such as:

- JSON payloads that did not correlate with the service
- boolean with no example to let user know whether to pass 0/1 or True/False
- flat schema structures that could be improved using oneOf
- lack of DRY principles in schema reuse


### Tools Used

Parameter | Description
--------- | -----------
Mkdocs | Cleaned up the YAML and rendered it within static site using Mkdoc (initially) and Mark Doc
Redoc linting | I customised the Redoc linter and worked with DevOps to include the custom ruleset as part of the CICD pipeline
IM | Instant messaging to chat to team members
GitBash | For version control via Git
Project management tools | For task management

<!-- Redoc element with (local) link to your OpenAPI definition looks like so inside the body element:


  <redoc spec-url="../swagger.yaml"></redoc>

  Link to Redoc JavaScript on CDN for rendering standalone element:

  <script src="https://cdn.redoc.ly/redoc/latest/bundles/redoc.standalone.js"></script>

-->

### API flat schema structure

As this is a publically-available API, I will rather showcase an example of how I can assist the developers to improve the end-user experience by suggesting improvements to the service itself.


<!-- openapi: 3.0.2
info:
  title:  Using oneOf to improve end-user experience
  version: 0.1.0
  description: >
    DeFi Wallet transaction request as per V1 vs. proposal for V2 improvements.
    <br>
    The API specification can be constructed in such a way as to support end-users by constraining the data they can send. If an end-user is expected to send value A **or** value B and sending A&B will trigger an error, then it is best practice to control this with a `oneOf`. The `oneOf` directs the user as to which data they may send and constrains attempts to diverge from this.
  contact:
    name: Example API Services
    url: "https://www.example.com/"
    email: a@example.com

servers:
  - url: https://example.net
    description: DEV API Server
    variables:
      basePath:
        default: /qapi/v1

security:
  - ApiKeyAuth: []

tags:

  - name: DeFi Wallet current
    description: The request made by the end-user attempts to support both legacy and current EVM transactions, but at the cost of end-user understanding, i.e., the schema structure does not allow the specification to control what is passed by the user.
 proposed
    description
  - name: DeFi Wallet: The request made by end-user has been simplified to logically support legacy and current EVM transactions and `gas` now acts as an object to support that. BUT ALL existing connected wallets would be impacted (MMI/Wallet connect, DeFi wallet) causing a Class 2 break between V1 and V2. -->

<!-- paths: -->

<!-- ### Proposed /transaction endpoint:
  post:
    tags:
      - DeFi Wallet proposed
    summary: Create new transaction
    description: >
      Proposed alt structure for the endpoint that creates an outgoing DeFi Wallet transaction. Note, this touches on:
        - supporting `gas` and `gasLimit` as per various chains requirements -- solution propsed supports end-user preventing them from making a call with both
        - supporting legacy and EIP-1559 style transactions, by supporting end user in preventing them from making an invalid call
        - removed `from`
        <!-- - wip: does not touch the issue of chainID -- these external (Layer 1) wallets that we support are all for EVM networks as we don't support any other chains atm. On the back-end and Qredochain these wallets are generated as type Currency_ETH. This is used just for telling the qredochain and MPC's that it's an EVM based wallet, anything else we don't care as all the state is stored on the respective Layer 1 chain the user interacts with. Regarding the "chainID", in theory it is optional. If you don't specify it we get the chain ID based on the Wallet's type (which is ETH = chainID 1). If you don't specify a chainID, we assume that the transaction is done on Ethereum Mainnet. -->
    parameters:
      - required: true
        schema:
          title: signature
          type: string
        in: header
        name: qredo-api-sig
      - required: true
        schema:
          title: qredo-api-ts
          type: string
        in: header
        name: qredo-api-ts
      - required: true
        schema:
          title: api-key
          type: string
        in: header
        name: qredo-api-key
    requestBody:
      content:
        application/json:
          schema:
            type: object
            properties:
              from:
                type: string
                description: Wallet address funds originate from.
                example: 0x84CE2138E54b1852689132FD73dDC4DExAmplE
              to:
                type: string
                description: The recipient Wallet's address.
                example: 0xac03bb73b6a9e108530aff4df5077c2ExAmplE
              value:
                type: string
                description: The amount to transfer.
                example: 100
              price:
                type: object
                oneOf:
                  - $ref: '#/components/schemas/gasPrice'
                  - $ref: '#/components/schemas/EIP1559Structure'
              gaslimit:
                type: object
                oneOf:
                  - $ref: '#/components/schemas/gasLimit'
                  - $ref: '#/components/schemas/gas'
              data:
                type: string
                description: User-defined, optional field; included if the recipient is a smart contract.
                example: ''
              chainID:
                type: string
                description: Unique identifier for the L1 chain trascation will occur on, defaults to ETH mainnet, pass ID for alternative chains.
                example: ''
              note:
                type: string
                description: User-defined, optional field to include arbitary data.
                example: Test Signing App test 34
            required:
              - from
              - to
              - value
    responses:
      '200':
        description: Success - transaction request created.
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/txInfoResponse'

<!-- All fields are strings because they are in the smallest denomination (wei) and if the value is big enough it could be problematic to serialise it in JSON. -->

/transaction:
  post:
    tags:
      - DeFi Wallet current
    summary: Create new transaction
    description: This endpoint creates an outgoing DeFi Wallet transaction.
    parameters:
      - required: true
        schema:
          title: signature
          type: string
        in: header
        name: qredo-api-sig
      - required: true
        schema:
          title: qredo-api-ts
          type: string
        in: header
        name: qredo-api-ts
      - required: true
        schema:
          title: api-key
          type: string
        in: header
        name: qredo-api-key
    requestBody:
      content:
        application/json:
          schema:
            type: object
            properties:
              from:
                type: string
                description: Wallet address funds originate from.
                example: 0x84CE2138E54b1852689132FD73dDC4DExAmplE
              to:
                type: string
                description: The receiver Wallet's address.
                example: 0x94GE4738E54b2952689132FD73dDC4DExAmplE
              value:
                type: string
                description: The amount to send.
                example: 100
              gas:
                type: string
                description: WARNING! unknown property.
                example:
              gasPrice:
                type: string
                description: Price user is willing to pay per unit, as per legacy EVM.
                example: 21000
              gasLimit:
                type: string
                description: The maximum units of gas units that can be consumed by the transaction.
                example: 21000
              maxFeePerGas:
                type: string
                description: As per EIP-1559; maximum amount of gas units the sender is willing to spend on the transaction (includes `maxFeePerGas` and `baseFeePerGas`).
                example: ''
              maxPriorityFeePerGas:
                type: string
                description: As per EIP-1559; the maximum amount of gas to be included as a tip to the validator.
                example: ''
              data:
                type: string
                description: User-defined, optional field to include arbitary data.
                example: Testnet test 36521
              chainID:
                type: string
                description: Unique identifier for the L1 chain trascation will occur on, defaults to ETH mainnet, pass ID for alternative chains.
                example: 1
              note:
                type: string
                description: User-defined, optional field to include arbitary data.
                example: Test Signing App test 34
            required:
              - from
              - to
              - value
    responses:
      200:
        description: Success - transaction request created.
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/txInfoResponse'

components:

  securitySchemes:
      ApiKeyAuth:
        description: API Key passed in header.
        type: apiKey
        in: header
        name: qredo-api-key


  schemas:

      Error:
        type: object
        properties:
          code:
            type: string
          msg:
            type: string
          detail:
            type: object
            properties:
              reason:
                type: string

      gasLimit:
        type: string
        description: The maximum units of gas units that can be consumed by the transaction.
        example: 21000

      gas:
        type: string
        description: The maximum units of gas units that can be consumed by the transaction.
        example: 21000

      gasPrice:
        type: string
        description: Price user is willing to pay per unit, as per legacy EVM.
        example: 100

      EIP1559Structure:
          type: object
          properties:
              maxFeePerGas:
                type: string
                description: The maximum amount of gas units the sender is willing to spend on the transaction (includes `maxPriorityFeePerGas` and `baseFeePerGas`). WIP description as per Ethereum docs BUT `baseFeePerGas` is not a propery of this tx, note could be this equates to `gas` as yet, unconfirmed.
                example: 300
              maxPriorityFeePerGas:
                type: string
                description: The maximum amount of gas to be included as a tip to the validator.
                example: 10

      txInfoResponse:
        type: object
        properties:
          accountID:
            type: string
            description: Unique identifier of the Entity that holds the Wallet originating this transaction.
            example: 5G7tj56Wv4bekm3VaQutHo1HJQvAwB9Gzu3WhB7K7rAD
          chainID:
            type: string
            description: Unique identifier for the L1 chain trascation will occur on.
            example: 1
          createdBy:
            type: string
            description: Unique identifier for the user that initiated the transaction. request.
            example: 5G7tj56Wv4bekm3VaQutHo1HJQvAwB9Gzu3WhB7K7rAD
          data:
            type: string
            description: Hex-encoded transaction data.
            example: ''
          from:
            type: string
            description: Wallet address funds originate from.
            example: 0x84CE2138E54b1852689132FD73dDC4D3973a44B8
          gasLimit:
            type: string
            description: The maximum units of gas units that can be consumed by the transaction.
            example: 2100
          gasPrice:
            type: string
            description: Price user is willing to pay per unit, as per legacy EVM.
            example: 2100
          network:
            type: string
            description: wip -- this is not sent, what is it?
            example: ''
          nonce:
            type: string
            description: wip -- does it make sense to return a nonce for a transaction request that still needs to be approved?
            example: 0x0
          rawTX:
            type: string
            description: wip is this the signed transaction? Does it make sense to return this for a transaction request that is not yet approved?
            example: 0xf88380018203339407a565b7ed7d7a678680a4c162885bedbb695fe080a44401a6e4000000000000000000000000000000000000000000000000000000000000001226a0223a7c9bcf5531c99be5ea7082183816eb20cfe0bbc322e97cc5c7f71ab8b20ea02aadee6b34b45bb15bc42d9c09de4a6754e7000908da72d48cc7704971491663
          status:
            type: string
            description: wip is this an enum list?
            example: ''
          timestamps:
            type: object
            properties:
              created:
                type: integer
                format: unix-timestamp
              pending:
                type: integer
                format: unix-timestamp
              authorized:
                type: integer
                format: unix-timestamp
              approved:
                type: integer
                format: unix-timestamp
              rejected:
                type: integer
                format: unix-timestamp
              failed:
                type: integer
                format: unix-timestamp
          to:
            type: string
            description: The receiver Wallet's address.
            example: 0x94GE4738E54b2952689132FD73dDC4D3973a47J2
          txHash:
            type: string
            description: Transaction hash from L1 chain. wip does it make sense to return this for a transaction request that is not yet approved?
            example: ''
          txID:
            type: string
            description: Transaction ID; as per `tx_id`.
            example: 279MBUMtAX3lVxXDOgXzG9jNDDu
          value:
            type: string
            example: 100
            description: The amount to send.
          events:
            type: array
            items:
              $ref: '#/components/schemas/txStatusUpdate'

      txStatusUpdate:
        type: object
        properties:
          id:
            type: string
            description: Transaction ID; as per `tx_id`.
            example: 279MBUMtAX3lVxXDOgXzG9jNDDu
          message:
            type: string
            description: Server message.
            example: ''
          status:
            type: string
            description: ''
            example: ''
            enum:
              - created
              - pending
              - authorized
              - approved
              - rejected
              - failed
          timestamp:
            type: integer
            format: unix-timestamp
            description: Timestamp of the status update.
            example: ''


 -->
