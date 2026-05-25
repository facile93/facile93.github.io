<!DOCTYPE html>
<html lang="fr">
<head>
  <link rel="manifest" href="manifest.json" />
<meta name="theme-color" content="#0a0a0f" />
<meta name="apple-mobile-web-app-capable" content="yes" />
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mon Split</title>
  <link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@300;400;600;700;900&family=Barlow:wght@300;400&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      min-height: 100vh;
      background: #0a0a0f;
      color: #f0f0f0;
      font-family: 'Barlow Condensed', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 0 16px 60px;
      position: relative;
      overflow-x: hidden;
    }
    #bg-glow {
      position: fixed;
      top: -20%;
      left: 50%;
      transform: translateX(-50%);
      width: 600px;
      height: 600px;
      border-radius: 50%;
      pointer-events: none;
      transition: background 0.8s ease;
      z-index: 0;
    }
    .wrap {
      width: 100%;
      max-width: 420px;
      position: relative;
      z-index: 1;
    }
    /* Header */
    #header {
      width: 100%;
      max-width: 420px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-top: 48px;
      padding-bottom: 8px;
      position: relative;
      z-index: 1;
    }
    #date-label { font-size: 13px; color: #666; letter-spacing: 3px; text-transform: uppercase; font-family: 'Barlow', sans-serif; }
    #app-title { font-size: 28px; font-weight: 900; letter-spacing: 1px; margin-top: 2px; }
    /* Pills */
    #pills {
      width: 100%;
      max-width: 420px;
      display: flex;
      gap: 6px;
      margin-bottom: 28px;
      position: relative;
      z-index: 1;
    }
    .pill { flex: 1; height: 4px; border-radius: 2px; background: rgba(255,255,255,0.1); transition: background 0.4s ease; }
    /* Buttons */
    button { font-family: 'Barlow Condensed', sans-serif; cursor: pointer; }
    .btn-ghost {
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 12px;
      color: #888;
      padding: 8px 14px;
      font-size: 12px;
      letter-spacing: 2px;
      text-transform: uppercase;
    }
    .btn-ghost.active { background: rgba(255,255,255,0.1); }
    /* Card */
    #main-card {
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 24px;
      padding: 32px 28px;
      margin-bottom: 16px;
      transition: border-color 0.5s ease;
      position: relative;
      overflow: hidden;
    }
    #card-glow-overlay {
      position: absolute;
      inset: 0;
      pointer-events: none;
      opacity: 0;
      transition: opacity 0.5s ease;
    }
    #card-top-row { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 16px; }
    #session-emoji { font-size: 64px; line-height: 1; }
    #btn-edit {
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 10px;
      color: #666;
      padding: 6px 12px;
      font-size: 12px;
      letter-spacing: 2px;
      text-transform: uppercase;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    #session-label { font-size: 11px; letter-spacing: 4px; text-transform: uppercase; margin-bottom: 6px; font-family: 'Barlow', sans-serif; }
    #session-name { font-size: 52px; font-weight: 900; line-height: 1; letter-spacing: -1px; margin-bottom: 8px; }
    #muscles { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 28px; }
    .muscle-tag {
      border-radius: 100px;
      padding: 4px 12px;
      font-size: 12px;
      font-family: 'Barlow', sans-serif;
    }
    #exercises { display: flex; flex-direction: column; gap: 10px; }
    .exercise-row { display: flex; align-items: center; gap: 12px; }
    .exercise-dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; }
    .exercise-label { font-size: 16px; font-weight: 400; color: #ccc; font-family: 'Barlow', sans-serif; transition: all 0.4s ease; }
    .exercise-label.done { color: #888; text-decoration: line-through; }
    /* CTA */
    #btn-complete {
      width: 100%;
      padding: 20px;
      border: none;
      border-radius: 16px;
      color: #0a0a0f;
      font-size: 20px;
      font-weight: 900;
      letter-spacing: 2px;
      text-transform: uppercase;
      transition: transform 0.2s ease, opacity 0.2s ease;
      margin-bottom: 16px;
    }
    #btn-complete:disabled { cursor: default; }
    #next-card {
      width: 100%;
      padding: 20px;
      background: rgba(255,255,255,0.04);
      border-radius: 16px;
      text-align: center;
      margin-bottom: 16px;
      display: none;
    }
    #next-label { font-size: 13px; color: #666; letter-spacing: 2px; text-transform: uppercase; font-family: 'Barlow', sans-serif; margin-bottom: 6px; }
    #next-name { font-size: 22px; font-weight: 700; }
    #streak { margin-top: 20px; display: flex; justify-content: center; gap: 6px; align-items: center; color: #555; font-size: 13px; font-family: 'Barlow', sans-serif; }
    /* Edit view */
    #edit-view { display: none; }
    #edit-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
    #edit-session-label { font-size: 11px; letter-spacing: 4px; text-transform: uppercase; font-family: 'Barlow', sans-serif; margin-bottom: 2px; }
    #edit-session-name { font-size: 26px; font-weight: 900; }
    #btn-done-edit {
      border: none;
      border-radius: 12px;
      color: #0a0a0f;
      padding: 8px 16px;
      font-size: 13px;
      font-weight: 700;
      letter-spacing: 1px;
    }
    #edit-list {
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 20px;
      padding: 20px;
      margin-bottom: 12px;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .edit-row { display: flex; align-items: center; gap: 8px; }
    .btn-delete {
      width: 28px; height: 28px; border-radius: 50%;
      background: rgba(255,80,80,0.12);
      border: 1px solid rgba(255,80,80,0.25);
      color: #ff5050;
      display: flex; align-items: center; justify-content: center;
      font-size: 16px; flex-shrink: 0; font-weight: 300; line-height: 1;
    }
    .edit-label-btn {
      flex: 1; display: flex; align-items: center; justify-content: space-between;
      background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.07);
      border-radius: 10px; padding: 10px 12px; cursor: pointer;
    }
    .edit-label-text { font-size: 15px; font-family: 'Barlow', sans-serif; color: #ccc; }
    .edit-pencil { color: #555; font-size: 12px; margin-left: 8px; flex-shrink: 0; }
    .edit-input-row { display: flex; flex: 1; gap: 6px; }
    .edit-input {
      flex: 1; background: rgba(255,255,255,0.08);
      border-radius: 8px; color: #f0f0f0;
      padding: 8px 12px; font-size: 15px;
      font-family: 'Barlow', sans-serif; outline: none;
    }
    .btn-ok {
      border: none; border-radius: 8px; color: #0a0a0f;
      padding: 8px 12px; font-weight: 700; font-size: 13px; flex-shrink: 0;
    }
    #btn-add-exercise {
      display: flex; align-items: center; gap: 10px;
      background: none; border-radius: 10px;
      padding: 10px 12px; font-size: 14px;
      font-family: 'Barlow', sans-serif;
      margin-top: 4px; width: 100%; text-align: left;
    }
    #btn-reset {
      display: none;
      width: 100%; padding: 14px; background: none;
      border: 1px solid rgba(255,80,80,0.2); border-radius: 12px;
      color: rgba(255,80,80,0.67); font-size: 14px;
      letter-spacing: 1px; font-family: 'Barlow', sans-serif;
    }
    /* History view */
    #history-view { display: none; }
    #history-title { font-size: 32px; font-weight: 900; margin-bottom: 20px; letter-spacing: 1px; }
    #history-empty { color: #555; font-family: 'Barlow', sans-serif; font-size: 16px; }
    #history-list { display: flex; flex-direction: column; gap: 10px; }
    .history-card {
      background: rgba(255,255,255,0.04); border: 1px solid rgba(255,255,255,0.07);
      border-radius: 14px; padding: 16px 20px;
      display: flex; justify-content: space-between; align-items: center;
    }
    .history-left { display: flex; align-items: center; gap: 12px; }
    .history-emoji { font-size: 24px; }
    .history-name { font-weight: 700; font-size: 16px; }
    .history-date { color: #555; font-size: 13px; font-family: 'Barlow', sans-serif; }
    .history-dot { width: 10px; height: 10px; border-radius: 50%; }
    /* Views */
    #session-view { display: block; }
  </style>
</head>
<body>

<div id="bg-glow"></div>

<!-- Header -->
<div id="header">
  <div>
    <div id="date-label"></div>
    <div id="app-title">MON SPLIT</div>
  </div>
  <div style="display:flex;gap:8px;">
    <button class="btn-ghost" id="btn-history-toggle">Historique</button>
  </div>
</div>

<!-- Pills -->
<div id="pills"></div>

<!-- SESSION VIEW -->
<div class="wrap" id="session-view">
  <div id="main-card">
    <div id="card-glow-overlay"></div>
    <div id="card-top-row">
      <div id="session-emoji"></div>
      <button id="btn-edit">✏️ Modifier</button>
    </div>
    <div id="session-label">Séance du jour</div>
    <div id="session-name"></div>
    <div id="muscles"></div>
    <div id="exercises"></div>
  </div>
  <button id="btn-complete">✓ Séance terminée</button>
  <div id="next-card">
    <div id="next-label">Demain</div>
    <div id="next-name"></div>
  </div>
  <div id="streak"></div>
</div>

<!-- EDIT VIEW -->
<div class="wrap" id="edit-view">
  <div id="edit-header">
    <div>
      <div id="edit-session-label">Modifier les exercices</div>
      <div id="edit-session-name"></div>
    </div>
    <button id="btn-done-edit">✓ Terminé</button>
  </div>
  <div id="edit-list"></div>
  <button id="btn-reset">Réinitialiser aux exercices par défaut</button>
</div>

<!-- HISTORY VIEW -->
<div class="wrap" id="history-view">
  <div id="history-title">HISTORIQUE</div>
  <div id="history-empty" style="display:none;">Aucune séance enregistrée pour l'instant.</div>
  <div id="history-list"></div>
</div>

<script>
const DEFAULT_SPLIT = [
  { name: "Pecs & Triceps", emoji: "💪", color: "#FF6B35", muscles: ["Pectoraux", "Triceps"], exercises: ["Développé couché", "Écarté poulie", "Dips", "Extension triceps"] },
  { name: "Dos & Biceps", emoji: "🏋️", color: "#4ECDC4", muscles: ["Grand dorsal", "Biceps"], exercises: ["Tractions", "Rowing barre", "Curl haltères", "Tirage poulie haute"] },
  { name: "Jambes", emoji: "🦵", color: "#FFE66D", muscles: ["Quadriceps", "Ischio-jambiers", "Mollets"], exercises: ["Squat", "Presse à cuisses", "Leg curl", "Mollets debout"] },
  { name: "Épaules", emoji: "🎯", color: "#A8E6CF", muscles: ["Deltoïdes", "Trapèzes"], exercises: ["Développé militaire", "Élévations latérales", "Oiseau", "Shrugs"] },
];
const DAYS = ["Dim","Lun","Mar","Mer","Jeu","Ven","Sam"];
const STORAGE_KEY = "split_tracker_v3";

// ── State ──
function loadData() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return { nextIndex: 0, history: [], customExercises: {} };
    const p = JSON.parse(raw);
    if (!p.customExercises) p.customExercises = {};
    return p;
  } catch { return { nextIndex: 0, history: [], customExercises: {} }; }
}
function saveData(d) { localStorage.setItem(STORAGE_KEY, JSON.stringify(d)); }
function getTodayKey() { return new Date().toISOString().slice(0, 10); }
function getExercises(idx, custom) { return custom[String(idx)] ?? DEFAULT_SPLIT[idx].exercises; }

let data = loadData();
let currentView = "session"; // session | edit | history
let editingIndex = null;
let animating = false;

// ── Derived ──
function derive() {
  const todayKey = getTodayKey();
  const history = data.history || [];
  const last = history[history.length - 1];
  const todayEntry = last && last.date === todayKey ? last : null;
  const done = todayEntry !== null;
  const sessionIdx = done ? todayEntry.sessionIndex : data.nextIndex % DEFAULT_SPLIT.length;
  const session = DEFAULT_SPLIT[sessionIdx];
  const nextSession = DEFAULT_SPLIT[(sessionIdx + 1) % DEFAULT_SPLIT.length];
  const exercises = getExercises(sessionIdx, data.customExercises || {});
  const isCustomized = !!(data.customExercises || {})[String(sessionIdx)];
  return { todayKey, history, done, sessionIdx, session, nextSession, exercises, isCustomized };
}

// ── Render ──
function render() {
  const { todayKey, history, done, sessionIdx, session, nextSession, exercises, isCustomized } = derive();

  // Date
  const now = new Date();
  document.getElementById("date-label").textContent = `${DAYS[now.getDay()]} · ${now.toLocaleDateString("fr-FR", { day: "numeric", month: "long" })}`;

  // Glow
  document.getElementById("bg-glow").style.background = `radial-gradient(circle, ${session.color}18 0%, transparent 70%)`;

  // Pills
  const pillsEl = document.getElementById("pills");
  pillsEl.innerHTML = "";
  DEFAULT_SPLIT.forEach((_, i) => {
    const d = document.createElement("div");
    d.className = "pill";
    d.style.background = i === sessionIdx ? session.color : "rgba(255,255,255,0.1)";
    pillsEl.appendChild(d);
  });

  // Views visibility
  document.getElementById("session-view").style.display = currentView === "session" ? "block" : "none";
  document.getElementById("edit-view").style.display = currentView === "edit" ? "block" : "none";
  document.getElementById("history-view").style.display = currentView === "history" ? "block" : "none";

  // History toggle button
  const btnHist = document.getElementById("btn-history-toggle");
  if (currentView === "history") {
    btnHist.textContent = "Retour";
    btnHist.classList.add("active");
  } else {
    btnHist.textContent = "Historique";
    btnHist.classList.remove("active");
  }

  // ── Session view ──
  if (currentView === "session") {
    document.getElementById("session-emoji").textContent = session.emoji;
    document.getElementById("session-label").textContent = done ? "✓ Séance du jour" : "Séance du jour";
    document.getElementById("session-label").style.color = session.color;

    // Name with colored &
    const nameParts = session.name.split(" & ");
    const nameEl = document.getElementById("session-name");
    if (nameParts.length === 2) {
      nameEl.innerHTML = `${nameParts[0].toUpperCase()} <span style="color:${session.color}">&amp;</span> ${nameParts[1].toUpperCase()}`;
    } else {
      nameEl.textContent = session.name.toUpperCase();
    }

    // Card border + overlay
    const card = document.getElementById("main-card");
    card.style.borderColor = done ? session.color + "60" : "rgba(255,255,255,0.08)";
    const overlay = document.getElementById("card-glow-overlay");
    overlay.style.opacity = done ? 1 : 0;
    overlay.style.background = `linear-gradient(135deg, ${session.color}08 0%, transparent 60%)`;

    // Edit btn color
    document.getElementById("btn-edit").style.color = "#666";

    // Muscles
    const musclesEl = document.getElementById("muscles");
    musclesEl.innerHTML = "";
    session.muscles.forEach(m => {
      const tag = document.createElement("span");
      tag.className = "muscle-tag";
      tag.textContent = m;
      tag.style.background = session.color + "18";
      tag.style.border = `1px solid ${session.color}30`;
      tag.style.color = session.color;
      musclesEl.appendChild(tag);
    });

    // Exercises
    const exEl = document.getElementById("exercises");
    exEl.innerHTML = "";
    exercises.forEach(ex => {
      const row = document.createElement("div");
      row.className = "exercise-row";
      const dot = document.createElement("div");
      dot.className = "exercise-dot";
      dot.style.background = session.color;
      const label = document.createElement("span");
      label.className = "exercise-label" + (done ? " done" : "");
      label.textContent = ex;
      row.appendChild(dot);
      row.appendChild(label);
      exEl.appendChild(row);
    });

    // CTA
    const btnComplete = document.getElementById("btn-complete");
    const nextCard = document.getElementById("next-card");
    if (!done) {
      btnComplete.style.display = "block";
      btnComplete.style.background = session.color;
      btnComplete.textContent = "✓ Séance terminée";
      nextCard.style.display = "none";
    } else {
      btnComplete.style.display = "none";
      nextCard.style.display = "block";
      nextCard.style.border = `1px solid ${nextSession.color}40`;
      document.getElementById("next-name").textContent = `${nextSession.emoji} ${nextSession.name}`;
      document.getElementById("next-name").style.color = nextSession.color;
    }

    // Streak
    const streakEl = document.getElementById("streak");
    if (history.length > 0) {
      streakEl.innerHTML = `<span>🔥</span><span>${history.length} séance${history.length > 1 ? "s" : ""} au total</span>`;
    } else {
      streakEl.innerHTML = "";
    }
  }

  // ── Edit view ──
  if (currentView === "edit") {
    document.getElementById("edit-session-label").style.color = session.color;
    document.getElementById("edit-session-name").textContent = `${session.emoji} ${session.name.toUpperCase()}`;
    const btnDone = document.getElementById("btn-done-edit");
    btnDone.style.background = session.color;

    const listEl = document.getElementById("edit-list");
    listEl.innerHTML = "";

    exercises.forEach((ex, i) => {
      const row = document.createElement("div");
      row.className = "edit-row";

      // Delete btn
      const btnDel = document.createElement("button");
      btnDel.className = "btn-delete";
      btnDel.textContent = "−";
      btnDel.style.opacity = exercises.length <= 1 ? "0.3" : "1";
      btnDel.style.cursor = exercises.length <= 1 ? "not-allowed" : "pointer";
      btnDel.onclick = () => deleteExercise(i);
      row.appendChild(btnDel);

      if (editingIndex === i) {
        // Input row
        const wrap = document.createElement("div");
        wrap.className = "edit-input-row";
        const inp = document.createElement("input");
        inp.className = "edit-input";
        inp.style.border = `1px solid ${session.color}80`;
        inp.value = ex;
        inp.placeholder = "Nom de l'exercice";
        inp.addEventListener("keydown", e => {
          if (e.key === "Enter") confirmEdit(i, inp.value);
          if (e.key === "Escape") { editingIndex = null; render(); }
        });
        setTimeout(() => inp.focus(), 0);
        const btnOk = document.createElement("button");
        btnOk.className = "btn-ok";
        btnOk.style.background = session.color;
        btnOk.textContent = "OK";
        btnOk.onclick = () => confirmEdit(i, inp.value);
        wrap.appendChild(inp);
        wrap.appendChild(btnOk);
        row.appendChild(wrap);
      } else {
        // Label btn
        const labelBtn = document.createElement("div");
        labelBtn.className = "edit-label-btn";
        const txt = document.createElement("span");
        txt.className = "edit-label-text";
        txt.textContent = ex;
        const pencil = document.createElement("span");
        pencil.className = "edit-pencil";
        pencil.textContent = "✏️";
        labelBtn.appendChild(txt);
        labelBtn.appendChild(pencil);
        labelBtn.onclick = () => { editingIndex = i; render(); };
        row.appendChild(labelBtn);
      }

      listEl.appendChild(row);
    });

    // Add exercise
    const btnAdd = document.createElement("button");
    btnAdd.id = "btn-add-exercise";
    btnAdd.style.border = `1px dashed ${session.color}50`;
    btnAdd.style.color = session.color;
    btnAdd.innerHTML = `<span style="font-size:20px;line-height:1;font-weight:300;">+</span> Ajouter un exercice`;
    btnAdd.onclick = addExercise;
    listEl.appendChild(btnAdd);

    // Reset btn
    const btnReset = document.getElementById("btn-reset");
    btnReset.style.display = isCustomized ? "block" : "none";
  }

  // ── History view ──
  if (currentView === "history") {
    const emptyEl = document.getElementById("history-empty");
    const listEl = document.getElementById("history-list");
    listEl.innerHTML = "";
    if (history.length === 0) {
      emptyEl.style.display = "block";
    } else {
      emptyEl.style.display = "none";
      [...history].reverse().forEach(h => {
        const s = DEFAULT_SPLIT[h.sessionIndex % DEFAULT_SPLIT.length];
        const card = document.createElement("div");
        card.className = "history-card";
        card.innerHTML = `
          <div class="history-left">
            <span class="history-emoji">${s.emoji}</span>
            <div>
              <div class="history-name">${h.name}</div>
              <div class="history-date">${h.date}</div>
            </div>
          </div>
          <div class="history-dot" style="background:${s.color}"></div>
        `;
        listEl.appendChild(card);
      });
    }
  }
}

// ── Actions ──
function handleComplete() {
  if (animating) return;
  const { done, todayKey, sessionIdx, session } = derive();
  if (done) return;
  animating = true;
  const btn = document.getElementById("btn-complete");
  btn.style.opacity = "0.7";
  btn.style.transform = "scale(0.97)";
  btn.textContent = "✓ Enregistrement...";
  setTimeout(() => {
    const newEntry = { date: todayKey, sessionIndex: sessionIdx, name: session.name };
    const newHistory = [...(data.history || []), newEntry].slice(-30);
    data = { ...data, nextIndex: (sessionIdx + 1) % DEFAULT_SPLIT.length, history: newHistory };
    saveData(data);
    animating = false;
    render();
  }, 600);
}

function confirmEdit(i, value) {
  const trimmed = value.trim();
  if (!trimmed) { editingIndex = null; render(); return; }
  const { sessionIdx, exercises } = derive();
  const key = String(sessionIdx);
  const newList = [...exercises];
  newList[i] = trimmed;
  data = { ...data, customExercises: { ...(data.customExercises || {}), [key]: newList } };
  saveData(data);
  editingIndex = null;
  render();
}

function deleteExercise(i) {
  const { sessionIdx, exercises } = derive();
  if (exercises.length <= 1) return;
  const key = String(sessionIdx);
  const newList = exercises.filter((_, idx) => idx !== i);
  data = { ...data, customExercises: { ...(data.customExercises || {}), [key]: newList } };
  saveData(data);
  if (editingIndex === i) editingIndex = null;
  render();
}

function addExercise() {
  const { sessionIdx, exercises } = derive();
  const key = String(sessionIdx);
  const newList = [...exercises, ""];
  data = { ...data, customExercises: { ...(data.customExercises || {}), [key]: newList } };
  saveData(data);
  editingIndex = newList.length - 1;
  render();
}

function resetExercises() {
  const { sessionIdx } = derive();
  const key = String(sessionIdx);
  const newCustom = { ...(data.customExercises || {}) };
  delete newCustom[key];
  data = { ...data, customExercises: newCustom };
  saveData(data);
  editingIndex = null;
  render();
}

// ── Event listeners ──
document.getElementById("btn-complete").addEventListener("click", handleComplete);

document.getElementById("btn-edit").addEventListener("click", () => {
  currentView = "edit";
  editingIndex = null;
  render();
});

document.getElementById("btn-done-edit").addEventListener("click", () => {
  currentView = "session";
  editingIndex = null;
  render();
});

document.getElementById("btn-history-toggle").addEventListener("click", () => {
  currentView = currentView === "history" ? "session" : "history";
  editingIndex = null;
  render();
});

document.getElementById("btn-reset").addEventListener("click", resetExercises);

// ── Init ──
render();
</script>
</body>
</html>
