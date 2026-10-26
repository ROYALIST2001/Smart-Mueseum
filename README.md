# Smart Museum Guide

A mobile visitor guide for the **Colombo National Museum**.

A visitor opens the app, picks English, Sinhala or Tamil, scans the QR code beside an exhibit, and gets an explanation with spoken narration.

Museum staff add new objects themselves through a web dashboard. No new app build is needed.

---

## How it works

1. Every exhibit has a QR sticker. The sticker holds only an ID, like `ART001`.
2. The app reads that ID.
3. The app asks the database for that record.
4. The record has a field called `experienceType`.
5. That one field decides which screen opens.

So the app has **4 generic screens, not one screen per object**. Adding object number 11 or number 500 is a database row, not new code.

## The three V1 experiences

| Type | What the visitor does | Camera used for | Suits |
|------|----------------------|-----------------|-------|
| `VIDEO` | Watches a video with narration | Nothing after the scan | Paintings, textiles, flat objects |
| `AR_3D` | Points at the floor, a full-size copy appears | Finding a flat surface | Statues and sculptures |
| `AR_TEXT` | Points at the real stone, the meaning appears on it | Recognising the real object | Stone inscriptions |

`AR_ANNOTATION` (labels on a carved panel, synced to narration) is **V2**. Not in this version.

## Important rules

- **No visitor ever sees an error.** If the phone cannot do AR, it still gets the content:
  - `AR_3D` becomes a 3D model the visitor spins with a finger.
  - `AR_TEXT` becomes a photo with the meaning laid over it.
  - The button says "View in 3D", not "View in AR".
- **Built on fake seed data.** Real museum content goes in later through the dashboard. Content delays never block development.
- **Sinhala and Tamil fonts are bundled in the app.** Never rely on the phone's own fonts. This is what broke the previous prototype.

---

## Tech stack

| Layer | Choice |
|-------|--------|
| App | React Native + Expo dev build, TypeScript |
| Navigation | React Navigation |
| QR scanning | react-native-vision-camera |
| AR | @reactvision/react-viro |
| 3D fallback | model-viewer in a WebView |
| Video / Audio | expo-video, expo-audio |
| Languages | i18next, bundled Noto Sans Sinhala + Tamil |
| Backend | Supabase (Postgres, Auth, Storage) |
| Local dev | Docker |
| Admin portal | React + Vite, deployed on Vercel |
| QR generation | qrcode.react |

> ViroReact needs an **Expo dev build**. It cannot run inside Expo Go.

---

## Phases

Each phase is built on its own branch.

| Phase | Branch | What gets built | Days |
|-------|--------|-----------------|------|
| 1 | `phase-1-foundation` | AR spike on a real phone, database, app shell, 3 languages | 6 |
| 2 | `phase-2-scanner-video` | QR scanner, object details, video experience | 5 |
| 3 | `phase-3-ar-3d` | 3D object in the room, phone capability check, fallback view | 5 |
| 4 | `phase-4-dashboard` | Staff dashboard, load objects, testing, handover | 6 |

The AR test is on **day one** on purpose. If the AR library fights us, we find out 1 day in, not 15.

---

## Project documents

Client-facing documents are in [`ProjectPlan/`](ProjectPlan/):

- `Smart_Museum_Guide_Project_Plan.pdf` — the four weeks, in plain words
- `Smart_Museum_Guide_How_It_Works.pdf` — diagrams and costs
