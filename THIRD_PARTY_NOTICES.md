# Third-party notices

PHEV Link is proprietary software, © 2026 Anwo91, all rights reserved (see `LICENSE`). This file lists the
software and data from others that it uses, and their licences. The same list, with the full licence texts,
ships inside the app (Settings → About → Licences) and in the bridge bundle (`NOTICES.txt`); its source is
`app/src/main/res/raw/licences.txt`.

## In the app and the bridge

### Kotlin standard library and kotlinx.coroutines

- Copyright JetBrains s.r.o. and Kotlin Programming Language contributors
- License: Apache License 2.0

### OBDb signal definitions

`core/src/main/resources/io/github/anwo91/phevlink/core/obd/obdb-outlander.tsv` holds the engine ECU service 21
values, instrument cluster, air conditioning, ABS wheel speeds and AWC signals of the non-PHEV Outlander. The app
reads that file at run time; no other part of PHEV Link is derived from OBDb.

- Project: OBDb, https://github.com/OBDb/Mitsubishi-Outlander
- File used: `signalsets/v3/default.json` (commit `2a1fef2f4916048bd43ac7f24ae2fe813b6c6099`)
- Copyright: the OBDb contributors
- License: Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0),
  https://creativecommons.org/licenses/by-sa/4.0/. The adapted file is shared under the same licence (it is
  published next to the releases).
- Changes: only a subset was taken (no signals that need a diagnostic session, no diesel/turbo-only or
  4th-generation signals), converted from JSON to a table with new ids, names and groups. Every one is marked
  experimental in the app, because the set was written for the non-PHEV Outlander.

## In the app

### Android Jetpack (AndroidX)

- Core, Activity, Lifecycle, Compose UI, Material 3, Material Icons, Glance, DataStore
- Copyright The Android Open Source Project
- License: Apache License 2.0

### Weather

- Forecast data from MET Norway (Locationforecast, https://api.met.no/), CC BY 4.0 and NLOD 2.0.

### Map of a drive's route (History → Trips)

- Map tiles © OpenStreetMap contributors, from https://tile.openstreetmap.org, shown under the Open Database License
  (https://www.openstreetmap.org/copyright). The app shows "© OpenStreetMap" on every map (tapping it opens that page),
  sends its own user agent and keeps fetched tiles for 30 days, as the OpenStreetMap tile usage policy asks.

## In the bridge (`bridge/matter`)

The bridge's Matter program (`phev-matter.mjs`, which lets Google Home, Apple Home and Alexa add the car) is one file
built with esbuild from the packages below. It ships in the Raspberry Pi bundle, not in the app.

- matter.js (`@matter/main`, `@matter/general`, `@matter/model`, `@matter/node`, `@matter/nodejs`,
  `@matter/protocol`, `@matter/types` 0.15.6), https://github.com/project-chip/matter.js — Apache License 2.0
- noble-curves 1.9.7 and noble-hashes 1.8.0, © Paul Miller — MIT
- qrcode-generator 1.4.4, © 2009 Kazuhiko Arase — MIT (draws the pairing QR code)

## Written for this project

Everything else is PHEV Link's own work, © 2026 Anwo91, all rights reserved. That includes the car protocol
implementation, the fault-code titles (`codes*.tsv`), the Mitsubishi code descriptions (`mitsubishi.tsv`), all
explanations and translations, the vehicle-settings names and texts, the tips and the live tests.

The fault-code explanations are general guidance only and do not replace a diagnosis by a qualified workshop. The
meaning of a code can differ between manufacturers and model years. Work on the high-voltage system (orange cables,
drive battery, inverters, charger) and on airbags must only be done by trained technicians.

Mitsubishi, Outlander and Outlander PHEV are trademarks of Mitsubishi Motors Corporation. PHEV Link is not
affiliated with, endorsed by or sponsored by Mitsubishi Motors Corporation.
