# CONTEXT.md: AI Knowledge Base for Google Ads Campaign Dashboard

**Last Updated:** 2025-07-08

**Instructions for AI:** Read this document in its entirety at the start of any new feature development or complex debugging task. Refer to it as the primary source of truth for project architecture, standards, and goals.

---

## 1.0 Project Overview

* **Project Name:** Google Ads Campaign Dashboard
* **Purpose/Mission:** To create a client-centric dashboard that transforms complex Google Ads data into simple, actionable narratives. The goal is to demystify ad performance, encourage proactive strategic decisions, and transform static reports into a collaborative workspace for clients, who are typically business owners and not marketing experts.
* **Core Functionality:** The application is a client-facing dashboard structured into two main views:
    * **Primary Insights View (Default):** A clean, uncluttered overview featuring an AI-generated "'Plain English' Summary", an AI-powered "'Opportunity Spotlight'" card with a simple approval mechanism, and an "'Anomaly Alert'" monitor.
    * **Detailed Analytics View:** Allows for deeper data exploration with components like a "Geographic 'Hotspot'" card, "Performance Heatmap", "The 'Flow Funnel' Visual", and "'The Customer Journey Story'".
    * **Collaborative Tools:** Includes a "'Client Co-Pilot' Annotator" for adding contextual questions directly to data points and a one-time "'Client Onboarding Flow'" for new users.

---

## 2.0 Core Technologies & Libraries

* **Frontend:** Next.js
* **Backend:** Node.js (via Next.js serverless functions)
* **Database:** PostgreSQL
* **Styling:** Tailwind CSS
* **Deployment:** Vercel
* **State Management:** SWR for client-side data fetching and caching; React Context for global state.
* **Key Libraries & Services:**
    * **Authentication:** Auth0
    * **ORM:** Prisma
    * **Validation:** Zod
    * **Data Visualisation:** Nivo (for Funnel/Heatmap) or Tremor
    * **Onboarding Tours:** React Joyride or Shepherd
    * **Contextual Comments:** Liveblocks

---

## 3.0 Architectural Principles

* **Overall Architecture:** The project uses a Next.js App Router structure. Development must follow principles from "SOP: Vibe Coding Principles & Workflow" and security patterns from "SOP: Advanced Security Rulebook for Next.js Development". The file structure is organised by function (`/app`, `/components`, `/lib`, `/context`, `/prisma`).
* **Data Flow:**
    * A detailed data flow diagram with multi-tenancy access patterns must be created before implementation to ensure strict data isolation.
    * Data Transfer Objects (DTOs) are mandatory to shape and filter all data passed from the server to the client, preventing accidental data leakage.
* **API Design:**
    * Backend logic is implemented as secure serverless functions within `/app/api/`.
    * All requests to external APIs (e.g., Google Ads) must be proxied through the backend to protect credentials.
    * State-changing operations should use Next.js Server Actions for built-in CSRF protection.
* **Security Principles:**
    * **Authorisation:** Every API route and Server Action must verify that the authenticated user is authorised to access the requested resource (Principle of Least Privilege).
    * **Input Validation:** All API inputs must be strictly validated using Zod.
    * **XSS Prevention:** All user-generated content (e.g., from the 'Client Co-Pilot' Annotator) must be sanitised before rendering.
    * **CSRF Protection:** Sensitive state-changing operations (e.g., 'Opportunity Spotlight' approval) must use Next.js Server Actions or explicit anti-CSRF token validation.
    * **Secure Sessions:** Session tokens must be stored in secure, HttpOnly cookies.
    * **SSRF Prevention:** Any feature fetching from external URLs (e.g., 'Market Pulse' Echo) must use a strict, hard-coded whitelist of allowed domains.

---

## 4.0 Data Models (High-Level)

This section outlines the proposed high-level database schema to be implemented in `/prisma/schema.prisma`. The design establishes clear relationships between entities and enforces the critical multi-tenancy requirement, ensuring strict data isolation for each client.

### **`Client` Model**

Represents a client business entity. It is the central point for data isolation.

* `id` (String): Unique identifier for the client.
* `name` (String): The client's business name.
* `googleAdsAccountId` (String): The client's Google Ads account ID, used for API integration.
* `users` (Relation): One-to-many relationship with the `User` model.
* `campaignData` (Relation): One-to-many relationship with `CampaignData` points.
* `annotations` (Relation): One-to-many relationship with `Annotation`s.

### **`User` Model**

Represents an individual with access to the dashboard. Each user is associated with a single client.

* `id` (String): Unique identifier for the user.
* `auth0Id` (String): Unique identifier from the Auth0 service, linking the user to their authentication record.
* `email` (String): The user's email address.
* `name` (String): The user's full name.
* `role` (Enum: `ADMIN`, `CLIENT`): Defines the user's role. `ADMIN` for internal staff, `CLIENT` for the client's personnel.
* `clientId` (String): Foreign key linking to the `Client` model. This is the core of the multi-tenancy enforcement.
* `client` (Relation): Many-to-one relationship with the `Client` model.
* `annotations` (Relation): One-to-many relationship with `Annotation`s created by this user.

### **`CampaignData` Model**

Stores snapshots of Google Ads performance metrics for a specific date.

* `id` (String): Unique identifier for the data entry.
* `date` (DateTime): The date for which the metrics are recorded.
* `impressions` (Int): Number of ad impressions.
* `clicks` (Int): Number of ad clicks.
* `conversions` (Decimal): Number of conversions.
* `cost` (Decimal): The total cost for the recorded date (use `Decimal` for financial accuracy).
* `clientId` (String): Foreign key linking to the `Client` model to ensure data isolation.
* `client` (Relation): Many-to-one relationship with the `Client` model.

### **`Annotation` Model**

Represents a comment or note placed on a chart by a user.

* `id` (String): Unique identifier for the annotation.
* `content` (String): The text content of the annotation.
* `chartId` (String): An identifier for the UI component this annotation is attached to (e.g., "flow-funnel").
* `position` (Json): Stores the x/y coordinates for placing the annotation on the chart.
* `userId` (String): Foreign key linking to the `User` who created the annotation.
* `user` (Relation): Many-to-one relationship with the `User` model.
* `clientId` (String): Foreign key linking to the `Client` whose dashboard the annotation is on.
* `client` (Relation): Many-to-one relationship with the `Client` model.

---

## 5.0 API Endpoints (Key Examples)

This section details the primary API endpoints required to power the dashboard. All endpoints must perform authorisation checks to ensure the authenticated user has permission to access the requested client's data.

### **Dashboard & Core Data**

* **`GET /api/v1/campaign-overview`**
    * **Description:** Fetches aggregated data needed for the Primary Insights View.
    * **Security:** Must verify the authenticated user is authorised to view the requested client's data.
    * **Response (200 OK):** Returns a DTO with shaped data for the 'Plain English Summary', 'Budget Pacing Bar', and 'Flow Funnel'.
* **`POST (Server Action)` for Opportunity Spotlight Approval**
    * **Description:** Handles the 'approve' action from the 'Opportunity Spotlight' card.
    * **Security:** Implemented as a Next.js Server Action for built-in CSRF protection.
    * **Body:** `{ "opportunityId": "string" }`
    * **Response (200 OK):** Returns the updated status.

### **Analytics & Visualisations**

* **`GET /api/v1/analytics/geographic-hotspot`**
    * **Description:** Fetches data aggregated by geographic location to power the 'Geographic "Hotspot"' card.
    * **Security:** Authorisation check is mandatory.
    * **Query Params:** `?dateRange=30d`
    * **Response (200 OK):** Returns a DTO with a list of locations and their conversion values.
* **`GET /api/v1/analytics/performance-heatmap`**
    * **Description:** Fetches performance data aggregated by time blocks (day of the week, hour of day) for 'The Performance Heatmap' visual.
    * **Security:** Authorisation check is mandatory.
    * **Query Params:** `?dateRange=30d&metric=conversions`
    * **Response (200 OK):** Returns a DTO structured for the heatmap grid.

### **Client Co-Pilot Annotator (CRUD)**

* **`POST /api/v1/annotations`**
    * **Description:** Creates a new annotation on a chart via the 'Client Co-Pilot' tool.
    * **Security:** Validates input with Zod; sanitises content to prevent XSS. The user must be associated with the client ID for which they are creating a comment.
    * **Body:** `{ "chartId": "string", "content": "string", "position": { "x": "number", "y": "number" } }`
    * **Response (201 Created):** Returns the new annotation object.
* **`GET /api/v1/annotations`**
    * **Description:** Retrieves all annotations for the client associated with the authenticated user.
    * **Security:** Authorisation check is mandatory.
    * **Query Params:** `?chartId=performance-heatmap` (Optional: to filter annotations for a specific chart).
    * **Response (200 OK):** Returns an array of annotation objects.
* **`PUT /api/v1/annotations/{annotationId}`**
    * **Description:** Updates the content of an existing annotation.
    * **Security:** User must be the original author of the annotation to perform an update. Input is validated and sanitised.
    * **Body:** `{ "content": "string" }`
    * **Response (200 OK):** Returns the updated annotation object.
* **`DELETE /api/v1/annotations/{annotationId}`**
    * **Description:** Deletes an annotation.
    * **Security:** User must be the original author of the annotation or an Admin to perform deletion.
    * **Response (204 No Content):** Returns an empty body on successful deletion.

---

## 6.0 Code Style Guide & Conventions

* **Naming Conventions:**
    * Must follow the conventions defined in the "SOP: Core Component Design System & Style Guide".
    * Components: PascalCase, organised by function (e.g., `FlowFunnel.tsx`, `OpportunitySpotlight.tsx`).
    * API Routes: kebab-case for directories (e.g., `/api/campaign-overview/route.ts`).
* **Linting:** The project must integrate and adhere to the ESLint rules specified in the "SOP: Advanced Security Rulebook".
* **Commenting:** All exported functions, classes, and types must be documented using TSDoc. Documentation blocks must include a concise summary of the element's purpose, `@param` tags for all parameters, and a `@returns` tag describing the output.
* **Error Handling:** All I/O operations (database queries, API calls) must be wrapped in `try...catch` blocks. For client-facing errors, the API must return a standardised JSON object. For validation failures (checked with Zod), the API will return a `400 Bad Request` status. For unexpected server issues, a generic `500 Internal Server Error` response must be provided to avoid leaking implementation details. The standard error shape is:
    ```json
    {
      "error": {
        "message": "A descriptive error message for the developer.",
        "details": [
          { "field": "fieldName", "issue": "Specific validation issue." }
        ]
      }
    }
    ```
* **Logging:** Structured JSON logging is the standard.
    * **`info` level:** Logged at the start and end of every public API function. Logs must include the function name and key context parameters like `clientId` and `userId` for traceability.
    * **`error` level:** Logged for all caught exceptions. Logs must include the full error stack trace and relevant request context to facilitate rapid debugging.


