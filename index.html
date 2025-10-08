<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>Lawn m² — Minimal</title>
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no"/>

<!-- Leaflet -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin="anonymous"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" crossorigin="anonymous"></script>

<!-- Leaflet.Draw -->
<link rel="stylesheet" href="https://unpkg.com/leaflet-draw@1.0.4/dist/leaflet.draw.css"/>
<script src="https://unpkg.com/leaflet-draw@1.0.4/dist/leaflet.draw.js"></script>

<!-- Turf.js -->
<script src="https://unpkg.com/@turf/turf@6.5.0/turf.min.js"></script>

<style>
  html, body { height:100%; margin:0; }
  #map { height:100%; }

  /* 🔧 Anti–hairline fixes: remove tile seams on mobile/iOS */
  .leaflet-container { background:#000; } /* hides any subpixel gaps */
  .leaflet-container .leaflet-pane,
  .leaflet-container .leaflet-tile,
  .leaflet-container .leaflet-map-pane,
  .leaflet-container .leaflet-tile-container {
    transform: translateZ(0);
    will-change: transform;
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
  }
  .leaflet-container .leaflet-tile { outline: 1px solid transparent; } /* nudge rasterization */

  * { -webkit-tap-highlight-color: transparent; }

  /* ===== Address drawer (magnifier) ===== */
  .addr-drawer {
    position:absolute; top:10px; left:10px; z-index:1200;
    display:flex; align-items:center; gap:6px;
    width:42px; height:38px; padding:6px 8px;
    background:rgba(255,255,255,.98); border-radius:999px;
    box-shadow:0 2px 10px rgba(0,0,0,.15);
    transition: width .25s ease; overflow:hidden;
  }
  .addr-drawer.open, .addr-drawer:hover { width:min(76vw, 360px); }
  .addr-btn {
    width:26px; height:26px; border-radius:999px; border:1px solid #ddd; background:#fff;
    display:grid; place-items:center; cursor:pointer; flex:0 0 auto;
  }
  .addr-input { flex:1 1 auto; min-width:0; border:none; outline:none; font:14px system-ui, Arial; background:transparent; }
  .addr-input::placeholder { color:#888; }
  @media (max-width:420px){ .addr-drawer.open, .addr-drawer:hover { width: 85vw; } }

  /* ===== Basemap toggle (bottom-right) ===== */
  .basemap-toggle {
    position:absolute; right:10px; bottom:12px; z-index:1200;
    background:rgba(255,255,255,.98); border-radius:10px; padding:6px;
    box-shadow:0 2px 10px rgba(0,0,0,.15); display:flex; gap:6px;
  }
  .bm-chip {
    width:42px; height:30px; border:1px solid #ddd; border-radius:6px; cursor:pointer;
    display:grid; place-items:center; font-size:16px; user-select:none;
  }
  .bm-chip.active { outline:2px solid #1E88E5; }

  /* ===== m² label centered in polygon ===== */
  .area-label {
    background:rgba(0,0,0,.45); color:#fff; padding:4px 8px; border-radius:8px;
    font:14px/1.2 system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
    box-shadow:0 2px 8px rgba(0,0,0,.25); white-space:nowrap;
  }
  .copy-toast {
    position:fixed; left:50%; bottom:18px; transform:translateX(-50%);
    background:#111; color:#fff; padding:6px 10px; border-radius:8px;
    font:12px system-ui; z-index:1500; opacity:0; transition:opacity .2s;
  }
  .copy-toast.show { opacity:1; }

  /* ===== Help controls (inline with draw tools) ===== */
  .help-btn {
    position:absolute; left:10px; z-index:1200;
    width:34px; height:34px;
    background:rgba(255,255,255,.98); border:1px solid #ddd; border-radius:999px;
    display:grid; place-items:center; cursor:pointer;
    box-shadow:0 2px 8px rgba(0,0,0,.15);
  }
  .help-panel {
    position:absolute; z-index:1200; left:54px;
    background:rgba(255,255,255,.98); padding:10px 12px; border-radius:12px;
    box-shadow:0 2px 12px rgba(0,0,0,.2);
    width:min(75vw, 320px); max-height:50vh; overflow-y:auto;
    font:14px/1.35 system-ui, Arial; display:none;
  }
  .help-panel.show { display:block; }

  /* ===== Draw toolbar: placement & size (keep icons intact; no filters) ===== */
  .leaflet-top.leaflet-left {
    margin-top: 64px;   /* leave room for magnifier above */
    margin-left: 10px;
  }
  .leaflet-bar a, .leaflet-bar a:hover {
    width: 38px; height: 38px; line-height: 38px;
  }
  /* Important: no filter/shadows on draw icons to avoid GPU seams on some devices */
  .leaflet-draw-toolbar a { /* intentionally empty */ }
</style>
</head>
<body>

<!-- Address drawer -->
<div class="addr-drawer" id="addrDrawer" title="Search address">
  <button class="addr-btn" id="addrBtn" aria-label="Search"><span>🔍</span></button>
  <input class="addr-input" id="addrInput" type="text" placeholder="enter address" />
</div>

<!-- Basemap toggle (bottom-right) -->
<div class="basemap-toggle" id="bmToggle">
  <div class="bm-chip" id="bmMap" title="Map (no satellite)"><span>🗺️</span></div>
  <div class="bm-chip active" id="bmSat" title="Satellite"><span>🛰️</span></div>
</div>

<!-- Help (inline with draw tools) -->
<div class="help-btn" id="helpBtn" title="Help">❓</div>
<div class="help-panel" id="helpPanel">
  <b>Tips</b><br/>
  ▱ Draw polygon → double-tap/click to finish.<br/>
  ✎ Edit vertices to refine.<br/>
  🗑️ Delete polygon.<br/>
  The white <b>m²</b> label sits in the polygon’s center — tap it to <u>copy</u>.
</div>

<div id="map"></div>
<div class="copy-toast" id="toast">Copied m² to clipboard</div>

<script>
  // ===== Basemaps =====
  const osm = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 22, attribution: '&copy; OSM contributors'
  });
  const esriSat = L.tileLayer(
    'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
    { maxZoom: 22, attribution: 'Imagery &copy; Esri, USGS, IGN, etc.' }
  );
  const labelsOverlay = L.tileLayer(
    'https://{s}.basemaps.cartocdn.com/light_only_labels/{z}/{x}/{y}.png',
    { maxZoom: 22, attribution: '&copy; CARTO' }
  );

  const map = L.map('map', {
    zoomControl:false,
    layers:[esriSat, labelsOverlay]
  }).setView([48.8566, 2.3522], 19);

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (pos)=> map.setView([pos.coords.latitude, pos.coords.longitude], 19),
      ()=>{}
    );
  }

  // ===== Draw + Area =====
  const drawnItems = new L.FeatureGroup().addTo(map);
  const drawControl = new L.Control.Draw({
    position:'topleft',
    draw: {
      polygon: { allowIntersection:false, showArea:false,
        shapeOptions:{ color:'#1E88E5', weight:2, fillOpacity:0.25 } },
      polyline:false, rectangle:false, circle:false, marker:false, circlemarker:false
    },
    edit: { featureGroup: drawnItems, edit:true, remove:true }
  });
  map.addControl(drawControl);

  let poly = null;
  let labelMarker = null;

  function fmt(num){
    if (!isFinite(num)) return "–";
    return (num>=100) ? Math.round(num).toLocaleString()
                      : num.toLocaleString(undefined,{maximumFractionDigits:2});
  }

  function polygonToGeoJSON(layer){
    const ll = layer.getLatLngs()[0];
    const ring = ll.map(p=>[p.lng,p.lat]);
    const f = ring[0], l = ring[ring.length-1];
    if (f[0]!==l[0] || f[1]!==l[1]) ring.push(f);
    return { type:"Feature", geometry:{ type:"Polygon", coordinates:[ring] }, properties:{} };
  }

  function copyToClipboard(text){
    const toast = document.getElementById('toast');
    if (navigator.clipboard && navigator.clipboard.writeText) {
      navigator.clipboard.writeText(text).then(()=>{
        toast.classList.add('show'); setTimeout(()=>toast.classList.remove('show'), 900);
      });
    }
  }

  function updateAreaAndLabel(){
    if (!poly) return;
    const gj = polygonToGeoJSON(poly);
    const areaM2 = turf.area(gj);
    const c = turf.centerOfMass(gj).geometry.coordinates; // [lng,lat]
    const latlng = [c[1], c[0]];
    const html = `<div class="area-label" title="Click to copy">${fmt(areaM2)} m²</div>`;

    if (!labelMarker) {
      labelMarker = L.marker(latlng, { icon: L.divIcon({ className:'', html, iconSize:null }) }).addTo(map);
      labelMarker.on('click', ()=> copyToClipboard(`${Math.round(areaM2)} m²`));
      labelMarker.on('tap',  ()=> copyToClipboard(`${Math.round(areaM2)} m²`));
    } else {
      labelMarker.setLatLng(latlng);
      labelMarker.setIcon(L.divIcon({ className:'', html, iconSize:null }));
    }
  }

  map.on(L.Draw.Event.CREATED, e=>{
    drawnItems.clearLayers();
    if (labelMarker) { map.removeLayer(labelMarker); labelMarker=null; }
    poly = e.layer; drawnItems.addLayer(poly);
    poly.on('edit', updateAreaAndLabel);
    updateAreaAndLabel();
  });
  map.on('draw:edited', ()=> { if (poly) updateAreaAndLabel(); });
  map.on('draw:deleted', ()=>{
    poly=null; if (labelMarker){ map.removeLayer(labelMarker); labelMarker=null; }
  });

  // ===== Address search =====
  const drawer = document.getElementById('addrDrawer');
  const addrBtn = document.getElementById('addrBtn');
  const addrInput = document.getElementById('addrInput');
  addrBtn.addEventListener('click', ()=> drawer.classList.toggle('open'));
  addrInput.addEventListener('focus', ()=> drawer.classList.add('open'));

  function geocode(q){
    if (!q) return;
    const url = `https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(q)}`;
    fetch(url).then(r=>r.json()).then(rows=>{
      if (rows && rows[0]) map.setView([+rows[0].lat, +rows[0].lon], 19);
    }).catch(()=>{});
  }
  addrInput.addEventListener('keydown', (e)=>{ if (e.key==='Enter') geocode(addrInput.value.trim()); });
  map.on('click', () => drawer.classList.remove('open')); // collapse drawer on map click

  // ===== Basemap toggle =====
  const bmMap = document.getElementById('bmMap');
  const bmSat = document.getElementById('bmSat');
  function setBase(mode){
    if (mode==='map'){
      if (map.hasLayer(esriSat)) map.removeLayer(esriSat);
      if (!map.hasLayer(osm)) map.addLayer(osm);
      if (map.hasLayer(labelsOverlay)) map.removeLayer(labelsOverlay);
      bmMap.classList.add('active'); bmSat.classList.remove('active');
    } else {
      if (map.hasLayer(osm)) map.removeLayer(osm);
      if (!map.hasLayer(esriSat)) map.addLayer(esriSat);
      if (!map.hasLayer(labelsOverlay)) map.addLayer(labelsOverlay);
      bmSat.classList.add('active'); bmMap.classList.remove('active');
    }
  }
  bmMap.addEventListener('click', ()=> setBase('map'));
  bmSat.addEventListener('click', ()=> setBase('sat'));
  setBase('sat');

  // ===== Help button placement; panel fits viewport using bottom-left anchor =====
  const helpBtn   = document.getElementById('helpBtn');
  const helpPanel = document.getElementById('helpPanel');

  function positionHelpButton() {
    const mapEl   = document.getElementById('map');
    const drawBar = mapEl.querySelector('.leaflet-draw');
    if (!drawBar) return;

    const barRect = drawBar.getBoundingClientRect();
    const mapRect = mapEl.getBoundingClientRect();
    const topPx = (barRect.top - mapRect.top) + barRect.height + 8; // under toolbar
    helpBtn.style.top = `${topPx}px`;
  }

  function toggleHelp() {
    if (helpPanel.classList.contains('show')) {
      helpPanel.classList.remove('show');
      return;
    }
    // Show invisibly to measure
    helpPanel.style.visibility = 'hidden';
    helpPanel.classList.add('show');

    const btnRect   = helpBtn.getBoundingClientRect();
    const panelRect = helpPanel.getBoundingClientRect();
    const viewportBottom = window.innerHeight - 12; // bottom margin
    let top = Math.min(btnRect.bottom, viewportBottom) - panelRect.height; // anchor by bottom-left
    top = Math.max(8, top); // top margin
    const mapRect = document.getElementById('map').getBoundingClientRect();
    helpPanel.style.top = `${top - mapRect.top}px`;
    helpPanel.style.visibility = '';
  }

  window.addEventListener('resize', positionHelpButton);
  setTimeout(positionHelpButton, 0);
  helpBtn.addEventListener('click', toggleHelp);
  map.on('click', () => helpPanel.classList.remove('show'));
</script>
</body>
</html>
