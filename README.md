# Superflix Extensions

A powerful and flexible media extensions management system for Superflix. This system supports multiple types of extensions that can fetch media streams and subtitles from various sources either externally or locally on device.

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

Optional parameters:

* `urlKeyword`: Specific keyword to match in captured URLs (defaults to .m3u8 and .mp4)
* `dataKeyword`: Keyword to match in response body
* `fetchOnly`: Boolean to only capture network requests without page interaction

### Server Extensions

Server extensions fetch data directly from APIs or servers that return JSON responses containing stream information.

_Note: Cross-compatible with_ [_Stremio Addons_](https://stremio-addons.netlify.app/)_. Simply use the Addon's url (e.g.: https://api.example.com/manifest.json)._

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
      "name": "1080p",
      "url": "https://example.com/stream.m3u8",
      "description": "Movie Title"
    },
    {
      "name": "Torrent 1080p",
      "url": "magnet:?xt=urn:btih:08ada5a7a6183aae1e09d831df6748d566095a10",
      "description": "Movie Title"
    },
    {
      "name": "Torrent Auto",
      "infoHash": "08ada5a7a6183aae1e09d831df6748d566095a10",
      "description": "Movie Title"
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
    "type": "Server",
    "name": "Source2",
    "url": "https://example2.com/stream/${s.imdb_id}"
  }
]
```

### Code Extensions

Code extensions allow for custom JavaScript code execution to fetch and process streams.

```json
{
  "type": "Code",
  "name": "CustomFetcher",
  "url": "https://example.com/fetcher.js"
}
```

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

Episode Information:

* `${s.selected_season_num}`: Selected season number
* `${s.selected_episode_num}`: Selected episode number

_Note: For Parameters that have included Season Number & Episode number (e.g.: s.tmdb\_id\_se), it will work dynamically for both Movies & Series. For example, if the media type is Movie, the Season Number and Episode Number will simply be ignored where s.tmdb\_id\_se will be the same as s.tmdb\_id._

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



_Note: Replace all example.com URLs with actual streaming sources._ \
_This README is for educational purposes only. Ensure compliance with content providers' terms of service and local regulations when implementing extensions._
