# Split-track<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
  <meta name="theme-color" content="#0a0a0f" />
  <title>Mon Split</title>
  <link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@300;400;600;700;900&family=Barlow:wght@300;400&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: #0a0a0f;
      color: #f0f0f0;
      font-family: 'Barlow Condensed', sans-serif;
      min-height: 100vh;
      min-height: 100dvh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 0 16px 60px;
      overflow-x: hidden;
    }

    #glow {
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

    /* HEADER */
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-top: 52px;
      padding-bottom: 8px;
    }
    .date-label {
      font-size: 12px;
      color: #666;
      letter-spacing: 3px;
      text-transform: uppercase;
      font-family: 'Barlow', sans-serif;
    }
    .app-title {
      font-size: 28px;
      font-weight: 900;
      letter-spacing: 1px;
      margin-top: 2px;
    }
    .hist-btn {
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 12px;
      color: #888;
      padding: 8px 14px;
      cursor: pointer;
      font-size: 11px;
      letter-spacing: 2px;
      text-transform: uppercase;
      font-family: 'Barlow', sans-serif;
    }

    /* PROGRESS PILLS */
    .pills {
      display: flex;
      gap: 6px;
      margin-bottom: 28px;
    }
    .pill {
      flex: 1;
      height: 4px;
      border-radius: 2px;
      background: rgba(255,255,255,0.1);
      transition: background 0.4s ease;
    }

    /* MAIN CARD */
    .card {
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.08);
      border-radius: 24px;
      padding: 32px 28px;
      margin-bottom: 16px;
      transition: border-color 0.5s ease;
      position: relative;
      overflow: hidden;
    }
    .card-glow {
      position: absolute;
      inset: 0;
      pointer-events: none;
      opacity: 0;
      transition: opacity 0.5s ease;
    }
    .card-emoji {
      font-size: 64px;
      line-height: 1;
      margin-bottom: 16px;
    }
    .card-label {
      font-size: 11px;
      letter-spacing: 4px;
      text-transform: uppercase;
      margin-bottom: 6px;
      font-family: 'Barlow', sans-serif;
    }
    .card-title {
      font-size: 52px;
      font-weight: 900;
      line-height: 1;
      letter-spacing: -1px;
      margin-bottom: 8px;
    }
    .tags {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-bottom: 28px;
    }
    .tag {
      border-radius: 100px;
      padding: 4px 12px;
      font-size: 12px;
      font-family: 'Barlow', sans-serif;
    }
    .exercises {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .exercise-row {
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .exercise-dot {
      width: 6px;
      height: 6px;
      border-radius: 50%;
      flex-shrink: 0;
    }
    .exercise-name {
      font-size: 16px;
      color: #ccc;
      font-family: 'Barlow', sans-serif;
      transition: all 0.4s ease;
    }
    .exercise-name.done {
      color: #888;
      text-decoration: line-through;
    }

    /* BUTTON */
    .complete-btn {
      width: 100%;
      padding: 20px;
      border: none;
      border-radius: 16px;
      color: #0a0a0f;
      font-size: 20px;
      font-weight: 900;
      letter-spacing: 2px;
      text-transform: uppercase;
      cursor: pointer;
      font-family: 'Barlow Condensed', sans-serif;
      transition: transform 0.2s ease, opacity 0.2s ease;
    }
    .complete-btn:active { transform: scale(0.97); opacity: 0.8; }

    /* NEXT SESSION */
    .next-card {
      width: 100%;
      padding: 20px;
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 16px;
      text-align: center;
    }
    .next-label {
      font-size: 12px;
      color: #666;
      letter-spacing: 2px;
      text-transform: uppercase;
      font-family: 'Barlow', sans-serif;
      margin-bottom: 6px;
    }
    .next-name {
      font-size: 22px;
      font-weight: 700;
    }

    /* STREAK */
    .streak {
      margin-top: 20px;
      display: flex;
      justify-content: center;
      gap: 6px;
      align-items: center;
      color: #555;
      font-size: 13px;
      font-family: 'Barlow', sans-serif;
    }

    /* HISTORY */
    .history-title {
      font-size: 32px;
      font-weight: 900;
      margin-bottom: 20px;
      letter-spacing: 1px;
    }
    .history-empty {
      color: #555;
      font-family: 'Barlow', sans-serif;
      font-size: 16px;
    }
    .history-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .history-item {
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.07);
      border-radius: 14px;
      padding: 16px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .history-left {
      display: flex;
      align-items: center;
      gap: 12px;
    }
    .history-emoji { font-size: 24px; }
    .history-name { font-weight: 700; font-size: 16px; }
    .history-date { color: #555; font-size: 13px; font-family: 'Barlow', sans-serif; }
    .history-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
    }

    /* HIDDEN */
    .hidden { display: none !important; }
  </style>
</head>
<body>

<div id="glow"></div>

<div class="wrap">
  <!-- HEADER -->
  <div class="header">
    <div>
      <div class="date-label" id="date-label"></div>
      <div class="app-title">MON SPLIT</div>
    </div>
    <button class="hist-btn" id="hist-btn" onclick="toggleView()">Historique</button>
  </div>

  <!-- PILLS -->
  <div class="pills" id="pills"></div>

  <!-- MAIN VIEW -->
  <div id="main-view">
    <div class="card" id="main-card">
      <div class="card-glow" id="card-glow"></div>
      <div class="card-emoji" id="card-emoji"></div>
      <div class="card-label" id="card-label">Séance du jour</div>
      <div class="card-title" id="card-title"></div>
      <div class="tags" id="card-tags"></div>
      <div class="exercises" id="card-exercises"></div>
    </div>

    <button class="complete-btn" id="complete-btn" onclick="completeSession()"></button>
    <div class="next-card hidden" id="next-card">
      <div class="next-label">Demain</div>
      <div class="next-name" id="next-name"></div>
    </div>

    <div class="streak hidden" id="streak">
      <span>🔥</span><span id="streak-text"></span>
    </div>
  </div>

  <!-- HISTORY VIEW -->
  <div id="history-view" class="hidden">
    <div class="history-title">HISTORIQUE</div>
    <div id="history-content"></div>
  </div>
</div>

<script>
  const SPLIT = [
    {
      name: "Pecs & Triceps",
      emoji: "💪",
      color: "#FF6B35",
      muscles: ["Pectoraux", "Triceps"],
      exercises: ["Développé couché", "Écarté poulie", "Dips", "Extension triceps"],
    },
    {
      name: "Dos & Biceps",
      emoji: "🏋️",
      color: "#4ECDC4",
      muscles: ["Grand dorsal", "Biceps"],
      exercises: ["Tractions", "Rowing barre", "Curl haltères", "Tirage poulie haute"],
    },
    {
      name: "Jambes",
      emoji: "🦵",
      color: "#FFE66D",
      muscles: ["Quadriceps", "Ischio-jambiers", "Mollets"],
      exercises: ["Squat", "Presse à cuisses", "Leg curl", "Mollets debout"],
    },
    {
      name: "Épaules",
      emoji: "🎯",
      color: "#A8E6CF",
      muscles: ["Deltoïdes", "Trapèzes"],
      exercises: ["Développé militaire", "Élévations latérales", "Oiseau", "Shrugs"],
    },
  ];

  const DAYS = ["Dim", "Lun", "Mar", "Mer", "Jeu", "Ven", "Sam"];

  function getData() {
    try {
      const raw = localStorage.getItem("split_tracker_v2");
      if (!raw) return { dayIndex: 0, history: [] };
      return JSON.parse(raw);
    } catch { return { dayIndex: 0, history: [] }; }
  }

  function saveData(d) {
    localStorage.setItem("split_tracker_v2", JSON.stringify(d));
  }

  function getTodayKey() {
    return new Date().toISOString().slice(0, 10);
  }

  let showingHistory = false;

  function toggleView() {
    showingHistory = !showingHistory;
    document.getElementById("main-view").classList.toggle("hidden", showingHistory);
    document.getElementById("history-view").classList.toggle("hidden", !showingHistory);
    document.getElementById("hist-btn").textContent = showingHistory ? "Séance" : "Historique";
    if (showingHistory) renderHistory();
  }

  function render() {
    const data = getData();
    const session = SPLIT[data.dayIndex % SPLIT.length];
    const todayKey = getTodayKey();
    const isDone = (data.history || []).some(h => h.date === todayKey);
    const next = SPLIT[(data.dayIndex + 1) % SPLIT.length];
    const history = data.history || [];

    // Date
    const now = new Date();
    const dayName = DAYS[now.getDay()];
    const dateStr = now.toLocaleDateString("fr-FR", { day: "numeric", month: "long" });
    document.getElementById("date-label").textContent = `${dayName} · ${dateStr}`;

    // Glow
    document.getElementById("glow").style.background =
      `radial-gradient(circle, ${session.color}18 0%, transparent 70%)`;

    // Pills
    const pillsEl = document.getElementById("pills");
    pillsEl.innerHTML = "";
    SPLIT.forEach((_, i) => {
      const pill = document.createElement("div");
      pill.className = "pill";
      if (i === data.dayIndex % SPLIT.length) pill.style.background = session.color;
      pillsEl.appendChild(pill);
    });

    // Card
    document.getElementById("main-card").style.borderColor =
      isDone ? session.color + "60" : "rgba(255,255,255,0.08)";

    const glowEl = document.getElementById("card-glow");
    glowEl.style.background = `linear-gradient(135deg, ${session.color}08 0%, transparent 60%)`;
    glowEl.style.opacity = isDone ? "1" : "0";

    document.getElementById("card-emoji").textContent = session.emoji;
    document.getElementById("card-label").style.color = session.color;

    // Title with colored &
    const titleEl = document.getElementById("card-title");
    if (session.name.includes(" & ")) {
      const parts = session.name.split(" & ");
      titleEl.innerHTML = `${parts[0].toUpperCase()} <span style="color:${session.color}">&amp;</span> ${parts[1].toUpperCase()}`;
    } else {
      titleEl.textContent = session.name.toUpperCase();
    }

    // Tags
    const tagsEl = document.getElementById("card-tags");
    tagsEl.innerHTML = "";
    session.muscles.forEach(m => {
      const tag = document.createElement("span");
      tag.className = "tag";
      tag.textContent = m;
      tag.style.background = session.color + "18";
      tag.style.border = `1px solid ${session.color}30`;
      tag.style.color = session.color;
      tagsEl.appendChild(tag);
    });

    // Exercises
    const exEl = document.getElementById("card-exercises");
    exEl.innerHTML = "";
    session.exercises.forEach(ex => {
      const row = document.createElement("div");
      row.className = "exercise-row";
      const dot = document.createElement("div");
      dot.className = "exercise-dot";
      dot.style.background = session.color;
      const name = document.createElement("span");
      name.className = "exercise-name" + (isDone ? " done" : "");
      name.textContent = ex;
      row.appendChild(dot);
      row.appendChild(name);
      exEl.appendChild(row);
    });

    // Button / Next card
    const btn = document.getElementById("complete-btn");
    const nextCard = document.getElementById("next-card");
    if (!isDone) {
      btn.classList.remove("hidden");
      nextCard.classList.add("hidden");
      btn.textContent = "✓ Séance terminée";
      btn.style.background = session.color;
    } else {
      btn.classList.add("hidden");
      nextCard.classList.remove("hidden");
      nextCard.style.borderColor = next.color + "40";
      document.getElementById("next-name").textContent = `${next.emoji} ${next.name}`;
      document.getElementById("next-name").style.color = next.color;
    }

    // Streak
    const streakEl = document.getElementById("streak");
    if (history.length > 0) {
      streakEl.classList.remove("hidden");
      document.getElementById("streak-text").textContent =
        `${history.length} séance${history.length > 1 ? "s" : ""} au total`;
    } else {
      streakEl.classList.add("hidden");
    }
  }

  function completeSession() {
    const data = getData();
    const todayKey = getTodayKey();
    const session = SPLIT[data.dayIndex % SPLIT.length];
    const btn = document.getElementById("complete-btn");

    btn.textContent = "✓ Enregistrement...";
    btn.style.opacity = "0.7";

    setTimeout(() => {
      const newHistory = [
        ...(data.history || []),
        { date: todayKey, dayIndex: data.dayIndex, name: session.name }
      ].slice(-30);
      const newData = {
        dayIndex: (data.dayIndex + 1) % SPLIT.length,
        history: newHistory,
      };
      saveData(newData);
      render();
    }, 600);
  }

  function renderHistory() {
    const data = getData();
    const history = [...(data.history || [])].reverse();
    const el = document.getElementById("history-content");
    if (history.length === 0) {
      el.innerHTML = '<div class="history-empty">Aucune séance enregistrée pour l\'instant.</div>';
      return;
    }
    const list = document.createElement("div");
    list.className = "history-list";
    history.forEach(h => {
      const s = SPLIT[h.dayIndex % SPLIT.length];
      const item = document.createElement("div");
      item.className = "history-item";
      item.innerHTML = `
        <div class="history-left">
          <span class="history-emoji">${s.emoji}</span>
          <div>
            <div class="history-name">${h.name}</div>
            <div class="history-date">${h.date}</div>
          </div>
        </div>
        <div class="history-dot" style="background:${s.color}"></div>
      `;
      list.appendChild(item);
    });
    el.innerHTML = "";
    el.appendChild(list);
  }

  render();
</script>
</body>
</html>
