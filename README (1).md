# Zoomies 🚗

**Same campus. Same destination. One ride.**

Zoomies is a campus-first travel coordination platform that helps verified college students find people travelling to the same destination around the same date and time. Students can offer available seats in a personal vehicle or an arranged cab, or post a travel intention when they still need transportation.

> **Project status:** Product planning / pre-MVP  
> **Initial launch:** One college, with an architecture designed to expand to multiple colleges  
> **Working name:** Zoomies

## Table of contents

- [Problem](#problem)
- [Solution](#solution)
- [Product principles](#product-principles)
- [Users and listing types](#users-and-listing-types)
- [Core features](#core-features)
- [Primary user journeys](#primary-user-journeys)
- [Matching](#matching)
- [MVP scope](#mvp-scope)
- [Proposed technology stack](#proposed-technology-stack)
- [High-level architecture](#high-level-architecture)
- [Initial data model](#initial-data-model)
- [Security and privacy](#security-and-privacy)
- [Success metrics](#success-metrics)
- [Roadmap](#roadmap)
- [Non-goals for the MVP](#non-goals-for-the-mvp)

## Problem

College students often travel to airports, railway stations, bus terminals, and their hometowns at similar times. Despite overlapping travel plans, they may not know who else is going the same way.

Today, coordinating through informal group chats is difficult: travel details are scattered, schedules change, and students may end up arranging separate rides and paying more individually.

## Solution

Zoomies provides a centralized, college-first place to:

- Publish a ride with available seats.
- Post a travel intention when transportation has not yet been arranged.
- Discover compatible students and rides by destination, date, and time.
- Request to join a ride and coordinate with its provider.
- Find other students with whom to arrange a shared cab.
- Agree on pickup points, departure details, and expense sharing.

**Zoomies is a student-to-student travel coordination platform, not a taxi-booking or transportation-operator service.**

## Product principles

1. **Campus-first trust:** Start with verified members of one college.
2. **Two ways to participate:** Students can offer transportation or look for companions.
3. **Choice:** Support both automatic suggestions and manual search.
4. **Flexible transport:** Support personal vehicles and shared cabs.
5. **Simple payments:** No in-app payment collection in the MVP; students coordinate expenses directly.
6. **Expandable foundation:** Model colleges explicitly so more institutions can be added later.

## Users and listing types

A student can act as either role on a particular trip; roles are not permanent account types.

### 1. Offer a ride (`OFFER`)

For a student who has a personal vehicle or has already arranged a cab and wants others to share the journey.

Typical fields:

- Origin and destination
- Travel date and departure time
- Flexible departure window (optional)
- Transport type: personal car or shared cab
- Available seats
- Estimated cost per passenger (optional)
- Vehicle or arrangement details (as appropriate)
- Notes and pickup instructions

The listing creator manages requests and confirms participants.

### 2. Looking for a ride (`LOOKING`)

For a student who knows where and when they want to travel but has not arranged transportation.

Typical fields:

- Origin and destination
- Travel date
- Preferred departure time and flexibility
- Number of travellers
- Optional budget per person
- Notes

A `LOOKING` listing can match with an `OFFER` listing or with other `LOOKING` listings so students can coordinate a shared cab together.

## Core features

### Authentication and college membership

- Email-based registration and login.
- College email verification for the initial institution.
- User profile with name, profile photo, college, and optional course/year.
- College membership and permissions validated on the server.

### Create and manage listings

- Create, edit, cancel, and view `OFFER` and `LOOKING` listings.
- Specify origin, destination, date, time, and time flexibility.
- For offers, specify transport type and available seats.
- Optionally provide an estimated per-person cost.
- Track listing status (for example: open, full, cancelled, completed).

### Discover travel plans

- Manual search by destination, origin, date, time, transport type, seats, and optional budget.
- Automatic suggestions for compatible listings.
- Clear distinction between an available ride and another student's travel intention.
- Listing details and relevant creator profile information.

### Requests and participant management

- Request to join an offered ride.
- Creator can accept or reject requests.
- Users can cancel their own pending requests.
- Accepted participants are recorded against the listing.
- Available seats update safely and cannot become negative.
- Prevent duplicate active requests from the same student for the same listing.

### Coordination

- Ride-specific messaging for confirmed or otherwise authorized participants.
- Discuss pickup points, timing, and expenses.
- Notifications for requests, decisions, listing changes, and messages.

### Safety and reporting

- Report and block users.
- Restrict private ride details and conversations to authorized users.
- Provide cancellation and participant-removal flows.
- Make it clear that college email verification is not a guarantee of personal or driving safety.

## Primary user journeys

### Journey A: Offer transportation

1. Student signs in.
2. Selects **Offer a Ride**.
3. Enters origin, destination, date, and departure time.
4. Selects personal car or arranged cab.
5. Enters available seats and optional estimated cost.
6. Publishes the listing.
7. Reviews incoming join requests.
8. Accepts suitable requests; the system reserves seats safely.
9. Coordinates trip details with participants.

### Journey B: Find transportation

1. Student signs in.
2. Selects **Find a Ride**.
3. Enters origin, destination, date, and preferred time.
4. Reviews manual search results and automatic suggestions.
5. Requests to join an available ride, or connects with other students looking for the same journey.
6. Coordinates a shared cab if no suitable offered ride exists.

## Matching

Matching works in two directions:

- **Offer-to-looking:** Find `LOOKING` students compatible with an `OFFER`.
- **Looking-to-looking:** Find students with compatible travel intentions who may arrange a shared cab together.

Initial matching factors:

1. Destination compatibility
2. Same travel date
3. Departure-time compatibility and flexibility
4. Origin / pickup proximity
5. Available capacity for offered rides
6. Budget compatibility when users provide a budget or estimated cost

The matching time window and ranking weights should be configurable, not hardcoded as one universal rule. Search should return useful results even when no automatic match is generated.

A match is only a suggestion. It is **not** a booking or confirmation. Participants must agree on the arrangement. A `LOOKING` group still needs to arrange its transportation.

## MVP scope

### P0 — Required for initial campus pilot

- College email authentication and membership.
- Student profiles.
- `OFFER` and `LOOKING` listings.
- Personal car and shared-cab transport types.
- Manual search and filters.
- Automatic matching suggestions.
- Join requests and creator approvals.
- Transaction-safe seat allocation.
- Listing lifecycle and cancellation.
- Basic in-app coordination channel or messaging.
- Essential notifications.
- Report and block functionality.
- Server-enforced access controls.

### P1 — Important enhancements

- Map-based pickup selection.
- Ratings and reviews.
- Improved notification preferences.
- Better match explanations and ranking.
- More detailed trip management.

### P2 — Later expansion

- Live location sharing.
- Integrated payment and expense splitting.
- Multi-college discovery.
- More advanced matching and recommendations.

## Proposed technology stack

| Layer | Proposed technology |
|---|---|
| Mobile app | React Native + Expo |
| Backend platform | Supabase |
| Database | PostgreSQL |
| Authentication | Supabase Auth |
| Realtime features | Supabase Realtime |
| File storage | Supabase Storage |
| Maps and places | Google Maps Platform (evaluate requirements and cost) |
| Push notifications | Expo Notifications |

These are proposed choices and can be revised during technical design.

## High-level architecture

```text
React Native / Expo mobile app
          |
          v
       Supabase
          |
          +-- Auth and college membership
          +-- PostgreSQL database
          +-- Row Level Security policies
          +-- Server-side functions / transactions
          +-- Realtime messaging and updates
          +-- Storage for profile images
          |
          +-- Matching queries and ranking
          +-- Push notification integration
          +-- Maps / places integration
```

### Important architecture decisions

- Use one common `travel_listings` table with a `listing_type` field (`OFFER` or `LOOKING`) rather than two disconnected listing systems.
- Include `college_id` on users and listings from the beginning. The initial app can restrict discovery to one college while keeping the data model multi-college-ready.
- Enforce college membership, listing permissions, and conversation access on the server with PostgreSQL Row Level Security (RLS).
- Use an atomic database transaction or server-side function when approving requests and reserving seats. Client-side seat checks alone are not sufficient to prevent overbooking.
- Treat matches as suggestions, not confirmed rides.
- Keep payments outside the MVP.

## Initial data model

The following entities are expected. Exact columns, constraints, indexes, and relationships will be defined in the technical design.

| Entity | Purpose |
|---|---|
| `colleges` | Institution records and verified email domains |
| `users` | Student profiles and college membership |
| `travel_listings` | `OFFER` and `LOOKING` travel listings |
| `join_requests` | Requests to join an `OFFER` |
| `ride_members` | Confirmed participants and seat reservations |
| `conversations` | Listing-related conversations |
| `messages` | Messages within conversations |
| `notifications` | User notification records |
| `reports` | Safety and moderation reports |
| `blocks` | User blocking relationships |

Potential `travel_listings` fields include:

- `id`
- `college_id`
- `creator_user_id`
- `listing_type` (`OFFER` / `LOOKING`)
- `origin` and `destination` (structured place references where available)
- `travel_date`
- `departure_time`
- `time_flexibility_minutes`
- `transport_type` (for offers)
- `available_seats` (for offers)
- `passenger_count` (for looking listings)
- `estimated_cost_per_person` (optional)
- `status`
- `created_at` and `updated_at`

## Security and privacy

- Apply Row Level Security to all user and trip data.
- Validate college membership server-side; do not trust a client-supplied `college_id`.
- Restrict editing to listing creators and authorized administrators.
- Restrict join-request visibility to the requester and relevant listing creator.
- Restrict private trip details and conversations to authorized participants.
- Do not expose personal phone numbers by default.
- Support reporting, blocking, cancellation, and moderation.
- Protect seat allocation against concurrent approvals and overbooking.
- Collect only the personal data needed to operate the service.

## Success metrics

Track the health of the campus pilot using:

- Verified student registrations.
- Active students per week.
- Number of `OFFER` and `LOOKING` listings created.
- Search-to-request conversion rate.
- Number of accepted join requests.
- Number of completed trips (where completion can be reliably recorded).
- Time from listing creation to a useful match.
- Repeat usage.
- Cancellation and report rates.

## Roadmap

1. **Foundation:** Expo project, Supabase setup, schema, authentication, college membership, profiles.
2. **Listings:** Create, edit, cancel, and view `OFFER` and `LOOKING` listings.
3. **Discovery:** Manual search, filters, automatic matching suggestions.
4. **Coordination:** Join requests, safe seat allocation, messaging, notifications.
5. **Pilot:** Security testing, usability testing, and a controlled launch within one college.
6. **Expansion:** Maps, ratings, multi-college discovery, and other enhancements.

## Non-goals for the MVP

- Zoomies will not dispatch or operate vehicles.
- Zoomies will not book taxis on behalf of users.
- Zoomies will not collect, hold, or transfer ride payments.
- Zoomies will not guarantee a match or a ride.
- Zoomies will not launch with unrestricted cross-college discovery.
- Zoomies will not require live tracking.

---

## Contributing

This project is currently in product-planning / pre-MVP stage. Development conventions, setup instructions, issue templates, and contribution guidelines will be added as implementation begins.

## License

No license has been selected yet. Until one is added, all rights are reserved by the project owner.
