# Gooveal — System Architecture
*A simple yet powerful visual discovery app.*

This document outlines the **frontend, backend, database**, **component communication**, and **feasibility** for Gooveal’s MVP.

---

## 1) Stack Summary
**Frontend (Mobile)**
- **Framework**: React Native (Expo)
- **Language and Tooling**: TypeScript, React Navigation, Expo Camera
- **OCR (on device)**: Google ML Kit – Text Recognition v2 (bridged in RN)
- **State and Data Fetching**: @tanstack/react-query
- **Auth SDK**: Firebase Authentication
- **Analytics and Crashes**: Firebase Analytics, Sentry React Native

**Backend (API Gateway & Integration Layer)**
- **Runtime and Framework**: Node.js v20 + Express.js
- **Deployment**: Vercel Serverless Functions
- **Responsibilities**: `/translate`, `/visual-search`, `/save`, auth token verification, rate limiting, analytics logging
- **SDKsLibraries**: firebase-admin, axios, express-rate-limit

**Database & Storage**
- **Auth**: Firebase Authentication
- **Database**: Firebase Firestore (users, saved results, events)
- **File Storage**: Firebase Storage (image thumbnails only)
- **CI/CD**: GitHub Actions (lint, test, deploy)
- **Mobile builds**: Expo EAS Build

**External AI/ML Services**
- **Translation**: DeepL API
- **Visual Search**: Clarifai Imaage Recognition API

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
