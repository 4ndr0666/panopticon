### **// PROJECT_PANOPTICON: DEFINITIVE MASTER SPECIFICATION**

#### **1. ARCHITECTURE & STACK**

* **Core:** React 18 + TypeScript (Strict Mode)
* **Build System:** Vite (SWC) + `vite-plugin-cesium`
* **Runtime:** Browser (Client-side heavy)
* **Globe Engine:** CesiumJS (via `resium`)
* **Terrain Provider:** Google Map Tiles API (Photorealistic 3D Tiles)
* **Physics:** `satellite.js` (SGP4/SDP4 propagation) **[OFF-MAIN-THREAD]**
* **State:** Zustand (Transient data store for high-frequency updates)

---

### **2. EXECUTION ROADMAP (The Combined Protocol)**

#### **PHASE 0: THE FOUNDATION (Infrastructure & Stealth)**

**Objective:** Initialize the construct. Establish the connection to the Google Photorealistic Matrix.

* **Tasks:**
* **Scaffold:** `npm create vite@latest panopticon --template react-ts`
* **Dependencies:** `npm install resium cesium satellite.js axios zustand date-fns`
* **Config:** Configure `vite.config.ts` to copy Cesium static assets (Workers, Widgets, Textures) to `public/`.
* **The Viewer:** Initialize `<Viewer />` component:
* Props: `timeline={false}`, `animation={false}`, `baseLayerPicker={false}`.
* **[RESTORED]:** Set `terrainExaggeration={1.5}` to emphasize topology.
* **[AMENDMENT B]:** Enable **Draco Compression** decoding in Cesium config to reduce 3D Tile bandwidth.


* **The World:** Inject Google Cloud API Key. Load `Cesium3DTileset` via `IonResource` or direct URL.


* **Superset Validation Checkpoint:**
* [ ] App loads < 1s.
* [ ] Google 3D Tiles render specific landmarks (e.g., Eiffel Tower, SF Skyline) at high LOD.
* [ ] No default Cesium UI widgets (clean slate).



#### **PHASE 1: THE HIGH GROUND (Orbital Mechanics)**

**Objective:** Mathematical dominance of the sky. Real-time propagation of 5,000+ assets.

* **Tasks:**
* **Data Ingest:** Fetch `active.txt` TLEs from CelesTrak. Parse into `SatRec` objects.
* **[AMENDMENT A] - The Worker Layer:**
* Create `orbital.worker.ts` (Web Worker).
* **Process:** Propagate SGP4 math for all 5,000 objects in parallel (off-main-thread).
* **Output:** `Float32Array` of Cartesian3 positions sent back to Main Thread.


* **Rendering:**
* Use `PointPrimitiveCollection` for performance (Entities are too heavy).
* **[RESTORED] Color Logic:** Starlink (Cyan), Debris (Red), GPS (Gold), Classified (Purple).
* **[RESTORED] Trajectories:** Render orbital paths (polylines) ahead/behind the object (optimize: only for *selected* or *visible* satellites to save draw calls).


* **UX:** Click-to-target. Camera smooth-locks to the satellite's ECI coordinates.


* **Superset Validation Checkpoint:**
* [ ] 60 FPS maintained with >5,000 objects (Worker integration success).
* [ ] ISS position matches real-world trackers (e.g., N2YO).
* [ ] Orbital paths (polylines) render correctly.



#### **PHASE 2: SIGNAL INTERCEPTION (Terrestrial Layers)**

**Objective:** Total visibility of air and sea traffic.

* **Tasks:**
* **ADS-B (Air):**
* Poll OpenSky Network API (`/states/all`) every 10s.
* **[RESTORED] Filtering:** Filter query by `viewer.camera.computeViewRectangle()` (Bounding Box) to optimize bandwidth.
* **[AMENDMENT C] - The Interceptor:** Implement "Backoff & Cache" for 429 errors.
* **Interpolation:** Lerp aircraft positions between poll ticks.


* **AIS (Sea):**
* Integrate AIS stream/scraper.
* Render ships as directional arrows.


* **CCTV (Ground):**
* Create a `CameraEntity` layer.
* Map public IP camera streams (M3U8) to billboards in 3D space.
* **[RESTORED] Feature:** "Cone of Vision" visualization showing the camera frustum.




* **Superset Validation Checkpoint:**
* [ ] Plane altitude corresponds to 3D terrain (not crashing into mountains).
* [ ] Smooth interpolation of aircraft movement.
* [ ] Video feeds load on click.



#### **PHASE 3: THE AESTHETIC (Shader Engineering)**

**Objective:** Make it look like a billion-dollar weapon system.

* **Tasks:**
* **Post-Process Stage:** Inject custom GLSL into `viewer.scene.postProcessStages`.
* **Mode A: Night Vision (NVG):**
* Luminance extraction + Green gradient mapping (`vec3(0.1, 1.0, 0.1)`).
* Static noise (Film grain) + Vignette (Tube effect).


* **Mode B: Thermal (Predator):**
* Invert colors + "Ironbow" palette texture lookup.


* **UI Overlay:**
* Implement "Reticle" center screen.
* Rolling number counters for lat/long/alt.
* Hex-code styling (Cyberpunk/Mil-Spec).




* **Superset Validation Checkpoint:**
* [ ] Shaders toggle instantly without WebGL context loss.
* [ ] NVG noise is animated.
* [ ] UI does not obstruct the tactical view.



---

### **3. GOALS & TRACKING (The Superset Protocol)**

| Feature | Legacy (Target) | 4NDR0666 Spec (FINAL) | Status |
| --- | --- | --- | --- |
| **Terrain** | Google 3D Tiles | Google 3D Tiles + **Draco Comp.** + **Terrain Exaggeration** | 🟡 |
| **Satellites** | Simple Dots | **Instanced Points (Worker) + Trails + TLE Cache** | 🔴 |
| **Air Traffic** | Basic Polling | **Bounding Box Filter + Backoff + 3D Models** | 🔴 |
| **Visuals** | CSS Overlays | **Native WebGL Fragment Shaders (NVG/Thermal)** | 🔴 |
| **Cameras** | None | **Live HLS Stream Injection + Cone of Vision** | 🔴 |

### **4. CRITICAL DIRECTIVES**

1. **NO API KEYS IN REPO:** Use `.env`. If you leak a key, you are compromised.
2. **PERFORMANCE IS A FEATURE:** If the frame rate drops below 30, the mission is a failure.
3. **THE "VIBE" IS MATHEMATICAL:** Do not guess the orbit. Calculate the orbit. The aesthetic is a byproduct of precision.
