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
| When things go wrong | — | Every step logged; share the log from Diagnostics |

## Features

- **Dashboard** — battery % with estimated electric range, charging state and "full at" time, plug, doors/boot/bonnet,
  lock state, lights, power switch, car clock.
- **Electric range** — today's range on a full battery, corrected for the outside temperature (the car's own over
  OBD, else the forecast for home: about a fifth less at 0 °C). It can also learn from your own driving (Settings →
  Charging → Electric range): this phone's GPS measures each drive — started by the car's Bluetooth or by hand — or
  the OBD adapter reads the odometer, and each drive is paired with the battery level the car reported before and
  after it. Recent drives count most; the drives are listed and can be forgotten. With petrol: once an OBD drive has
  read the tank, the home screen also shows the total range — the tank (45 L) at what this car's engine burns
  (learned from the drives, else about 6.5 L/100 km). With a car phone, the forecast is for where the car is.
- **Car picture** — the car on the home screen shows its state at a glance: open doors, boot and bonnet, lights and
  hazards, the cabin glowing warm or cool while climate runs, the lock, and the charging cable (animated while
  charging). Tap a part to see what it means and act on it — switch the lights off, start climate, charge now; hold it
  for a large view. Three styles (Settings → Car picture): from above, from the side, or a 3D model you turn with a
  finger (and tilt in the large view). What can be tapped shows as icons, as labels, or lights up when touched.
- **Climate** — heat, cool or defrost for 10/20/30 minutes; **Smart** mode picks mode and duration from the outside
  temperature (MET Norway forecast for where the car is — the car phone's location, else the saved place), with a run time you can make shorter or longer (Settings → Smart climate run time). Handles "stopped by the car" (door opened / battery low) with a one-tap reset.
- **Activity** — who did what with the car (each phone by the name you give it, also when used right next to the
  car) and what the car did: plugged in/out, charging started/stopped and why, every 10 %, a summary per charge
  (from → to %, time, ≈ kWh), climate, locks, doors, lights, power switch, alarm; plus the bridge's departures and its
  own events. Kept on the bridge for every phone (and on its web page); filters for charging, climate, doors & lights,
  people and the bridge.
- **Charging and trips** — the bridge keeps every charge at home with its energy from the wall (from the battery levels;
  a full charge ≈ 12 kWh on 2014–2018 cars, adjustable) and what it cost: the area's spot price hour by hour
  (hvakosterstrommen.no for Norway, elprisetjustnu.se for Sweden, with VAT and the Norwegian electricity support),
  Norgespris or a fixed price, plus your grid fee — with monthly totals. Trips: from leaving the bridge's reach until
  back, with the battery used (≈ electric km). On Sunday evening every phone gets the week's summary. Settings →
  Charging, or Home → More → Charging and trips.
- **Smart charging** (opt-in, 2014–2017 cars, spot price) — when the car is plugged in at home, the bridge works out
  how long it needs to get full (from its own earlier charges) and moves tonight's charging to the cheapest hours
  before you need the car (the next departure, or a time you pick, with half an hour to spare). It uses the car's own
  charge timer for that one night — tonight's days get a timer slot no other day uses, with "full charge" so the car
  keeps going past the window until full — then puts your timer back (full, unplugged, morning over, or switched
  off). Your timer is read only from what the car sent this run and saved before anything is written; if a phone
  changes the timer meanwhile, that change stays. Windows follow the car's own clock. Tomorrow's prices (out around
  13:00) or an earlier departure can move the window. Every decision is in the bridge log.
- **Smart home** — with the bridge, the car shows up in Home Assistant by itself (MQTT discovery): battery, charging,
  climate switch, lights, charge buttons, and events for automations (charging finished, alarm, car home or away).
  Google Home and Alexa through Home Assistant. Set up from the app; the bridge can run the MQTT broker itself.
  Or straight into **Google Home** (also Apple Home, Alexa) without Home Assistant: the bridge speaks Matter — scan
  the code in the app.
- **Car phone** (PHEV Link Plus) — a spare Android phone left in the car holds the car's Wi-Fi all the time and runs the
  bridge itself; your own phone reaches it over Tailscale from anywhere. Tailscale is built in on the car phone (the
  Tailscale app's VPN would keep the app off the car's Wi-Fi), signed in once with a link. A bridge at home (for Google
  Home and Home Assistant) is taken over by itself: it goes through the car phone instead of joining the car's Wi-Fi.
  The OBD adapter connected to the car phone can be used from your own phone; the car phone connects to it by itself
  when the car is switched on (it starts charging from the car's USB, the car's Wi-Fi goes, or the car reports the
  ignition on) and records the drive, whoever drives. Your phone shows whether the car phone is on the car's Wi-Fi
  (and since when not), can make it try again, and fetches its log to share. Departures are started once, by the car phone. The car phone
  updates itself (or
  when your phone asks). Setting it up: on the spare phone choose "This phone stays in the car" at the start; then, on
  your own phone on the same Wi-Fi, Settings → Car phone → Add a car phone sends it the car's Wi-Fi and the rest. The
  car phone only shows its status; everything else (stay connected, updates, release) is on your own phone. Your own
  phone reaches the car phone from anywhere through Tailscale built into PHEV Link as well (sign in once on the car phone
  card; no VPN, no Tailscale app needed) — or through the Tailscale app, if you prefer. **Another person's phone**:
  Settings → Car phone → Add another phone — share the car phone with their own Tailscale account in Tailscale (they
  see nothing else of yours), then they scan the code on the first screen ("Join a car phone"); it works at home and
  away, they see the trips the car phone records, and departures and settings are shared with yours.
- **Charge timer** — set the car's own charge timer (2014–2017 cars): up to three time windows, each on the days you
  pick, plus "full charge" days; the car keeps it and waits for the window when plugged in. "Charge now" skips it once.
- **Departure timers** — "ready to leave at 07:30 on weekdays": 35 min before, the app checks the forecast and starts
  defrost/heat/cool just long enough, so the car is ready when you walk out. Mild morning? It skips and tells you.
  With a bridge, departures are shared by all your phones and the bridge starts them itself while the car is home.
- **One-tap everywhere** — home-screen widget in three sizes (battery, electric range, charging, climate, next departure, lock; preheat, flash-lights and refresh buttons; old data dimmed), quick-settings tile
  ("Preheat"), and long-press app shortcuts (preheat, stop, flash lights, refresh).
- **Find my car** — flash the headlights.
- **Alerts** — charging complete, charging stopped early (power cut, loose cable), cabin ready, climate stopped by the car,
  car left unlocked, door left open, lights left on.
- **Background watch** — while the car charges or preconditions, the app checks in every few minutes and alerts you.
- **Charge now** — override the charge timer (experimental).
- **Car clock** kept in sync (the car's own timers depend on it), like the official app does.
- **Diagnostics** (Settings → Diagnostics) — live log of every connection step and packet (with raw hex), raw register
  table, raw register writes for experiments, previous-session log, one-tap share.
  Every OBD reading goes into the log (what changed each second, all of them once a minute). Tools → Automatic log
  upload (developer, off by default): with a GitHub token, this phone's, the Pi's and the car phone's logs go gzipped
  to a private GitHub repository (`logs/<device>/<day>/`) every 30–60 min and after each trip and charge; 30 days kept.
- **Vehicle settings** — the car's own customisable functions over Wi-Fi, like in the official app: lights (auto-cutout,
  headlights on exit, lights on remote unlock), locks (auto relock, unlock mode, auto unlock), lock/unlock beeps and horn
  answerback, wipers, turn signals, power windows and A/C behaviour; each change is confirmed by the car and saved.
  Also lists setting slots the car reports that Mitsubishi's app never names.
- **Health tab: OBD (ELM327 adapter)** — Bluetooth Classic, Bluetooth LE or Wi-Fi adapters, read-only:
  - **drive battery health** from the battery computer (BMU): charge level, capacity in Ah and % of new (38 Ah reference
    before model year 2017, 40 Ah from 2017, taken from the VIN), stored energy, pack voltage, highest/lowest cell and the
    cell difference, charge counters;
  - **live values** from all control units that answer (standard OBD-II plus documented Mitsubishi values, each marked
    standard / owner-documented / experimental);
  - **fault codes** from all control units with a plain-language explanation of 3,500 codes (what it means, how serious,
    can I drive, likely causes, what to check first; 122 written by hand for this car), code lookup, and clearing of
    engine/emission codes behind a confirmation;
  - **car health over time** (with a car phone, or a phone with its own adapter): every time the car is switched on
    it notes the 12 V, the drive battery's health, capacity and cells, and once a day reads the fault codes of every
    unit; parked, the car phone reads the 12 V battery at rest every 12 hours with the adapter alone (the car stays
    asleep). The OBD tab shows the 12 V at rest with its last readings, the battery health month by month, a cell that
    keeps coming out lowest, and each fault code's history (first seen, gone, came back). A low 12 V battery or a new
    or returning fault code is a notification on every phone;
  - **guided live tests** you can do on the spot: full fault scan, 12 V battery, 12 V charging in READY (DC/DC), drive
    battery capacity, cell balance, EU-kontroll readiness, charging check, engine warm-up/thermostat, fuel trims and
    front/rear tyre match — each with step-by-step instructions and a pass/warning/fail verdict with explanation
    ;
  - CSV recording, adapter details, raw battery data and a terminal (non-read commands need a confirmation).
- **Help & FAQ** (Settings, or Home → More) — answers to the common questions (connecting, the bridge, departures,
  charging, smart home, privacy), searchable, and linked from where they matter ("What does this mean?" when the car
  isn't within reach). Plus 26 well-known problems of the 2014–2017 Outlander PHEV (12 V battery, EV system warnings, capacity loss,
  brake disc rust, charging, …) with why they happen and what to do, and a guide to all 24 beeps and sounds: which ones
  this app, the car's menu, a switch or only a dealer can change. With sources.
- **Languages** — English, Norsk (bokmål), Svenska, Dansk and Deutsch (Settings → Language).
- **Bridge (optional)** — a small always-on computer near the car (e.g. Raspberry Pi) keeps the link and lets the phone
  reach the car **from anywhere** through a VPN such as Tailscale. Includes a web control page and push alerts via ntfy.

## Install

1. On your phone, open the latest release of this repository (GitHub → Releases) and download `PHEV-Link-*.apk`.
2. Open it and allow installing from your browser/files app when Android asks.
3. Updates: install a newer APK over the old one; settings are kept.

**Updates:** Settings → Updates shows the app's and the bridge's version and updates both from inside the app (the app downloads the newest release from GitHub, checks it is signed with the same key, and opens Android's installer — the first time, Android asks to allow installs from PHEV Link).

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
complete or stopped, left unlocked, door or lights left on, security alarm, scheduled charge didn't start) and, at 21:00
by default, reminds you to plug in when the car is home, low and not plugged in (time and level shared by all phones,
Settings → Alerts). With the app closed, the phone reads the bridge's saved status once an hour — every few minutes
while the car charges or preheats, and just after the reminder time — which never wakes the car and takes about a
second on home Wi-Fi. It shows the bridge's new alerts (each once) and keeps the widget fresh. While the app is open
they arrive at once over the live stream. "Alerts from the bridge" can be turned off per phone (then the phone checks
for alerts itself while connected, as before).

**When the car isn't within reach** the app says so plainly at the top of the Car page — "Car not within reach · not
seen since 14:32" (the bridge hasn't seen the car's Wi-Fi: it may be away, or just out of the bridge's reach), "Car not
nearby" (this phone searched for the car's Wi-Fi), "Bridge not answering" or "Can't reach the bridge at home" — in grey
rather than as an error, with the last known values marked "As of 14:32". Commands then answer within seconds with the
reason instead of spinning, and a departure that can't start for that reason gets one quiet notification instead of a
"failed, try again".

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

**Modes:** `interval` (default) connects every 10 minutes for a status check and on demand for commands;
`persistent` stays connected all the time. Interval is gentler on the car's 12 V battery — a parked car whose Wi-Fi
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
| Sensors | battery %, electric range (from the shared full range), time to full, full at, charge timer, climate ends, last update, energy charged (kWh, for the Energy dashboard) |
| Diagnostic sensors | 12 V battery (now and at rest, and its state), drive battery health and capacity, cell spread, fault codes (the codes as an attribute), health checked (filled in by the car phone's OBD adapter) |
| Binary sensors | charging, charging cable, lock, doors (each door as an attribute), theft alarm, switched on, at home, bridge link |
| Switches | climate (mode and time from the shared one-tap choice, or the weather when smart), headlights, parking lights |
| Selects | climate mode, climate time (shared with the phones' one-tap climate) |
| Buttons | charge now, charge on timer, flash lights, refresh |
| Event | "Car event": charge_started, charge_finished, charge_stopped, plugged, unplugged, climate_on/off/cancelled, alarm, locked, unlocked, door_open, doors_closed, car_home, car_away, departure_started/failed, left_unlocked, door_left_open, lights_left_on, command_failed, car_health, … |

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

With a car phone (above), choose the adapter once on the car phone's status screen; your own phone then picks
"In the car" in its adapter list and uses it from anywhere — readings, fault codes, clearing and tests, a little slower
than an adapter of its own. One phone at a time; the car phone's own OBD session steps aside while it is lent.

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
