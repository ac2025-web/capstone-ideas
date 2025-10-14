# Capstone – Initial Project Ideas (Step 1)

This document proposes three project ideas and explores candidate APIs. Final API choice will be made in Step 3. Target effort: ~45–65 hours.

---

## How I evaluated APIs (brief)
- Free tier & permissive usage (no or easy API key).
- Good docs / stable CORS / JSON.
- Entities match my features.
- Simple rate limits or easy caching.

---

## Idea #1 — CineLog (TV/Movie Watchlist) — *easiest to ship*
**One-liner:** Track TV & movies with statuses, ratings, and notes; “Continue Watching” view.  
**Users/Problem:** People forget what they’re watching; want a single list w/ progress.

**External API (read only):**  
- **TVMaze** – https://www.tvmaze.com/api (no key)  
  - Search shows: `GET https://api.tvmaze.com/search/shows?q=<query>`  
  - Show: `GET https://api.tvmaze.com/shows/<id>`  
  - Episodes: `GET https://api.tvmaze.com/shows/<id>/episodes`

**My API (Express + Postgres):**  
- Auth (register/login).  
- My List (status = planning|watching|completed, rating 1–5, notes).  
- (Optional) cache titles/episodes.

**MVP (must-have):**
- Sign up/login.
- Search via TVMaze → save to My List.
- Update status/rating/notes.
- “Continue Watching” section.
- Responsive UI.

**Stretch:**
- Episode guide + mark last watched.
- Simple recommendations by genre.
- Public profile / follow friends.
- Import/export CSV.

**Data model (sketch):**
- `users(id, email, password_hash, created_at)`
- `titles(id, ext_api, ext_id, name, type, poster_url, created_at)`
- `user_titles(user_id, title_id, status, rating, notes, last_episode_id, updated_at)`
- *(optional)* `episodes(id, title_id, season, number, name, airdate)`

**Risks & mitigations:**
- Rate limits → server-side caching.  
- HTML in summaries → sanitize.  
- Scope creep → ship MVP first.

---

## Idea #2 — PawPals (Dogbook / Nearby Parks + Posts)
**One-liner:** Micro social app for dog owners: dog profiles, posts, and nearby park map.  
**Users/Problem:** Owners want easy playdates and local dog spots.

**External API (places):**  
- **OpenTripMap** – https://opentripmap.io/product  
  - Places around point: `GET /en/places/radius?radius=…&lon=…&lat=…&kinds=parks`
- *(Alt)* Google Places (paid after free quota).

**My API:**
- Auth, dog profiles, posts, comments/likes.
- Store parks (by ext_id), simple geo search (by city or lat/lng).

**MVP:**
- Sign up/login.
- Create dog profile (name, breed, photo).
- Feed of nearby posts (by city).
- Park map with recent posts.

**Stretch:**
- Availability windows / schedule playdates.
- Real-time chat on posts.
- Park leaderboards.

**Data model:**
- `users`, `dogs(user_id, name, breed, age, photo_url)`
- `parks(id, ext_id, name, lat, lng, source)`
- `posts(id, dog_id, park_id, text, photo_url, created_at)`
- `likes`, `comments`

**Risks:**
- Location perms → start with city search; GPS later.
- API quotas → use OpenTripMap first.

---

## Idea #3 — TripTiles (Weekend Trip Planner)
**One-liner:** Pick a city; drag top POIs into a 2-day plan; see weather and share.  
**Users/Problem:** Quick lightweight trip plan without overcomplicating.

**External APIs:**
- **OpenTripMap** (places/POIs) – https://opentripmap.io/product  
- **OpenWeather** (forecast) – https://openweathermap.org/api

**My API:**
- Auth; `trips` with dates/city; `trip_pois` (day/order); share slug.

**MVP:**
- Search city → list POIs by category.
- Drag into Day 1/Day 2 board; save trip.
- Show 5-day weather for dates.
- Public share link (read-only).

**Stretch:**
- Route order optimization (nearest neighbor).
- Export .ics to Calendar.
- Collaborative editing.

**Data model:**
- `users`
- `trips(id, user_id, city, start_date, end_date, share_slug)`
- `trip_pois(trip_id, day, order, ext_id, name, lat, lng, category)`
- `notes(trip_id, text)`

**Risks:**
- City/geo quirks → use OpenTripMap geonames.
- Timezones/dates → normalize in UTC.

---

## Ranking (today)
1) **CineLog** — quickest & most reliable API (TVMaze, no key).  
2) TripTiles — great demo value (maps + weather).  
3) PawPals — most social features (more time).

---

## Questions for my mentor
- Is TVMaze acceptable for Step-3, or should I consider TMDB instead?
- For MVP, should I cache TVMaze responses server-side from day 1?
- Any scope cuts you’d recommend to keep it within ~60 hours?

