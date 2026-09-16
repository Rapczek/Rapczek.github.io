<!DOCTYPE html>
<html lang="pl">
<head>
<meta charset="UTF-8">
<title>Countdown</title>
<style>
  :root{
    --red: #ff0033;
    --red-glow: #ff003377;
    --bg: #060606;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html,body{
    height:100%;
    background: var(--bg);
    background-image:
      radial-gradient(circle at 20% 20%, rgba(255,0,51,0.08), transparent 40%),
      radial-gradient(circle at 80% 80%, rgba(255,0,51,0.06), transparent 45%),
      repeating-linear-gradient(0deg, rgba(255,255,255,0.015) 0px, rgba(255,255,255,0.015) 1px, transparent 1px, transparent 3px);
    color: var(--red);
    font-family: 'Courier New', monospace;
    display:flex;
    align-items:center;
    justify-content:center;
    overflow:hidden;
  }
  .wrap{
    text-align:center;
    padding: 40px;
    position:relative;
    z-index: 2;
  }
  .label{
    letter-spacing: 8px;
    font-size: clamp(12px, 2vw, 18px);
    color: #ff003399;
    text-transform: uppercase;
    margin-bottom: 20px;
    font-weight: bold;
  }
  .target-date{
    color: #ffffff;
    font-size: clamp(16px, 2.5vw, 24px);
    letter-spacing: 4px;
    margin-bottom: 50px;
    opacity: 0.85;
  }
  .timer{
    display:flex;
    gap: clamp(15px, 4vw, 50px);
    justify-content:center;
    flex-wrap: wrap;
  }
  .unit{
    display:flex;
    flex-direction:column;
    align-items:center;
  }
  .num{
    font-size: clamp(48px, 12vw, 140px);
    font-weight: 900;
    color: var(--red);
    text-shadow:
      0 0 10px var(--red-glow),
      0 0 30px var(--red-glow),
      0 0 60px var(--red-glow),
      0 0 100px rgba(255,0,51,0.3);
    line-height: 1;
    font-variant-numeric: tabular-nums;
    animation: flicker 4s infinite;
  }
  .unit-label{
    margin-top: 10px;
    font-size: clamp(10px, 1.5vw, 16px);
    letter-spacing: 6px;
    color: #ffffff88;
    text-transform: uppercase;
  }
  .sep{
    font-size: clamp(48px, 12vw, 140px);
    font-weight: 900;
    color: var(--red);
    opacity: 0.3;
    align-self: flex-start;
  }
  @keyframes flicker{
    0%, 92%, 100% { opacity: 1; }
    93% { opacity: 0.75; }
    94% { opacity: 1; }
    95% { opacity: 0.6; }
    96% { opacity: 1; }
  }
  .done{
    font-size: clamp(28px, 6vw, 60px);
    color: #fff;
    text-shadow: 0 0 20px var(--red-glow);
    letter-spacing: 4px;
  }
  .scanline{
    position: fixed;
    top:0; left:0; right:0;
    height: 2px;
    background: linear-gradient(90deg, transparent, rgba(255,0,51,0.5), transparent);
    animation: scan 6s linear infinite;
    z-index: 1;
  }
  @keyframes scan{
    0%{ top: -2px; }
    100%{ top: 100%; }
  }
  .vignette{
    position: fixed;
    inset: 0;
    pointer-events: none;
    box-shadow: inset 0 0 200px rgba(0,0,0,0.9);
    z-index: 3;
  }
</style>
</head>
<body>

<div class="scanline"></div>
<div class="vignette"></div>

<div class="wrap">
  <div class="timer" id="timer">
    <div class="unit"><div class="num" id="days">00</div><div class="unit-label">dni</div></div>
    <div class="sep">:</div>
    <div class="unit"><div class="num" id="hours">00</div><div class="unit-label">godz</div></div>
    <div class="sep">:</div>
    <div class="unit"><div class="num" id="minutes">00</div><div class="unit-label">min</div></div>
    <div class="sep">:</div>
    <div class="unit"><div class="num" id="seconds">00</div><div class="unit-label">sek</div></div>
  </div>
</div>

<script>
  // Ustaw datę docelową: 29 września bieżącego (lub przyszłego, jeśli już minęła) roku, 19:00 czasu lokalnego
  function getTargetDate(){
    const now = new Date();
    let year = now.getFullYear();
    let target = new Date(year, 8, 29, 10, 0, 0); // miesiące liczone od 0, więc 8 = wrzesień
    if(target.getTime() < now.getTime()){
      target = new Date(year + 1, 8, 29, 10, 0, 0);
    }
    return target;
  }

  const targetDate = getTargetDate();

  function pad(n){ return String(n).padStart(2, '0'); }

  function update(){
    const now = new Date().getTime();
    const distance = targetDate.getTime() - now;

    if(distance <= 0){
      document.getElementById('timer').innerHTML = '<div class="done">CZAS MINĄŁ</div>';
      clearInterval(interval);
      return;
    }

    const days = Math.floor(distance / (1000*60*60*24));
    const hours = Math.floor((distance % (1000*60*60*24)) / (1000*60*60));
    const minutes = Math.floor((distance % (1000*60*60)) / (1000*60));
    const seconds = Math.floor((distance % (1000*60)) / 1000);

    document.getElementById('days').textContent = pad(days);
    document.getElementById('hours').textContent = pad(hours);
    document.getElementById('minutes').textContent = pad(minutes);
    document.getElementById('seconds').textContent = pad(seconds);
  }

  update();
  const interval = setInterval(update, 1000);
</script>

</body>
</html>
