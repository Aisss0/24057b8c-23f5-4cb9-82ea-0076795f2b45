# Community Extensions

{% hint style="info" %}
_Cross-compatible with_ [_Stremio Addons_](https://stremio-addons.com/)_._ \
&#xNAN;_&#x41;dd the Addon's url (e.g.: https://api.example.com/manifest.json) as a Server extension._
{% endhint %}

{% hint style="info" %}
_You can add the by copying the entire list below or selected extensions._\
_Guide on Adding Extensions can be referred_ [_here_](adding-extensions/)_._
{% endhint %}

```json
[
  {
    "type": "Embed Link",
    "name": "MovieClub",
    "url": "https://moviesapi.club/${s.type3}/${s.tmdb_id_dash}",
    "description": "Links to Embed externally"
  },
  {
    "type": "Embed Link",
    "name": "SmashyStream",
    "url": "https://player.smashy.stream/${s.type3}/${s.tmdb_id_se}",
    "description": "Links to Embed externally"
  },
  {
    "type": "Embed Link",
    "name": "111Movies",
    "url": "https://111movies.com/${s.type3}/${s.tmdb_id_slash}",
    "description": "Links to Embed externally"
  },
  {
    "type": "Embed Link",
    "name": "EmbedSu",
    "url": "https://embed.su/embed/${s.type3}/${s.tmdb_id_slash}",
    "description": "Links to Embed externally"
  },
  {
    "type": "Website",
    "name": "xPrime",
    "url": "https://xprime.tv/watch/${s.tmdb_id_slash}",
    "description": "Checks website for video stream"
  },
  {
    "type": "Website",
    "name": "VidlinkPro",
    "url": "https://vidlink.pro/${s.type3}/${s.tmdb_id_slash}",
    "description": "Checks website for video stream"
  },
  {
    "type": "Website",
    "name": "VidJoy",
    "url": "https://vidjoy.pro/embed/${s.type3}/${s.tmdb_id_slash}",
    "description": "Checks website for video stream",
    "replaceReqUrlKeywords": [
      {
        "/embed/api/routes/watch2gather?url=": ""
      }
    ],
    "decodeReqUrl": true,
    "matchNum": 2,
    "quality": "480p"
  },
  {
    "type": "Website",
    "name": "Streamflix",
    "url": "https://watch.streamflix.one/${s.type3}/${s.tmdb_id}/watch?server=4&${s.se2}",
    "description": "Checks website for video stream"
  },
  {
    "type": "Website",
    "name": "PopcornMovies",
    "url": "https://popcornmovies.to/${s.type7}/${s.slug_se}",
    "description": "Checks website for video stream"
  },
  {
    "type": "Server",
    "name": "Torrentio",
    "url": "https://torrentio.strem.fun/stream/${s.type2}/${s.imdb_id_colon}.json",
    "description": "Fetches torrents for media"
  },
  {
    "type": "Server",
    "name": "MediaFusion | Elfhosted",
    "url": "https://mediafusion.elfhosted.com/D-O7UtAOauPlvjL2n7HQfwWnz6Nr48lPk2ZnTCCYoVvD0/manifest.json",
    "description": "Fetches torrents for media"
  }
]
```
