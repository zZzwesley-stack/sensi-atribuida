<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>SENSI ELITE</title>

<style>
body{
  margin:0;
  background:#050505;
  font-family:Arial;
  display:flex;
  justify-content:center;
  align-items:center;
  height:100vh;
  color:#00ff9c;
  overflow:hidden;
}

/* PARTICULAS */
canvas{
  position:fixed;
  top:0;
  left:0;
}

/* 4:3 */
.wrapper{
  aspect-ratio:4/3;
  width:min(95vw,900px);
  z-index:2;
}

.panel{
  width:100%;
  height:100%;
  background:#000;
  border-radius:20px;
  box-shadow:0 0 40px #00ff9c;
  padding:15px;
  display:flex;
  flex-direction:column;
  justify-content:space-between;
}

/* TOPO */
.top{
  display:flex;
  justify-content:space-between;
  font-size:12px;
}

.title{
  text-align:center;
  text-shadow:0 0 10px #00ff9c;
}

/* SELECT */
select{
  width:100%;
  padding:6px;
  margin:3px 0;
  background:#111;
  color:#00ff9c;
  border:1px solid #00ff9c;
}

/* CONTROLES */
.control{
  font-size:11px;
}

input[type=range]{
  width:100%;
}

/* BOTÕES */
.btn{
  padding:8px;
  border:none;
  border-radius:10px;
  background:#00ff9c;
  font-weight:bold;
  cursor:pointer;
  margin-top:4px;
}

/* STATUS */
.status{
  font-size:11px;
}

.bar{
  height:6px;
  background:#111;
}

.fill{
  height:100%;
  width:0%;
  background:#00ff9c;
}

.footer{
  text-align:right;
  font-size:10px;
}
</style>
</head>

<body>

<canvas id="bg"></canvas>

<div class="wrapper">
<div class="panel">

<div class="top">
<div id="fps">FPS:60</div>
<div id="clock"></div>
<div>ONLINE</div>
</div>

<div class="title">
SENSI E OTIMIZAÇÃO<br>
SYSTEM CONTROL PANEL
</div>

<select id="marca" onchange="updateModel()">
<option>iPhone</option>
<option>Android</option>
<option>Xiaomi</option>
<option>Samsung</option>
<option>Motorola</option>
<option>Emulador</option>
</select>

<select id="modelo" onchange="applySensi()"></select>

<div>

<div class="control">Geral: <span id="v_geral">0</span>
<input id="geral" type="range" min="0" max="200" disabled></div>

<div class="control">Red Dot: <span id="v_red">0</span>
<input id="red" type="range" min="0" max="200" disabled></div>

<div class="control">2x: <span id="v_x2">0</span>
<input id="x2" type="range" min="0" max="200" disabled></div>

<div class="control">4x: <span id="v_x4">0</span>
<input id="x4" type="range" min="0" max="200" disabled></div>

<div class="control">AWM: <span id="v_awm">0</span>
<input id="awm" type="range" min="0" max="200" disabled></div>

<div class="control">DPI: <span id="v_dpi">0</span>
<input id="dpi" type="range" min="100" max="1000" disabled></div>

</div>

<button class="btn" onclick="liberar()">🔓 Liberar edição</button>
<button class="btn" onclick="copiar()">📋 Copiar Sensi</button>
<button class="btn" onclick="start()">🚀 Iniciar Free Fire</button>

<div class="status" id="status">Sistema aguardando...</div>
<div class="bar"><div class="fill" id="fill"></div></div>

<div class="footer">@__wesleyxn_7</div>

</div>
</div>

<script>

/* PARTICULAS */
let c=document.getElementById("bg");
let ctx=c.getContext("2d");
c.width=innerWidth;
c.height=innerHeight;

let p=[];
for(let i=0;i<80;i++){
 p.push({x:Math.random()*c.width,y:Math.random()*c.height});
}

setInterval(()=>{
 ctx.clearRect(0,0,c.width,c.height);
 ctx.fillStyle="#00ff9c";
 p.forEach(e=>{
  ctx.fillRect(e.x,e.y,2,2);
  e.y+=1;
  if(e.y>c.height)e.y=0;
 });
},30);

/* FPS */
setInterval(()=>{
 fps.innerText="FPS:"+(55+Math.random()*10|0);
},1000);

/* CLOCK */
setInterval(()=>{
 clock.innerText=new Date().toLocaleTimeString();
},1000);

/* GERADOR */
function gen(n,tipo){
 let a=[];
 for(let i=0;i<n;i++){
  if(tipo==="emu"){
    a.push({
      geral:Math.random()*30|0,
      red:Math.random()*30|0,
      x2:Math.random()*30|0,
      x4:Math.random()*30|0,
      awm:Math.random()*30|0,
      dpi:400+Math.random()*200|0
    });
  } else {
    a.push({
      geral:120+Math.random()*80|0,
      red:110+Math.random()*80|0,
      x2:100+Math.random()*80|0,
      x4:90+Math.random()*80|0,
      awm:80+Math.random()*80|0,
      dpi:500+Math.random()*500|0
    });
  }
 }
 return a;
}

/* BANCO */
const banco={
 iPhone:gen(150,"m"),
 Android:gen(150,"m"),
 Xiaomi:gen(150,"m"),
 Samsung:gen(150,"m"),
 Motorola:gen(150,"m")
};

const emu={
 Marechal:gen(5,"emu"),
 Fantasma:gen(5,"emu"),
 ChengFFX:gen(5,"emu"),
 Haru:gen(5,"emu"),
 DanRLQ:gen(5,"emu"),
 Ferreira7FFH4X:gen(5,"emu"),
 DudsFFX:gen(5,"emu"),
 NotDeed:gen(5,"emu"),
 WLSurf:gen(5,"emu"),
 KillaURLQ:gen(5,"emu")
};

/* UPDATE */
function updateModel(){
 modelo.innerHTML="";
 if(marca.value==="Emulador"){
  for(let f in emu){
   let o=document.createElement("option");
   o.text=f;
   modelo.add(o);
  }
 } else {
  for(let i=1;i<=150;i++){
   let o=document.createElement("option");
   o.text=marca.value+" "+i;
   modelo.add(o);
  }
 }
 applySensi();
}

/* APPLY */
function applySensi(){
 let s;
 if(marca.value==="Emulador"){
  s=emu[modelo.value][Math.random()*5|0];
 } else {
  s=banco[marca.value][Math.random()*150|0];
 }

 geral.value=s.geral;
 red.value=s.red;
 x2.value=s.x2;
 x4.value=s.x4;
 awm.value=s.awm;
 dpi.value=s.dpi;

 v_geral.innerText=s.geral;
 v_red.innerText=s.red;
 v_x2.innerText=s.x2;
 v_x4.innerText=s.x4;
 v_awm.innerText=s.awm;
 v_dpi.innerText=s.dpi;

 status.innerText="Sensi carregada...";
}

/* LIBERAR */
function liberar(){
 document.querySelectorAll("input").forEach(e=>e.disabled=false);
 status.innerText="Edição liberada...";
}

/* COPIAR */
function copiar(){
 let txt=`Geral:${geral.value} Red:${red.value} 2x:${x2.value} 4x:${x4.value} AWM:${awm.value} DPI:${dpi.value}`;
 navigator.clipboard.writeText(txt);
 status.innerText="Sensi copiada!";
}

/* START */
function start(){
 let s=["Iniciando sistema...","Carregando módulos...","Sincronizando...","Conectado ✔"];
 let i=0;
 function r(){
  if(i<s.length){
    status.innerText=s[i];
    fill.style.width=(i+1)*25+"%";
    i++;
    setTimeout(r,700);
  } else {
    window.open("https://play.google.com/store/apps/details?id=com.dts.freefireth");
  }
 }
 r();
}

updateModel();

</script>

</body>
</html>
