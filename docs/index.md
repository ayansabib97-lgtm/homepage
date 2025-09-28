<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Wallpaper Preview</title>
  <style>
    :root{
      --img-url: url("https://4kwallpapers.com/images/walls/thumbs_3t/9287.jpg");
      --bg-size: cover;
      --bg-repeat: no-repeat;
      --bg-position: center center;
    }

    html,body{
      height:100%;
      margin:0;
      font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: #0b0b0b;
      color:#f5f5f5;
    }

    /* Full-screen preview area */
    .preview {
      min-height: calc(100vh - 120px);
      display:flex;
      align-items:center;
      justify-content:center;
      background-image: var(--img-url);
      background-size: var(--bg-size);
      background-repeat: var(--bg-repeat);
      background-position: var(--bg-position);
      transition: background 220ms ease;
      box-shadow: inset 0 0 60px rgba(0,0,0,0.6);
    }

    .panel {
      max-width:1100px;
      width:94%;
      margin: 18px auto;
    }

    .controls {
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      justify-content:center;
      margin-top:10px;
    }

    button, select {
      background: rgba(255,255,255,0.06);
      border:1px solid rgba(255,255,255,0.06);
      color:inherit;
      padding:10px 14px;
      border-radius:10px;
      font-size:14px;
      cursor:pointer;
      backdrop-filter: blur(4px);
    }
    button:hover, select:hover { transform: translateY(-2px); }

    .meta {
      display:flex;
      justify-content:space-between;
      align-items:center;
      gap:12px;
      flex-wrap:wrap;
      margin-top:12px;
      color:#bfc7cf;
      font-size:13px;
    }

    .badge {
      padding:8px 10px;
      background: rgba(255,255,255,0.03);
      border-radius:8px;
      border:1px solid rgba(255,255,255,0.03);
    }

    footer {
      text-align:center;
      color:#9aa7b2;
      font-size:13px;
      margin:18px 0 36px;
    }

    /* small layout niceties for very wide images */
    .preview .frame {
      width:100%;
      height:100%;
      max-height:720px;
      max-width:1280px;
      border-radius:12px;
      overflow:hidden;
      box-shadow: 0 10px 40px rgba(0,0,0,0.6);
      background: linear-gradient(0deg, rgba(0,0,0,0.25), transparent 40%);
    }

    /* mobile adjustments */
    @media (max-width:560px){
      button { padding:10px; font-size:13px; }
      .meta { font-size:12px; }
    }
  </style>
</head>
<body>
  <div class="panel">
    <h1 style="text-align:center; margin:10px 0 0 0;">Browser Wallpaper — Preview</h1>
    <p style="text-align:center; margin:6px 0 14px 0; color:#b9c1c9;">
      Source: 4kwallpapers (image URL embedded). Use the controls to preview how it looks as wallpaper, then Download.
    </p>

    <div class="preview" id="preview">
      <div class="frame" aria-hidden="true"></div>
    </div>

    <div class="controls" role="toolbar" aria-label="Wallpaper controls">
      <button data-size="cover">Cover</button>
      <button data-size="contain">Contain</button>
      <button data-size="auto">Auto</button>

      <select id="repeatSelect" title="Background repeat">
        <option value="no-repeat">No repeat (single)</option>
        <option value="repeat">Tile (repeat)</option>
        <option value="repeat-x">Repeat horizontally</option>
        <option value="repeat-y">Repeat vertically</option>
      </select>

      <select id="positionSelect" title="Background position">
        <option value="center center">Center</option>
        <option value="top center">Top</option>
        <option value="bottom center">Bottom</option>
        <option value="left center">Left</option>
        <option value="right center">Right</option>
      </select>

      <button id="downloadBtn">Download / Open Image</button>
      <button id="openRaw">Open Raw Image</button>
    </div>

    <div class="meta" aria-hidden="true">
      <div class="badge">Preview resolution constraint: max 1280×720 in the frame</div>
      <div class="badge">Original image URL: <em style="opacity:.9">9287.jpg</em></div>
    </div>

    <footer>
      Tip: to use as your desktop wallpaper, click "Download / Open Image" then save the full-resolution image and set it from your OS display settings.
    </footer>
  </div>

  <script>
    // the image URL used as wallpaper
    const img = "https://4kwallpapers.com/images/walls/thumbs_3t/9287.jpg";

    // helper to apply CSS variables on the root element
    function setRootVar(name, value){
      document.documentElement.style.setProperty(name, value);
    }

    // initialize from :root defaults
    setRootVar('--img-url', `url("${img}")`);

    // wire up buttons
    document.querySelectorAll('button[data-size]').forEach(btn=>{
      btn.addEventListener('click', ()=>{
        const size = btn.getAttribute('data-size');
        // map 'auto' to 'initial' so background behaves naturally
        setRootVar('--bg-size', size === 'auto' ? 'auto' : size);
      });
    });

    // repeat selector
    document.getElementById('repeatSelect').addEventListener('change', (e)=>{
      setRootVar('--bg-repeat', e.target.value);
    });

    document.getElementById('positionSelect').addEventListener('change', (e)=>{
      setRootVar('--bg-position', e.target.value);
    });

    // download / open
    document.getElementById('downloadBtn').addEventListener('click', ()=>{
      // open image in new tab (user can right-click Save As)
      window.open(img, '_blank', 'noopener');
    });

    // open raw (same as download, but kept separate for UX)
    document.getElementById('openRaw').addEventListener('click', ()=>{
      window.open(img, '_blank', 'noopener');
    });

    // small keyboard shortcuts: C=cover, T=tile, D=download
    window.addEventListener('keydown',(e)=>{
      if(document.activeElement.tagName === 'INPUT' || document.activeElement.tagName === 'SELECT' ) return;
      if(e.key.toLowerCase() === 'c') setRootVar('--bg-size','cover');
      if(e.key.toLowerCase() === 't') setRootVar('--bg-repeat','repeat');
      if(e.key.toLowerCase() === 'd') window.open(img, '_blank','noopener');
    });
  </script>
</body>
</html>
