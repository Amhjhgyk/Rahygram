# Build RahyGram APK Without Android Studio

You can build RahyGram with GitHub Actions or Codemagic. Both produce an APK you can download.

## Option 1: GitHub Actions

1. Create a GitHub repository.
2. Upload this whole project folder to the repository.
3. Open the repository on GitHub.
4. Go to **Actions**.
5. Choose **Android Debug APK**.
6. Click **Run workflow**.
7. Wait until the build finishes.
8. Open the finished workflow run.
9. Download the artifact named `rahygram-debug-apk`.

The APK will be inside that downloaded zip file.

### Firebase mode on GitHub

For demo mode, do nothing else.

For real Firebase mode:

1. Download `google-services.json` from Firebase.
2. Convert it to Base64.
3. Add it as a GitHub repository secret named:

```text
GOOGLE_SERVICES_JSON_BASE64
```

4. In `app/build.gradle.kts`, change:

```kotlin
buildConfigField("Boolean", "USE_FIREBASE_BACKEND", "false")
```

to:

```kotlin
buildConfigField("Boolean", "USE_FIREBASE_BACKEND", "true")
```

5. Run the workflow again.

## Option 2: Codemagic

1. Create a GitHub repository.
2. Upload this project to GitHub.
3. Create an account at Codemagic.
4. Connect your GitHub account.
5. Select the RahyGram repository.
6. Codemagic will detect `codemagic.yaml`.
7. Start the workflow named **RahyGram Debug APK**.
8. Download the APK from **Artifacts** after the build finishes.

### Firebase mode on Codemagic

For demo mode, do nothing else.

For real Firebase mode:

1. Convert `google-services.json` to Base64.
2. In Codemagic, create an environment variable:

```text
GOOGLE_SERVICES_JSON_BASE64
```

3. Put it in an environment group named:

```text
firebase
```

4. Set `USE_FIREBASE_BACKEND` to `true` in `app/build.gradle.kts`.
5. Start the build again.

## Important

The generated APK is a debug APK. It is good for testing and installing on your phone.

For Bazaar, Myket, Google Play, or public release, you need a signed release APK/AAB.
