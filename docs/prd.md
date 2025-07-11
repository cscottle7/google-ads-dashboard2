# Product Requirements Document: Google Ads Campaign Dashboard

---

## 1. Project Overview & Goals

### 1.1. Brief Description
This project is for a client-centric dashboard that transforms complex Google Ads data into simple, actionable narratives. Its purpose is to demystify ad performance, foster proactive strategic decisions, and strengthen client relationships by turning static reports into a collaborative workspace.

### 1.2. Key Objectives
* **User Adoption:** Achieve 80% of clients logging in at least once per month within 6 months of launch.
* **Task Success Rate / Engagement:** Have 25% of active users click the 'approve' button on an 'Opportunity Spotlight' card within 3 months.
* **User Satisfaction:** Achieve a Net Promoter Score (NPS) of +40 when surveyed about the new dashboard.
* **Business Impact:** Reduce time spent by account managers on manual client performance reports by 5 hours per week.

### 1.3. AI Assistance Expectations
AI will be used for initial component scaffolding, generating boilerplate for serverless API functions, assisting with data model implementation for Prisma, and suggesting unit tests. Heavier human oversight will be applied to the core data visualisation logic and specific AI prompts.

---

## 2. Target Audience

### 2.1. Primary Users
Our clients, who are typically business owners and may not be marketing experts. They need clarity and value, not complex analytics.

### 2.2. User Stories
* "As a time-poor business owner, I want to be presented with a single, actionable strategic suggestion with a simple 'approve' button, so that I can make informed decisions about my campaign without needing to analyse complex data myself."
* "As a client who is unfamiliar with marketing analytics, I want a successful conversion to be visualised as a simple 3-step story, so that I can intuitively understand the path a customer takes from search to action."
* "As a client reviewing my dashboard, I want to add a question directly onto a chart or data point, so that I can get contextual clarification from my account manager without having to write a separate email."

---

## 3. Core Features & Scope
The application is structured into two main views to ensure a focused user experience.

### 3.1. In-Scope Features: Primary Insights View (Default)
This view provides an immediate, high-level overview.

* **Design Intent:** This view must feel exceptionally clean and uncluttered. The components should be given significant white space to increase their individual impact and reduce cognitive load. The path to the "Detailed Analytics View" must be clearly signposted.
* **"'Plain English' Summary":** An AI-generated paragraph summarizing the week's key performance takeaways.
* **AI Guardrails:** Generative AI prompts must include robust guardrails to ensure outputs are factually grounded in the provided data and to minimise the risk of fabrication.
* **"'Opportunity Spotlight' Card":** An AI-powered card highlighting a single strategic opportunity. The 'approve' button must use the primary variant from the design system to make it the most prominent interactive element.
* **"Discuss this" Functionality:** To build user trust, this card must include a "Discuss this" button. This will allow a client to seek clarification via the "Client Co-pilot" annotator before approving a suggestion.
* **"'Anomaly Alert' Monitor":** An AI-powered monitor flagging significant deviations from the performance baseline.
* **Client Onboarding Flow:** A one-time, guided tour highlighting key features and explaining the dashboard's layout and purpose for first-time users.

### 3.2. In-Scope Features: Detailed Analytics View
This view allows users to explore data more deeply.

* **Design Intent:** Features in this view should be organised logically (e.g., grouped by category) to avoid overwhelming users. Simplified metrics must be presented with clear contextual framing (e.g., colour-coding, explicit 'Good/Better/Worse' indicators) to prevent misinterpretation. The UI must provide clear, concise tooltips or 'i' icons explaining the purpose of each AI-powered feature.
* **Geographic "Hotspot" Card:** Displays the most valuable conversion location.
* **Most Profitable Hour Card:** Identifies the most efficient day and time block.
* **Device Snapshot Card:** Shows the conversion breakdown by device type.
* **Budget Pacing Bar:** Compares budget spent against time elapsed.
* **The "Flow Funnel" Visual:** Shows the relationship between Impressions, Clicks, and Conversions.
* **The "Performance Heatmap" Visual:** A calendar-style grid visualizing performance.
* **"'Unsung Heroes' Report":** Surfaces high-potential but underperforming assets.
* **"'Audience Persona Generator":** Synthesizes data into a simple user persona.
* **"'The Customer Journey Story' View":** Visualises a successful conversion as a storyboard.
* **"'The Market Pulse' Echo":** Provides an anonymised benchmark against the client's market. (Note: Data source must be a pre-approved, hard-coded whitelist).
* **"'The Budget Dial'" Simulator:** An interactive dial to see simplified ad spend projections.
* **"'Client Co-Pilot' Annotator":** A two-way communication tool for contextual notes.

### 3.3. Future Enhancements (Out of Scope for Initial Launch)
* **Intelligent Links Between Views:** If an "Anomaly Alert" flags an issue, it should include a deep link to the relevant visualisation in the Detailed Analytics View.
* **Integrate "Anomaly Alert" with "Plain English" Summary:** Feed the output of the Anomaly Alert into the context for the Plain English Summary prompt to create more dynamic narratives.

---

## 4. Technology Stack Constraints & Preferences
* **4.1. Frontend Framework/Libraries:** Next.js
* **4.2. Backend Language/Framework:** Node.js
* **4.3. Database:** PostgreSQL
* **4.4. Styling Approach:** Tailwind CSS
* **4.5. Key Libraries/Services:** Auth0, Prisma, SWR, Zod
* **4.6. Deployment Target:** Vercel
* **4.7. Version Control System:** Git

---

## 5. Key Development Modules & Structure
**Pre-development Task:** It is critical to create a detailed data flow diagram before implementation. This diagram must include explicit multi-tenancy access patterns to ensure strict data isolation between clients.

* **/app/:** Core application routes.
* **/app/(dashboard)/page.tsx:** The default "Primary Insights View".
* **/app/(dashboard)/detailed-analytics/page.tsx:** The "Detailed Analytics View".
* **/app/api/:** Serverless functions for the secure backend.
* **/components/:** React components, organised by view and function.
* **/lib/:** Helper functions, utility scripts, and API clients.
* **/context/:** React Context providers for global state.
* **/prisma/:** Database schema definition.

---

## 6. Initial Prompts for Scaffolding (Examples)
* **For a Data Visualisation Component:** `@AI, create a new React component in /components/charts/FlowFunnel.tsx...`
* **For a Secure Backend API Route:** `@AI, create a new Next.js API route in /app/api/campaign-overview/route.ts...`
* **For a Client-Side Data Fetching Hook:** `@AI, create a custom React hook named useCampaignOverview in /lib/hooks.ts...`

---

## 7. Non-Functional Requirements

### 7.1. Security Considerations
* **Secure API Proxy:** All requests to external APIs (e.g., Google Ads) must be proxied through the backend.
* **Input Validation & Sanitisation:** All API inputs must be validated with Zod. All user-generated content must be sanitised to prevent XSS.
* **CSRF Protection:** State-changing operations (e.g., "Opportunity Spotlight" approval) must use Next.js Server Actions or explicit anti-CSRF token validation.
* **Secure Token Handling:** Session tokens must be stored in secure, HttpOnly cookies.
* **Principle of Least Privilege:** Every API route must verify that the authenticated user is authorised to access the requested resource.

### 7.2. Performance Goals
* The main dashboard view should achieve a First Contentful Paint (FCP) of under 2.5 seconds.
* The application should achieve a Google Lighthouse score of over 90 for Performance, Accessibility, and Best Practices.

### 7.3. Accessibility Standards
* The application must be compliant with WCAG 2.1 at the AA level.

### 7.4. Data Integrity & Availability
* **AI Quality Assurance:** A process must be defined for validating the accuracy of AI-generated insights before they are presented to a client (e.g., an internal review step or confidence scoring).
* **Data Freshness:** The dashboard must have a clear visual state (e.g., a banner or timestamp) to indicate when the displayed data is stale or if the connection to the data source is unavailable.

---

## 8. Project Rules & Conventions

### Core Development Methodology & Principles
* The project must adhere to the principles outlined in the **SOP: Vibe Coding Principles & Workflow**.
* The architecture must follow the patterns in the **SOP: Advanced Security Rulebook for Next.js Development**.
* Data Transfer Objects (DTOs) must be used to shape all data passed from the server to the client.
* All new UI components must accord with the specifications in the **SOP: Core Component Design System & Style Guide**.

### Coding & Project Governance
* The project must follow the naming conventions defined in the **SOP: Core Component Design System & Style Guide**.
* The project must integrate the ESLint rules specified in the **SOP: Advanced Security Rulebook**.
* New custom Cursor rules must follow the procedure outlined in the **SOP: Rule Promotion & Governance Workflow**.