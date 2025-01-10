---
description: Guide on how to create extensions for Superflix
---

# Creating Extensions

Anyone can create & share extensions for <mark style="color:purple;">**Superflix**</mark>**.**\
Our flexible extensions allow users to fetch streams either from remote sources or locally on device!

***

{% hint style="info" %}
_<mark style="background-color:purple;">**Dynamic URLs**</mark> are used in all extensions to fetch streams for both Movies & Series from a single URL._\
_You can read the full URL Parameters_ [_here_](adding-extensions/extension-url-parameters.md)_._
{% endhint %}

## Extension Types

### Website Extensions

Website extensions are used to scrape media content directly from websites. They are processed through a WebView and can capture stream URLs and subtitles.

```json
{
  "type": "Website",
  "name": "Example Stream",
  "url": "https://example.com/embed/${s.type3}/${s.tmdb_id_se}"
}
```

Optional advanced parameters can be passed by adding Website extension through List extension:

* `urlKeyword`: Specific keyword to match in captured URLs (defaults to .m3u8 and .mp4)
* `dataKeyword`: Keyword to match in response body
* `fetchOnly`: Boolean to only capture network requests without page interaction

### Server Extensions

Server extensions fetch data directly from APIs or servers that return JSON responses containing stream information.

```json
{
  "type": "Server",
  "name": "Stream API",
  "url": "https://api.example.com/stream/${s.type2}/${s.imdb_id_colon}"
}
```

Expected response format:

```json
{
  "streams": [
    {
      "name": "HLS Auto",
      "description": "Movie Title",
      "url": "https://example.com/stream.m3u8"
    },
    {
      "name": "Torrent 1080p",
      "description": "Movie Title",
      "url": "magnet:?xt=urn:btih:08ada5a7a6183aae1e09d831df6748d566095a10"
    },
    {
      "name": "Torrent Hash 1080p", 
      "description": "Movie Title",
      "infoHash": "08ada5a7a6183aae1e09d831df6748d566095a10"
    },
    {
      "name": "External 720p",
      "description": "Movie Title", 
      "externalUrl": "https://example.com/12345"
    }
  ],
  "subtitles": [
    {
      "lang": "English",
      "url": "https://example.com/subs.vtt"
    }
  ]
}
```

Stream Parameters:

* `name`: Stream source name and/or quality
* `description`: Media Title and/or other info
* `url`: Direct video stream URL or torrent magnet link
* `infoHash`: Torrent info hash
* `externalUrl`: URL to be opened externally in default browser

Subtitle Parameters:

* `lang`: Language and/or source name
* `url`: URL for .srt or .vtt format subtitles

{% hint style="info" %}
_Cross-compatible with_ [_Stremio Addons_](https://stremio-addons.com/)_._ \
&#xNAN;_&#x53;imply use the Addon's url (e.g.: https://api.example.com/manifest.json)._
{% endhint %}

### Code Extensions

Code extensions allow for custom JavaScript/Typescript code execution to fetch and process streams locally on device.

```json
{
  "type": "Code",
  "name": "Custom Fetcher",
  "url": "https://example.com/fetcher.js"
}
```

The imported code is run in Typescript environment using eval(). The [URL Parameters](creating-extensions.md#url-parameters) of `${s}` can also be referenced in the code execution.

Expected response format:

* Code execution should **return a list variable** in the same response format as [#server-extensions](creating-extensions.md#server-extensions "mention")

Available libraries are:

```typescript
React from 'react'
axios from 'axios'
cheerio from 'react-native-cheerio'
WebView from 'react-native-webview'
Crypto from 'expo-crypto'
decode as base64decode from 'base-64'
semver from 'semver'
ISO6391 from 'iso-639-1'
slugify from 'slugify'
```

### List Extensions

List extensions allow importing multiple extensions from a single JSON endpoint.

```json
{
  "type": "List",
  "name": "Extension List",
  "url": "https://example.com/extensions.json"
}
```

Expected response format:

```json
[
  {
    "type": "Website",
    "name": "Source1",
    "url": "https://example1.com/embed/${s.tmdb_id}"
  },
  {
    "type": "Website", 
    "name": "Source2",
    "url": "https://example2.com/embed/${s.tmdb_id}",
    "fetchOnly": "true"
  },
  {
    "type": "Server",
    "name": "Source3",
    "url": "https://example3.com/stream/${s.imdb_id}"
  }
]
```
