# PHEV Link — privacy policy

*Last updated: 27 October 2026 (your settings can be kept in Android's own backup, and saved to or restored from a file; the monthly costs can be saved as a spreadsheet; departures can come from your calendar)*

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
| The electric range the car hub learned (car hub mode only) | Home Assistant's range and the weekly summary on your bridge | The car hub's range per full battery (now and at 20 °C, the temperature, how many drives it is from) is kept with the shared data on the car hub and copied to your bridge at home. Never to us. |
| Fill-ups you enter (optional) | To check and correct the trips' fuel against the pump | The litres, whether the tank was filled up to full, the price per litre, when (between which trips), and the name of the phone that saved it, kept on your phone and, with a bridge, with the shared data on your bridge or car hub so your other phones see them. Never to us. |
| Where the car is, rounded to about 1 km (car hub mode only) | The weather forecast where the car is, for smart climate (departures) and the range estimate | The car hub's last known location, rounded to about 1 km, is passed to your own paired phones over the car hub's bridge, and sent to MET Norway (api.met.no) for the forecast. Never to us. Without it, the saved location is used. |
| The area around a trip's route | To draw the route map | Map pictures are fetched from OpenStreetMap's tile servers (tile.openstreetmap.org), which see which map area is shown and your IP address. |
| Nearby Wi-Fi networks (car hub mode only, optional) | On a spare phone left in the car: to tell when the car is at home (your home Wi-Fi in sight), so charges are kept and smart charging runs only there | Stays on that phone. Android counts seeing Wi-Fi networks as location, so it asks for location access "all the time". |
| The car hub's bridge (car hub mode only) | Your own phone reaches the car through the spare phone in the car; the car hub also tells it which of its permissions and settings are OK (Bluetooth, location, notifications, battery, background data, alarms, installing updates, motion sensor — only allowed or not — and whether its battery is kept full on power or stops at a charging limit, worked out from its own battery level and charging state), so you can see from your own phone what to fix. It also tells its own battery level and whether it is charging; your phone keeps the last one, to say so if the car hub stops answering | Only between your own devices: over Tailscale (a private network you set up with your own Tailscale account) or your home Wi-Fi. The car hub never answers on mobile data. |
| Built-in Tailscale (car hub mode, and the owner's phone when its bridge is a car hub) | The spare phone in the car joins your own Tailscale network (your tailnet) so your phones and your bridge reach it; your own phone joins it too, inside the app, to reach the car hub from anywhere — only PHEV Link's connections to the car hub go through it (no VPN), and it stops when unused (with instant alerts on, it stays on to keep their connection) | Connects to Tailscale's coordination and relay servers like any Tailscale device, under your own Tailscale account and Tailscale's privacy policy. No logs are sent to Tailscale. The names and Tailscale addresses of the other devices in your tailnet are shown on the car hub and passed to your own paired phone (to tell why it can't reach the car hub); they stay on your devices. |
| Instant alerts (owner's phone with a car hub or bridge, on by default, Settings → Notifications) | Your phone keeps one connection to your car hub (or bridge) open in the background, so its alerts and the car's main changes (locked, doors, plug, charging, climate, alarm, battery level) arrive within seconds with the app closed. A quiet notification shows while it runs | Only between your own devices: over Tailscale (your own tailnet) or your home Wi-Fi. No push service (no Google Firebase) and no server of ours in between. Nothing is stored beyond the car data and alerts the app already keeps. |
| Drives recorded by the car hub (car hub mode only) | The car hub connects to the OBD adapter when the car is switched on and records the drive (distance, speed, engine and battery use; and the route with the car hub's GPS, on by default on a car hub when location "all the time" is allowed there; on other phones only with "Route with GPS" on), so every drive shows in History → Trips on your phones, whoever drove. The car hub also watches its own motion sensor (Android's "significant motion", no permission needed) and its accelerometer (how much the phone is shaken, read on the car hub only, no permission needed): moving counts as a sign the car may be on, and makes it look at the car. Only the largest movement per hour and each look go into its log | Kept on the car hub and passed to your own paired phones over the car hub's bridge (Tailscale or your home Wi-Fi). Never to us. |
| Camera (optional) | Only to scan the code of "Add another phone" (Settings → Car hub → Join a car hub), asked when you tap Scan | The picture stays on the phone and isn't kept; only the code's text is used. |
| Another person's phone (car hub mode, optional) | "Add another phone" lets someone else use the car from their own phone: you share just the car hub with their own Tailscale account in Tailscale, and give them a one-time code | The code works once, for a day, and is kept on the car hub only as a hash. Sharing is done in Tailscale under your account; they see only the car hub. |
| Car health over time (car hub mode, or a phone with its own OBD adapter) | 12 V battery voltage, drive battery health, capacity, cell voltages and temperatures, the fault codes found, with when, and when the car was last switched off — so you can see how the car is doing over months and get a warning when the 12 V battery is low or a new fault code appears | Kept on the phone that reads the adapter (at most the last 30 looks with the car on, 48 parked 12 V readings, 40 fault codes and 36 months). In car hub mode also passed to your own paired phones and your bridge at home with the shared data, and to your Home Assistant if you set that up. Never to us. |
| OBD readings through the car hub (car hub mode only, optional) | Your own phone reads the car's diagnostics (live values, fault codes, tests) through the OBD adapter connected to the car hub — or, while the car hub is connected to the car itself, it shows the car hub's live values and has the car hub read or clear the fault codes. Your phone can also list the Bluetooth devices paired on or found near the car hub (names and addresses) to choose its adapter; a PIN you give for it is kept on the car hub to pair with it. While the dashboard is open, new values are sent about once a second (more mobile data than the OBD tab otherwise uses). While your phone borrows the car hub's adapter (live tests, terminal), it sends its live readings back to the car hub every few seconds, so the car hub keeps recording the drive | Passed straight between your own two phones, the same way as the car hub's bridge, with the bridge's key. Nothing more is kept on the car hub. |
| Bluetooth devices | To connect to an OBD adapter, and to notice when your phone joins the car's Bluetooth | Stays on your phone. |
| Your electricity price area | Charging costs and smart charging | Your bridge fetches the day's prices for that area from hvakosterstrommen.no (Norway) or elprisetjustnu.se (Sweden). Only the area and the date are sent. |
| The app's own log | To find out what went wrong | Stays on your phone: kept in the app's private storage (at most about 8 MB, the oldest lines dropped first) so it survives restarts and crashes. It only leaves the phone if you share it yourself (Settings → Help and support). The car hub's log can be fetched by your own paired phone (Settings → Car hub → Get its log), over the car hub's bridge, to share it from there. |
| Automatic log upload (developer feature, **off by default**, only in the version from GitHub — the Google Play version has no log upload) | So the developer can read the logs of their own phones, Pi and car hub without sharing them by hand | Only when switched on in Diagnostics → Tools with a GitHub token: the app's log (and the Pi's and car hub's, when switched on there from the same phone) is sent, gzipped, to the private GitHub repository the token gives access to (GitHub's privacy policy applies), every 30–60 minutes and after each trip and charge. The log can hold places (GPS of drives, the weather place), car readings and device names. Each device's files are deleted from the repository after 30 days. The token is kept in the app's private storage (on the Pi in a file only the bridge can read), goes to the Pi and the car hub only over the link the app already uses with them (home Wi-Fi or Tailscale), and is never logged. Switching it off stops it and deletes what was waiting to be sent. |
| The Developer tab (only in the version from GitHub, temporary; Settings → Developer) | For the developer to confirm, on their own car, what the app knows about it: checks of the app's own log and the car's reports, the car's reports before and after something is done at the car, the phone's view of the car's Wi-Fi, a scan of the car's address for open ports, read-only OBD scans, and a recorder for the official Mitsubishi app's connection to the car | Stays on your phone. The recorder, only while you run it, uses Android's VPN (Android shows its key symbol and asks you first) for one address only, the car's own address on its Wi-Fi (192.168.8.46): the official app's connection to the car goes through PHEV Link and on to the car unchanged; nothing else on the phone goes through it, and nothing is sent anywhere else. The recording (what the app and the car sent each other, which can hold the car's VIN and Wi-Fi name) is kept in the app's private storage until you delete it or share it yourself. The port scan connects only to the car's address. The results go into the app's log, so they are sent with the automatic log upload when that is on. |
| Google Home, Apple Home, Alexa, Home Assistant (optional) | Smart-home control of the car | Runs on your bridge in your home network, between the bridge and your smart-home system. |
| Your settings, for a backup (owner's phone; not on a car hub) | So a new phone gets your PHEV Link settings back, and so you can keep a copy | Android's own backup takes one small file of the app's settings (car name and colour, departures, notification choices, prices, home location, the car's Wi-Fi name, the car's and the OBD adapter's Bluetooth addresses, this phone's name, the app language) to **your own Google account**, only when that backup is end-to-end encrypted (your phone has a screen lock), and puts it back when you set up a new phone from it (Android's backup settings decide whether it runs; Google's privacy policy applies). Settings → Backup and restore saves the same file where you choose, and restores one. **Never in it:** passwords, the bridge's and car hub's keys, the car's pairing, the link to your car hub or bridge, Tailscale, the log upload token. Never to us. |
| Your calendar (optional, PHEV Link Plus, off by default; asked when you switch on Plan → From your calendar) | A departure before each event in the calendars you pick, so the car is warm and charged when you leave | Read on your phone only: the start time, whether it is all day, the calendar and the title (only to look for the word you set). Only the departure time (and date) goes into your departures, which your other phones, your car hub and your bridge get like any departure; the event's title, place, people and notes never leave the phone and are never logged. The calendars you picked and the word are kept in the app's settings (and its backup). Never to us. |
| What the car cost month by month, as a spreadsheet (only when you ask) | So you can keep or work on your own figures | History → Trips → Month by month → Save as a spreadsheet writes a CSV file (per month: trips, km, fuel, electricity, costs) where you choose on your phone. Nothing is sent anywhere. Never to us. |
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
