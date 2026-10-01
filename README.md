[index.html](https://github.com/user-attachments/files/32924168/index.html)
# star-game<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#f5f3ff">
  <title>星星快手｜點擊挑戰</title>
  <style>
    :root {
      color-scheme: light;
      --ink: #29244b;
      --muted: #77728f;
      --purple: #7257e8;
      --purple-dark: #593fd0;
      --pink: #ff77a8;
      --card: rgba(255, 255, 255, .88);
    }
    * { box-sizing: border-box; }
    body {
      min-height: 100vh; margin: 0; padding: 28px 18px;
      display: grid; place-items: center; overflow-x: hidden;
      font-family: "Segoe UI", "Noto Sans TC", "Microsoft JhengHei", sans-serif;
      color: var(--ink);
      background: radial-gradient(ellipse at 12% 12%, #fff0f6 0, transparent 34%),
                  radial-gradient(ellipse at 88% 85%, #e3f6ff 0, transparent 35%),
                  linear-gradient(135deg, #f8f5ff, #f4f4ff 55%, #fff7ef);
    }
    button { font: inherit; }
    .game { width: min(100%, 560px); }
    .topline { display:flex; justify-content:space-between; align-items:center; margin:0 4px 13px; }
    .brand { display:flex; align-items:center; gap:9px; font-weight:850; letter-spacing:.04em; }
    .brand-icon { width:38px; height:38px; display:grid; place-items:center; border-radius:13px; background:#fff; box-shadow:0 5px 16px #6555a51c; font-size:21px; }
    .tag { color:var(--muted); font-size:12px; font-weight:700; }
    .card { padding:clamp(22px, 6vw, 38px); border:1px solid #ffffff; border-radius:28px; background:var(--card); box-shadow:0 22px 70px #51449a18, 0 3px 12px #51449a0c; backdrop-filter:blur(12px); }
    .intro { text-align:center; }
    h1 { margin:0; font-size:clamp(28px, 7vw, 38px); letter-spacing:.02em; }
    .subtitle { margin:9px 0 24px; color:var(--muted); line-height:1.7; }
    .stats { display:grid; grid-template-columns:1fr 1fr; gap:12px; margin:0 0 20px; }
    .stat { padding:13px 10px; border-radius:17px; background:#f7f5ff; }
    .stat-label { display:block; color:var(--muted); font-size:12px; font-weight:700; }
    .stat-value { display:block; margin-top:3px; font-size:25px; font-weight:850; font-variant-numeric:tabular-nums; }
    .timer-track { height:9px; margin-bottom:20px; overflow:hidden; border-radius:9px; background:#eeeafb; }
    .timer-bar { width:100%; height:100%; border-radius:inherit; background:linear-gradient(90deg, #8b6cf2, #ff86b1); transform-origin:left; transition:transform .12s linear; }
    .arena { position:relative; min-height:245px; display:grid; place-items:center; overflow:hidden; border:2px dashed #ded8f8; border-radius:22px; background:linear-gradient(145deg,#faf9ff,#f6faff); }
    .arena::before,.arena::after { position:absolute; content:"✦"; color:#d8cffb; font-size:19px; pointer-events:none; }
    .arena::before { top:16px; left:19px; }.arena::after { right:20px; bottom:15px; font-size:15px; }
    .target { position:absolute; display:none; width:72px; height:72px; align-items:center; justify-content:center; border:0; border-radius:50%; color:#fff; background:linear-gradient(145deg,#ff90b8,#fa5e9b); box-shadow:0 8px 0 #dc4782, 0 12px 23px #ed5a9738; font-size:31px; cursor:pointer; user-select:none; touch-action:manipulation; transition:transform .1s, filter .1s; animation:pop .16s ease-out; }
    .target:hover { filter:brightness(1.05); transform:scale(1.07); }.target:active { transform:scale(.9) translateY(4px); box-shadow:0 3px 0 #dc4782, 0 6px 12px #ed5a9738; }
    @keyframes pop { from { transform:scale(.3) rotate(-25deg); opacity:.3; } to { transform:scale(1) rotate(0); opacity:1; } }
    .message { position:absolute; inset:0; display:grid; place-content:center; padding:24px; text-align:center; }
    .message-icon { margin-bottom:8px; font-size:38px; }.message strong { font-size:18px; }.message span { margin-top:7px; color:var(--muted); font-size:14px; line-height:1.6; }
    .actions { display:flex; justify-content:center; gap:10px; margin-top:20px; }
    .btn { min-height:48px; padding:0 23px; border:0; border-radius:15px; font-weight:800; cursor:pointer; transition:transform .15s, background .15s; }
    .btn:hover { transform:translateY(-2px); }.btn:active { transform:translateY(0); }
    .btn-primary { color:#fff; background:linear-gradient(135deg,var(--purple),#9275f1); box-shadow:0 7px 16px #7257e83b; }.btn-primary:hover { background:linear-gradient(135deg,var(--purple-dark),#8464ed); }
    .btn-secondary { color:var(--purple-dark); background:#f0edff; }
    .hint { margin:17px 0 0; text-align:center; color:var(--muted); font-size:12px; }
    .sr-only { position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); white-space:nowrap; border:0; }
    @media (max-width:420px) { .arena { min-height:220px; }.tag { max-width:110px; text-align:right; } }
    @media (prefers-reduced-motion:reduce) { *,*::before,*::after { animation-duration:.01ms !important; transition-duration:.01ms !important; scroll-behavior:auto !important; } }
  </style>
</head>
<body>
  <main class="game">
    <div class="topline">
      <div class="brand"><span class="brand-icon" aria-hidden="true">🌟</span><span>星星快手</span></div>
      <span class="tag">30 秒反應力挑戰</span>
    </div>
    <section class="card" aria-labelledby="title">
      <header class="intro">
        <h1 id="title">點亮你的手速！</h1>
        <p class="subtitle">星星出現時，快快點下去！<br>30 秒內收集越多，分數越高。</p>
      </header>
      <div class="stats" aria-label="遊戲狀態">
        <div class="stat"><span class="stat-label">目前分數</span><span class="stat-value" id="score">0</span></div>
        <div class="stat"><span class="stat-label">剩餘時間</span><span class="stat-value"><span id="time">30</span><small style="font-size:13px;color:var(--muted)"> 秒</small></span></div>
      </div>
      <div class="timer-track" role="progressbar" aria-label="剩餘時間" aria-valuemin="0" aria-valuemax="30" aria-valuenow="30"><div class="timer-bar" id="timerBar"></div></div>
      <div class="arena" id="arena" aria-label="遊戲區域">
        <div class="message" id="message" aria-live="polite">
          <div class="message-icon" aria-hidden="true">✨</div>
          <strong id="messageTitle">準備好收集星星了嗎？</strong>
          <span id="messageText">按下「開始遊戲」，在星星消失前點擊它！</span>
        </div>
        <button class="target" id="target" type="button" aria-label="點擊星星加分">⭐</button>
      </div>
      <div class="actions"><button class="btn btn-primary" id="startBtn" type="button">開始遊戲</button><button class="btn btn-secondary" id="resetBtn" type="button">重新開始</button></div>
      <p class="hint">小提示：星星會隨機換位置，眼明手快就是高分秘訣！</p>
      <p class="sr-only" id="announcement" aria-live="polite"></p>
    </section>
  </main>
  <script>
    (() => {
      "use strict";
      const ROUND_SECONDS = 30;
      const arena = document.getElementById("arena");
      const target = document.getElementById("target");
      const scoreEl = document.getElementById("score");
      const timeEl = document.getElementById("time");
      const timerBar = document.getElementById("timerBar");
      const progress = document.querySelector('[role="progressbar"]');
      const message = document.getElementById("message");
      const messageIcon = document.querySelector(".message-icon");
      const messageTitle = document.getElementById("messageTitle");
      const messageText = document.getElementById("messageText");
      const announcement = document.getElementById("announcement");
      const startBtn = document.getElementById("startBtn");
      const resetBtn = document.getElementById("resetBtn");
      let score = 0;
      let running = false;
      let endsAt = 0;
      let timerId = null;
      let moveId = null;

      function placeStar() {
        const padding = 12;
        const maxX = Math.max(padding, arena.clientWidth - target.offsetWidth - padding);
        const maxY = Math.max(padding, arena.clientHeight - target.offsetHeight - padding);
        target.style.left = `${padding + Math.random() * (maxX - padding)}px`;
        target.style.top = `${padding + Math.random() * (maxY - padding)}px`;
      }

      function setMessage(icon, title, text) {
        messageIcon.textContent = icon;
        messageTitle.textContent = title;
        messageText.textContent = text;
        message.hidden = false;
      }

      function updateClock() {
        if (!running) return;
        const remaining = Math.max(0, (endsAt - Date.now()) / 1000);
        const shown = Math.ceil(remaining);
        timeEl.textContent = String(shown);
        timerBar.style.transform = `scaleX(${remaining / ROUND_SECONDS})`;
        progress.setAttribute("aria-valuenow", String(shown));
        if (remaining <= 0) finishGame();
      }

      function startGame() {
        if (running) return;
        score = 0;
        scoreEl.textContent = "0";
        timeEl.textContent = String(ROUND_SECONDS);
        timerBar.style.transform = "scaleX(1)";
        progress.setAttribute("aria-valuenow", String(ROUND_SECONDS));
        running = true;
        endsAt = Date.now() + ROUND_SECONDS * 1000;
        startBtn.textContent = "遊戲進行中…";
        startBtn.disabled = true;
        message.hidden = true;
        target.style.display = "flex";
        placeStar();
        timerId = window.setInterval(updateClock, 80);
        moveId = window.setInterval(placeStar, 1250);
        target.focus({ preventScroll: true });
      }

      function finishGame() {
        if (!running) return;
        running = false;
        window.clearInterval(timerId);
        window.clearInterval(moveId);
        timerId = null;
        moveId = null;
        target.style.display = "none";
        timeEl.textContent = "0";
        timerBar.style.transform = "scaleX(0)";
        progress.setAttribute("aria-valuenow", "0");
        startBtn.disabled = false;
        startBtn.textContent = "再玩一次";
        const result = score >= 25 ? ["🏆", "你是星星收集大師！", `超厲害！你收集了 ${score} 顆星星。`]
          : score >= 12 ? ["🎉", "手速很不錯！", `你收集了 ${score} 顆星星，再挑戰一次突破紀錄吧！`]
          : ["💪", "熱身完成！", `你收集了 ${score} 顆星星，多玩幾次會越來越快！`];
        setMessage(...result);
        announcement.textContent = `遊戲結束，你的分數是 ${score} 分。`;
      }

      target.addEventListener("click", () => {
        if (!running) return;
        score += 1;
        scoreEl.textContent = String(score);
        if (score % 5 === 0) announcement.textContent = `太棒了！目前 ${score} 分。`;
        placeStar();
      });

      function resetGame() {
        running = false;
        window.clearInterval(timerId);
        window.clearInterval(moveId);
        timerId = null;
        moveId = null;
        score = 0;
        scoreEl.textContent = "0";
        timeEl.textContent = String(ROUND_SECONDS);
        timerBar.style.transform = "scaleX(1)";
        progress.setAttribute("aria-valuenow", String(ROUND_SECONDS));
        target.style.display = "none";
        startBtn.disabled = false;
        startBtn.textContent = "開始遊戲";
        announcement.textContent = "";
        setMessage("✨", "準備好收集星星了嗎？", "按下「開始遊戲」，在星星消失前點擊它！");
      }

      startBtn.addEventListener("click", startGame);
      resetBtn.addEventListener("click", resetGame);
      window.addEventListener("resize", () => { if (running) placeStar(); });
    })();
  </script>
</body>
</html>
