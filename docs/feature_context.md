This file lists the key project files and directories that are relevant for the implementation of the Google Ads Campaign Dashboard feature.

### Core Application Structure
- **/app/**: Core application routes.
- **/app/(dashboard)/page.tsx**: The default "Primary Insights View".
- **/app/(dashboard)/detailed-analytics/page.tsx**: The "Detailed Analytics View".
- **/app/layout.tsx**: Root layout, including global styles and context providers.
- **/app/api/**: Directory for all secure backend serverless functions.

### Components
- **/components/ui/**: Contains core UI elements based on the Design System Profile (e.g., Button, Card).
- **/components/charts/**: Contains all data visualisation components (e.g., FlowFunnel, BudgetPacingBar, PerformanceHeatmap).
- **/components/dashboard/**: Contains composite components specific to the main dashboard view (e.g., PlainEnglishSummary, OpportunitySpotlight).
- **/components/analytics/**: Contains components for the detailed analytics view (e.g., GeoHotspotCard, DeviceSnapshotCard).

### Shared Logic & State
- **/lib/**: Contains helper functions, utility scripts, and third-party API clients.
- **/lib/hooks.ts**: Location for custom client-side data fetching hooks using SWR.
- **/lib/actions.ts**: Location for Next.js Server Actions.
- **/lib/utils.ts**: General utility functions.
- **/context/**: Contains React Context providers for global state if needed (e.g., user session, feature flags).

### Data & Configuration
- **/prisma/schema.prisma**: The single source of truth for the database schema.
- **.env.example**: Template for required environment variables (e.g., database URL, Auth0 credentials, Google API keys).
- **package.json**: Project dependencies and scripts.
- **tailwind.config.ts**: Tailwind CSS configuration file.
- **.cursor/rules/**: Directory for custom AI behaviour rules.
