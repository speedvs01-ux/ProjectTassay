<!DOCTYPE html>

<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Windows Security Center</title>

<style>
* { box-sizing: border-box; }

body {
    margin: 0;
    background: #101827;
    color: white;
    font-family: Arial, sans-serif;
}

.page {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
}

.card {
    background: #202e42;
    width: 100%;
    max-width: 420px;
    padding: 30px 22px;
    border-radius: 20px;
    text-align: center;
}

.logo { font-size: 55px; }

button {
    border: 0;
    padding: 16px;
    border-radius: 10px;
    width: 100%;
    font-size: 17px;
    font-weight: bold;
    cursor: pointer;
    margin-top: 20px;
    background: #1675ed;
    color: white;
}

#prank {
    display: none;
    background: #170000;
    min-height: 100vh;
    padding: 20px;
    overflow: hidden;
}

#prank h1 {
    color: red;
    text-align: center;
    font-size: clamp(35px, 10vw, 65px);
    animation: blink .3s infinite alternate;
}

@keyframes blink {
    to { opacity: .3; }
}

.error {
    background: #ddd;
    color: black;
    margin: 10px auto;
    width: min(100%, 300px);
    border: 3px solid #777;
    box-shadow: 0 0 15px red;
}

.error-title {
    background: darkred;
    color: white;
    padding: 9px;
    font-weight: bold;
}

.error-content {
    padding: 15px;
}

#stop {
    background: #333;
}
</style>

</head>

<body>

<div class="page" id="welcome">
    <div class="card">
        <div class="logo">🛡️</div>

```
    <h2>Windows Security Center</h2>

    <p>Центр безопасности устройства</p>

    <p>Готовы проверить безопасность вашего устройства?</p>

    <div>🟢 Система готова к проверке</div>

    <button onclick="startPrank()">
        НАЧАТЬ ПРОВЕРКУ
    </button>

    <p style="font-size:12px;color:#aaa">
        Демонстрационный розыгрыш
    </p>
</div>
```

</div>

<div id="prank">
    <h1>SYSTEM FAILURE</h1>

```
<h2 style="text-align:center;color:red">
    CRITICAL ERROR DETECTED!
</h2>

<div id="errors"></div>

<button id="stop" onclick="stopPrank()">
    ОСТАНОВИТЬ ЗВУК И ЭФФЕКТ
</button>
```

</div>

<script>
let timer;
let audio;
let oscillator;
let gain;
let soundTimer;

function startPrank() {
    document.getElementById("welcome").style.display = "none";
    document.getElementById("prank").style.display = "block";

    // Звук после нажатия
    try {
        audio = new (window.AudioContext || window.webkitAudioContext)();

        audio.resume().then(() => {
            oscillator = audio.createOscillator();
            gain = audio.createGain();

            oscillator.type = "sawtooth";
            oscillator.frequency.value = 180;

            gain.gain.value = 0.25;

            oscillator.connect(gain);
            gain.connect(audio.destination);

            oscillator.start();

            soundTimer = setInterval(() => {
                oscillator.frequency.setValueAtTime(
                    120 + Math.random() * 500,
                    audio.currentTime
                );
            }, 300);
        });
    } catch(e) {}

    let count = 0;

    timer = setInterval(() => {
        if (count >= 20) {
            clearInterval(timer);
            return;
        }

        let box = document.createElement("div");
        box.className = "error";

        box.innerHTML = `
            <div class="error-title">⚠ SYSTEM ERROR</div>
            <div class="error-content">
                <b>ERROR 0x000${count + 1}</b>
                <p>CRITICAL SYSTEM FAILURE!</p>
                <button onclick="this.parentElement.parentElement.remove()">
                    OK
                </button>
            </div>
        `;

        document.getElementById("errors").prepend(box);
        count++;
    }, 400);
}

function stopPrank() {
    clearInterval(timer);
    clearInterval(soundTimer);

    if (oscillator) {
        try { oscillator.stop(); } catch(e) {}
    }

    if (audio) audio.close();

    document.getElementById("prank").style.display = "none";
    document.getElementById("welcome").style.display = "flex";
}
</script>

</body>
</html>
