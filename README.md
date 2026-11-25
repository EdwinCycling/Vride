# Golden Hour Ride

A proof-of-concept indoor cycling ride that runs in the browser (Three.js, one `index.html`).

Live: https://golden-hour-ride.space.fast/ · Leckhampton loop: https://golden-hour-ride.space.fast/?course=leckhampton

## What's in it

- Two courses: an invented mountain valley, and a real 11.9 km Leckhampton loop built from Environment Agency lidar, dressed as the 19th-century hill (sheep, hay stooks, quarry smoke).
- Four looks: Golden, Overcast, Rain and Mist. Buttons at the top, keys 1–4, or by voice.
- Skip forward and back 250 m with the arrow keys.
- A launch screen for route, trainer, power, hill and weather setup. Imported GPX tracks get an OSM map, four map layers, route statistics and an elevation profile.
- A procedural Live Route Lab with flat, rolling, short-climb, wall, 3/5/7/9% climb and matching descent profiles. The profile can be changed from Settings during the ride; road elevation, terrain around the road and the landscape tint/fog update together without a reload.
- GPX elevations are median-filtered and smoothed over distance before they drive the road surface and trainer gradient.
- Hide everything on screen for a clean view: the Hide info button, the H key, or say "hide". Press H, click, or say "show" to bring it back.
- Power from a slider, or from a Bluetooth trainer (Chrome or Edge). If the trainer supports resistance control (FTMS), the ride also sends it the road gradient, so climbs feel harder; the button shows "Trainer: power and gradient" or "Trainer: power only". The trainer feels half the road gradient by default; the − and + buttons beside the trainer button change that in steps of 10 %, from off to the full gradient, and the choice is kept in the browser (`?hills=` sets it for one visit). With voice on, say "easier" or "harder" to move it by 20 %, "flat" for off, or "full" for all of it. The last trainer is remembered: the button reads "Reconnect trainer" and rejoins it without the browser's device list where Chrome allows that, and a dropped connection is retried automatically.
- Descents count half in the speed model and the rider brakes above 54 km/h, so downhill speed stays sensible.

## Run it locally

```bash
python3 -m http.server 8791
```

Then open http://localhost:8791/.

## GPX routes and regional scenery

Import a GPX from the Route creator in the launch screen. The route is stored in the browser and can be ridden as `?course=custom`.

The browser handles lightweight route work: parsing, smoothing, statistics, map layers and place labels. A future route builder should handle heavier regional enrichment: DEM terrain, OpenStreetMap buildings, vegetation, road furniture and place-name signs. Generating those assets once is more reliable and performs better than downloading and generating them during every ride.

## Layout

- `index.html` – the whole ride.
- `assets/` – textures and course data. `textures.js` is an embedded copy of the textures, used only when the page is opened straight from disk.
- `tools/` – scripts that build the Leckhampton course data.
- `deploy.sh` – publishes to Spacefast.

## Credits

- Textures: Poly Haven (CC0).
- Terrain: Environment Agency LiDAR Composite DTM, Open Government Licence v3.0.
- Roads, woodland and field boundaries: © OpenStreetMap contributors, ODbL.
- 3D engine: Three.js (MIT), loaded from jsDelivr.

## Licence

The code is released under the MIT licence: see `LICENSE`. Use it, change it, build your own ride.

## Tacx NEO Road Feel

Standard FTMS does not contain a road-surface or vibration command. The Surface Lab therefore changes the FTMS rolling-resistance coefficient (`Crr`) for every trainer, and optionally uses Tacx's unofficial FE-C-over-BLE page `0xFC` Road Feel command for compatible NEO trainers.

Road Feel is off by default and must be enabled explicitly. The experimental implementation starts at 40% intensity, switches among smooth asphalt, concrete plates, cattle grid, hard/soft cobbles, brick, off-road, gravel, ice and wooden boards, and remains limited to Chrome/Edge Web Bluetooth. Stop the ride or switch Road Feel off to send the "simulation off" command.

## Telemetry and device settings

Trainer, heart-rate device IDs and display preferences are stored only in the browser. Where Chromium grants persistent permission, saved devices are reconnectable without repeating the chooser. Browser origin, cleared site data and revoked permissions can still require pairing again.

The display supports Basic, Advanced and Pro metric sets plus 3/5/10/30/60-second rolling windows. Trainer power/cadence, FTMS speed/power, heart rate, RR-derived RMSSD, balance and energy are shown only when the connected device supplies them. Average/max values are time-sampled once per second; NP, IF, TSS, work, estimated calories and W/kg are derived from these measurements plus the rider profile.

Live FTP estimates use the best complete continuous power window in the current ride: 85% of the best 5-minute power and 95% of the best 20-minute power. Both estimates also show their percentage of the configured FTP. The optional left sidebar hides all ride controls except the Metrics button, keeping the rest of the view clear.

Sound, weather and device connections are available from Settings rather than the main riding toolbar. Weather can be fixed, chosen randomly once, or changed to a different supported condition after a random 1–10 minute interval.

Sound and external map services are optional browser settings. VRide itself does not set tracking cookies; external map providers receive the ordinary requests required to display tiles.

The MIT licence covers the code only. The course data in `assets/leckhampton.js` and `assets/leckhampton_osm.js` is derived from the sources credited above and stays under their terms: the Open Government Licence v3.0 for the terrain and the ODbL for the OpenStreetMap data. If you share or adapt that data, keep those credits.
