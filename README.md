# Netflix Titles Analysis

End-to-end analysis of 8,807 Netflix titles (movies and TV shows) built with PostgreSQL and Power BI.
This project uses the **Netflix Movies and TV Shows** dataset from Kaggle.

- **Source:** [Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows) by Shivam Bansal
## What's Inside

- **SQL** - data cleaning, view creation, and business-question queries
- **Power BI** - interactive dashboards covering catalog growth, content mix, genre, duration, and ratings
- **Executive Report** - a narrative-style page summarizing the catalog for a non-technical audience
- **Key Insights & Recommendations** - findings paired with concrete, actionable recommendations

## Tech Stack

PostgreSQL · SQL · Power BI · DAX

## Setup / Installation

### 1. Prerequisites
- PostgreSQL (local instance or any Postgres-compatible server)
- Power BI Desktop (Windows only - free download from Microsoft)

### 2. Installation Steps
1. Create a new database in PostgreSQL and run `Data cleaning.sql` first - this creates the `netflix_analysis` schema, builds the `netflix_titles` table, and cleans the raw data (standardizing text casing, replacing blanks/nulls with `'unknown'`, and fixing the `release_year` formatting).
2. Run `analysis and business questions.sql` next - this answers the core business questions and creates four views (`vw_netflix_titles`, `vw_countries`, `vw_director`, `vw_periods`) that the Power BI report reads from.
3. Open `netflix_power_bi.pbix` in Power BI Desktop.
   - **Offline / view-only:** just open the file - Power BI caches the last-refreshed data inside the `.pbix`, so all dashboards and visuals display immediately with no database connection needed.
   - **Online / live connection:** to pull live data instead of the cached snapshot, go to **Home → Transform Data → Data Source Settings**, point the PostgreSQL connection at your own database, then click **Refresh**.

## Quick Summary

| | |
|---|---|
| **Records** | 8,807 titles |
| **Type split** | 69.6% Movies / 30.4% TV Shows |
| **Countries represented** | 87 |
| **Peak catalog growth year** | 2019 (title additions peaked, then declined for two straight years) |
| **Longest movie** | 312 minutes |
| **Average movie duration** | 99.58 minutes |

## Workflow
The project follows a clean → model → analyze → visualize pipeline.

1. **Data Cleaning (SQL)** - Standardized text casing, replaced blank/null values with `'unknown'` across `director`, `cast`, `country`, `rating`, and `duration`, and fixed a data-entry issue where some `release_year` values had been stored with commas.
2. **Analytical Views (SQL)** - Built a fact view (`vw_netflix_titles`) plus three supporting views (`vw_countries`, `vw_director`, `vw_periods`) so Power BI reads from clean, purpose-built views instead of the raw table.
3. **Business-Question Queries (SQL)** - Answered specific questions directly in SQL: content mix, top genres, top countries, ratings breakdown, average time-to-add, seasonality, and decade-by-decade release counts.
4. **Dashboarding (Power BI + DAX)** - Built two dashboard pages covering catalog composition, growth trend, genre/director breakdown, and duration patterns.
5. **Executive Report** - Wrote a plain-language summary of the catalog for a non-technical audience.
6. **Key Insights & Recommendations** - Paired each key finding with a concrete recommendation for the business.

## Example Queries - Business Questions Answered in SQL

**Content Mix**
*Is Netflix's catalog movie-first or series-first?*
→ Counts titles grouped by `type` — 69.6% Movies, 30.4% TV Shows.

**Top Genres**
*Which genres does Netflix invest in most heavily?*
→ Groups by `listed_in`, ranked by count, then narrowed to the top 3 for a quick headline view.

**Most Frequent Cast**
*Which actors appear across the most titles?*
→ Groups by `cast`, ranked by appearance count, limited to the top 10.

**Most Popular Content by Country**
*Where is content concentrated, and does that differ by type (Movie vs. TV Show)?*
→ Groups by `country` and `type` together, ranked by count.

**Director Output**
*Which directors have the largest footprint on the platform?*
→ Groups by `director`, counting how many titles each has contributed.

**Oldest and Newest Content**
*What's the full time range the catalog spans?*
→ Simple `MIN`/`MAX` on `release_year`.

**Titles per Decade**
*How is the catalog distributed across different eras of film/TV?*
→ Buckets `release_year` into 10-year decades and counts titles in each.

**Unique Countries**
*How geographically diverse is the catalog?*
→ `COUNT(DISTINCT country)` — the basis for the 87-countries figure used throughout the dashboards.

## Dashboards

### Main Dashboard — Catalog Composition, Growth & Ratings
![alt text](https://github.com/JaysonJob/Netflix-titles-analysis/blob/47ffc5e9ae2de5411917882618cd68f9f71cc417/analysis%20dashboard.png)

The catalog totals 8,807 titles, split roughly 70/30 between Movies (69.62%) and TV Shows (30.38%). Titles are sourced from 87 countries. Catalog growth climbed steadily from 2008, accelerated sharply after 2014, peaked around 2019 at just over 2,000 titles added, and has declined for two consecutive years since. Dramas, Documentaries, and Stand-Up Comedy are the most represented genres, and TV-MA is by far the most common content rating, followed by TV-14 - confirming the catalog skews toward mature audiences.

### Dashboard 2 - Duration & Seasonal Patterns
![alt text](https://github.com/JaysonJob/Netflix-titles-analysis/blob/ac97cde980fa7e75271ef5d6ccf882cf2dfe61f3/dashboard%202.png)

The longest movie in the catalog runs 312 minutes, with an average movie duration of 99.58 minutes - close to a typical feature-length runtime. Content additions by month show a consistent pattern across the year for both Movies and TV Shows, with no single month dramatically outpacing the others. Looking at director output by genre, Dramas & International, Documentaries, and Stand-Up Comedy account for the highest director counts, reinforcing those as Netflix's most heavily produced genres.

### Executive Report
![alt text](https://github.com/JaysonJob/Netflix-titles-analysis/blob/71a75315607088b47f3b2903b95870a9d6869a7e/executive%20report.png)

This report looks at what's on Netflix and how it's growing, covering total titles, content types, countries of origin, viewer ratings, and genres. Netflix has 8,807 titles, about 70% of them movies, sourced from 87 countries - though nearly half of all titles come from just the United States and India. Growth peaked in 2019 at 2,016 titles, then dropped for two years straight to 1,498 in 2021. TV-MA and TV-14 make up more than 60% of all content, showing a lean toward older teens and adults, while family and niche genres are comparatively thin. The report flags a clear opportunity to investigate the recent growth slowdown, and to assess whether the current U.S./India-heavy, mature-rated content mix aligns with plans to reach more international and family viewers.

### Key Insights & Recommendations
![alt text](https://github.com/JaysonJob/Netflix-titles-analysis/blob/7acf15fe9e462d12b41d5d60004d83fc2df49f22/insights%20and%20recomendations.png)

**Key Insights:**
1. Netflix added the most titles in 2019 (2,016), but additions dropped every year after — down to 1,879 in 2020 and 1,498 in 2021.
2. The catalog is movie-heavy (70% Movies vs. 30% TV Shows).
3. Content comes from 87 countries, but the U.S. and India alone produce nearly half of all titles.
4. Over 60% of titles are rated for mature audiences (TV-MA and TV-14).
5. International Movies, Dramas, and Comedies dominate the genre mix, while niche genres are scarce.

**Recommendations:**
1. **Figure out why new titles keep dropping** - determine whether the 2019 peak and subsequent decline was intentional or driven by production delays before treating it as a warning sign.
2. **Put more investment into TV shows** - if series drive longer watch time than movies, shifting some budget from movies to shows could pay off.
3. **Get more content from other countries** - heavy reliance on the U.S. and India may limit growth in other markets without more local content.
4. **Add more content for families and kids** - with over 60% of the catalog rated for mature audiences, family-focused growth will need a stronger kids' content pipeline.
5. **Check what people are actually searching for** - before investing in underrepresented genres, validate real subscriber demand rather than assuming a gap needs filling.

---


