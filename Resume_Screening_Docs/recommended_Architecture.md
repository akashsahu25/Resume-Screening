┌─────────────────────────────┐
│ Next.js Web Application     │
│ React + TypeScript          │
└──────────────┬──────────────┘
               │ HTTPS / REST  
               │ 
┌──────────────▼──────────────┐
│ Spring Boot Backend         │
│                             │
│ Authentication              │
│ User and subscription       │
│ Job management              │
│ Candidate management        │
│ Billing and credits         │
│ Notes and decisions         │
│ Export                      │
└───────┬─────────┬───────────┘
        │         │
        │         └───────────────┐
        ▼                         ▼
┌───────────────┐        ┌─────────────────┐
│ PostgreSQL    │        │ Redis           │
│ Main database │        │ Cache/progress  │
└───────────────┘        └─────────────────┘
        │
        ▼
┌───────────────────────┐
│ RabbitMQ              │
│ Resume processing     │
│ job queue             │
└───────────┬───────────┘
            |
            ▼
┌─────────────────────────────┐
│ Python AI Worker            │
│ FastAPI                     │
│                             │
│ Text extraction             │
│ Skill detection             │
│ Experience calculation      │
│ Match scoring               │
│ Candidate summarization     │
│ Fraud detection             │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│ S3 / Cloudflare R2          │
│ Resume and JD files         │
└─────────────────────────────┘ 