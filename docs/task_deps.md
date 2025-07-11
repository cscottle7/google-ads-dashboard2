### TASK_DEPS.md

| ID | Phase | Category | Task Description | Status | Dependencies | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **0.1** | 0: Setup | Planning | Create a detailed data flow diagram for the application. | 📋 To Do | - | Crucial first step from the PRD. |
| **0.2** | 0: Setup | Preparation | Initialise a new Next.js 14 project with TypeScript and Tailwind CSS. | 📋 To Do | - | |
| **0.3** | 0: Setup | Preparation | Set up Git repository and connect to GitHub. | 📋 To Do | 0.2 | |
| **0.4** | 0: Setup | Preparation | Set up PostgreSQL database and obtain connection string. | 📋 To Do | 0.2 | |
| **0.5** | 0: Setup | Preparation | Install all core dependencies (`next-auth`, `prisma`, `zod`, `react-query`). | 📋 To Do | 0.2 | |
| **0.6** | 0: Setup | Cursor Knowledge Base Prep | Create and populate the master `CONTEXT.md` file. | 📋 To Do | 0.2 | [cite_start]Based on Cottle Protocol[cite: 1128]. |
| **0.7** | 0: Setup | Cursor Knowledge Base Prep | Create initial `.cursor/rules` files for style and security. | 📋 To Do | 0.2 | |
| **0.8** | 0: Setup | Cursor Knowledge Base Prep | Onboard AI to external libraries using `@Docs`. | 📋 To Do | 0.5 | [cite_start]For NextAuth.js, Prisma, and key UI libs [cite: 9462-9463]. |
| **0.9** | 0: Setup | Preparation | Define the basic application file structure (`/app`, `/components`, `/lib`). | 📋 To Do | 0.2 | |
| **0.10** | 0: Setup | Preparation | Define initial database schema in `prisma/schema.prisma`. | 📋 To Do | 0.4 | |
| **0.11** | 0: Setup | Preparation | Run `prisma generate` to create the Prisma Client. | 📋 To Do | 0.10 | |
| **1.1** | 1: MVP | Implementation | Implement user authentication using NextAuth.js with the Auth0 provider. | 📋 To Do | 0.5, 0.11 | |
| **1.2** | 1: MVP | Implementation | Implement API proxy to securely handle requests to the Google Ads API. | 📋 To Do | 1.1 | |
| **1.3** | 1: MVP | Implementation | Define Zod schemas for all API responses to ensure type safety. | 📋 To Do | 0.5 | |
| **1.4** | 1: MVP | Implementation | Create Data Transfer Objects (DTOs) for passing data between services. | 📋 To Do | - | |
| **1.5** | 1: MVP | Implementation | Build the primary dashboard view for displaying campaign data. | 📋 To Do | 1.2 | |
| **1.6** | 1: MVP | Implementation | Implement the 'Plain English' Summary component using an AI prompt. | 📋 To Do | 1.5 | |
| **1.7** | 1: MVP | Implementation | Implement the 'Opportunity Spotlight' card with a Server Action for dismissal. | 📋 To Do | 1.5 | |
| **1.8** | 1: MVP | Research | Research and select a suitable library for contextual comments/annotations. | 📋 To Do | - | |
| **1.9** | 1: MVP | Implementation | Build the base "'Client Co-Pilot' Annotator" tool using the selected library. | 📋 To Do | 1.8 | |
| **1.10** | 1: MVP | Implementation | Integrate the Annotator tool with the dashboard. | 📋 To Do | 1.9 | |
| **1.11** | 1: MVP | Testing & Checks | [cite_start]Ensure all user-facing inputs are sanitised to prevent XSS attacks[cite: 9422]. | 📋 To Do | 1.10 | |
| **1.12** | 1: MVP | Research | Research and select a library for creating the 'Flow Funnel' chart. | 📋 To Do | - | |
| **1.13** | 1: MVP | Implementation | Implement the 'Flow Funnel' chart component. | 📋 To Do | 1.12 | |
| **1.14** | 1: MVP | Research | Research and select a library for creating the 'Budget Pacing Bar'. | 📋 To Do | - | |
| **1.15** | 1: MVP | Implementation | Implement the 'Budget Pacing Bar' component. | 📋 To Do | 1.14 | |
| **1.16** | 1: MVP | Testing & Checks | Define and document the process for validating AI-generated insights. | 📋 To Do | 1.6, 1.7 | |
| **1.17** | 1: MVP | Testing & Checks | Write unit tests for all key services and utility functions. | 📋 To Do | 1.4 | [cite_start]Adhere to TEST-01 rule[cite: 9439]. |
| **1.18** | 1: MVP | Testing & Checks | Perform full accessibility audit on the dashboard. | 📋 To Do | 1.15 | |
| **1.19** | 1: MVP | Testing & Checks | Conduct performance testing with Lighthouse. | 📋 To Do | 1.15 | |
| **2.1** | 2: Should Have | Implementation | Create the 'Detailed Analytics View' page. | 📋 To Do | 1.5 | |
| **2.2** | 2: Should Have | Implementation | Implement the 'Anomaly Alert' Monitor component. | 📋 To Do | 2.1 | |
| **2.3** | 2: Should Have | Implementation | Implement the 'Geographic "Hotspot"' card. | 📋 To Do | 2.1 | |
| **2.4** | 2: Should Have | Implementation | Implement the 'Most Profitable Hour' card. | 📋 To Do | 2.1 | |
| **2.5** | 2: Should Have | Implementation | Implement the 'Device Snapshot' card. | 📋 To Do | 2.1 | |
| **2.6** | 2: Should Have | Implementation | Implement the 'Performance Heatmap' component. | 📋 To Do | 2.1 | |
| **2.7** | 2: Should Have | Implementation | Implement tooltips for all data visualisations. | 📋 To Do | 2.6 | |
| **3.1** | 3: Could Have | Implementation | Implement the "'Unsung Heroes' Report" component. | 📋 To Do | 2.1 | Based on PRD feature list. |
| **3.2** | 3: Could Have | Implementation | Implement the "'Audience Persona Generator'" component. | 📋 To Do | 2.1 | Synthesises data into a simple user persona. |
| **3.3** | 3: Could Have | Implementation | Implement "'The Customer Journey Story'" view. | 📋 To Do | 2.1 | Creates a storyboard-style view. |
| **3.4** | 3: Could Have | Implementation | Implement "'The Market Pulse' Echo" component. | 📋 To Do | 2.1 | Data source must be a hard-coded whitelist. |
| **3.5** | 3: Could Have | Research | Research and select a library for an interactive dial component. | 📋 To Do | - | Required for 'The Budget Dial' Simulator. |
| **3.6** | 3: Could Have | Implementation | Implement "'The Budget Dial'" Simulator component. | 📋 To Do | 2.1, 3.5 | An interactive tool for ad spend projections. |
| **4.1** | 4: Deployment | Deployment | Configure Vercel for production deployment. | 📋 To Do | 3.6 | Final step to launch the application. |

