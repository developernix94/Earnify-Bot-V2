<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Easy Earning Bot</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

body {
  font-family: 'Roboto', sans-serif;
  background: linear-gradient(135deg, #1e1e2f, #3a3a5c);
  color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  margin: 0;
  padding: 20px;
  overflow-x: hidden;
}

h1 {
  margin-top: 40px;
  font-size: 3em;
  color: #ffcc00;
  text-shadow: 2px 2px 10px rgba(0,0,0,0.5);
  animation: glow 1.5s infinite alternate;
}

@keyframes glow {
  0% { text-shadow: 0 0 10px #ffcc00, 0 0 20px #ffcc00; }
  100% { text-shadow: 0 0 20px #ffcc00, 0 0 40px #ffcc00; }
}

.welcome-msg {
  background: rgba(41,41,70,0.9);
  padding: 15px 25px;
  border-radius: 15px;
  font-size: 1.3em;
  margin: 20px 0;
  text-align: center;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
  animation: slideIn 1s ease-out forwards;
}

@keyframes slideIn {
  from { opacity: 0; transform: translateY(-30px);}
  to { opacity: 1; transform: translateY(0);}
}

.ad-container {
  width: 100%;
  max-width: 500px;
  background: rgba(41,41,70,0.95);
  padding: 20px;
  border-radius: 20px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  overflow: hidden;
}

.ad-title {
  font-size: 1.5em;
  margin-bottom: 15px;
  color: #00ffd5;
  text-shadow: 1px 1px 5px #000;
}

.ad-box {
  width: 100%;
  min-height: 250px;
  background: #3a3a5c;
  border-radius: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  position: relative;
  overflow: hidden;
}

.anime-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
}

.anime-character {
  position: absolute;
  width: 80px;
  height: 80px;
  background-size: contain;
  background-repeat: no-repeat;
  opacity: 0.15;
  animation: floatAnime 8s ease-in-out infinite;
}

@keyframes floatAnime {
  0% { transform: translateY(0) rotate(0deg);}
  50% { transform: translateY(-25px) rotate(5deg);}
  100% { transform: translateY(0) rotate(0deg);}
}

footer {
  margin-top: auto;
  font-size: 0.9em;
  opacity: 0.7;
  padding: 20px 0;
}

.reward-msg {
  position: absolute;
  top: 10px;
  background: #ffcc00;
  color: #1e1e2f;
  padding: 8px 15px;
  border-radius: 12px;
  font-weight: bold;
  display: none;
  animation: pop 0.5s forwards;
}

@keyframes pop {
  0% { transform: scale(0); opacity: 0;}
  100% { transform: scale(1); opacity: 1;}
}
</style>
</head>
<body>
<h1>Easy Earning Bot</h1>

<div class="welcome-msg">
    Welcome! Watch ads below to earn rewards.
</div>

<div class="ad-container">
    <div class="ad-title">Your Ad</div>
    <div class="ad-box" id="ad-box">
        <div class="anime-bg" id="anime-bg"></div>
    </div>
    <div class="reward-msg" id="reward-msg">🎉 Reward Earned!</div>
</div>

<footer>© 2025 Easy Earning Bot</footer>

<!-- Updated Adexora Script -->
<script src="https://adexora.com/cdn/ads.js?id=455"></script>
<script>
window.showAdexora()
  .then(() => {
    const rewardMsg = document.getElementById('reward-msg');
    rewardMsg.style.display = 'block';
    setTimeout(() => rewardMsg.style.display = 'none', 2500);

    // ✅ Your reward logic here
    console.log('Reward logic executed');
  })
  .catch(e => {
    // ❌ Handle errors here
    console.error("Failed to show ad:", e);
  });
</script>

<!-- Telegram Web App JS -->
<script src="https://telegram.org/js/telegram-web-app.js"></script>
<script>
// Initialize Telegram user
const tgUser = window.Telegram?.WebApp?.initDataUnsafe?.user || null;

if (!tgUser) {
  alert("⚠️ Please open this Mini App inside Telegram to track rewards.");
}

const animeBg = document.getElementById('anime-bg');

// Array of anime character images
const characters = [
  'https://i.imgur.com/3U7Y2XZ.png',
  'https://i.imgur.com/Lj9cXQv.png',
  'https://i.imgur.com/KcY2Z9V.png'
];

// Spawn floating characters
characters.forEach((src, i) => {
  const char = document.createElement('div');
  char.classList.add('anime-character');
  char.style.backgroundImage = `url(${src})`;
  char.style.left = `${10 + i*30}%`;
  char.style.animationDelay = `${i*2}s`;
  animeBg.appendChild(char);
});
</script>
</body>
</html>
