<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>꧁༒Kairox༒꧂ Giveaway</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600&display=swap" rel="stylesheet">
<style>
body {
    font-family:'Orbitron',sans-serif;
    background: radial-gradient(circle at top,#1a1a2e,#0f0f0f);
    color:white;
    text-align:center;
    margin:0;
}
h1 { margin-top:30px; color:#00f7ff; text-shadow:0 0 15px #00f7ff; }
.container {
    width:90%; max-width:400px; margin:20px auto;
    background: rgba(255,255,255,0.05);
    padding:20px; border-radius:20px;
    box-shadow:0 0 25px rgba(0,255,255,0.3);
}
input { padding:10px; width:80%; border-radius:8px; border:none; margin-bottom:10px; text-align:center; }
button { padding:10px 20px; border-radius:8px; border:none; margin:5px; cursor:pointer; background: linear-gradient(45deg,#ff00cc,#3333ff); color:white; }
button:hover { transform:scale(1.1);}
#participants { margin-top:15px; padding:10px; background: rgba(0,0,0,0.2); border-radius:10px; max-height:200px; overflow-y:auto; text-align:left; }
.participant { padding:5px 10px; border-radius:5px; }
.highlight { background:#00ff88; color:black; font-weight:bold; }
#winner { margin-top:20px; font-size:22px; color:#00ff88; text-shadow:0 0 20px #00ff88; }
#timer { color:#ffcc00; margin-bottom:10px; font-size:16px; }
</style>
</head>
<body>

<h1>🔥 ꧁༒Kairox༒꧂ Giveaway 🔥</h1>

<div class="container">
<p>⏳ Temps restant : <span id="timer"></span></p>

<input type="text" id="pseudo" placeholder="Entre ton pseudo">
<br>
<button onclick="addParticipant()">🎮 Participer</button>

<hr style="opacity:0.3;margin:20px 0;">

<input type="password" id="adminPass" placeholder="Mot de passe admin">
<br>
<button onclick="startDraw()">🎬 Tirage (Admin)</button>

<div id="participants">Aucun participant pour le moment.</div>
<div id="winner"></div>
</div>

<script>
let participants = JSON.parse(localStorage.getItem("participants")) || [];

function saveParticipants(){ localStorage.setItem("participants", JSON.stringify(participants)); }

function updateParticipants(){
    let div = document.getElementById("participants");
    if(participants.length===0){ div.innerHTML="Aucun participant pour le moment."; }
    else {
        div.innerHTML="";
        participants.forEach((p,i)=>{
            let pDiv=document.createElement("div");
            pDiv.textContent=p;
            pDiv.className="participant";
            pDiv.id="participant-"+i;
            div.appendChild(pDiv);
        });
    }
}

function addParticipant(){
    let pseudo=document.getElementById("pseudo").value.trim();
    if(!pseudo) return alert("Entre un pseudo !");
    if(participants.includes(pseudo)) return alert("Pseudo déjà inscrit !");
    participants.push(pseudo);
    saveParticipants();
    updateParticipants();
    document.getElementById("pseudo").value="";
}

const ADMIN_PASSWORD="TWISTONE123"; // 🔥 change ton mot de passe

function startDraw(){
    let pass=document.getElementById("adminPass").value;
    if(pass!==ADMIN_PASSWORD) return alert("Mot de passe incorrect !");
    if(participants.length===0) return alert("Aucun participant !");

    let winnerDiv=document.getElementById("winner");
    winnerDiv.textContent="";
    let index=0, cycles=0, totalCycles=20+Math.floor(Math.random()*20);
    let interval=setInterval(()=>{
        participants.forEach((_,i)=>{
            let elem=document.getElementById("participant-"+i);
            if(elem) elem.classList.remove("highlight");
        });
        let elem=document.getElementById("participant-"+index);
        if(elem) elem.classList.add("highlight");
        index=(index+1)%participants.length;
        cycles++;
        if(cycles>totalCycles){
            clearInterval(interval);
            let winnerIndex=(index-1+participants.length)%participants.length;
            winnerDiv.textContent="🏆 GAGNANT : "+participants[winnerIndex]+" 🎉";
        }
    },100);
}

// --- TIMER 24h ---
let endTime=localStorage.getItem("endTime");
if(!endTime){ endTime=new Date().getTime()+24*60*60*1000; localStorage.setItem("endTime",endTime); }
setInterval(()=>{
    let now=new Date().getTime();
    let distance=endTime-now;
    if(distance<0) document.getElementById("timer").textContent="Terminé";
    else {
        let h=Math.floor((distance%(1000*60*60*24))/(1000*60*60));
        let m=Math.floor((distance%(1000*60*60))/(1000*60));
        let s=Math.floor((distance%(1000*60))/1000);
        document.getElementById("timer").textContent=h+"h "+m+"m "+s+"s";
    }
},1000);

updateParticipants();
</script>

</body>
</html>
