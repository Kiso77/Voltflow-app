# Voltflow-app
# VoltFlow - Electrical House Wiring Booking App

A native Android app (Kotlin, Jetpack Compose, Material 3) where customers browse home wiring services and book an electrician visit.

## Features

- 7 wiring services: new house wiring, rewiring, MCB/distribution board, earthing, fan and light fitting, fault repair
- Booking form: name, phone, site address, date (future dates only), time slot, notes, with validation
- Confirmation screen with a booking ID
- My bookings list with cancel option
- Bookings saved on the device, light and dark theme

## Run it

1. Install Android Studio (Koala or newer).
2. Open the `VoltFlow` folder (File > Open). Let Gradle sync finish.
3. Press Run with an emulator or a phone (Android 8.0+, API 26).
4. For an installable APK: Build > Build Bundle(s) / APK(s) > Build APK(s).

## Project layout

- `app/src/main/java/com/voltflow/app/data/Models.kt` - service catalog, time slots (edit these to change offerings)
- `app/src/main/java/com/voltflow/app/data/BookingRepository.kt` - on-device storage
- `app/src/main/java/com/voltflow/app/BookingViewModel.kt` - booking logic
- `app/src/main/java/com/voltflow/app/ui/VoltFlowApp.kt` - all screens

## Current limits and next steps

Bookings are stored only on the customer's phone, so no electrician receives them yet. To make this a working business, add a backend (Firebase Firestore is the quickest option), phone OTP login, an electrician or admin app to accept jobs, push notifications, and online payment (Razorpay or UPI).

## Supabase

Bookings are stored online in Supabase. One-time setup:
1. Supabase dashboard -> SQL Editor -> paste and run `supabase_setup.sql`.
2. To confirm a booking, open Table Editor -> bookings and set `status` to `confirmed`.

Connection details are in `app/src/main/java/com/voltflow/app/data/SupabaseConfig.kt` (publishable key only; never put the secret key in the app).

To let the electrician block days, also run `supabase_blocked_days.sql` (it is included with the admin app).

## Releasing an update

1. In `app/build.gradle.kts` raise `versionCode` by 1 (e.g. 1 -> 2) and change `versionName` (e.g. "1.1").
2. Build the signed release APK (Build > Generate Signed App Bundle / APK > APK > release) with the SAME key store as before.
3. Upload `VoltFlow-release.apk` somewhere with a direct download link (GitHub Releases, your website, ...).
4. Supabase > Table Editor > `app_update` > edit the one row:
   - `latest_version_code` = the new versionCode (2), `latest_version_name` = "1.1"
   - `download_url` = the APK link, `release_notes` = a short "what's new"
   - `min_version_code` = leave as is for an optional update; set it equal to the new versionCode to FORCE everyone to update.
5. Customers see "Update available" the next time they open the app.

First run `supabase_app_update.sql` once in the Supabase SQL Editor.
