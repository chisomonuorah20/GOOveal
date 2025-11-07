# Gooveal — System Architecture
*A lightweight, privacy-first visual discovery app.*

This document outlines the **frontend, backend, database**, **component communication**, and **feasibility** for Gooveal’s MVP.

---

## 1) Stack Summary
**Frontend (Mobile)**
- React Native (Expo), React Navigation
- Camera: Expo Camera (or RN Camera)
- On-device OCR: Google ML Kit (Android) / Apple Vision (iOS)

**Backend (API Gateway)**
- Node.js + Express (or Firebase Cloud Functions)
- Responsibilities: translate proxy, visual search proxy, auth verification, analytics

**Database & Storage**
- Firebase Authentication
- Firestore (users, saved results, events)
- (Optional) Firebase Storage/S3 for cached thumbnails

**External Services (v1)**
- Translation: DeepL or Google Translate API
- Visual Search: Clarifai / ACRCloud (swap to CLIP + Vector DB later)

---

## 2) Component Responsibilities
**Mobile app**
- Two modes: **Text** (OCR → Translate/Copy/Share), **Search** (image capture → matches)
- Local caching & Collections
- Privacy controls: cloud on/off, delete all, language prefs

**API Gateway**
- `/translate`: forwards text to translation API, returns normalized JSON
- `/visual-search`: forwards image (compressed) to visual search API, returns matches
- Auth: verify Firebase ID token; rate limiting; event logging

**Data Model (simplified)**
- `users/{userId}`
- `users/{userId}/saved_results/{resultId}`
- `events/{eventId}` → { type, userId, ts, metadata }

---

## 3) Communication Flow
**Text/Translate**
1. App performs OCR **on-device**.
2. If user taps Translate → `POST /translate { text, target }`.
3. API → Translation provider → returns `{ translatedText }`.
4. User can **Save** → Firestore.

**Visual Search**
1. App captures compressed image.
2. `POST /visual-search { imageBase64, mode }`.
3. API → Visual search provider → returns normalized matches `{ title, type, confidence, links, facts }`.
4. User can **Save** → Firestore.

**Diagram (text)**
[Gooveal App (RN)]
| on-device OCR
v
[API Gateway (Node)]
|--> Translation API
|--> Visual Search API
v
---

## 4) Non-Functional Choices
- **Privacy:** OCR local by default; cloud is opt-in; minimize payloads (embeddings/compressed images); Delete-All in app.
- **Performance:** OCR 150–250 ms; visual search round-trip < 2.5 s; compress to ~640px; cache recent results.
- **Scalability:** Serverless API scales on demand; Firestore auto-scales; swap APIs for self-hosted CLIP + Vector DB as traffic grows.
- **Observability:** Basic latency/success/error logs; event analytics (no PII) in Firestore (BigQuery export later).

---

## 5) Feasibility (Why this works)
- **Fast to ship:** RN + on-device OCR delivers value with minimal backend.
- **Cost-controlled:** Heavy ML outsourced in v1; migrate to CLIP/FAISS later.
- **Trust-building:** Local processing and explicit toggles reduce privacy risk.
- **Portable:** One codebase (Android/iOS); thin, replaceable API layer.
