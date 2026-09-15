# 03. Web Architecture Specification (UK Level 3 Computing)
## British Broadcasting Corporation

### Architectural Model
The BBC operates one of the world’s most sophisticated cloud-native web architectures. Built upon its internal Global Experience Language (GEL) and micro-frontends framework, the platform decouples editorial content authoring from multi-channel delivery using headless CMS engines, AWS multi-region infrastructure, and geo-distributed CDN caching.

### Data Pipeline Flow
Editorial staff publish through the CPS/Nitro CMS -> Stored in DynamoDB & Aurora -> GraphQL Federation layer aggregates entities -> Edge CDN nodes (Fastly & Akamai) cache HTML/JSON -> Client browser receives semantic HTML5 + modern CSS/JS bundles with progressive enhancement.

### Detailed Component Analysis (Grading Criteria)
---------------------------------------------------------
### 1. HTML5 Semantic Markup & Accessibility
- **Technology Stack:** Semantic HTML5, WAI-ARIA 1.2, Microdata schema.org
- **Technical Role & Function:** HTML forms the foundational skeleton of every BBC webpage. The BBC uses strict semantic tags such as <header>, <nav>, <main>, <article>, <section>, and <footer>. This semantic clarity ensures that assistive technologies (screen readers like JAWS, NVDA, and VoiceOver) can interpret page hierarchies effortlessly. Headings (H1 to H6) follow strict sequential numbering without skipping levels.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Semantic HTML improves search engine ranking (SEO) by defining meaningful content boundaries, enables web crawlers to categorize news topics, and complies with UK Public Sector Bodies Accessibility Regulations (PSBAR 2018).

```
<article class="bbc-story" role="article" aria-labelledby="story-head-104">
  <h2 id="story-head-104">UK Inflation Drops to Target</h2>
  <time datetime="2026-09-15T08:30:00Z">15 September 2026</time>
  <p>Consumer prices index figures released this morning show...</p>
</article>
```

---------------------------------------------------------
### 2. Cascading Style Sheets (CSS3 & GEL)
- **Technology Stack:** CSS3, Modern CSS Grid, Flexbox, BBC Global Experience Language (GEL)
- **Technical Role & Function:** CSS controls typography, visual hierarchy, colour contrast, and responsive layout across phones, tablets, smart TVs, and desktop monitors. The BBC GEL framework enforces mathematical font sizing (BBC Reith font family), consistent 8px spatial grid gutters, and strict 4.5:1 minimum colour contrast ratios to guarantee readability.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Fluid CSS Grid and media queries (@media (min-width: 768px)) ensure responsive reflow without horizontal scrolling, reducing mobile bounce rates and data consumption.

```
@media (min-width: 1024px) {
  .news-grid {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
    gap: 1.5rem;
  }
}
```

---------------------------------------------------------
### 3. JavaScript & Progressive Enhancement
- **Technology Stack:** TypeScript, ES2022, React, Server-Side Rendering (SSR), Web Workers
- **Technical Role & Function:** JavaScript powers interactive components such as the iPlayer video scrubber, live score tickers, navigation drawers, and weather search autocomplete. Crucially, the BBC follows the "Progressive Enhancement" philosophy: core news text is pre-rendered on the server so if a user has JavaScript disabled or suffers a slow connection, the article remains fully readable.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Code-splitting and tree-shaking prevent oversized JS bundles, keeping initial Time-to-Interactive (TTI) under 2.0 seconds over 4G mobile networks.

```
// Asynchronous live-ticker polling with fallback
async function fetchBreakingTicker() {
  try {
    const response = await fetch("/api/v2/news/ticker", { headers: { "Accept": "application/json" } });
    const data = await response.json();
    updateTickerDOM(data.headline);
  } catch (err) {
    console.warn("Fallback to static headline", err);
  }
}
```

---------------------------------------------------------
### 4. Application Programming Interfaces (APIs)
- **Technology Stack:** RESTful JSON APIs, GraphQL Apollo Federation, WebSocket Streams
- **Technical Role & Function:** APIs connect the user-facing web applications to back-end editorial databases and media streaming encoders. The BBC uses GraphQL federation to allow front-end components to query only the precise fields needed (e.g. headline, thumbnail, publishedTimestamp), eliminating over-fetching.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Rate limiting and API gateway throttling prevent denial-of-service spikes during major breaking news broadcasts (e.g., General Elections).

```
query GetArticle($id: ID!) {
  newsArticle(id: $id) {
    headline
    summary
    mediaUrl
    byline
  }
}
```

---------------------------------------------------------
### 5. Content Management System (CMS)
- **Technology Stack:** BBC CPS (Content Production System), Headless Nitro Editorial Engine
- **Technical Role & Function:** Journalists in London, Salford, Glasgow, and worldwide author articles, audio clips, and video bulletins within CPS. The CMS validates editorial standards, spelling, copyright licensing, and legal defamation checks before pushing articles to the publishing pipeline.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Decoupled (headless) CMS architecture separates the authoring UI from presentation servers, preventing editorial bottlenecks from affecting public-facing web uptime.

```
CMS Workflow: Author Draft -> Senior Editor Review -> Legal Compliance Flag -> Digital Asset Metadata Tagging -> Automated Publish to S3 & CDN Flush.
```

---------------------------------------------------------
### 6. Database Layer & Data Storage
- **Technology Stack:** Amazon Aurora PostgreSQL, AWS DynamoDB, Redis Cluster, Amazon S3
- **Technical Role & Function:** The BBC uses a polyglot persistence strategy. Structured relational data (user accounts, permissions, TV licence validation) resides in Amazon Aurora PostgreSQL with multi-Availability Zone replication. High-velocity unstructured data (live article comments, telemetry, programme metadata) is stored in Amazon DynamoDB for millisecond key-value retrieval, backed by Redis in-memory caching.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: ACID compliance in PostgreSQL guarantees that account data cannot become corrupted during simultaneous transactions.

```
SELECT user_id, licence_status, expiry_date FROM bbc_accounts WHERE email_hash = $1;
```

---------------------------------------------------------
### 7. Cloud Hosting & Infrastructure
- **Technology Stack:** Amazon Web Services (AWS eu-west-1 & eu-west-2), Kubernetes (EKS)
- **Technical Role & Function:** Rather than maintaining physical on-premise servers for web delivery, the BBC hosts its digital estate in AWS multi-region infrastructure. Containerised microservices run inside Amazon Elastic Kubernetes Service (EKS) pods that automatically scale up or down based on incoming CPU load and network traffic.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Auto-scaling allows the BBC to surge from 10,000 requests per second to 250,000 requests per second within three minutes during major national events.

```
Kubernetes HPA (Horizontal Pod Autoscaler): MinReplicas: 20, MaxReplicas: 400, TargetCPUUtilization: 65%.
```

---------------------------------------------------------
### 8. Content Delivery Network (CDN) & Edge Caching
- **Technology Stack:** Fastly & Akamai Multi-CDN Architecture, Anycast DNS, Varnish (VCL)
- **Technical Role & Function:** The BBC utilizes a multi-CDN strategy combining Fastly and Akamai. When a user requests an article or an iPlayer video segment, the request is routed to the geographically nearest CDN Point of Presence (PoP) in London, Manchester, Edinburgh, or Belfast. 95%+ of all requests are served directly from cache memory.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: CDNs absorb distributed denial-of-service (DDoS) volumetric attacks before malicious requests can touch the BBC origin servers.

```
HTTP/2 200 OK
Cache-Control: public, max-age=60, stale-while-revalidate=300
X-Cache: HIT from Fastly-LHR
```

---------------------------------------------------------
### 9. HTTPS, SSL/TLS 1.3 & Transport Security
- **Technology Stack:** TLS 1.3, Strict Transport Security (HSTS), Perfect Forward Secrecy
- **Technical Role & Function:** Every connection to bbc.co.uk is enforced over HTTPS using TLS 1.3 encryption. HTTP Strict Transport Security (HSTS) with preloading in modern browsers instructs the browser to never communicate over unencrypted HTTP, preventing Man-in-the-Middle (MitM) eavesdropping.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Encryption protects users reading sensitive political, health, or investigative journalism from being monitored by third parties on open Wi-Fi networks.

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Content-Security-Policy: default-src 'self' https://*.bbc.co.uk;
```

---------------------------------------------------------
### 10. User Authentication & BBC ID
- **Technology Stack:** BBC Account Identity Service, OAuth 2.0, OpenID Connect (OIDC), JWT
- **Technical Role & Function:** BBC ID allows users to sign in, verify their TV Licence declaration, save programmes to their iPlayer watchlist, and resume podcast playback across devices. Passwords are salted and hashed using Argon2/bcrypt algorithms. JSON Web Tokens (JWT) manage short-lived stateless sessions.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Token-based stateless authentication allows millions of users to remain authenticated across auto-scaled microservices without central session database locks.

```
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJiYmMtdXNlci04NDkyMSIsImV4cCI6MTgwMDAwMDAwMH0...
```

---------------------------------------------------------
### 11. System Monitoring, Telemetry & Observability
- **Technology Stack:** Datadog, Prometheus, Grafana, CloudWatch, Synthetic User Monitors
- **Technical Role & Function:** BBC engineering teams monitor thousands of metrics per second: HTTP 5xx error rates, page load latency (LCP/FID/CLS), video buffer ratios, and database connection pools. Automated synthetic probes test login and playback every 60 seconds from external UK internet providers.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Real-time alerting via PagerDuty notifies on-call DevOps engineers when error rates exceed 0.05% of total traffic.

```
Alert Rule: IF rate(http_requests_total{status=~"5.."}[2m]) / rate(http_requests_total[2m]) > 0.005 THEN alert_oncall_engineer();
```

---------------------------------------------------------
### 12. Disaster Recovery & Automated Backup Strategy
- **Technology Stack:** Multi-Region Active-Active Replication, Automated Daily Snapshots, RTO/RPO
- **Technical Role & Function:** To satisfy public service broadcasting resilience mandates, BBC data is replicated continuously between AWS London (eu-west-2) and AWS Ireland (eu-west-1). If an entire cloud data centre experiences catastrophic failure, automated DNS failover redirects traffic within 30 seconds. Database snapshots are stored in immutable write-once-read-many (WORM) S3 buckets.
- **BTEC Level 3 Assessment Focus:** Level 3 Assessment Note: Recovery Time Objective (RTO) is under 5 minutes for core news services, and Recovery Point Objective (RPO) is zero data loss for news copy.

```
RTO: < 5 mins | RPO: < 10 seconds | Cross-Region Automated Sync with Point-in-Time Database Recovery (PITR).
```

