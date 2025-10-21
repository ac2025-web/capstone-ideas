# Capstone – Initial Project Ideas (Step 1)

This document proposes three project ideas and explores candidate APIs.  
The final API choice will be made in Oct 21.  
**Target effort:** ~45–65 hours.

---

## How I Evaluated APIs (brief)

- Free tier / simple API key process  
- JSON format with good CORS support  
- Reliable documentation and uptime  
- Entities match my project needs  
- Simple rate limits or easy caching  

---

## 🥇 Idea #1 — CineLog 🎬 (TV / Movie Watchlist) — *easiest to ship*

**One-liner:** Track TV & movies with statuses, ratings, and notes; “Continue Watching” view.  
**Users / Problem:** People forget what they’re watching and want one clean list to track viewing progress.

**External API:**  
- **TVMaze** – https://www.tvmaze.com/api (no key)  
  - Search shows: `GET /search/shows?q=<query>`  
  - Show details: `GET /shows/<id>`  
  - Episodes: `GET /shows/<id>/episodes`

**My API (Express + PostgreSQL):**  
- Auth (register / login)  
- My List (status = planned | watching | completed, rating 1–5, notes)  
- Optional caching for TVMaze API responses  

**MVP (Must-Have):**
- Sign up / login  
- Search via TVMaze → add to My List  
- Update status / rating / notes  
- “Continue Watching” section  
- Responsive UI  

**Stretch Goals:**
- Episode guide + mark last watched  
- Simple recommendations by genre  
- Public profile / follow friends  
- Import / export CSV  

**Data Model (Sketch):**
users(id, email, password_hash, created_at)
titles(id, ext_api, ext_id, name, type, poster_url, created_at)
user_titles(user_id, title_id, status, rating, notes, last_episode_id, updated_at)
(optional) episodes(id, title_id, season, number, name, airdate)


**Risks & Mitigations:**
- Rate limits → server-side cache  
- HTML in summaries → sanitize  
- Scope creep → MVP first  

---

## 🥈 Idea #2 — MealMate 🍳 (Recipe Finder & Meal Planner)

**One-liner:** Search recipes by ingredient and create a personal weekly meal plan.  
**Users / Problem:** Busy people want to quickly find recipes and plan their meals without juggling multiple apps.

**External API:**  
- **TheMealDB** – https://www.themealdb.com/api.php (free + no key)  
  - Search by ingredient: `GET /filter.php?i=<ingredient>`  
  - Get recipe details: `GET /lookup.php?i=<meal_id>`  
  - Filter by category or area  

**My API (Express + MongoDB):**  
- Auth (register / login)  
- User’s weekly meal plan (day + recipe_id + notes)  
- Favorites collection  
- Optional grocery list generator  

**MVP (Must-Have):**
- Search recipes by ingredient or keyword  
- View recipe details (ingredients, instructions, image)  
- Add to meal plan (Mon–Sun)  
- Save favorites  

**Stretch Goals:**
- Generate grocery list automatically  
- Filter by calories / cuisine type  
- Drag-and-drop planner UI  
- Share meal plans with friends  

**Data Model (Sketch):**
users(id, email, password_hash)
meals(id, ext_id, name, category, area, thumbnail)
mealplans(user_id, meal_id, day, notes)
favorites(user_id, meal_id)


**Risks & Mitigations:**
- Inconsistent recipe fields → validate data  
- API downtime → store minimal recipe cache  
- Too many features → restrict to weekly planning MVP  

---

## 🥉 Idea #3 — TripTiles 🗺️ (Weekend Trip Planner)

**One-liner:** Pick a city, drag top POIs into a 2-day plan, view weather, and share.  
**Users / Problem:** Travelers want a quick way to plan short trips without complex tools.

**External APIs:**  
- **OpenTripMap** (places/POIs) – https://opentripmap.io/product  
- **OpenWeather** (forecast) – https://openweathermap.org/api  

**My API (Express + PostgreSQL):**  
- Auth, trips (city + date), trip_pois (day/order), public share slug  

**MVP (Must-Have):**
- Search city → get POIs  
- Drag POIs into Day 1 / Day 2 board  
- Save trip plan  
- Show weather for trip dates  
- Public share link (read-only)  

**Stretch Goals:**
- Optimize route order (nearest neighbor)  
- Export .ics (calendar)  
- Collaborative trip editing  
- Google Maps integration for routes  

**Data Model (Sketch):**
users(id, email, password_hash)
trips(id, user_id, city, start_date, end_date, share_slug)
trip_pois(trip_id, day, order, ext_id, name, lat, lng, category)
notes(trip_id, text)


**Risks & Mitigations:**
- City/geo quirks → use OpenTripMap geonames  
- Weather delay / API error → handle gracefully  
- Timezone differences → normalize in UTC  

---


## Questions for My Mentor

- Is TVMaze acceptable for Step 3, or should I consider TMDB instead?  
- Would TheMealDB’s free tier be sufficient for basic meal planning?  
- Should I start with one API first (TVMaze or TheMealDB) for faster MVP delivery?  
- Any scope adjustments recommended to keep it under ~60 hours?
