# Extension Examples

### Website

```json
{
  "type": "Website",
  "name": "StreamProvider",
  "url": "https://stream.example.com/embed/${s.type3}/${s.tmdb_id}"
}
```

### Server

```json
{
  "type": "Server",
  "name": "Stream Quality",
  "url": "https://api.example.com/stream/${s.type2}/${s.imdb_id_colon}"
}
```

### Code

```json
{
  "type": "Code",
  "name": "Stream Extractor",
  "url": "https://api.example.com/stream/${s.type3}/${s.tmdb_id}.js"
}
```

### List

```json
{
  "type": "List",
  "url": "[
    {
      "type": "Website",
      "name": "Stream Provider & Quality",
      "url": "https://stream.example.com/embed/${s.type3}/${s.tmdb_id}"
    },
    {
      "type": "Server 1",
      "name": "Stream Provider & Quality",
      "url": "https://api.example.com/stream1/${s.type2}/${s.imdb_id_colon}"
    },
    {
      "type": "Server 2",
      "name": "Stream Provider & Quality",
      "url": "https://api.example.com/stream2/${s.type2}/${s.imdb_id_colon}"
    },
  ]"
}
```

or

```json
{
  "type": "List",
  "url": "https://bundle.example.com/extensions/all.json"
}
```
