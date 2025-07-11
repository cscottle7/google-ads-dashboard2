sequenceDiagram
    actor User
    participant Browser as Next.js Frontend
    participant Server as Next.js Backend
    participant Auth0
    participant DB as PostgreSQL DB (via Prisma)
    participant GoogleAPI as Google Ads API

    %% --- Authentication Flow ---
    rect rgb(235, 245, 255)
        Note over User, Auth0: User Authentication Flow
        User->>Browser: 1. Accesses the application
        Browser->>Server: 2. Requests dashboard page
        Server-->>Browser: 3. Returns page; NextAuth.js middleware redirects to Auth0 for login
        Browser->>Auth0: 4. Redirects user to Auth0 login page
        User->>Auth0: 5. Enters credentials
        Auth0-->>User: 6. Authenticates user
        Auth0->>Browser: 7. Redirects back to application callback URL
        Browser->>Server: 8. Hits /api/auth/callback with Auth0 code
        activate Server
        Server->>Auth0: 9. Verifies code and fetches user profile
        Auth0-->>Server: 10. Returns user profile info
        Server-->>Browser: 11. Creates secure HttpOnly session cookie
        deactivate Server
        Browser->>User: 12. Renders authenticated dashboard view
    end

    %% --- Dashboard Data Fetching Flow ---
    rect rgb(235, 255, 245)
        Note over User, GoogleAPI: Dashboard Data Fetching Flow
        User->>Browser: 13. Views Primary Insights Dashboard
        activate Browser
        Note right of Browser: Client component calls 'useCampaignOverview' hook (SWR)
        Browser->>Server: 14. GET /api/campaign-overview<br/>(Session cookie is sent automatically)
        deactivate Browser
        activate Server
        Note left of Server: NextAuth.js middleware validates session
        Server->>DB: 15. Fetches cached data via Prisma Client
        activate DB
        DB-->>Server: 16. Returns data
        deactivate DB
        Server->>GoogleAPI: 17. Proxies secure request for fresh data
        activate GoogleAPI
        GoogleAPI-->>Server: 18. Returns Google Ads data
        deactivate GoogleAPI
        Note left of Server: Performs Zod validation and<br/>shapes data with a DTO
        Server-->>Browser: 19. Returns combined data as JSON payload
        deactivate Server
        activate Browser
        Note right of Browser: SWR hook receives data, React components re-render
        Browser->>User: 20. Displays dashboard with charts and insights
        deactivate Browser
    end

