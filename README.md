<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<title>大老二 Big Two</title>
<style>
body {
  font-family: Arial;
  background: #2e7d32;
  color: white;
  text-align: center;
}
.card {
  display: inline-block;
  padding: 8px 10px;
  margin: 5px;
  background: white;
  color: black;
  border-radius: 5px;
  cursor: pointer;
}
.selected {
  background: gold;
}
button {
  padding: 10px 20px;
  margin: 10px;
}
</style>
</head>
<body>

<h1>🎴 大老二 Big Two</h1>

<h2>你的手牌</h2>
<div id="player"></div>

<button onclick="playCards()">出牌</button>
<button onclick="pass()">Pass</button>

<h2>上一手牌</h2>
<div id="table"></div>

<p id="status"></p>

<script>
const suits = ["♣","♦","♥","♠"];
const values = [3,4,5,6,7,8,9,10,11,12,13,14,15]; // 11 J, 12 Q, 13 K, 14 A, 15 = 2
let deck = [];
let players = [[],[],[],[]];
let currentPlayer = 0;
let tableCards = [];

function createDeck() {
  for (let s of suits) {
    for (let v of values) {
      deck.push({suit:s, value:v});
    }
  }
}

function shuffle() {
  deck.sort(() => Math.random() - 0.5);
}

function deal() {
  for (let i = 0; i < 52; i++) {
    players[i % 4].push(deck[i]);
  }
  players.forEach(p => p.sort((a,b)=>a.value-b.value));
}

function renderPlayer() {
  const div = document.getElementById("player");
  div.innerHTML = "";
  players[0].forEach((c,i)=>{
    let d = document.createElement("div");
    d.className = "card";
    d.innerText = c.suit + c.value;
    d.onclick = ()=>d.classList.toggle("selected");
    div.appendChild(d);
  });
}

function playCards() {
  const selected = [...document.querySelectorAll(".selected")];
  if (selected.length === 0) return;

  tableCards = selected.map(d => d.innerText);
  document.getElementById("table").innerText = tableCards.join(" ");
  
  selected.forEach(d=>{
    let idx = [...document.getElementById("player").children].indexOf(d);
    players[0].splice(idx,1);
  });

  renderPlayer();
  status("你出牌了");
  aiTurn();
}

function aiTurn() {
  for (let i = 1; i < 4; i++) {
    if (players[i].length === 0) continue;
    let card = players[i].shift();
    tableCards = [card.suit + card.value];
    document.getElementById("table").innerText = tableCards.join(" ");
    status(`玩家 ${i+1} 出牌`);
  }
}

function pass() {
  status("你 Pass");
  aiTurn();
}

function status(msg) {
  document.getElementById("status").innerText = msg;
}

createDeck();
shuffle();
deal();
renderPlayer();
</script>

</body>
</html>
