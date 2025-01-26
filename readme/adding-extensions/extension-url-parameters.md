---
description: Breakdown of Superflix's robust Dynamic URLs
---

# Extension URL Parameters

### <mark style="color:purple;">Superflix</mark> uses <mark style="background-color:purple;">Dynamic URLs</mark> that can use a number parameters. Thus, we can fetch streams for both Movie & Series using a single URL.

## Title Parameters:

* `${s.title}`: Media title (Example: "The Matrix")
* `${s.slug}`: URL-friendly version of the title (Example: "the-matrix", "breaking-bad")
* `${s.slug_se}`: Slug with season/episode for series (Example Movie: "the-matrix", Example Series: "breaking-bad/1-5")

## Type Parameters:

* `${s.type}`: Media type ("Movies" or "Series")
* `${s.type2}`: Media type ("movie" or "series")
* `${s.type3}`: Media type ("movie" or "tv")
* `${s.type4}`: Media type ("movie" or "show")
* `${s.type5}`: Media type ("m" or "t")
* `${s.type6}`: Media type ("movie" or "episode")
* `${s.type7}`: Media type ("movie" or "tv-show")

## Series Information:

* `${s.selected_season_num}`: Selected season number (Example: "1")
* `${s.selected_episode_num}`: Selected episode number (Example: "5")
* `${s.se}`: s/e format\
  (Example Movie: "", Example Series: "s=1\&e=1")
* `${s.se2}`: s/e format\
  (Example Movie: "", Example Series: "season=1\&e=1")

## TMDB ID Parameters:

* `${s.tmdb_id}`: Basic TMDB ID \
  (Example: "603")
* `${s.tmdb_id_colon}`: TMDB ID with colon format \
  (Example Movie: "603", Example Series: "1396:1:5")
* `${s.tmdb_id_slash}`: TMDB ID with slash format \
  (Example Movie: "603", Example Series: "1396/1/5")
* `${s.tmdb_id_se}`: TMDB ID with s/e format \
  (Example Movie: "603", Example Series: "1396?s=1\&e=1")
* `${s.tmdb_id_se2}`: TMDB ID with s/e format  \
  (Example Movie: "603", Example Series: "1396\&s=1\&e=1")
* `${s.tmdb_id_se3}`: TMDB ID with s/e format  \
  (Example Movie: "603", Example Series: "1396\&season=1\&episode=1")
* `${s.tmdb_id_se4}`: TMDB ID with s/e format  \
  (Example Movie: "603", Example Series: "1396?season=1\&episode=1")
* `${s.tmdb_id_se5}`: TMDB ID with s/e format  \
  (Example Movie: "603", Example Series: "1396.1.1")
* `${s.tmdb_id_se6}`: TMDB ID with s/e format  \
  (Example Movie: "603", Example Series: "1-1/1396")

## IMDB ID Parameters:

* `${s.imdb_id}`: Basic IMDB ID \
  (Example: "tt0133093")
* `${s.imdb_id_colon}`: IMDB ID with colon format \
  (Example Movie: "tt0133093", Example Series: "tt0903747:1:5")
* `${s.imdb_id_slash}`: IMDB ID with slash format \
  (Example Movie: "tt0133093", Example Series: "tt0903747/1/5")
* `${s.imdb_id_se}`: IMDB ID with s/e format \
  (Example Movie: "tt0133093", Example Series: "tt0903747?s=1\&e=5")

