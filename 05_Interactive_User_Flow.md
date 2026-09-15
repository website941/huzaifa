# 05. User Experience Flow Architecture
## British Broadcasting Corporation - Scenario: BBC iPlayer Programme Playback & TV Licence Verification Flow

**Objective:** Map the complete technical journey of an audience member searching for a documentary, authenticating their BBC Account, verifying licence eligibility, and establishing an encrypted adaptive bitrate video stream.

### End-to-End Step-by-Step Architecture
#### Step 1: User Lands on BBC iPlayer & Enters Search Query
- **Actor:** User
- **User Action:** Navigates to bbc.co.uk/iplayer and searches for "Planet Earth III" in the search bar.
- **Technical Execution & API Responses:** Browser sends client-side debounced GET request to /api/v1/search?q=planet+earth with auto-suggest JSON response.
- **Exception Handling Path:** Zero search results returns curated trending programmes and spelling correction hints.

#### Step 2: Frontend Requests Programme Metadata
- **Actor:** Client Frontend
- **User Action:** Transmits GraphQL query to the BBC Apollo Federation Gateway.
- **Technical Execution & API Responses:** GraphQL fetches programme UUID, synopsis, age rating (12), 4K video asset URLs, and subtitle tracks from Nitro CMS repository.


#### Step 3: BBC ID Authentication & TV Licence Verification Check
- **Actor:** Backend Service
- **User Action:** Checks for active session JWT token and verifies TV Licence declaration status.
- **Technical Execution & API Responses:** If no valid token is found, user is redirected to /account/signin with returnUrl query parameter. Postgres database checks user record.
- **Exception Handling Path:** If unauthenticated, user is prompted to sign in or create an account; playback paused.

#### Step 4: DRM Token Issuance & Geo-IP Location Validation
- **Actor:** API Gateway
- **User Action:** Validates UK IP address via MaxMind GeoIP database and issues Widevine/FairPlay DRM license key.
- **Technical Execution & API Responses:** Confirms client IP originates from the United Kingdom to comply with rights licensing. Issues encrypted session token.
- **Exception Handling Path:** If non-UK IP detected, displays informative "BBC iPlayer only works in the UK due to rights agreements" modal.

#### Step 5: Adaptive Bitrate Manifest Ingestion & Video Playback
- **Actor:** Database / External
- **User Action:** Client HTML5 video player requests m3u8 (HLS) or mpd (DASH) manifest from Fastly CDN.
- **Technical Execution & API Responses:** Player measures available bandwidth every 2 seconds, dynamically adjusting bitrate between 1080p, 4K, and 720p chunks to eliminate buffering.


#### Step 6: Telemetry & Viewing Position Save to Redis
- **Actor:** Backend Service
- **User Action:** Background beacon periodically transmits playback timestamp back to BBC Account service.
- **Technical Execution & API Responses:** Saves resume timestamp (e.g., 24m:15s) into Redis cache so user can continue seamlessly on smart TV.


