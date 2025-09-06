<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fortune Cookie Generator</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; font-family: 'Poppins', sans-serif; }
  body {
    display:flex; align-items:center; justify-content:center; min-height:100vh;
    background: linear-gradient(-45deg,#ff6a00,#ee0979,#23a6d5,#23d5ab);
    background-size:400% 400%; animation: gradientBG 15s ease infinite;
    transition: background 0.5s, color 0.5s;
    flex-direction: column;
  }
  @keyframes gradientBG {
    0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}
  }
  .dark-mode { background:#121212; color:white; }
  h1 { margin-bottom:20px; color:white; text-shadow: 1px 1px 5px black; }
  .cookie-container { position: relative; text-align:center; }
  #cookie {
    width:150px; cursor:pointer; transition: transform 0.3s;
  }
  #cookie:hover { transform: scale(1.2) rotate(-10deg); }
  #fortune {
    margin-top:20px; font-size:20px; font-style:italic;
    color:white; min-height:60px; transition: opacity 0.5s;
  }
  button {
    margin:10px 5px; padding:10px 20px; border:none; border-radius:25px;
    cursor:pointer; background: linear-gradient(45deg,#ff416c,#ff4b2b); color:white;
    font-weight:bold; transition:0.3s, box-shadow 0.3s;
  }
  button:hover { transform: scale(1.1); box-shadow:0 0 15px rgba(255,65,108,0.7); }
  .favorites { margin-top:20px; text-align:left; max-width:300px; }
  .favorites h3 { font-size:16px; margin-bottom:5px; color:white; }
  ul { list-style:none; padding:0; color:white; }
  li { font-size:14px; margin:3px 0; cursor:pointer; }
  li:hover { color:#ff416c; }
</style>
</head>
<body>

<h1>🍪 Fortune Cookie Generator 🍪</h1>

<div class="cookie-container">
  <img src="file:///C:/Users/MADHUYUGA/Downloads/picture%202" id="cookie" alt="Fortune Cookie">
  <p id="fortune">Click the cookie to reveal your fortune!</p>
</div>

<div>
  <button onclick="copyFortune()">📋 Copy Fortune</button>
  <button onclick="toggleDark()">🌗 Dark/Light Mode</button>
  <button onclick="addFavorite()">⭐ Add to Favorites</button>
</div>

<div class="favorites">
  <h3>⭐ Favorites:</h3>
  <ul id="favList"></ul>
</div>

<audio id="crackSound">
  <source src="https://www.soundjay.com/button/beep-07.wav" type="audio/wav">
</audio>

<script>
const fortunes = [
  "You will have a great day today!",
  "A pleasant surprise is waiting for you.",
  "Someone is thinking of you fondly.",
  "Happiness begins with facing life with a smile.",
  "A new opportunity will present itself soon.",
  "Trust your instincts in making decisions.",
  "Good news will come to you by mail.",
  "An old friend will reappear in your life."
];

let favorites = JSON.parse(localStorage.getItem("cookieFavorites")) || [];

function newFortune() {
  const idx = Math.floor(Math.random() * fortunes.length);
  const fortune = document.getElementById("fortune");
  fortune.style.opacity = 0;
  setTimeout(() => {
    fortune.innerText = fortunes[idx];
    fortune.style.opacity = 1;
    document.getElementById("crackSound").play();
  }, 300);
}

document.getElementById("cookie").addEventListener("click", newFortune);

function copyFortune() {
  const fortuneText = document.getElementById("fortune").innerText;
  navigator.clipboard.writeText(fortuneText);
  alert("✅ Copied: " + fortuneText);
}

function toggleDark() {
  document.body.classList.toggle("dark-mode");
}

function addFavorite() {
  const fortuneText = document.getElementById("fortune").innerText;
  if(!favorites.includes(fortuneText)) {
    favorites.push(fortuneText);
    localStorage.setItem("cookieFavorites", JSON.stringify(favorites));
    updateFav();
  } else { alert("⭐ Already in favorites!"); }
}

function updateFav() {
  const list = document.getElementById("favList");
  list.innerHTML = "";
  favorites.forEach(f => {
    const li = document.createElement("li");
    li.innerText = f;
    li.onclick = () => { navigator.clipboard.writeText(f); alert("✅ Copied: "+f); };
    list.appendChild(li);
  });
}

// Load favorites on page load
updateFav();
</script>

</body>
</html>
