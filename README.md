<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Nouri — Nutrition Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&display=swap" rel="stylesheet">
<style>
:root {
  --sage: #7aab8a;
  --sage-light: #a8c9b4;
  --sage-pale: #e8f2ec;
  --sage-dark: #4a7a5c;
  --mint: #c8e6d4;
  --cream: #faf9f6;
  --white: #ffffff;
  --stone: #f2efea;
  --pebble: #e0dbd2;
  --ink: #1a1f1c;
  --ink-soft: #3d4740;
  --muted: #8a9490;
  --warn: #e07b54;
  --warn-pale: #fdf0eb;
  --danger: #d44f4f;
  --calorie-color: #7aab8a;
  --protein-color: #5b9bd5;
  --fat-color: #e8b84b;
  --carb-color: #c97dd0;
  --water-color: #6bb5d6;
  --shadow-sm: 0 1px 3px rgba(26,31,28,0.06), 0 1px 2px rgba(26,31,28,0.04);
  --shadow: 0 4px 16px rgba(26,31,28,0.08), 0 2px 6px rgba(26,31,28,0.04);
  --shadow-lg: 0 16px 48px rgba(26,31,28,0.12), 0 4px 16px rgba(26,31,28,0.06);
  --radius: 16px;
  --radius-sm: 10px;
  --radius-lg: 24px;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--cream);
  color: var(--ink);
  min-height: 100vh;
  overflow-x: hidden;
}

/* ── LAYOUT ── */
.app { display: flex; flex-direction: column; min-height: 100vh; max-width: 480px; margin: 0 auto; position: relative; background: var(--cream); }

/* ── TOP BAR ── */
.topbar {
  background: var(--white);
  padding: 16px 20px 0;
  position: sticky; top: 0; z-index: 100;
  border-bottom: 1px solid var(--pebble);
  box-shadow: var(--shadow-sm);
}
.topbar-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.app-logo { font-family: 'DM Serif Display', serif; font-size: 22px; color: var(--sage-dark); letter-spacing: -0.5px; }
.app-logo span { color: var(--sage); }
.topbar-actions { display: flex; gap: 8px; }
.icon-btn { width: 36px; height: 36px; border-radius: 50%; border: none; background: var(--stone); color: var(--ink-soft); cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all .2s; font-size: 16px; }
.icon-btn:hover { background: var(--sage-pale); color: var(--sage-dark); }

/* Calendar strip */
.calendar-strip { display: flex; gap: 6px; overflow-x: auto; padding-bottom: 14px; scrollbar-width: none; }
.calendar-strip::-webkit-scrollbar { display: none; }
.cal-day {
  flex: 0 0 44px; height: 62px; border-radius: var(--radius-sm); display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 2px; cursor: pointer; transition: all .2s; border: 2px solid transparent;
  background: var(--stone);
}
.cal-day:hover { background: var(--sage-pale); border-color: var(--sage-light); }
.cal-day.active { background: var(--sage); border-color: var(--sage); color: white; box-shadow: 0 4px 12px rgba(122,171,138,0.4); }
.cal-day.today:not(.active) { border-color: var(--sage-light); }
.cal-day .day-name { font-size: 10px; font-weight: 500; opacity: 0.7; letter-spacing: 0.5px; text-transform: uppercase; }
.cal-day .day-num { font-size: 17px; font-weight: 600; }
.cal-day.active .day-name, .cal-day.active .day-num { color: white; opacity: 1; }

/* ── MAIN CONTENT ── */
.main { flex: 1; padding: 0 16px 100px; overflow-y: auto; }

/* ── TABS ── */
.tabs { display: flex; gap: 4px; padding: 14px 0 2px; }
.tab { flex: 1; padding: 8px 4px; border: none; background: none; font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 500; color: var(--muted); cursor: pointer; border-radius: var(--radius-sm); transition: all .2s; position: relative; }
.tab.active { color: var(--sage-dark); background: var(--sage-pale); }
.tab-indicator { position: absolute; bottom: 0; left: 50%; transform: translateX(-50%); width: 20px; height: 2px; background: var(--sage); border-radius: 2px; display: none; }
.tab.active .tab-indicator { display: block; }

/* ── PAGE SECTIONS ── */
.page { display: none; animation: fadeIn .3s ease; }
.page.active { display: block; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

/* ── SECTION HEADERS ── */
.section-label { font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: var(--muted); margin: 20px 0 10px; }

/* ── CALORIE RING ── */
.summary-card {
  background: var(--white); border-radius: var(--radius-lg); padding: 20px; margin-top: 10px;
  box-shadow: var(--shadow); display: flex; align-items: center; gap: 20px;
}
.ring-container { position: relative; width: 110px; height: 110px; flex-shrink: 0; }
.ring-svg { width: 110px; height: 110px; transform: rotate(-90deg); }
.ring-track { fill: none; stroke: var(--pebble); stroke-width: 8; }
.ring-fill { fill: none; stroke-width: 8; stroke-linecap: round; transition: stroke-dashoffset 1s cubic-bezier(0.4,0,0.2,1); }
.ring-center { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); text-align: center; }
.ring-cal { font-family: 'DM Serif Display', serif; font-size: 22px; color: var(--ink); line-height: 1; }
.ring-label { font-size: 9px; font-weight: 600; letter-spacing: 0.8px; text-transform: uppercase; color: var(--muted); margin-top: 2px; }
.summary-details { flex: 1; }
.summary-title { font-family: 'DM Serif Display', serif; font-size: 16px; color: var(--ink); margin-bottom: 6px; }
.cal-remaining { font-size: 12px; color: var(--muted); margin-bottom: 12px; }
.cal-remaining strong { color: var(--sage-dark); }

/* Macro mini bars */
.macro-bars { display: flex; flex-direction: column; gap: 7px; }
.macro-row { display: flex; align-items: center; gap: 8px; }
.macro-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }
.macro-name { font-size: 11px; color: var(--muted); width: 14px; flex-shrink: 0; }
.macro-bar-track { flex: 1; height: 5px; background: var(--pebble); border-radius: 3px; overflow: hidden; }
.macro-bar-fill { height: 100%; border-radius: 3px; transition: width 1s cubic-bezier(0.4,0,0.2,1); }
.macro-val { font-size: 11px; font-weight: 600; color: var(--ink-soft); width: 32px; text-align: right; }

/* ── WATER WIDGET ── */
.water-card {
  background: linear-gradient(135deg, #e8f4fa 0%, #d0eaf6 100%);
  border-radius: var(--radius); padding: 16px 18px; margin-top: 12px;
  display: flex; align-items: center; gap: 14px; box-shadow: var(--shadow-sm);
}
.water-icon { font-size: 26px; }
.water-info { flex: 1; }
.water-title { font-size: 13px; font-weight: 600; color: #2c6b8a; margin-bottom: 4px; }
.water-glasses { display: flex; gap: 5px; flex-wrap: wrap; margin-top: 6px; }
.glass-btn {
  width: 28px; height: 28px; border-radius: 8px; border: 2px solid rgba(107,181,214,0.3);
  background: transparent; cursor: pointer; transition: all .15s; display: flex; align-items: center; justify-content: center; font-size: 14px;
}
.glass-btn.filled { background: var(--water-color); border-color: var(--water-color); }
.glass-btn:hover:not(.filled) { border-color: var(--water-color); background: rgba(107,181,214,0.15); }
.water-ml { font-size: 20px; font-weight: 700; color: #2c6b8a; font-family: 'DM Serif Display', serif; }
.water-target { font-size: 11px; color: #5a9ab8; margin-left: 2px; }

/* ── VITAMIN SECTION ── */
.vitamins-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 0; }
.vitamin-pill {
  background: var(--white); border-radius: var(--radius-sm); padding: 12px 14px;
  box-shadow: var(--shadow-sm); display: flex; flex-direction: column; gap: 6px;
}
.vitamin-header { display: flex; justify-content: space-between; align-items: center; }
.vitamin-name { font-size: 12px; font-weight: 600; color: var(--ink-soft); }
.vitamin-pct { font-size: 12px; font-weight: 700; }
.vitamin-bar-track { height: 4px; background: var(--pebble); border-radius: 2px; overflow: hidden; }
.vitamin-bar-fill { height: 100%; border-radius: 2px; transition: width 1s ease; }
.vitamin-amounts { font-size: 10px; color: var(--muted); }

/* ── MEAL SECTIONS ── */
.meal-block { background: var(--white); border-radius: var(--radius); margin-top: 12px; overflow: hidden; box-shadow: var(--shadow-sm); }
.meal-header { padding: 14px 16px; display: flex; align-items: center; justify-content: space-between; cursor: pointer; user-select: none; }
.meal-header-left { display: flex; align-items: center; gap: 10px; }
.meal-emoji { font-size: 18px; }
.meal-name { font-size: 14px; font-weight: 600; color: var(--ink); }
.meal-cal-badge { font-size: 11px; color: var(--muted); background: var(--stone); padding: 3px 8px; border-radius: 20px; }
.meal-chevron { color: var(--muted); font-size: 12px; transition: transform .2s; }
.meal-block.expanded .meal-chevron { transform: rotate(180deg); }
.meal-body { display: none; border-top: 1px solid var(--pebble); }
.meal-block.expanded .meal-body { display: block; }
.food-item { display: flex; align-items: center; padding: 11px 16px; gap: 10px; border-bottom: 1px solid var(--stone); }
.food-item:last-child { border-bottom: none; }
.food-item-info { flex: 1; }
.food-item-name { font-size: 13px; font-weight: 500; color: var(--ink); }
.food-item-meta { font-size: 11px; color: var(--muted); margin-top: 2px; }
.food-item-cal { font-size: 13px; font-weight: 600; color: var(--ink-soft); }
.food-item-del { width: 24px; height: 24px; border-radius: 50%; border: none; background: none; color: var(--muted); cursor: pointer; font-size: 14px; display: flex; align-items: center; justify-content: center; transition: all .15s; }
.food-item-del:hover { background: #fde8e8; color: var(--danger); }
.add-food-btn {
  width: 100%; padding: 11px 16px; border: none; background: var(--sage-pale); color: var(--sage-dark);
  font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 500; cursor: pointer;
  display: flex; align-items: center; justify-content: center; gap: 6px; transition: all .15s;
}
.add-food-btn:hover { background: var(--mint); }

/* ── LIBRARY PAGE ── */
.library-search { display: flex; gap: 8px; margin-top: 10px; }
.search-input {
  flex: 1; padding: 10px 14px; border-radius: var(--radius-sm); border: 2px solid var(--pebble);
  font-family: 'DM Sans', sans-serif; font-size: 14px; background: var(--white); outline: none;
  transition: border-color .2s;
}
.search-input:focus { border-color: var(--sage); }
.primary-btn {
  padding: 10px 16px; background: var(--sage); color: white; border: none; border-radius: var(--radius-sm);
  font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 600; cursor: pointer; transition: all .2s; white-space: nowrap;
}
.primary-btn:hover { background: var(--sage-dark); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(122,171,138,0.4); }

.food-cards { display: flex; flex-direction: column; gap: 8px; margin-top: 10px; }
.food-card {
  background: var(--white); border-radius: var(--radius-sm); padding: 14px 16px;
  box-shadow: var(--shadow-sm); display: flex; align-items: center; gap: 12px;
  cursor: pointer; transition: all .2s; border: 2px solid transparent;
}
.food-card:hover { border-color: var(--sage-light); transform: translateX(2px); }
.food-card-emoji { font-size: 24px; }
.food-card-info { flex: 1; }
.food-card-name { font-size: 14px; font-weight: 600; color: var(--ink); }
.food-card-macros { font-size: 11px; color: var(--muted); margin-top: 3px; }
.macro-chips { display: flex; gap: 4px; margin-top: 5px; }
.macro-chip { font-size: 10px; font-weight: 600; padding: 2px 7px; border-radius: 20px; }
.chip-p { background: #dbedf8; color: #3a7ab8; }
.chip-f { background: #fef3d8; color: #b8892a; }
.chip-c { background: #f5e8f9; color: #9a50a8; }
.food-card-cal { font-size: 15px; font-weight: 700; color: var(--ink); font-family: 'DM Serif Display', serif; }
.food-card-cal-unit { font-size: 10px; color: var(--muted); }

/* ── ANALYTICS PAGE ── */
.analytics-header { display: flex; justify-content: space-between; align-items: center; margin-top: 14px; margin-bottom: 4px; }
.analytics-title { font-family: 'DM Serif Display', serif; font-size: 18px; color: var(--ink); }
.period-toggle { display: flex; background: var(--stone); border-radius: 8px; padding: 3px; gap: 2px; }
.period-btn { padding: 5px 10px; border-radius: 6px; border: none; background: none; font-size: 12px; font-weight: 500; color: var(--muted); cursor: pointer; transition: all .15s; }
.period-btn.active { background: var(--white); color: var(--ink); box-shadow: var(--shadow-sm); }

.chart-card { background: var(--white); border-radius: var(--radius); padding: 16px; margin-top: 12px; box-shadow: var(--shadow-sm); }
.chart-title { font-size: 13px; font-weight: 600; color: var(--ink-soft); margin-bottom: 12px; }
.chart-area { position: relative; height: 120px; }
canvas { width: 100% !important; }

.insights-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 12px; }
.insight-card { background: var(--white); border-radius: var(--radius-sm); padding: 14px; box-shadow: var(--shadow-sm); }
.insight-val { font-family: 'DM Serif Display', serif; font-size: 22px; color: var(--ink); }
.insight-label { font-size: 11px; color: var(--muted); margin-top: 2px; }
.insight-trend { font-size: 11px; font-weight: 600; margin-top: 4px; }
.trend-up { color: var(--sage); }
.trend-down { color: var(--warn); }

/* ── MODAL ── */
.modal-overlay {
  position: fixed; inset: 0; background: rgba(26,31,28,0.45); z-index: 200;
  display: none; align-items: flex-end; justify-content: center;
  backdrop-filter: blur(4px);
}
.modal-overlay.open { display: flex; animation: overlayIn .25s ease; }
@keyframes overlayIn { from { opacity: 0; } to { opacity: 1; } }

.modal {
  background: var(--white); border-radius: var(--radius-lg) var(--radius-lg) 0 0;
  width: 100%; max-width: 480px; max-height: 90vh; overflow-y: auto;
  padding: 0 0 40px; animation: slideUp .3s cubic-bezier(0.34,1.56,0.64,1);
}
@keyframes slideUp { from { transform: translateY(60px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }

.modal-handle { width: 40px; height: 4px; background: var(--pebble); border-radius: 2px; margin: 12px auto 0; }
.modal-header { padding: 16px 20px 4px; display: flex; align-items: center; justify-content: space-between; }
.modal-title { font-family: 'DM Serif Display', serif; font-size: 20px; color: var(--ink); }
.modal-close { width: 32px; height: 32px; border-radius: 50%; border: none; background: var(--stone); color: var(--muted); cursor: pointer; font-size: 16px; display: flex; align-items: center; justify-content: center; transition: all .15s; }
.modal-close:hover { background: var(--pebble); }
.modal-body { padding: 16px 20px; display: flex; flex-direction: column; gap: 14px; }

.form-field { display: flex; flex-direction: column; gap: 5px; }
.form-label { font-size: 12px; font-weight: 600; color: var(--muted); letter-spacing: 0.5px; text-transform: uppercase; }
.form-input, .form-select {
  padding: 11px 14px; border: 2px solid var(--pebble); border-radius: var(--radius-sm);
  font-family: 'DM Sans', sans-serif; font-size: 14px; background: var(--white); outline: none;
  transition: border-color .2s; color: var(--ink);
}
.form-input:focus, .form-select:focus { border-color: var(--sage); }
.form-row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; }
.form-hint { font-size: 11px; color: var(--muted); }

.vitamin-entry { display: flex; gap: 8px; align-items: center; }
.vitamin-entry .form-select { flex: 1.5; }
.vitamin-entry .form-input { flex: 1; }
.remove-vit-btn { width: 28px; height: 28px; border-radius: 50%; border: none; background: var(--stone); color: var(--muted); cursor: pointer; font-size: 16px; flex-shrink: 0; display: flex; align-items: center; justify-content: center; transition: all .15s; }
.remove-vit-btn:hover { background: #fde8e8; color: var(--danger); }
.add-vit-btn { display: flex; align-items: center; gap: 6px; background: none; border: 2px dashed var(--pebble); color: var(--muted); border-radius: var(--radius-sm); padding: 9px 14px; font-family: 'DM Sans', sans-serif; font-size: 13px; cursor: pointer; transition: all .15s; width: 100%; }
.add-vit-btn:hover { border-color: var(--sage-light); color: var(--sage-dark); background: var(--sage-pale); }

.form-divider { height: 1px; background: var(--pebble); margin: 2px 0; }
.form-section-label { font-size: 11px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; color: var(--sage-dark); padding: 2px 0; }

.saved-items-scroll { display: flex; flex-direction: column; gap: 6px; max-height: 180px; overflow-y: auto; padding: 2px; }
.saved-item-btn {
  display: flex; align-items: center; gap: 10px; padding: 10px 12px;
  background: var(--stone); border: 2px solid transparent; border-radius: var(--radius-sm);
  cursor: pointer; text-align: left; width: 100%; transition: all .15s; font-family: 'DM Sans', sans-serif;
}
.saved-item-btn:hover, .saved-item-btn.selected { border-color: var(--sage); background: var(--sage-pale); }
.saved-item-emoji { font-size: 18px; }
.saved-item-name { font-size: 13px; font-weight: 500; color: var(--ink); flex: 1; }
.saved-item-info { font-size: 11px; color: var(--muted); }

.modal-footer { padding: 0 20px; display: flex; gap: 10px; }
.secondary-btn { flex: 1; padding: 13px; border: 2px solid var(--pebble); border-radius: var(--radius-sm); background: none; font-family: 'DM Sans', sans-serif; font-size: 14px; font-weight: 600; color: var(--muted); cursor: pointer; transition: all .15s; }
.secondary-btn:hover { border-color: var(--ink-soft); color: var(--ink-soft); }
.submit-btn { flex: 2; padding: 13px; background: var(--sage); border: none; border-radius: var(--radius-sm); font-family: 'DM Sans', sans-serif; font-size: 14px; font-weight: 700; color: white; cursor: pointer; transition: all .2s; }
.submit-btn:hover { background: var(--sage-dark); box-shadow: 0 4px 16px rgba(122,171,138,0.4); }

/* Status colors */
.bar-green { background: var(--sage); }
.bar-warn { background: var(--warn); }
.bar-red { background: var(--danger); }
.pct-green { color: var(--sage-dark); }
.pct-warn { color: var(--warn); }

/* ── BOTTOM NAV ── */
.bottom-nav {
  position: fixed; bottom: 0; left: 50%; transform: translateX(-50%);
  width: 100%; max-width: 480px; background: var(--white);
  border-top: 1px solid var(--pebble); padding: 8px 0 20px;
  display: flex; z-index: 100; box-shadow: 0 -4px 20px rgba(26,31,28,0.06);
}
.nav-item { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 3px; cursor: pointer; padding: 4px 0; transition: all .2s; border: none; background: none; }
.nav-icon { font-size: 20px; transition: transform .2s; }
.nav-label { font-size: 10px; font-weight: 500; color: var(--muted); transition: color .2s; }
.nav-item.active .nav-label { color: var(--sage-dark); font-weight: 700; }
.nav-item.active .nav-icon { transform: translateY(-2px); }

/* toast */
.toast {
  position: fixed; top: 20px; left: 50%; transform: translateX(-50%) translateY(-80px);
  background: var(--ink); color: white; padding: 10px 20px; border-radius: 40px;
  font-size: 13px; font-weight: 500; z-index: 999; transition: transform .3s cubic-bezier(0.34,1.56,0.64,1);
  white-space: nowrap;
}
.toast.show { transform: translateX(-50%) translateY(0); }

/* empty state */
.empty-state { text-align: center; padding: 30px 20px; color: var(--muted); }
.empty-state-icon { font-size: 36px; margin-bottom: 10px; }
.empty-state-text { font-size: 13px; }
</style>
</head>
<body>

<div class="app">
  <!-- TOP BAR -->
  <div class="topbar">
    <div class="topbar-header">
      <div class="app-logo">Nou<span>ri</span></div>
      <div class="topbar-actions">
        <button class="icon-btn" onclick="openModal('new-food')">＋</button>
        <button class="icon-btn" onclick="openModal('settings')">⚙</button>
      </div>
    </div>
    <div class="calendar-strip" id="calendarStrip"></div>
  </div>

  <!-- TABS -->
  <div class="main">
    <div class="tabs">
      <button class="tab active" data-tab="dashboard">Today<div class="tab-indicator"></div></button>
      <button class="tab" data-tab="library">Library<div class="tab-indicator"></div></button>
      <button class="tab" data-tab="analytics">Analytics<div class="tab-indicator"></div></button>
    </div>

    <!-- ── DASHBOARD PAGE ── -->
    <div class="page active" id="page-dashboard">
      <!-- Calorie ring -->
      <p class="section-label">Daily Overview</p>
      <div class="summary-card">
        <div class="ring-container">
          <svg class="ring-svg" viewBox="0 0 110 110">
            <circle class="ring-track" cx="55" cy="55" r="45"/>
            <circle class="ring-fill" id="calRing" cx="55" cy="55" r="45"
              stroke="var(--sage)" stroke-dasharray="283" stroke-dashoffset="283"/>
          </svg>
          <div class="ring-center">
            <div class="ring-cal" id="calConsumed">0</div>
            <div class="ring-label">kcal</div>
          </div>
        </div>
        <div class="summary-details">
          <div class="summary-title">Calories</div>
          <div class="cal-remaining" id="calRemaining">Start logging your meals today</div>
          <div class="macro-bars">
            <div class="macro-row">
              <div class="macro-dot" style="background:var(--protein-color)"></div>
              <div class="macro-name" style="color:var(--protein-color);font-size:10px;font-weight:700;">P</div>
              <div class="macro-bar-track"><div class="macro-bar-fill" id="pBar" style="background:var(--protein-color);width:0%"></div></div>
              <div class="macro-val" id="pVal">0g</div>
            </div>
            <div class="macro-row">
              <div class="macro-dot" style="background:var(--fat-color)"></div>
              <div class="macro-name" style="color:var(--fat-color);font-size:10px;font-weight:700;">F</div>
              <div class="macro-bar-track"><div class="macro-bar-fill" id="fBar" style="background:var(--fat-color);width:0%"></div></div>
              <div class="macro-val" id="fVal">0g</div>
            </div>
            <div class="macro-row">
              <div class="macro-dot" style="background:var(--carb-color)"></div>
              <div class="macro-name" style="color:var(--carb-color);font-size:10px;font-weight:700;">C</div>
              <div class="macro-bar-track"><div class="macro-bar-fill" id="cBar" style="background:var(--carb-color);width:0%"></div></div>
              <div class="macro-val" id="cVal">0g</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Water -->
      <div class="water-card">
        <div class="water-icon">💧</div>
        <div class="water-info">
          <div class="water-title">Hydration</div>
          <div>
            <span class="water-ml" id="waterMl">0</span>
            <span class="water-target">/ 2000 ml</span>
          </div>
          <div class="water-glasses" id="waterGlasses"></div>
        </div>
      </div>

      <!-- Vitamins -->
      <p class="section-label">Vitamins & Micronutrients</p>
      <div class="vitamins-grid" id="vitaminsGrid"></div>

      <!-- Meals -->
      <p class="section-label">Meals</p>
      <div id="mealsContainer"></div>
    </div>

    <!-- ── LIBRARY PAGE ── -->
    <div class="page" id="page-library">
      <div class="library-search">
        <input class="search-input" id="librarySearch" placeholder="Search saved foods…" oninput="filterLibrary()">
        <button class="primary-btn" onclick="openModal('create-food')">＋ New</button>
      </div>
      <p class="section-label">Saved Foods</p>
      <div class="food-cards" id="libraryCards"></div>
    </div>

    <!-- ── ANALYTICS PAGE ── -->
    <div class="page" id="page-analytics">
      <div class="analytics-header">
        <div class="analytics-title">Insights</div>
        <div class="period-toggle">
          <button class="period-btn active" onclick="setPeriod(this,'week')">Week</button>
          <button class="period-btn" onclick="setPeriod(this,'month')">Month</button>
        </div>
      </div>

      <div class="insights-grid">
        <div class="insight-card">
          <div class="insight-val" id="avgCal">—</div>
          <div class="insight-label">Avg. Calories</div>
          <div class="insight-trend trend-up" id="calTrend"></div>
        </div>
        <div class="insight-card">
          <div class="insight-val" id="avgProtein">—</div>
          <div class="insight-label">Avg. Protein</div>
          <div class="insight-trend trend-up" id="proteinTrend"></div>
        </div>
        <div class="insight-card">
          <div class="insight-val" id="streakDays">—</div>
          <div class="insight-label">Logging Streak</div>
          <div class="insight-trend trend-up">🔥 Keep going!</div>
        </div>
        <div class="insight-card">
          <div class="insight-val" id="goalDays">—</div>
          <div class="insight-label">Goal Met Days</div>
          <div class="insight-trend trend-up" id="goalTrend"></div>
        </div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Calorie Intake — Last 7 Days</div>
        <div class="chart-area"><canvas id="calChart"></canvas></div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Macronutrient Split (avg)</div>
        <div style="display:flex;align-items:center;justify-content:center;gap:24px;padding:8px 0">
          <canvas id="pieChart" width="130" height="130"></canvas>
          <div id="pieLegend" style="display:flex;flex-direction:column;gap:8px;"></div>
        </div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Macronutrient Trend</div>
        <div class="chart-area"><canvas id="macroChart"></canvas></div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Vitamin Coverage</div>
        <div id="vitaminAnalytics" style="display:flex;flex-direction:column;gap:10px;margin-top:4px;"></div>
      </div>
    </div>
  </div>
</div>

<!-- ── BOTTOM NAV ── -->
<div class="bottom-nav">
  <button class="nav-item active" data-nav="dashboard">
    <span class="nav-icon">🏠</span><span class="nav-label">Today</span>
  </button>
  <button class="nav-item" data-nav="library">
    <span class="nav-icon">📚</span><span class="nav-label">Library</span>
  </button>
  <button class="nav-item" data-nav="analytics">
    <span class="nav-icon">📊</span><span class="nav-label">Analytics</span>
  </button>
</div>

<!-- ── ADD FOOD MODAL ── -->
<div class="modal-overlay" id="modal-new-food">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <div class="modal-title">Log Food</div>
      <button class="modal-close" onclick="closeModal('new-food')">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-field">
        <label class="form-label">Meal</label>
        <select class="form-select" id="nf-meal">
          <option value="breakfast">🌅 Breakfast</option>
          <option value="lunch">☀️ Lunch</option>
          <option value="dinner">🌙 Dinner</option>
          <option value="snacks">🍎 Snacks</option>
        </select>
      </div>

      <div class="form-divider"></div>
      <div class="form-section-label">From Saved Foods</div>
      <div class="saved-items-scroll" id="savedItemsList"></div>

      <div class="form-field" id="weightField" style="display:none">
        <label class="form-label">Weight (g)</label>
        <input class="form-input" type="number" id="nf-weight" placeholder="e.g. 80" oninput="recalcFromSaved()">
        <div class="form-hint" id="weightHint"></div>
      </div>

      <div class="form-divider"></div>
      <div class="form-section-label">Or Enter Manually</div>

      <div class="form-field">
        <label class="form-label">Food Name</label>
        <input class="form-input" type="text" id="nf-name" placeholder="e.g. Greek Yogurt">
      </div>
      <div class="form-field">
        <label class="form-label">Calories (kcal)</label>
        <input class="form-input" type="number" id="nf-cal" placeholder="e.g. 250">
      </div>
      <div class="form-field">
        <label class="form-label">Macros (g)</label>
        <div class="form-row">
          <div>
            <div style="font-size:11px;color:var(--protein-color);font-weight:600;margin-bottom:4px;">Protein</div>
            <input class="form-input" type="number" id="nf-p" placeholder="0">
          </div>
          <div>
            <div style="font-size:11px;color:var(--fat-color);font-weight:600;margin-bottom:4px;">Fat</div>
            <input class="form-input" type="number" id="nf-f" placeholder="0">
          </div>
          <div>
            <div style="font-size:11px;color:var(--carb-color);font-weight:600;margin-bottom:4px;">Carbs</div>
            <input class="form-input" type="number" id="nf-c" placeholder="0">
          </div>
        </div>
      </div>
      <div class="form-field">
        <label class="form-label">Vitamins (optional)</label>
        <div id="nf-vitamins"></div>
        <button class="add-vit-btn" onclick="addVitaminEntry('nf-vitamins')">＋ Add Vitamin</button>
      </div>
    </div>
    <div class="modal-footer">
      <button class="secondary-btn" onclick="closeModal('new-food')">Cancel</button>
      <button class="submit-btn" onclick="logFood()">Log Food</button>
    </div>
  </div>
</div>

<!-- ── CREATE FOOD MODAL ── -->
<div class="modal-overlay" id="modal-create-food">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <div class="modal-title">Save Food (per 100g)</div>
      <button class="modal-close" onclick="closeModal('create-food')">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-field">
        <label class="form-label">Food Name</label>
        <input class="form-input" type="text" id="cf-name" placeholder="e.g. Chicken Breast">
      </div>
      <div class="form-field">
        <label class="form-label">Emoji</label>
        <input class="form-input" type="text" id="cf-emoji" placeholder="🍗" maxlength="2">
      </div>
      <div class="form-field">
        <label class="form-label">Calories per 100g</label>
        <input class="form-input" type="number" id="cf-cal" placeholder="e.g. 165">
      </div>
      <div class="form-field">
        <label class="form-label">Macros per 100g</label>
        <div class="form-row">
          <div>
            <div style="font-size:11px;color:var(--protein-color);font-weight:600;margin-bottom:4px;">Protein</div>
            <input class="form-input" type="number" id="cf-p" placeholder="0">
          </div>
          <div>
            <div style="font-size:11px;color:var(--fat-color);font-weight:600;margin-bottom:4px;">Fat</div>
            <input class="form-input" type="number" id="cf-f" placeholder="0">
          </div>
          <div>
            <div style="font-size:11px;color:var(--carb-color);font-weight:600;margin-bottom:4px;">Carbs</div>
            <input class="form-input" type="number" id="cf-c" placeholder="0">
          </div>
        </div>
      </div>
      <div class="form-field">
        <label class="form-label">Vitamins per 100g (optional)</label>
        <div id="cf-vitamins"></div>
        <button class="add-vit-btn" onclick="addVitaminEntry('cf-vitamins')">＋ Add Vitamin</button>
      </div>
    </div>
    <div class="modal-footer">
      <button class="secondary-btn" onclick="closeModal('create-food')">Cancel</button>
      <button class="submit-btn" onclick="saveFood()">Save to Library</button>
    </div>
  </div>
</div>

<!-- ── SETTINGS MODAL ── -->
<div class="modal-overlay" id="modal-settings">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <div class="modal-title">Daily Goals</div>
      <button class="modal-close" onclick="closeModal('settings')">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-section-label">Calories & Macros</div>
      <div class="form-field">
        <label class="form-label">Calories (kcal)</label>
        <input class="form-input" type="number" id="sg-cal" placeholder="2000">
      </div>
      <div class="form-field">
        <label class="form-label">Macros (g/day)</label>
        <div class="form-row">
          <div>
            <div style="font-size:11px;color:var(--protein-color);font-weight:600;margin-bottom:4px;">Protein</div>
            <input class="form-input" type="number" id="sg-p" placeholder="150">
          </div>
          <div>
            <div style="font-size:11px;color:var(--fat-color);font-weight:600;margin-bottom:4px;">Fat</div>
            <input class="form-input" type="number" id="sg-f" placeholder="65">
          </div>
          <div>
            <div style="font-size:11px;color:var(--carb-color);font-weight:600;margin-bottom:4px;">Carbs</div>
            <input class="form-input" type="number" id="sg-c" placeholder="250">
          </div>
        </div>
      </div>
      <div class="form-field">
        <label class="form-label">Water (ml/day)</label>
        <input class="form-input" type="number" id="sg-water" placeholder="2000">
      </div>

      <div class="form-divider"></div>
      <div class="form-section-label">Vitamins & Minerals</div>
      <div id="sg-micros" style="display:flex;flex-direction:column;gap:10px;"></div>
    </div>
    <div class="modal-footer">
      <button class="secondary-btn" onclick="closeModal('settings')">Cancel</button>
      <button class="submit-btn" onclick="saveSettings()">Save Goals</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ── DATA ──
const DEFAULT_GOALS = { cal: 2000, p: 150, f: 65, c: 250, water: 2000 };
let GOALS = JSON.parse(localStorage.getItem('nouri_goals') || 'null') || { ...DEFAULT_GOALS };
function saveGoals() { localStorage.setItem('nouri_goals', JSON.stringify(GOALS)); }

const MICRONUTRIENT_DEFAULTS = {
  A: 900, D: 20, E: 15, K: 120, C: 90,
  B1: 1.2, B2: 1.3, B3: 16, B5: 5, B6: 1.7, B9: 400, B12: 2.4,
  Calcium: 1000, Iron: 18, Magnesium: 400, Potassium: 3500,
  Zinc: 11, Sodium: 2300, Iodine: 150
};
const MICRONUTRIENT_UNITS = {
  A: 'μg', D: 'μg', E: 'mg', K: 'μg', C: 'mg',
  B1: 'mg', B2: 'mg', B3: 'mg', B5: 'mg', B6: 'mg', B9: 'μg', B12: 'μg',
  Calcium: 'mg', Iron: 'mg', Magnesium: 'mg', Potassium: 'mg',
  Zinc: 'mg', Sodium: 'mg', Iodine: 'μg'
};
const MICRONUTRIENT_LABELS = {
  A: 'Vit. A', D: 'Vit. D', E: 'Vit. E', K: 'Vit. K', C: 'Vit. C',
  B1: 'Vit. B1', B2: 'Vit. B2', B3: 'Vit. B3', B5: 'Vit. B5',
  B6: 'Vit. B6', B9: 'Vit. B9', B12: 'Vit. B12',
  Calcium: 'Calcium', Iron: 'Iron', Magnesium: 'Magnesium',
  Potassium: 'Potassium', Zinc: 'Zinc', Sodium: 'Sodium', Iodine: 'Iodine'
};
let VITAMIN_GOALS = JSON.parse(localStorage.getItem('nouri_micros') || 'null') || { ...MICRONUTRIENT_DEFAULTS };
function saveMicroGoals() { localStorage.setItem('nouri_micros', JSON.stringify(VITAMIN_GOALS)); }

const VITAMIN_UNITS = MICRONUTRIENT_UNITS;
const VITAMIN_COLORS = [
  '#ff7e5f','#f9c74f','#90be6d','#4cc9f0','#7b2d8b','#f77f00',
  '#d62828','#023e8a','#2d6a4f','#e76f51','#457b9d','#a8dadc',
  '#6d6875','#b5838d','#e9c46a','#264653','#2a9d8f','#e9c46a','#f4a261'
];

const MEAL_CONFIG = [
  { id: 'breakfast', name: 'Breakfast', emoji: '🌅' },
  { id: 'lunch', name: 'Lunch', emoji: '☀️' },
  { id: 'dinner', name: 'Dinner', emoji: '🌙' },
  { id: 'snacks', name: 'Snacks', emoji: '🍎' },
];

// Seed library
let foodLibrary = JSON.parse(localStorage.getItem('nouri_lib') || 'null') || [
  { id: 'f1', name: 'Chicken Breast', emoji: '🍗', cal: 165, p: 31, f: 3.6, c: 0, vitamins: [{type:'B12',qty:0.3}] },
  { id: 'f2', name: 'Greek Yogurt', emoji: '🥛', cal: 59, p: 10, f: 0.4, c: 3.6, vitamins: [{type:'D',qty:1.2}] },
  { id: 'f3', name: 'Banana', emoji: '🍌', cal: 89, p: 1.1, f: 0.3, c: 23, vitamins: [{type:'B6',qty:0.4}] },
  { id: 'f4', name: 'Brown Rice', emoji: '🍚', cal: 112, p: 2.3, f: 0.9, c: 24, vitamins: [] },
  { id: 'f5', name: 'Broccoli', emoji: '🥦', cal: 34, p: 2.8, f: 0.4, c: 7, vitamins: [{type:'C',qty:89},{type:'K',qty:102}] },
  { id: 'f6', name: 'Salmon', emoji: '🐟', cal: 208, p: 20, f: 13, c: 0, vitamins: [{type:'D',qty:11},{type:'B12',qty:3.2}] },
  { id: 'f7', name: 'Oats', emoji: '🥣', cal: 389, p: 17, f: 7, c: 66, vitamins: [] },
  { id: 'f8', name: 'Egg', emoji: '🥚', cal: 155, p: 13, f: 11, c: 1.1, vitamins: [{type:'D',qty:2},{type:'B12',qty:1.1}] },
];

// Daily logs keyed by date string
let dailyLogs = JSON.parse(localStorage.getItem('nouri_logs') || '{}');
let waterLogs = JSON.parse(localStorage.getItem('nouri_water') || '{}');
let selectedDate = todayStr();
let selectedSavedFood = null;

function todayStr() { return new Date().toISOString().slice(0, 10); }
function saveLogs() { localStorage.setItem('nouri_logs', JSON.stringify(dailyLogs)); }
function saveLib() { localStorage.setItem('nouri_lib', JSON.stringify(foodLibrary)); }
function saveWater() { localStorage.setItem('nouri_water', JSON.stringify(waterLogs)); }

function getDay(d) {
  if (!dailyLogs[d]) dailyLogs[d] = { breakfast: [], lunch: [], dinner: [], snacks: [] };
  return dailyLogs[d];
}
function getWater(d) { return waterLogs[d] || 0; }

// ── CALENDAR STRIP ──
function buildCalendar() {
  const strip = document.getElementById('calendarStrip');
  strip.innerHTML = '';
  const today = new Date();
  const days = ['S','M','T','W','T','F','S'];
  for (let i = -3; i <= 3; i++) {
    const d = new Date(today); d.setDate(d.getDate() + i);
    const ds = d.toISOString().slice(0, 10);
    const el = document.createElement('div');
    el.className = 'cal-day' + (ds === selectedDate ? ' active' : '') + (i === 0 ? ' today' : '');
    el.innerHTML = `<span class="day-name">${days[d.getDay()]}</span><span class="day-num">${d.getDate()}</span>`;
    el.onclick = () => { selectedDate = ds; buildCalendar(); renderDashboard(); };
    strip.appendChild(el);
  }
}

// ── DASHBOARD ──
function renderDashboard() {
  const day = getDay(selectedDate);
  let totCal = 0, totP = 0, totF = 0, totC = 0;
  const vitTotals = {};
  Object.keys(VITAMIN_GOALS).forEach(v => vitTotals[v] = 0);

  MEAL_CONFIG.forEach(m => {
    day[m.id].forEach(item => {
      totCal += item.cal || 0; totP += item.p || 0; totF += item.f || 0; totC += item.c || 0;
      (item.vitamins || []).forEach(v => { vitTotals[v.type] = (vitTotals[v.type] || 0) + v.qty; });
    });
  });

  // Ring
  const pct = Math.min(totCal / GOALS.cal, 1);
  const circ = 2 * Math.PI * 45;
  const offset = circ * (1 - pct);
  const ring = document.getElementById('calRing');
  ring.style.strokeDashoffset = offset;
  ring.style.stroke = totCal > GOALS.cal ? 'var(--danger)' : totCal / GOALS.cal >= 0.9 ? 'var(--warn)' : 'var(--sage)';
  document.getElementById('calConsumed').textContent = Math.round(totCal);
  const rem = GOALS.cal - totCal;
  document.getElementById('calRemaining').innerHTML =
    rem > 0 ? `<strong>${Math.round(rem)} kcal</strong> remaining` :
    totCal === 0 ? 'Start logging your meals today' :
    `<span style="color:var(--danger)">+${Math.round(-rem)} kcal over goal</span>`;

  // Macro bars
  function setBar(id, val, goal, valId, unit='g') {
    const pct = Math.min(val / goal * 100, 100);
    const bar = document.getElementById(id);
    bar.style.width = pct + '%';
    bar.style.background = pct >= 100 ? 'var(--sage)' : pct > 80 ? 'var(--warn)' : bar.style.background;
    document.getElementById(valId).textContent = Math.round(val) + unit;
  }
  setBar('pBar', totP, GOALS.p, 'pVal'); setBar('fBar', totF, GOALS.f, 'fVal'); setBar('cBar', totC, GOALS.c, 'cVal');

  // Vitamins
  const grid = document.getElementById('vitaminsGrid');
  grid.innerHTML = '';
  Object.entries(VITAMIN_GOALS).forEach(([name, goal], i) => {
    const val = vitTotals[name] || 0;
    const pct = Math.min(val / goal * 100, 100);
    const color = VITAMIN_COLORS[i % VITAMIN_COLORS.length];
    const unit = VITAMIN_UNITS[name];
    const isGood = pct >= 100;
    const pill = document.createElement('div');
    pill.className = 'vitamin-pill';
    pill.innerHTML = `
      <div class="vitamin-header">
        <span class="vitamin-name">${MICRONUTRIENT_LABELS[name] || name}</span>
        <span class="vitamin-pct" style="color:${isGood ? 'var(--sage-dark)' : pct > 70 ? 'var(--warn)' : 'var(--muted)'}">${Math.round(pct)}%</span>
      </div>
      <div class="vitamin-bar-track"><div class="vitamin-bar-fill" style="width:${pct}%;background:${isGood ? 'var(--sage)' : color}"></div></div>
      <div class="vitamin-amounts">${val.toFixed(1)} / ${goal}${unit}</div>`;
    grid.appendChild(pill);
  });

  // Water
  const water = getWater(selectedDate);
  document.getElementById('waterMl').textContent = water;
  const glasses = document.getElementById('waterGlasses');
  glasses.innerHTML = '';
  const total = 8;
  const filled = Math.min(Math.floor(water / 250), total);
  for (let i = 0; i < total; i++) {
    const btn = document.createElement('button');
    btn.className = 'glass-btn' + (i < filled ? ' filled' : '');
    btn.textContent = i < filled ? '💧' : '○';
    btn.onclick = () => addWater(selectedDate);
    glasses.appendChild(btn);
  }

  // Meals
  const container = document.getElementById('mealsContainer');
  container.innerHTML = '';
  MEAL_CONFIG.forEach(m => {
    const items = day[m.id];
    const mealCal = items.reduce((s, i) => s + (i.cal || 0), 0);
    const block = document.createElement('div');
    block.className = 'meal-block expanded';
    block.id = 'meal-' + m.id;

    const itemsHtml = items.length === 0 ? '' : items.map((item, idx) => `
      <div class="food-item">
        <div class="food-item-info">
          <div class="food-item-name">${item.name}</div>
          <div class="food-item-meta">P ${item.p}g · F ${item.f}g · C ${item.c}g${item.weight ? ' · ' + item.weight + 'g' : ''}</div>
        </div>
        <div class="food-item-cal">${Math.round(item.cal)} kcal</div>
        <button class="food-item-del" onclick="deleteItem('${m.id}',${idx})">✕</button>
      </div>`).join('');

    block.innerHTML = `
      <div class="meal-header" onclick="this.parentElement.classList.toggle('expanded')">
        <div class="meal-header-left">
          <span class="meal-emoji">${m.emoji}</span>
          <span class="meal-name">${m.name}</span>
        </div>
        <div style="display:flex;align-items:center;gap:8px">
          <span class="meal-cal-badge">${Math.round(mealCal)} kcal</span>
          <span class="meal-chevron">▼</span>
        </div>
      </div>
      <div class="meal-body">
        ${itemsHtml || '<div class="empty-state" style="padding:14px"><div style="font-size:11px;color:var(--muted)">No items yet</div></div>'}
        <button class="add-food-btn" onclick="openModal('new-food','${m.id}')">＋ Add Food</button>
      </div>`;
    container.appendChild(block);
  });
}

function deleteItem(mealId, idx) {
  const day = getDay(selectedDate);
  day[mealId].splice(idx, 1);
  saveLogs(); renderDashboard();
}

function addWater(d) {
  waterLogs[d] = (waterLogs[d] || 0) + 250;
  if (waterLogs[d] > GOALS.water) waterLogs[d] = GOALS.water;
  saveWater(); renderDashboard();
  showToast('💧 Glass added!');
}

// ── LIBRARY ──
function renderLibrary() {
  const q = (document.getElementById('librarySearch')?.value || '').toLowerCase();
  const filtered = foodLibrary.filter(f => f.name.toLowerCase().includes(q));
  const container = document.getElementById('libraryCards');
  if (!container) return;
  container.innerHTML = filtered.length === 0
    ? '<div class="empty-state"><div class="empty-state-icon">🔍</div><div class="empty-state-text">No foods found</div></div>'
    : filtered.map(f => `
      <div class="food-card" onclick="quickAddFromLib('${f.id}')">
        <div class="food-card-emoji">${f.emoji}</div>
        <div class="food-card-info">
          <div class="food-card-name">${f.name}</div>
          <div class="macro-chips">
            <span class="macro-chip chip-p">P ${f.p}g</span>
            <span class="macro-chip chip-f">F ${f.f}g</span>
            <span class="macro-chip chip-c">C ${f.c}g</span>
          </div>
        </div>
        <div>
          <div class="food-card-cal">${f.cal}</div>
          <div class="food-card-cal-unit">kcal/100g</div>
        </div>
      </div>`).join('');
}
function filterLibrary() { renderLibrary(); }

function quickAddFromLib(id) {
  const food = foodLibrary.find(f => f.id === id);
  if (!food) return;
  openModal('new-food');
  setTimeout(() => selectSavedFood(id), 100);
}

// ── MODAL LOGIC ──
function openModal(name, mealPreset) {
  const overlay = document.getElementById('modal-' + name);
  if (!overlay) return;
  overlay.classList.add('open');
  document.body.style.overflow = 'hidden';

  if (name === 'new-food') {
    populateSavedList();
    selectedSavedFood = null;
    document.getElementById('weightField').style.display = 'none';
    if (mealPreset) document.getElementById('nf-meal').value = mealPreset;
    clearNFForm();
  }

  if (name === 'settings') {
    document.getElementById('sg-cal').value = GOALS.cal;
    document.getElementById('sg-p').value = GOALS.p;
    document.getElementById('sg-f').value = GOALS.f;
    document.getElementById('sg-c').value = GOALS.c;
    document.getElementById('sg-water').value = GOALS.water;
    const microsDiv = document.getElementById('sg-micros');
    microsDiv.innerHTML = '';
    Object.entries(VITAMIN_GOALS).forEach(([key, val]) => {
      const unit = MICRONUTRIENT_UNITS[key];
      const row = document.createElement('div');
      row.style.cssText = 'display:flex;align-items:center;gap:10px;';
      row.innerHTML = `
        <label style="font-size:13px;font-weight:500;color:var(--ink-soft);flex:1;min-width:80px">${MICRONUTRIENT_LABELS[key]}</label>
        <input class="form-input" type="number" data-key="${key}" value="${val}" style="width:90px;text-align:right">
        <span style="font-size:12px;color:var(--muted);width:24px">${unit}</span>`;
      microsDiv.appendChild(row);
    });
  }
}
function closeModal(name) {
  document.getElementById('modal-' + name).classList.remove('open');
  document.body.style.overflow = '';
}

function saveSettings() {
  GOALS.cal = parseFloat(document.getElementById('sg-cal').value) || GOALS.cal;
  GOALS.p = parseFloat(document.getElementById('sg-p').value) || GOALS.p;
  GOALS.f = parseFloat(document.getElementById('sg-f').value) || GOALS.f;
  GOALS.c = parseFloat(document.getElementById('sg-c').value) || GOALS.c;
  GOALS.water = parseFloat(document.getElementById('sg-water').value) || GOALS.water;
  document.querySelectorAll('#sg-micros input[data-key]').forEach(input => {
    const key = input.dataset.key;
    const val = parseFloat(input.value);
    if (key && val > 0) VITAMIN_GOALS[key] = val;
  });
  saveGoals(); saveMicroGoals();
  closeModal('settings');
  renderDashboard();
  showToast('✓ Goals saved!');
}

function clearNFForm() {
  ['nf-name','nf-cal','nf-p','nf-f','nf-c'].forEach(id => {
    const el = document.getElementById(id); if (el) el.value = '';
  });
  document.getElementById('nf-vitamins').innerHTML = '';
  document.getElementById('nf-weight').value = '';
  selectedSavedFood = null;
}

function populateSavedList() {
  const list = document.getElementById('savedItemsList');
  list.innerHTML = foodLibrary.map(f => `
    <button class="saved-item-btn" id="si-${f.id}" onclick="selectSavedFood('${f.id}')">
      <span class="saved-item-emoji">${f.emoji}</span>
      <span class="saved-item-name">${f.name}</span>
      <span class="saved-item-info">${f.cal} kcal/100g</span>
    </button>`).join('');
}

function selectSavedFood(id) {
  selectedSavedFood = foodLibrary.find(f => f.id === id);
  if (!selectedSavedFood) return;
  document.querySelectorAll('.saved-item-btn').forEach(b => b.classList.remove('selected'));
  const btn = document.getElementById('si-' + id);
  if (btn) btn.classList.add('selected');

  document.getElementById('weightField').style.display = 'block';
  document.getElementById('nf-name').value = selectedSavedFood.name;
  document.getElementById('nf-weight').value = 100;
  recalcFromSaved();
}

function recalcFromSaved() {
  if (!selectedSavedFood) return;
  const w = parseFloat(document.getElementById('nf-weight').value) || 0;
  const scale = w / 100;
  document.getElementById('nf-cal').value = (selectedSavedFood.cal * scale).toFixed(1);
  document.getElementById('nf-p').value = (selectedSavedFood.p * scale).toFixed(1);
  document.getElementById('nf-f').value = (selectedSavedFood.f * scale).toFixed(1);
  document.getElementById('nf-c').value = (selectedSavedFood.c * scale).toFixed(1);
  document.getElementById('weightHint').textContent =
    `Auto-calculated from ${w}g (baseline: ${selectedSavedFood.cal} kcal / 100g)`;

  const vitDiv = document.getElementById('nf-vitamins');
  vitDiv.innerHTML = '';
  (selectedSavedFood.vitamins || []).forEach(v => {
    addVitaminEntry('nf-vitamins', v.type, (v.qty * scale).toFixed(2));
  });
}

function addVitaminEntry(containerId, typeVal = '', qtyVal = '') {
  const container = document.getElementById(containerId);
  const entry = document.createElement('div');
  entry.className = 'vitamin-entry';
  entry.style.marginBottom = '8px';
  entry.innerHTML = `
    <select class="form-select">
      ${Object.keys(VITAMIN_GOALS).map(v => `<option value="${v}" ${v === typeVal ? 'selected' : ''}>${MICRONUTRIENT_LABELS[v]}</option>`).join('')}
    </select>
    <input class="form-input" type="number" placeholder="qty" value="${qtyVal}" style="width:80px">
    <button class="remove-vit-btn" onclick="this.parentElement.remove()">✕</button>`;
  container.appendChild(entry);
}

function logFood() {
  const name = document.getElementById('nf-name').value.trim();
  const cal = parseFloat(document.getElementById('nf-cal').value) || 0;
  const p = parseFloat(document.getElementById('nf-p').value) || 0;
  const f = parseFloat(document.getElementById('nf-f').value) || 0;
  const c = parseFloat(document.getElementById('nf-c').value) || 0;
  const meal = document.getElementById('nf-meal').value;
  const weight = parseFloat(document.getElementById('nf-weight').value) || null;

  if (!name) { showToast('⚠️ Please enter a food name'); return; }

  const vitamins = [];
  document.querySelectorAll('#nf-vitamins .vitamin-entry').forEach(entry => {
    const type = entry.querySelector('select').value;
    const qty = parseFloat(entry.querySelector('input').value) || 0;
    if (qty > 0) vitamins.push({ type, qty });
  });

  const day = getDay(selectedDate);
  day[meal].push({ name, cal, p, f, c, vitamins, weight });
  saveLogs(); renderDashboard(); closeModal('new-food');
  showToast(`✓ ${name} logged!`);
}

function saveFood() {
  const name = document.getElementById('cf-name').value.trim();
  const emoji = document.getElementById('cf-emoji').value || '🍽';
  const cal = parseFloat(document.getElementById('cf-cal').value) || 0;
  const p = parseFloat(document.getElementById('cf-p').value) || 0;
  const f = parseFloat(document.getElementById('cf-f').value) || 0;
  const c = parseFloat(document.getElementById('cf-c').value) || 0;
  if (!name) { showToast('⚠️ Please enter a food name'); return; }

  const vitamins = [];
  document.querySelectorAll('#cf-vitamins .vitamin-entry').forEach(entry => {
    const type = entry.querySelector('select').value;
    const qty = parseFloat(entry.querySelector('input').value) || 0;
    if (qty > 0) vitamins.push({ type, qty });
  });

  foodLibrary.push({ id: 'f' + Date.now(), name, emoji, cal, p, f, c, vitamins });
  saveLib(); renderLibrary(); closeModal('create-food');
  ['cf-name','cf-emoji','cf-cal','cf-p','cf-f','cf-c'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('cf-vitamins').innerHTML = '';
  showToast(`✓ ${name} saved to library!`);
}

// ── ANALYTICS ──
let analyticsPeriod = 'week';
function setPeriod(btn, period) {
  document.querySelectorAll('.period-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  analyticsPeriod = period;
  renderAnalytics();
}

function renderAnalytics() {
  const days = analyticsPeriod === 'week' ? 7 : 30;
  const labels = [], calData = [], pData = [], fData = [], cData = [];
  let totalCal = 0, totalP = 0, goalMetDays = 0, loggedDays = 0;

  for (let i = days - 1; i >= 0; i--) {
    const d = new Date(); d.setDate(d.getDate() - i);
    const ds = d.toISOString().slice(0, 10);
    const day = dailyLogs[ds];
    let cal = 0, p = 0, f = 0, c = 0;
    if (day) {
      MEAL_CONFIG.forEach(m => {
        (day[m.id] || []).forEach(item => { cal += item.cal || 0; p += item.p || 0; f += item.f || 0; c += item.c || 0; });
      });
      if (cal > 0) { loggedDays++; totalCal += cal; totalP += p; }
      if (cal >= GOALS.cal * 0.8 && cal <= GOALS.cal * 1.1) goalMetDays++;
    }
    labels.push(d.getDate() + '/' + (d.getMonth() + 1));
    calData.push(Math.round(cal)); pData.push(Math.round(p)); fData.push(Math.round(f)); cData.push(Math.round(c));
  }

  const avgCal = loggedDays ? Math.round(totalCal / loggedDays) : 0;
  const avgP = loggedDays ? Math.round(totalP / loggedDays) : 0;
  document.getElementById('avgCal').textContent = avgCal || '—';
  document.getElementById('avgProtein').textContent = avgP ? avgP + 'g' : '—';
  document.getElementById('streakDays').textContent = loggedDays;
  document.getElementById('goalDays').textContent = goalMetDays;
  document.getElementById('calTrend').textContent = avgCal ? (avgCal >= GOALS.cal * 0.9 ? '✓ On track' : '⚡ Below target') : '';
  document.getElementById('proteinTrend').textContent = avgP ? (avgP >= GOALS.p * 0.8 ? '✓ Good' : '↑ Needs more') : '';
  document.getElementById('goalTrend').textContent = goalMetDays > 0 ? `out of ${days} days` : '';

  drawCalChart(labels, calData);
  drawPieChart(pData, fData, cData);
  drawMacroChart(labels, pData, fData, cData);
  renderVitaminAnalytics();
}

function drawPieChart(pData, fData, cData) {
  const canvas = document.getElementById('pieChart');
  const ctx = canvas.getContext('2d');
  const size = 130;
  canvas.width = size; canvas.height = size;
  ctx.clearRect(0, 0, size, size);

  const avgP = pData.reduce((a, b) => a + b, 0) / pData.length || 0;
  const avgF = fData.reduce((a, b) => a + b, 0) / fData.length || 0;
  const avgC = cData.reduce((a, b) => a + b, 0) / cData.length || 0;

  // convert to calories for proportional display
  const calP = avgP * 4, calF = avgF * 9, calC = avgC * 4;
  const total = calP + calF + calC;

  const legend = document.getElementById('pieLegend');
  legend.innerHTML = '';

  if (total === 0) {
    ctx.fillStyle = 'var(--pebble)';
    ctx.beginPath(); ctx.arc(size/2, size/2, size/2 - 10, 0, Math.PI * 2); ctx.fill();
    ctx.fillStyle = 'var(--muted)'; ctx.font = '10px DM Sans'; ctx.textAlign = 'center';
    ctx.fillText('No data', size/2, size/2 + 4);
    return;
  }

  const slices = [
    { label: 'Protein', val: calP, avg: avgP, unit: 'g', color: '#5b9bd5' },
    { label: 'Fat', val: calF, avg: avgF, unit: 'g', color: '#e8b84b' },
    { label: 'Carbs', val: calC, avg: avgC, unit: 'g', color: '#c97dd0' },
  ];

  const cx = size / 2, cy = size / 2, r = size / 2 - 8, innerR = r * 0.52;
  let startAngle = -Math.PI / 2;

  slices.forEach(s => {
    const sweep = (s.val / total) * Math.PI * 2;
    ctx.beginPath();
    ctx.moveTo(cx, cy);
    ctx.arc(cx, cy, r, startAngle, startAngle + sweep);
    ctx.closePath();
    ctx.fillStyle = s.color;
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
    startAngle += sweep;
  });

  // donut hole
  ctx.beginPath(); ctx.arc(cx, cy, innerR, 0, Math.PI * 2);
  ctx.fillStyle = '#fff'; ctx.fill();

  // center text
  ctx.fillStyle = 'var(--ink)'; ctx.font = `bold 13px DM Sans`; ctx.textAlign = 'center';
  ctx.fillText(Math.round(total), cx, cy - 2);
  ctx.fillStyle = 'var(--muted)'; ctx.font = '9px DM Sans';
  ctx.fillText('avg kcal', cx, cy + 10);

  // legend
  slices.forEach(s => {
    const pct = total > 0 ? Math.round(s.val / total * 100) : 0;
    const item = document.createElement('div');
    item.style.cssText = 'display:flex;align-items:center;gap:6px;';
    item.innerHTML = `
      <div style="width:10px;height:10px;border-radius:3px;background:${s.color};flex-shrink:0"></div>
      <div>
        <div style="font-size:12px;font-weight:600;color:var(--ink-soft)">${s.label}</div>
        <div style="font-size:11px;color:var(--muted)">${Math.round(s.avg)}${s.unit} · ${pct}%</div>
      </div>`;
    legend.appendChild(item);
  });
}

function drawCalChart(labels, data) {
  const canvas = document.getElementById('calChart');
  canvas.height = 120;
  const ctx = canvas.getContext('2d');
  const w = canvas.parentElement.offsetWidth || 320;
  canvas.width = w;
  ctx.clearRect(0, 0, w, 120);

  const pad = { l: 30, r: 10, t: 10, b: 24 };
  const cw = w - pad.l - pad.r, ch = 120 - pad.t - pad.b;
  const max = Math.max(...data, GOALS.cal) * 1.1 || 2200;
  const n = data.length;

  // Goal line
  const gy = pad.t + ch * (1 - GOALS.cal / max);
  ctx.strokeStyle = 'rgba(122,171,138,0.3)'; ctx.lineWidth = 1; ctx.setLineDash([4, 4]);
  ctx.beginPath(); ctx.moveTo(pad.l, gy); ctx.lineTo(pad.l + cw, gy); ctx.stroke();
  ctx.setLineDash([]);

  // Grid
  ctx.strokeStyle = 'rgba(0,0,0,0.05)'; ctx.lineWidth = 1;
  [0.25, 0.5, 0.75, 1].forEach(f => {
    const y = pad.t + ch * f;
    ctx.beginPath(); ctx.moveTo(pad.l, y); ctx.lineTo(pad.l + cw, y); ctx.stroke();
  });

  // Gradient fill
  const grad = ctx.createLinearGradient(0, pad.t, 0, pad.t + ch);
  grad.addColorStop(0, 'rgba(122,171,138,0.25)'); grad.addColorStop(1, 'rgba(122,171,138,0)');
  ctx.beginPath();
  data.forEach((v, i) => {
    const x = pad.l + (i / (n - 1)) * cw;
    const y = pad.t + ch * (1 - v / max);
    i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
  });
  ctx.lineTo(pad.l + cw, pad.t + ch); ctx.lineTo(pad.l, pad.t + ch); ctx.closePath();
  ctx.fillStyle = grad; ctx.fill();

  // Line
  ctx.beginPath(); ctx.strokeStyle = 'var(--sage)'; ctx.lineWidth = 2.5; ctx.lineJoin = 'round';
  data.forEach((v, i) => {
    const x = pad.l + (i / (n - 1)) * cw;
    const y = pad.t + ch * (1 - v / max);
    i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
  });
  ctx.stroke();

  // Dots & labels
  ctx.font = '9px DM Sans'; ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'center';
  data.forEach((v, i) => {
    if (n > 10 && i % 3 !== 0) return;
    const x = pad.l + (i / (n - 1)) * cw;
    const y = pad.t + ch * (1 - v / max);
    ctx.beginPath(); ctx.arc(x, y, 3, 0, Math.PI * 2);
    ctx.fillStyle = 'var(--sage)'; ctx.fill();
    ctx.fillStyle = 'var(--muted)';
    ctx.fillText(labels[i], x, pad.t + ch + 16);
  });

  // Y axis
  ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'right'; ctx.font = '8px DM Sans';
  [0, 0.5, 1].forEach(f => {
    ctx.fillText(Math.round(max * (1 - f)), pad.l - 3, pad.t + ch * f + 3);
  });
}

function drawMacroChart(labels, pData, fData, cData) {
  const canvas = document.getElementById('macroChart');
  canvas.height = 120;
  const ctx = canvas.getContext('2d');
  const w = canvas.parentElement.offsetWidth || 320;
  canvas.width = w;
  ctx.clearRect(0, 0, w, 120);

  const pad = { l: 30, r: 10, t: 10, b: 24 };
  const cw = w - pad.l - pad.r, ch = 120 - pad.t - pad.b;
  const max = Math.max(...pData, ...fData, ...cData, 50) * 1.1;
  const n = pData.length;

  const series = [
    { data: pData, color: 'var(--protein-color)' },
    { data: fData, color: 'var(--fat-color)' },
    { data: cData, color: 'var(--carb-color)' },
  ];

  series.forEach(s => {
    ctx.beginPath(); ctx.strokeStyle = s.color; ctx.lineWidth = 2; ctx.lineJoin = 'round';
    s.data.forEach((v, i) => {
      const x = pad.l + (i / (n - 1)) * cw;
      const y = pad.t + ch * (1 - v / max);
      i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
    });
    ctx.stroke();
  });

  ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'center'; ctx.font = '9px DM Sans';
  pData.forEach((_, i) => {
    if (n > 10 && i % 3 !== 0) return;
    const x = pad.l + (i / (n - 1)) * cw;
    ctx.fillText(labels[i], x, pad.t + ch + 16);
  });

  // Legend
  const legend = [['P', 'var(--protein-color)'], ['F', 'var(--fat-color)'], ['C', 'var(--carb-color)']];
  let lx = pad.l;
  legend.forEach(([label, color]) => {
    ctx.fillStyle = color; ctx.fillRect(lx, pad.t, 8, 8);
    ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'left'; ctx.font = '9px DM Sans';
    ctx.fillText(label, lx + 11, pad.t + 7);
    lx += 28;
  });
}

function renderVitaminAnalytics() {
  const container = document.getElementById('vitaminAnalytics');
  container.innerHTML = '';
  const days = analyticsPeriod === 'week' ? 7 : 30;
  const vitAvg = {};
  let counted = 0;
  for (let i = 0; i < days; i++) {
    const d = new Date(); d.setDate(d.getDate() - i);
    const ds = d.toISOString().slice(0, 10);
    const day = dailyLogs[ds];
    if (!day) continue;
    counted++;
    MEAL_CONFIG.forEach(m => {
      (day[m.id] || []).forEach(item => {
        (item.vitamins || []).forEach(v => { vitAvg[v.type] = (vitAvg[v.type] || 0) + v.qty; });
      });
    });
  }
  if (counted > 0) Object.keys(vitAvg).forEach(k => vitAvg[k] /= counted);

  Object.entries(VITAMIN_GOALS).forEach(([name, goal], i) => {
    const avg = vitAvg[name] || 0;
    const pct = Math.min(avg / goal * 100, 100);
    const color = VITAMIN_COLORS[i % VITAMIN_COLORS.length];
    const row = document.createElement('div');
    row.innerHTML = `
      <div style="display:flex;justify-content:space-between;margin-bottom:4px">
        <span style="font-size:12px;font-weight:600;color:var(--ink-soft)">${MICRONUTRIENT_LABELS[name] || name}</span>
        <span style="font-size:12px;color:${pct >= 90 ? 'var(--sage-dark)' : 'var(--warn)'};font-weight:600">${Math.round(pct)}% avg</span>
      </div>
      <div style="height:6px;background:var(--pebble);border-radius:3px;overflow:hidden">
        <div style="height:100%;width:${pct}%;background:${pct >= 90 ? 'var(--sage)' : color};border-radius:3px;transition:width 1s ease"></div>
      </div>`;
    container.appendChild(row);
  });
}

// ── TABS / NAV ──
document.querySelectorAll('.tab').forEach(tab => {
  tab.onclick = () => {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    tab.classList.add('active');
    const name = tab.dataset.tab;
    document.getElementById('page-' + name).classList.add('active');
    document.querySelector(`[data-nav="${name}"]`)?.classList.add('active');
    if (name === 'library') renderLibrary();
    if (name === 'analytics') setTimeout(renderAnalytics, 50);
  };
});
document.querySelectorAll('.nav-item').forEach(nav => {
  nav.onclick = () => {
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    nav.classList.add('active');
    const name = nav.dataset.nav;
    document.getElementById('page-' + name).classList.add('active');
    document.querySelector(`[data-tab="${name}"]`)?.classList.add('active');
    if (name === 'library') renderLibrary();
    if (name === 'analytics') setTimeout(renderAnalytics, 50);
  };
});

// close modals on overlay click
document.querySelectorAll('.modal-overlay').forEach(overlay => {
  overlay.addEventListener('click', e => { if (e.target === overlay) { overlay.classList.remove('open'); document.body.style.overflow = ''; } });
});

// ── TOAST ──
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg; t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2200);
}

// ── INIT ──
buildCalendar();
renderDashboard();
</script>
</body>
</html>
