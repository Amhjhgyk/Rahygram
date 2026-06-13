# RahyGram

RahyGram is an original Persian-first secure messaging starter app for Android. It is inspired by modern messaging workflows, but it uses its own naming, colors, layouts, bubble shapes, and architecture.

## What is included

- Kotlin Android app with Jetpack Compose
- MVVM-style view models and repository interface
- Demo OTP login flow with phone or email mode
- One-to-one and group chat UI
- Text messaging with delivery marks, reaction and delete actions
- Placeholder controls for image, file, voice note, and voice call expansion
- Status-style "Moments" row with original visual treatment
- Firebase-ready package dependencies for Auth, Firestore, Storage, and FCM
- Secure token storage wrapper using Encrypted SharedPreferences
- AES-GCM message encryption helper for the client-side encryption layer
- Localized resources for Persian, English, Arabic, Turkish, and Russian
- RTL support through Android resources and manifest support
- Light and dark Material 3 color schemes using blue, gold, and charcoal

## Project structure

```text
app/src/main/java/com/rahya/rahygram
├── core
│   ├── localization
│   └── security
├── data
│   ├── firebase
│   ├── model
│   └── repository
├── notifications
├── ui
│   ├── components
│   ├── navigation
│   ├── screens
│   └── theme
└── viewmodel
```

## Firebase setup

1. Create a Firebase project.
2. Add an Android app with package name `com.rahya.rahygram`.
3. Download `google-services.json` and place it in `app/`.
4. Add the Google Services Gradle plugin:

```kotlin
// root build.gradle.kts
id("com.google.gms.google-services") version "4.4.2" apply false

// app/build.gradle.kts
id("com.google.gms.google-services")
```

5. Enable these Firebase products:
   - Authentication: Phone provider, and Email/Password if desired
   - Firestore Database
   - Cloud Storage
   - Cloud Messaging
6. Publish `firestore.rules` and `storage.rules` from this project before opening the app to real users.

## Firestore data model

```text
users/{userId}
  displayName: string
  phoneNumber: string
  email: string?
  isOnline: boolean
  lastSeenEpochMillis: number
  fcmToken: string
  publicIdentityKey: string

chats/{chatId}
  kind: "Direct" | "Group"
  participantIds: string[]
  createdAtEpochMillis: number
  updatedAtEpochMillis: number
  lastMessage: map

chats/{chatId}/messages/{messageId}
  senderId: string
  type: "Text" | "Image" | "Voice" | "File"
  encryptedPayload: string
  attachmentUrl: string?
  createdAtEpochMillis: number
  editedAtEpochMillis: number?
  deliveryState: "Sending" | "Sent" | "Delivered" | "Seen"
  reactions: map

groups/{groupId}
  title: string
  ownerId: string
  adminIds: string[]
  memberIds: string[]
```

## End-to-end encryption architecture

The included `EncryptionManager` demonstrates AES-GCM message encryption. For production, add:

- Per-device identity keys
- Signed pre-keys and one-time pre-keys
- X3DH-style session setup or a vetted secure messaging protocol library
- Double-ratchet message keys
- Server storage of public keys only
- Encrypted Firestore payloads, never plaintext message bodies

## Running the app

Open this folder in Android Studio, let Gradle sync, then run the `app` configuration on an emulator or device.

The first build uses an in-memory demo repository so it runs before Firebase is configured. Any 4-6 digit OTP code signs in during demo mode.

## Switching from demo mode to Firebase mode

In `app/build.gradle.kts`, change:

```kotlin
buildConfigField("Boolean", "USE_FIREBASE_BACKEND", "false")
```

to:

```kotlin
buildConfigField("Boolean", "USE_FIREBASE_BACKEND", "true")
```

Then add `google-services.json`, publish the Firebase rules/indexes, and sync the project in Android Studio. The Google Services plugin is applied automatically when `app/google-services.json` exists.

See `PROGRESS_REPORT.md` for the current completion estimate and remaining release blockers.

## Cloud APK build

If you do not want to use Android Studio, use GitHub Actions or Codemagic. The project includes:

- `.github/workflows/android-debug-apk.yml`
- `codemagic.yaml`
- `CLOUD_BUILD_GUIDE.md`

Follow `CLOUD_BUILD_GUIDE.md` to upload the project and download a debug APK.

Persian quick guide: `BUILD_APK_FA.md`.
