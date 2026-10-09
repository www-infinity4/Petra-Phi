# PetriPhi — Halloween Art Studio

Repository: `www-infinity4/Petra-Phi`. PetriPhi is the creative counterpart to TerriPhi (`www-infinity4/Terra-phi`).

## First build

Open `index.html` in a modern browser. The poster maker renders original Halloween motifs (`pumpkin`, `ghost`, `cat`, `castle`, `moon`) using a 800 × 1000 Canvas. Users can edit titles, lines and palettes, generate variations, save **up to 12 designs in local browser storage**, download a PNG and share a PNG via the browser if file sharing is supported. Cards are electric violet/gold and mobile responsive. No external image-generation API is falsely represented as working.

## WidgetPhi integration

Widget `seasonal-card`, mode `art`, links to the deployed PetriPhi page using its `artUrl` field. Use a themed story/vintage card for additional creative entry points. Set `seasonal:true` by default; the host may choose the card from permissioned first-party interaction counts such as image searches.

## Public gallery is future integration

The current gallery is **local**, not a shared gallery. The repository includes `docs/GALLERY_API.md` with the required Cloudflare D1 and moderation contract. Do not claim that images are public, charge wallets, or grant credits without a real, verified Cloudflare backend. Original poster PNGs can be exported without signing in.

## Publishing

The standalone page is code-only until deployed. After Cloudflare routes, gallery storage and identity are available, connect real user galleries and AI image creation behind a trustworthy provider/permission system, with stable asset URLs and creator credits.
