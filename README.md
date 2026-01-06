# My-website
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Step Reward Game (SPCK)</title>

<style>
body{font-family:Arial;background:#eef2f3;text-align:center}
.card{background:white;margin:10px auto;padding:15px;width:340px;border-radius:10px}
.steps{font-size:40px;color:#2980b9}
button,input{padding:8px;width:90%;margin:5px}
canvas{background:#555;border:2px solid black}
#gameBox{display:none}
</style>
</head>

<body>

<h2>🚶 Step → Reward → Game</h2>

<div class="card">
<h3>Steps</h3>
<div class="steps" id="steps">0</div>
<button onclick="addStep()">➕ Add Step</button>
</div>

<div class="card">
<h3>Set Daily Task</h3>
<input type="number" id="goal" placeholder="1000–10000">
<button onclick="setGoal()">Save Task</button>
<p id="goalText"></p>
</div>

<div class="card">
<h3>What You Get</h3>
<p>1000 → Quote 💬</p>
<p>3500 → 🥉 Bronze + 100 coins</p>
<p>7500 → 🥈 Silver + 200 coins</p>
<p>9000 → 🥇 Gold + 400 coins</p>
</div>

<div class="card">
<h3>Rewards</h3>
<p id="reward">No reward yet</p>
<p id="coins">Coins: 0</p>
</div>

<div class="card" id="gameBox">
<h3>🚗 Endless Car Game</h3>
<canvas id="game" width="300" height="400"></canvas>
<button onclick="restart()">Play Again</button>
</div>

<script>
let steps=0,coins=0,goal=5000;

function addStep(){
 steps++;
 document.getElementById("steps").innerText=steps;
 checkRewards();
}

function setGoal(){
 let g=document.getElementById("goal").value;
 if(g<1000||g>10000){alert("1000–10000 only");return;}
 goal=g;
 document.getElementById("goalText").innerText="Today's Task: "+goal+" steps";
}

function checkRewards(){
 let r=document.getElementById("reward");

 if(steps>=1000) r.innerText="💬 Great start! Keep walking!";
 if(steps>=3500&&coins<100){coins=100;r.innerText="🥉 Bronze Medal";}
 if(steps>=7500&&coins<300){coins=300;r.innerText="🥈 Silver Medal";}
 if(steps>=9000&&coins<700){coins=700;r.innerText="🥇 Gold Medal";}

 document.getElementById("coins").innerText="Coins: "+coins;

 if(coins>=400){
  document.getElementById("gameBox").style.display="block";
  startGame();
 }
}

/* GAME */
let canvas=document.getElementById("game");
let ctx=canvas.getContext("2d");
let carX=130,enemies=[],over=false;

function startGame(){
 enemies=[]; over=false;
 for(let i=0;i<5;i++) enemies.push({x:Math.random()*260,y:-i*80});
 requestAnimationFrame(loop);
}

function loop(){
 if(over) return;
 ctx.clearRect(0,0,300,400);
 ctx.fillStyle="white";
 ctx.fillRect(carX,330,30,50);
 ctx.fillStyle="red";
 enemies.forEach(e=>{
  e.y+=4;
  if(e.y>400){e.y=0;e.x=Math.random()*260}
  ctx.fillRect(e.x,e.y,30,50);
  if(Math.abs(e.x-carX)<30&&Math.abs(e.y-330)<40){
   over=true; alert("💥 Crash! Try again");
  }
 });
 requestAnimationFrame(loop);
}

document.addEventListener("keydown",e=>{
 if(e.key==="ArrowLeft")carX-=20;
 if(e.key==="ArrowRight")carX+=20;
});

function restart(){startGame();}
</script>

</body>
</html>
