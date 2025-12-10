# Product Requirements Document (PRD)

## Web-Based Audio Streaming API for DJs & Podcasters

---

```json
{
  "document_metadata": {
    "title": "Web-Based Audio Streaming API for DJs & Podcasters",
    "version": "1.0",
    "date": "2025-06-03",
    "author": "Product Architecture Team",
    "status": "Final",
    "document_type": "Product Requirements Document"
  },

  "executive_summary": {
    "overview": "A comprehensive RESTful API service enabling DJs and podcasters to manage live audio streaming via Shoutcast relay, on-demand playlist hosting with direct file uploads, podcast RSS hosting with custom domains, monetization through subscriptions and ad insertion, and detailed analytics. The platform provides scalable, secure infrastructure for tens of thousands of concurrent listeners.",
    
    "business_objectives": [
      "Enable DJs to broadcast live audio streams with minimal technical overhead",
      "Provide seamless on-demand playlist and podcast hosting with direct file uploads",
      "Support custom domain branding for podcast RSS feeds with automated DNS/TLS",
      "Monetize content through subscription tiers and dynamic ad insertion",
      "Deliver comprehensive analytics for streams, playlists, and podcasts",
      "Ensure 99.9% uptime with horizontally scalable architecture"
    ],
    
    "key_differentiators": [
      "Integrated Shoutcast relay with automatic HLS segmentation via FFmpeg",
      "Direct file upload with background transcoding to HLS",
      "Automated custom domain provisioning with Let's Encrypt TLS",
      "Built-in monetization via Stripe integration and ad insertion hooks",
      "Real-time analytics with exportable reports",
      "GDPR-compliant data handling with Right to Be Forgotten"
    ],
    
    "success_criteria": {
      "technical": [
        "Support 10,000+ concurrent listeners per stream",
        "Achieve <2s latency for HLS segment delivery",
        "Maintain 99.9% API uptime",
        "Process file uploads and transcode to HLS within 5 minutes for 100MB files"
      ],
      "business": [
        "Onboard 500+ DJs within first 6 months",
        "Generate $50K MRR from subscription plans by month 12",
        "Achieve 80% DJ retention rate",
        "Deliver 1M+ podcast downloads monthly by month 9"
      ]
    }
  },

  "product_overview": {
    "vision": "To become the leading API-first platform for DJs and podcasters, providing enterprise-grade streaming infrastructure with consumer-friendly simplicity.",
    
    "mission": "Empower content creators to focus on their craft while we handle the complex infrastructure of live streaming, on-demand hosting, and audience monetization.",
    
    "core_capabilities": [
      {
        "capability": "Live Streaming Relay",
        "description": "Ingest Shoutcast feeds, segment via FFmpeg into HLS chunks, and multicast to unlimited listeners",
        "key_features": [
          "Shoutcast source registration with credentials",
          "Real-time HLS segmentation (6-second chunks)",
          "Continuous MP3 proxy streaming",
          "Live metadata extraction (current track, bitrate, listener count)",
          "Automatic relay worker scaling"
        ]
      },
      {
        "capability": "On-Demand Playlist Management",
        "description": "Upload audio files or reference external URLs, generate HLS manifests, and serve continuous streams",
        "key_features": [
          "Direct MP3 file upload (up to 100MB)",
          "Background HLS transcoding via FFmpeg",
          "External URL validation and integration",
          "Playlist creation with mixed sources (uploads + external)",
          "HLS and MP3 streaming endpoints"
        ]
      },
      {
        "capability": "Podcast Hosting & RSS",
        "description": "Create podcast series, upload episodes, auto-generate RSS 2.0 feeds, and serve via custom domains",
        "key_features": [
          "Podcast series creation with metadata",
          "Episode upload (direct or external URL)",
          "Automated RSS 2.0 XML generation",
          "Custom domain registration (e.g., podcast.djname.com)",
          "Automated DNS verification and TLS provisioning via Let's Encrypt",
          "iTunes-compatible RSS tags"
        ]
      },
      {
        "capability": "Monetization & Ad Insertion",
        "description": "Configure subscription plans, enforce paywalls, and insert ads dynamically into streams",
        "key_features": [
          "Stripe-integrated subscription plans (monthly/yearly)",
          "Paywall enforcement for streams, playlists, and podcast episodes",
          "Ad marker configuration (pre-roll, mid-roll, post-roll)",
          "Dynamic ad insertion via FFmpeg input switching",
          "Subscription status webhooks"
        ]
      },
      {
        "capability": "Analytics & Reporting",
        "description": "Track listener counts, play counts, downloads, and provide exportable reports",
        "key_features": [
          "Real-time listener count for live streams",
          "Daily aggregated analytics (listeners, plays, downloads)",
          "Date-range queries with JSON/CSV export",
          "Peak listener tracking",
          "Two-year analytics retention, 90-day raw log retention"
        ]
      }
    ],
    
    "technology_stack": {
      "frontend": {
        "framework": "React 18+",
        "state_management": "Redux Toolkit",
        "ui_library": "Material-UI (MUI)",
        "build_tool": "Vite",
        "language": "TypeScript"
      },
      "backend": {
        "framework": "FastAPI (Python 3.11+)",
        "async_runtime": "asyncio with uvicorn",
        "orm": "SQLAlchemy 2.0 with asyncpg",
        "validation": "Pydantic v2",
        "task_queue": "Celery with Redis broker"
      },
      "database": {
        "primary": "PostgreSQL 15+ (AWS RDS Multi-AZ)",
        "caching": "Redis 7+ (AWS ElastiCache)",
        "object_storage": "AWS S3 with lifecycle policies"
      },
      "infrastructure": {
        "container_orchestration": "Kubernetes (AWS EKS)",
        "relay_workers": "Docker containers with FFmpeg 6+",
        "cdn": "AWS CloudFront",
        "dns": "AWS Route 53",
        "tls": "Let's Encrypt via cert-manager",
        "monitoring": "Prometheus + Grafana",
        "logging": "ELK Stack (Elasticsearch, Logstash, Kibana)"
      },
      "integrations": {
        "payment": "Stripe API v2023-10-16",
        "authentication": "JWT (RS256) with refresh tokens",
        "email": "AWS SES",
        "file_scanning": "ClamAV for virus detection"
      }
    }
  },

  "target_users": {
    "primary_personas": [
      {
        "persona_name": "Professional DJ",
        "demographics": {
          "age_range": "25-45",
          "technical_skill": "Intermediate",
          "location": "Global, primarily US/EU",
          "income": "$40K-$150K annually"
        },
        "goals": [
          "Broadcast live DJ sets to global audience",
          "Monetize premium content through subscriptions",
          "Track listener engagement and growth",
          "Host podcast series with custom branding"
        ],
        "pain_points": [
          "Complex Shoutcast server setup",
          "Lack of integrated monetization tools",
          "Difficulty managing multiple audio sources",
          "Limited analytics on listener behavior"
        ],
        "use_cases": [
          "Register weekly live stream with Shoutcast relay",
          "Upload recorded sets as on-demand playlists",
          "Create podcast series with custom domain (podcast.djname.com)",
          "Configure subscription paywall for premium streams",
          "View real-time listener counts and historical analytics"
        ]
      },
      {
        "persona_name": "Podcaster / Content Creator",
        "demographics": {
          "age_range": "22-50",
          "technical_skill": "Beginner to Intermediate",
          "location": "Global",
          "income": "$30K-$100K annually"
        },
        "goals": [
          "Host podcast with professional RSS feed",
          "Use custom domain for brand consistency",
          "Monetize episodes through ads or subscriptions",
          "Track download metrics and audience growth"
        ],
        "pain_points": [
          "Manual RSS feed generation and updates",
          "Lack of custom domain support on podcast platforms",
          "Difficulty inserting ads into episodes",
          "Limited download analytics"
        ],
        "use_cases": [
          "Create podcast series with automated RSS generation",
          "Upload episodes via direct file upload",
          "Register custom domain (podcast.example.com) with automated TLS",
          "Configure mid-roll ad markers for monetization",
          "Export monthly download reports as CSV"
        ]
      },
      {
        "persona_name": "API Consumer / Developer",
        "demographics": {
          "age_range": "24-40",
          "technical_skill": "Advanced",
          "location": "Global",
          "role": "Full-stack developer, mobile app developer"
        },
        "goals": [
          "Integrate streaming API into custom DJ dashboard",
          "Build mobile apps for podcast listening",
          "Automate content uploads and playlist management",
          "Implement custom analytics dashboards"
        ],
        "pain_points": [
          "Poorly documented APIs",
          "Lack of SDKs for common languages",
          "Rate limiting without clear documentation",
          "Inconsistent error responses"
        ],
        "use_cases": [
          "Authenticate via JWT and manage streams programmatically",
          "Upload audio files via multipart/form-data endpoints",
          "Fetch real-time listener counts for live streams",
          "Integrate Stripe webhooks for subscription updates",
          "Build custom analytics dashboard using analytics endpoints"
        ]
      }
    ],
    
    "secondary_personas": [
      {
        "persona_name": "Listener / End User",
        "role": "Implicit user consuming streams, playlists, and podcasts",
        "interaction_points": [
          "Access live HLS streams via web/mobile players",
          "Subscribe to podcasts via RSS feed in podcast apps",
          "Pay for premium subscriptions to access paywalled content",
          "View inserted ads during streams"
        ]
      },
      {
        "persona_name": "Platform Administrator",
        "role": "Internal user managing platform operations",
        "responsibilities": [
          "Monitor system health and relay worker status",
          "Manage user accounts and subscriptions",
          "Review analytics across all DJs",
          "Handle GDPR data deletion requests"
        ]
      }
    ]
  },

  "functional_requirements": {
    "feature_categories": [
      {
        "category": "Authentication & Authorization",
        "priority": "Critical",
        "user_stories": [
          {
            "id": "AUTH-001",
            "as_a": "DJ",
            "i_want_to": "Register a new account with email and password",
            "so_that": "I can access the API and manage my streams",
            "acceptance_criteria": [
              "POST /v1/auth/register accepts email, password, name",
              "Password must be ≥8 characters with uppercase, lowercase, number, special char",
              "Email must be unique and validated",
              "Returns 201 Created with user_id and role='dj'",
              "Sends verification email via AWS SES"
            ],
            "api_endpoint": "POST /v1/auth/register",
            "priority": "P0"
          },
          {
            "id": "AUTH-002",
            "as_a": "DJ",
            "i_want_to": "Login with email and password",
            "so_that": "I receive JWT access and refresh tokens",
            "acceptance_criteria": [
              "POST /v1/auth/login accepts email and password",
              "Returns 200 OK with access_token (24h expiry) and refresh_token",
              "Returns 401 Unauthorized for invalid credentials",
              "Rate limit: 5 failed attempts per 15 minutes triggers CAPTCHA"
            ],
            "api_endpoint": "POST /v1/auth/login",
            "priority": "P0"
          },
          {
            "id": "AUTH-003",
            "as_a": "DJ",
            "i_want_to": "Refresh my JWT access token",
            "so_that": "I maintain authenticated sessions without re-login",
            "acceptance_criteria": [
              "POST /v1/auth/refresh accepts refresh_token",
              "Returns 200 OK with new access_token",
              "Returns 401 Unauthorized if refresh_token expired or invalid",
              "Refresh tokens rotate on each use"
            ],
            "api_endpoint": "POST /v1/auth/refresh",
            "priority": "P0"
          },
          {
            "id": "AUTH-004",
            "as_a": "DJ",
            "i_want_to": "Reset my password via email",
            "so_that": "I can regain access if I forget my password",
            "acceptance_criteria": [
              "PUT /v1/auth/password-reset with email sends reset token via email",
              "PUT /v1/auth/password-reset with reset_token and new_password updates password",
              "Reset tokens expire after 1 hour",
              "Returns 200 OK on successful password update"
            ],
            "api_endpoint": "PUT /v1/auth/password-reset",
            "priority": "P1"
          },
          {
            "id": "AUTH-005",
            "as_a": "API",
            "i_want_to": "Enforce role-based access control (RBAC)",
            "so_that": "DJs can only access their own resources and admins have global access",
            "acceptance_criteria": [
              "JWT contains user_id, role (dj/admin), and scopes",
              "Middleware validates scopes for each endpoint (e.g., streams:write, playlists:read)",
              "DJs can only modify resources they own (user_id match)",
              "Admins have full read/write access across all resources",
              "Returns 403 Forbidden for unauthorized access"
            ],
            "priority": "P0"
          }
        ]
      },
      {
        "category": "Live Streaming (Shoutcast Relay)",
        "priority": "Critical",
        "user_stories": [
          {
            "id": "STREAM-001",
            "as_a": "DJ",
            "i_want_to": "Register a Shoutcast source",
            "so_that": "My live stream is relayed and segmented into HLS",
            "acceptance_criteria": [
              "POST /v1/streams accepts shoutcast_url, mount, stream_key, name, genre, description, visibility, monetization_enabled",
              "Validates Shoutcast source connectivity (HEAD request to admin endpoint)",
              "Encrypts stream_key before storing in database",
              "Spins up Relay Worker Pod in Kubernetes",
              "Returns 201 Created with stream_id and status='registered'",
              "Returns 422 Unprocessable Entity if Shoutcast source unreachable"
            ],
            "api_endpoint": "POST /v1/streams",
            "priority": "P0"
          },
          {
            "id": "STREAM-002",
            "as_a": "DJ",
            "i_want_to": "Update my stream configuration",
            "so_that": "I can change visibility, enable/disable monetization, or pause the stream",
            "acceptance_criteria": [
              "PATCH /v1/streams/{stream_id} accepts any subset of fields (name, genre, description, visibility, monetization_enabled, enabled)",