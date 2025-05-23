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
    "details": "Links to Embed externally"
  },
  {
    "type": "Embed Link",
    "name": "SmashyStream",
    "url": "https://player.smashy.stream/${s.type3}/${s.tmdb_id_se}",
    "details": "Links to Embed externally"
  },
  {
    "type": "Embed Link",
    "name": "111Movies",
    "url": "https://111movies.com/${s.type3}/${s.tmdb_id_slash}",
    "details": "Links to Embed externally"
  },
  {
    "type": "Embed Link",
    "name": "EmbedSu",
    "url": "https://embed.su/embed/${s.type3}/${s.tmdb_id_slash}",
    "details": "Links to Embed externally"
  },
  {
    "type": "Website",
    "name": "xPrime",
    "url": "https://xprime.tv/watch/${s.tmdb_id_slash}",
    "details": "Checks website for video stream",
    "resBodyKeywords": [".m3u8",".mp4"],
    "customCodeString": "return (function(streamData) { try { const data = typeof streamData === 'string' ? JSON.parse(streamData) : streamData; const convertedData = { streams: [], subtitles: [] }; if (data && data.streams) { for (const quality in data.streams) { convertedData.streams.push({ url: data.streams[quality], quality: quality }); } } if (data && data.subtitles && Array.isArray(data.subtitles)) { convertedData.subtitles = data.subtitles.map(subtitle => { return { lang: subtitle.label, url: subtitle.file }; }); } return convertedData; } catch (e) { console.error('Error in conversion:', e); return { error: e.message, streams: [], subtitles: [] }; } })(streamData);"
  },
  {
    "type": "Website",
    "name": "VidJoy",
    "url": "https://vidjoy.pro/embed/${s.type3}/${s.tmdb_id_slash}",
    "details": "Checks website for video stream",
    "replaceReqUrlKeywords": {
      "/embed/api/routes/watch2gather?url=": "",
      "https://vidjoy.pro": ""
    },
    "decodeReqUrl": true,
    "matchNum": 2,
    "quality": "480p"
  },
  {
    "type": "Website",
    "name": "Streamflix",
    "url": "https://watch.streamflix.one/${s.type3}/${s.tmdb_id}/watch?server=4&${s.se2}",
    "details": "Checks website for video stream",
    "quality": "1080p"
  },
  {
    "type": "Website",
    "name": "VidlinkPro",
    "url": "https://vidlink.pro/${s.type3}/${s.tmdb_id_slash}",
    "details": "Checks website for subtitles",
    "resBodyKeywords": [".m3u8",".mp4"],
    "customCodeString": "return (function(streamData) { try { const data = typeof streamData === 'string' ? JSON.parse(streamData) : streamData; const convertedData = { streams: [], subtitles: [] }; if (data && data.stream && data.stream.captions && Array.isArray(data.stream.captions)) { convertedData.subtitles = data.stream.captions.map(caption => ({ lang: caption.language, url: caption.url })); } return convertedData; } catch (e) { console.error('Error in conversion:', e); return { error: e.message, streams: [], subtitles: [] }; } })(streamData);"
  },
  {
    "type": "Server",
    "name": "Torrentio",
    "url": "https://torrentio.strem.fun/stream/${s.type2}/${s.imdb_id_colon}.json",
    "details": "Fetches torrents for media"
  },
  {
    "type": "Server",
    "name": "MediaFusion | Elfhosted",
    "url": "https://mediafusion.elfhosted.com/D-O7UtAOauPlvjL2n7HQfwWnz6Nr48lPk2ZnTCCYoVvD0/manifest.json",
    "details": "Fetches torrents for media"
  }
]
```
