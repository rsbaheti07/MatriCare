# MatriCare - Premium Pregnancy Companion App

## Overview
MatriCare is a premium, scalable pregnancy companion app tailored for the Indian market, built with modern web technologies following best practices for performance, security, and user experience.

## Tech Stack
- **Framework**: Next.js 14+ (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **State Management**: 
  - Zustand (global UI/session state)
  - React Query (server-state/caching)
- **Animations**: Framer Motion
- **UI Components**: Radix UI primitives
- **Backend**: Supabase (Auth, Database, Storage)
- **Icons**: Lucide React

## Key Features Implemented

### 1. Authentication & Onboarding
- Phone number OTP authentication (optimized for Indian market)
- Email/password authentication
- Multi-step onboarding flow capturing:
  - Due Date/LMP
  - Pregnancy Number (Gravida/Para)
  - Dietary preferences (Veg/Non-Veg/Jain)
- State persistence using Zustand middleware

### 2. Dashboard
- Personalized welcome message
- Pregnancy week tracker
- Quick stats overview (vitals, appointments, community)
- Quick action buttons
- Recent activity feed

### 3. Medical Logs
- Vitals logger (BP, Weight, Blood Sugar)
- Form validation with realistic ranges
- Data persistence to Supabase
- Historical tracking with visualization
- Toast notifications for user feedback

### 4. Community Features
- Role-based access (Users vs. Verified Doctors - foundation laid)
- Post creation and sharing
- Post listing with user avatars
- Like, comment, share functionality
- Public/private post options

### 5. Partner Sync
- Unique invite code system
- Partner invitation flow
- Invitation acceptance with code verification
- Expiry handling (7 days)
- Connection status tracking

### 6. Security & Privacy
- Row-Level Security (RLS) ready via Supabase
- "Cloak Mode" foundation via ThemeProvider (dark mode toggle)
- Secure authentication flows
- Input validation and sanitization

### 7. Localization & Cultural Adaptation
- Indian date formats (en-IN)
- Culturally relevant dietary preferences
- Foundation for Hindi/English i18n
- Traditional milestone tracking ready for expansion

## Folder Structure
```
src/
├── app/                    # Next.js App Router
│   ├── (auth)/             # Auth routes (login, etc.)
│   ├── dashboard/          # Dashboard protected routes
│   ├── onboarding/         # Onboarding flow
│   ├── community/          # Community features
│   ├── medical-logs/       # Medical tracking
│   ├── partner-sync/       # Partner connections
│   ├── layout.tsx          # Root layout with providers
│   └── globals.css         # Global styles & theme variables
├── components/
│   ├── ui/                 # Reusable UI primitives (Button, Card, etc.)
│   └── layout/             # Layout components
├── features/               # Feature-based organization
│   ├── onboarding/
│   ├── medical-logs/
│   ├── community/
│   └── partner-sync/
├── lib/
│   ├── providers/          # React Context providers
│   ├── stores/             # Zustand stores
│   ├── utils/              # Utility functions
│   └── supabaseClient.ts   # Supabase initialization
├── hooks/                  # Custom React hooks
├── styles/                 # Additional styles
└── types/                  # TypeScript type definitions
```

## Design System
- **Color Scheme**: Soft pinks and violets for premium feel
- **Typography**: Geist font family (optimized for readability)
- **Effects**: Glassmorphism, smooth animations, hover states
- **Accessibility**: Proper contrast, ARIA labels foundation
- **Responsiveness**: Mobile-first design for diverse Indian device landscape

## Getting Started
1. Install dependencies: `npm install`
2. Set up Supabase project and add environment variables:
   ```
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
   ```
3. Run database migrations (SQL files would be provided separately)
4. Start development server: `npm run dev`

## Future Enhancements
- Hindi/English i18n implementation
- Advanced analytics and insights engine
- Video consultation integration
- Medicine reminder system
- Garbh Sanskar (traditional practices) guidance
- Offline data synchronization
- Push notifications for important milestones

## Code Quality
- All components follow reusable patterns (no single-use components)
- Error boundaries implemented in data fetching
- Comprehensive type safety with TypeScript
- Modular, maintainable architecture
- JSDoc documentation on complex functions
- Consistent coding standards