# material-videos

A personal YouTube client for the web (PWA) and Android, built with the YouTube Data API v3, the YouTube IFrame Player and Firebase. The goal is a calmer, more personal and better-designed way to watch YouTube than the official app. It is for one user (me), not a commercial product.

This README is the design brief for Claude Design.

---

## 1. Design goals

1. **Calm, not addictive.** No infinite rabbit holes, no clickbait-first layouts. The feed shows what I chose to follow, in an order I control.
2. **Personal.** My channel groups, my filters, my notes and history are first-class.
3. **Beautiful and expressive.** Material 3 Expressive, dynamic color, bold shape and motion, done with restraint.
4. **Fast to scan.** Clear hierarchy on cards: thumbnail, title, channel, duration, age, progress.
5. **One design, three sizes.** It works equally well on a phone, a tablet/foldable and a desktop browser.

## 2. Visual direction: Material 3 Expressive

- **Color:** Material You dynamic color. On Android it comes from the wallpaper. On the web the user picks a seed color, with a fallback seed of deep red-coral (avoid copying YouTube's exact red). It needs full light and dark schemes and an optional pure-black (AMOLED) dark mode.
- **Shape:** expressive shape scale with large rounded corners on cards and sheets (28 dp). Use shape morphing on key interactions, for example the play button morphing from a circle to a squircle, or a chip becoming selected.
- **Typography:** Roboto Flex / Google Sans-style variable type. Use emphasized (heavier) styles for section headers and video titles, and a regular weight for metadata.
- **Motion:** spring-based expressive motion. A shared-element transition takes a feed thumbnail into the player. Use a container transform for opening channels and playlists, and subtle physics on pull-to-refresh and the FAB.
- **Components:** use M3 components wherever possible: navigation bar/rail, top app bars (large/collapsing), cards, filter chips, segmented buttons, FAB/extended FAB, bottom sheets, side sheets, search bar/view, snackbars, loading indicator (expressive) and progress indicators (wavy where it fits).
- **Density:** comfortable by default, with an optional "compact" density setting.

## 3. Platforms and layout

| Window size | Width | Navigation | Feed layout |
|---|---|---|---|
| Compact (phone) | < 600 dp | Bottom navigation bar | 1 column, large cards |
| Medium (tablet, foldable) | 600–839 dp | Navigation rail | 2 columns |
| Expanded (desktop, landscape tablet) | ≥ 840 dp | Navigation rail or drawer | 3–4 columns. Watch page uses list-detail with the player plus a side panel |

- **PWA:** installable, standalone display, offline shell, with cached feed metadata and thumbnails.
- **Android:** same design, either native or a PWA/TWA wrapper. It must feel native: edge-to-edge, predictive back gesture, Material You colors.

## 4. Navigation (top-level destinations)

1. **Home:** the personal feed.
2. **Explore:** search, trending by country/category, and discovery from my groups.
3. **Subscriptions:** channels and channel groups.
4. **Library:** history, Watch Later, playlists, liked videos, notes.

Settings sits behind the avatar in the top app bar.

## 5. Screens

### 5.1 Home (feed): the priority screen

A vertically scrolling page made of **sections** I can reorder and toggle:

- **Continue watching:** horizontal carousel of partly watched videos, with a progress bar on each thumbnail.
- **New from favourites:** recent uploads from starred channels.
- **Latest from subscriptions:** the main chronological list.
- **Group sections:** one carousel per channel group (for example "Tech", "Cooking", "Music").
- **Trending in Spain:** optional, off by default.

Top of the page:

- A large collapsing top app bar with a greeting or the date.
- A **filter chip row**: All · Unwatched · Short (< 10 min) · Long (> 20 min) · Live · group chips.
- A toggle to **hide Shorts** (on by default).

Feed card content:

- Thumbnail (16:9) with a duration badge, a "LIVE" or "NEW" badge, and a watch-progress bar.
- Title (max 2 lines), channel avatar and name, relative age ("3 h ago") and view count.
- An overflow menu: Watch later · Add to playlist · Mark as watched · Hide channel · Add note.
- Swipe gestures (phone): swipe right = Watch later, swipe left = Mark watched / dismiss.

States to design:

- Loading (skeleton cards with shimmer).
- Pull-to-refresh, plus a "Last updated 12 min ago" line.
- Empty (no subscriptions yet: an onboarding prompt to sign in and import them).
- Offline (cached content with an offline banner).
- "All caught up" end-of-feed message instead of infinite scroll.
- API quota reached (friendly message, cached feed still shown).

### 5.2 Watch (player)

- The embedded YouTube player at the top, 16:9, always fully visible and **never overlapped** (see constraints).
- Below the player:
  - Title, channel row with a subscribe button, and stats.
  - An action row: Like · Dislike · Watch later · Save to playlist · Share · Note.
- **Chapters:** a chip list or vertical list parsed from description timestamps. Tapping one seeks to it, and the current chapter is highlighted.
- **Timestamped notes:** add a note at the current time; notes appear as a list and as markers.
- Collapsible description with clickable timestamps and links.
- **Up next:** the next item from the feed or the current playlist, with an autoplay toggle.
- Comments in a bottom sheet (phone) or a side panel (expanded).
- Custom controls around the player (not on top of it): playback speed segmented button, captions toggle, loop, and "skip intro" (jump to the first chapter).
- **Mini player:** when navigating away, the video collapses into a docked mini player above the nav bar (phone) or a corner card (desktop), still visible and still the official player.
- Expanded layout: player plus a side panel with tabs (Up next / Chapters / Notes / Comments).

### 5.3 Explore / Search

- An M3 search bar that expands into a search view with recent searches and suggestions.
- Results tabs: Videos · Channels · Playlists, with filters (duration, upload date, live).
- Trending section by country and category chips.
- Because search is expensive in API quota, show recent searches prominently and cache results.

### 5.4 Channel page

- A large header with the banner, avatar, name, subscriber count and subscribe button.
- Star (favourite) and "Add to group" actions.
- Per-channel notification toggle.
- Tabs: Videos · Playlists · About.
- Sort: Latest / Popular.

### 5.5 Subscriptions and groups

- A grid or list of subscribed channels with search and A–Z / recent-upload sorting.
- **Channel groups:** create, rename, recolor, set an emoji or icon, and drag channels in.
- Per-channel settings: favourite, mute from feed, notify on upload.

### 5.6 Library

- **History:** my own watch history, tracked by the app, grouped by day, searchable, and deletable.
- **Watch later:** a reorderable list.
- **Playlists:** my YouTube playlists (create, edit, reorder) plus local-only collections.
- **Liked videos.**
- **Notes:** all timestamped notes, grouped by video.
- **Stats (optional):** watch time per week and top channels, as a simple chart card.

### 5.7 Settings

- Theme: system / light / dark / AMOLED black, a seed color on the web, and dynamic color on/off.
- Feed: section order and visibility, hide Shorts, minimum/maximum duration, keyword mute list, refresh frequency.
- Player: default speed, autoplay next, captions on by default, preferred caption language.
- Notifications: new uploads from favourites.
- Account: Google sign-in status, YouTube connection, sign out, clear local data.
- About: version, API attribution and links to the terms.

### 5.8 Onboarding (first run)

1. A welcome screen.
2. "Sign in with Google" (Firebase Auth plus YouTube read/manage scope).
3. Import subscriptions.
4. Pick favourites and create first groups (optional, skippable).
5. Choose a theme.

## 6. Constraints the design must respect

These come from the YouTube API Services terms and the embedded player's limits. They are not optional.

- The **embedded YouTube player** must be used as is. Nothing may be drawn over it (no overlays, controls, ads or badges on top), and it must be at least 200×200 px. Our own controls go beside or below it.
- **No background or screen-off playback, no downloads, no audio-only mode, no ad blocking.** Don't design screens or toggles for these.
- Keep **YouTube attribution** visible where YouTube content is shown (for example a small "Content from YouTube" note in About and on the watch page, and channel names linked to their channel).
- Don't copy YouTube's logo, exact red or iconography. The app has its own identity.
- There is **no access to YouTube's own watch history, Watch Later or recommendations**, so history and watch later are the app's own features (stored in Firebase).
- Public dislike counts are not available. Show the dislike action only, with no count.

## 7. Data available for each video

Use this to decide what the cards and pages can show:

- Title, description, thumbnail (several sizes), duration, publish date.
- View count, like count, comment count.
- Channel: name, avatar, banner, subscriber count.
- Live status (upcoming / live / ended) and scheduled start time.
- Category and tags, plus chapters parsed from the description.
- App-only data: watch progress, watched flag, notes, groups, favourite flag.

## 8. Accessibility

- WCAG AA contrast in all color schemes, including dynamic ones.
- Minimum touch targets of 48 dp, full keyboard navigation on the web, and visible focus rings.
- Screen-reader labels on thumbnails (title, channel, duration, watched status).
- Respect the "reduce motion" setting by replacing springs and shared-element transitions with simple fades.
- Support text scaling up to 200% without broken layouts.

## 9. Tech stack and architecture

### 9.1 Stack

- **Web app:** Angular 22 as an installable PWA (`@angular/pwa` service worker).
- **Android:** the same Angular app wrapped with Capacitor (or a Trusted Web Activity), so there is one codebase.
- **UI:** Angular Material (M3 theming via system tokens), extended with custom tokens and components for the Expressive shapes and motion. Use Angular CDK for overlays, drag-and-drop (group editing, reordering) and virtual scrolling.
- **Backend:** Firebase Auth (Google sign-in with the YouTube scope), Firestore (history, notes, groups, preferences, cached feed) and Hosting. Cloud Functions plus FCM are optional, for background feed refresh and upload notifications.
- **APIs:** YouTube Data API v3 (10,000 quota units/day) and the YouTube IFrame Player API (`@angular/youtube-player`).
- **Testing:** Vitest for unit tests and Playwright for end-to-end tests.

### 9.2 Angular 22 best practices

- **Standalone everything.** No NgModules.
- **Signals first:**
  - `signal`, `computed` and `linkedSignal` for state.
  - `input()`, `output()` and `model()` instead of decorators.
  - `resource()` / `httpResource()` for async data.
  - RxJS only where streams really fit (player events, debounced search).
- **Zoneless change detection** (`provideZonelessChangeDetection()`), with **OnPush** on every component (the default in v22).
- **Built-in control flow** (`@if`, `@for` with `track`, `@switch`), plus `@defer` for below-the-fold sections (comments, related videos) and `@boundary` / `@catch` so one failing feed section doesn't break the page.
- **Signal Forms** for settings, search filters and notes.
- **`inject()`** instead of constructor injection, and **functional** guards, resolvers and interceptors.
- **Lazy routes per domain** (`loadChildren` / `loadComponent`), with typed route params bound to inputs (`withComponentInputBinding()`).
- **HttpClient with the Fetch backend** (`provideHttpClient(withFetch())`), and interceptors for the API key, OAuth token, quota errors and retry.
- **Strict TypeScript** and strict templates, with ESLint (angular-eslint) and Prettier.
- **Smart/dumb split:** route-level "feature" components talk to facades, while "ui" components are presentational, take inputs, emit outputs and contain no services.
- **Images:** `NgOptimizedImage` for thumbnails and avatars, with lazy loading and correct `sizes`.
- **Accessibility:** Angular Aria / CDK a11y utilities.

### 9.3 Domain-driven structure

The app is split into **bounded contexts** (domains). Each domain has the same four layers, and dependencies only point inward:

```
presentation  →  application  →  domain  ←  infrastructure
```

- **domain:** pure TypeScript with no Angular or Firebase imports. Holds entities, value objects, domain services and repository interfaces (ports).
- **application:** use cases and signal-based facades/stores that orchestrate the domain. This is the only layer presentation talks to.
- **infrastructure:** adapters that implement the ports: YouTube API clients, Firestore repositories, RSS fetchers, and DTO → domain mappers. They are wired with DI providers.
- **presentation:** routes, feature (smart) components and ui (dumb) components.

```
src/
├── app/
│   ├── app.config.ts              # providers: router, http, zoneless, firebase, service worker
│   ├── app.routes.ts              # lazy-loads each domain's routes
│   ├── app.ts                     # shell: nav bar/rail, top app bar, mini player outlet
│   │
│   ├── core/                      # app-wide, loaded once
│   │   ├── auth/                  # Firebase Auth session, Google + YouTube OAuth scope
│   │   ├── http/                  # interceptors: api-key, auth-token, quota, retry, error
│   │   ├── firebase/              # Firebase/Firestore initialization and providers
│   │   ├── layout/                # adaptive shell (compact / medium / expanded)
│   │   ├── theme/                 # dynamic color, light/dark/AMOLED, M3 tokens
│   │   └── error-handling/
│   │
│   ├── shared/                    # reusable, domain-agnostic
│   │   ├── ui/                    # design-system components: video-card, chip-row, skeleton, empty-state…
│   │   ├── util/                  # pure helpers: duration parsing (ISO 8601), relative time, chapter parsing
│   │   ├── pipes/
│   │   └── directives/
│   │
│   └── domains/
│       ├── catalog/               # Video, Channel, Playlist as read from YouTube; search & trending
│       │   ├── domain/            # video.entity.ts, channel.entity.ts, duration.vo.ts, video.repository.ts (port)
│       │   ├── application/       # search-videos.use-case.ts, catalog.facade.ts
│       │   ├── infrastructure/    # youtube-video.repository.ts, youtube.dto.ts, video.mapper.ts
│       │   ├── presentation/      # search/, channel-page/, catalog.routes.ts
│       │   └── index.ts           # public API — the only file other domains may import
│       │
│       ├── feed/                  # home page: sections, ranking, filters, refresh
│       │   ├── domain/            # feed-item.entity.ts, feed-section.vo.ts, feed-filter.vo.ts, feed-ranker.service.ts
│       │   ├── application/       # build-home-feed.use-case.ts, refresh-feed.use-case.ts, feed.store.ts
│       │   ├── infrastructure/    # firestore-feed-cache.repository.ts, uploads-playlist.source.ts, rss.source.ts
│       │   ├── presentation/      # home/, feed-section/, filter-chips/, feed.routes.ts
│       │   └── index.ts
│       │
│       ├── player/                # watch page, mini player, chapters, progress tracking
│       │   ├── domain/            # playback-session.entity.ts, chapter.vo.ts, watch-progress.vo.ts
│       │   ├── application/       # player.store.ts, track-progress.use-case.ts
│       │   ├── infrastructure/    # youtube-iframe-player.adapter.ts
│       │   ├── presentation/      # watch-page/, mini-player/, chapter-list/, player.routes.ts
│       │   └── index.ts
│       │
│       ├── subscriptions/         # subscribed channels, channel groups, favourites, mutes
│       │   ├── domain/            # subscription.entity.ts, channel-group.aggregate.ts, subscription.repository.ts
│       │   ├── application/       # import-subscriptions.use-case.ts, manage-groups.use-case.ts
│       │   ├── infrastructure/    # youtube-subscription.repository.ts, firestore-group.repository.ts
│       │   ├── presentation/
│       │   └── index.ts
│       │
│       ├── library/               # history, watch later, playlists, liked, notes, stats
│       │   ├── domain/            # history-entry.entity.ts, note.entity.ts, watch-later.aggregate.ts
│       │   ├── application/
│       │   ├── infrastructure/    # firestore-*.repository.ts, youtube-playlist.repository.ts
│       │   ├── presentation/
│       │   └── index.ts
│       │
│       ├── engagement/            # like/dislike, subscribe, comments (write actions to YouTube)
│       │   └── domain/ application/ infrastructure/ presentation/ index.ts
│       │
│       └── preferences/           # settings: theme, feed rules, player defaults, notifications
│           └── domain/ application/ infrastructure/ presentation/ index.ts
│
├── environments/                  # Firebase config, restricted YouTube API key (HTTP-referrer locked)
├── styles/                        # global M3 theme, tokens, typography
└── manifest.webmanifest
```

**Rules:**

1. A domain **never** imports another domain's internals. It goes through that domain's `index.ts`, and only from `application` (or shared `domain` types).
2. `domain/` has **zero** framework imports, so it is unit-testable in plain Vitest.
3. YouTube and Firestore DTOs never leave `infrastructure/`. Mappers convert them to domain objects.
4. `shared/ui` components are presentational only, with no injected services.
5. The boundaries are enforced with **Sheriff** (`@softarc/sheriff`) or `eslint-plugin-boundaries`, so the rules fail lint instead of relying on discipline.

**Ubiquitous language:** Video, Channel, Subscription, ChannelGroup, Favourite, FeedItem, FeedSection, FeedFilter, WatchProgress, HistoryEntry, WatchLater, Note (timestamped), Chapter, Playlist, Preferences.

### 9.4 Quota-aware data flow (feed)

1. `subscriptions` provides the channel list (imported once, then refreshed daily).
2. `feed/infrastructure` fetches the latest uploads per channel from the uploads playlist (1 unit), or via RSS from a Cloud Function (0 units). It batches `videos.list` calls (50 IDs per call, 1 unit) to get durations and stats.
3. Results are cached in Firestore with a timestamp. The UI reads the cache first, then refreshes in the background.
4. `feed/domain` applies filters (hide Shorts, duration, muted channels or keywords, watched) and ranking, and builds the sections.
5. Search (100 units) is the only expensive call. It is debounced, cached and never triggered automatically.

## 10. Deliverables wanted from Claude Design

1. Design tokens: color schemes (light/dark/AMOLED plus a dynamic-color example), type scale, shape scale and motion specs.
2. Component sheet: feed card (large, compact, carousel), chips, section header, mini player, chapter list, note item, channel row, group chip.
3. Screens at compact and expanded sizes: Home, Watch, Search, Channel, Subscriptions & Groups, Library, Settings, Onboarding.
4. Key states: loading, empty, offline, quota reached, all caught up.
5. A prototype flow: Home → tap card → shared-element transition to Watch → back to Home with the mini player.