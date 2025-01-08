# Superflix Extensions

A powerful and flexible media extensions management system for **Superflix**. 
This system supports multiple types of extensions that can fetch media streams and subtitles from various sources either externally or locally on device.

[View Community Extensions](https://superflix.gitbook.io/community-extensions-list) | 
[How to Add Extensions](#adding-extensions) | 
[How to Create Extensions](#extension-types)

## Table of Contents

* [Features](#features)
* [Extension Types](#extension-types)
  * [Website Extensions](#website-extensions)
  * [Server Extensions](#server-extensions)
  * [List Extensions](#list-extensions)
  * [Code Extensions](#code-extensions)
* [Adding Extensions](#adding-extensions)
* [URL Parameters](#url-parameters)
* [Examples](#examples)

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

Subtitle Parameters:
* `lang`: Language and/or source name
* `url`: URL for .srt or .vtt format subtitles

_Note:_
_Cross-compatible with [Stremio Addons](https://stremio-addons.netlify.app/)._
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

The imported code is run in Typescript environment using eval().
The [URL Parameters](#url-parameters) of `${s}` can also be referenced in the code execution.

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
```

## Adding Extensions

Extensions can be added to the app in two ways:

1. Through the UI:
   * Navigate to the Extensions screen
   * Select the extension type
   * Enter the extension name and URL
   * Click "Add"
2. Through the List extension type:
   * Add a List type extension pointing to your JSON file
   * The app will automatically import all extensions from the list

## URL Parameters

The following parameters are available for dynamic URL construction for both Movies & Series:

Basic Parameters:
* `${s.title}`: Media title (Example: "The Matrix")
* `${s.name}`: Show name for TV shows (Example: "Breaking Bad")
* `${s.slug}`: URL-friendly version of the title (Example: "the-matrix", "breaking-bad")
* `${s.slug_se}`: Slug with season/episode for series (Example Movie: "the-matrix", Example Series: "breaking-bad/1-5")
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
* `${s.type6}`: Media type ("movie" or "episode")
* `${s.type7}`: Media type ("movie" or "tv-show")

Series Information:
* `${s.selected_season_num}`: Selected season number (Example: "1")
* `${s.selected_episode_num}`: Selected episode number (Example: "5")

TMDB ID Parameters:
* `${s.tmdb_id}`: Basic TMDB ID (Example: "603")
* `${s.tmdb_id_colon}`: TMDB ID with colon format (Example Movie: "603", Example Series: "1396:1:5")
* `${s.tmdb_id_slash}`: TMDB ID with slash format (Example Movie: "603", Example Series: "1396/1/5")
* `${s.tmdb_id_se}`: TMDB ID with s/e query (Example Movie: "603", Example Series: "1396?s=1&e=1")
* `${s.tmdb_id_se2}`: TMDB ID with alternative s/e format (Example Movie: "603", Example Series: "1396&s=1&e=1")
* `${s.tmdb_id_se3}`: TMDB ID with season/episode format (Example Movie: "603", Example Series: "1396&season=1&episode=1")
* `${s.tmdb_id_se4}`: TMDB ID with season/episode query (Example Movie: "603", Example Series: "1396?season=1&episode=1")
* `${s.tmdb_id_se5}`: TMDB ID with dot format (Example Movie: "603", Example Series: "1396.1.1")
* `${s.tmdb_id_se6}`: TMDB ID with reversed format (Example Movie: "603", Example Series: "1-1/1396")

IMDB ID Parameters:
* `${s.imdb_id}`: Basic IMDB ID (Example: "tt0133093")
* `${s.imdb_id_colon}`: IMDB ID with colon format (Example Movie: "tt0133093", Example Series: "tt0903747:1:5")
* `${s.imdb_id_slash}`: IMDB ID with slash format (Example Movie: "tt0133093", Example Series: "tt0903747/1/5")
* `${s.imdb_id_se}`: IMDB ID with s/e query (Example Movie: "tt0133093", Example Series: "tt0903747?s=1&e=5")

Recently Watched Parameters:
* `${s.watchedDuration}`: Previously watched duration in seconds
* `${s.previous_tmdb_id_colon}`: Previously watched TMDB ID in colon format
* `${s.previous_selected_season_num}`: Previously selected season number
* `${s.previous_selected_episode_num}`: Previously selected episode number
* `${s.recently_watched}`: Whether the media was recently watched (true/false)
```

## Examples

### Website Extension
```json
{
  "type": "Website",
  "name": "StreamProvider",
  "url": "https://stream.example.com/embed/${s.type3}/${s.tmdb_id}"
}
```

### Server Extension
```json
{
  "type": "Server",
  "name": "Stream Quality",
  "url": "https://api.example.com/stream/${s.type2}/${s.imdb_id_colon}"
}
```

### List Extension
```json
{
  "type": "List",
  "name": "StreamBundle",
  "url": "https://bundle.example.com/extensions/all.json"
}
```

### Code Extension
```json
{
  "type": "List",
  "name": "StreamBundle",
  "url": "https://api.example.com/stream/${s.type3}/${s.tmdb_id}.js"
}
```

---

_Note:_
_Replace all example URLs with actual streaming sources._
_This README is for educational purposes only. Ensure compliance with content providers' terms of service and local regulations when implementing extensions._
