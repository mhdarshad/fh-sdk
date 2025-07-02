# 📦 FHSDK Integration Guide
[![](https://jitpack.io/v/mhdarshad/fh-sdk.svg)](https://jitpack.io/#mhdarshad/fh-sdk)

## Overview

`FHSDK` is a lightweight Android SDK that integrates Firebase Cloud Messaging (FCM) and Matomo Analytics, along with FirstHive subscription APIs. It offers seamless push tracking, user analytics, and event monitoring for Android apps.

---

## 📁 Package

```kotlin
package com.cxwai.fh.fhsdk
```

---

## 🔧 Prerequisites

Before integrating FHSDK, ensure the following:

1. **Matomo Tracking Setup**
   - Matomo Server URL (e.g., `https://your-matomo-instance.com`)
   - Valid Site ID

2. **Firebase Setup**
   - Add `google-services.json` to your app module

3. **Permissions**
   Add the following to `AndroidManifest.xml`:

   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```

4. **Minimum SDK Version**
   - Ensure your app’s `minSdkVersion` is **23** (as per SDK requirement)

---

## ⚙️ Dependency Setup

Sample FHSDK Project Structure:

```
├── app/
├── fhsdk/
├── packages/
│   └── fhsdk-release.aar ← 📦 AAR File
├── build.gradle.kts
└── settings.gradle.kts
```

Include the SDK in your `build.gradle.kts` or `build.gradle`:

```kotlin
implementation(files("../packages/fhsdk-release.aar"))
```

---

## 🔌 SDK Initialization

Import and initialize the SDK (e.g., in `Application` class):

```kotlin
FHSDK.initialize(
    context = applicationContext,
    matomoUrl = "https://your-matomo-url.com",
    siteId = 1
)
```

This will:

- Set up the Matomo Tracker  
- Prepare internal structures for event and screen tracking

---

## 🚀 Usage Guide

### 1. Track Events (Predefined)

```kotlin
FHSDK.trackEvent(
    category = FHSDK.Category.VIEWED,
    action = FHSDK.Action.PAGE_VISITED,
    name = "HomeScreen"
)
```

### 2. Track Events (Custom Category/Action)

```kotlin
FHSDK.trackEvent(
    category = "UserJourney",
    action = "Step1",
    name = "FromHomeToSearch"
)
```

### 3. Track Screens

```kotlin
FHSDK.trackScreen(
    path = "/home",
    title = "Home Screen"
)
```

### 4. Track Activity Automatically

```kotlin
FHSDK.trackActivity(this)
```

### 5. Track Event with Custom Dimensions

```kotlin
FHSDK.trackEvent(
    category = FHSDK.Category.PURCHASED,
    action = FHSDK.Action.PRODUCT_PURCHASED,
    Dimension(1, "ProductID:1234"),
    Dimension(2, "UserType:Premium")
)
```

### 6. Update User ID

```kotlin
// (Only used internally by SDK, handled via `subscribe`)
FHSDK.updateUser("user_123")
```

---

## 🔔 FirstHive Subscription

Subscribe a user using mobile/email and updated FCM token:

```kotlin
// Store updated token on token refresh
FHSDK.updateToken(context, token = "new_fcm_token")

// Then call subscribe (usually on app start)
FHSDK.subscribe(
    context = this,
    mobileNo = "9999999999",
    email = "user@example.com"
)
```

🔹 `environmentName` is **mandatory** and is handled internally.

---

## 📚 Enums Reference

### Categories (Examples)

- `VIEWED`
- `PURCHASED`
- `APP`
- `VIDEOS`
- `LIVE_TV`

### Actions (Examples)

- `PAGE_VISITED`
- `PRODUCT_PURCHASED`
- `LOGIN`
- `VIDEO_PLAY`

---

## 🛠 Logging & Debugging

SDK logs are tagged with:

```
TAG: FHSDK
```

Use `Log.d` and `Log.e` filters to debug events and track API activity.

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

- `google-services.json` must be correctly placed in your `app/` folder
- FirstHive subscription is asynchronous; monitor logs for response
- FCM token updates must be handled via `updateToken()` for reliable tracking
- Matomo Tracker is a singleton – only initialize once

---
