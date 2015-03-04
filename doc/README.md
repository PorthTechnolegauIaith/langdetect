[scroll down for english](#language-detection-api)

# API Adnabod Iaith

Mae'r API yn gweithio dros HTTPS GET, felly gellir defnyddio unrhyw iaith/meddalwedd HTTP er mwyn cysylltu at yr API.

## Fersiwn Cyfredol

Un fersiwn o'r API sydd ar gael, sef v1 neu 'fersiwn 1'.
Dychwelir enw y fersiwn sy'n cael ei roi yn y canlyniadau JSON.

Bydd yr URL yn newid gyda phob fersiwn newydd o'r API. Ar hyn o bryd, dylid defnyddio `/v1` ar gyfer fersiwn 1.

## Sgema

Mae cysylltiad â'r API yn gweithio dros HTTPS yn unig, gan ddefnyddio'r parth `api.techiaith.org/langdetect`. Mae'r holl ddata sy'n cael eu derbyn/hanfon yn cael ei drosglwyddo ar ffurf JSON ([unicode-escaped ASCII](http://tools.ietf.org/html/rfc5137)).

## Paramedrau yr API

| Paramedr   | Disgrifiad | Sylwadau |
|------------|------------|----------|
| `api_key`  | Eich allwedd API, ar gael o'r Canolfan APIs (https://api.techiaith.org) | angenrheidiol |
| `text`     | Y testun ar gyfer adnabod iaith. Dylid ei fformatio yn ôl RFC 3986 (percent-encoded) | angenrheidiol |
| `lang`     | Yr iaith ar gyfer unrhyw destun fydd yn cael ei ddychwelyd (e.e. negeseuon gwall). Dewis o `en` neu `cy`. Rhagosodiad: `cy` | dewisiol |
| `callback` | Enw 'function' ar gyfer unrhyw callback JSON-P (gweler isod) | dewisiol |

### Enghraifft

```
$ curl https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau

{
    "result": {
        "probabilities": {"cy": 0.9999919323188298},
        "lang": "cy"
    },
    "success": true,
    "version": 1
}
```

## Canlyniadau

Mae'r canlyniadau mewn fformat dict:

```
"result": {
    "probabilities": {"cy": 0.9999919323188298},
    "lang": "cy"
}
```

Mae'r gwerth `lang` yn cyfeirio at yr iaith mwyaf tebygol ar gyfer y testun.
Mae'r gwerth `probabilities` yn rhoi rhestr o'r holl ieithoedd mae'r API wedi'u codi fel ieithoedd posib y testun (Yn yr achos uchod, mae'r API wedi nodi mae dim ond Cymraeg sy'n debygol o fod yn iawn).
Mae'r ieithoedd yn cael eu dychwelyd mewn ffurf dau nod [ISO 639-1](http://en.wikipedia.org/wiki/ISO_639-1)

## JSON-P Callbacks

Gellir defnyddio'r API gyda JSON-P callbacks trwy ychwanegu'r paramedr `callback` i'ch galwad:

```
$ curl https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau&callback=foo
foo({
    "result": {
        "probabilities": {"cy": 0.9999919323188298},
        "lang": "cy"
    },
    "success": true,
    "version": 1
});
```


## Cyfyngu nifer yr alwadau yr awr

Mae gan yr API gyfyngiad ar nifer yr alwadau y gellir eu gwneud mewn awr.

Mae'r cyfyngiad yn un haul iawn, ac rydym yn ystyried bydd hyn yn ddigon i'r rhan fwyaf o ein defnyddwyr. Os ydych eisiau cynyddu nifer y galwadau at yr API sydd gennych, cysylltwch â ni.

Gellir gweld cyfanswm nifer eich galwadau ar unrhyw adeg trwy edrych ar y 'HTTP headers' yn eich galwad API:

```
$ curl -i https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau                                                           

HTTP/1.1 200 OK
Date: Mon, 17 Nov 2014 14:41:21 GMT
Content-Type: application/json
Content-Language: cy
X-Ratelimit-Remaining: 276
X-Ratelimit-Limit: 300
X-Ratelimit-Reset: 1416237399
```

Mae'r headers yn cynnwys yr holl wybodaeth sydd ei angen:

| Enw'r Header | Disgrifiad |
|--------------|------------|
| X-RateLimit-Limit | Y nifer mwyaf o alwadau allwch chi eu gwneud mewn awr |
| X-RateLimit-Remaining | Y nifer o alwadau sydd gennych ar ôl yn y 'blwch' cyfyngu presennol |
| X-RateLimit-Reset | Yr amser y bydd y 'blwch' cyfyngu presennol yn cael ei ail-osod, mewn [eiliadau epoch UTC](http://en.wikipedia.org/wiki/Unix_time) |

Os ydych chi angen yr amser mewn fformat gwahanol, gellir gwneud hyn gydag unrhyw iaith raglennu modern. Er engraifft, gellir gwneud hyn trwy gonsol eich porwr (gyda Javascript) a dychwelych gwrthrych 'Javascript Date'.

```javascript
new Date(1416237399 * 1000)
Date 2014-11-17T15:16:39.000Z
```

Ar ôl i chi fynd dros eich nifer mwyaf o alwadau yr awr, byddwch yn derbyn gwall gan y gweinydd (403 Forbidden):

```
curl -i https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau

HTTP/1.1 200 OK
Date: Tue, 18 Nov 2014 10:45:10 GMT
Content-Type: application/json
X-Ratelimit-Limit: 300
X-Ratelimit-Remaining: 0
Content-Language: cy
X-Ratelimit-Reset: 1416310586

{
    "success": false,
    "errors": ["403 Forbidden: Rydych chi wedi mynd dros eich cyfyngiad nifer yr alwadau yr awr"]
}
```

-----------------------

## Language Detection API

The API works using HTTPS GET, meaning you can use it with any programming language/software package of your choice which works over HTTP

## Current Version

Currently, there is only one version of the API available: v1 or 'version 1'.
The version used for the request is returned in the JSON result.

## Schema

The connection to the API is over HTTPS only, from the domain `api.techiaith.org/langdetect`. All data sent to and received from the API is in JSON ([unicode-escaped ASCII](http://tools.ietf.org/html/rfc5137))

## API Parameters

| Parameter   | Description | Notes |
|------------|------------|----------|
| `api_key`  | Your API key, from the API Centre (https://api.techiaith.org) | required |
| `text`     | The text to detect the language from. Formatted according to RFC 3986 (percent-encoded) | required |
| `lang`     | The language for any feedback text returned by the API (e.e. error messages). Choices: `en` or `cy`. Default: `cy` | optional |
| `callback` | Name of the function to wrap the response in for a JSON-P callback (see below) | optional |

### Example

```
$ curl https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau

{
    "result": {
        "probabilities": {"cy": 0.9999919323188298},
        "lang": "cy"
    },
    "success": true,
    "version": 1
}
```

## Results

The results are in a dict format:

```
"result": {
    "probabilities": {"cy": 0.9999919323188298},
    "lang": "cy"
}
```

Tha value of `lang` refers to the language detected from the text with the highest probability.
The value of `probabilties` is a list of all possible languages for the text, with their corresponding probabilties (in the above result, the API has only noted one language - Welsh - as a possible language for the text).
The languages are returned in their [ISO 639-1](http://en.wikipedia.org/wiki/ISO_639-1) format.

## JSON-P Callbacks

You can use the API with JSON-P callbacks by adding the parameter `callback` to your request:

```
$ curl https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau&callback=foo
foo({
    "result": {
        "probabilities": {"cy": 0.9999919323188298},
        "lang": "cy"
    },
    "success": true,
    "version": 1
});
```

## Rate Limiting

The API has a limit on the number of requests you can make per hour, linked to your API key.

You are initially given a high number of requests per hour, which we believe will be enough for most of our users.
If you would like to increase the number of requests you can make to the API per hour, use the form within the 'API Centre'.

You can view the number of requests you have made/have remaining at any time by looking at the 'HTTP headers' of any response to the API:

```
$ curl -i https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau                                                            

HTTP/1.1 200 OK
Date: Mon, 17 Nov 2014 14:41:21 GMT
Content-Type: application/json
Content-Language: cy
X-Ratelimit-Remaining: 276
X-Ratelimit-Limit: 300
X-Ratelimit-Reset: 1416237399
```

The headers contain all information you may require:

| Header Name | Description |
|--------------|------------|
| X-RateLimit-Limit | Maximum number of requests you can make per hour (rate limit) |
| X-RateLimit-Remaining | The number of requests remaining in the current rate limit window |
| X-RateLimit-Reset | The time at which the current rate limit window resets in [UTC epoch seconds](http://en.wikipedia.org/wiki/Unix_time) |


If you need the time in a different format, any modern programming language can get the job done. For example, if you open up the console on your web browser, you can easily get the reset time as a JavaScript Date object.

```javascript
new Date(1416237399 * 1000)
Date 2014-11-17T15:16:39.000Z
```

Once you go over the rate limit you will receive an error response:

```
$ curl -i https://api.techiaith.org/langdetect/v1/?api_key=123&text=mae%20hen%20wlad%20fy%20nhadau&lang=en

HTTP/1.1 200 OK
Date: Tue, 18 Nov 2014 10:44:37 GMT
Content-Type: application/json
X-Ratelimit-Limit: 300
X-Ratelimit-Remaining: 0
Content-Language: en
X-Ratelimit-Reset: 1416310586

{
    "success": false,
    "errors": ["403 Forbidden: You have exceeded your request limit"]
}
```
