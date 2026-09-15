# 02. Digital Services Catalogue & Technical Delivery
## British Broadcasting Corporation

### 1. BBC News Multi-Platform
- **Category:** Information & Journalism
- **Target Audience:** UK public and global readership (over 450m weekly users).
- **Description:** Continuous 24-hour breaking news, regional UK reports, live text commentaries, and impartial verified reporting.
- **Technical Delivery Pipeline:** Distributed microservices architecture delivering dynamic JSON feeds cached across Tier-1 CDN edge servers in sub-50ms latency.

### 2. BBC iPlayer Video-on-Demand
- **Category:** Streaming Media
- **Target Audience:** UK licence-fee payers accessing via web browsers, smart TVs, mobile apps, and consoles.
- **Description:** Ultra HD 4K, HDR, and live TV streaming of BBC One, Two, Three, Four, and extensive boxsets with parental controls and TV Licence verification.
- **Technical Delivery Pipeline:** Adaptive Bitrate Streaming (ABR) using HLS and MPEG-DASH formats with DRM encryption and edge-cached video segments.

### 3. BBC Sounds Audio Platform
- **Category:** Audio & Podcasts
- **Target Audience:** Audio listeners seeking news, drama, comedy, and music curation.
- **Description:** Live broadcast of 10 national radio stations, 40+ local stations, on-demand podcasts, curated music mixes, and offline downloads.
- **Technical Delivery Pipeline:** Real-time AAC/MP3 audio stream ingestion through AWS Elemental MediaStore with GraphQL metadata aggregation.

### 4. BBC Bitesize Educational Portal
- **Category:** Education & Learning
- **Target Audience:** UK primary, secondary students, teachers, and home-educating parents.
- **Description:** Curriculum-aligned revision materials, interactive quizzes, video tutorials, and study aids for KS1, KS2, KS3, GCSE, and National 5 students.
- **Technical Delivery Pipeline:** Accessible static-generated HTML5 modules with interactive React assessment engines and high WCAG 2.2 AA accessibility scoring.

### 5. BBC Sport Real-Time Centre
- **Category:** Sports & Live Telemetry
- **Target Audience:** Sports fans wanting instant, verified match updates.
- **Description:** Live scores, football tables, Premier League match commentaries, Formula 1 telemetry, and video highlights packages.
- **Technical Delivery Pipeline:** Low-latency WebSocket connections and Server-Sent Events (SSE) pushing instant score changes directly to client viewports.

### 6. BBC Weather Forecast Service
- **Category:** Meteorological Data
- **Target Audience:** General public and UK emergency planning authorities.
- **Description:** Pinpoint hourly UK and international weather forecasts, rain radars, UV warnings, and severe weather alert banners.
- **Technical Delivery Pipeline:** Ingestion of Met Office gridded meteorological API data parsed into spatial GeoJSON layers and rendered via client-side vector charts.

