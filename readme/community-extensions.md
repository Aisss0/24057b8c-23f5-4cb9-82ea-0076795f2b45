# Community Extensions

{% hint style="info" %}
_You can add the either entire list or selected extensions._\
_Guide on Adding Extensions can be referred_ [_here_](adding-extensions/)_._
{% endhint %}

```json
[
  {
    type: 'Server',
    name: 'MediaFusion | Elfhosted',
    url: 'https://mediafusion.elfhosted.com/eJwBgAB__5repNJ3wgotw9v6KFw7ZDoLUUfQX-H-qY9_QvYmWjYokXt7XaTaeB0A3tzOMqcELPd2iN2zreylchUZBjNCh6xBAVWml5eeHpc1srfaBgVaNKriJBFA5QaO4dcy2EizSi-iuxVgM7z0z-lpGjQzr9sm9WnL-yyET6Y33l-fKVrmsEo-Ng==/manifest.json',
    description: 'Fetches torrents for media'
  },
  {
    type: 'Server',
    name: 'Torrentio',
    url: 'https://torrentio.strem.fun/limit=10/stream/${s.type2}/${s.imdb_id_colon}.json',
    description: 'Fetches torrents for media'
  },
  {
    type: 'Website',
    name: 'VidsrcDev',
    url: 'https://vidsrc.dev/embed/${s.type3}/${s.tmdb_id_slash}',
    description: 'Checks website for video stream',
    urlKeyword: '==.m3u8'
  },
  {
    type: 'Website',
    name: 'VidlinkPro',
    url: 'https://vidlink.pro/${s.type3}/${s.tmdb_id_slash}',
    description: 'Checks website for video stream',
    urlKeyword: '==.m3u8'
  },
  {
    type: 'Website',
    name: 'PopcornMovies',
    url: 'https://popcornmovies.to/${s.type7}/${s.slug_se}',
    description: 'Checks website for video stream'
  },
  
]
```
