<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no"/>
  <title>Easy Earning Bot</title>
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
      margin: 0; padding: 0;
      background: var(--background-color);
      color: var(--text-color);
      font-family: 'Segoe UI', 'Roboto', sans-serif;
      display: flex; justify-content: center; align-items: center;
      height: 100vh;
    }
    .container { width:100%; max-width:400px; height:100%; background:var(--background-color); display:flex; flex-direction:column; }
    main { flex-grow:1; overflow-y:auto; padding:20px; padding-bottom:80px; }
    .view { display:none; } .view.active { display:block; }

    .user-header-card { background:var(--card-bg); border-radius:16px; padding:15px; display:flex; align-items:center; margin-bottom:20px; }
    .user-avatar { width:50px; height:50px; border-radius:50%; background:linear-gradient(135deg,var(--primary-color),#006633); display:flex; align-items:center; justify-content:center; margin-right:15px; font-weight:bold; }
    .balance-card { background:linear-gradient(135deg,var(--primary-color),#006633); border-radius:16px; padding:20px; text-align:center; margin-bottom:20px; }
    .balance-value { font-size:36px; font-weight:800; margin:0; }
    .points-value { font-size:16px; margin-top:10px; }
    .task-card { background:var(--card-bg); border-radius:12px; padding:20px; margin-bottom:15px; display:flex; align-items:center; cursor:pointer; }
    .task-card:hover { background:#111; }
    .task-icon { font-size:22px; color:var(--primary-color); margin-right:20px; }

    .list-item { background:var(--card-bg); padding:12px; border-radius:10px; margin-bottom:10px; display:flex; align-items:center; }
    .list-item .rank { width:30px; color:var(--text-muted-color); }
    .list-item .info { flex-grow:1; }
    .list-item .score { font-weight:700; color:var(--accent-color); }

    .footer-nav { position:fixed; bottom:0; left:0; right:0; max-width:400px; display:flex; justify-content:space-around; background:#111; padding:10px 0; }
    .nav-button { color:var(--text-muted-color); background:none; border:none; font-size:12px; display:flex; flex-direction:column; align-items:center; }
    .nav-button.active { color:var(--primary-color); transform:scale(1.1); }

    .modal-overlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.8); display:none; align-items:center; justify-content:center; }
    .modal-overlay.active { display:flex; }
    .modal-content { background:#111; padding:20px; border-radius:12px; width:90%; max-width:350px; text-align:center; }
    .modal-content h3 { margin:0 0 15px; }
    .modal-actions button { margin:5px; padding:10px 15px; border:none; border-radius:8px; cursor:pointer; font-weight:bold; }
    .primary { background:var(--primary-color); color:#000; }
    .secondary { background:#222; color:var(--text-color); }
  </style>

  <script src="https://adexora.com/cdn/ads.js?id=455"></script>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
</head>
<body>
  <div class="container">
    <main>
      <!-- Home -->
      <div id="home-view" class="view active">
        <div class="user-header-card">
          <div class="user-avatar" id="profile-pic">U</div>
          <div>
            <h2 id="username">Loading...</h2>
            <p style="color:var(--text-muted-color)">Welcome Back!</p>
          </div>
        </div>
        <div class="balance-card">
          <p>Total Balance</p>
          <h1 class="balance-value" id="balance-value">৳0.00</h1>
          <p class="points-value"><i class="fa-solid fa-coins"></i> <span id="points-value">0</span> Points</p>
        </div>
        <h2 class="section-title">Quick Actions</h2>
        <div class="task-card" onclick="showRewardedInterstitial()">
          <div class="task-icon"><i class="fa-solid fa-rectangle-ad"></i></div>
          <div class="info"><h3>Watch Ads</h3><p>Earn rewards by watching ads.</p></div>
        </div>
        <div class="task-card" onclick="joinTelegramChannel()">
          <div class="task-icon"><i class="fa-brands fa-telegram"></i></div>
          <div class="info"><h3>Join Telegram</h3><p>Join our channel for updates.</p></div>
        </div>
        <div class="task-card" onclick="openWithdrawModal()">
          <div class="task-icon"><i class="fa-solid fa-wallet"></i></div>
          <div class="info"><h3>Withdraw</h3><p>Request payment via bKash or Nagad.</p></div>
        </div>
      </div>

      <!-- Leaderboard -->
      <div id="leaderboard-view" class="view">
        <h2>Top Earners</h2>
        <div id="leaderboard-list"></div>
      </div>

      <!-- History -->
      <div id="history-view" class="view">
        <h2>Recent Activity</h2>
        <div id="history-list"></div>
      </div>
    </main>

    <nav class="footer-nav">
      <button class="nav-button active" onclick="showView('home-view')"><i class="fa-solid fa-house"></i>Home</button>
      <button class="nav-button" onclick="showView('leaderboard-view')"><i class="fa-solid fa-trophy"></i>Leaders</button>
      <button class="nav-button" onclick="showView('history-view')"><i class="fa-solid fa-clock-rotate-left"></i>History</button>
    </nav>
  </div>

  <!-- Withdraw Modal -->
  <div class="modal-overlay" id="withdraw-modal">
    <div class="modal-content">
      <h3>Withdraw Funds</h3>
      <p>Select a method and enter your number:</p>
      <select id="method" style="padding:8px; border-radius:6px; margin-bottom:10px;">
        <option value="bKash">bKash</option>
        <option value="Nagad">Nagad</option>
      </select><br>
      <input type="text" id="accountNumber" placeholder="Enter Account Number" style="padding:8px; width:90%; border-radius:6px; margin-bottom:15px;">
      <div class="modal-actions">
        <button class="secondary" onclick="closeWithdrawModal()">Cancel</button>
        <button class="primary" onclick="requestWithdraw()">Confirm</button>
      </div>
    </div>
  </div>

  <script>
    let points = 0, balance = 0, historyLog = [], tgUser = null;

    document.addEventListener('DOMContentLoaded', initApp);

    function initApp() {
      if (window.Telegram && Telegram.WebApp) {
        Telegram.WebApp.ready(); Telegram.WebApp.expand();
        tgUser = Telegram.WebApp.initDataUnsafe.user;
      }
      loadData(); loadTelegramUser(); updateDisplay(); renderLeaderboard();
    }

    function loadTelegramUser() {
      if (tgUser) {
        document.getElementById('username').textContent = tgUser.first_name || 'User';
        const pic = document.getElementById('profile-pic');
        if (tgUser.photo_url) pic.innerHTML = `<img src="${tgUser.photo_url}" style="width:100%;height:100%;border-radius:50%">`;
        else pic.textContent = (tgUser.first_name||'U')[0].toUpperCase();
      }
    }

    function loadData() {
      if (!tgUser) return;
      const saved = localStorage.getItem('easyBotUser_'+tgUser.id);
      if (saved) {
        const d = JSON.parse(saved);
        points=d.points||0; balance=d.balance||0; historyLog=d.historyLog||[];
      }
    }
    function saveData() {
      if (!tgUser) return;
      localStorage.setItem('easyBotUser_'+tgUser.id, JSON.stringify({points,balance,historyLog}));
    }
    function updateDisplay() {
      document.getElementById('points-value').textContent = points;
      document.getElementById('balance-value').textContent = `৳${balance.toFixed(2)}`;
    }
    function addToHistory(msg) {
      historyLog.unshift({msg,time:new Date().toLocaleString()});
      if(historyLog.length>50) historyLog.pop(); saveData();
    }
    function renderHistory() {
      const list=document.getElementById('history-list');
      list.innerHTML = historyLog.map(h=>`<div class="list-item"><div class="info"><div>${h.msg}</div><div style="font-size:12px;color:var(--text-muted-color)">${h.time}</div></div></div>`).join('');
    }
    function renderLeaderboard() {
      const data=[{name:"CryptoKing",score:2500},{name:"Elena",score:2200},{name:"ProMiner",score:2000}];
      document.getElementById('leaderboard-list').innerHTML=data.map((u,i)=>`<div class="list-item"><div class="rank">#${i+1}</div><div class="info"><div>${u.name}</div></div><div class="score">${u.score} pts</div></div>`).join('');
    }

    function grantReward() {
      const reward=0.5; // ✅ per ad reward
      points+=1; balance+=reward;
      updateDisplay(); addToHistory(`Earned ৳${reward}`); saveData();
      alert(`You earned ৳${reward}`);
    }
    function showRewardedInterstitial() {
      if(typeof window.showAdexora!=='function') return alert("Ad not ready");
      window.showAdexora().then(grantReward).catch(()=>alert("Ad failed"));
    }
    function joinTelegramChannel() {
      const link="https://t.me/Abincome536";
      if(Telegram&&Telegram.WebApp) Telegram.WebApp.openTelegramLink(link); else window.open(link,"_blank");
      addToHistory("Joined Telegram Channel"); saveData();
    }

    // Withdraw
    function openWithdrawModal(){ document.getElementById('withdraw-modal').classList.add('active'); }
    function closeWithdrawModal(){ document.getElementById('withdraw-modal').classList.remove('active'); }
    function requestWithdraw(){
      const method=document.getElementById('method').value;
      const acc=document.getElementById('accountNumber').value.trim();
      if(!acc) return alert("Enter account number");
      if(balance<100) return alert("Minimum withdraw ৳100"); // ✅ set to 100
      const msg=`💸 *Withdrawal Request*\n👤 User: ${tgUser?.first_name||'Unknown'}\n🆔 ID: ${tgUser?.id||'N/A'}\n💰 Amount: ৳${balance.toFixed(2)}\n🏦 Method: ${method}\n📱 Number: ${acc}\n🔗 Username: ${tgUser?.username ? '@'+tgUser.username : 'N/A'}`;
      const botToken="7690572851:AAGxS4fR-oJbeSs84s89m07-KZ7HjqxfgAs"; 
      const chatId="@NEXGENPAYMENTS9";
      fetch(`https://api.telegram.org/bot${botToken}/sendMessage`,{
        method:"POST",headers:{'Content-Type':'application/json'},
        body:JSON.stringify({chat_id:chatId,text:msg,parse_mode:"Markdown"})
      }).then(r=>r.json()).then(res=>{
        if(res.ok){ 
          alert("Withdraw request sent!"); 
          addToHistory(`Withdraw request ৳${balance.toFixed(2)}`); 
          balance=0; points=0; 
          saveData(); updateDisplay(); closeWithdrawModal(); 
        }
        else alert("Failed to send request");
      }).catch(()=>alert("Error sending request"));
    }

    function showView(v){
      document.querySelectorAll('.view').forEach(x=>x.classList.remove('active'));
      document.getElementById(v).classList.add('active');
      document.querySelectorAll('.nav-button').forEach(b=>b.classList.remove('active'));
      document.querySelector(`.nav-button[onclick="showView('${v}')"]`).classList.add('active');
      if(v==='history-view') renderHistory();
    }
  </script>
</body>
</html>
