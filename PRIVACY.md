# PHEV Link — privacy policy

*Last updated: 26 October 2026 (the last weather forecast is kept on the phone, so the range and smart climate still have a temperature after a restart without internet)*

PHEV Link is an app for the Mitsubishi Outlander PHEV (2014–2017), with an optional "bridge" (a small computer such as a
Raspberry Pi) that you run at home. It is made by Anwo91, a sole proprietorship in Norway ("we").

**In short:** there is no PHEV Link account and no PHEV Link server. What the app learns about your car, your trips and
your charging stays on your phone and on your own bridge. The app only talks to the outside services listed below,
for the reasons given. The Google Play version also shows ads from Google until you buy PHEV Link Plus.

## What the app uses, and where it goes

| What | Why | Where it goes |
| --- | --- | --- |
| Your car's status and the commands you send (battery, charging, climate, locks, VIN) | To show and control the car | Your phone, the car's own Wi-Fi, and your bridge on your home network. Never to us. |
| Precise location, also in the background (optional, off by default) | To measure trips (route and distance) for the electric range | Stays on your phone. Recorded only while a trip is being measured, and only after you allow it. |
| Approximate home location (optional) | The weather forecast, used for the range estimate | The coordinates are sent to MET Norway (api.met.no) to get the forecast. The last forecast, with the place it is for (rounded to about 10 m), is kept on your phone. |
| Where the car is, rounded to about 1 km (car hub mode only) | The weather forecast where the car is, for smart climate (departures) and the range estimate | The car hub's last known location, rounded to about 1 km, is passed to your own paired phones over the car hub's bridge, and sent to MET Norway (api.met.no) for the forecast. Never to us. Without it, the saved location is used. |
| The area around a trip's route | To draw the route map | Map pictures are fetched from OpenStreetMap's tile servers (tile.openstreetmap.org), which see which map area is shown and your IP address. |
| Nearby Wi-Fi networks (car hub mode only, optional) | On a spare phone left in the car: to tell when the car is at home (your home Wi-Fi in sight), so charges are kept and smart charging runs only there | Stays on that phone. Android counts seeing Wi-Fi networks as location, so it asks for location access "all the time". |
| The car hub's bridge (car hub mode only) | Your own phone reaches the car through the spare phone in the car; the car hub also tells it which of its permissions and settings are OK (Bluetooth, location, notifications, battery, background data, alarms, installing updates, motion sensor — only allowed or not), so you can see from your own phone what to fix. It also tells its own battery level and whether it is charging; your phone keeps the last one, to say so if the car hub stops answering | Only between your own devices: over Tailscale (a private network you set up with your own Tailscale account) or your home Wi-Fi. The car hub never answers on mobile data. |
| Built-in Tailscale (car hub mode, and the owner's phone when its bridge is a car hub) | The spare phone in the car joins your own Tailscale network (your tailnet) so your phones and your bridge reach it; your own phone joins it too, inside the app, to reach the car hub from anywhere — only PHEV Link's connections to the car hub go through it (no VPN), and it stops when unused | Connects to Tailscale's coordination and relay servers like any Tailscale device, under your own Tailscale account and Tailscale's privacy policy. No logs are sent to Tailscale. The names and Tailscale addresses of the other devices in your tailnet are shown on the car hub and passed to your own paired phone (to tell why it can't reach the car hub); they stay on your devices. |
| Drives recorded by the car hub (car hub mode only) | The car hub connects to the OBD adapter when the car is switched on and records the drive (distance, speed, engine and battery use; and the route with the car hub's GPS, on by default on a car hub when location "all the time" is allowed there; on other phones only with "Route with GPS" on), so every drive shows in History → Trips on your phones, whoever drove. The car hub also watches its own motion sensor (Android's "significant motion", no permission needed) and its accelerometer (how much the phone is shaken, read on the car hub only, no permission needed): moving counts as a sign the car may be on, and makes it look at the car. Only the largest movement per hour and each look go into its log | Kept on the car hub and passed to your own paired phones over the car hub's bridge (Tailscale or your home Wi-Fi). Never to us. |
| Camera (optional) | Only to scan the code of "Add another phone" (Settings → Car hub → Join a car hub), asked when you tap Scan | The picture stays on the phone and isn't kept; only the code's text is used. |
| Another person's phone (car hub mode, optional) | "Add another phone" lets someone else use the car from their own phone: you share just the car hub with their own Tailscale account in Tailscale, and give them a one-time code | The code works once, for a day, and is kept on the car hub only as a hash. Sharing is done in Tailscale under your account; they see only the car hub. |
| Car health over time (car hub mode, or a phone with its own OBD adapter) | 12 V battery voltage, drive battery health, capacity, cell voltages and temperatures, and the fault codes found, with when — so you can see how the car is doing over months and get a warning when the 12 V battery is low or a new fault code appears | Kept on the phone that reads the adapter (at most the last 30 looks, 40 fault codes and 36 months). In car hub mode also passed to your own paired phones and your bridge at home with the shared data, and to your Home Assistant if you set that up. Never to us. |
| OBD readings through the car hub (car hub mode only, optional) | Your own phone reads the car's diagnostics (live values, fault codes, tests) through the OBD adapter connected to the car hub — or, while the car hub is connected to the car itself, it shows the car hub's live values and has the car hub read or clear the fault codes. Your phone can also list the Bluetooth devices paired on or found near the car hub (names and addresses) to choose its adapter; a PIN you give for it is kept on the car hub to pair with it. While the dashboard is open, new values are sent about once a second (more mobile data than the OBD tab otherwise uses). While your phone borrows the car hub's adapter (live tests, terminal), it sends its live readings back to the car hub every few seconds, so the car hub keeps recording the drive | Passed straight between your own two phones, the same way as the car hub's bridge, with the bridge's key. Nothing more is kept on the car hub. |
| Bluetooth devices | To connect to an OBD adapter, and to notice when your phone joins the car's Bluetooth | Stays on your phone. |
| Your electricity price area | Charging costs and smart charging | Your bridge fetches the day's prices for that area from hvakosterstrommen.no (Norway) or elprisetjustnu.se (Sweden). Only the area and the date are sent. |
| The app's own log | To find out what went wrong | Stays on your phone: kept in the app's private storage (at most about 8 MB, the oldest lines dropped first) so it survives restarts and crashes. It only leaves the phone if you share it yourself (Settings → Help and support). The car hub's log can be fetched by your own paired phone (Settings → Car hub → Get its log), over the car hub's bridge, to share it from there. |
| Automatic log upload (developer feature, **off by default**, only in the version from GitHub — the Google Play version has no log upload) | So the developer can read the logs of their own phones, Pi and car hub without sharing them by hand | Only when switched on in Diagnostics → Tools with a GitHub token: the app's log (and the Pi's and car hub's, when switched on there from the same phone) is sent, gzipped, to the private GitHub repository the token gives access to (GitHub's privacy policy applies), every 30–60 minutes and after each trip and charge. The log can hold places (GPS of drives, the weather place), car readings and device names. Each device's files are deleted from the repository after 30 days. The token is kept in the app's private storage (on the Pi in a file only the bridge can read), goes to the Pi and the car hub only over the link the app already uses with them (home Wi-Fi or Tailscale), and is never logged. Switching it off stops it and deletes what was waiting to be sent. |
| Google Home, Apple Home, Alexa, Home Assistant (optional) | Smart-home control of the car | Runs on your bridge in your home network, between the bridge and your smart-home system. |
| Update checks | To offer new versions | The GitHub version of the app, and the bridge, ask GitHub (github.com) for the newest release. GitHub sees your IP address. The Google Play version is updated by Google Play. |

## Google Play version: purchases and ads

**Purchases.** PHEV Link Plus and tips are bought through Google Play. Google handles the payment; we only learn from
Google Play whether Plus has been bought (no name, address or payment details). See Google's privacy policy:
https://policies.google.com/privacy

**Ads.** Until PHEV Link Plus is bought, the Google Play version shows small ads from Google AdMob. To show ads, measure
them and prevent fraud, Google's ads software on your phone collects information such as your device's advertising ID,
your IP address and the approximate location derived from it, and how ads are shown and tapped. In the EEA, the UK and
Switzerland, Google asks for your consent first (personalised ads, or not), and you can change your answer at any time
under Settings → PHEV Link Plus → Ad privacy choices. You can also reset or delete your advertising ID in Android's
settings. How Google uses this information: https://policies.google.com/technologies/partner-sites

With PHEV Link Plus there are no ads, and the ads software is not started.

## How long it is kept

Everything the app keeps is on your phone (and on your bridge, if you have one) until you delete it in the app, clear the
app's data or uninstall it. We keep nothing, because nothing is sent to us.

## Your rights

Under the GDPR you can ask for access to, correction or deletion of personal data about you. We hold no personal data
about you; for data collected by Google for ads and purchases, see Google's privacy policy and
https://myaccount.google.com. You can also complain to the Norwegian Data Protection Authority (Datatilsynet).

## Children

PHEV Link is made for car owners and is not directed at children.

## Changes

If this policy changes, the new version is published here with a new date.

## Contact

Write to the developer address shown under "Developer contact" on PHEV Link's Google Play page.
