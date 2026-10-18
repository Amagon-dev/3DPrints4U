<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#f4f5f1">
  <title>Filament Vault | 3DPrints4U</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      color-scheme: light;
      --page: #f4f5f1;
      --surface: #fff;
      --surface-soft: #eaede7;
      --ink: #20241f;
      --muted: #737a70;
      --line: #dfe3dc;
      --accent: #c8ef52;
      --accent-ink: #25320a;
      --alert: #d96845;
      --shadow: 0 12px 32px rgb(32 36 31 / 7%);
    }
    :root.dark {
      color-scheme: dark;
      --page: #191c19;
      --surface: #232723;
      --surface-soft: #303630;
      --ink: #edf0e9;
      --muted: #a1a89e;
      --line: #3b423a;
      --accent: #c8ef52;
      --accent-ink: #25320a;
      --alert: #f08a69;
      --shadow: 0 12px 32px rgb(0 0 0 / 20%);
    }
    * { box-sizing: border-box; }
    body { margin: 0; background: var(--page); color: var(--ink); font: 15px/1.45 "Space Grotesk", sans-serif; transition: background .2s, color .2s; }
    button, input, select, textarea { font: inherit; }
    button { color: inherit; cursor: pointer; }
    .shell { max-width: 1440px; margin: auto; padding: 28px clamp(18px, 4vw, 56px) 64px; }
    .topbar { display: flex; align-items: center; justify-content: space-between; gap: 20px; margin-bottom: 44px; }
    .brand { display: flex; align-items: center; gap: 12px; font-size: 15px; font-weight: 700; letter-spacing: .02em; }
    .brand-mark { width: 34px; height: 34px; display: grid; place-items: center; border-radius: 10px; background: var(--ink); color: var(--accent); }
    .brand-mark svg { width: 21px; height: 21px; }
    .brand small { display: block; color: var(--muted); font: 10px "DM Mono", monospace; letter-spacing: .11em; text-transform: uppercase; }
    .top-actions { display: flex; align-items: center; gap: 10px; }
    .icon-button, .secondary-button, .primary-button { min-height: 40px; border: 1px solid var(--line); border-radius: 8px; background: var(--surface); padding: 0 14px; font-size: 13px; font-weight: 600; transition: transform .15s, border-color .15s, background .15s; }
    .icon-button { width: 40px; padding: 0; font-size: 17px; }
    .primary-button { border-color: var(--accent); background: var(--accent); color: var(--accent-ink); }
    .icon-button:hover, .secondary-button:hover { border-color: var(--ink); }
    .primary-button:hover { transform: translateY(-1px); }
    .intro { position: relative; display: flex; align-items: end; justify-content: space-between; gap: 28px; padding: 30px 0 34px; border-top: 1px solid var(--line); background-image: repeating-linear-gradient(0deg, transparent, transparent 31px, rgb(112 125 104 / 6%) 32px), repeating-linear-gradient(90deg, transparent, transparent 31px, rgb(112 125 104 / 6%) 32px); background-position: right top; background-size: 320px 160px; background-repeat: no-repeat; }
    .eyebrow { margin: 0 0 9px; color: var(--muted); font: 11px "DM Mono", monospace; letter-spacing: .13em; text-transform: uppercase; }
    h1 { margin: 0; font-size: clamp(34px, 5vw, 55px); line-height: 1; letter-spacing: 0; }
    .intro-copy { max-width: 440px; margin: 13px 0 0; color: var(--muted); font-size: 14px; }
    .last-updated { color: var(--muted); font: 11px "DM Mono", monospace; white-space: nowrap; }
    .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin: 10px 0 30px; }
    .stat { min-height: 114px; padding: 18px 20px; background: var(--surface); border: 1px solid var(--line); border-radius: 10px; }
    .stat-label { color: var(--muted); font: 11px "DM Mono", monospace; letter-spacing: .08em; text-transform: uppercase; }
    .stat-value { display: flex; align-items: baseline; gap: 7px; margin-top: 11px; font-size: 32px; font-weight: 700; line-height: 1; }
    .stat-unit { color: var(--muted); font-size: 13px; font-weight: 500; }
    .stat-low .stat-value { color: var(--alert); }
    .inventory-head { display: flex; justify-content: space-between; align-items: center; gap: 18px; margin-bottom: 14px; }
    .inventory-head h2 { margin: 0; font-size: 18px; }
    .toolbar { display: flex; align-items: center; gap: 10px; }
    .search-wrap { position: relative; }
    .search-wrap input { width: min(310px, 36vw); height: 40px; padding: 0 14px 0 38px; border: 1px solid var(--line); border-radius: 8px; background: var(--surface); color: var(--ink); outline: none; }
    .search-wrap input:focus, .field input:focus, .field select:focus, .field textarea:focus { border-color: #829c3b; box-shadow: 0 0 0 3px rgb(200 239 82 / 23%); }
    .search-icon { position: absolute; left: 13px; top: 11px; color: var(--muted); font-size: 14px; }
    .filters { display: flex; gap: 7px; overflow-x: auto; padding: 2px 0 15px; scrollbar-width: thin; }
    .filter { flex-shrink: 0; padding: 7px 12px; border: 1px solid var(--line); border-radius: 6px; background: transparent; color: var(--muted); font-size: 12px; font-weight: 600; }
    .filter[aria-pressed="true"] { border-color: var(--ink); background: var(--ink); color: var(--surface); }
    .inventory-grid { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 12px; }
    .spool-card { min-width: 0; padding: 17px; border: 1px solid var(--line); border-radius: 10px; background: var(--surface); box-shadow: 0 2px 5px rgb(32 36 31 / 2%); animation: arrive .35s both; }
    @keyframes arrive { from { opacity: 0; transform: translateY(7px); } to { opacity: 1; transform: translateY(0); } }
    .card-top { display: flex; align-items: flex-start; justify-content: space-between; gap: 14px; }
    .spool-info { display: flex; align-items: center; gap: 13px; min-width: 0; }
    .spool-art { position: relative; display: grid; width: 54px; height: 54px; flex: 0 0 auto; place-items: center; border-radius: 50%; background: #343a34; border: 5px solid #4c534b; }
    .spool-art::before { content: ""; width: 29px; height: 29px; border-radius: 50%; background: var(--spool-color); box-shadow: inset 0 0 0 6px rgb(0 0 0 / 8%); }
    .spool-art::after { content: ""; position: absolute; width: 12px; height: 12px; border: 3px solid #687066; border-radius: 50%; background: #20241f; }
    .spool-copy { min-width: 0; }
    .brand-line { display: flex; align-items: center; flex-wrap: wrap; gap: 7px; color: var(--muted); font-size: 11px; }
    .material-tag { padding: 3px 6px; border-radius: 4px; background: var(--surface-soft); color: var(--ink); font: 10px "DM Mono", monospace; }
    .spool-copy h3 { overflow: hidden; margin: 5px 0 2px; font-size: 16px; text-overflow: ellipsis; white-space: nowrap; }
    .spool-subtitle { color: var(--muted); font-size: 12px; }
    .edit-button { flex: 0 0 auto; padding: 6px 9px; border: 0; border-radius: 6px; background: var(--surface-soft); color: var(--ink); font-size: 11px; font-weight: 600; }
    .card-meta { display: flex; flex-wrap: wrap; gap: 14px; margin: 17px 0 12px; color: var(--muted); font: 11px "DM Mono", monospace; }
    .card-meta span { display: inline-flex; align-items: center; gap: 5px; }
    .meta-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--spool-color); }
    .level-row { display: flex; align-items: center; gap: 11px; }
    .progress { overflow: hidden; height: 6px; flex: 1; border-radius: 10px; background: var(--surface-soft); }
    .progress span { display: block; height: 100%; border-radius: inherit; background: var(--spool-color); transition: width .2s; }
    .level-button { flex: 0 0 auto; padding: 0; border: 0; background: transparent; color: var(--muted); font: 11px "DM Mono", monospace; }
    .card-bottom { display: flex; align-items: center; justify-content: space-between; gap: 10px; margin-top: 15px; padding-top: 12px; border-top: 1px solid var(--line); }
    .location { overflow: hidden; color: var(--muted); font-size: 11px; text-overflow: ellipsis; white-space: nowrap; }
    .stepper { display: flex; align-items: center; gap: 9px; flex: 0 0 auto; }
    .stepper button { display: grid; width: 27px; height: 27px; place-items: center; border: 1px solid var(--line); border-radius: 6px; background: var(--surface); font-size: 16px; line-height: 1; }
    .stepper span { min-width: 17px; text-align: center; font: 12px "DM Mono", monospace; }
    .empty-state { padding: 65px 20px; border: 1px dashed var(--line); border-radius: 10px; text-align: center; }
    .empty-state h3 { margin: 0 0 6px; font-size: 18px; }
    .empty-state p { margin: 0 auto 18px; color: var(--muted); font-size: 13px; }
    dialog { width: min(560px, calc(100% - 28px)); max-height: min(90vh, 790px); padding: 0; border: 1px solid var(--line); border-radius: 12px; background: var(--page); color: var(--ink); box-shadow: var(--shadow); }
    dialog::backdrop { background: rgb(15 19 15 / 55%); backdrop-filter: blur(3px); }
    .dialog-head { display: flex; justify-content: space-between; align-items: center; padding: 20px 22px; border-bottom: 1px solid var(--line); }
    .dialog-head h2 { margin: 0; font-size: 18px; }
    .dialog-close { width: 32px; height: 32px; border: 1px solid var(--line); border-radius: 7px; background: var(--surface); font-size: 18px; }
    .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; padding: 20px 22px; overflow-y: auto; }
    .field { min-width: 0; display: flex; flex-direction: column; gap: 6px; }
    .field.full { grid-column: 1 / -1; }
    .field label { color: var(--muted); font: 10px "DM Mono", monospace; letter-spacing: .07em; text-transform: uppercase; }
    .field input, .field select, .field textarea { width: 100%; min-height: 40px; padding: 9px 10px; border: 1px solid var(--line); border-radius: 7px; background: var(--surface); color: var(--ink); outline: none; }
    .field input[type="color"] { height: 40px; padding: 3px; }
    .field textarea { min-height: 70px; resize: vertical; }
    .form-actions { display: flex; align-items: center; gap: 9px; padding: 15px 22px 20px; border-top: 1px solid var(--line); }
    .form-actions .spacer { flex: 1; }
    .delete-button { border: 0; background: transparent; color: var(--alert); font-size: 12px; font-weight: 600; }
    .toast { position: fixed; bottom: 22px; left: 50%; z-index: 5; padding: 10px 16px; transform: translate(-50%, 15px); border-radius: 7px; background: var(--ink); color: var(--surface); font-size: 12px; opacity: 0; pointer-events: none; transition: .2s; }
    .toast.visible { transform: translate(-50%, 0); opacity: 1; }
    @media (max-width: 760px) {
      .shell { padding-top: 18px; }
      .topbar { margin-bottom: 30px; }
      .intro { align-items: flex-start; flex-direction: column; gap: 14px; }
      .inventory-head { align-items: flex-start; flex-direction: column; }
      .toolbar, .search-wrap, .search-wrap input { width: 100%; }
      .toolbar .primary-button { flex: 0 0 auto; }
      .inventory-grid { grid-template-columns: 1fr; }
    }
    @media (max-width: 480px) {
      .shell { padding-inline: 14px; }
      .brand { font-size: 13px; }
      .secondary-button { display: none; }
      .stats { gap: 7px; }
      .stat { min-height: 90px; padding: 13px 10px; }
      .stat-label { font-size: 9px; letter-spacing: .03em; }
      .stat-value { font-size: 25px; }
      .form-grid { gap: 11px; padding: 16px; }
      .dialog-head { padding: 16px; }
      .form-actions { padding-inline: 16px; }
    }
    @media (prefers-reduced-motion: reduce) { *, *::before, *::after { animation-duration: .01ms !important; transition-duration: .01ms !important; } }
  </style>
</head>
<body>
  <div class="shell">
    <header class="topbar">
      <div class="brand">
        <span class="brand-mark" aria-hidden="true"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="12" cy="12" r="9"/><circle cx="12" cy="12" r="4"/><path d="M3.5 12h4.5m8 0h4.5M12 3.5V8m0 8v4.5"/></svg></span>
        <span>3DPrints4U<small>Workshop inventory</small></span>
      </div>
      <div class="top-actions">
        <button class="secondary-button" id="exportButton" type="button">Export inventory</button>
        <button class="icon-button" id="themeButton" type="button" aria-label="Toggle dark theme" title="Toggle theme">◐</button>
      </div>
    </header>

    <main>
      <section class="intro" aria-labelledby="pageTitle">
        <div>
          <p class="eyebrow">Materials / Filament</p>
          <h1 id="pageTitle">Filament Vault</h1>
          <p class="intro-copy">A clear view of what's on the shelf, what's loaded, and what needs a restock.</p>
        </div>
        <div class="last-updated" id="lastUpdated"></div>
      </section>

      <section class="stats" aria-label="Inventory totals">
        <article class="stat"><div class="stat-label">Total rolls</div><div class="stat-value" id="totalRolls">0</div></article>
        <article class="stat"><div class="stat-label">Estimated material</div><div class="stat-value"><span id="totalWeight">0.0</span><span class="stat-unit">kg</span></div></article>
        <article class="stat stat-low"><div class="stat-label">Low rolls · ¼ or less</div><div class="stat-value" id="lowRolls">0</div></article>
      </section>

      <section aria-labelledby="inventoryTitle">
        <div class="inventory-head">
          <h2 id="inventoryTitle">Your inventory</h2>
          <div class="toolbar">
            <label class="search-wrap"><span class="search-icon" aria-hidden="true">⌕</span><input id="searchInput" type="search" placeholder="Search material, color, location" aria-label="Search inventory"></label>
            <button class="primary-button" id="addButton" type="button">＋ Add roll</button>
          </div>
        </div>
        <div class="filters" id="filters" aria-label="Filter inventory">
          <button class="filter" data-filter="ALL" aria-pressed="true">All materials</button>
          <button class="filter" data-filter="PLA" aria-pressed="false">PLA</button>
          <button class="filter" data-filter="PETG" aria-pressed="false">PETG</button>
          <button class="filter" data-filter="ENGINEERING" aria-pressed="false">Engineering</button>
          <button class="filter" data-filter="FLEXIBLE" aria-pressed="false">Flexible</button>
          <button class="filter" data-filter="SPECIALTY" aria-pressed="false">Specialty</button>
        </div>
        <div class="inventory-grid" id="inventoryGrid"></div>
        <div class="empty-state" id="emptyState" hidden>
          <h3>No rolls match</h3>
          <p>Try another search or add a roll to your inventory.</p>
          <button class="primary-button" id="emptyAddButton" type="button">＋ Add first roll</button>
        </div>
      </section>
    </main>
  </div>

  <dialog id="spoolDialog" aria-labelledby="dialogTitle">
    <form id="spoolForm">
      <div class="dialog-head"><h2 id="dialogTitle">Add filament roll</h2><button class="dialog-close" id="closeDialog" type="button" aria-label="Close">×</button></div>
      <div class="form-grid">
        <input type="hidden" id="spoolId">
        <div class="field"><label for="brandInput">Manufacturer</label><input id="brandInput" name="brand" required maxlength="60" placeholder="e.g. Bambu Lab"></div>
        <div class="field"><label for="materialInput">Material</label><select id="materialInput" name="material"><option>PLA</option><option>PETG</option><option>ABS</option><option>ASA</option><option>TPU</option><option>PC</option><option>PA-CF</option><option>PVA</option><option>Other</option></select></div>
        <div class="field"><label for="colorNameInput">Color name</label><input id="colorNameInput" name="colorName" required maxlength="60" placeholder="e.g. Jade Green"></div>
        <div class="field"><label for="colorHexInput">Color swatch</label><input id="colorHexInput" name="colorHex" type="color" value="#34c759"></div>
        <div class="field"><label for="finishInput">Finish</label><select id="finishInput" name="finish"><option>Standard</option><option>Matte</option><option>Silk</option><option>Translucent</option><option>Glitter</option><option>Glow-in-the-dark</option><option>Carbon fiber</option><option>Wood composite</option><option>Other</option></select></div>
        <div class="field"><label for="weightInput">Spool weight (g)</label><select id="weightInput" name="spoolWeight"><option value="250">250 g</option><option value="500">500 g</option><option value="750">750 g</option><option value="1000" selected>1,000 g</option><option value="2000">2,000 g</option></select></div>
        <div class="field"><label for="remainingInput">Remaining</label><select id="remainingInput" name="amountRemaining"><option value="1">Full · 100%</option><option value="0.75">¾ · 75%</option><option value="0.5">½ · 50%</option><option value="0.25">¼ · 25%</option></select></div>
        <div class="field"><label for="quantityInput">Roll quantity</label><input id="quantityInput" name="quantity" type="number" min="1" max="999" step="1" value="1" required></div>
        <div class="field full"><label for="locationInput">Storage location</label><input id="locationInput" name="location" maxlength="100" placeholder="e.g. Dry box shelf 2"></div>
        <div class="field full"><label for="notesInput">Notes</label><textarea id="notesInput" name="notes" maxlength="500" placeholder="Print settings, special handling, or other notes"></textarea></div>
      </div>
      <div class="form-actions">
        <button class="delete-button" id="deleteButton" type="button" hidden>Delete roll</button>
        <span class="spacer"></span>
        <button class="secondary-button" id="cancelButton" type="button">Cancel</button>
        <button class="primary-button" type="submit">Save roll</button>
      </div>
    </form>
  </dialog>
  <div class="toast" id="toast" role="status" aria-live="polite"></div>

  <script>
    const STORAGE_KEY = 'filament_vault_data';
    const LEVELS = [1, 0.75, 0.5, 0.25];
    const ENGINEERING = ['ABS', 'ASA', 'PC', 'PA-CF'];
    const SPECIALTY = ['PVA', 'Other'];
    const seedInventory = [
      { id: 'spool_1', brand: 'Bambu Lab', material: 'PLA', finish: 'Standard', colorName: 'Bambu Green', colorHex: '#34c759', spoolWeight: 1000, amountRemaining: 1, quantity: 2, location: 'AMS 1 · Slot 1', notes: 'Official RFID spool, 220°C' },
      { id: 'spool_2', brand: 'Polymaker', material: 'PETG', finish: 'Translucent', colorName: 'Translucent Cyan', colorHex: '#007aff', spoolWeight: 1000, amountRemaining: 0.75, quantity: 1, location: 'Dry box · Shelf 2', notes: '240°C nozzle, 75°C bed' },
      { id: 'spool_3', brand: 'Atomic Filament', material: 'ABS', finish: 'Matte', colorName: 'Carbon Black', colorHex: '#292c2b', spoolWeight: 1000, amountRemaining: 0.5, quantity: 3, location: 'Voron cabinet', notes: 'Enclosure needed' },
      { id: 'spool_4', brand: 'ERYONE', material: 'PLA', finish: 'Silk', colorName: 'Blue / Magenta Silk', colorHex: '#2878dc', color2Hex: '#ed4275', spoolWeight: 1000, amountRemaining: 0.75, quantity: 1, location: 'AMS 2 · Slot 3', notes: 'Rotate model for color split' },
      { id: 'spool_5', brand: 'Overture', material: 'TPU', finish: 'Standard', colorName: 'High-Vis Orange', colorHex: '#f28b36', spoolWeight: 1000, amountRemaining: 0.25, quantity: 1, location: 'Dry box · Shelf 1', notes: 'Direct drive only' }
    ];

    const grid = document.getElementById('inventoryGrid');
    const dialog = document.getElementById('spoolDialog');
    const form = document.getElementById('spoolForm');
    const toast = document.getElementById('toast');
    let inventory = loadInventory();
    let activeFilter = 'ALL';
    let searchQuery = '';
    let toastTimer;

    function loadInventory() {
      try {
        const saved = localStorage.getItem(STORAGE_KEY);
        if (saved === null) {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(seedInventory));
          return structuredClone(seedInventory);
        }
        const parsed = JSON.parse(saved);
        return Array.isArray(parsed) ? parsed : structuredClone(seedInventory);
      } catch {
        return structuredClone(seedInventory);
      }
    }

    function saveInventory() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(inventory));
      document.getElementById('lastUpdated').textContent = `UPDATED ${new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}`;
    }

    function element(tag, className, text) {
      const node = document.createElement(tag);
      if (className) node.className = className;
      if (text !== undefined) node.textContent = text;
      return node;
    }

    function visibleInventory() {
      return inventory.filter((item) => {
        const material = item.material || 'Other';
        if (activeFilter === 'PLA' && material !== 'PLA') return false;
        if (activeFilter === 'PETG' && material !== 'PETG') return false;
        if (activeFilter === 'ENGINEERING' && !ENGINEERING.includes(material)) return false;
        if (activeFilter === 'FLEXIBLE' && material !== 'TPU') return false;
        if (activeFilter === 'SPECIALTY' && !SPECIALTY.includes(material)) return false;
        const searchable = [item.brand, material, item.finish, item.colorName, item.location, item.notes].join(' ').toLowerCase();
        return searchable.includes(searchQuery);
      });
    }

    function renderCard(spool, index) {
      const card = element('article', 'spool-card');
      card.style.animationDelay = `${Math.min(index * 35, 175)}ms`;
      const color = /^#[0-9a-f]{6}$/i.test(spool.colorHex) ? spool.colorHex : '#999999';
      card.style.setProperty('--spool-color', color);
      const top = element('div', 'card-top');
      const info = element('div', 'spool-info');
      info.append(element('div', 'spool-art'));
      const copy = element('div', 'spool-copy');
      const brandLine = element('div', 'brand-line');
      brandLine.append(element('span', 'material-tag', spool.material || 'Other'));
      brandLine.append(element('span', '', spool.brand || 'Unknown brand'));
      copy.append(brandLine, element('h3', '', spool.colorName || 'Unnamed color'));
      copy.append(element('div', 'spool-subtitle', `${spool.finish || 'Standard'} finish`));
      info.append(copy);
      const edit = element('button', 'edit-button', 'Edit');
      edit.type = 'button';
      edit.setAttribute('aria-label', `Edit ${spool.colorName || 'spool'}`);
      edit.addEventListener('click', () => openEditor(spool));
      top.append(info, edit);

      const meta = element('div', 'card-meta');
      const colorMeta = element('span');
      colorMeta.append(element('i', 'meta-dot'));
      colorMeta.append(document.createTextNode(color.toUpperCase()));
      meta.append(colorMeta, element('span', '', `${Number(spool.spoolWeight) || 1000} g roll`));
      meta.append(element('span', '', `~${Math.round((Number(spool.spoolWeight) || 1000) * (Number(spool.amountRemaining) || 0))} g remaining`));

      const levelRow = element('div', 'level-row');
      const progress = element('div', 'progress');
      const fill = element('span');
      const remaining = LEVELS.includes(Number(spool.amountRemaining)) ? Number(spool.amountRemaining) : 1;
      fill.style.width = `${remaining * 100}%`;
      progress.append(fill);
      const levelButton = element('button', 'level-button', `${remaining * 100}% · cycle`);
      levelButton.type = 'button';
      levelButton.setAttribute('aria-label', `Cycle remaining amount for ${spool.colorName || 'spool'}`);
      levelButton.addEventListener('click', () => cycleLevel(spool.id));
      levelRow.append(progress, levelButton);

      const bottom = element('div', 'card-bottom');
      bottom.append(element('div', 'location', spool.location ? `⌖  ${spool.location}` : 'No location set'));
      const stepper = element('div', 'stepper');
      const decrease = element('button', '', '−');
      decrease.type = 'button';
      decrease.setAttribute('aria-label', `Remove one ${spool.colorName || 'spool'}`);
      decrease.addEventListener('click', () => adjustQuantity(spool.id, -1));
      const quantity = element('span', '', String(Number(spool.quantity) || 1));
      const increase = element('button', '', '+');
      increase.type = 'button';
      increase.setAttribute('aria-label', `Add one ${spool.colorName || 'spool'}`);
      increase.addEventListener('click', () => adjustQuantity(spool.id, 1));
      stepper.append(decrease, quantity, increase);
      bottom.append(stepper);
      card.append(top, meta, levelRow, bottom);
      return card;
    }

    function render() {
      grid.replaceChildren();
      const filtered = visibleInventory();
      filtered.forEach((spool, index) => grid.append(renderCard(spool, index)));
      document.getElementById('emptyState').hidden = filtered.length > 0;
      document.getElementById('totalRolls').textContent = inventory.reduce((sum, item) => sum + (Number(item.quantity) || 1), 0);
      const grams = inventory.reduce((sum, item) => sum + (Number(item.spoolWeight) || 1000) * (Number(item.amountRemaining) || 0) * (Number(item.quantity) || 1), 0);
      document.getElementById('totalWeight').textContent = (grams / 1000).toFixed(1);
      document.getElementById('lowRolls').textContent = inventory.reduce((sum, item) => sum + (Number(item.amountRemaining) <= 0.25 ? (Number(item.quantity) || 1) : 0), 0);
    }

    function adjustQuantity(id, delta) {
      const spool = inventory.find((item) => item.id === id);
      if (!spool) return;
      const quantity = Math.max(0, (Number(spool.quantity) || 1) + delta);
      if (quantity === 0) {
        inventory = inventory.filter((item) => item.id !== id);
        showToast('Roll removed from inventory');
      } else {
        spool.quantity = quantity;
      }
      saveInventory();
      render();
    }

    function cycleLevel(id) {
      const spool = inventory.find((item) => item.id === id);
      if (!spool) return;
      const current = LEVELS.indexOf(Number(spool.amountRemaining));
      spool.amountRemaining = LEVELS[(current + 1) % LEVELS.length];
      saveInventory();
      render();
    }

    function openEditor(spool) {
      form.reset();
      document.getElementById('spoolId').value = spool?.id || '';
      document.getElementById('dialogTitle').textContent = spool ? 'Edit filament roll' : 'Add filament roll';
      document.getElementById('deleteButton').hidden = !spool;
      if (spool) {
        for (const key of ['brand', 'material', 'colorName', 'colorHex', 'finish', 'spoolWeight', 'amountRemaining', 'quantity', 'location', 'notes']) {
          const field = form.elements.namedItem(key);
          if (field) field.value = spool[key] ?? (key === 'quantity' ? 1 : '');
        }
      } else {
        document.getElementById('colorHexInput').value = '#34c759';
        document.getElementById('quantityInput').value = '1';
      }
      dialog.showModal();
      document.getElementById('brandInput').focus();
    }

    function showToast(message) {
      toast.textContent = message;
      toast.classList.add('visible');
      clearTimeout(toastTimer);
      toastTimer = setTimeout(() => toast.classList.remove('visible'), 2200);
    }

    document.getElementById('addButton').addEventListener('click', () => openEditor());
    document.getElementById('emptyAddButton').addEventListener('click', () => openEditor());
    document.getElementById('closeDialog').addEventListener('click', () => dialog.close());
    document.getElementById('cancelButton').addEventListener('click', () => dialog.close());
    document.getElementById('searchInput').addEventListener('input', (event) => {
      searchQuery = event.target.value.trim().toLowerCase();
      render();
    });
    document.getElementById('filters').addEventListener('click', (event) => {
      const button = event.target.closest('[data-filter]');
      if (!button) return;
      activeFilter = button.dataset.filter;
      document.querySelectorAll('.filter').forEach((filter) => filter.setAttribute('aria-pressed', String(filter === button)));
      render();
    });
    document.getElementById('themeButton').addEventListener('click', () => {
      document.documentElement.classList.toggle('dark');
      localStorage.setItem('filament_vault_theme', document.documentElement.classList.contains('dark') ? 'dark' : 'light');
    });
    document.getElementById('exportButton').addEventListener('click', () => {
      const blob = new Blob([JSON.stringify(inventory, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const link = element('a');
      link.href = url;
      link.download = `filament_inventory_${new Date().toISOString().slice(0, 10)}.json`;
      link.click();
      URL.revokeObjectURL(url);
      showToast('Inventory exported');
    });
    document.getElementById('deleteButton').addEventListener('click', () => {
      const id = document.getElementById('spoolId').value;
      if (!id || !window.confirm('Delete this roll from your inventory?')) return;
      inventory = inventory.filter((item) => item.id !== id);
      saveInventory();
      render();
      dialog.close();
      showToast('Roll deleted');
    });
    form.addEventListener('submit', (event) => {
      event.preventDefault();
      if (!form.reportValidity()) return;
      const data = new FormData(form);
      const id = document.getElementById('spoolId').value;
      const spool = {
        id: id || (crypto.randomUUID ? crypto.randomUUID() : `spool_${Date.now()}`),
        brand: String(data.get('brand')).trim(),
        material: String(data.get('material')),
        colorName: String(data.get('colorName')).trim(),
        colorHex: String(data.get('colorHex')),
        finish: String(data.get('finish')),
        spoolWeight: Number(data.get('spoolWeight')),
        amountRemaining: Number(data.get('amountRemaining')),
        quantity: Number(data.get('quantity')),
        location: String(data.get('location')).trim(),
        notes: String(data.get('notes')).trim(),
        updatedAt: Date.now()
      };
      const index = inventory.findIndex((item) => item.id === id);
      if (index === -1) inventory.unshift(spool);
      else inventory[index] = { ...inventory[index], ...spool };
      saveInventory();
      render();
      dialog.close();
      showToast(index === -1 ? 'Roll added to inventory' : 'Roll details saved');
    });

    if (localStorage.getItem('filament_vault_theme') === 'dark') document.documentElement.classList.add('dark');
    document.getElementById('lastUpdated').textContent = `LOCAL STORAGE · ${inventory.length} FILAMENT TYPES`;
    render();
  </script>
</body>
</html>
