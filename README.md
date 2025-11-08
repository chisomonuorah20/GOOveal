# GOOveal
*A simple yet powerful visual discovery app.*

Gooveal turns your phone camera into an instant understanding tool. Point it at text or objects to **translate, identify, and learn** in less than 3 seconds through a clean, two-tab experience which is **Text** and **Search**.

---

## Why it matters
GOOveal is a free, powerful feature that allows users to search based on what they see, using a photograph, the camera, or any image. It bridges the gap between the physical and digital worlds by gathering relevant content from the internet.

---

## Who it’s for
- **Students** — translate notes, capture quotes etc.
- **Travelers** — read menus, signs, labels abroad.
- **Creators & shoppers** — find similar items or learn about products.
- **Curious people** — identify landmarks, objects, artwork.

---

## Key features
Gooveal emphasizes two main features, offering immediate utility to the user:
1. **Live Text Option**: Enables the user to utilize the camera to detach, highlight, and analyze live words or numbers, allowing for focused evaluation.
    - **User Story**: As a student, I want to capture and analyze specific text from a printed textbook so I can easily copy it without manual typing.
2. **Visual Search Option**: Allows users to upload images of things, places, clothes, or people. Gooveal then searches the internet to reveal the same or similar results.
    - **User Story**  As a shopper, I want to upload a picture of an outfit I like so I can find its source or comparable items to purchase online.


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

