# GOOveal
*A simple, privacy-first visual discovery app.*

Gooveal turns your phone camera into an instant understanding tool. Point it at text or objects to **translate, identify, and learn**—in seconds—through a clean, two-tab experience: **Text** and **Search**.

---

## Why it matters
People meet unknown words, signs, and objects every day (travel, school, shopping) but existing tools feel hidden or too complex. Gooveal makes visual learning **simple, fast, and respectful of privacy**, so anyone can get answers without hunting through menus or sharing more data than necessary.

---

## Who it’s for
- **Students** — translate notes, capture quotes.
- **Travelers** — read menus, signs, labels abroad.
- **Creators & shoppers** — find similar items or learn about products.
- **Curious people** — identify landmarks, objects, artwork.

---

## Key features
- **Live Text Translation** – highlight text in camera view, tap to Translate/Copy/Share.
- **Visual Search** – snap an item to see “Buy / Learn / Save” results.
- **Save & Collections** – keep discoveries organized for later.
- **Explain the Match** – a small “Why this match” note builds trust.
- **Privacy Controls** – on-device OCR by default; cloud search is opt-in.

---

## Technologies (current plan)
- **App:** React Native (Android & iOS)
- **On-device OCR:** Google ML Kit / Apple Vision
- **Translation:** DeepL or Google Translate API (via backend proxy)
- **Visual Search:** hosted API (e.g., Clarifai/ACR) in v1, upgradeable to CLIP + vector DB
- **Backend:** Node.js + Express (API gateway & aggregation)
- **Data:** Firebase/Firestore (saved results & preferences)

---

## How Gooveal works (at a glance)
1. Open the app → pick **Text** or **Search**.
2. **Text:** on-device OCR highlights words → tap **Translate / Copy / Share**.
3. **Search:** capture an image → backend requests visual matches → show **Buy / Learn / Save**.
4. Save to Collections, export or revisit anytime.

---

## Roadmap (MVP → v1)
- **MVP:** Text translate/copy, basic visual search, Save & Collections, privacy toggles.
- **v1:** “Why this match”, offline language packs, export to notes, improved latency.
- **Later:** plant/animal ID, affiliate shopping feeds, learning cards.

---

## Privacy
- OCR runs **on-device** by default.
- Cloud requests are **opt-in** and minimized (send fingerprints/embeddings, not raw images when possible).
- Clear **Delete All Data** option in-app.

