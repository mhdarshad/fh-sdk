# 📦 FHSDK Integration Guide
[![](https://jitpack.io/v/mhdarshad/fh-sdk.svg)](https://jitpack.io/#mhdarshad/fh-sdk)

## Overview

`FHSDK` is a lightweight Android SDK designed to seamlessly integrate Firebase Cloud Messaging (FCM) and Matomo tracking, along with FirstHive subscription capabilities, providing unified analytics and push tracking in your Android applications.

---

## 📁 Package

```kotlin
package com.cxwai.fh.fhsdk
```

---

## 🔧 Prerequisites

Before integrating FHSDK, ensure the following:

1. **Matomo Tracking Setup**
   - Your Matomo server URL (e.g., `https://your-matomo-instance.com`)
   - A valid Site ID

2. **Firebase Setup**
   - Add a valid `google-services.json` file to your app module

3. **Permissions**
   Add internet access permission in your `AndroidManifest.xml`:

   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```

---

## ⚙️ Dependency Setup


Sample Testing FHSDK/

```txt
├── .gradle/
├── .idea/
├── .kotlin/
├── app/
├── build/
├── fhsdk/
├── fhsdkshared/
├── gradle/
├── packages/
│ └── fhsdk-release.aar ← 📦 AAR File
├── .gitignore
├── build.gradle.kts
├── gradle.properties
├── gradlew
├── gradlew.bat
├── local.properties
└── settings.gradle.kts

```

In your app’s `build.gradle.kts` or `build.gradle`: file, include the following dependencies:

```kotlin
implementation(files("../packages/fhsdk-release.aar"))
```

---

## 🔌 SDK Initialization

Import the SDK:

```kotlin
import com.firsthive.fhsdk.FHSDK
```

Initialize `FHSDK` in your `Application` class or at app launch:

```kotlin
FHSDK.initialize(
    context = applicationContext,
    matomoUrl = "https://your-matomo-url.com",
    siteId = 1
)
```

This call performs:

- Matomo tracker setup  
- Firebase token fetch and tracking  
- Core SDK initialization

---

## 🚀 Usage Guide

### 1. Track Events

```kotlin
FHSDK.trackEvent(
    category = FHSDK.Category.VIEWED,
    action = FHSDK.Action.PAGE_VISITED,
    name = "HomeScreen"
)
```

### 2. Track Screens

```kotlin
FHSDK.trackScreen(
    path = "/home",
    title = "Home Screen"
)
```

### 3. Track Activity Automatically

Inside any `Activity`:

```kotlin
FHSDK.trackActivity(this)
```

### 4. Track Event with Custom Dimensions

```kotlin
FHSDK.trackEvent(
    category = FHSDK.Category.PURCHASED,
    action = FHSDK.Action.PRODUCT_PURCHASED,
    Dimension(1, "ProductID:1234"),
    Dimension(2, "UserType:Premium")
)
```

### 5. Update User ID

To associate events with a user:

```kotlin
FHSDK.updateUser(userId = "user_123")
```

---

## 🔔 FirstHive Subscription

Subscribe a user using mobile/email and FCM token:

```kotlin
FHSDK.subscribe(
    mobileNo = "9999999999",
    email = "user@example.com",
    userId = "user_123",
    token = "fcm_token_received"
)
```

---

## 📚 Enums Reference

### Categories

Event groupings, e.g.:

- `VIEWED`, `PURCHASED`, `APP`, `VIDEOS`, `LIVE_TV`, etc.

### Actions

User actions, e.g.:

- `PAGE_VISITED`, `PRODUCT_PURCHASED`, `LOGIN`, `VIDEO_PLAY`, etc.

---

## 🛠 Logging & Debugging

Internal logs and events are tagged with:

```
TAG: FHSDK
```

Use `Log.d` and `Log.e` filters to monitor SDK behavior.

---

## ✅ Example: Application Class Setup

```kotlin
class MyApp : Application() {
    override fun onCreate() {
        super.onCreate()
        FirebaseApp.initializeApp(this)
        FHSDK.initialize(
            context = this,
            matomoUrl = "https://matomo.yoursite.com",
            siteId = 2
        )
    }
}
```

---

## 🔍 Notes

- Make sure Firebase is initialized correctly using `google-services.json`.
- The Matomo `Tracker` is a singleton and is initialized only once.
- FirstHive subscriptions are asynchronous; check logs for results.

---

