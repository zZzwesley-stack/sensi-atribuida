<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>GERADOR DE NICK SUPREMO</title>

<style>

body{
background:#050505;
font-family:Arial;
color:white;
text-align:center;
margin:0;
overflow:hidden;
}

canvas{
position:fixed;
top:0;
left:0;
z-index:-1;
}

.container{
margin-top:60px;
}

h1{
font-size:45px;
color:#ff3300;
text-shadow:0 0 25px red;
}

input{
padding:15px;
font-size:18px;
width:260px;
border:none;
border-radius:10px;
}

button{
padding:15px 30px;
font-size:18px;
border:none;
border-radius:10px;
background:linear-gradient(45deg,#ff0000,#ffcc00);
color:white;
cursor:pointer;
margin-top:10px;
}

#results{
margin-top:30px;
max-height:500px;
overflow:auto;
width:90%;
margin:auto;
}

.nick{
background:#111;
margin:6px;
padding:10px;
border-radius:10px;
font-size:22px;
cursor:pointer;
}

.nick:hover{
background:#1a1a1a;
}

</style>
</head>

<body>

<canvas id="particles"></canvas>

<div class="container">

<h1>🔥 GERADOR DE NICK SUPREMO 🔥</h1>

<input id="name" placeholder="Digite seu nome">

<br>

<button onclick="generate()">GERAR NICKS</button>

<div id="results"></div>

</div>

<script>

const symbols=[
"亗","么","メ","ツ","〆","乂","々","王","乡","义","气","刁","卍","乄","۝","×",
"★","☆","✦","✧","✩","✪","✫","✬","✭","✮","✯",
"⚡","☢","☣","☠","☯","♛","♚","♕","♔",
"༒","༻","༺","ঔ","ৡ","๛","۝","۞","₪",
"꧁","꧂","『","』","《","》","【","】","〈","〉",
"⧼","⧽","⟦","⟧","⟨","⟩","⟪","⟫",
"◥","◤","◣","◢","▲","▼","◆","◇","■","□",
"∞","∆","∇","∑","∏","∂","∫","≈","≠",
"✞","✟","✠","✢","✣","✤","✥","✦","✧",
"☀","☁","☂","☃","☄","☾","♠","♣","♥","♦"
];

const alphabets=[
"abcdefghijklmnopqrstuvwxyz",
"ａｂｃｄｅｆｇｈｉｊｋｌｍｎｏｐｑｒｓｔｕｖｗｘｙｚ",
"αв¢∂єƒgнιנкℓмησρqяѕтυνωχуz",
"ᴀʙᴄᴅᴇғɢʜɪᴊᴋʟᴍɴᴏᴘǫʀsᴛᴜᴠᴡxʏᴢ"
];

function randomSymbol(){
return symbols[Math.floor(Math.random()*symbols.length)];
}

function stylize(name){

let alphabet=alphabets[Math.floor(Math.random()*alphabets.length)];

let result="";

for(let i=0;i<name.length;i++){

let char=name[i].toLowerCase();

let index="abcdefghijklmnopqrstuvwxyz".indexOf(char);

if(index>=0){
result+=alphabet[index] || char;
}else{
result+=char;
}

}

return result;

}

function generate(){

let name=document.getElementById("name").value;

let results=document.getElementById("results");

results.innerHTML="";

for(let i=0;i<1000;i++){

let base=stylize(name);

let formats=[

randomSymbol()+base+randomSymbol(),
randomSymbol()+randomSymbol()+base+randomSymbol(),
randomSymbol()+base+randomSymbol()+randomSymbol(),
"["+base+"]",
"『"+base+"』",
"★"+base+"★",
"亗"+base+"亗",
"꧁"+base+"꧂",
"◥"+base+"◤",
"⟦"+base+"⟧",
"✦"+base+"✦",
"⚡"+base+"⚡"

];

let nick=formats[Math.floor(Math.random()*formats.length)];

let div=document.createElement("div");

div.className="nick";

div.innerText=nick;

div.onclick=()=>{

navigator.clipboard.writeText(nick);

alert("Nick copiado!");

};

results.appendChild(div);

}

}

</script>

<script>

const canvas=document.getElementById("particles");
const ctx=canvas.getContext("2d");

canvas.width=window.innerWidth;
canvas.height=window.innerHeight;

let particles=[];

for(let i=0;i<200;i++){

particles.push({
x:Math.random()*canvas.width,
y:Math.random()*canvas.height,
r:Math.random()*3,
d:Math.random()*1
})

}

function draw(){

ctx.clearRect(0,0,canvas.width,canvas.height);

for(let p of particles){

ctx.beginPath();
ctx.fillStyle=Math.random()>0.5?"red":"yellow";
ctx.arc(p.x,p.y,p.r,0,Math.PI*2,true);
ctx.fill();

p.y+=p.d;

if(p.y>canvas.height)p.y=0;

}

requestAnimationFrame(draw);

}

draw();

</script>

</body>
</html>
