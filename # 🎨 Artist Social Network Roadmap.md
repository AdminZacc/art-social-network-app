# 🎨 Artist Social Network Roadmap

## Vision
A social network for artists that **displays art with high-fidelity, zoomable, context-rich views** — built with modern, simple, and affordable technology.

---

## Product Differentiators
- **Deep-Zoom + Context**: View artworks in fine detail (zoom & pan like a museum viewer).
- **Smart Mosaic / Focus Mode**: Dynamic feed grouped by color, medium, or theme with distraction-free viewing.
- **Process Layers**: Show progress shots, before/after, or time-lapse creation.
- **Infinite Collaborative Canvas** (later): Spatial, group-based art displays.
- **Low-Friction Monetization**: Tips, commissions, and print-on-demand.
- **Accessibility-First**: WCAG AA compliance, alt text prompts, keyboard navigation.

---

## Technology Stack

### Frontend
- **Next.js** (React) + TypeScript
- **Tailwind CSS** for styling
- **React Query** or **SWR** for data fetching
- **OpenSeadragon** for deep-zoom
- Optional: **three.js** for 3D gallery views

### Backend & Storage
- **Supabase** (Postgres, Auth, Storage, Realtime)
- Alternative storage: **Cloudflare R2**
- Serverless functions for image processing (e.g., libvips)

### Image Processing & CDN
- Progressive WebP/AVIF with `srcset`
- Deep-zoom tiling on upload
- CDN caching for fast load

### Realtime & Notifications
- Supabase Realtime or Pusher

### Search
- Postgres full-text search
- Algolia (optional for advanced search)

### Payments
- **Stripe** for tips, commissions, subscriptions

### Hosting & CI
- **Vercel** or Netlify for hosting
- GitHub Actions for CI/CD

### Monitoring
- **Sentry** for error tracking
- Lighthouse for performance audits

---

## High-Level Architecture
1. **Next.js UI** ↔ **Supabase REST/Realtime + Storage CDN**
2. **Image Processing Function**: Creates responsive images + deep-zoom tiles
3. **Client**: Fetches tile manifests for OpenSeadragon or responsive images for feed
4. **Realtime Notifications**: Delivered via Supabase Realtime/Pusher

---

## Product Roadmap

### Phase A — Discovery & Design
- Interview artists, curators, collectors
- Define user flows & acceptance criteria
- Create wireframes for:
  - Feed
  - Artist profile
  - Upload flow
  - Focus Mode
- Draft moderation & privacy rules

**Deliverables:** Product spec, clickable prototype, prioritized MVP features

---

### Phase B — MVP
**Goal:** Let artists sign up, upload art (with process images), view in feed and Focus Mode, follow/like, search.

**Core Features**
- Signup/login (email + OAuth)
- Artist profile (bio, links, portfolio)
- Upload flow with metadata + process images
- Masonry feed grouped by color/medium
- Focus Mode: zoomable art, details panel
- Follow/unfollow, like, comment
- Search by tag/artist
- Admin panel for moderation

**Acceptance Criteria:**  
Upload 20MB+ image → users can zoom without full download; follow/like works; feed updates in real time.

---

### Phase C — Beta
- Add messaging & collections
- Shareable galleries
- Print-on-demand integration
- Artist analytics (views, likes)
- Trending/challenges/staff picks
- Beta test with selected artists

---

### Phase D — Scale & Advanced Features
- Infinite collaborative canvas
- AR/VR gallery mode
- Commission marketplace with escrow
- AI-powered discovery & recommendations
- PWA for offline-first usage
- Advanced moderation tools
- Monetization tiers

---

## Technical Tasks

### Backend
- Create Supabase schema:
  - `users`, `artworks`, `images`, `tags`, `follows`, `likes`, `comments`, `notifications`, `collections`
- Configure storage bucket for artwork & tiles
- Build image processing function (libvips):
  - Generate responsive images + deep-zoom tiles
  - Store manifests & tiles
- Enable realtime channels for notifications

### Frontend
- Next.js app skeleton with Tailwind
- Authentication flows (Supabase client)
- Upload UI with metadata & process ordering
- Masonry feed with infinite scroll & clustering
- Focus Mode (OpenSeadragon + metadata panel)
- Profile & collection pages
- Admin dashboard

### DevOps
- GitHub repo + branch protection
- Vercel integration for preview deployments
- CI checks: lint, type-check, tests
- Sentry error tracking

---

## Example Minimal DB Schema
```sql
users(id PK, email, name, username, bio, avatar_url, created_at)
artworks(id PK, artist_id FK users.id, title, description, year, medium, width_cm, height_cm, tile_manifest_url, main_image_url, created_at)
images(id PK, artwork_id FK, url, type ENUM('main','process','thumbnail'), order, created_at)
tags(id PK, name)
artwork_tags(artwork_id, tag_id)
follows(follower_id, followee_id, created_at)
likes(user_id, artwork_id, created_at)
comments(id, artwork_id, user_id, parent_comment_id nullable, text, created_at)
