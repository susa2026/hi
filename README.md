<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Quotex Signal Generator</title>
<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(to right, #1e1e2f, #0f0f1a);
    color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding-top: 40px;
    min-height: 100vh;
}

h1 { margin-bottom: 20px; text-shadow: 2px 2px 8px #000; text-align: center; font-size: 28px; }

.pair-select { margin-bottom: 20px; font-size: 18px; padding: 10px; border-radius: 8px; border: none; width: 90%; max-width: 300px; }

.buttons, .signal-btn { display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap; justify-content: center; }

button {
    padding: 12px 16px;
    font-size: 16px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    color: #fff;
    box-shadow: 2px 2px 6px #000;
    transition: all 0.2s ease;
    flex: 1 1 100px;
    min-width: 80px;
}

/* Different background colors for each duration button */
button[data-duration="5s"] { background: #e74c3c; }      /* red */
button[data-duration="10s"] { background: #f39c12; }     /* orange */
button[data-duration="15s"] { background: #3498db; }     /* blue */
button[data-duration="1m"] { background: #9b59b6; }      /* purple */
button[data-duration="5m"] { background: #16a085; }      /* teal */

button#activeButton { opacity: 0.8; transform: scale(1.05); }

button#getSignalBtn { background: #2ecc71; }              /* green */

button:hover:enabled { filter: brightness(1.2); transform: scale(1.05); }
button:disabled { background: #555566; cursor: not-allowed; }

.signal {
    font-size: 36px;
    font-weight: bold;
    padding: 20px 20px;
    border-radius: 12px;
    box-shadow: 4px 4px 15px #000;
    text-shadow: 2px 2px 6px #000;
    min-width: 200px;
    max-width: 90%;
    text-align: center;
    background: rgba(255,255,255,0.1);
    word-wrap: break-word;
}

.signal.success { color: #2ecc71; }

@media (max-width: 480px) {
    h1 { font-size: 24px; }
    .pair-select { font-size: 16px; padding: 8px; }
    button { font-size: 14px; padding: 10px; flex: 1 1 80px; }
    .signal { font-size: 28px; padding: 16px; }
}
</style>
</head>
<body>

<h1>Quotex Random Signal Generator</h1>

<select id="pairSelect" class="pair-select">
    <option value="BTC/USD">BTC/USD</option>
    <option value="USD/BRL">USD/BRL</option>
    <option value="USD/INR">USD/INR</option>
    <option value="BDT/USD">BDT/USD</option>
</select>

<div class="buttons">
    <button data-duration="5s" onclick="setDuration('5s', this)">5 Seconds</button>
    <button data-duration="10s" onclick="setDuration('10s', this)">10 Seconds</button>
    <button data-duration="15s" onclick="setDuration('15s', this)">15 Seconds</button>
    <button data-duration="1m" onclick="setDuration('1m', this)">1 Minute</button>
    <button data-duration="5m" onclick="setDuration('5m', this)">5 Minutes</button>
</div>

<div class="signal-btn">
    <button id="getSignalBtn" onclick="getSignal()">Get Signal</button>
</div>

<div class="signal" id="signalDisplay">---</div>

<script>
let selectedDuration = '1m';
let activeButton = null;

function setDuration(duration, btn){
    selectedDuration = duration;
    document.getElementById('signalDisplay').innerText = `Duration set: ${duration}`;
    if(activeButton) activeButton.style.border = 'none';
    btn.style.border = '2px solid #fff';
    activeButton = btn;
}

function getSignal(){
    const display = document.getElementById('signalDisplay');
    const btn = document.getElementById('getSignalBtn');

    display.innerText = '';
    display.classList.remove('success');

    // 30 seconds cooldown for button
    btn.disabled = true;
    let remaining = 30;
    btn.innerText = `Wait ${remaining}s`;

    const interval = setInterval(() => {
        remaining--;
        if(remaining > 0){
            btn.innerText = `Wait ${remaining}s`;
        } else {
            clearInterval(interval);
            btn.disabled = false;
            btn.innerText = 'Get Signal';
            display.innerText = 'Success';
            display.classList.add('success');
            setTimeout(() => {
                display.innerText = '---';
                display.classList.remove('success');
            }, 1000);
        }
    }, 1000);
}
</script>

</body>
</html>
