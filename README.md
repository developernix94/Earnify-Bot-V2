<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no"/>
  <title>Easy Earning Bot</title>

  <!-- FontAwesome -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

  <style>
    :root {
      --primary-color: #00ff66;
      --primary-hover: #33ff88;
      --secondary-color: #111;
      --background-color: #000;
      --text-color: #e0ffe0;
      --text-muted-color: #88aa88;
      --accent-color: #00cc55;
      --card-bg: #0a0a0a;
    }
    * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
    body {
      margin: 0;
      padding: 0;
      background-color: var(--background-color);
      color: var(--text-color);
      font-family: 'Segoe UI', 'Roboto', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      width: 100%;
      height: 100%;
      max-width: 400px;
      background: var(--background-color);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      position: relative;
    }
    main { flex-grow: 1; overflow-y: auto; padding: 20px; padding-bottom: 80px; }

    .view { display: none; animation: fadeIn 0.4s ease-in-out; }
    .view.active { display: block; }
    @keyframes fadeIn { from { opacity:0; transform:translateY(10px);} to { opacity:1; transform:translateY(0);} }

    /* User card */
    .user-header-card {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 15px;
      margin-bottom: 25px;
      display: flex;
      align-items: center;
    }
    .user-avatar {
      width: 50px; height: 50px;
      border-radius: 50%;
      margin-right: 15px;
      background: linear-gradient(135deg, var(--primary-color), #006633);
      display: flex; align-items: center; justify-content: center;
      font-weight: bold; font-size: 20px;
      overflow: hidden;
    }
    .user-avatar img { width:100%; height:100%; object-fit:cover; }
    .user-info .username { font-size:18px; font-weight:700; margin:0; }
    .user-info .title { font-size:14px; color:var(--text-muted-color); margin:0; }

    /* Balance */
    .balance-card {
      background: linear-gradient(135deg, var(--primary-color), #006633);
      border-radius: 16px;
      padding: 20px;
      text-align: center;
      margin-bottom: 25px;
    }
    .balance-card .label { font-size:16px; opacity:0.8; margin-bottom:5px; }
    .balance-card .balance-value { font-size:36px; font-weight:800; margin:0; }
    .balance-card .points-value { font-size:16px; font-weight:600; color:#fff; margin-top:10px; }

    /* Section */
    .section-title { font-size:20px; font-weight:700; margin-bottom:15px; border-left:4px solid var(--primary-color); padding-left:5px; }
    .task-card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 15px;
      display: flex; align-items: center;
      cursor: pointer;
      transition: transform 0.2s ease;
    }
    .task-card:hover { transform: translateY(-3px); background-color: #111; }
    .task-icon { font-size:24px; color:var(--primary-color); margin-right:20px; }
    .task-details h3 { margin:0; font-size:16px; font-weight:600; }
    .task-details p { margin:3px 0 0; font-size:13px; color:var(--text-muted-color); }
    .task-arrow { margin-left:auto; font-size:18px; color:var(--text-muted-color); }

    /* List (Leaderboard & History) */
    .list-item {
      display:flex; align-items:center;
      background: var(--card-bg);
      padding:12px 15px;
      border-radius:10px;
      margin-bottom:10px;
    }
    .list-item .rank { font-size:16px; font-weight:700; color:var(--text-muted-color); width:30px; }
    .list-item .avatar { width:40px; height:40px; border-radius:50%; background: #222; margin-right:15px; object-fit:cover; }
    .list-item .info { flex-grow:1; }
    .list-item .info .name { font-weight:600; font-size:15px; }
    .list-item .info .detail { font-size:13px; color:var(--text-muted-color); }
    .list-item .score { font-size:16px; font-weight:700; color:var(--accent-color); }

    /* Footer Nav */
    .footer-nav {
      position: fixed;
      bottom: 0; left: 0; right: 0;
      width: 100%; max-width: 400px;
      display: flex; justify-content: space-around;
      background: #111;
      padding: 10px 0;
      border-top: 1px solid #222;
    }
    .nav-button {
      display: flex; flex-direction: column; align-items: center;
      color: var(--text-muted-color);
      background: none; border: none; font-size:12px; cursor:pointer;
    }
    .nav-button .icon { font-size:22px; margin-bottom:4px; }
    .nav-button.active { color: var(--primary-color); transform: scale(1.1); }
  </style>

  <!-- Adexora (your first code link) -->
  <script src="https://adexora.com/cdn/ads.js?id=455"></script>
  <!-- Telegram -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
</head>
<body>
  <div class="container">
    <main>
      <!-- Home -->
      <div id="home-view" class="view active">
        <div class="user-header-card">
          <div class="user-avatar" id="profile-pic">U</div>
          <div class="user-info">
            <h2 class="username" id="username">Loading...</h2>
            <p class="title">Welcome Back!</p>
          </div>
        </div>
        <div class="balance-card">
          <p class="label">Total Balance</p>
          <h1 class="balance-value" id="balance-value">৳0.00</h1>
          <p class="points-value"><i class="fa-solid fa-coins"></i> <span id="points-value">0</span> Points</p>
        </div>
        <h2 class="section-title">Quick Actions</h2>
        <div class="task-card" onclick="showRewardedInterstitial()">
          <div class="task-icon"><i class="fa-solid fa-rectangle-ad"></i></div>
          <div class="task-details"><h3>Watch Ads</h3><p>Watch ads to earn rewards.</p></div>
          <div class="task-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        </div>
        <div class="task-card" onclick="joinTelegramChannel()">
          <div class="task-icon"><i class="fa-brands fa-telegram"></i></div>
          <div class="task-details"><h3>Join Telegram</h3><p>Click to join our official channel.</p></div>
          <div class="task-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        </div>
      </div>

      <!-- Leaderboard -->
      <div id="leaderboard-view" class="view">
        <h2 class="section-title">Top Earners</h2>
        <div id="leaderboard-list"></div>
      </div>

      <!-- History -->
      <div id="history-view" class="view">
        <h2 class="section-title">Recent Activity</h2>
        <div id="history-list"></div>
      </div>
    </main>

    <nav class="footer-nav">
      <button class="nav-button active" onclick="showView('home-view')"><i class="icon fa-solid fa-house"></i>Home</button>
      <button class="nav-button" onclick="showView('leaderboard-view')"><i class="icon fa-solid fa-trophy"></i>Leaders</button>
      <button class="nav-button" onclick="showView('history-view')"><i class="icon fa-solid fa-clock-rotate-left"></i>History</button>
    </nav>
  </div>

  <script>
    let points = 0;
    let balance = 0;
    let historyLog = [];
    let tgUser = null;

    document.addEventListener('DOMContentLoaded', () => {
      initApp();
    });

    function initApp() {
      if (window.Telegram && Telegram.WebApp) {
        Telegram.WebApp.ready();
        Telegram.WebApp.expand();
        tgUser = Telegram.WebApp.initDataUnsafe.user;
      }
      loadData();
      loadTelegramUser();
      updateDisplay();
      renderLeaderboard();
    }

    function loadTelegramUser() {
      if (tgUser) {
        document.getElementById('username').textContent = tgUser.first_name || 'Telegram User';
        const picDiv = document.getElementById('profile-pic');
        if (tgUser.photo_url) {
          picDiv.innerHTML = `<img src="${tgUser.photo_url}">`;
        } else {
          picDiv.textContent = (tgUser.first_name || 'U').charAt(0).toUpperCase();
        }
      }
    }

    function loadData() {
      if (!tgUser) return;
      const saved = localStorage.getItem('easyBotUser_' + tgUser.id);
      if (saved) {
        const data = JSON.parse(saved);
        points = data.points || 0;
        balance = data.balance || 0;
        historyLog = data.historyLog || [];
      }
    }

    function saveData() {
      if (!tgUser) return;
      localStorage.setItem('easyBotUser_' + tgUser.id, JSON.stringify({ points, balance, historyLog }));
    }

    function updateDisplay() {
      document.getElementById('points-value').textContent = points;
      document.getElementById('balance-value').textContent = `৳${balance.toFixed(2)}`;
    }

    function addToHistory(detail) {
      const entry = { detail, timestamp: new Date().toISOString() };
      historyLog.unshift(entry);
      if (historyLog.length > 50) historyLog.pop();
      saveData();
    }

    function renderHistory() {
      const list = document.getElementById('history-list');
      if (historyLog.length === 0) {
        list.innerHTML = `<div class="list-item"><div class="info"><div class="name">No activity yet.</div></div></div>`;
        return;
      }
      list.innerHTML = historyLog.map(item => `
        <div class="list-item">
          <div class="info">
            <div class="name">${item.detail}</div>
            <div class="detail">${new Date(item.timestamp).toLocaleString()}</div>
          </div>
        </div>
      `).join('');
    }

    function renderLeaderboard() {
      const leaderboardData = [
        { name: "CryptoKing", score: 2540, avatar: "https://i.pravatar.cc/150?img=1" },
        { name: "Elena", score: 2210, avatar: "https://i.pravatar.cc/150?img=2" },
        { name: "ProMiner", score: 1980, avatar: "https://i.pravatar.cc/150?img=3" }
      ];
      document.getElementById('leaderboard-list').innerHTML = leaderboardData.map((user, i) => `
        <div class="list-item">
          <div class="rank">#${i+1}</div>
          <img src="${user.avatar}" class="avatar">
          <div class="info"><div class="name">${user.name}</div></div>
          <div class="score">${user.score} pts</div>
        </div>
      `).join('');
    }

    function grantReward() {
      const reward = 5; // example reward
      points += 1;
      balance += reward;
      updateDisplay();
      addToHistory(`Earned ৳${reward}`);
      saveData();
      alert(`You earned ৳${reward}!`);
    }

    function showRewardedInterstitial() {
      if (typeof window.showAdexora !== 'function') return alert("Ad not ready");
      window.showAdexora()
        .then(grantReward)
        .catch(e => alert("Ad failed: " + e));
    }

    function joinTelegramChannel() {
      const link = "https://t.me/Abincome536";
      if (Telegram && Telegram.WebApp) {
        Telegram.WebApp.openTelegramLink(link);
      } else {
        window.open(link, "_blank");
      }
      addToHistory("Joined Telegram Channel");
      saveData();
    }

    function showView(viewId) {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById(viewId).classList.add('active');
      document.querySelectorAll('.nav-button').forEach(btn => btn.classList.remove('active'));
      const activeBtn = document.querySelector(`.nav-button[onclick="showView('${viewId}')"]`);
      if (activeBtn) activeBtn.classList.add('active');
      if (viewId === 'history-view') renderHistory();
    }
  </script>
</body>
</html>
