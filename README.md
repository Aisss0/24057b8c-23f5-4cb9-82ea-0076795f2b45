# Superflix Extensions

A powerful and flexible media extensions management system for <mark style="color:purple;">**Superflix**</mark>. \
This system supports multiple types of extensions that can fetch media streams and subtitles from various sources either externally or locally on device.

## Table of Contents

* [Features](./#features)
* [Extension Types](./#extension-types)
  * [Website Extensions](./#website-extensions)
  * [Server Extensions](./#server-extensions)
  * [List Extensions](./#list-extensions)
  * [Code Extensions](./#code-extensions)
* [Adding Extensions](./#adding-extensions)
* [URL Parameters](./#url-parameters)
* [Examples](./#examples)

## Features

* Multiple extension types support (Website, Server, List, Code)
* Works with torrent magnet links or info hash
* Able to run locally on device to prevent CORS or rate limit issues
* Dynamic URL parameter substitution
* Easy sharing of extensions setup
* Parallel processing of multiple sources
* Automatic subtitle fetching and parsing
* Quality-based sorting of sources
* Deduplication of streams and subtitles

## Extension Types

### Website Extensions

Website extensions are used to scrape media content directly from websites. They are processed through a WebView and can capture stream URLs and subtitles.

```json
{
  "type": "Website",
  "name": "ExampleStream",
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
  "name": "StreamAPI",
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

Subtitle Parametes:

* `lang`: Language and/or source name
* `url`: URL for .srt or .vtt format subtitles

_Note:_\
_Cross-compatible with_ [_Stremio Addons_](https://stremio-addons.netlify.app/)_._ \
_Simply use the Addon's url (e.g.: https://api.example.com/manifest.json)._

### List Extensions

List extensions allow importing multiple extensions from a single JSON endpoint.

```json
{
  "type": "List",
  "name": "ExtensionList",
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

### Code Extensions

Code extensions allow for custom JavaScript/Typescript code execution to fetch and process streams locally on device.

```json
{
  "type": "Code",
  "name": "CustomFetcher",
  "url": "https://example.com/fetcher.js"
}
```

The imported code is run in Typescript environment using eval(). \
The [URL Parameters](./#url-parameters) of `${s}` can also be referenced in the code execution.

Available libraries are:

<pre class="language-typescript" data-full-width="false"><code class="lang-typescript"><strong>React from 'react'
</strong>axios from 'axios';
cheerio from 'react-native-cheerio'
WebView from 'react-native-webview'
Crypto from 'expo-crypto'
decode as base64decode from 'base-64'
semver from 'semver'
ISO6391 from 'iso-639-1'
</code></pre>

## Adding Extensions

Extensions can be added to the app in three ways:

1. Through the UI:
   * Navigate to the Extensions screen
   * Select the extension type
   * Enter the extension name and URL
   * Click "Add"
2. Through the List extension type:
   * Add a List type extension pointing to your JSON file
   * The app will automatically import all extensions from the list
3. Hardcoded defaults:
   * Edit the hardcodedExtensions array in the code
   * Extensions will be added on first launch

## URL Parameters

The following parameters are available for dynamic URL construction for both Movies & Series:

Basic Parameters:

* `${s.title}`: Media title
* `${s.name}`: Show name (for TV shows)
* `${s.slug}`: URL-friendly version of the title
* `${s.backdrop_url}`: Backdrop image URL
* `${s.poster_url}`: Poster image URL
* `${s.logo_url}`: Primary logo URL
* `${s.logo_url2}`: Secondary logo URL

Type Parameters:

* `${s.type}`: Media type ("Movies" or "Series")
* `${s.type2}`: Media type ("movie" or "series")
* `${s.type3}`: Media type ("movie" or "tv")
* `${s.type4}`: Media type ("movie" or "show")
* `${s.type5}`: Media type ("m" or "t")

TMDB ID Parameters:

* `${s.tmdb_id}`: Basic TMDB ID
* `${s.tmdb_id_colon}`: TMDB ID with colon format (e.g.: "123:1:1" for TV)
* `${s.tmdb_id_slash}`: TMDB ID with slash format (e.g.: "123/1/1" for TV)
* `${s.tmdb_id_se}`: TMDB ID with s/e query (e.g.: "123?s=1\&e=1" for TV)
* `${s.tmdb_id_se2}`: TMDB ID with alternate s/e format
* `${s.tmdb_id_se3}`: TMDB ID with season/episode format
* `${s.tmdb_id_se4}`: TMDB ID with season/episode query
* `${s.tmdb_id_se5}`: TMDB ID with dot format (e.g.: "123.1.1" for TV)

IMDB ID Parameters:

* `${s.imdb_id}`: Basic IMDB ID
* `${s.imdb_id_colon}`: IMDB ID with colon format (e.g.: "123:1:1" for TV)
* `${s.imdb_id_slash}`: IMDB ID with slash format (e.g.: "tt123/1/1" for TV)
* `${s.imdb_id_se}`: IMDB ID with s/e query (e.g.: "tt123?s=1\&e=1" for TV)

Series Information:

* `${s.selected_season_num}`: Selected season number
* `${s.selected_episode_num}`: Selected episode number

_Note:_ \
_For Parameters that have included Season Number & Episode number (e.g.: s.tmdb\_id\_se), it will work dynamically for both Movies & Series._ \
_For example, if the media type is Movie, the Season Number and Episode Number will simply be ignored where s.tmdb\_id\_se will be the same as s.tmdb\_id._

## Examples

### Basic Website Extension

```json
{
  "type": "Website",
  "name": "StreamProvider",
  "url": "https://stream.example.com/embed/${s.type3}/${s.tmdb_id}"
}
```

### Server Extension with Quality Filter

```json
{
  "type": "Server",
  "name": "Stream Quality",
  "url": "https://api.example.com/qualityfilter=4k|limit=5/stream/${s.type2}/${s.imdb_id_colon}"
}
```

### List Extension with Multiple Sources

```json
{
  "type": "List",
  "name": "StreamBundle",
  "url": "https://bundle.example.com/extensions/all.json"
}
```



***



_Note:_ \
_Replace all example URLs with actual streaming sources._ \
_This README is for educational purposes only. Ensure compliance with content providers' terms of service and local regulations when implementing extensions._
