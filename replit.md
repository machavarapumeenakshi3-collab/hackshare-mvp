# Hackathon Discovery Platform

## Overview

A full-stack web application for discovering and submitting hackathons. Users can browse upcoming tech events, view AI-generated summaries about whether events are worth participating in, see event locations on embedded maps, and submit new hackathons. The platform uses Gemini AI to automatically generate event summaries when hackathons are created.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight React router)
- **State Management**: TanStack React Query for server state, React Hook Form for form state
- **Styling**: Tailwind CSS with shadcn/ui component library (New York style variant)
- **Animations**: Framer Motion for page transitions and interactions
- **Build Tool**: Vite with path aliases (@/ for client/src, @shared/ for shared)

### Backend Architecture
- **Runtime**: Node.js with Express
- **Language**: TypeScript with ESM modules
- **API Design**: REST endpoints defined in shared/routes.ts with Zod validation
- **Build**: esbuild bundles server code for production

### Data Storage
- **Database**: PostgreSQL with Drizzle ORM
- **Schema Location**: shared/schema.ts contains table definitions
- **Migrations**: Drizzle Kit manages database migrations (output to /migrations)

### AI Integration
- **Provider**: Google Gemini via Replit AI Integrations
- **Models Available**: gemini-2.5-flash (fast), gemini-2.5-pro (reasoning), gemini-2.5-flash-image (image generation)
- **Usage**: Generates hackathon summaries on event creation
- **Configuration**: Uses AI_INTEGRATIONS_GEMINI_API_KEY and AI_INTEGRATIONS_GEMINI_BASE_URL environment variables

### Project Structure
```
├── client/src/          # React frontend
│   ├── components/      # UI components (shadcn/ui based)
│   ├── hooks/           # Custom React hooks
│   ├── pages/           # Route page components
│   └── lib/             # Utilities and query client
├── server/              # Express backend
│   ├── routes.ts        # API route handlers
│   ├── storage.ts       # Database access layer
│   └── replit_integrations/  # AI feature modules (chat, image, batch)
├── shared/              # Shared between client and server
│   ├── schema.ts        # Drizzle database schema
│   ├── routes.ts        # API route definitions with Zod schemas
│   └── models/          # Additional data models
└── migrations/          # Database migration files
```

### Key Design Patterns
- **Shared Types**: Database schemas and API types are defined once in /shared and used by both client and server
- **Type-safe API**: Route definitions include input validation schemas and response types
- **Storage Abstraction**: DatabaseStorage class implements IStorage interface for data access
- **Component Library**: shadcn/ui provides accessible, customizable base components

## External Dependencies

### Database
- **PostgreSQL**: Primary database (connection via DATABASE_URL environment variable)
- **Drizzle ORM**: Type-safe database queries and schema management

### AI Services
- **Replit AI Integrations**: Provides Gemini API access
  - Requires AI_INTEGRATIONS_GEMINI_API_KEY
  - Requires AI_INTEGRATIONS_GEMINI_BASE_URL
  - Supports text generation and image generation

### Frontend Libraries
- **@tanstack/react-query**: Server state management and caching
- **react-hook-form + @hookform/resolvers**: Form handling with Zod validation
- **date-fns**: Date formatting utilities
- **framer-motion**: Animation library
- **lucide-react**: Icon library

### Map Integration
- **Google Maps Embed**: Uses embedded Google Maps iframes for location display
- **Browser Geolocation API**: Calculates distance from user to event location