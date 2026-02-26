**TARGET:** PROJECT_PANOPTICON // SUPREMACY_TIER
**AUTHORITY:** ROOT
**ENCRYPTION:** NONE

This is not a "dashboard." This is a **Situational Awareness Weapon**. We are building the God's Eye View. The following specification is designed to crush the "weekend project" standard and establish a permanent, extensible intelligence platform.

### **// PROJECT_PANOPTICON: PRODUCTION SPECIFICATION**

#### **1. ARCHITECTURE & STACK**

* **Core:** React 18 + TypeScript (Strict Mode)
* **Build System:** Vite (SWC)
* **Runtime:** Browser (Client-side heavy)
* **Globe Engine:** CesiumJS (via `resium`)
* **Terrain Provider:** Google Map Tiles API (Photorealistic 3D Tiles)
* **Physics:** `satellite.js` (SGP4/SDP4 propagation)
* **State:** Zustand (Transient data store for high-frequency updates)

---

### **2. EXECUTION ROADMAP (Aggressive Timeline)**

#### **PHASE 0: THE FOUNDATION (Day 1)**

**Objective:** Initialize the construct. Establish the connection to the Google Photorealistic Matrix.

* **Tasks:**
* `npm create vite@latest panopticon --template react-ts`
* Install dependencies: `resium`, `cesium`, `satellite.js`, `axios`, `zustand`.
* **CRITICAL:** Configure `vite.config.ts` to copy Cesium static assets (Workers, Textures) to `public/`.
* Initialize `<Viewer />` component with `timeline={false}` and `animation={false}` (we control the clock).
* Inject Google Cloud API Key. Load `Cesium3DTileset` via `IonResource` or direct URL.


* **Validation Checkpoint:**
* [ ] App loads < 1s.
* [ ] Google 3D Tiles render specific landmarks (e.g., Eiffel Tower, SF Skyline) at high LOD.
* [ ] No default Cesium UI widgets (clean slate).



#### **PHASE 1: THE HIGH GROUND (Orbital Mechanics) (Days 2-3)**

**Objective:** Mathematical dominance of the sky. Real-time propagation of 5,000+ assets.

* **Tasks:**
* **Data Ingest:** Fetch `active.txt` TLEs from CelesTrak. Parse into `SatRec` objects.
* **The Loop:** Create a `useFrame` hook equivalent. On every tick:
* Get current GMT.
* Propagate *all* satellites.
* Update `Cartesian3` positions.


* **Visuals:**
* Use `PointPrimitiveCollection` for performance (Entities are too heavy).
* Color code by type: `Starlink` (Cyan), `Debris` (Red), `GPS` (Gold), `Classified` (Purple).


* **UX:** Click-to-target. Camera smooth-locks to the satellite's ECI coordinates.


* **Validation Checkpoint:**
* [ ] 60 FPS maintained with >5,000 objects.
* [ ] ISS position matches real-world trackers (e.g., N2YO).
* [ ] Orbital paths (polylines) render ahead/behind the object.



#### **PHASE 2: SIGNAL INTERCEPTION (Terrestrial Layers) (Days 4-5)**

**Objective:** Total visibility of air and sea traffic.

* **Tasks:**
* **ADS-B (Air):**
* Poll OpenSky Network API (`/states/all`) every 10s.
* Filter by bounding box (optimize bandwidth).
* Interpolate positions between polls to prevent "teleporting" planes.


* **AIS (Sea):**
* Integrate AIS stream. If API costs are prohibitive, use a localized scraper for major ports or WebSocket proxy.
* Render ships as directional arrows.


* **CCTV (Ground):**
* Create a `CameraEntity` layer.
* Map public IP camera streams (M3U8) to billboards in 3D space.
* **Feature:** "Cone of Vision" visualization showing where the camera is looking.




* **Validation Checkpoint:**
* [ ] Plane altitude corresponds to 3D terrain (not crashing into mountains).
* [ ] Smooth interpolation of aircraft movement.
* [ ] Video feeds load on click.



#### **PHASE 3: THE AESTHETIC (Shader Engineering) (Days 6-7)**

**Objective:** Make it look like a billion-dollar weapon system.

* **Tasks:**
* **Post-Process Stage:** Inject custom GLSL into `viewer.scene.postProcessStages`.
* **Mode A: Night Vision (NVG):**
* Luminance extraction.
* Green gradient mapping (`vec3(0.1, 1.0, 0.1)`).
* Static noise (Film grain).
* Vignette (Tube effect).


* **Mode B: Thermal (Predator):**
* Invert colors.
* Apply "Ironbow" palette texture lookup.


* **UI Overlay:**
* Implement "Reticle" center screen.
* Rolling number counters for lat/long/alt.
* Hex-code styling (Cyberpunk/Mil-Spec).




* **Validation Checkpoint:**
* [ ] Shaders toggle instantly without WebGL context loss.
* [ ] NVG noise is animated.
* [ ] UI does not obstruct the tactical view.



---

### **3. GOALS & TRACKING (The Superset Protocol)**

| Feature | Legacy (Target) | 4NDR0666 Spec (Yours) | Status |
| --- | --- | --- | --- |
| **Terrain** | Google 3D Tiles | Google 3D Tiles + **Terrain Exaggeration** | 🟡 |
| **Satellites** | Simple Dots | **Instanced Points + Orbit Trails + TLE Auto-Update** | 🔴 |
| **Air Traffic** | Basic Polling | **Lerp Interpolation + 3D Models** | 🔴 |
| **Visuals** | CSS Overlays | **Native WebGL Fragment Shaders (NVG/Thermal)** | 🔴 |
| **Cameras** | None | **Live HLS Stream Injection** | 🔴 |

### **4. CRITICAL DIRECTIVES**

1. **NO API KEYS IN REPO:** Use `.env`. If you leak a key, you are compromised.
2. **PERFORMANCE IS A FEATURE:** If the frame rate drops below 30, the mission is a failure. Use Web Workers for parsing TLE data.
3. **THE "VIBE" IS MATHEMATICAL:** Do not guess the orbit. Calculate the orbit. The aesthetic is a byproduct of precision.
