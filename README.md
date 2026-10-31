# PHEV Link

> This repository holds PHEV Link's releases (download the newest APK and bridge files under **Releases**).
> PHEV Link is proprietary software, © 2026 Anwo91, all rights reserved — see `LICENSE`. The source code is not
> public. Third-party notices: `THIRD_PARTY_NOTICES.md`.

A from-scratch replacement for Mitsubishi's **Outlander PHEV Remote Ctrl** app, built for the 2014–2017 Outlander PHEV
(2018+ cars use the same protocol family and are supported by the protocol layer, but less tested).

The main goal: **a connection that just works.**

| | Official app | PHEV Link |
|---|---|---|
| Joining the car's Wi-Fi | Asks Android with an SSID *pattern* → "Connect to device?" picker every time | Exact SSID + BSSID after a one-time pairing → Android connects silently, even in the background |
| Network binding | Binds the **whole app** to the car network; never notices when it drops → stuck until restart | Binds only the car socket; follows Wi-Fi lost/available; reconnects with backoff |
| Packets split over Wi-Fi | Dropped | Reassembled; garbage skipped and logged |
| Key out of sync | Silently counts errors | Re-synchronises from the car's hint, resends, asks for a new key if needed |
| Phone already on the car Wi-Fi | — | Detected and used directly |
| When things go wrong | — | Every step logged; share the log from Diagnostics; crash reports (Firebase Crashlytics, the last log lines cleaned of places, names and addresses; off in Settings → About) |

## Features

- **Dashboard** — battery % with estimated electric range, charging state and "full at" time, plug, doors/boot/bonnet,
  lock state, lights, power switch, car clock.
- **Where the car is** — on the Car tab: a small map with the car, the saved place it is at (or "On the move"), when
  it was found and how precise, and "Open in maps" to walk or drive there. With a car hub it is the hub's own location
  (a fresh fix when the one it knows is more than 2 minutes old), asked only by your own paired phones; without one,
  where the last trip with a route ended.
- **Pull down to refresh** — on the Car tab, pull down for the car's data now: straight from the car, or the car hub
  (or bridge at home) connects to the car at once instead of waiting for its next look; when the car can't be reached
  it tries again first. On History, a pull brings the car hub's new trips and the latest charges and activity.
- **Electric range** — today's range on a full battery, corrected for the outside temperature (the car's own over
  OBD, else the forecast for home: about a fifth less at 0 °C). Full is the car's 94 % (it reports its battery in steps
  of about 6 %, and a full charge ends at 94 %), empty its 6 %, where it keeps the battery up with the engine; the range
  left is counted between the two by the battery controller's level at each of the car's steps (measured on a real car:
  the steps aren't evenly spread, and its 76 % is about 73 % of a full battery). It can also learn from your own driving (Settings →
  Charging → Electric range): this phone's GPS measures each drive — started by the car's Bluetooth or by hand — and
  pairs it with the battery level the car reported before and after it; or the OBD adapter (on this phone or the car
  phone) records each drive, and only the km driven with the engine off count, against the battery controller's level
  used for them (its 100 % down to 33 % is a full battery), so drives where the engine ran part of the way teach it
  too. Recent drives count most; the drives are listed and can be forgotten. Everything the app learns from your own
  trips, charges and fill-ups (the range, the fuel correction, petrol use, charging speed, where the car stops charging,
  the power meter) is listed under Settings → Learned values, with a reset for each that reaches every phone, the car
  hub and the bridge; History keeps every trip, charge and fill-up. With petrol: once an OBD drive has
  read the tank, the home screen also shows the total range — the tank (45 L) at what this car's engine burns
  (learned from the drives, else about 6.5 L/100 km; the petrol the engine spent charging the battery is left out, as
  that charge is driven on electricity and counts in the electric range). The forecast is always for where the car is: the car
  hub's location, or without one where a phone was while connected to the car's own Wi-Fi (rounded to about 1 km; no
  setting to type a place in). The forecast is looked up every half hour, when the app is opened and when the internet
  is back (Android holds a background app off the internet); Settings → Electric range shows when it was last updated
  and why it couldn't be. Without internet the last forecast (two days ahead, kept through restarts) is used.
- **Car picture** — the car on the home screen shows its state at a glance: open doors, boot and bonnet, lights and
  hazards, the cabin glowing warm or cool while climate runs, the lock, and the charging cable (animated while
  charging). Tap a part to see what it means and act on it — switch the lights off, start climate, charge now; hold it
  for a large view. Three styles (Settings → Appearance): from above, from the side, or a 3D model you turn with a
  finger (and tilt in the large view). What can be tapped shows as icons, as labels, or lights up when touched.
- **Climate** — heat, cool or defrost for 10/20/30 minutes; **Smart** mode picks mode and duration from the outside
  temperature (MET Norway forecast for where the car is — the car hub's location, else where a phone was on the car's Wi-Fi), with a run time you can make shorter or longer (Settings → Smart climate run time; the one-tap climate's time follows it too: 10, 20 or 30 min when the weather doesn't pick it). Handles "stopped by the car" (door opened / battery low) with a one-tap reset.
- **Activity** — who did what with the car (each phone by the name you give it, also when used right next to the
  car) and what the car did: plugged in/out, charging started/stopped and why, every 10 %, a summary per charge
  (from → to %, time, ≈ kWh), climate, locks, doors, lights, power switch, alarm, and the car's clock when it is moved
  5 minutes or more (the app sets it on every connection: "set to winter time: 03:00 → 02:01", or a clock that had
  run off); plus the bridge's departures and its own events. Kept on the bridge for every phone (and on its web page), and shown as the History tab's third tab, in
  one row with Trips and Charging; filters for charging, climate, doors & lights, people and the bridge. Tap any row for its details: the date and time to the second (each time, for a repeated row),
  which shared settings changed, whether the bridge was updated or restarted, a trip's times and battery used, how long
  the car or its Wi-Fi was out of touch, and the entry as logged.
- **Charging and trips** — the bridge keeps every charge at home with its energy from the wall (its share of a full
  battery, the car's 6 % to 94 %, times what a full charge takes: worked out from the battery capacity the car hub reads
  over OBD — about 5.7 kWh at 63 % battery health, 9.1 kWh on a new battery — or your own figure) and what it cost: the area's spot price hour by hour
  (hvakosterstrommen.no for Norway, elprisetjustnu.se for Sweden, with VAT and the Norwegian electricity support),
  Norgespris or a fixed price, plus your grid fee — with monthly totals. Trips: from leaving the bridge's reach until
  back, with the battery used (≈ electric km). Drives recorded with the OBD adapter split the battery into used,
  won back by regeneration and charged by the engine, next to the petrol burnt and the tank at the start and the end
  (% and litres; the gauge sways with the fuel while driving, so every reading is lined up with the fuel burnt after it
  and the median taken; the list shows the tank at the end). The last 7 days and the month show the averages: L/100 km
  over all the km, and kWh/100 km on electricity over the km driven with the engine off (each drive's too). What a drive
  cost uses one electricity price with charging: with a bridge, what charging at home cost this month (else the last
  7 days, else the price plan's fixed price or Norgespris); a price typed under Trips only without a bridge, or until
  home charging has a cost. Fill-ups are found by themselves (the tank 10 % or more higher at the start of a trip than
  at the end of the one before): the app asks for the litres from the pump (a notification and a "Filled up?" card in
  Trips; a fill-up can also be typed in or edited there, and its price per litre becomes the trips' fuel price). After
  two full tanks (filled until the pump clicks off, with every trip in between recorded) the trips' fuel and the
  petrol range are corrected by the pump litres against what the app counted, shown under Trips → Fill-ups with each
  tank. Shared between your phones through the bridge. Each engine start gets its likely reason
  (speed, hard acceleration, low battery, cold/heater, Charge or Save button), worked out from the readings, with a tip
  for keeping the engine off. **Month by month** (Trips) shows what the car cost: the fuel, the electricity (what
  charging at home cost where the bridge kept it, else the battery energy the trips used), the two together and per
  10 km, next to the month before; the earlier months come with Plus, and every month can be saved as a spreadsheet
  (CSV, semicolons and decimal commas where Excel expects them). On Sunday evening every phone gets the week's summary (with a car hub: the drives it
  recorded, the km driven and the km with the engine off). Settings →
  Charging, or Home → More → Charging and trips. With the demo car (Settings → Demo mode) History shows a sample month
  instead — commutes, an errand, a weekend trip that uses the battery up, a charge most nights in the cheap hours and
  tonight's smart-charging plan — made on the phone each time and never saved.
- **Smart charging** (opt-in, 2014–2017 cars, spot price) — when the car is plugged in at home, the bridge works out
  how long it needs to get full (from its own earlier charges) and moves tonight's charging to the cheapest hours
  before you need the car (the next departure, or a time you pick, with half an hour to spare). It uses the car's own
  charge timer for that one night — tonight's days get a timer slot no other day uses, with "full charge" so the car
  keeps going past the window until full — then puts your timer back (full, unplugged, morning over, or switched
  off). Your timer is read only from what the car sent this run and saved before anything is written; if a phone
  changes the timer meanwhile, that change stays. Windows follow the car's own clock. Tomorrow's prices (out around
  13:00) or an earlier departure can move the window. Every decision is in the bridge log.
  With a **charge limit** (PHEV Link Plus, 60–90 %) the car stops at the limit, and with "Fill up to full just before
  departures" it waits there — for days if the car stays plugged in — and fills to 100 % just before the next
  scheduled departure (the latest window, done half an hour early, whatever the price). Switch that off to always stop
  at the limit. A charge that ends at the limit says "charged to your limit", not "charging stopped". A battery already
  at the car's own full point (where its charges end by themselves, often 94 % on an older battery) isn't planned to
  100 %.
  **This charge** on the Home screen (while plugged in, with a bridge): *Full now* (no smart charging this time; when the
  car waits for its own timer, the bridge sends "Charge now"), *Cheapest* (smart charging as set) or *80 % now*
  (PHEV Link Plus: to the limit — 80 %, or the one in Settings — right away, waiting there, full just before the next
  departure; one charge to full when that departure is too close to wait). "Right away" is a timer window from the
  10-minute step under way, so the car charges as soon as it has it; should it still wait for its timer two minutes
  later, the bridge sends "Charge now" and gives the timer back at the window's end. It shows as one line ("This charge:
  Full now") with *Change* for the three choices. A choice is for that plug-in only (the bridge clears it when the car
  is unplugged, and every plug-in starts at *Full now*) unless **Remember for every plug-in** is ticked: *Cheapest* then
  switches smart charging on in Settings, *80 % now* does too and keeps "right away" on every plug-in. Under *80 % now*
  the Home screen shows the departure it fills up for, with **Set departure** (a one-time departure; it opens by itself
  when there is none in the next 8 days).
- **Smart home** — with the bridge, the car shows up in Home Assistant by itself (MQTT discovery): battery, charging,
  climate switch, lights, charge buttons, and events for automations (charging finished, alarm, car home or away).
  Google Home and Alexa through Home Assistant. Set up from the app; the bridge can run the MQTT broker itself.
  Or straight into **Google Home** (also Apple Home, Alexa) without Home Assistant: the bridge speaks Matter — scan
  the code in the app.
- **Car hub** (PHEV Link Plus) — a spare Android phone left in the car holds the car's Wi-Fi all the time and runs the
  bridge itself; your own phone reaches it over Tailscale from anywhere. Tailscale is built in on the car hub (the
  Tailscale app's VPN would keep the app off the car's Wi-Fi), signed in once with a link. A bridge at home (for Google
  Home and Home Assistant) is taken over by itself: it goes through the car hub instead of joining the car's Wi-Fi.
  The OBD adapter connected to the car hub can be used from your own phone; the car hub connects to it by itself
  when the car is switched on and records the drive, whoever drives. The car's own ignition report (over its Wi-Fi)
  decides; only while that isn't known do other signs count (it starts charging from the car's USB, moves, or the
  car's Wi-Fi goes), so plugging the car in to charge doesn't set it trying. The car hub also feels the car move
  (its accelerometer: someone getting in, a door shutting, the car driving off) and then looks at the car at once instead
  of at its next look every 10 minutes; a look that finds nothing new (wind, a lorry going by) makes it less sensitive for a
  while. After a door, the lock or switching off it stays in touch with the car for 10 minutes, so the car being switched
  on is seen at once and the drive is recorded from the start, and it stays in touch while the car is on, so switching
  off, locking and the lights show on your phone within seconds. Your phone shows whether the car hub is on the car's Wi-Fi
  (and since when not), can make it try again, and fetches its log to share. Departures are started once, by the car hub. When
  the car hub sits on power at 100%, it reminds you to turn on its battery protection (Samsung: Settings →
  Battery → Battery protection → Maximum, 80%), on its own screen and on your phone's car
  hub card ("Ask on car hub" opens its battery settings there). The car hub
  updates itself (or
  when your phone asks). Setting it up: on the spare phone choose "This phone stays in the car" at the start; then, on
  your own phone on the same Wi-Fi, Settings → Car hub → Add a car hub sends it the car's Wi-Fi and the rest. The
  car hub only shows its status; everything else (stay connected, updates, release) is on your own phone. Your own
  phone reaches the car hub from anywhere through Tailscale built into PHEV Link as well (sign in once on the car hub
  card; no VPN, no Tailscale app needed) — or through the Tailscale app, if you prefer. **Another person's phone**:
  Settings → Car hub → Add another phone — share the car hub with their own Tailscale account in Tailscale (they
  see nothing else of yours), then they scan the code on the first screen ("Join a car hub"); it works at home and
  away, they see the trips the car hub records, and departures and settings are shared with yours.
- **Charge timer** — set the car's own charge timer (2014–2017 cars): up to three time windows, each on the days you
  pick, plus "full charge" days; the car keeps it and waits for the window when plugged in. "Charge now" skips it once.
- **Departure timers** — "ready to leave at 07:30 on weekdays": 35 min before, the app checks the forecast and starts
  defrost/heat/cool just long enough, so the car is ready when you walk out. Mild morning? It skips and tells you.
  With a bridge, departures are shared by all your phones and the bridge starts them itself while the car is home.
  A departure is every week on the days you pick, or **once** on a day in the week ahead; a one-time departure comes
  out of the list half a day after it (the bridge and every phone take it out). **From your calendar** (Plus): a
  departure before each event in the calendars you pick (optionally only events with a word such as "car" in the
  title), 5–60 minutes before, up to 8 days ahead; moved or cancelled events move or remove it. The calendar is read
  on your phone only; the departure carries just the time, never the event's name.
- **One-tap everywhere** — home-screen widget in three sizes (battery, electric range, charging, climate, next departure, lock; preheat, flash-lights and refresh buttons; old data dimmed), quick-settings tile
  ("Preheat"), and long-press app shortcuts (preheat, stop, flash lights, refresh).
- **Swipe between tabs** — swipe left or right to move between Car, Plan, History, Health and Settings. On History and
  Health the swipe goes through their own tabs on top first (Trips, Charging, Activity; Overview to Tools), then on to
  the next tab. Things you drag sideways (the 12 V graph, the 3D car) still turn and slide as before.
- **Find my car** — flash the headlights.
- **Alerts** — charging complete (or charged to your limit), charging stopped early (power cut, loose cable; not when
  you unplug or switch the car on), cabin ready, climate stopped by the car, car left unlocked, door left open, lights
  left on, and "out of touch" when the car hub or the bridge at home hasn't answered for half an hour (told at the
  next background look, so within about an hour; with the car hub's battery level when it was last heard; only looks
  that could have reached it count, and it goes away when the car hub answers again). **Plug-in reminder**: when the
  car is switched off at home and still isn't plugged in 10 minutes later (5–30, your choice), every phone is
  reminded ("Parked at home since 13:25 with 18% battery, not plugged in"), and again in the evening (21:00 by
  default) if it still isn't; both only below the battery level you choose (80% by default).
- **Places** (PHEV Link Plus, Settings → Places, with a car hub) — save places such as work or the cabin: where the car
  parked, where your phone is, or one of the spots where the car often stops (from your trips), each with a size. The
  car hub tells you when the car arrives or leaves (a notification, switched per place) and it shows in Activity; it
  goes by its own location (the GPS of its trips, no extra battery). A place added where the car already stands
  tells nothing. With **Smart charging here too**, smart charging works at that place as at home (the cheapest
  hours, your ready-by time and limit), with the spot prices of its own price area (guessed from where it is, or
  picked); those charges show in History with the place's name.
- **Background watch** — while the car charges or preconditions, the app checks in every few minutes and alerts you.
- **Charge now** — override the charge timer (experimental).
- **Car clock** kept in sync (the car's own timers depend on it), like the official app does.
- **Diagnostics** (Settings → Diagnostics) — live log of every connection step and packet (with raw hex), raw register
  table, raw register writes for experiments, previous-session log, one-tap share.
  Every OBD reading goes into the log (what changed each second, all of them once a minute). Tools → Automatic log
  upload (developer, off by default, GitHub build only — not in the Google Play build): with a GitHub token, this phone's, the Pi's and the car hub's logs go gzipped
  to a private GitHub repository (`logs/<device>/<day>/`) every 30–60 min and after each trip and charge; 30 days kept.
- **Developer tab** (Settings → Developer, GitHub build only, temporary, English) — the tests that confirm what the
  app knows about the car from our own findings: checks of the logs and the car's reports, a recorder for the official
  app's connection to the car (Android VPN for the car's address only; the car hub steps off the car's Wi-Fi for it),
  a register watch, the car's Wi-Fi and open ports, and the read-only OBD scans (full car scan with every unit's fault
  codes, signal cross-check, charging pass). Each marks a facts list that can be shared as a report.
- **Vehicle settings** — the car's own customisable functions over Wi-Fi, like in the official app: lights (auto-cutout,
  headlights on exit, lights on remote unlock), locks (auto relock, unlock mode, auto unlock), lock/unlock beeps and horn
  answerback, wipers, turn signals, power windows and A/C behaviour; each change is confirmed by the car and saved.
  Also lists setting slots the car reports that Mitsubishi's app never names.
- **Health tab: OBD (ELM327 adapter)** — Bluetooth Classic, Bluetooth LE or Wi-Fi adapters, read-only:
  - **drive battery health** from the battery computer (BMU): charge level, capacity in Ah and % of new (38 Ah reference
    before model year 2017, 40 Ah from 2017, taken from the VIN), stored energy, pack voltage, highest/lowest cell and the
    cell difference, charge counters;
  - **dashboard** (from the OBD overview, or the Dashboard shortcut on the Home car card): speed and engine dials, a
    battery power meter (charging ↔ using; its decoding was measured on a real car and is still checked against the
    battery's stored charge while driving), the drive battery, temperatures (coolant, intake, outside, cabin,
    warmest/coldest battery module), fuel, 12 V, engine load, throttle, motor speeds and odometer. Live while an adapter
    answers (this phone's, or the car hub's lent one); otherwise the last values seen on this phone, greyed. The
    screen stays on while it is open. While it is open, speed and battery power are asked for every round (engine
    speed too while the engine runs), engine and motor speeds take turns about every 2 s, and the rest less often;
    through the car hub each new round is sent the moment it is read. Engine load and throttle read 0 while the
    engine is stopped, the fuel gauge shows the median of the last minute (the fuel sloshes), and stored energy uses
    the pack voltage at rest so it doesn't dip while accelerating. The power meter is marked "not checked yet" until a drive confirms it;
    the battery % is the battery controller's (it counts its reserve), with the car's own % beside it; the outside
    temperature shows the forecast when the car's value is far off it (the standard value reads the engine-bay air);
  - **live values** from all control units that answer (standard OBD-II plus documented Mitsubishi values, each marked
    standard / owner-documented / experimental);
  - **fault codes** from all control units with a plain-language explanation of 3,500 codes (what it means, how serious,
    can I drive, likely causes, what to check first; 122 written by hand for this car), code lookup, and clearing of
    engine/emission codes behind a confirmation;
  - **car health over time** (with a car hub, or a phone with its own adapter): every time the car is switched on
    it notes the 12 V, the drive battery's health, capacity and cells, and once a day reads the fault codes of every
    unit; parked, the car hub reads the 12 V battery at rest with the adapter alone (the car stays asleep), first 2
    hours after the car was switched off (when it counts as at rest), then every 3 hours. The parked time counts from
    the latest sign the car was on (its OBD session, the car's own report over its Wi-Fi, another phone using the lent
    adapter), and a reading while the 12 V battery is being charged (13 V or more) isn't counted as at rest. The OBD
    tab shows the 12 V at rest with when it was measured and after how long parked, whether the car has been used
    since and when the next reading comes, a graph of the last readings (tap or drag to see each one), the 12 V with
    the car on (the DC/DC converter charging it; live in the OBD screen while connected), the battery health month by
    month, a cell that keeps coming out lowest, and each fault code's history (first seen, gone, came back). A low
    12 V battery or a new or returning fault code is a notification on every phone. **Battery trends** (Plus) draw
    the drive battery's health, capacity and cell difference and the 12 V at rest month by month (tap or drag for a
    month; the 12 V with the month's lowest), and the electric range on a full battery by outside temperature in 5 °C
    steps, from your own trips with the engine off;
  - **guided live tests** you can do on the spot: full fault scan, 12 V battery, 12 V charging in READY (DC/DC), drive
    battery capacity, cell balance, EU-kontroll readiness, charging check, engine warm-up/thermostat, fuel trims,
    front/rear tyre match and a full car scan (listens to the car, maps every control unit with its part numbers and all its data, reads the standard OBD extras and finds where the car reports its buttons; continues where it left off if it stops) — each with step-by-step instructions and a pass/warning/fail verdict with explanation
    ;
  - CSV recording, adapter details, raw battery data and a terminal (non-read commands need a confirmation).
- **Help & FAQ** (Settings, or Home → More) — answers to the common questions (connecting, the bridge, departures,
  charging, smart home, privacy), searchable, and linked from where they matter ("What does this mean?" when the car
  isn't within reach). Plus 26 well-known problems of the 2014–2017 Outlander PHEV (12 V battery, EV system warnings, capacity loss,
  brake disc rust, charging, …) with why they happen and what to do, and a guide to all 24 beeps and sounds: which ones
  this app, the car's menu, a switch or only a dealer can change. With sources.
- **Backup and restore** (Settings → Backup and restore) — Android's own backup keeps your settings in your Google
  account and a new phone gets them back at its first start; or save them to a file and restore that. Passwords, keys
  and pairing are never in a backup: on a new phone you connect to the car again.
- **Languages** — English, Norsk (bokmål), Svenska, Dansk and Deutsch (Settings → Language).
- **Bridge (optional)** — a small always-on computer near the car (e.g. Raspberry Pi) keeps the link and lets the phone
  reach the car **from anywhere** through a VPN such as Tailscale. Includes a web control page and push alerts via ntfy.

## Install

1. On your phone, open the latest release of this repository (GitHub → Releases) and download `PHEV-Link-*.apk`.
2. Open it and allow installing from your browser/files app when Android asks.
3. Updates: install a newer APK over the old one; settings are kept.

**Updates:** Settings → Updates shows the app's and the bridge's version and updates both from inside the app (the app downloads the newest release from GitHub, checks it is signed with the same key, and opens Android's installer — the first time, Android asks to allow installs from PHEV Link). With **Install updates automatically** (on unless turned off), every time the app opens it checks GitHub and installs a new version right away without a tap: the app closes for a moment and a notification offers to open it again. It waits while a car command, the OBD adapter or a trip is in progress, skips roaming and Data Saver, and asks the car hub to update too. Some phones (Xiaomi) don't allow an install without a tap: after the first refusal, the phone's own install screen opens when the app opens, and one tap installs the update.

## First setup

The app walks you through it:

1. **Car Wi-Fi** — SSID (`REMOTExxxxxx`) and password from the card that came with the keys.
2. **Pair with car** (recommended, car nearby) — pick the car from Android's list once. From then on Android joins the
   car's Wi-Fi without asking, also for the widget, tile and departure timers.
3. **Connect.** If it stops at "Saying hello to the car" and then fails, this phone isn't registered with the car.
   Phones that used the official app usually are.

### Registering the phone (official procedure)

1. Sit in the car with the key and close the driver's door, power switch **OFF**.
2. Press the power switch once to **ACC** (don't press the brake).
3. Within 10 seconds press **LOCK** and **UNLOCK** on the key alternately — 10 presses in total, starting with LOCK.
4. The car beeps: 1 beep = no phones registered, 2 = one, 3 = two (memory full). It waits up to 10 minutes.
   In the app: Settings → **Register**.
5. Switch the power **OFF** to finish. The car may restart its Wi-Fi; the app reconnects by itself.

The car remembers at most 2 phones. **Memory full / start over:** close the driver's door, hazard lights **ON**, enter
registration mode (steps 2–3), then within 5 minutes press LOCK and UNLOCK alternately 20 times (starting with LOCK).
One beep = every phone deleted. This also resets the car's Wi-Fi name and password to the card and clears its timers.

Note: Android uses a per-network random MAC address, and the car registers that MAC. If you ever "forget" the car's
network in Android's Wi-Fi settings, you may need to register again.

## The bridge (control from anywhere)

The car only talks Wi-Fi within ~10–20 m. The bridge sits within that range at home, keeps in touch with the car, and
offers an HTTP API + web page. The app uses it automatically when it has fresh data about the car.

What you need: a Raspberry Pi (Zero 2 W, 3, 4, 5) or any small Linux box with Wi-Fi and NetworkManager, within range of
where the car parks, plus a second connection to your home network (Ethernet, or a USB Wi-Fi adapter) — the built-in
Wi-Fi is reserved for the car. With two Wi-Fi radios, the Bridge page shows how well each sees the car and lets you move
the car to the other one (e.g. a USB adapter with an antenna). 2.4 GHz is all the car needs; adapters with an
in-kernel driver (e.g. RTL8188FU/FTV, MT7601U, RTL8811CU/8821CU) work on current Raspberry Pi OS without installing
anything.

**Install** (once, on the Pi):

```bash
curl -sL https://github.com/Anwo91/phev-remote-plus/releases/latest/download/phev-bridge-pi.tar.gz | tar xz
sudo phev-bridge/install.sh phev-bridge/phev-bridge.jar      # also updates an existing bridge, keeping its token
```

**Everything else happens in the app** — first-run setup asks for the car's Wi-Fi card, then offers *I have a PHEV Link
bridge* (also under Settings → Bridge). It

1. finds the bridge on your home network (no addresses to type),
2. pairs with it: unplug the bridge for 5 seconds and plug it back in — for 10 minutes after power-on the bridge hands
   its token to the first app that asks, once (physical access is the proof; a web page can't do it behind your back),
3. connects the bridge's Wi-Fi to the car (the car's Wi-Fi name and password come from the app; the profile pins the
   Pi's real MAC address, keeps your internet on the home connection and never gives up re-joining when the car returns),
4. walks you through the car's registration mode, asking how many times the car beeped (3 = memory full),
5. checks that the bridge reaches the car, and switches the app to use it at home.

While setup is open the app stays off the car's Wi-Fi itself: joining it takes most phones off the home network (where
the bridge is), and the car's Wi-Fi seems to take only one device at a time. For the same reason the app goes through
the bridge whenever the bridge is on the car's Wi-Fi. To register a phone while the bridge holds the car's Wi-Fi,
Settings → Phone registration has **Let the bridge step aside (10 min)**: the bridge leaves the car's Wi-Fi and rejoins
by itself afterwards (or after a reboot).

The bridge watches its own link to the car's Wi-Fi: NetworkManager gives up on a network after failed joins (for
example while a phone held the car's Wi-Fi), so when the bridge finds itself off the car's Wi-Fi it asks NetworkManager
to join again — after 1, 2, 4, 8 and then every 10 minutes, and at once when the car's network comes back in sight. A
phone that is about to use the car's Wi-Fi itself first asks the bridge to rejoin. While a phone is on the same
network as the bridge it never joins the car's Wi-Fi by itself (only Settings → Phone registration's step-aside does);
away from home it may, e.g. next to the car at work. When the car keeps refusing a phone a
session (the phone isn't registered), that phone leaves the car's Wi-Fi to the bridge for an hour (longer if it happens
again) while the car is near the bridge. Losing and getting back the car's Wi-Fi shows in the activity log.

**Another phone:** on a phone that already uses the bridge, Settings → Bridge → **Add another phone** opens pairing
for one more phone for 10 minutes (no unplugging). On the new phone: install PHEV Link, enter the car's Wi-Fi card,
choose *I have a PHEV Link bridge* — it pairs, sees the bridge already talks to the car and finishes without another
registration. Both phones then use the car through the bridge.

The car remembers at most two devices — the bridge takes one. If it answers the registration sequence with three beeps,
setup walks you through clearing its memory (one beep confirms it; the car's Wi-Fi then restarts with the name and
password from the card), waits until the bridge is back on the car's Wi-Fi, and only then asks for registration mode
again.

**Settings → Bridge** (status, log and updates) shows what the bridge is doing — its car Wi-Fi and car connection, last
car data, the computer (model, temperature, memory, disk) and the bridge's own log, live — and lets you connect it to
the car, restart it, restart the computer and **update it from the app**: the bridge downloads the newest release from
GitHub, checks its SHA-256 and reinstalls itself (bridges installed before 1.0.38 need the install command above once
more; after that, updates happen in the app).

**Alerts from the bridge, and the phone's background look:** the bridge watches the car around the clock (charge
complete or stopped, left unlocked, door or lights left on, security alarm, scheduled charge didn't start) and reminds
you to plug in when the car is home, low and not plugged in: 10 minutes after it was switched off at home, and at 21:00
(the minutes, the time and the level are shared by all phones, Settings → Notifications → Plug-in reminder). With the app closed, the phone reads the bridge's saved status once an hour — every few minutes
while the car charges or preheats, and just after the reminder time — which never wakes the car and takes about a
second on home Wi-Fi. It shows the bridge's new alerts (each once) and keeps the widget fresh. While the app is open
they arrive at once over the live stream. **Instant alerts** (Settings → Notifications, on by default) keep that within
seconds with the app closed too, with nothing kept running on the phone and no lasting notification: the car hub (or
bridge) sends each alert the moment it raises it, as a push through Google's Firebase Cloud Messaging and PHEV Link's
own small push relay (a Cloudflare Worker that holds Firebase's sending key). The alert is encrypted on the car hub
with a key only your paired phones have, so neither the relay, Cloudflare nor Google can read it; it carries the alerts
of the last half hour and the car's main state (battery, plug, lock, doors, lights, climate) for the alert's words.
The phone shows each alert once, by the same at-home/away choices, and the background look stays as the backup.
"Send a test alert" there tries the whole way and says how long it took. Some phones (Xiaomi and others) hold pushes
back while the app is closed, so Settings offers to let PHEV Link run without battery optimisation and says when to
allow Autostart. For the fastest alerts, also turn on "Stay connected to the car" on the car hub. Each phone chooses per kind whether it wants it while the car is **at home**
and while it is **away** (Settings → Notifications), e.g. "left unlocked" at work but not every night in the drive: the
bridge at home sees the car only at home, and the car hub tells from the home Wi-Fi. When it isn't known where the
car is, a tick in either column is enough. Reminders, the weekly summary, car health and "out of touch" have one switch.

**When the car isn't within reach** the app says so plainly at the top of the Car page — "Car not within reach · not
seen since 14:32" (the bridge hasn't seen the car's Wi-Fi: it may be away, or just out of the bridge's reach), "Car not
nearby" (this phone searched for the car's Wi-Fi), "Bridge not answering" or "Can't reach the bridge at home" — in grey
rather than as an error, with the last known values marked "As of 14:32". Commands then answer within seconds with the
reason instead of spinning, and a departure that can't start for that reason gets one quiet notification instead of a
"failed, try again".

**Every screen names the devices the same way.** With a car hub, Settings says "Through the car hub" and "Car hub only",
and a Pi kept next to it is "Bridge at home" everywhere (Settings → Updates, its row and page, the update card on Home).
When one doesn't answer, the app says why from what it knows: a home-network address only works on the home Wi-Fi, and
for a Tailscale address it shows Tailscale's own "offline" (a Pi without power or network) instead of telling you to
join your home Wi-Fi. While connected, the connection sheet offers only Diagnostics; "Why can't it connect?" and
"Try again now" appear when it isn't. When the car hub doesn't answer, "Why can't it connect?" opens its own question in
Help & FAQ.

Every step is logged in the app (Settings → Diagnostics) and in the bridge log (`journalctl -u phev-bridge -f`). The bridge also keeps
its log in `/var/log/phev-bridge/bridge.log` (at most 4 MB, the previous file as `bridge.log.1`), and every start says
how the last run ended — normally, the computer restarted (power cut), or the program was stopped abruptly (with the
number of programs the system stopped for lack of memory) — plus how much memory is free; it warns when memory runs low.

**Shared by all phones:** the bridge keeps the departures, the car's name, full range, the quick-climate defaults and
the weather place, so every phone paired with it sees the same (changed on one phone → the others take it over; a
change made on two phones at once: the bridge's wins). While the car is home (the bridge is on its Wi-Fi) the bridge
runs the departures itself — weather check 35 min before, climate on in time — even with every phone switched off; the
phones get the usual "started" notification. When the car is away, a phone near it does the job, as before.
Language, alerts and the connection itself stay per phone.

<details><summary>Doing it by hand instead</summary>

```bash
# Join the car's Wi-Fi on wlan0: pinned MAC, not the default route, keeps re-trying when the car is away
sudo nmcli connection add type wifi ifname wlan0 con-name phev-car ssid REMOTExxxxxx \
  wifi-sec.key-mgmt wpa-psk wifi-sec.psk 'xxxxxxxx' ipv4.never-default yes ipv6.method disabled \
  connection.autoconnect yes connection.autoconnect-retries 0 \
  802-11-wireless.cloned-mac-address permanent 802-11-wireless.powersave 2
sudo nmcli connection up phev-car
# Car in registration mode (see above), then:
curl -X POST -H "Authorization: Bearer <token>" -d '{"action":"register"}' http://localhost:8099/api/command
```
If the register call says it couldn't reach the car in time, send it again — the car listens for 10 minutes.
</details>

**Remote access:** install Tailscale on the Pi and the phone (https://tailscale.com/download), then use the Pi's
Tailscale address as the bridge address in Settings.

Try it before going near the car: set `emulator=true` in `/etc/phev-bridge.properties` to run against a simulated car.

Push alerts without the phone being near the car: create a private topic on [ntfy](https://ntfy.sh), install the ntfy app,
and set `ntfy.url=https://ntfy.sh/<your-topic>` in the bridge config.

**Modes:** `interval` (default) connects every 10 minutes for a status check and on demand for commands, and stays
connected for 10 minutes after a door opens or shuts, the car is locked or unlocked, or switched off (someone is at the
car), and while the car is on (given up after 3 minutes without an answer: the car drove off); `persistent` stays
connected all the time. Interval is gentler on the car's 12 V battery — a parked car whose Wi-Fi
module never rests may drain it over long periods.

**Security:** never expose port 8099 to the internet directly — use Tailscale/WireGuard. Anyone with the token can
control the car's climate and lights (not locks: the protocol has no remote unlock).

### Google Home, Apple Home, Alexa (Matter)

The bridge can show up in Google Home as a Matter bridge — no Home Assistant, no cloud: a small Node.js program
(`bridge/matter`, matter.js) runs next to the bridge when switched on in the app (**Settings → Smart home → Google Home → Set
up**). It shares the car's climate (on/off), and optionally the headlights and parking lights; while the car is away
they show as offline. Commands run like a phone's and appear in the activity log as "Google Home" (or Apple Home,
Alexa…). Climate on uses the shared one-tap choice (and the weather when smart).

Google Home only adds uncertified Matter devices that are registered in its developer console under the same Google
account, once: console.home.google.com → create a project → Add Matter integration → product name PHEV Link → Vendor ID
`0xFFF1`, Product ID `0x8000` → Save. Then Google Home app → Devices → Add → Matter-enabled device → scan the code the
app shows. A Google speaker, display or TV box at home is needed (Matter's hub). Apple Home and Alexa can be added
afterwards from Google Home (Linked Matter apps & services). The bridge installer sets up Node.js (18+) when missing;
Matter needs IPv6 on the bridge's home network (on by default).

### Smart home (Home Assistant)

The bridge can put the car into Home Assistant over MQTT, with Home Assistant's discovery: no YAML, the car just shows
up as a device. Set it up in the app: **Settings → Smart home → Home Assistant → Set up**. The bridge looks for MQTT brokers on its
network (and marks which one runs Home Assistant); pick one and enter a user (with Home Assistant's Mosquitto add-on:
a Home Assistant user made for the car). No broker? **Run a broker on the bridge** installs Mosquitto on the bridge
computer with a generated password, and the app shows what to type into Home Assistant's MQTT integration.

What the car brings into Home Assistant (names in the app's language):

| Kind | Entities |
|---|---|
| Sensors | battery %, electric range (from the range the car hub learned from its drives, else the shared full range), time to full, full at, charge timer, climate ends, last update, energy charged (kWh, for the Energy dashboard) |
| Diagnostic sensors | 12 V battery (now and at rest, and its state), drive battery health and capacity, cell spread, fault codes (the codes as an attribute), health checked (filled in by the car hub's OBD adapter) |
| Binary sensors | charging, charging cable, lock, doors (each door as an attribute), theft alarm, switched on, at home, bridge link |
| Switches | climate (mode from the shared one-tap choice, time from the smart climate run time, or the weather when smart), headlights, parking lights |
| Selects | climate mode, climate time (10, 20 or 30 min = the smart climate run time shorter, normal or longer, shared with the phones) |
| Buttons | charge now, charge on timer, flash lights, refresh |
| Event | "Car event": charge_started, charge_finished, charge_stopped, plugged, unplugged, climate_on/off/cancelled, alarm, locked, unlocked, door_open, doors_closed, car_home, car_away, departure_started/failed, left_unlocked, door_left_open, lights_left_on, command_failed, car_health, bridge_offline, … |

Commands from Home Assistant run like a phone's and appear in the activity log as "Home Assistant". Retained command
messages are never acted on (they would replay on every reconnect). Google Home and Alexa reach the car through Home
Assistant (expose e.g. the Climate switch). Other systems can use the topics directly:

```
phevlink/<id>/availability   online / offline (the bridge's last will)
phevlink/<id>/state          everything as one JSON object (retained)
phevlink/<id>/event          {"event_type": "charge_finished", "text": "…", …}
phevlink/<id>/activity       every activity line as JSON
phevlink/<id>/cmd/<what>     climate ON|OFF, climate_mode, climate_minutes, headlights ON|OFF, parking_lights ON|OFF,
                             locate, charge_now, charge_timer, refresh (PRESS)
```

Turning the link off takes the car out of Home Assistant again. Keep the broker on your home network with a login;
like the bridge's own port, never open it to the internet.

### Bridge API

| Method | Path | |
|---|---|---|
| GET | `/` | Control page (asks for the token once) |
| GET | `/api/status` | `{link, car, reach, bridge}` JSON; `reach.state`: `in_reach`, `checking`, `not_seen` (off the car's Wi-Fi ≥ 90 s and not in a real scan), `stepped_aside`, `unknown`, with `lastSeenMs`/`checkedAtMs` |
| GET | `/api/events` | Server-sent events, a `status` event per change |
| POST | `/api/command` | `{"action":"climate","mode":"HEAT","minutes":20}` · `climate_off` · `headlights`/`parking_lights` + `on` · `locate` · `refresh` · `charge_now` · `ack_climate` · `register` · `unregister` · `sync_clock` · `raw` (+ `register`, `data` hex) |
| POST | `/api/connect`, `/api/disconnect` | |
| GET | `/api/log?n=300` | Diagnostic log; `&file=1` reads the log file, earlier runs included (the app's Share sends 1500 lines of it) |
| GET/POST | `/api/logupload` | Automatic log upload (developer, off by default): on/off, last upload, files waiting; POST `{"enabled":true,"token":"…","now":true}` (the token is never answered back) |
| GET | `/api/hello` | No token: identity for discovery + whether pairing is open |
| POST | `/api/pair` | No token, header `X-PHEV-Pair: 1`: the token, once per boot, within 10 min of power-on |
| GET | `/api/carwifi` | The bridge's own link to the car's Wi-Fi (interface, connected, IP, MAC, signal) |
| POST | `/api/carwifi/scan`, `/api/carwifi/join`, `/api/carwifi/forget` | `{"country":"NO"}` · `{"ssid":…,"password":…}` |
| POST | `/api/pair/open` | `{"minutes":10}` — "Add another phone": pairing open for one more phone (token needed) |
| POST | `/api/carwifi/pause`, `/api/carwifi/resume` | `{"minutes":10}` — leave the car's Wi-Fi so a phone can use it (comes back by itself) |
| GET/POST | `/api/mqtt` | smart-home link settings and status / save and (re)connect: `{"enabled":true,"host":"…","port":1883,"username":"…","password":"…","language":"nb"}` |
| POST | `/api/mqtt/find`, `/api/mqtt/local` | MQTT brokers on the home network / set up Mosquitto on the bridge computer and use it |
| GET/POST | `/api/matter` | Google Home / Matter: status, pairing code (QR rows, manual code), paired apps / `{"enabled":true,"devices":{"climate":true,"headlights":false,"parking":false}}` |
| POST | `/api/matter/reset` | remove every Matter pairing; a new code |
| POST | `/api/carwifi/rejoin` | off the car's Wi-Fi and not paused: join it now; `connected`, `tried`, `inSight`, `signal` (takes up to ~35 s) |
| POST | `/api/config` | `{"emulator":false}` — saved, then the bridge restarts |
| GET | `/api/system` | The computer: model, OS, uptime, temperature, memory, disk, addresses, bridge build |
| GET | `/api/update` | Installed and newest version, whether an update runs, its log (`?check=1` asks GitHub now) |
| POST | `/api/update` | Update now: download the newest release, check its SHA-256, reinstall, restart |
| POST | `/api/restart`, `/api/reboot` | Restart the bridge program · restart the computer |
| GET | `/api/shared` | What all phones share (departures, car name, range, climate defaults, weather place, time zone) + `rev` and `lastDeparture` |
| POST | `/api/shared` | `{"departures":[…],"carName":"…"}` — saves only the keys sent |
| GET | `/api/activity` | `?limit=200&before=<ms>` — who did what and what the car did, newest first, with an English `text` |
| POST | `/api/activity` | `{"entries":[…]}` — a phone reports what it did straight over the car's Wi-Fi (`cmd.…` only) |
| GET | `/api/carwifi/radios` | The computer's Wi-Fi radios: built in or USB, driver, which one talks to the car / carries the home network, how well each sees the car |
| POST | `/api/carwifi/radio` | `{"iface":"wlan1"}` — move the car's Wi-Fi to that radio (never the home one), save `car.iface`, restart |
| GET | `/api/history` | `?limit=300` — charges at home (`fromPct`, `toPct`, `kwh` from the wall, `cost`, `perKwh`, `currency`, `area`) and trips (`leftMs`, `backMs`, battery before/after), newest first; `week` and `month` summaries; the price `plan` |

Auth (all but hello/pair): `Authorization: Bearer <token>` or `?token=<token>`.

## OBD adapter

Any ELM327-compatible adapter works for standard data and fault codes. The drive-battery data needs an adapter that
supports custom CAN flow control (`ATFCSH`); many cheap "v1.5/v2.1" clones don't — the app tells you if yours doesn't.
Known good: OBDLink LX / MX+ / CX, Vgate iCar 2 / iCar Pro, Konnwei KW902, Veepeak. Pair classic Bluetooth adapters in
the phone's Bluetooth settings first; Bluetooth LE adapters are found with a scan in the app. The OBD socket is under the
steering wheel. Unplug the adapter when the car stands for days (it draws from the 12 V battery).

With a car hub (above), choose the adapter once on the car hub's status screen, or from your own phone ("Choose its
adapter" on the car hub card: the Bluetooth devices paired on the car hub, the adapters your phone has used, or
what "Search near the car hub" finds). Nothing has to be done at the car hub: it pairs with a new adapter by itself,
entering the PIN given on your phone (or 1234, 0000 and 6789 in turn). A car hub without an adapter takes the one
your phone used by itself. Adapters asking for a pairing confirmation instead of a PIN still need one tap on the car
phone (Android allows no app to answer that). Your own phone then uses "The adapter in
the car" (through the car hub) by itself. While the car hub is connected to the car (the car on), the OBD tab and the dashboard show the
car hub's own connection live, and "Scan codes" and "Clear codes" run on the car hub, so it keeps recording the
drive. When it isn't connected (the car off), Connect borrows the adapter through the car hub — readings, fault codes,
clearing and tests, a little slower than an adapter of its own. One phone at a time; the car hub's own OBD session
steps aside while it is lent, and takes the adapter back when the car is switched on (borrowed before that) to record
the drive. "Borrow the adapter" borrows it while the car is on, for live tests and the terminal: your phone sends its
readings back to the car hub, which keeps recording the drive as one trip (a live test pauses the speed readings, so
run it parked). The shared view stops after 5 minutes with the app in the background.
The car hub connects to the adapter by itself when the car is switched on and records the drive. If it can't (no
adapter chosen, Bluetooth off or not allowed for PHEV Link), it shows a notification, its status screen has an "Allow
Bluetooth" button, and your own phone's car hub card says so too.
Your own phone's car hub card also lists every permission and setting the car hub depends on (Bluetooth, nearby
Wi-Fi, location, notifications, battery, background use and data, alarms, installing updates, the motion sensor), each
OK or with what to do on the car hub.
The card also shows the car hub's own OBD connection: connected (recording a drive since when), or when the car last
answered and how the last automatic try went.

The OBD side never writes to the car except "Clear codes" (OBD-II mode 04), which asks first. The terminal asks before
sending anything that isn't a read request.

## Troubleshooting

- **"Looking for the car…" forever** — out of range, or the car's Wi-Fi is asleep. After several days parked it sleeps;
  using the key (lock/unlock) usually wakes it.
- **"Can't join car Wi-Fi"** — wrong password, or you declined Android's prompt. Fix the password and tap the status
  chip → *Try again now*.
- **Stops at "Saying hello"** — phone not registered (see above).
- **Drops when the car is switched on** — the car turns its Wi-Fi off a little while after the power switch goes ON
  (2014–2017 cars always; 2018+ unless *Keep the car's Wi-Fi on* is set in Settings → Staying connected). ACC is fine.
- **Heat/cool does nothing** — remote climate only runs with the power switch OFF and not while quick charging; the car
  also refuses with a door open, a low or very cold drive battery. The app now refuses up front when the car reports
  ACC/ON, and reports a failure (with the car's state in the log) if the car acks but never starts.
- **OBD tool connects but never answers** — scan tools with their own app (Autel "Maxi…", Launch, ThinkCar…) don't
  speak ELM327 and can't be used; use an ELM327-compatible adapter (see above).
- **Anything else** — Settings → Help & FAQ, or Settings → Diagnostics → Share. The log shows each step with timestamps and raw packets.

## Credits

- Thanks to the public protocol research of [phev2mqtt](https://github.com/buxtronix/phev2mqtt) (Ben Buxton and
  contributors; the key schedule was worked out by @zivillian) and [phev-remote](https://github.com/phev-remote)
  (Jamie Nuttall). PHEV Link's protocol code is its own implementation and contains no code from those projects.
- Weather: [MET Norway](https://api.met.no/) Locationforecast (CC BY 4.0). Maps: © OpenStreetMap contributors.
- Drive-battery values: owners on myoutlanderphev.com. Thanks to [OBDb](https://github.com/OBDb), whose
  Outlander signal set first showed where the engine, cluster and climate values live (the app's own table
  replaced it after 1.0.98). Tips research sources are listed with each tip. See
  [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

Not affiliated with or endorsed by Mitsubishi Motors. Use at your own risk.

## License

Proprietary: © 2026 Anwo91, all rights reserved. See `LICENSE`. The published builds may be installed and used for
your own, non-commercial use. Versions up to and including 1.0.91 were released under GPL-3.0-or-later; copies of
those stay under that licence.
