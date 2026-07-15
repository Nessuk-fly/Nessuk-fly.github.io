<CZASOMIERZ>
<html lang="pl">
<head>
<meta charset="UTF-8">
<title>Timer</title>
<style>
body{
    margin:0;
    background:#191919;
    color:white;
    font-family:Arial,sans-serif;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:center;
    height:100vh;
}
#timer{
    font-size:72px;
    font-weight:bold;
    margin-bottom:20px;
    <div style="margin-bottom:15px;">
<input id="timeInput" type="number" value="45" min="1" style="width:70px;font-size:18px;text-align:center;">
<button onclick="setTime()">Ustaw</button>
</div>
}
button{
    font-size:18px;
    padding:10px 20px;
    margin:5px;
    border:none;
    border-radius:8px;
    cursor:pointer;
}
</style>
</head>
<body>

<div id="timer">00:45</div>

<div>
<button onclick="start()">Start</button>
<button onclick="reset()">Reset</button>
</div>

<script>
let seconds=45;
let interval;

function update(){
    let m=Math.floor(seconds/60);
    let s=seconds%60;
    document.getElementById("timer").textContent=
    String(m).padStart(2,"0")+":"+
    String(s).padStart(2,"0");

    function setTime(){
    clearInterval(interval);
    seconds = parseInt(document.getElementById("timeInput").value);
    update();
    }
}

function start(){
    clearInterval(interval);
    interval=setInterval(()=>{
        if(seconds>0){
            seconds--;
            update();
        }else{
            clearInterval(interval);
beep();;
        }
    },1000);
}

function reset(){
    clearInterval(interval);
    seconds = parseInt(document.getElementById("timeInput").value);
    update();
}
    function beep(){
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = ctx.createOscillator();
    osc.connect(ctx.destination);
    osc.frequency.value = 800;
    osc.start();
    osc.stop(ctx.currentTime + 0.3);
    }
}

update();
</script>

</body>
</html>
