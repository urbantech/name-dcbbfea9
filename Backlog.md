# Agile Backlog

**Generated**: 2025-12-10T03:34:10.685959
**Total Epics**: 6
**Total Stories**: 18
**Total Points**: 114
**Estimation Scale**: fibonacci
**SSCS Compliance**: v2.0
**TDD Workflow**: RED → GREEN → REFACTOR
**Branch Naming**: `feature/{issue-number}-{slug}`


## Epic #1000: Authentication & User Management

Implement secure user authentication, authorization, and account management for DJs and admins with JWT-based tokens and role-based access control

### User Stories

#### Story #1001: As a DJ, I want to register an account so that I can access the streaming platform

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement user registration endpoint with email/password validation, password hashing, and JWT token generation. Store user data securely in PostgreSQL with encrypted sensitive fields.

**Acceptance Criteria**:
1. Given a user provides valid email and password, When they submit registration form, Then account is created with hashed password and JWT tokens are returned
2. Given a user provides invalid email format, When they submit registration, Then a 400 Bad Request error is returned with validation details
3. Given a user provides weak password, When they submit registration, Then a 400 Bad Request error is returned with password requirements
4. Given a user provides duplicate email, When they submit registration, Then a 409 Conflict error is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for user registration endpoint`
- 🟢 GREEN: `green: user registration with JWT and password hashing`
- 🔵 REFACTOR: `refactor: extract password validation and token generation utilities`

**Branch**: `feature/1001-user-registration`

**Labels**: user-story, feature, authentication

#### Story #1002: As a DJ, I want to login to my account so that I can manage my streams and content

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement login endpoint with email/password authentication, JWT token generation (access + refresh), and rate limiting to prevent brute force attacks.

**Acceptance Criteria**:
1. Given a user provides valid credentials, When they submit login form, Then access and refresh tokens are returned with 200 OK
2. Given a user provides invalid credentials, When they submit login, Then a 401 Unauthorized error is returned
3. Given a user attempts login 5 times with wrong password, When they try again, Then rate limiting blocks further attempts for 15 minutes
4. Given a user has valid refresh token, When they request token refresh, Then new access token is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for login and token refresh`
- 🟢 GREEN: `green: JWT authentication with access and refresh tokens`
- 🔵 REFACTOR: `refactor: add rate limiting middleware and improve error handling`

**Branch**: `feature/1002-user-login`

**Labels**: user-story, feature, authentication

#### Story #1003: As a DJ, I want role-based access control so that only I can manage my content

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement RBAC middleware with JWT scope validation to enforce permissions (streams:read, streams:write, playlists:read, playlists:write, etc.) and resource ownership checks.

**Acceptance Criteria**:
1. Given a DJ has valid JWT with 'streams:write' scope, When they create a stream, Then the stream is created successfully
2. Given a DJ has valid JWT without 'streams:write' scope, When they attempt to create stream, Then 403 Forbidden is returned
3. Given a DJ attempts to modify another DJ's stream, When they submit update request, Then 403 Forbidden is returned
4. Given an admin user, When they access any resource, Then full access is granted

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for RBAC middleware and scope validation`
- 🟢 GREEN: `green: JWT scope validation and resource ownership checks`
- 🔵 REFACTOR: `refactor: extract authorization decorators and improve permission logic`

**Branch**: `feature/1003-rbac-authorization`

**Labels**: user-story, feature, authentication, authorization


## Epic #2000: Live Streaming (Shoutcast Relay)

Enable DJs to register Shoutcast sources, relay live audio feeds through cloud infrastructure, segment streams into HLS chunks, and serve to listeners with real-time metadata

### User Stories

#### Story #2001: As a DJ, I want to register my Shoutcast source so that my live stream can be relayed to listeners

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement POST /v1/streams endpoint to register Shoutcast URL, mount, and credentials. Validate connection to Shoutcast source and spin up Relay Worker Pod in Kubernetes to begin ingesting and segmenting the feed.

**Acceptance Criteria**:
1. Given a DJ provides valid Shoutcast URL and credentials, When they register stream, Then stream record is created and Relay Worker Pod is started
2. Given a DJ provides invalid Shoutcast URL, When they register stream, Then 422 Unprocessable Entity is returned with connection error
3. Given a DJ provides missing required fields, When they register stream, Then 400 Bad Request is returned with validation errors
4. Given a stream is registered, When Relay Worker connects to Shoutcast, Then HLS segments begin generating within 30 seconds

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for Shoutcast stream registration`
- 🟢 GREEN: `green: stream registration endpoint with Kubernetes Pod orchestration`
- 🔵 REFACTOR: `refactor: extract Shoutcast validation and Pod creation logic`

**Branch**: `feature/2001-shoutcast-registration`

**Labels**: user-story, feature, live-streaming

#### Story #2002: As a DJ, I want to view real-time metadata for my live stream so that I can monitor current track and listener count

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement GET /v1/streams/{stream_id}/status endpoint to fetch current track, bitrate, listener count, and relay connection status from Shoutcast admin API and Relay Worker heartbeat.

**Acceptance Criteria**:
1. Given a live stream is active, When DJ requests status, Then current track, bitrate, listeners, and uptime are returned
2. Given a stream's Relay Worker is offline, When DJ requests status, Then 503 Service Unavailable is returned
3. Given a stream does not exist, When DJ requests status, Then 404 Not Found is returned
4. Given a private stream, When unauthorized user requests status, Then 403 Forbidden is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for live stream status endpoint`
- 🟢 GREEN: `green: fetch metadata from Shoutcast admin and Relay Worker`
- 🔵 REFACTOR: `refactor: add caching for metadata and improve error handling`

**Branch**: `feature/2002-stream-status`

**Labels**: user-story, feature, live-streaming

#### Story #2003: As a listener, I want to access HLS manifest for live streams so that I can play audio in my browser or app

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement GET /v1/streams/{stream_id}/live.m3u8 endpoint to serve HLS manifest generated by Relay Worker. Enforce authentication for private streams and subscription checks for monetized streams. Cache manifest at CDN with 30s TTL.

**Acceptance Criteria**:
1. Given a public live stream, When listener requests HLS manifest, Then manifest with TS segment URLs is returned
2. Given a private stream without authentication, When listener requests manifest, Then 403 Forbidden is returned
3. Given a monetized stream without active subscription, When listener requests manifest, Then 403 Forbidden is returned
4. Given a stream does not exist, When listener requests manifest, Then 404 Not Found is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for HLS manifest endpoint with auth`
- 🟢 GREEN: `green: serve HLS manifest with authentication and subscription checks`
- 🔵 REFACTOR: `refactor: add CDN caching and improve manifest generation`

**Branch**: `feature/2003-hls-manifest`

**Labels**: user-story, feature, live-streaming


## Epic #3000: Audio File Upload & On-Demand Playlists

Allow DJs to upload MP3 files directly, transcode to HLS segments, create on-demand playlists with uploaded or external tracks, and serve HLS/MP3 streams to listeners

### User Stories

#### Story #3001: As a DJ, I want to upload audio files so that I can use them in playlists and podcasts

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement POST /v1/uploads/audio endpoint with multipart/form-data support. Validate file type (audio/mpeg), size limit (100MB), perform virus scan, store in S3, and enqueue background transcoding job to generate HLS segments.

**Acceptance Criteria**:
1. Given a DJ uploads valid MP3 file, When upload completes, Then file is stored in S3 and transcoding job is queued
2. Given a DJ uploads non-MP3 file, When upload is attempted, Then 400 Bad Request is returned
3. Given a DJ uploads file larger than 100MB, When upload is attempted, Then 413 Payload Too Large is returned
4. Given transcoding completes, When DJ checks upload status, Then status is 'ready' with HLS manifest URL

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for audio file upload endpoint`
- 🟢 GREEN: `green: multipart upload with S3 storage and transcoding queue`
- 🔵 REFACTOR: `refactor: extract file validation and virus scanning logic`

**Branch**: `feature/3001-audio-upload`

**Labels**: user-story, feature, file-upload

#### Story #3002: As a DJ, I want to create on-demand playlists so that listeners can stream my curated tracks

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement POST /v1/playlists endpoint to create playlists with tracks from uploads or external URLs. Validate track sources, generate HLS manifest, and provide HLS/MP3 streaming endpoints.

**Acceptance Criteria**:
1. Given a DJ provides valid tracks (uploads and external URLs), When playlist is created, Then playlist record is stored and HLS manifest is generated
2. Given a DJ references non-existent upload_id, When playlist is created, Then 422 Unprocessable Entity is returned
3. Given a DJ provides invalid external URL, When playlist is created, Then 422 Unprocessable Entity is returned
4. Given a playlist is created, When listener requests HLS manifest, Then manifest with all track segments is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for playlist creation endpoint`
- 🟢 GREEN: `green: playlist creation with track validation and HLS generation`
- 🔵 REFACTOR: `refactor: extract track validation and manifest generation utilities`

**Branch**: `feature/3002-playlist-creation`

**Labels**: user-story, feature, playlists

#### Story #3003: As a listener, I want to stream on-demand playlists so that I can listen to DJ's curated content

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement GET /v1/playlists/{playlist_id}/stream.m3u8 and GET /v1/playlists/{playlist_id}/stream.mp3 endpoints to serve HLS manifest and continuous MP3 stream. Enforce authentication for private playlists and subscription checks for monetized content.

**Acceptance Criteria**:
1. Given a public playlist, When listener requests HLS manifest, Then manifest with TS segments is returned
2. Given a private playlist without authentication, When listener requests stream, Then 403 Forbidden is returned
3. Given a monetized playlist without subscription, When listener requests stream, Then 403 Forbidden is returned
4. Given a playlist does not exist, When listener requests stream, Then 404 Not Found is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for playlist streaming endpoints`
- 🟢 GREEN: `green: serve HLS and MP3 streams with authentication`
- 🔵 REFACTOR: `refactor: add CDN caching and improve streaming performance`

**Branch**: `feature/3003-playlist-streaming`

**Labels**: user-story, feature, playlists


## Epic #4000: Podcast Hosting & RSS with Custom Domains

Enable DJs to create podcast series, upload episodes, generate RSS 2.0 feeds, and serve podcasts via custom domains with automated DNS and TLS certificate provisioning

### User Stories

#### Story #4001: As a DJ, I want to create a podcast series with custom domain so that I can host my podcast professionally

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement POST /v1/podcasts endpoint to create podcast with metadata and custom domain. Validate domain ownership via DNS TXT record, create CNAME, and provision TLS certificate via Let's Encrypt ACME.

**Acceptance Criteria**:
1. Given a DJ provides valid podcast metadata and custom domain, When podcast is created, Then podcast record is stored and DNS validation is initiated
2. Given a DJ provides invalid domain format, When podcast is created, Then 400 Bad Request is returned
3. Given a DJ provides domain already in use, When podcast is created, Then 409 Conflict is returned
4. Given DNS validation succeeds, When TLS provisioning completes, Then RSS feed URL is available at custom domain

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for podcast creation with custom domain`
- 🟢 GREEN: `green: podcast creation with DNS validation and TLS provisioning`
- 🔵 REFACTOR: `refactor: extract DNS validation and ACME certificate logic`

**Branch**: `feature/4001-podcast-creation`

**Labels**: user-story, feature, podcasts

#### Story #4002: As a DJ, I want to add episodes to my podcast so that listeners can download and subscribe

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement POST /v1/podcasts/{podcast_id}/episodes endpoint to add episodes with uploaded or external media. Validate media sources, store episode metadata, and trigger RSS feed regeneration.

**Acceptance Criteria**:
1. Given a DJ provides valid episode metadata with upload_id, When episode is added, Then episode record is created and RSS feed is regenerated
2. Given a DJ provides invalid upload_id, When episode is added, Then 422 Unprocessable Entity is returned
3. Given a DJ provides invalid external URL, When episode is added, Then 422 Unprocessable Entity is returned
4. Given an episode is added, When RSS feed is requested, Then new episode appears in feed within 60 seconds

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for podcast episode creation`
- 🟢 GREEN: `green: episode creation with media validation and RSS regeneration`
- 🔵 REFACTOR: `refactor: extract RSS generation and caching logic`

**Branch**: `feature/4002-podcast-episodes`

**Labels**: user-story, feature, podcasts

#### Story #4003: As a listener, I want to subscribe to podcast RSS feed so that I can receive new episodes automatically

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement GET /v1/podcasts/{podcast_id}/rss.xml and custom domain RSS endpoint to serve valid RSS 2.0 XML with iTunes tags. Enforce subscription checks for paywalled episodes via signed URLs.

**Acceptance Criteria**:
1. Given a public podcast, When listener requests RSS feed, Then valid RSS 2.0 XML with all episodes is returned
2. Given a podcast with custom domain, When listener requests RSS at custom domain, Then RSS feed is served via HTTPS
3. Given a paywalled episode without subscription, When listener requests enclosure URL, Then 403 Forbidden is returned
4. Given RSS feed is requested, When CDN cache is hit, Then feed is served with 300s TTL

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for RSS feed generation and serving`
- 🟢 GREEN: `green: generate and serve RSS 2.0 XML with iTunes tags`
- 🔵 REFACTOR: `refactor: add CDN caching and improve RSS validation`

**Branch**: `feature/4003-rss-feed`

**Labels**: user-story, feature, podcasts


## Epic #5000: Monetization & Ad Insertion

Provide subscription plans, payment processing via Stripe, paywall enforcement for streams/playlists/podcasts, and ad insertion hooks for live and on-demand content

### User Stories

#### Story #5001: As a DJ, I want to create subscription plans so that I can monetize my content

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement POST /v1/monetization/plans endpoint to create subscription plans with Stripe integration. Store plan metadata, create Stripe product and price, and return plan details.

**Acceptance Criteria**:
1. Given a DJ provides valid plan details, When plan is created, Then plan record is stored and Stripe product/price are created
2. Given a DJ provides invalid price, When plan is created, Then 400 Bad Request is returned
3. Given a plan is created, When DJ retrieves plan details, Then Stripe product_id and price_id are included
4. Given a plan is created, When listener subscribes, Then Stripe subscription is created successfully

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for subscription plan creation`
- 🟢 GREEN: `green: create plans with Stripe product and price integration`
- 🔵 REFACTOR: `refactor: extract Stripe API calls and improve error handling`

**Branch**: `feature/5001-subscription-plans`

**Labels**: user-story, feature, monetization

#### Story #5002: As a listener, I want to subscribe to DJ's plan so that I can access premium content

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement POST /v1/monetization/subscriptions endpoint to create subscriptions via Stripe. Handle payment method attachment, subscription creation, and webhook events for status updates.

**Acceptance Criteria**:
1. Given a listener provides valid payment method, When subscription is created, Then Stripe subscription is active and stored in database
2. Given a listener provides invalid payment method, When subscription is attempted, Then 402 Payment Required is returned
3. Given a subscription is active, When listener accesses paywalled content, Then access is granted
4. Given Stripe webhook receives payment_failed event, When webhook is processed, Then subscription status is updated to 'past_due'

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for subscription creation and webhooks`
- 🟢 GREEN: `green: create subscriptions with Stripe and handle webhooks`
- 🔵 REFACTOR: `refactor: extract webhook processing and improve subscription logic`

**Branch**: `feature/5002-subscriptions`

**Labels**: user-story, feature, monetization

#### Story #5003: As a DJ, I want to insert ads into my streams so that I can generate additional revenue

**Points**: 8 | **Type**: feature | **Status**: unstarted

Implement POST /v1/streams/{stream_id}/ads endpoint to configure ad markers (pre-roll, mid-roll, post-roll). Relay Worker fetches ad content and inserts at specified positions by switching FFmpeg input.

**Acceptance Criteria**:
1. Given a DJ configures ad markers, When markers are saved, Then Relay Worker picks up configuration within 30 seconds
2. Given ad marker at 300 seconds, When live stream reaches 300s, Then 30-second ad is inserted seamlessly
3. Given ad insertion completes, When stream resumes, Then main feed continues without interruption
4. Given invalid ad marker position, When configuration is submitted, Then 400 Bad Request is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for ad marker configuration and insertion`
- 🟢 GREEN: `green: configure ad markers and implement insertion in Relay Worker`
- 🔵 REFACTOR: `refactor: extract ad fetching and improve insertion timing`

**Branch**: `feature/5003-ad-insertion`

**Labels**: user-story, feature, monetization


## Epic #6000: Analytics & Reporting

Collect and expose analytics for live streams (listener counts), playlists (play counts), and podcasts (download counts) with date-range queries and exportable reports

### User Stories

#### Story #6001: As a DJ, I want to view live stream analytics so that I can track listener engagement

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement GET /v1/analytics/streams/{stream_id} endpoint to fetch daily listener counts, peak listeners, and total listens over a date range. Aggregate data from analytics table and cache results.

**Acceptance Criteria**:
1. Given a DJ requests analytics for date range, When endpoint is called, Then daily listener counts and peak listeners are returned
2. Given invalid date range, When analytics are requested, Then 400 Bad Request is returned
3. Given stream does not exist, When analytics are requested, Then 404 Not Found is returned
4. Given analytics are requested, When data is cached, Then subsequent requests return cached data within 5 minutes

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for live stream analytics endpoint`
- 🟢 GREEN: `green: aggregate and return listener analytics with date filtering`
- 🔵 REFACTOR: `refactor: add caching and improve query performance`

**Branch**: `feature/6001-stream-analytics`

**Labels**: user-story, feature, analytics

#### Story #6002: As a DJ, I want to view playlist analytics so that I can understand which tracks are popular

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement GET /v1/analytics/playlists/{playlist_id} endpoint to fetch daily play counts and total plays over a date range. Track play events when listeners request HLS manifest or MP3 stream.

**Acceptance Criteria**:
1. Given a DJ requests playlist analytics, When endpoint is called, Then daily play counts and total plays are returned
2. Given a listener plays playlist, When HLS manifest is requested, Then play count is incremented
3. Given invalid date range, When analytics are requested, Then 400 Bad Request is returned
4. Given playlist does not exist, When analytics are requested, Then 404 Not Found is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for playlist analytics endpoint`
- 🟢 GREEN: `green: track play events and aggregate analytics`
- 🔵 REFACTOR: `refactor: optimize play tracking and improve analytics queries`

**Branch**: `feature/6002-playlist-analytics`

**Labels**: user-story, feature, analytics

#### Story #6003: As a DJ, I want to view podcast analytics so that I can measure episode downloads

**Points**: 5 | **Type**: feature | **Status**: unstarted

Implement GET /v1/analytics/podcasts/{podcast_id} endpoint to fetch daily download counts and total downloads over a date range. Track downloads when listeners request episode enclosure URLs from RSS feed.

**Acceptance Criteria**:
1. Given a DJ requests podcast analytics, When endpoint is called, Then daily download counts and total downloads are returned
2. Given a listener downloads episode, When enclosure URL is accessed, Then download count is incremented
3. Given invalid date range, When analytics are requested, Then 400 Bad Request is returned
4. Given podcast does not exist, When analytics are requested, Then 404 Not Found is returned

**TDD Workflow**:
- 🔴 RED: `WIP: red tests for podcast analytics endpoint`
- 🟢 GREEN: `green: track downloads and aggregate analytics`
- 🔵 REFACTOR: `refactor: add CSV export and improve download tracking`

**Branch**: `feature/6003-podcast-analytics`

**Labels**: user-story, feature, analytics

