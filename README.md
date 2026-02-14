<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<title>SYSTEM ULTRA</title>
<style>
body{
  background:black;
  color:#00ff66;
  font-family:monospace;
  text-align:center;
  padding:20px;
  overflow:hidden;
}
.red{color:red;font-weight:bold;}
#timer{font-size:3em;color:red;margin-top:20px;}
#crash{
  display:none;
  color:white;
  background:#0000aa;
  position:fixed;
  inset:0;
  padding:30px;
  font-family:monospace;
  text-align:left;
}
</style>
</head>

<body>
<h1>⚡ SYSTEM NANO-X ⚡</h1>
<div id="box"></div>
<div id="timer"></div>

<div id="crash">
<h2>:(</h2>
<p>Dispositivo ha riscontrato un errore critico.</p>
<p>Raccolta informazioni errore...</p>
<p id="percent">0%</p>
<p style="margin-top:30px">
SYSTEM_THREAD_EXCEPTION_NOT_HANDLED<br>
STOP CODE: 0x00000BOT
</p>
</div>

<script>
// ---------- AUDIO ----------
const AudioCtx = window.AudioContext || window.webkitAudioContext;
const audioCtx = new AudioCtx();

function beep(f,d){
  const o=audioCtx.createOscillator();
  const g=audioCtx.createGain();
  o.frequency.value=f;
  o.connect(g);
  g.connect(audioCtx.destination);
  o.start();
  g.gain.setValueAtTime(0.3,audioCtx.currentTime);
  g.gain.exponentialRampToValueAtTime(0.001,audioCtx.currentTime+d);
  o.stop(audioCtx.currentTime+d);
}

// ---------- VOCE ROBOT ----------
function robotVoice(text){
  const msg = new SpeechSynthesisUtterance(text);
  msg.lang="it-IT";
  msg.pitch=0.2;
  msg.rate=0.75;
  speechSynthesis.speak(msg);
}

// ---------- TESTI ----------
const lines=[
"Connessione sistema...",
"Accesso memoria...",
"Nanobot online...",
"⚠ ERRORE CRITICO ⚠"
];

let i=0;
const box=document.getElementById("box");
const timerBox=document.getElementById("timer");

function typeLine(){
  if(i>=lines.length) return;
  const p=document.createElement("p");
  if(lines[i].includes("ERRORE")) p.className="red";
  box.appendChild(p);
  let j=0;
  const t=setInterval(()=>{
    p.textContent+=lines[i][j];
    beep(900,0.03);
    j++;
    if(j>=lines[i].length){
      clearInterval(t);
      if(lines[i].includes("ERRORE")){
        robotVoice("Errore critico. Avvio timer autodistruzione.");
        setTimeout(startTimer,2000);
        return;
      }
      i++;
      setTimeout(typeLine,800);
    }
  },45);
}

// -------- TIMER ----------
function startTimer(){
  let t=5;
  timerBox.textContent=t;
  const c=setInterval(()=>{
    beep(400,0.1);
    t--;
    timerBox.textContent=t;
    if(t<=0){
      clearInterval(c);
      explosion();
    }
  },1000);
}

// -------- ESPLOSIONE --------
function explosion(){
  robotVoice("Sistema compromesso. Arresto forzato.");
  let s=0;
  const e=setInterval(()=>{
    document.body.style.background=
      `rgb(${r()},${r()},${r()})`;
    document.body.style.transform=
      `translate(${r2()}px,${r2()}px)`;
    beep(80+Math.random()*200,0.1);
    s++;
    if(s>20){
      clearInterval(e);
      fakeCrash();
    }
  },90);
}

// -------- FINTA CRASH --------
function fakeCrash(){
  document.body.style.transform="none";
  document.body.style.background="black";
  document.getElementById("crash").style.display="block";
  let p=0;
  const perc=document.getElementById("percent");
  const load=setInterval(()=>{
    p+=Math.floor(Math.random()*8);
    if(p>100) p=100;
    perc.textContent=p+"%";
    if(p>=100){
      clearInterval(load);
      robotVoice("Dispositivo bloccato. Riavvio necessario.");
    }
  },400);
}

function r(){return Math.floor(Math.random()*255);}
function r2(){return Math.floor(Math.random()*30-15);}

// AVVIO
document.body.addEventListener("click",()=>{
  audioCtx.resume();
  robotVoice("Avvio sistema.");
  typeLine();
},{once:true});
</script>

<p style="color:#555;font-size:0.8em">(tocca per avviare)</p>
</body>
</html>
