# Improvement Plan for Pregnancy App

Based on code review, the following suggestions are identified for improving the pregnancy app.

## High Priority

1. **Configure Supabase environment variables**
   - Location: `.env.local` (missing) and `src/lib/supabaseClient.ts`
   - Action: Add `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` to environment variables.
   - Steps:
     - Obtain Supabase project URL and anon key from Supabase dashboard.
     - Create or update `.env.local` with:
       ```
       NEXT_PUBLIC_SUPABASE_URL=your_project_url
       NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
       ```
     - Ensure the variables are loaded in Next.js (they are prefixed with NEXT_PUBLIC).
     - Verify that the supabase client initializes correctly.

2. **Replace hardcoded stats in Dashboard with real data**
   - Location: `src/app/dashboard/page.tsx` (lines 94-124)
   - Action: Implement Supabase queries to fetch actual counts for vitals logged, appointments, and community posts.
   - Steps:
     - Create Supabase queries (or use existing hooks) to get counts from respective tables.
     - Replace the hardcoded "0" values with dynamic data.
     - Add loading states while fetching data.

3. **Fix weeks pregnant calculation to avoid absolute value issue**
   - Location: `src/lib/utils/dateCalculations.ts` (line 6) and `src/app/dashboard/page.tsx` (line 17)
   - Action: Remove `Math.abs` and ensure we only calculate weeks when due date is in the future. If due date is past, show 0 or indicate postpartum.
   - Steps:
     - Update `calculateWeeksPregnant` in utils to compute diffTime as `dueDate.getTime() - date.getTime()` (if dueDate >= date) else negative.
     - Return 0 if diffTime < 0 (or handle as needed).
     - Update dashboard and any other usage accordingly.

4. **Remove duplicate weeks pregnant storage in localStorage**
   - Location: `src/features/onboarding/components/OnboardingFlow.tsx` (lines 14-25)
   - Action: Remove the `useEffect` that stores weeks pregnant in localStorage.
   - Steps:
     - Delete the useEffect block.
     - Remove any references to `localStorage.getItem('weeksPregnant')` if they exist elsewhere (check).

5. **Improve onboarding validation for gravida**
   - Location: `src/features/onboarding/components/OnboardingFlow.tsx` (line 109 min="0")
   - Action: Change min attribute to "1" because gravida should be at least 1 (current pregnancy).
   - Steps:
     - Update Input min="1".
     - Adjust validation in `isFormValid()` to check `>=1`.

6. **Add is_public column to community_posts table**
   - Location: Migration needed; referenced in `src/features/community/components/NewPostForm.tsx` line 36 comment.
   - Action: Alter the Supabase table to add a boolean column `is_public` default true.
   - Steps:
     - Use Supabase migration or SQL editor to add column.
     - Ensure the insert in NewPostForm includes the column (already done).

7. **Fix dietary preference type mismatch**
   - Location: `src/features/nutrition/utils/chartData.ts` line 92-94
   - Action: Ensure the dietaryType type matches the actual options from onboarding (veg, non-veg, jain). Or update onboarding to include vegan and eggitarian if intended.
   - Steps:
     - Check product requirements: if only veg, non-veg, jain are supported, adjust the type and remove vegan/eggitarian from the union.
     - Alternatively, add those options to the onboarding select.

## Medium Priority

8. **Implement recent activity feed in Dashboard**
   - Location: `src/app/dashboard/page.tsx` lines 196-211
   - Action: Replace placeholder with actual recent activity (e.g., latest vitals logs, new posts, appointments).
   - Steps:
     - Fetch recent activity from Supabase (maybe from a unified activity log or individual tables).
     - Display in a list with timestamps and icons.

9. **Make quick action buttons functional**
   - Location: `src/app/dashboard/page.tsx` lines 159-191
   - Action: Replace outline buttons with actual navigation or modals.
   - Steps:
     - Link Log Vitals button to vitals logging page or open a vitals log modal.
     - Link Join Community button to community feed.
     - Link Read Articles button to articles section (if exists) or external resources.

10. **Create missing Label component (if not already existing)**
    - Location: Referenced in `src/features/medical-logs/components/VitalsForm.tsx` line 6 comment.
    - Action: Verify if `@/components/ui/label` exists; if not, create a simple label component.
    - Steps:
      - Check `src/components/ui/label.tsx`.
      - If missing, create it (similar to other UI components).

11. **Implement food/medical logs integration for nutrition intake**
    - Location: `src/features/nutrition/utils/chartData.ts` line 22 comment.
    - Action: Replace the placeholder `getNutritionalIntake()` with actual data fetching from food logs or medical logs.
    - Steps:
      - Define schema for food intake logs.
      - Create Supabase queries to aggregate nutritional intake per day/week.
      - Update `getNutritionalIntake` to return actual intake.

## Low Priority

12. **Add unit selection for vitals (optional)**
    - Location: `src/features/medical-logs/components/VitalsForm.tsx`
    - Action: Allow users to switch between metric and imperial units for weight, blood pressure, etc.
    - Steps:
      - Add toggle for unit system.
      - Convert values before storing to Supabase (store in metric units).
      - Display appropriate units.

13. **Expand dietary preference options**
    - Location: `src/features/onboarding/components/OnboardingFlow.tsx` lines 149-153
    - Action: Add more options like vegan, eggitarian, etc., if required.
    - Steps:
      - Update select options.
      - Ensure nutrition calculations handle the new types.

14. **Add ability to edit onboarding profile after completion**
    - Location: Need to create a profile edit screen.
    - Action: Allow users to update due date, gravida, para, dietary preference.
    - Steps:
      - Create a profile edit page accessible from settings.
      - Fetch current profile from Supabase (or onboarding store).
      - Allow updates and sync with Supabase.

15. **Implement real-time updates for dashboard stats**
    - Location: `src/app/dashboard/page.tsx`
    - Action: Use Supabase subscriptions to refresh stats when data changes.
    - Steps:
      - Set up Supabase channels for vitals_logs, appointments, community_posts tables.
      - Refetch counts on change.

## Implementation Order

We recommend tackling high priority items first, as they affect core functionality and data accuracy.

### Suggested Tasks

1. Configure Supabase environment variables
2. Fix weeks pregnant calculation (utils and dashboard)
3. Remove localStorage weeks storage
4. Update onboarding gravida validation
5. Add is_public column to community_posts
6. Fix dietary preference type mismatch
7. Replace hardcoded stats with real data
8. Implement recent activity feed
9. Make quick action buttons functional
10. Create Label component if missing
11. Integrate actual nutritional intake data
12. Add profile edit screen
13. Add unit selection (optional)
14. Expand dietary options
15. Implement real-time updates

Each task can be broken down into sub-tasks as needed.

## Next Steps

Review this plan with the team/product owner to prioritize and assign tasks.