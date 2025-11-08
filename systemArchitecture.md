# GOOveal — System Architecture
*A simple yet powerful visual discovery app.*

This document outlines the **frontend, backend, database**, **component communication**, and **feasibility** for GOOveal’s MVP.

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

## 2) How components communicate
This section explains how different parts of GOOveal work together — from the mobile app to the backend and database.

---

### Text Translation Flow (Live Text Mode)

1. The user opens the **GOOveal app** and selects **Live Text** mode.  
2. The app uses **Google ML Kit** directly on the phone to detect and read text through the camera.  
3. When the user taps “Translate,” the app sends the detected text to the backend using a secure request.  
4. The **Node.js + Express backend** receives the request and passes it to the **DeepL Translation API**.  
5. DeepL sends back the translated text.  
6. The backend returns the translation to the app, which displays it instantly on the screen.  
7. If the user chooses to save it, the translated text is stored in **Firebase Firestore** under their account.

---

###  Visual Search Flow (Image Search Mode)

1. The user switches to **Visual Search** mode and takes or uploads a photo.  
2. The app compresses the image (to keep it light and fast).  
3. The image is sent to the **Express backend** through a secure HTTPS connection.  
4. The backend forwards the image to the **Clarifai Image Recognition API**, which finds similar or matching items.  
5. Clarifai sends back results like product names, landmarks, or visual matches.  
6. The backend returns this data to the app.  
7. The user can view, open, or save the results in **Firestore** for later reference.

---

### Data Storage & Events

- **Firebase Firestore** stores all saved searches, results, and user preferences.  
- **Firebase Authentication** keeps users logged in securely across devices.  
- **Analytics & Events** (like translation used or visual searches made) are recorded for product performance insights.

---

###  Simplified Architecture Flow (Text Diagram)

```text
           ┌───────────────────────────────┐
           │       Gooveal App (RN)        │
           │   React Native (Expo) Client  │
           └──────────────┬────────────────┘
                          │
          (Text) Google ML Kit (on-device OCR)
                          │
                          ▼
           ┌───────────────────────────────┐
           │     Node.js + Express API     │
           │  (Backend Integration Layer)  │
           └──────────────┬────────────────┘
                          │
          ┌───────────────┼────────────────┐
          ▼                               ▼
┌───────────────────────┐       ┌──────────────────────┐
│  DeepL API (Translate)│       │ Clarifai API (Visual)│
└───────────────────────┘       └──────────────────────┘
                          │
                          ▼
           ┌───────────────────────────────┐
           │ Firebase Firestore Database   │
           │ (Saved results, history, prefs)│
           └───────────────────────────────┘

```
---

## 3) Feasibility (Why this works)
1. **Privacy First**:
GOOveal protects users by doing most of its work on the phone.
Text recognition (OCR) happens directly on the device, not online.
When someone searches using an image, the app only uploads a small, compressed version, never the full photo.
Users can also clear all their saved data anytime with a single button.

2. **Fast Performance**:
The app is designed to be quick and smooth.
Text scanning happens in under half a second, and image searches return results in less than 3 seconds because of light image uploads and fast servers.

3. **Easy to Build and Maintain**:
Using React Native means the same app works on both Android and iOS, saving time and effort.
The backend is simple, it only manages three main functions: translation, visual search, and saving results.

4. **Low Cost and Safe to Scale**:
Instead of building complex AI from scratch, GOOveal uses trusted services like DeepL for translations and Clarifai for image recognition.
As the app grows, these can later be replaced with in-house tools without starting over.

5. **Scalable and Reliable Setup**:
Vercel automatically adjusts server capacity based on traffic, and Firestore grows with user data.
GitHub Actions handles updates automatically, while Sentry and Firebase Analytics help track performance and fix issues early.
