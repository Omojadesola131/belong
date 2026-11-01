# BELONG

**Find your people. Find your purpose. Make a difference.**
*(Proposed brand name, subject to trademark and domain checks.)*

A cross-platform mobile app (iOS, Android and web) built with **Expo + React Native + TypeScript + expo-router**, in **English and German** with a toggle in every header.

## What's in this first version

| Tab | What it does |
|---|---|
| **Give Back** | Volunteering opportunities nearby, with search and filters (*Under 2 hours*, *Online*, *My activities*). |
| **Meet People** | Community groups, friendships and social activities. |
| **Join Events** | Cultural events, workshops and community gatherings. |
| **Start Something** | Create your own local social-impact initiative. |
| **My Impact** | The **Belonging Passport**: impact hours, verified activities, achievements, contribution history, and an opt-in share button. |

### The Belonging Passport – participation flow
1. **Join** an activity →
2. after it, tap **I took part** (status: *Awaiting confirmation*) →
3. the **organiser verifies** it (status: *Verified*). Only verified activities count towards hours and achievements.

In this prototype, step 3 is simulated with a *“Demo: organiser confirms”* button. In production it moves to an organiser dashboard (see Next steps).

The app starts with three sample past activities (two verified, one pending) so the Passport isn't empty. *My Impact → Reset demo data* restores them.

### Fair by design (no competition)
- **No leaderboards, rankings or comparisons** with other people. The Passport is private; sharing is opt-in.
- **Achievements reward taking part and variety, not volume.** Every badge can be earned with a few hours; there are no “Gold / 100 hours” tiers.
- **Low-time options are first-class:** filters for *Under 2 hours* and *Online* make it easy to take part with little free time.
- Copy reinforces it: *“Every contribution counts the same, whether it is one hour or one hundred.”*

Achievements: First Step, Community Helper (3 activities), Green Champion, Learning Supporter, Connector, Culture Explorer, Changemaker. Rules live in `src/lib/achievements.ts`.

## Run it

You need [Node.js](https://nodejs.org) (LTS) and the **Expo Go** app on your phone.

```bash
cd belong
npm install
npx expo install --fix     # aligns package versions with your installed Expo SDK
npx expo start
```
Scan the QR code with Expo Go (Android) or the Camera app (iPhone). Press `w` to open it in a browser instead.

Type-check: `npm run typecheck`

> Versions target Expo SDK 54. If a newer SDK is current, run `npx expo install expo@latest` and then `npx expo install --fix`.

## Project structure

```
app/                     screens (expo-router: file = route)
  _layout.tsx            providers + stack navigator
  (tabs)/_layout.tsx     5-tab bar, header with EN/DE toggle
  (tabs)/index.tsx       Give Back
  (tabs)/meet.tsx        Meet People
  (tabs)/events.tsx      Join Events
  (tabs)/start.tsx       Start Something
  (tabs)/impact.tsx      My Impact / Belonging Passport
  activity/[id].tsx      activity detail + join / took part / verify
src/
  data/types.ts          data model (Activity, Participation, Initiative)
  data/activities.ts     sample activities (fictional organisers, Rhein-Neckar area)
  i18n/translations.ts   all UI text, EN + DE
  i18n/LanguageContext.tsx  language state, t(), date formatting
  store/AppStore.tsx     app state, saved on device (AsyncStorage)
  lib/achievements.ts    Passport maths + achievement rules
  components/            cards, list, buttons, chips, tags
  theme.ts               colours, spacing, radii
```

**Adding a language:** add a dictionary in `translations.ts`, extend `Locale` in `types.ts`, add the new key to each activity's `title`/`description`, and add it to `LanguageToggle`.

## Next steps (suggested roadmap)

1. **Backend & accounts** – e.g. Supabase or Firebase: users, activities, participations, initiatives. Replace `ACTIVITIES` and `AppStore` persistence with API calls.
2. **Organiser side** – a web dashboard where organisations publish activities and confirm attendance (QR check-in at the event is a simple, trusted option). This replaces the demo verify button.
3. **Verified certificates** – organisations issue a signed PDF certificate per activity or per year; later, Open Badges / verifiable credentials.
4. **Location** – “near me” sorting with `expo-location` and a map view.
5. **Moderation & safety** – review new initiatives before they go public; reporting; safeguarding requirements for activities with children.
6. **Accessibility & inclusion** – screen-reader pass, step-free access info on activities, plain-language mode, more languages (e.g. Turkish, Arabic, Ukrainian for the Rhein-Neckar region).
7. **CSR reporting for companies (later)** – employees opt in to share aggregated, anonymised hours with their employer; export aligned with reporting frameworks. Keep it strictly opt-in and aggregated so it never becomes a performance metric.
8. **Privacy / GDPR** – data minimisation, consent screens, data export and deletion.
