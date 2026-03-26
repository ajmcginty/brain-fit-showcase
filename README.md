# BrainFit — Cognitive Health Tracker

A full-stack mobile application built for a private client, helping adults 50+ build daily habits across five pillars of cognitive health.

> **Role**: Sole developer — designed, built, and shipped end-to-end
> **Stack**: React Native · TypeScript · Firebase · Expo

---

## Screenshots

<!-- HOME SCREEN — Daily goal progress rings -->
> 📸 *Screenshot: Home screen with progress rings*

&nbsp;

<!-- GOAL DETAIL MODAL — Logging a specific goal -->
> 📸 *Screenshot: Goal detail modal (e.g. logging exercise minutes)*

&nbsp;

<!-- WEEKLY CALENDAR — Completion history view -->
> 📸 *Screenshot: Weekly calendar showing daily streaks*

&nbsp;

<!-- PROGRESS / WEEKLY SUMMARY — Stats and comparisons -->
> 📸 *Screenshot: Weekly summary report with week-over-week comparison*

&nbsp;

<!-- ARTICLES / LEARN SCREEN — Educational resources -->
> 📸 *Screenshot: Learn screen with curated articles and category filters*

&nbsp;

<!-- AUTH SCREEN — Sign in / Sign up -->
> 📸 *Screenshot: Authentication screen*

---

## What It Does

BrainFit tracks five evidence-based pillars of cognitive health — daily. Users log each pillar, build streaks, and review weekly progress reports with comparisons to the prior week.

| Pillar | Metrics |
|---|---|
| Exercise | Duration, weekly goal (150 min/week) |
| Cognitive Stimulation | Activities completed, daily challenges |
| Social Engagement | Interactions, new connections |
| Sleep Quality | Hours slept, quality rating |
| Nutrition | Meal quality (1–5 scale), healthy meal count |

---

## Technical Highlights

### Offline-First Architecture
The app is fully functional without an internet connection. Goals are written to local AsyncStorage immediately, then synced to Firebase Firestore in the background. On login, local and cloud data are merged with conflict resolution logic — cloud data takes precedence if it's newer.

### Per-User Data Isolation
Early in development I identified and fixed a critical data leak: local storage was keyed by `deviceId` instead of `Firebase UID`, meaning a second user logging in on the same device could read the first user's data. I re-scoped all storage keys to the authenticated UID and added store cleanup on logout to prevent leakage between sessions.

### Firebase Security Rules
Firestore rules enforce server-side ownership — users can only read or write documents under their own UID. This means even if client-side validation were bypassed, the database would reject unauthorized requests.

```javascript
match /goals/{uid}/daily/{date} {
  allow read, write: if request.auth != null && request.auth.uid == uid;
}
```

### Secure Account Deletion
Implemented full account deletion that removes all user data — both from Firestore and local storage — before deleting the Firebase Auth account. This ensures no orphaned data remains after a user deletes their account.

### State Management with Zustand
Separate stores for auth, profile, goals, and articles. Each store handles its own persistence and Firebase sync logic, keeping concerns isolated. Auth state drives the root navigation, switching between the auth flow and the main app in response to Firebase listener events.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | React Native + Expo |
| Language | TypeScript |
| Navigation | React Navigation v7 |
| State | Zustand |
| Local Storage | AsyncStorage |
| Backend | Firebase Auth + Firestore |
| Testing | Jest + jest-expo |

---

## Architecture Overview

```
src/
├── components/     # Reusable UI (ProgressRing, WeeklyCalendar, GoalDetailModal...)
├── screens/        # HomeScreen, GoalsScreen, ProfileScreen, InfoScreen, AuthScreen
├── services/       # Firebase auth, Firestore sync, local storage, data migration
├── store/          # Zustand: authStore, goalsStore, profileStore, articleStore
├── types/          # Shared TypeScript interfaces
├── utils/          # Date helpers, goal utilities
└── constants/      # Theme, colors, typography
```

---

## Source Code

The source code for this project is private as it was built for a client.
Feel free to reach out if you'd like to discuss the implementation in more detail.

**Aidan McGinty** · ajmcginty1@gmail.com · [github.com/ajmcginty](https://github.com/ajmcginty)
