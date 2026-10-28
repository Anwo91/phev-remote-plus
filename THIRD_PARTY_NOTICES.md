# Third-party notices

PHEV Link is proprietary software, © 2026 Anwo91, all rights reserved (see `LICENSE`). This file lists the
software and data from others that it uses, and their licences. The same list, with the full licence texts,
ships inside the app (Settings → About → Licences) and in the bridge bundle (`NOTICES.txt`); its source is
`app/src/main/res/raw/licences.txt`.

## In the app and the bridge

### Kotlin standard library and kotlinx.coroutines

- Copyright JetBrains s.r.o. and Kotlin Programming Language contributors
- License: Apache License 2.0

### Former: OBDb signal definitions (1.0.92 to 1.0.98)

Versions 1.0.92 to 1.0.98 carried `obdb-outlander.tsv`, adapted from OBDb
(https://github.com/OBDb/Mitsubishi-Outlander, © the OBDb contributors) under CC BY-SA 4.0. Later versions use
their own table, `outlander-signals.tsv`, written in a clean room from a facts-only description (which control unit,
which bytes, which scaling) without access to the OBDb data; the record is kept in `docs/cleanroom/obd-signals/`.
Thanks to the OBDb contributors, whose work first showed where these values live.

## In the app

### Android Jetpack (AndroidX)

- Core, Activity, Lifecycle, Compose UI, Material 3, Material Icons, Glance, DataStore, WorkManager
- Copyright The Android Open Source Project
- License: Apache License 2.0

### ZXing and ZXing Android Embedded (QR codes for "Add another phone")

- ZXing core 3.5.3, https://github.com/zxing/zxing — Copyright ZXing authors — Apache License 2.0
- ZXing Android Embedded 4.3.0, https://github.com/journeyapps/zxing-android-embedded — Copyright JourneyApps — Apache License 2.0

### Firebase Cloud Messaging (instant alerts by push)

- Firebase Android SDK: Cloud Messaging 25.0.2 and what it brings (Installations, Common, Components, Data Transport),
  https://github.com/firebase/firebase-android-sdk — © Google LLC — Apache License 2.0
- Google Play services client libraries it needs (`com.google.android.gms:play-services-base`, `-basement`, `-tasks`,
  `-cloud-messaging`, `-stats`) — © Google LLC — Android Software Development Kit License
  (https://developer.android.com/studio/terms)
- The alerts go through Firebase Cloud Messaging (Google) and PHEV Link's own push relay (`relay/`, on Cloudflare
  Workers), encrypted so neither can read them. No Firebase Analytics.

### Weather

- Forecast data from MET Norway (Locationforecast, https://api.met.no/), CC BY 4.0 and NLOD 2.0.

### Map of a drive's route (History → Trips)

- Map tiles © OpenStreetMap contributors, from https://tile.openstreetmap.org, shown under the Open Database License
  (https://www.openstreetmap.org/copyright). The app shows "© OpenStreetMap" on every map (tapping it opens that page),
  sends its own user agent and keeps fetched tiles for 30 days, as the OpenStreetMap tile usage policy asks.

### Electricity prices (smart charging)

- Prices provided by Hva koster strømmen.no (https://www.hvakosterstrommen.no, Norway) and Elpriset just nu.se
  (https://www.elprisetjustnu.se, Sweden), fetched by the bridge; both are free for any use and ask to be named where
  the prices are shown, which the app does under the price chart.

### Built-in Tailscale (car hub mode, 64-bit phones)

The car hub joins the owner's tailnet with Tailscale's `tsnet` library, built from `tailnet/` with gomobile into
`libgojni.so` (the Tailscale app's VPN would keep the app off the car's Wi-Fi). No logs are sent to Tailscale.

- Tailscale, https://github.com/tailscale/tailscale — © Tailscale Inc & contributors — BSD 3-Clause License
- wireguard-go (Tailscale's fork) — © WireGuard LLC — MIT License
- gVisor network stack, https://gvisor.dev — © The gVisor Authors — Apache License 2.0
- Go standard library and golang.org/x modules (crypto, net, sys, text, time, mobile …) — © The Go Authors — BSD 3-Clause
- Other Go modules it pulls in, each under a BSD, MIT, ISC or Apache 2.0 licence: listed with their full licence
  texts at the end of the in-app list, generated at build time by `tools/go_notices.py` (the build stops if a
  module has no licence file or a copyleft one)

### Google Play version only

- Google Mobile Ads SDK (`com.google.android.gms:play-services-ads`), Google User Messaging Platform
  (`com.google.android.ump:user-messaging-platform`) and Google Play Billing Library
  (`com.android.billingclient:billing`), © Google LLC — Android Software Development Kit License and the Google APIs
  Terms of Service (https://developers.google.com/terms). Not in the GitHub build.

## In the bridge (`bridge/matter`)

The bridge's Matter program (`phev-matter.mjs`, which lets Google Home, Apple Home and Alexa add the car) is one file
built with esbuild from the packages below. It ships in the Raspberry Pi bundle, not in the app.

- matter.js (`@matter/main`, `@matter/general`, `@matter/model`, `@matter/node`, `@matter/nodejs`,
  `@matter/protocol`, `@matter/types` 0.15.6), https://github.com/project-chip/matter.js — Apache License 2.0
- noble-curves 1.9.7 and noble-hashes 1.8.0, © Paul Miller — MIT
- qrcode-generator 1.4.4, © 2009 Kazuhiko Arase — MIT (draws the pairing QR code)

## Only for testing (not in the app or the bridge)

Used by the tests in CI; none of it ships.

- JUnit 4 — Eclipse Public License 1.0
- Robolectric 4.16.1 (runs the app's tests on a PC), https://robolectric.org — MIT
- AndroidX Test and Compose UI Test (`androidx.test:core`, `androidx.compose.ui:ui-test-junit4`) — Apache License 2.0
- kotlinx-coroutines-test — Apache License 2.0
- Wrangler 4.143.1 (Cloudflare's tool; deploys the push relay from CI), https://github.com/cloudflare/workers-sdk — MIT or
  Apache License 2.0

## Written for this project

Everything else is PHEV Link's own work, © 2026 Anwo91, all rights reserved. That includes the car protocol
implementation, the fault-code titles (`codes*.tsv`), the Mitsubishi code descriptions (`mitsubishi.tsv`), all
explanations and translations, the vehicle-settings names and texts, the tips and the live tests.
The fault-code titles, the component names in the explanations, the Mitsubishi code descriptions and the
vehicle-settings names were written in a clean room (records in `docs/cleanroom/`).

The fault-code explanations are general guidance only and do not replace a diagnosis by a qualified workshop. The
meaning of a code can differ between manufacturers and model years. Work on the high-voltage system (orange cables,
drive battery, inverters, charger) and on airbags must only be done by trained technicians.

Mitsubishi, Outlander and Outlander PHEV are trademarks of Mitsubishi Motors Corporation. PHEV Link is not
affiliated with, endorsed by or sponsored by Mitsubishi Motors Corporation.
