# Golden Hour Ride

A proof-of-concept indoor cycling ride that runs in the browser (Three.js, one `index.html`).

Live: https://golden-hour-ride.space.fast/ · Leckhampton loop: https://golden-hour-ride.space.fast/?course=leckhampton

## What's in it

- Two courses: an invented mountain valley, and a real 11.9 km Leckhampton loop built from Environment Agency lidar, dressed as the 19th-century hill (sheep, hay stooks, quarry smoke).
- Five looks: Golden, Overcast, Rain, Mist and Starry (a painted night). Buttons at the top, keys 1–5, or by voice.
- Skip forward and back 250 m with the arrow keys.
- Power from a slider, or from a Bluetooth trainer (Chrome or Edge).

## Run it locally

```bash
python3 -m http.server 8791
```

Then open http://localhost:8791/.

## Layout

- `index.html` – the whole ride.
- `assets/` – textures and course data. `textures.js` is an embedded copy of the textures, used only when the page is opened straight from disk.
- `tools/` – scripts that build the Leckhampton course data.
- `deploy.sh` – publishes to Spacefast.

## Credits

- Textures: Poly Haven (CC0).
- Terrain: Environment Agency LiDAR Composite DTM, Open Government Licence v3.0.
- Roads, woodland and field boundaries: © OpenStreetMap contributors, ODbL.
