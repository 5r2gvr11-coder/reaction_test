<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>專注力測驗 - 極速飆分版</title>
<style>
  @font-face {
    font-family: 'Press Start 2P';
    src: url('https://fonts.gstatic.com/s/pressstart2p/v14/8Lg6LZwJ9Nzvlgy5K_S-l2-uT0yE4Z_qR3_P.woff2') format('woff2');
  }
  body {
    text-align: center; font-family: 'Press Start 2P', cursive, sans-serif;
    background: #6B8EFC; color: white; user-select: none; overflow: hidden; margin: 0;
    height: 100vh; display: flex; flex-direction: column;
  }

  .cloud { background: white; border-radius: 50%; position: absolute; z-index: 1; animation: cloud-move 25s linear infinite; }
  .c1 { width: 80px; height: 40px; top: 15%; left: -100px; opacity: 0.8; }
  .c2 { width: 60px; height: 30px; top: 25%; left: -150px; opacity: 0.5; animation-delay: -10s; }
  @keyframes cloud-move { from { transform: translateX(-10vw); } to { transform: translateX(110vw); } }

  #grass { position: absolute; bottom: 0; width: 100%; height: 60px; background: linear-gradient(#8BC34A, #4CAF50); border-top: 4px solid #388E3C; z-index: 2; }
  
  #mario-container { position: absolute; bottom: 60px; left: 40px; width: 32px; height: 32px; z-index: 5; image-rendering: pixelated; transition: transform 0.1s ease-out; }
  #mario-sprite { width: 32px; height: 32px; background-image: url('https://thealphabeat.github.io/Super-Mario-Bros/img/mario-sprites.png'); background-position: -80px -32px; animation: mario-run 0.4s steps(3) infinite; animation-play-state: paused; }
  @keyframes mario-run { from { background-position: -80px -32px; } to { background-position: -160px -32px; } }

  #progress-container { width: 100%; height: 12px; background: rgba(0,0,0,0.3); position: relative; }
  #progress-bar { width: 0%; height: 100%; background: #FFD700; transition: width 0.3s; }

  #instruction-sticky { background: rgba(0,0,0,0.6); padding: 12px 5px; font-size: 10px; line-height: 1.6; border-bottom: 3px solid #FFD700; display: flex; justify-content: center; gap: 10px; letter-spacing: -0.5px; }
  .rule-tag { padding: 2px 6px; border-radius: 4px; border: 1px solid #FFF; }
  .rule-white { background: #FFF; color: #000; }
  .rule-yellow { background: #FFD700; color: #000; }
  .rule-red { background: #FF4444; color: #FFF; }

  #rule-overlay {
    position: absolute; top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0,0,0,0.85); z-index: 200; display: none;
    flex-direction: column; align-items: center; justify-content: center;
  }
  .rule-box { background: #333; border: 4px solid #FFF; padding: 20px; width: 80%; border-radius: 10px; text-align: left; }
  .rule-item { margin: 15px 0; font-size: 12px; line-height: 1.5; display: flex; align-items: center; }
  .rule-icon { width: 40px; height: 40px; display: flex; align-items: center; justify-content: center; font-size: 24px; margin-right: 15px; border: 2px solid #FFF; background: #000; }

  #ui-layer { position: relative; z-index: 10; flex: 1; display: flex; flex-direction: column; }
  #game-header { padding: 10px; background: rgba(0,0,0,0.2); display: flex; justify-content: space-around; font-size: 10px; align-items: center; }
  #score-display { font-size: 24px; color: #FFD700; text-shadow: 2px 2px #000; transition: transform 0.1s; display: inline-block; }
  .score-bump { transform: scale(1.2); }
  
  #letter-container { flex: 1; display: flex; align-items: center; justify-content: center; position: relative; }
  #letter { font-size: 100px; font-weight: 900; text-shadow: 5px 5px #000; z-index: 15; transition: transform 0.05s; }
  #mode-tag { position: absolute; top: 20px; font-size: 14px; background: rgba(255,0,0,0.6); padding: 5px 15px; border-radius: 20px; display: none; }
  
  .score-pop { 
    position: absolute; color: #FFD700; font-size: 28px; font-weight: bold; 
    animation: jumpUp 0.5s forwards; z-index: 50; text-shadow: 2px 2px #000; 
    pointer-events: none;
  }
  @keyframes jumpUp { 
    0% { opacity: 1; transform: translateY(0) scale(1); } 
    100% { opacity: 0; transform: translateY(-120px) scale(1.5); } 
  }

  #result-screen { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background: #000; z-index: 100; display: none; flex-direction: column; align-items: center; justify-content: center; color: #FFF; }
  .res-title { color: #FFF; font-size: 20px; margin-bottom: 25px; }
  .res-line { font-size: 11px; margin: 8px 0; width: 85%; display: flex; justify-content: space-between; font-family: sans-serif; font-weight: bold; }
  .res-score-big { font-size: 32px; color: #FFD700; margin: 15px 0; text-shadow: 3px 3px #E76900; }

  #control-area { padding-bottom: 80px; }
  button { font-family: 'Press Start 2P', cursive; font-size: 18px; padding: 15px 30px; background: #E76900; color: white; border: 4px solid #000; border-radius: 10px; cursor: pointer; }
  .start-prac-btn { font-size: 12px; background: #5C9400; margin-top: 20px; }
</style>
</head>
<body onclick="resumeAudio()">

<audio id="bgmMusic" loop preload="auto">
  <source src="https://thealphabeat.github.io/Super-Mario-Bros/audio/01-main-theme-overworld.mp3" type="audio/mpeg">
</audio>

<div class="cloud c1"></div>
<div class="cloud c2"></div>

<div id="rule-overlay">
  <div class="rule-box">
    <div style="text-align:center; color:#FFD700; margin-bottom:20px; font-size:16px;">HOW TO PLAY</div>
    <div class="rule-item">
      <div class="rule-icon" style="color:#FFF;">X</div>
      <div>白色 X：<br><span style="color:#FFD700;">按 1 下</span></div>
    </div>
    <div class="rule-item">
      <div class="rule-icon" style="color:#FFD700;">A</div>
      <div>黃色字母：<br><span style="color:#FFD700;">連按 2 下</span></div>
    </div>
    <div class="rule-item">
      <div class="rule-icon" style="color:#FF4444;">X</div>
      <div>紅/其他：<br><span style="color:#FF4444;">🚫 不要按</span></div>
    </div>
    <div style="font-size:10px; color:#FFD700; margin:10px 0;">練習模式不計入最終分數！</div>
    <center><button class="start-prac-btn" onclick="startPractice()">開始練習！</button></center>
  </div>
</div>

<div id="ui-layer">
  <div id="progress-container"><div id="progress-bar"></div></div>
  <div id="instruction-sticky">
    <span class="rule-tag rule-white">⚪白色X:1下</span>
    <span class="rule-tag rule-yellow">🟡黃色:2下</span>
    <span class="rule-tag rule-red">🚫其他:不按</span>
  </div>

  <div id="game-header">
    <div>TOP: <span id="top-score">000000</span></div>
    <div id="score-display">000000</div>
  </div>
  
  <div id="letter-container">
    <div id="mode-tag">TRIAL MODE</div>
    <div id="letter">專注力<br>測驗</div>
    
    <div id="result-screen">
        <div class="res-title">COURSE CLEAR !</div>
        <div class="res-line" style="color:#4CAF50;"><span>CORRECT</span> <span id="res-hit">0</span></div>
        <div class="res-line" style="color:#FF4444;"><span>MISTAKES</span> <span id="res-mistake">0</span></div>
        <div class="res-line" style="color:#FFA500;"><span>MISSED</span> <span id="res-missed">0</span></div>
        <div class="res-line"><span>AVG SPEED</span> <span id="res-rt">0ms</span></div>
        <div class="res-score-big" id="res-score">000000</div>
        <div style="margin-top:20px; font-size:10px; color:#aaa;">REFRESH TO PLAY AGAIN</div>
    </div>
  </div>

  <div id="control-area">
    <button id="startBtn" onclick="showRules()">PUSH START</button>
  </div>
</div>

<div id="mario-container"><div id="mario-sprite"></div></div>
<div id="grass"></div>

<script>
let score, targetScore, currentDisplayScore, mistakes, missed, total, running, canClick, clickCount;
let currentIsTarget, currentIsYellow, timer, startTime, reactionTimes = [];
let lastWasTarget = false, combo = 0, isPractice = true;
let tickerId = null;

const bgm = document.getElementById('bgmMusic');
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
const marioSprite = document.getElementById('mario-sprite');
const scoreEl = document.getElementById("score-display");
const modeTag = document.getElementById("mode-tag");
const ruleOverlay = document.getElementById("rule-overlay");

window.onload = () => {
    const top = localStorage.getItem("mario_top_score") || "000000";
    document.getElementById("top-score").innerText = top.toString().padStart(6, '0');
};

function resumeAudio() { if (audioCtx.state === 'suspended') audioCtx.resume(); }

function showRules() {
    resumeAudio();
    document.getElementById("startBtn").style.display = "none";
    ruleOverlay.style.display = "flex";
}

function startPractice() {
    ruleOverlay.style.display = "none";
    bgm.currentTime = 0; bgm.playbackRate = 1.0; bgm.play().catch(()=>{});
    marioSprite.style.animationPlayState = 'running';
    // 初始化所有數據
    score = 0; targetScore = 0; currentDisplayScore = 0; mistakes = 0; missed = 0; total = 0; combo = 0;
    reactionTimes = []; running = true; isPractice = true;
    modeTag.style.display = "block";
    modeTag.innerText = "TRIAL: 10 ROUNDS";
    startScoreTicker();
    nextRound();
}

function playTone(freq, type, duration, vol) {
    const osc = audioCtx.createOscillator();
    const g = audioCtx.createGain();
    osc.type = type;
    osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
    g.gain.setValueAtTime(vol, audioCtx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
    osc.connect(g); g.connect(audioCtx.destination);
    osc.start(); osc.stop(audioCtx.currentTime + duration);
}

const sounds = {
    coin: () => { playTone(987, 'square', 0.1, 0.05); setTimeout(() => playTone(1318, 'square', 0.2, 0.05), 50); },
    bump: () => playTone(150, 'sawtooth', 0.1, 0.1),
    fall: () => playTone(200, 'triangle', 0.3, 0.1),
    levelUp: () => { playTone(523, 'square', 0.1, 0.1); setTimeout(()=>playTone(659, 'square', 0.1, 0.1), 100); setTimeout(()=>playTone(783, 'square', 0.3, 0.1), 200); }
};

function startScoreTicker() {
    if (tickerId
