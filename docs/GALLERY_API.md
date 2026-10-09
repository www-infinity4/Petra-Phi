# Public Halloween Gallery — Cloudflare deployment contract

**Status:** architecture prepared; **not deployed**. The PetriPhi homepage currently creates original, browser-rendered art and keeps a 12-item *local* gallery only.

## Storage

Use Cloudflare R2 for immutable original image assets and resized previews. Use D1 for metadata, moderation status, verified creator identities, source credits and reports. Do not put large PNG base64 images in D1 or localStorage.

Recommended D1 table:

```sql
CREATE TABLE IF NOT EXISTS petriphi_art (
 id TEXT PRIMARY KEY,
 creator_account_id TEXT NOT NULL,
 title TEXT NOT NULL,
 asset_key TEXT NOT NULL UNIQUE,
 preview_key TEXT,
 motif TEXT,
 palette TEXT,
 credit TEXT,
 license TEXT NOT NULL,
 status TEXT NOT NULL CHECK(status IN ('pending','approved','rejected','removed')),
 created_at TEXT NOT NULL DEFAULT CURRENT_TIMESTAMP,
 published_at TEXT
);
CREATE INDEX IF NOT EXISTS petriphi_art_public ON petriphi_art(status,published_at);
CREATE TABLE IF NOT EXISTS petriphi_reports (
 id TEXT PRIMARY KEY,
 artwork_id TEXT NOT NULL,
 reporter_account_id TEXT NOT NULL,
 reason TEXT NOT NULL,
 created_at TEXT NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## API sketch (to implement after binding R2 and D1)

- `POST /api/petriphi/art`: authenticated creator, rights attestation, server-side size/type limits and rate limits; save private asset and metadata with **pending** status; return submission id, never a public gallery placement.
- `GET /api/petriphi/art?cursor=...`: approved artworks only, stable short-lived media URLs or owned CDN URLs, bounded pagination (max 24).
- `GET /api/petriphi/art/:id`: approved artwork + source/creator attribution; fail closed for pending/rejected.
- `POST /api/petriphi/art/:id/report`: authenticated anti-abuse report; prevent duplicates and flooding.
- `DELETE /api/petriphi/art/:id`: verified creator or moderator removes the artwork and any published images.
- `POST /api/petriphi/art/:id/collect`: forward to the existing authoritative Unified Wallet/StarCoin settlement service; idempotent and proof-verified. A click alone never mints tokens.

## Safe publishing and privacy

Check artwork for abusive/illegal uploads and copyrighted material before approval. Provide report/delete paths and block duplicate spam. Do not collect device fingerprints or expose private account ids. Ask permission before making art public; default to private/local. If using AI image generation, call a real configured model provider and label the output accurately. Credit creators and historic source collections.

## Connection plan

1. Deploy TerriPhi and PetriPhi standalone pages to their QuantaPhi.org routes.
2. Connect WidgetPhi's `seasonal-card` to route-specific URLs and click-to-play assets.
3. Deploy R2, D1, authentication, moderation and verified wallet service.
4. Activate public gallery endpoints only after end-to-end tests confirm identity, privacy, publishing and takedown behavior.
