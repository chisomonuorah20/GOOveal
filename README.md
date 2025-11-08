# GOOveal
*A simple yet powerful visual discovery app.*

GOOveal turns your phone camera into an instant understanding tool. Point it at text or objects to **translate, identify, and learn** in less than 3 seconds through a clean, two-tab experience which is **Text** and **Search**.

---

## Why it matters
GOOveal is a free, powerful standalone application that allows users to search based on what they see, using a photograph, the camera, or any image. It bridges the gap between the physical and digital worlds by gathering relevant content from the internet.

---

## Who it’s for
- **Students** — translate notes, capture quotes etc.
- **Travelers** — read menus, signs, labels abroad.
- **Creators & shoppers** — find similar items or learn about products.
- **Curious people** — identify landmarks, objects, artwork.

---

## Key features
GOOveal emphasizes two main features, offering immediate utility to the user:
1. **Live Text Option**: Enables the user to utilize the camera to detach, highlight, and analyze live words or numbers, allowing for focused evaluation.
    - **User Story**: As a student, I want to capture and analyze specific text from a printed textbook so I can easily copy it without manual typing.
2. **Visual Search Option**: Allows users to upload images of things, places, clothes, or people. GOOveal then searches the internet to reveal the same or similar results.
    - **User Story**  As a shopper, I want to upload a picture of an outfit I like so I can find its source or comparable items to purchase online.


---

## Technologies
- **App**: React Native (Expo) – cross-platform for Android & iOS
- **Programming Language**: TypeScript
- **Navigation**: React Navigation
- **Camera**: Expo Camera
- **On-Device OCR**: Google ML Kit (Text Recognition v2)
- **Translation API**: DeepL API (accessed through backend proxy)
- **Visual Search Engine**: Clarifai Image Recognition API
- **Backend Framework**: Node.js (v20) + Express.js
- **Database**: Firebase Firestore (for users, saved results, and preferences)
- **Authentication**: Firebase Authentication (Email & Google Sign-In)
- **File Storage**: Firebase Storage (for saved image thumbnails)
- **Hosting & Deployment**:

    - **Backend**: Vercel (Serverless Functions)

    - **Mobile App**: Expo EAS Build

       **Analytics & Monitoring**: Firebase Analytics + Sentry React Native SDK

       **Version Control**: Git & GitHub

       **Continuous Integration**: GitHub Actions (lint, test, deploy)

---

## How GOOveal works (at a glance)

1. **Onboarding**: Users are prompted to **sign up** or **log in** to ensure **search history** is preserved.
2. **Landing Page**: A minimalist page featuring only a **camera icon**.
3. **Search Mode Selection**: Clicking the icon leads to a choice between the **Text Tab** (Live Text) and the **Search Tab** (Visual Search).
4. **Result Handling**: For all search types (items, location, or people), users have the option to save the results for later or click the results to be taken directly to the external source.