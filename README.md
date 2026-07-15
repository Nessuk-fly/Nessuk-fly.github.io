<!DOCTYPE html>
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
}
input{
    width:60px;
    font-size:18px;
    text-align:center;
    margin:5px;
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

<div>
<input type="number" id="minutes" min="0" value="0"> :
<input type="number" id="secondsInput" min="0" max="59" value="45">
</div>

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
}

function beep(){
    const audio=new (window.AudioContext||window.webkitAudioContext)();
    const osc=audio.createOscillator();
    const gain=audio.createGain();

    osc.connect(gain);
    gain.connect(audio.destination);

    osc.frequency.value=1000;
    osc.start();

    gain.gain.exponentialRampToValueAtTime(
        0.0001,
        audio.currentTime+0.25
    );

    osc.stop(audio.currentTime+0.25);
}

function start(){
    clearInterval(interval);

    let m=parseInt(document.getElementById("minutes").value)||0;
    let s=parseInt(document.getElementById("secondsInput").value)||0;

    seconds=m*60+s;
    update();

    interval=setInterval(()=>{
        if(seconds>0){
            seconds--;
            update();
        }else{
            clearInterval(interval);
            beep();
        }
    },1000);
}

function reset(){
    clearInterval(interval);
    let m=parseInt(document.getElementById("minutes").value)||0;
    let s=parseInt(document.getElementById("secondsInput").value)||0;
    seconds=m*60+s;
    update();
}

update();
</script>

</body>
</html>
