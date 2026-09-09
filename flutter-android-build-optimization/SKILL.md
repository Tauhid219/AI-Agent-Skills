---
name: flutter-android-build-optimization
description: Enforces best practices for building, optimizing, and packaging Flutter Android applications. Prevents unintentional debug builds, oversized artifacts (Fat APKs), release signing issues, and deployment errors. Use when building APK/AAB, analyzing build size, or preparing for Play Store release.
---

# Flutter Android Build & Release Optimization

This skill enforces production-grade standards for compiling, shrinking, signing, and packaging Flutter Android applications. It prevents unintentional debug compilation, oversized monolithic Fat APKs, signing vulnerabilities, and Play Store upload rejections.

## Trigger Conditions
Activate whenever:
- The user requests to build, export, or generate an Android APK or App Bundle (`.aab`).
- Preparing a Flutter application for manual QA testing, client demos, or Play Store distribution.
- Diagnosing or troubleshooting large build sizes (e.g., APK > 25–40 MB for standard apps).
- Configuring Android release signing, R8/ProGuard shrinking, or build obfuscation.

---

## Operational Guidelines & Rules

### 1. Default Mode & Target Clarification
- **NEVER suggest a bare `flutter build apk` without context.** A bare build produces an unoptimized, monolithic "Fat APK" containing native binaries for all CPU architectures (`armeabi-v7a`, `arm64-v8a`, `x86_64`), leading to bloated downloads.
- Clarify the deployment destination first:
  1. **Direct Device Testing / Client Demo:** Use ABI-split APKs.
  2. **Google Play Store / Production Distribution:** Use Android App Bundle (`.aab`).
  3. **Universal APK (Direct web download for heterogeneous devices):** Only if specifically requested; warn about file size tradeoffs.

### 2. Command Standards

#### A. For Device Testing & Client Review (ABI Split)
Always recommend ABI splitting to keep file sizes minimal (~10–18 MB for baseline apps):
```bash
flutter build apk --split-per-abi --release
```
- **Target Artifacts** (located in `build/app/outputs/flutter-apk/`):
  - `app-arm64-v8a-release.apk` *(Default for modern physical Android devices; ~95% of current active devices)*
  - `app-armeabi-v7a-release.apk` *(Legacy 32-bit Android phones)*
  - `app-x86_64-release.apk` *(Emulators and ChromeOS)*

#### B. For Play Store Submissions (App Bundle)
Google Play requires `.aab` format so Google Play Dynamic Delivery generates device-tailored APKs:
```bash
flutter build appbundle --release
```
- **Crucial Rule:** Explicitly clarify to the user that `.aab` files **cannot** be sideloaded directly onto a physical phone via USB/WhatsApp/Drive without `bundletool`.
- Output path: `build/app/outputs/bundle/release/app-release.aab`.

#### C. Code Obfuscation & Symbol Stripping (Production Hardening)
To hide class/method names and further reduce release size, split debug symbols:
```bash
# For App Bundle:
flutter build appbundle --release --obfuscate --split-debug-info=build/app/outputs/symbols

# For APK:
flutter build apk --split-per-abi --release --obfuscate --split-debug-info=build/app/outputs/symbols
```
*(Store the `symbols` directory safely to de-obfuscate crash traces from Crashlytics or Play Console).*

---

### 3. Size Optimization & Code Shrinking (R8 / ProGuard)

Verify code and resource shrinking in Android build configuration:

#### Groovy DSL (`android/app/build.gradle`):
```groovy
android {
    ...
    buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
    }
}
```

#### Kotlin DSL (`android/app/build.gradle.kts`):
```kotlin
android {
    ...
    buildTypes {
        release {
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
            signingConfig = signingConfigs.getByName("release")
        }
    }
}
```

#### Handling R8 / ProGuard Breakages:
If third-party plugins fail at runtime in release mode due to reflection, preserve them in `android/app/proguard-rules.pro`:
```proguard
# Preserve Flutter wrapper classes
-keep class io.flutter.app.** { *; }
-keep class io.flutter.plugin.**  { *; }
-keep class io.flutter.util.**  { *; }
-dontwarn io.flutter.embedding.**
```

---

### 4. Release Keystore & Signing Guardrails

- **NEVER** commit raw keystores (`key.jks`, `upload-keystore.jks`) or passwords to version control.
- Use `key.properties` pattern:
  1. Create `android/key.properties` (ensure it is added to root `.gitignore`):
     ```properties
     storePassword=your-store-password
     keyPassword=your-key-password
     keyAlias=upload
     storeFile=../upload-keystore.jks
     ```
  2. Load securely in `android/app/build.gradle`:
     ```groovy
     def keystoreProperties = new Properties()
     def keystorePropertiesFile = rootProject.file('key.properties')
     if (keystorePropertiesFile.exists()) {
         keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
     }

     android {
         signingConfigs {
             release {
                 if (keystorePropertiesFile.exists()) {
                     keyAlias keystoreProperties['keyAlias']
                     keyPassword keystoreProperties['keyPassword']
                     storeFile file(keystoreProperties['storeFile'])
                     storePassword keystoreProperties['storePassword']
                 }
             }
         }
     }
     ```

---

### 5. Build Diagnostics & Troubleshooting Checklist

- **Detect Unintentional Debug Builds:**
  If the generated file is `app-debug.apk`, the project was compiled in debug mode with the JIT Dart VM enabled (slow performance, 3x size). Always enforce `--release`.
- **Pre-release Clean:**
  Always flush cached artifacts before release builds to avoid stale asset bundling or Gradle daemon anomalies:
  ```bash
  flutter clean && flutter pub get
  ```
- **Size Profiling & Deep Audit:**
  To analyze exactly what contributes to the APK/AAB size:
  ```bash
  flutter build apk --analyze-size --split-per-abi
  # or
  flutter build appbundle --analyze-size
  ```
- **Asset Auditing:**
  If the APK is unexpectedly large (> 25 MB for basic apps):
  - Audit `assets/` for uncompressed `.png` images; convert to `.webp` or vector `.svg` where applicable.
  - Check for unused custom `.ttf` or `.otf` fonts.
  - Verify if heavy unused dependencies are included in `pubspec.yaml`.
- **Version Number Alignment:**
  Ensure version code bumps to avoid Play Console reject errors:
  - In `pubspec.yaml`: `version: 1.0.1+2` (format: `x.y.z+buildNumber`) or pass via CLI: `--build-name=1.0.1 --build-number=2`.
