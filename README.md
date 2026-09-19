# CLARIO Android wrapper

Native Android WebView shell for the production CLARIO app:
https://debate-social.hatchable.site

- App ID: `app.clario.social`
- Version: `1.0.0` (`versionCode 1`)
- minSdk 26 / targetSdk 36 / compileSdk 36
- Android Gradle Plugin 8.13.2 / Gradle 8.13
- No backend/database secrets are embedded. The Supabase publishable key remains part of the existing public web client, as designed.
- HTTPS cleartext is disabled.
- File/image/video chooser is supported by `WebChromeClient`.
- Fullscreen HTML5 video is supported.
- Android back/predictive-back calls the production page's `window.CLARIO_ANDROID_BACK()` handler before allowing native exit.
- HTTPS deep links route to the live CLARIO URL.

## Automatic GitHub build

The included `.github/workflows/android.yml` builds on every push to `main` and can also be run manually from GitHub Actions.

It produces:

- `CLARIO-v1.0.0-test.apk` — release APK signed with a one-time test-only CI key so it can be installed directly for testing.
- `app-release.aab` — release bundle generated for later Play preparation. Before Play submission, use a private long-lived upload key / Play App Signing rather than the temporary test key.

Because the test APK signing key is generated fresh for each workflow run, a later test build may require uninstalling the previous test APK before installing the new one. This is intentional and avoids storing a reusable private signing key in the repository.

## Local build

Open this folder in a current Android Studio installation with Android SDK 36 installed, or build with Gradle 8.13 + JDK 17.

A production Play release must use a private long-lived signing key / Play App Signing; do not reuse any CI test key.

## Deep-link verification

The manifest declares HTTPS App Links. For automatic verified opening without an Android chooser, CLARIO must serve `/.well-known/assetlinks.json` containing the SHA-256 certificate fingerprint of the final Android signing certificate. The Play App Signing fingerprint should be used for the Play release.

## OAuth note

The wrapper keeps CLARIO/Supabase pages in the WebView and opens non-CLARIO/non-Supabase sites in the system browser. OAuth providers can restrict embedded WebViews; production Google sign-in should be verified on a physical Android device. Email magic-link sign-in remains available through CLARIO's existing auth flow.
