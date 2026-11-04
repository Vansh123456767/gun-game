<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Diwali City Simulator</title>
<style>
html,body{margin:0;height:100%;overflow:hidden;background:#0a0618;font-family:system-ui,"Segoe UI",Arial,sans-serif;color:#fff;user-select:none;-webkit-user-select:none}
#game{position:fixed;inset:0}
.panel{position:fixed;background:rgba(20,10,40,.62);border:1px solid rgba(255,200,100,.35);border-radius:10px;padding:8px 12px;font-size:14px;line-height:1.5;backdrop-filter:blur(3px)}
#hud{left:12px;top:12px;min-width:150px}
#hud b{color:#ffd36b}
#net{right:12px;top:12px;text-align:right}
#help{left:12px;bottom:12px;font-size:12.5px;max-width:470px}
#help kbd{background:#3b2a63;border-radius:4px;padding:0 5px;border:1px solid #6e58a8;font-size:12px}
#prompt{left:50%;bottom:90px;transform:translateX(-50%);display:none;font-size:16px;color:#ffd36b}
#pop{position:fixed;left:50%;top:28%;transform:translate(-50%,-50%);font-size:60px;font-weight:900;color:#ffe066;text-shadow:0 4px 0 #b3195a,0 0 22px #ff9d00;opacity:0;pointer-events:none;letter-spacing:2px;font-family:Impact,"Arial Black",sans-serif}
#pop.go{animation:pop .7s ease-out}
@keyframes pop{0%{opacity:0;transform:translate(-50%,-50%) scale(.4) rotate(-8deg)}20%{opacity:1;transform:translate(-50%,-50%) scale(1.25) rotate(4deg)}100%{opacity:0;transform:translate(-50%,-70%) scale(1) rotate(0)}}
#start{position:fixed;inset:0;background:radial-gradient(circle at 50% 40%,#3a1466 0%,#0a0618 70%);display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;cursor:pointer;z-index:10}
#start h1{font-size:54px;margin:0 0 6px;color:#ffd36b;text-shadow:0 0 24px #ff7a00}
#start p{margin:6px 0;color:#e8dcff;font-size:17px}
#start .btn{margin-top:22px;padding:12px 34px;background:#ff9d00;color:#2a0a40;font-weight:800;border-radius:30px;font-size:20px}
#toast{position:fixed;left:50%;bottom:140px;transform:translateX(-50%);display:none;background:rgba(40,16,70,.9);border:1px solid #ffb020;border-radius:10px;padding:9px 16px;font-size:15px;color:#fff;max-width:min(560px,90vw);text-align:center;z-index:6}
#admBtn{position:fixed;right:12px;bottom:12px;z-index:5;background:rgba(80,20,50,.8);color:#ffb3c4;border:1px solid #ff4d6d;border-radius:8px;padding:5px 10px;font-size:12px;cursor:pointer}
#admin{position:fixed;right:12px;top:76px;bottom:50px;width:min(340px,92vw);display:none;overflow-y:auto;z-index:7;background:rgba(24,10,44,.96);border:2px solid #ff4d6d;border-radius:12px;padding:10px 12px;font-size:13px;line-height:1.45}
#admin .ahead{display:flex;justify-content:space-between;align-items:center;color:#ff8fa8;font-size:15px;margin-bottom:6px}
#admin .ahead span{cursor:pointer;padding:0 6px}
#admin h4{margin:12px 0 5px;color:#ffd36b;font-size:13px;border-bottom:1px solid rgba(255,255,255,.15);padding-bottom:2px}
#admin label{display:block;margin:4px 0;cursor:pointer}
#admin input[type=password],#admin input[type=number]{width:100%;box-sizing:border-box;background:#120826;color:#fff;border:1px solid #6e58a8;border-radius:6px;padding:6px 8px;font-size:14px;margin-bottom:6px}
#admin .arow{display:flex;flex-wrap:wrap;gap:5px;margin:5px 0}
#admin .ab{background:#ff9d00;border:0;border-radius:7px;padding:6px 9px;font-weight:700;font-size:12px;cursor:pointer;color:#2a0a40}
#admin .ab.red{background:#ff4d6d;color:#fff}
#admin .ab.gray{background:#59467f;color:#fff}
#admin .aplayers{background:rgba(255,255,255,.06);border-radius:8px;padding:6px 8px;margin:6px 0;font-size:12px}
#admin .aplayer{align-items:center;justify-content:space-between}
#admin .anote{font-size:11px;opacity:.7}
#admErr{color:#ff8fa8;font-size:12px;min-height:14px}
#kicked{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(10,0,20,.94);z-index:20;text-align:center;font-size:18px}
#talk{position:fixed;left:50%;bottom:48px;transform:translateX(-50%);display:none;z-index:9;background:#241042;border:2px solid #ffb020;border-radius:14px;padding:14px 18px;width:min(460px,92vw);box-shadow:0 8px 30px rgba(0,0,0,.5)}
#talk .who{color:#ffd36b;font-weight:800;font-size:14px;letter-spacing:.5px;margin-bottom:4px}
#talktxt{font-size:19px;margin-bottom:12px}
#talk .opts{display:flex;gap:10px}
#talk .opts button{flex:1;background:#ff9d00;border:0;border-radius:10px;padding:10px 12px;font-weight:800;font-size:16px;cursor:pointer;color:#2a0a40}
#talk .opts button.bye{background:#59467f;color:#fff}
#talk .opts button:hover{filter:brightness(1.12)}
#shop{position:fixed;inset:0;background:rgba(5,0,15,.7);display:none;align-items:center;justify-content:center;z-index:8}
#shopbox{max-height:86vh;overflow-y:auto;background:#241042;border:2px solid #ffb020;border-radius:14px;padding:18px 22px;width:min(480px,92vw)}
#shopbox h2{margin:0 0 4px;color:#ffd36b}
.row{display:flex;align-items:center;justify-content:space-between;gap:10px;padding:8px 0;border-bottom:1px solid rgba(255,255,255,.1)}
.row small{color:#bda9e6;display:block}
.row button,#shopbox .close{background:#ff9d00;border:0;border-radius:8px;padding:7px 14px;font-weight:700;cursor:pointer;color:#2a0a40}
.row button:disabled{background:#59467f;color:#a994cf;cursor:default}
#shopbox .close{margin-top:12px;width:100%}
</style>
</head>
<body>
<div id="game"></div>
<div id="hud" class="panel"><div id="admbadge" style="display:none;color:#ff6b8a;font-weight:800">ADMIN MODE</div>Coins: <b id="coins">0</b><br>Vehicle: <b id="veh">Car</b><br>Condition: <b id="cond">100%</b> &middot; Armor <b id="armor">0</b>/5<br><span id="spd">0 km/h</span></div>
<div id="net" class="panel">Players here: <b id="players">1</b><br><small style="opacity:.75">(open a 2nd tab to test)</small></div>
<div id="help" class="panel">
<kbd>W A S D</kbd>/arrows drive &nbsp; <kbd>Space</kbd> handbrake (plane: climb) &nbsp; <kbd>Shift</kbd> nitro (plane: descend)<br>
<kbd>1</kbd> Car <kbd>2</kbd> Truck <kbd>3</kbd> Scooter <kbd>4</kbd> Bike <kbd>5</kbd> Plane &nbsp; <kbd>F</kbd> rocket volley &nbsp; <kbd>G</kbd> pataka chain<br>
<kbd>X</kbd> get out / get in &nbsp; <kbd>C</kbd> cheer (on foot) &nbsp; <kbd>E</kbd> talk to the shopkeeper (pink Diwali Shop, one is beside the start) &nbsp; <kbd>V</kbd> camera &nbsp; <kbd>M</kbd> mute &nbsp; <kbd>H</kbd> hide help
</div>
<div id="prompt" class="panel">Press E to open the Diwali Shop</div>
<div id="toast"></div>
<div id="pop">BONK!</div>
<button id="admBtn">Admin (F2)</button>
<div id="kicked"><div><h2>You were removed by the admin</h2><p>Reload the page to rejoin.</p></div></div>
<div id="admin">
  <div class="ahead"><b>ADMIN PANEL</b><span id="admClose" title="Close (F2)">&#10005;</span></div>
  <div id="admLock"><p>Enter the admin password to continue.</p><input id="admPw" type="password" placeholder="Admin password" autocomplete="off"><button class="ab" id="admUnlock">Unlock</button><div id="admErr"></div></div>
  <div id="admBody" style="display:none">
    <h4>MONEY</h4>
    <input id="admAmt" type="number" value="100000" step="1000">
    <div class="arow"><button class="ab" id="admAdd">Give me</button><button class="ab" id="admSet">Set my coins</button><button class="ab" id="admAll">Give everyone</button></div>
    <div id="admPlayers" class="aplayers">No other players online.</div>
    <h4>ME</h4>
    <label><input type="checkbox" id="admGod"> God mode (no vehicle damage)</label>
    <label><input type="checkbox" id="admNoclip"> No-clip (drive through everything)</label>
    <label>Speed x<b id="admSpeedV">1</b> <input type="range" id="admSpeed" min="1" max="4" step="0.5" value="1" style="vertical-align:middle"></label>
    <div class="arow"><button class="ab" id="admRepair">Repair vehicle</button><button class="ab" id="admMax">Unlock ALL upgrades</button><button class="ab" id="admShow">Mega fireworks</button></div>
    <div class="arow"><button class="ab gray" id="admTpSpawn">Go to spawn</button><button class="ab gray" id="admTpShop1">Spawn shop</button><button class="ab gray" id="admTpShop2">Main shop</button></div>
    <div class="arow"><button class="ab" id="admCollect">Collect all coins</button><button class="ab" id="admRespawn">Respawn all coins</button><button class="ab" id="admCrowd">Add bystanders</button></div>
    <h4>REMOVE THINGS</h4>
    <label><input type="checkbox" id="admDel"> Delete tool: click any object in the world to remove it (click a player to kick them)</label>
    <div class="arow"><button class="ab red" id="admRmVeh">All traffic</button><button class="ab red" id="admRmObs">All obstacles</button><button class="ab red" id="admRmPpl">All people</button><button class="ab red" id="admRmCoin">All coins</button><button class="ab red" id="admRmTree">All trees</button><button class="ab red" id="admRmBld">All buildings</button><button class="ab red" id="admRmKeep">Shopkeepers</button></div>
    <div class="arow"><button class="ab" id="admUndo">Undo last removal</button><button class="ab gray" id="admLockBtn">Lock admin</button></div>
    <p class="anote">Reloading the page brings back everything you removed.</p>
  </div>
</div>
<div id="talk"><div class="who">SHOPKEEPER</div><div id="talktxt">Namaste! Why are you here?</div><div class="opts"><button id="optBuy">1. Buy patakas</button><button id="optBye" class="bye">2. Bye</button></div></div>
<div id="shop"><div id="shopbox"><h2>Diwali Shop</h2><div style="color:#bda9e6;margin:0 0 4px">Shopkeeper: "Take a look! Fresh patakas and more."</div><div>Coins: <b id="shopcoins">0</b></div><div id="shoplist"></div><button class="close" id="shopclose">Bye</button></div></div>
<div id="start"><h1>Diwali City Simulator</h1><p>Drive, fly, collect coins and light up the night sky.</p><p>Turn your sound on for the full firework + crash audio.</p><div class="btn">Click or press any key to start</div></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
(function(){
'use strict';
if(typeof THREE==='undefined'){
  document.getElementById('start').innerHTML='<h1>Could not load the 3D engine</h1><p>This game needs an internet connection once to load Three.js from cdnjs.cloudflare.com. Please check your connection and reload.</p>';
  return;
}
const $=id=>document.getElementById(id);
const rnd=(a,b)=>a+Math.random()*(b-a);
const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
const TAU=Math.PI*2;
function angDiff(a,b){let d=(b-a)%TAU;if(d>Math.PI)d-=TAU;if(d<-Math.PI)d+=TAU;return d;}

/* ================= SAVE ================= */
const save={coins:0,up:{volley:0,auto:0,nitro:0,gold:0,magnet:0},str:{car:0,truck:0,scooter:0,bike:0,plane:0}};
try{const s=JSON.parse(localStorage.getItem('dcs_save_v2')||'null');if(s){save.coins=s.coins|0;Object.assign(save.up,s.up||{});Object.assign(save.str,s.str||{});}}catch(e){}
function persist(){try{localStorage.setItem('dcs_save_v2',JSON.stringify(save));}catch(e){}}

/* ================= ADMIN SETTINGS ================= */
// Change this password! Press F2 (or the Admin button) in the game and enter it to become admin.
// Set it to '' (empty) to be admin automatically, with no password.
const ADMIN_PASSWORD='diwali-admin';
const ADM={unlocked:false,open:false,god:false,noclip:false,speed:1,del:false};
let kicked=false;
// (hidden feature state) MOON.on = you are on the moon surface world, SPACE.open = the plane has broken through the ceiling
const MOON={on:false,y:1800,r:260,rocks:[],world:null,hidden:[]};
const SPACE={open:false,hold:0,busy:false};
function GY(){return MOON.on?MOON.y:0;} // ground height of whichever world you are in
try{if(ADMIN_PASSWORD===''||sessionStorage.getItem('dcs_admin')==='1')ADM.unlocked=true;}catch(e){if(ADMIN_PASSWORD==='')ADM.unlocked=true;}

/* ================= RENDERER / SCENE ================= */
const PR=Math.min(window.devicePixelRatio||1,2);
const renderer=new THREE.WebGLRenderer({antialias:true});
renderer.setPixelRatio(PR);
renderer.setSize(window.innerWidth,window.innerHeight);
$('game').appendChild(renderer.domElement);
const scene=new THREE.Scene();
scene.background=new THREE.Color(0x0a0618);
scene.fog=new THREE.FogExp2(0x120a2a,0.0055);
const camera=new THREE.PerspectiveCamera(72,window.innerWidth/window.innerHeight,0.5,3000);
scene.add(new THREE.AmbientLight(0x7a68b0,0.95));
const moonLight=new THREE.DirectionalLight(0xaab4ff,0.65);moonLight.position.set(-80,120,60);scene.add(moonLight);
const burstLight=new THREE.PointLight(0xffffff,0,220);scene.add(burstLight);
let burstLightI=0;

const mat=c=>new THREE.MeshLambertMaterial({color:c,flatShading:true});
const basic=(c)=>new THREE.MeshBasicMaterial({color:c});
const darkM=mat(0x17171d),glassM=mat(0x86c0ea),skinM=mat(0xc68a5a),lightM=basic(0xfff2b0),tailM=basic(0xff3030),roofM=mat(0x555060);
function box(w,h,d,m,x,y,z,parent){const me=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),m);me.position.set(x,y,z);parent.add(me);return me;}
function wheel(r,wd,x,y,z,parent){const me=new THREE.Mesh(new THREE.CylinderGeometry(r,r,wd,10),darkM);me.rotation.z=Math.PI/2;me.position.set(x,y,z);parent.add(me);return me;}

// stars + moon
(function(){
  const n=700,p=new Float32Array(n*3);
  for(let i=0;i<n;i++){const u=Math.random(),a=Math.random()*TAU,y=0.05+u*0.95,s=Math.sqrt(1-y*y);p[i*3]=Math.cos(a)*s*1200;p[i*3+1]=y*1200;p[i*3+2]=Math.sin(a)*s*1200;}
  const g=new THREE.BufferGeometry();g.setAttribute('position',new THREE.BufferAttribute(p,3));
  scene.add(new THREE.Points(g,new THREE.PointsMaterial({color:0xffffff,size:2,sizeAttenuation:false,fog:false})));
  const moon=new THREE.Mesh(new THREE.SphereGeometry(38,16,12),new THREE.MeshBasicMaterial({color:0xfff2c8,fog:false}));
  moon.position.set(-450,380,-700);scene.add(moon);
})();

/* ================= WORLD ================= */
const ROAD=40,HALF=200;
const buildings=[],statics=[],trees=[];
const ground=new THREE.Mesh(new THREE.PlaneGeometry(1400,1400),mat(0x141a22));
ground.rotation.x=-Math.PI/2;ground.position.y=-0.05;scene.add(ground);

function roadTex(horizontal){
  const c=document.createElement('canvas');c.width=c.height=64;const g=c.getContext('2d');
  g.fillStyle='#26232e';g.fillRect(0,0,64,64);
  g.fillStyle='#e8d98a';
  if(horizontal)g.fillRect(8,30,32,4);else g.fillRect(30,8,4,32);
  const t=new THREE.CanvasTexture(c);t.wrapS=t.wrapT=THREE.RepeatWrapping;
  if(horizontal)t.repeat.set(2*HALF*1.1/8,1);else t.repeat.set(1,2*HALF*1.1/8);
  return t;
}
const roadHM=new THREE.MeshLambertMaterial({map:roadTex(true)}),roadVM=new THREE.MeshLambertMaterial({map:roadTex(false)}),roadPlain=mat(0x26232e);
for(let r=-5;r<=5;r++){
  const h=new THREE.Mesh(new THREE.PlaneGeometry(2*HALF*1.1,12),roadHM);h.rotation.x=-Math.PI/2;h.position.set(0,0.02,r*ROAD);scene.add(h);
  const v=new THREE.Mesh(new THREE.PlaneGeometry(12,2*HALF*1.1),roadVM);v.rotation.x=-Math.PI/2;v.position.set(r*ROAD,0.03,0);scene.add(v);
}
const interGeo=new THREE.PlaneGeometry(12,12);
for(let i=-5;i<=5;i++)for(let j=-5;j<=5;j++){const m=new THREE.Mesh(interGeo,roadPlain);m.rotation.x=-Math.PI/2;m.position.set(i*ROAD,0.04,j*ROAD);scene.add(m);}

// building window materials
function makeBuildingMat(base){
  const mk=()=>{const c=document.createElement('canvas');c.width=c.height=64;return c;};
  const c1=mk(),c2=mk(),g1=c1.getContext('2d'),g2=c2.getContext('2d');
  g1.fillStyle=base;g1.fillRect(0,0,64,64);g2.fillStyle='#000';g2.fillRect(0,0,64,64);
  for(let i=0;i<2;i++)for(let j=0;j<2;j++){
    const lit=Math.random()<0.62;
    g1.fillStyle=lit?'#ffd36b':'#2a2440';g1.fillRect(6+i*30,8+j*28,20,18);
    if(lit){g2.fillStyle=Math.random()<0.8?'#ffc766':'#ff9a5a';g2.fillRect(6+i*30,8+j*28,20,18);}
  }
  const t1=new THREE.CanvasTexture(c1),t2=new THREE.CanvasTexture(c2);
  t1.wrapS=t1.wrapT=t2.wrapS=t2.wrapT=THREE.RepeatWrapping;
  return new THREE.MeshLambertMaterial({map:t1,emissive:0xffffff,emissiveMap:t2,emissiveIntensity:0.85,flatShading:true});
}
const BM=['#8a5a44','#6d6a8f','#a0644a','#5f7a8a','#9a7b4f','#7c5a86','#b0604e','#566b6e'].map(makeBuildingMat);
const strA=[],strB=[];let strCount=0;
function addBuilding(x,z,w,d,h,material){
  const geo=new THREE.BoxGeometry(w,h,d);
  const uv=geo.attributes.uv;
  for(let i=0;i<uv.count;i++)uv.setXY(i,uv.getX(i)*Math.max(1,w/9),uv.getY(i)*Math.max(1,h/7));
  const m=new THREE.Mesh(geo,material||BM[(Math.random()*BM.length)|0]);
  m.position.set(x,h/2+0.3,z);scene.add(m);
  const bd={x:x,z:z,hw:w/2,hd:d/2,h:h+0.3,mesh:m,roof:null,ls:[]};buildings.push(bd); // mesh/roof/ls let the admin remove a building completely
  if(Math.random()<0.5)bd.roof=box(w*0.3,1.4,d*0.3,roofM,x+rnd(-w*0.2,w*0.2),h+1.0,z+rnd(-d*0.2,d*0.2),scene);
  const ly=h+0.65,push=(px,pz)=>{const A=(strCount++%2)?strA:strB;A.push(px,ly,pz);bd.ls.push([A===strA?0:1,A.length/3-1]);};
  for(let lx=-w/2+0.8;lx<=w/2-0.4;lx+=1.8){push(x+lx,z-d/2-0.1);push(x+lx,z+d/2+0.1);}
  for(let lz=-d/2+0.8;lz<=d/2-0.4;lz+=1.8){push(x-w/2-0.1,z+lz);push(x+w/2+0.1,z+lz);}
}
const slabGeo=new THREE.BoxGeometry(28,0.3,28),slabMat=mat(0x2d2a3b),parkMat=mat(0x2f6b3a);
const trunkM=mat(0x5a3a22),leafM=mat(0x2f8a45),leafM2=mat(0x3aa356);
const lampPos=[],diyaPos=[];
const shopPts=[]; // every Diwali Shop entrance (press E near any of them)
const keeperSpots=[]; // where each shopkeeper stands (the people themselves are built after the human model exists)
function rangoli(x,z,rad){
  const c=document.createElement('canvas');c.width=c.height=256;const g=c.getContext('2d');
  const cols=['#ff3b6b','#ffd23b','#2fd0ff','#7dff6b','#ff8a1f','#c35bff'];
  g.translate(128,128);
  for(let ring=5;ring>=0;ring--){g.fillStyle=cols[ring%cols.length];g.beginPath();g.arc(0,0,22+ring*18,0,TAU);g.fill();}
  for(let k=0;k<12;k++){g.save();g.rotate(k/12*TAU);g.fillStyle=cols[(k+2)%cols.length];g.beginPath();g.ellipse(0,-70,10,26,0,0,TAU);g.fill();g.restore();}
  g.fillStyle='#fff';g.beginPath();g.arc(0,0,12,0,TAU);g.fill();
  const t=new THREE.CanvasTexture(c);
  const m=new THREE.Mesh(new THREE.CircleGeometry(rad,32),new THREE.MeshBasicMaterial({map:t,transparent:true,opacity:0.92}));
  m.rotation.x=-Math.PI/2;m.position.set(x,0.06,z);scene.add(m);
}
function addShop(cx,cz){
  addBuilding(cx,cz,20,14,9,mat(0xd04a8c));
  const sc=document.createElement('canvas');sc.width=512;sc.height=128;const g=sc.getContext('2d');
  g.fillStyle='#b3195a';g.fillRect(0,0,512,128);g.strokeStyle='#ffd36b';g.lineWidth=8;g.strokeRect(8,8,496,112);
  g.fillStyle='#ffe9a8';g.font='bold 62px Arial';g.textAlign='center';g.textBaseline='middle';g.fillText('DIWALI SHOP',256,68);
  const sign=new THREE.Mesh(new THREE.PlaneGeometry(14,3.5),new THREE.MeshBasicMaterial({map:new THREE.CanvasTexture(sc)}));
  sign.position.set(cx,7.2,cz-7.2);sign.rotation.y=Math.PI;scene.add(sign);
  box(20.4,0.4,3,mat(0xffb020),cx,4.4,cz-8.6,scene);
  box(3,3.4,0.25,darkM,cx,2.0,cz-7.1,scene);
  rangoli(cx,cz-12,3.4);
  shopPts.push({x:cx,z:cz-10});
  keeperSpots.push({x:cx+3.6,z:cz-8.8,face:Math.PI});
}
// Second shop right beside the spawn point (spawn is at 0,-6): a smaller pink shop facing the road
function addSpawnShop(cx,cz){
  const sx=cx-7.5,sz=cz+7,w=12,d=8,front=sz+d/2;
  addBuilding(sx,sz,w,d,7,mat(0xd04a8c));
  const sc=document.createElement('canvas');sc.width=512;sc.height=128;const g=sc.getContext('2d');
  g.fillStyle='#b3195a';g.fillRect(0,0,512,128);g.strokeStyle='#ffd36b';g.lineWidth=8;g.strokeRect(8,8,496,112);
  g.fillStyle='#ffe9a8';g.font='bold 62px Arial';g.textAlign='center';g.textBaseline='middle';g.fillText('DIWALI SHOP',256,68);
  const sign=new THREE.Mesh(new THREE.PlaneGeometry(9,2.25),new THREE.MeshBasicMaterial({map:new THREE.CanvasTexture(sc)}));
  sign.position.set(sx,5.3,front+0.12);scene.add(sign);
  box(w+0.4,0.4,2.4,mat(0xffb020),sx,3.6,front+1.1,scene);
  box(2.4,3,0.25,darkM,sx,1.6,front+0.06,scene);
  rangoli(sx,front+1.4,1.8);
  shopPts.push({x:sx,z:front+1.4});
  keeperSpots.push({x:sx-3.6,z:front+0.9,face:0}); // on the spawn side of the door, so he is the first thing you see
  addBuilding(cx+2,cz-8,20,8,rnd(14,28)); // fill the back of the block so the street still looks built-up
}
for(let bi=-5;bi<5;bi++)for(let bj=-5;bj<5;bj++){
  const cx=bi*ROAD+20,cz=bj*ROAD+20,isShop=(bi===0&&bj===0),isShop2=(bi===0&&bj===-1),isPark=!isShop&&!isShop2&&Math.random()<0.12;
  const slab=new THREE.Mesh(slabGeo,isPark?parkMat:slabMat);slab.position.set(cx,0.15,cz);scene.add(slab);
  if(isShop)addShop(cx,cz);
  else if(isShop2)addSpawnShop(cx,cz);
  else if(isPark){
    for(let t=0;t<5;t++){
      const tx=cx+rnd(-10,10),tz=cz+rnd(-10,10),s=rnd(0.9,1.4),tg=new THREE.Group();scene.add(tg);
      const trunk=new THREE.Mesh(new THREE.CylinderGeometry(0.35,0.45,2.4*s,6),trunkM);trunk.position.set(tx,1.5*s,tz);tg.add(trunk);
      const l1=new THREE.Mesh(new THREE.ConeGeometry(2.0*s,3.4*s,7),leafM);l1.position.set(tx,3.6*s,tz);tg.add(l1);
      const l2=new THREE.Mesh(new THREE.ConeGeometry(1.5*s,2.8*s,7),leafM2);l2.position.set(tx,5.2*s,tz);tg.add(l2);
      const tst={x:tx,z:tz,r:1.1,h:8};statics.push(tst);trees.push({g:tg,st:tst});
    }
  }else if(Math.random()<0.5){
    addBuilding(cx,cz,rnd(16,22),rnd(16,22),rnd(14,42));
  }else{
    for(const sx of[-1,1])for(const sz of[-1,1])addBuilding(cx+sx*6.2,cz+sz*6.2,rnd(8,10.4),rnd(8,10.4),rnd(8,30));
  }
  lampPos.push([cx-12.6,cz-12.6],[cx+12.6,cz-12.6],[cx-12.6,cz+12.6],[cx+12.6,cz+12.6]);
  for(const q of lampPos.slice(-4))statics.push({x:q[0],z:q[1],r:0.55,h:8});
  for(let k=-10.5;k<=10.5;k+=3){diyaPos.push([cx+k,cz-13.3],[cx+k,cz+13.3],[cx-13.3,cz+k],[cx+13.3,cz+k]);}
}
rangoli(0,0,4.4);

// instanced decor
(function(){
  const dummy=new THREE.Object3D(),col=new THREE.Color();
  const palette=[0xff4466,0xffd23b,0x4de0ff,0x8dff6b,0xff8a1f,0xd264ff];
  function make(arr,geo,matl,withColor){
    const n=arr.length/3;if(!n)return null;
    const im=new THREE.InstancedMesh(geo,matl,n);
    for(let i=0;i<n;i++){dummy.position.set(arr[i*3],arr[i*3+1],arr[i*3+2]);dummy.updateMatrix();im.setMatrixAt(i,dummy.matrix);if(withColor){col.setHex(palette[(Math.random()*palette.length)|0]);im.setColorAt(i,col);}}
    im.frustumCulled=false;scene.add(im);return im;
  }
  const sg=new THREE.SphereGeometry(0.22,5,4);
  window.__lightsA=make(strA,sg,new THREE.MeshBasicMaterial({color:0xffffff}),true);
  window.__lightsB=make(strB,sg,new THREE.MeshBasicMaterial({color:0xffffff}),true);
  // lamp posts + heads
  const poles=[],heads=[];
  for(const q of lampPos){poles.push(q[0],4,q[1]);heads.push(q[0],8.2,q[1]);}
  make(poles,new THREE.CylinderGeometry(0.18,0.25,8,6),mat(0x3a3548),false);
  make(heads,new THREE.SphereGeometry(0.55,7,6),new THREE.MeshBasicMaterial({color:0xffe3a0}),false);
  const dy=[];for(const q of diyaPos)dy.push(q[0],0.45,q[1]);
  const diya=make(dy,new THREE.CylinderGeometry(0.34,0.24,0.25,6),new THREE.MeshBasicMaterial({color:0xffffff}),false);
  if(diya){for(let i=0;i<diyaPos.length;i++){col.setHex(Math.random()<0.5?0xffa21f:0xff7a1a);diya.setColorAt(i,col);}}
})();

/* ================= VEHICLES ================= */
const DEFS={
  car:{name:'Car',max:38,acc:22,turn:1.9,r:1.6,fz:2.1,w:2.0,sy:1.2},
  truck:{name:'Truck',max:28,acc:14,turn:1.3,r:2.2,fz:3.2,w:2.4,sy:2.4},
  scooter:{name:'Scooter',max:26,acc:20,turn:2.6,r:0.9,fz:0.9,w:0.6,sy:1.0},
  bike:{name:'Bike',max:44,acc:26,turn:2.2,r:1.0,fz:1.0,w:0.5,sy:1.1},
  plane:{name:'Aeroplane',max:70,acc:22,turn:1.0,r:3.2,fz:3.4,w:1.2,sy:1.0}
};
const ORDER=['car','truck','scooter','bike','plane'];
function rider(g,color,y,z){box(0.5,0.7,0.35,mat(color),0,y,z,g);const h=new THREE.Mesh(new THREE.SphereGeometry(0.28,8,6),skinM);h.position.set(0,y+0.62,z);g.add(h);}
function buildVehicle(type,color){
  const g=new THREE.Group();g.rotation.order='YXZ';
  const bm=mat(color),bodyMats=[bm],wheels=[];let prop=null;
  if(type==='car'){
    box(2,0.7,4.2,bm,0,0.8,0,g);box(1.7,0.65,2.3,glassM,0,1.45,-0.2,g);
    box(2.05,0.22,0.4,darkM,0,0.5,2.05,g);box(2.05,0.22,0.4,darkM,0,0.5,-2.05,g);
    box(0.5,0.25,0.1,lightM,0.7,0.95,2.13,g);box(0.5,0.25,0.1,lightM,-0.7,0.95,2.13,g);
    box(0.5,0.22,0.1,tailM,0.7,0.95,-2.13,g);box(0.5,0.22,0.1,tailM,-0.7,0.95,-2.13,g);
    for(const sx of[-1,1])for(const sz of[-1,1])wheels.push(wheel(0.42,0.32,sx*1.05,0.42,sz*1.4,g));
  }else if(type==='truck'){
    box(2.4,1.9,1.9,bm,0,1.5,2.2,g);box(2.0,0.8,0.1,glassM,0,1.9,3.17,g);
    const cm=mat(0xe8e0d0);box(2.7,2.6,4.6,cm,0,1.9,-0.6,g);
    box(0.5,0.25,0.1,lightM,0.8,0.9,3.17,g);box(0.5,0.25,0.1,lightM,-0.8,0.9,3.17,g);
    box(2.5,0.3,8,darkM,0,0.5,0.4,g);
    for(const sx of[-1,1])for(const sz of[2.4,-0.6,-2.2])wheels.push(wheel(0.55,0.4,sx*1.25,0.55,sz,g));
  }else if(type==='scooter'){
    box(0.65,0.25,1.7,bm,0,0.45,0,g);box(0.6,0.7,0.25,bm,0,0.9,0.7,g);box(0.9,0.1,0.1,darkM,0,1.3,0.7,g);
    box(0.5,0.18,0.7,darkM,0,0.75,-0.4,g);rider(g,0x2e7d5b,1.1,-0.35);
    box(0.3,0.1,0.1,lightM,0,1.1,0.85,g);
    for(const sz of[-0.75,0.8])wheels.push(wheel(0.3,0.2,0,0.3,sz,g));
  }else if(type==='bike'){
    box(0.5,0.55,1.9,bm,0,0.85,0,g);box(0.45,0.3,0.8,bm,0,1.2,0.25,g);box(1.0,0.1,0.1,darkM,0,1.45,0.65,g);
    box(0.3,0.15,0.9,darkM,0.35,0.55,-0.5,g);box(0.4,0.2,0.1,lightM,0,1.1,1.0,g);
    rider(g,0xc0392b,1.35,-0.35);
    for(const sz of[-0.95,1.0])wheels.push(wheel(0.45,0.22,0,0.45,sz,g));
  }else{ // plane
    const fus=new THREE.Mesh(new THREE.CylinderGeometry(0.7,0.4,6,8),bm);fus.rotation.x=Math.PI/2;g.add(fus);
    const nose=new THREE.Mesh(new THREE.ConeGeometry(0.7,1.2,8),mat(0xdddddd));nose.rotation.x=Math.PI/2;nose.position.z=3.6;g.add(nose);
    box(10,0.15,1.8,mat(0xf2f2f2),0,0.1,0.4,g);box(3.4,0.12,0.9,mat(0xf2f2f2),0,0.2,-2.7,g);box(0.15,1.5,1.1,bm,0,0.9,-2.7,g);
    box(1.0,0.5,1.4,glassM,0,0.65,1.0,g);
    prop=box(2.8,0.12,0.12,darkM,0,0,4.25,g);
    for(const sx of[-1,1])wheels.push(wheel(0.28,0.18,sx*0.8,-0.8,0.9,g));
    box(0.12,0.18,0.12,tailM,5,0.15,0.4,g);box(0.12,0.18,0.12,tailM,-5,0.15,0.4,g);
  }
  return{group:g,bodyMats:bodyMats,wheels:wheels,prop:prop,baseHex:color,p1:null,p2:null,debris:null};
}
const SOOT=new THREE.Color(0x2e2a28);
function tintVehicle(v,f){for(const m of v.bodyMats){m.color.setHex(v.baseHex);if(f>0)m.color.lerp(SOOT,f*0.65);}}
function paintGold(v){v.baseHex=0xe0b030;tintVehicle(v,0);}

/* player */
const P={type:'car',x:0,y:0,z:0,h:0,speed:0,v:null,hitT:0,steer:0,hp:100,pull:1};
const HPS={car:100,truck:100,scooter:100,bike:100,plane:100}; // condition is remembered per vehicle type
function clearVehicle(v){
  scene.remove(v.group);
  if(v.debris){for(const d of v.debris)scene.remove(d);v.debris=null;}
}
// Shows damage on the current vehicle: soot tint, hanging bumper, popped bonnet, scattered parts
function refreshDamage(){
  const v=P.v,def=DEFS[P.type],g=v.group,f=1-P.hp/100;
  tintVehicle(v,f);
  if(P.hp>=75&&v.p1){g.remove(v.p1);v.p1=null;}
  if(P.hp>=45&&v.p2){g.remove(v.p2);v.p2=null;}
  if(P.hp>0&&v.debris){for(const d of v.debris)scene.remove(d);v.debris=null;}
  if(P.hp<75&&!v.p1){v.p1=box(def.w*0.95,0.22,0.3,darkM,0,0.5,def.fz+0.05,g);v.p1.rotation.z=0.35;}
  if(P.hp<45&&!v.p2&&P.type!=='plane'){v.p2=box(def.w*0.8,0.1,def.fz*0.5,mat(0x2a2a30),0,def.sy+0.15,def.fz*0.55,g);v.p2.rotation.x=-0.45;}
  if(P.hp<=0&&!v.debris){
    v.debris=[];
    for(let k=0;k<4;k++){
      const d=new THREE.Mesh(new THREE.BoxGeometry(rnd(0.3,0.8),0.2,rnd(0.3,0.7)),mat(k%2?v.baseHex:0x2a2a30));
      const a=P.h+rnd(-0.9,0.9),dist=def.fz+rnd(0.8,2.4);
      d.position.set(P.x+Math.sin(a)*dist,0.15,P.z+Math.cos(a)*dist);d.rotation.set(0,rnd(0,TAU),0);scene.add(d);v.debris.push(d);
    }
  }
}
function setVehicle(type){
  if(P.v)clearVehicle(P.v);
  dismissCrowd();
  P.type=type;P.v=buildVehicle(type,[0xd63a3a,0x2f6fd6,0x1fa37a,0xd68a1f,0xb34ad6][ORDER.indexOf(type)]);
  if(save.up.gold)paintGold(P.v);
  scene.add(P.v.group);P.speed=0;
  P.hp=HPS[type];P.pull=Math.random()<0.5?-1:1;
  refreshDamage();
  P.y=GY()+((type==='plane')?0.95:0);
  $('veh').textContent=DEFS[type].name;
  netSendNow();
}

/* ================= AI TRAFFIC ================= */
const ais=[];
(function(){
  const lanes=[];
  for(let r=-5;r<=5;r++)for(const horiz of[true,false])for(const dir of[-1,1])lanes.push({r:r,horiz:horiz,dir:dir});
  for(let i=lanes.length-1;i>0;i--){const j=(Math.random()*(i+1))|0;const t=lanes[i];lanes[i]=lanes[j];lanes[j]=t;}
  const types=['car','car','truck','scooter','bike'],colors=[0xe8c23a,0x3ac2e8,0xe85a8a,0x9a6be8,0x6be8a1,0xe8863a,0xf0f0f0];
  for(let i=0;i<26;i++){
    const L=lanes[i],type=types[(Math.random()*types.length)|0];
    const v=buildVehicle(type,colors[(Math.random()*colors.length)|0]);scene.add(v.group);
    const a={type:type,v:v,horiz:L.horiz,dir:L.dir,road:L.r,lane:L.r*ROAD+L.dir*3,speed:rnd(8,15),cur:0,along:rnd(-190,190),cross:L.r*ROAD+L.dir*3,kx:0,kz:0,pause:0,spin:0,r:DEFS[type].r,x:0,z:0,h:0};
    a.h=L.horiz?(L.dir>0?Math.PI/2:-Math.PI/2):(L.dir>0?0:Math.PI);
    ais.push(a);
  }
})();
function updateAI(dt){
  for(const a of ais){
    if(a.pause>0){a.pause-=dt;a.cur=Math.max(0,a.cur-30*dt);}else a.cur+=(a.speed-a.cur)*Math.min(1,dt*1.5);
    if(WK.on){ // traffic queues politely behind a vehicle you left parked in its lane
      const pr=DEFS[P.type].r,da=a.horiz?(P.x-a.x)*a.dir:(P.z-a.z)*a.dir,lat=Math.abs(a.horiz?P.z-a.z:P.x-a.x);
      if(da>0&&da<pr+a.r+9&&lat<pr+a.r*0.5+1.2)a.cur=Math.min(a.cur,Math.max(0,da-(pr+a.r))*3);
    }
    a.along+=a.dir*a.cur*dt;
    if(a.along>205)a.along=-205;else if(a.along<-205)a.along=205;
    a.cross+=(a.lane-a.cross)*Math.min(1,dt*1.5);
    let ax=a.horiz?a.along:a.cross,az=a.horiz?a.cross:a.along;
    ax+=a.kx*dt;az+=a.kz*dt;
    if(a.horiz){a.along+=a.kx*dt;a.cross+=a.kz*dt;}else{a.cross+=a.kx*dt;a.along+=a.kz*dt;}
    const kd=Math.exp(-3*dt);a.kx*=kd;a.kz*=kd;a.spin*=kd;
    a.x=a.horiz?a.along:a.cross;a.z=a.horiz?a.cross:a.along;
    a.v.group.position.set(a.x,0,a.z);a.v.group.rotation.y=a.h+a.spin;
    for(const w of a.v.wheels)w.rotation.x+=a.cur*dt*2;
  }
}

/* ================= OBSTACLES ================= */
const obstacles=[];
(function(){
  const coneG=new THREE.ConeGeometry(0.45,1.0,8),barrelG=new THREE.CylinderGeometry(0.55,0.55,1.2,10),crateG=new THREE.BoxGeometry(1.2,1.2,1.2);
  const coneM=mat(0xff7a1a),b1=mat(0xc0392b),b2=mat(0x2f6fd6),crM=mat(0x9a6a3a);
  for(let i=0;i<70;i++){
    const kind=['cone','barrel','crate'][(Math.random()*3)|0];
    const horiz=Math.random()<0.5,road=((Math.random()*11)|0)-5,along=rnd(-185,185),off=(Math.random()<0.5?-1:1)*rnd(1,5.2);
    const x=horiz?along:road*ROAD+off,z=horiz?road*ROAD+off:along;
    if(Math.abs(x)<10&&Math.abs(z)<10)continue;
    let me,r;
    if(kind==='cone'){me=new THREE.Mesh(coneG,coneM);r=0.6;me.position.y=0.5;}
    else if(kind==='barrel'){me=new THREE.Mesh(barrelG,Math.random()<0.5?b1:b2);r=0.7;me.position.y=0.6;}
    else{me=new THREE.Mesh(crateG,crM);r=0.9;me.position.y=0.6;}
    me.position.x=x;me.position.z=z;scene.add(me);
    obstacles.push({mesh:me,x:x,z:z,vx:0,vz:0,r:r,spin:0,baseY:me.position.y});
  }
})();
function updateObstacles(dt){
  for(const o of obstacles){
    if(Math.abs(o.vx)+Math.abs(o.vz)<0.05&&o.spin===0)continue;
    o.x+=o.vx*dt;o.z+=o.vz*dt;const k=Math.exp(-2.4*dt);o.vx*=k;o.vz*=k;
    o.mesh.rotation.y+=o.spin*dt;o.spin*=k;if(Math.abs(o.spin)<0.05)o.spin=0;
    const sp=Math.hypot(o.vx,o.vz);o.mesh.position.set(o.x,o.baseY+Math.min(sp*0.06,0.8),o.z);
    if(sp<0.05){o.vx=o.vz=0;o.mesh.position.y=o.baseY;}
  }
}

/* ================= HUMANS (faceted low-poly man: t-shirt, shorts, bare arms and legs) ================= */
const HG={},HM={};
const SKINS=[0xc9976a,0xb87d4f,0x8d5a3a,0xe0b48a,0x6e4429],HAIRS=[0x1f1a17,0x2b1d14,0x3a2a1c,0x111111],SHORTS=[0x2b2b30,0x3a3550,0x4a3b2c,0x23323f];
function hmat(c){return HM[c]||(HM[c]=new THREE.MeshLambertMaterial({color:c,flatShading:true}));}
function hcyl(a,b,h,s){const k='c'+a+'_'+b+'_'+h+'_'+s;return HG[k]||(HG[k]=new THREE.CylinderGeometry(a,b,h,s||6));}
function hsph(r,ws,hs,tl){const k='s'+r+'_'+ws+'_'+hs+'_'+(tl||0);return HG[k]||(HG[k]=tl?new THREE.SphereGeometry(r,ws,hs,0,TAU,0,tl):new THREE.SphereGeometry(r,ws,hs));}
function hbox(w,h,d){const k='b'+w+'_'+h+'_'+d;return HG[k]||(HG[k]=new THREE.BoxGeometry(w,h,d));}
function hpart(geo,m,x,y,z,parent,sx,sy,sz){const me=new THREE.Mesh(geo,m);me.position.set(x,y,z);if(sx)me.scale.set(sx,sy,sz);parent.add(me);return me;}
function buildHuman(shirtC,shortsC,skinC,hairC){
  const skin=hmat(skinC),sh=hmat(shirtC),st=hmat(shortsC),hair=hmat(hairC),shoe=hmat(0x1c1c22),sock=hmat(0xf2f2f2),dark=hmat(0x16120f);
  const g=new THREE.Group(),body=new THREE.Group();g.add(body);
  hpart(hcyl(0.3,0.22,0.72,6),sh,0,1.32,0,body,1,1,0.62);          // torso
  hpart(hcyl(0.23,0.25,0.3,6),st,0,0.95,0,body,1,1,0.7);           // shorts / hips
  hpart(hcyl(0.07,0.08,0.14,6),skin,0,1.73,0,body);                 // neck
  hpart(hsph(0.19,8,6),skin,0,1.93,0,body,1,1.15,1.02);             // head
  hpart(hsph(0.205,8,5,Math.PI*0.5),hair,0,1.95,-0.012,body,1,1.1,1.06); // hair cap
  hpart(hsph(0.026,5,4),dark,-0.07,1.95,0.178,body);hpart(hsph(0.026,5,4),dark,0.07,1.95,0.178,body); // eyes
  hpart(hbox(0.04,0.06,0.05),skin,0,1.9,0.19,body);                 // nose
  hpart(hsph(0.04,5,4),skin,-0.19,1.92,0,body);hpart(hsph(0.04,5,4),skin,0.19,1.92,0,body); // ears
  function leg(x){
    const l=new THREE.Group();l.position.set(x,0.95,0);body.add(l);
    hpart(hcyl(0.115,0.105,0.28,6),st,0,-0.14,0,l);
    hpart(hcyl(0.095,0.08,0.2,6),skin,0,-0.38,0,l);
    hpart(hcyl(0.08,0.06,0.42,6),skin,0,-0.69,0,l);
    hpart(hcyl(0.065,0.065,0.05,6),sock,0,-0.9,0,l);
    hpart(hbox(0.14,0.1,0.3),shoe,0,-0.95,0.06,l);
    return l;
  }
  function arm(x){
    const a=new THREE.Group();a.position.set(x,1.6,0);body.add(a);
    hpart(hcyl(0.085,0.075,0.26,6),sh,0,-0.13,0,a);
    const f=new THREE.Group();f.position.y=-0.26;a.add(f);
    hpart(hcyl(0.07,0.055,0.3,6),skin,0,-0.15,0,f);
    hpart(hbox(0.09,0.12,0.075),skin,0,-0.35,0,f);
    return{a:a,f:f};
  }
  const A1=arm(-0.37),A2=arm(0.37);
  g.scale.setScalar(0.92);
  return{g:g,body:body,l1:leg(-0.12),l2:leg(0.12),a1:A1.a,a2:A2.a,f1:A1.f,f2:A2.f};
}
// swing = walk-cycle amount, up = 0..1 blend into the arms-raised cheer pose
function poseHuman(h,swing,up){
  h.l1.rotation.x=swing;h.l2.rotation.x=-swing;
  h.a1.rotation.x=-swing*(1-up)*0.8;h.a2.rotation.x=swing*(1-up)*0.8;
  h.a1.rotation.z=-up*2.3;h.a2.rotation.z=up*2.3;
  h.f1.rotation.z=-up*1.1;h.f2.rotation.z=up*1.1;
  h.f1.rotation.x=h.f2.rotation.x=-0.3*(1-up);
}

/* ================= PEOPLE ================= */
const people=[];
(function(){
  const cols=[0xe8567a,0x4a90e2,0xf5a623,0x7ed321,0xbd10e0,0xf8e71c,0xff6b6b,0x50e3c2];
  for(let i=0;i<130;i++){
    const h=buildHuman(cols[(Math.random()*cols.length)|0],SHORTS[(Math.random()*SHORTS.length)|0],SKINS[(Math.random()*SKINS.length)|0],HAIRS[(Math.random()*HAIRS.length)|0]);
    scene.add(h.g);
    const horiz=Math.random()<0.5,road=((Math.random()*11)|0)-5,side=Math.random()<0.5?-1:1;
    people.push({g:h.g,h:h,up:0,horiz:horiz,base:road*ROAD+side*9.5,off:0,along:rnd(-190,190),dir:Math.random()<0.5?-1:1,sp:rnd(1.3,2.4),ph:rnd(0,6),cheer:0,x:0,z:0});
  }
})();
function updatePeople(dt){
  const near=P.y<4;
  for(const p of people){
    p.along+=p.dir*p.sp*dt;if(p.along>195){p.dir=-1;}else if(p.along<-195){p.dir=1;}
    p.ph+=dt*p.sp*4;
    const px=p.horiz?p.along:p.base+p.off,pz=p.horiz?p.base+p.off:p.along;
    let tgt=0;
    if(near){const dx=px-P.x,dz=pz-P.z;if(dx*dx+dz*dz<64){const crossD=p.horiz?dz:dx;tgt=(crossD>=0?1:-1)*3;}}
    p.off+=(tgt-p.off)*Math.min(1,dt*3);
    p.x=p.horiz?p.along:p.base+p.off;p.z=p.horiz?p.base+p.off:p.along;
    let y=0.3;
    if(p.cheer>0){p.cheer-=dt;if(p.cheer<0.6)y+=Math.abs(Math.sin(p.cheer*14))*0.45;}
    p.g.position.set(p.x,y,p.z);
    p.g.rotation.y=p.horiz?(p.dir>0?Math.PI/2:-Math.PI/2):(p.dir>0?0:Math.PI);
    p.up+=((p.cheer>0?1:0)-p.up)*Math.min(1,dt*10);poseHuman(p.h,Math.sin(p.ph)*0.6,p.up);
  }
}

/* ================= SHOPKEEPERS (one outside each shop) ================= */
const keepers=[];
(function(){
  keeperSpots.forEach(function(s){
    const h=buildHuman(0xff9d2e,0xf2ead8,0x9a6a45,0xbfbfbf);                       // orange kurta, grey hair
    hpart(hsph(0.215,8,5,Math.PI*0.5),hmat(0xd8362e),0,1.99,0,h.body,1,0.85,1.08);  // red turban
    hpart(hbox(0.13,0.025,0.03),hmat(0x16120f),0,1.865,0.192,h.body);               // moustache
    h.g.position.set(s.x,0.3,s.z);h.g.rotation.y=s.face;scene.add(h.g);
    keepers.push({h:h,x:s.x,z:s.z,face:s.face,up:0,ph:Math.random()*6});
  });
})();
function updateKeepers(dt){
  const A=actor();
  for(let i=0;i<keepers.length;i++){
    const k=keepers[i],dx=A.x-k.x,dz=A.z-k.z,d=Math.hypot(dx,dz);
    k.h.g.rotation.y+=angDiff(k.h.g.rotation.y,d<30?Math.atan2(dx,dz):k.face)*Math.min(1,dt*5); // turns to watch you
    k.up+=((d<16&&!shopOpen?1:0)-k.up)*Math.min(1,dt*6);k.ph+=dt*8;                              // waves when you are close
    poseHuman(k.h,0,0);
    k.h.a2.rotation.z=k.up*2.3;k.h.f2.rotation.z=k.up*1.1;k.h.a2.rotation.x=Math.sin(k.ph)*0.3*k.up;
  }
}

/* ================= COINS ================= */
const coins=[];
(function(){
  const cg=new THREE.CylinderGeometry(0.7,0.7,0.16,14),cm=new THREE.MeshLambertMaterial({color:0xffc800,emissive:0x664400,flatShading:true});
  for(let i=0;i<100;i++){
    const horiz=Math.random()<0.5,road=((Math.random()*11)|0)-5,along=rnd(-190,190),off=(Math.random()<0.5?-1:1)*3;
    const x=horiz?along:road*ROAD+off,z=horiz?road*ROAD+off:along;
    const grp=new THREE.Group(),m=new THREE.Mesh(cg,cm);m.rotation.x=Math.PI/2;grp.add(m);grp.position.set(x,1.5,z);scene.add(grp);
    coins.push({g:grp,x:x,z:z,alive:true,respawn:0});
  }
})();
function updateCoins(dt){
  const A=actor(),mr=2.3+save.up.magnet*2.2,ground=A.y<5,reach=WK.on?1:DEFS[P.type].r*2;
  for(const c of coins){
    if(!c.alive){c.respawn-=dt;if(c.respawn<=0){c.alive=true;c.g.visible=true;}continue;}
    c.g.rotation.y+=dt*3;c.g.position.y=1.5+Math.sin(T*3+c.x)*0.25;
    if(ground){
      const dx=c.x-A.x,dz=c.z-A.z;
      if(dx*dx+dz*dz<mr*mr+reach){c.alive=false;c.g.visible=false;c.respawn=25;save.coins+=5;persist();sfxCoin();updateHUDcoins();}
    }
  }
}

/* ================= AUDIO ================= */
let AC=null,master=null,noiseBuf=null,muted=false,engOsc=null,engGain=null,engFilt=null;
function initAudio(){
  if(AC){if(AC.state==='suspended')AC.resume();return;}
  try{
    const C=window.AudioContext||window.webkitAudioContext;AC=new C();
    master=AC.createGain();master.gain.value=muted?0:0.85;
    const comp=AC.createDynamicsCompressor();master.connect(comp);comp.connect(AC.destination);
    const len=AC.sampleRate*2,buf=AC.createBuffer(1,len,AC.sampleRate),d=buf.getChannelData(0);
    for(let i=0;i<len;i++)d[i]=Math.random()*2-1;noiseBuf=buf;
    engOsc=AC.createOscillator();engOsc.type='sawtooth';engOsc.frequency.value=40;
    engFilt=AC.createBiquadFilter();engFilt.type='lowpass';engFilt.frequency.value=500;
    engGain=AC.createGain();engGain.gain.value=0;
    engOsc.connect(engFilt);engFilt.connect(engGain);engGain.connect(master);engOsc.start();
  }catch(e){AC=null;}
}
function noiseSrc(t,dur){const s=AC.createBufferSource();s.buffer=noiseBuf;s.loop=true;s.start(t,Math.random()*1.5);s.stop(t+dur+0.05);return s;}
function envGain(t,peak,att,dec,dest){const g=AC.createGain();g.gain.setValueAtTime(0.0001,t);g.gain.linearRampToValueAtTime(Math.max(peak,0.0002),t+att);g.gain.exponentialRampToValueAtTime(0.0001,t+att+dec);g.connect(dest);return g;}
function panNode(p){if(AC.createStereoPanner){const n=AC.createStereoPanner();n.pan.value=clamp(p,-1,1);n.connect(master);return n;}return master;}
function filt(type,f,q){const b=AC.createBiquadFilter();b.type=type;b.frequency.value=f;if(q)b.Q.value=q;return b;}
function osc(type,f0,f1,t,dur){const o=AC.createOscillator();o.type=type;o.frequency.setValueAtTime(f0,t);if(f1!==f0)o.frequency.exponentialRampToValueAtTime(f1,t+dur);return o;}
let camH=0;
function panFor(x,z){const dx=x-camera.position.x,dz=z-camera.position.z,d=Math.hypot(dx,dz)||1,fx=Math.sin(camH),fz=Math.cos(camH);return clamp((dx/d)*(-fz)+(dz/d)*fx,-1,1);}

// Pataka: sharp double crack + thump (kept for crackers, bursts and the G chain)
function pataka(t,vol,pan,big){
  const out=panNode(pan);
  for(let k=0;k<2;k++){
    const t0=t+k*0.045,v=vol*(k?0.6:1);
    const n=noiseSrc(t0,0.3),hp=filt('highpass',1100+rnd(0,1800)),g=envGain(t0,v,0.002,big?0.2:0.09,out);
    n.connect(hp);hp.connect(g);
  }
  const o=osc('sine',230,48,t,0.14),og=envGain(t,vol*0.9,0.003,big?0.22:0.14,out);o.connect(og);o.start(t);o.stop(t+0.3);
}
function sfxLaunch(x,z){
  if(!AC)return;const t=AC.currentTime,d=Math.hypot(x-P.x,z-P.z),vol=clamp(1/(1+d/45),0.12,0.6),out=panNode(panFor(x,z));
  const n=noiseSrc(t,2.6),bp=filt('bandpass',600,1.2);bp.frequency.setValueAtTime(600,t);bp.frequency.exponentialRampToValueAtTime(2800,t+2.4);
  const g=AC.createGain();g.gain.setValueAtTime(0.0001,t);g.gain.linearRampToValueAtTime(0.32*vol,t+0.2);g.gain.linearRampToValueAtTime(0.0001,t+2.6);
  n.connect(bp);bp.connect(g);g.connect(out);
  const o=osc('sine',650,1900,t,2.4),og=AC.createGain();og.gain.setValueAtTime(0.0001,t);og.gain.linearRampToValueAtTime(0.05*vol,t+0.3);og.gain.linearRampToValueAtTime(0.0001,t+2.5);
  o.connect(og);og.connect(out);o.start(t);o.stop(t+2.6);
  pataka(t,0.18*vol,panFor(x,z),false);
}
let lastBoomT=-1;
function sfxBurst(x,y,z){
  if(!AC)return;const t=AC.currentTime,d=Math.hypot(x-P.x,z-P.z,y*0.5),vol=clamp(1.3/(1+d/35),0.14,1),out=panNode(panFor(x,z));
  const delay=Math.min(d/340*0.6,0.35),t0=t+delay;
  const o=osc('sine',120,34,t0,0.6),og=envGain(t0,vol*0.9,0.004,0.55,out);o.connect(og);o.start(t0);o.stop(t0+0.7);
  const n=noiseSrc(t0,0.5),lp=filt('lowpass',1100),ng=envGain(t0,vol*0.75,0.003,0.4,out);n.connect(lp);lp.connect(ng);
  pataka(t0,vol*0.8,panFor(x,z),true);
  const crowd=(t-lastBoomT)<0.25;lastBoomT=t;
  const cnt=crowd?4:(7+((Math.random()*6)|0));
  for(let i=0;i<cnt;i++)pataka(t0+0.12+Math.random()*1.1,vol*rnd(0.12,0.3),clamp(panFor(x,z)+rnd(-0.3,0.3),-1,1),false);
}
// Crash: deep BOOM + metallic CLANG + short tyre skid + brief rumble
function playCrash(s){
  if(!AC)return;const t=AC.currentTime,out=master;
  const o=osc('sine',150,36,t,0.5),og=envGain(t,0.95*s,0.004,0.5,out);o.connect(og);o.start(t);o.stop(t+0.6);
  const o2=osc('triangle',90,30,t,0.35),o2g=envGain(t,0.5*s,0.004,0.35,out);o2.connect(o2g);o2.start(t);o2.stop(t+0.45);
  const th=noiseSrc(t,0.25),thf=filt('lowpass',420),thg=envGain(t,0.7*s,0.002,0.22,out);th.connect(thf);thf.connect(thg);
  const fr=[612,1010,1471,2290],base=rnd(0.92,1.08);
  for(let i=0;i<fr.length;i++){
    const c=osc(i%2?'square':'triangle',fr[i]*base,fr[i]*base*0.985,t+0.015,0.5),cf=filt('bandpass',fr[i]*base,6),cg=envGain(t+0.015,(0.2-i*0.03)*s,0.001,0.28+0.06*(3-i),out);
    c.connect(cf);cf.connect(cg);c.start(t+0.015);c.stop(t+0.6);
  }
  const sk=noiseSrc(t+0.05,0.4),sf=filt('bandpass',2600,3.5);sf.frequency.setValueAtTime(2600,t+0.05);sf.frequency.exponentialRampToValueAtTime(1200,t+0.4);
  const sg=AC.createGain();sg.gain.setValueAtTime(0.0001,t+0.05);sg.gain.linearRampToValueAtTime(0.16*s,t+0.1);sg.gain.linearRampToValueAtTime(0.0001,t+0.42);
  sk.connect(sf);sf.connect(sg);sg.connect(out);
  const rb=noiseSrc(t+0.05,0.7),rf=filt('lowpass',160),rg=AC.createGain();
  rg.gain.setValueAtTime(0.0001,t+0.05);rg.gain.linearRampToValueAtTime(0.5*s,t+0.18);rg.gain.linearRampToValueAtTime(0.0001,t+0.75);
  rb.connect(rf);rf.connect(rg);rg.connect(out);
  const ro=osc('sine',52,44,t+0.05,0.7),rog=AC.createGain();rog.gain.setValueAtTime(0.0001,t+0.05);rog.gain.linearRampToValueAtTime(0.25*s,t+0.2);rog.gain.linearRampToValueAtTime(0.0001,t+0.75);
  ro.connect(rog);rog.connect(out);ro.start(t+0.05);ro.stop(t+0.8);
}
function sfxCoin(){
  if(!AC)return;const t=AC.currentTime;
  [988,1319].forEach((f,i)=>{const o=osc('sine',f,f,t+i*0.07,0.2),g=envGain(t+i*0.07,0.16,0.003,0.18,master);o.connect(g);o.start(t+i*0.07);o.stop(t+i*0.07+0.25);});
}
function sfxBuy(){
  if(!AC)return;const t=AC.currentTime;
  [523,659,784].forEach((f,i)=>{const o=osc('triangle',f,f,t+i*0.08,0.25),g=envGain(t+i*0.08,0.18,0.004,0.22,master);o.connect(g);o.start(t+i*0.08);o.stop(t+i*0.08+0.3);});
}
function sfxDoor(){
  if(!AC)return;const t=AC.currentTime;
  const o=osc('sine',170,70,t,0.14),g=envGain(t,0.4,0.003,0.14,master);o.connect(g);o.start(t);o.stop(t+0.2);
  const n=noiseSrc(t,0.1),f=filt('lowpass',900),ng=envGain(t,0.25,0.002,0.07,master);n.connect(f);f.connect(ng);
}
function sfxBreak(){ // sad cartoon breakdown: hiss + clunk + sputter
  if(!AC)return;const t=AC.currentTime;
  const n=noiseSrc(t,1.4),hp=filt('highpass',3200),g=envGain(t,0.22,0.05,1.3,master);n.connect(hp);hp.connect(g);
  const o=osc('square',210,55,t,0.45),og=envGain(t,0.28,0.004,0.45,master);o.connect(og);o.start(t);o.stop(t+0.5);
  for(let i=0;i<3;i++){const s=t+0.5+i*0.18,so=osc('sawtooth',90-i*14,50,s,0.12),sg=envGain(s,0.16,0.004,0.12,master);so.connect(sg);so.start(s);so.stop(s+0.16);}
}
function updateEngine(){
  if(!AC||!engOsc)return;const t=AC.currentTime,sp=Math.abs(P.speed),def=DEFS[P.type];
  let f=P.type==='plane'?65+sp*2.0:(P.type==='bike'?55+sp*3.6:(P.type==='truck'?34+sp*3:42+sp*3.2));
  engOsc.type=P.type==='plane'?'square':'sawtooth';
  engOsc.frequency.setTargetAtTime(f,t,0.08);engFilt.frequency.setTargetAtTime(350+sp*14,t,0.1);
  engGain.gain.setTargetAtTime((muted||!started||shopOpen||WK.on||P.hp<=0)?0:0.03+Math.min(sp/def.max,1)*0.035,t,0.1);
}

/* ================= PARTICLES ================= */
const MAXP=10000;
const pPos=new Float32Array(MAXP*3),pCol=new Float32Array(MAXP*3),pSize=new Float32Array(MAXP),pVel=new Float32Array(MAXP*3),pBase=new Float32Array(MAXP*3);
const pLife=new Float32Array(MAXP),pMax=new Float32Array(MAXP),pDrag=new Float32Array(MAXP),pGrav=new Float32Array(MAXP),pEmit=new Float32Array(MAXP),pType=new Uint8Array(MAXP);
let pHead=0;
const pGeo=new THREE.BufferGeometry();
const posAttr=new THREE.BufferAttribute(pPos,3),colAttr=new THREE.BufferAttribute(pCol,3),sizeAttr=new THREE.BufferAttribute(pSize,1);
pGeo.setAttribute('position',posAttr);pGeo.setAttribute('aColor',colAttr);pGeo.setAttribute('aSize',sizeAttr);
const pMat=new THREE.ShaderMaterial({
  uniforms:{uScale:{value:window.innerHeight*0.5*PR}},
  vertexShader:'attribute vec3 aColor;attribute float aSize;uniform float uScale;varying vec3 vC;void main(){vC=aColor;vec4 mv=modelViewMatrix*vec4(position,1.0);gl_PointSize=min(aSize*uScale/max(-mv.z,0.1),70.0);gl_Position=projectionMatrix*mv;}',
  fragmentShader:'varying vec3 vC;void main(){float d=length(gl_PointCoord-vec2(0.5));if(d>0.5)discard;float a=1.0-smoothstep(0.0,0.5,d);a=a*a;gl_FragColor=vec4(vC*(0.35+a),a);}',
  transparent:true,depthWrite:false,blending:THREE.AdditiveBlending
});
const points=new THREE.Points(pGeo,pMat);points.frustumCulled=false;scene.add(points);
// type: 0 plain, 1 trail-emitting spark, 2 flickering sparkle, 3 crackle-on-death
function spawnP(x,y,z,vx,vy,vz,r,g,b,life,drag,grav,type,size){
  const i=pHead;pHead=(pHead+1)%MAXP;const i3=i*3;
  pPos[i3]=x;pPos[i3+1]=y;pPos[i3+2]=z;pVel[i3]=vx;pVel[i3+1]=vy;pVel[i3+2]=vz;
  pBase[i3]=r;pBase[i3+1]=g;pBase[i3+2]=b;pCol[i3]=r;pCol[i3+1]=g;pCol[i3+2]=b;
  pLife[i]=life;pMax[i]=life;pDrag[i]=drag;pGrav[i]=grav;pType[i]=type;pSize[i]=size;pEmit[i]=0.02+Math.random()*0.04;
}
let crackleBudget=0;
function updateParticles(dt){
  crackleBudget=2;
  for(let i=0;i<MAXP;i++){
    const L=pLife[i];if(L<=0)continue;
    const nl=L-dt,i3=i*3;
    if(nl<=0){
      pLife[i]=0;pSize[i]=0;pCol[i3]=pCol[i3+1]=pCol[i3+2]=0;
      if(pType[i]===3){
        const x=pPos[i3],y=pPos[i3+1],z=pPos[i3+2];
        for(let k=0;k<4;k++)spawnP(x,y,z,rnd(-8,8),rnd(-8,8),rnd(-8,8),1,0.95,0.7,0.3,2,0,2,0.7);
        if(crackleBudget>0&&AC&&Math.random()<0.35){crackleBudget--;const d=Math.hypot(x-P.x,z-P.z);pataka(AC.currentTime+Math.random()*0.05,0.2/(1+d/30),panFor(x,z),false);}
      }
      continue;
    }
    pLife[i]=nl;
    const dk=Math.exp(-pDrag[i]*dt);
    pVel[i3]*=dk;pVel[i3+1]=pVel[i3+1]*dk-pGrav[i]*dt;pVel[i3+2]*=dk;
    pPos[i3]+=pVel[i3]*dt;pPos[i3+1]+=pVel[i3+1]*dt;pPos[i3+2]+=pVel[i3+2]*dt;
    const f=nl/pMax[i];let k=f<0.55?f/0.55:1;k=Math.pow(k,1.2);
    const fl=pType[i]===2?(0.5+0.5*Math.random()):1;
    pCol[i3]=pBase[i3]*k*fl;pCol[i3+1]=pBase[i3+1]*k*fl;pCol[i3+2]=pBase[i3+2]*k*fl;
    if(pType[i]===1){
      pEmit[i]-=dt;
      if(pEmit[i]<=0){
        pEmit[i]=0.05;
        spawnP(pPos[i3],pPos[i3+1],pPos[i3+2],rnd(-0.7,0.7),rnd(-0.8,0.3),rnd(-0.7,0.7),pBase[i3]*0.8,pBase[i3+1]*0.75,pBase[i3+2]*0.7,0.35+Math.random()*0.3,2.5,2,2,0.65);
      }
    }
  }
  posAttr.needsUpdate=true;colAttr.needsUpdate=true;sizeAttr.needsUpdate=true;
}

/* ================= FIREWORKS ================= */
const PALS=[[[1,.2,.25],[1,.85,.3]],[[1,.2,.8],[.3,.9,1]],[[.3,1,.4],[1,.8,.2]],[[1,.5,.1],[1,.9,.5]],[[.3,.5,1],[1,1,1]],[[.7,.3,1],[1,.5,.9]]];
const KINDS=['peony','multi','ring','willow','crossette','peony','multi'];
const rockets=[],timers=[];
const rocketGeo=new THREE.CylinderGeometry(0.09,0.09,1.3,6),rocketMat=new THREE.MeshBasicMaterial({color:0xff5a2a});
let T=0;
function after(sec,fn){timers.push({t:T+sec,fn:fn});}
function aroundPlayer(bias){
  const A=actor(),a=(bias&&Math.random()<0.65)?camH+rnd(-1.1,1.1):Math.random()*TAU,d=rnd(14,62);
  return[A.x+Math.sin(a)*d,A.z+Math.cos(a)*d];
}
function launchRocket(x,z){
  if(rockets.length>26)return;
  const gy=GY(),r={x:x,y:gy+0.6,z:z,vx:rnd(-1.5,1.5),vz:rnd(-1.5,1.5),vy:6,ty:gy+rnd(30,56),t:0,ph:Math.random()*6,mesh:new THREE.Mesh(rocketGeo,rocketMat)};
  scene.add(r.mesh);rockets.push(r);
  for(let i=0;i<12;i++)spawnP(x,gy+0.5,z,rnd(-4,4),rnd(2,8),rnd(-4,4),1,0.7,0.25,0.4+Math.random()*0.3,1.8,10,2,0.8);
  sfxLaunch(x,z);
}
function updateRockets(dt){
  for(let i=rockets.length-1;i>=0;i--){
    const r=rockets[i];r.t+=dt;
    r.vy=Math.min(r.vy+4*dt,17);
    r.x+=(r.vx+Math.sin(r.t*7+r.ph)*1.3)*dt;r.z+=(r.vz+Math.cos(r.t*6+r.ph)*1.3)*dt;r.y+=r.vy*dt;
    r.mesh.position.set(r.x,r.y,r.z);
    for(let k=0;k<3;k++)spawnP(r.x+rnd(-0.1,0.1),r.y-0.6-k*0.35,r.z+rnd(-0.1,0.1),rnd(-0.8,0.8),rnd(-3,-1),rnd(-0.8,0.8),1,0.55+Math.random()*0.25,0.15,0.6+Math.random()*0.5,1.5,3,2,0.55);
    spawnP(r.x,r.y-0.2,r.z,0,0,0,1,0.85,0.5,0.06,1,0,0,1.9);
    if(r.y>=r.ty){scene.remove(r.mesh);rockets.splice(i,1);explode(r.x,r.y,r.z);}
  }
}
function explode(x,y,z){
  const pal=PALS[(Math.random()*PALS.length)|0],kind=KINDS[(Math.random()*KINDS.length)|0],n=kind==='willow'?210:270;
  const c0=pal[0],c1=pal[1],tilt=rnd(0,Math.PI);
  for(let i=0;i<n;i++){
    let dx,dy,dz;
    if(kind==='ring'&&i<n*0.75){
      const a=i/(n*0.75)*TAU;dx=Math.cos(a);dy=0;dz=Math.sin(a);
      const ct=Math.cos(tilt),st=Math.sin(tilt),y2=dy*ct-dz*st,z2=dy*st+dz*ct;dy=y2;dz=z2;
    }else{const u=Math.random()*2-1,a=Math.random()*TAU,s=Math.sqrt(1-u*u);dx=s*Math.cos(a);dy=u;dz=s*Math.sin(a);}
    let sp=rnd(14,23),life=rnd(1.8,2.6),c=c0,type=Math.random()<0.25?1:2,grav=5,drag=1.6,size=rnd(1.0,1.6);
    if(kind==='multi')c=pal[(Math.random()*2)|0];
    else if(kind==='ring'){if(i<n*0.75){sp=19;c=c0;life=2.2;}else{sp=rnd(5,9);c=c1;life=1.8;}}
    else if(kind==='willow'){sp=rnd(8,16);c=[1,0.75,0.3];life=rnd(3,4.2);drag=1.1;grav=7;type=Math.random()<0.6?1:2;}
    else if(kind==='crossette'){sp=rnd(14,20);life=rnd(1.2,1.7);type=3;size=1.5;}
    const v=rnd(0.85,1.05);
    spawnP(x,y,z,dx*sp,dy*sp,dz*sp,Math.min(1,c[0]*v+0.08),Math.min(1,c[1]*v+0.08),Math.min(1,c[2]*v+0.08),life,drag,grav,type,size);
  }
  spawnP(x,y,z,0,0,0,1,0.95,0.85,0.28,1,0,0,11);
  burstLight.position.set(x,y,z);burstLight.color.setRGB(c0[0],c0[1],c0[2]);burstLightI=2.6;
  sfxBurst(x,y,z);
  for(const p of people)if(Math.random()<0.3)p.cheer=rnd(0.4,1.2);
  if(WK.on&&Math.random()<0.5)WK.cheerT=rnd(1,1.8); // your man cheers at the fireworks too
}
function volley(remote){
  const n=Math.min(9,3+2*save.up.volley);
  for(let i=0;i<n;i++)after(i*0.13,function(){const q=aroundPlayer(true);launchRocket(q[0],q[1]);});
  if(!remote)netSend({fw:1,x:P.x,z:P.z});
}
let autoT=3;
function updateFireworks(dt){
  autoT-=dt;
  if(autoT<=0){
    autoT=Math.max(1.0,2.6-0.5*save.up.auto)*rnd(0.8,1.2);
    const q=aroundPlayer(true);launchRocket(q[0],q[1]);
    if(save.up.auto>=2){const q2=aroundPlayer(false);after(0.4,function(){launchRocket(q2[0],q2[1]);});}
  }
  updateRockets(dt);
  burstLightI=Math.max(0,burstLightI-dt*3);burstLight.intensity=burstLightI;
  for(let i=timers.length-1;i>=0;i--)if(T>=timers[i].t){const f=timers[i].fn;timers.splice(i,1);f();}
  updateParticles(dt);
}
function patakaChain(){
  const A=actor(),fx=A.x+Math.sin(A.h)*10,fz=A.z+Math.cos(A.h)*10;
  for(let i=0;i<14;i++)after(i*0.11+Math.random()*0.04,function(){
    const x=fx+rnd(-2.5,2.5),z=fz+rnd(-2.5,2.5);
    if(AC)pataka(AC.currentTime,0.5,panFor(x,z),false);
    for(let k=0;k<12;k++)spawnP(x,0.4,z,rnd(-6,6),rnd(5,12),rnd(-6,6),1,0.8,0.35,0.4+Math.random()*0.3,1.5,14,2,0.7);
    spawnP(x,0.6,z,0,0,0,1,0.9,0.6,0.08,1,0,0,5);
  });
}

/* ================= CRASH EFFECTS ================= */
const flashes=[];
(function(){
  const c=document.createElement('canvas');c.width=c.height=128;const g=c.getContext('2d');
  const grd=g.createRadialGradient(64,64,2,64,64,62);grd.addColorStop(0,'rgba(255,255,230,1)');grd.addColorStop(0.35,'rgba(255,200,80,0.8)');grd.addColorStop(1,'rgba(255,120,0,0)');
  g.fillStyle=grd;g.fillRect(0,0,128,128);
  g.strokeStyle='rgba(255,240,160,0.9)';g.lineWidth=5;g.translate(64,64);
  for(let k=0;k<8;k++){g.rotate(TAU/8);g.beginPath();g.moveTo(14,0);g.lineTo(60,0);g.stroke();}
  const tex=new THREE.CanvasTexture(c);
  for(let i=0;i<3;i++){const sp=new THREE.Sprite(new THREE.SpriteMaterial({map:tex,transparent:true,blending:THREE.AdditiveBlending,depthWrite:false,opacity:0}));sp.visible=false;scene.add(sp);flashes.push({sp:sp,life:0,s:1});}
})();
let shake=0,lastCrashT=-10,contactNow=false,contactPrev=false,popIdx=0;
const POPS=['BONK!','CLANG!','BAM!','OOF!','WHAM!'];
function crashFx(x,y,z,s){
  let f=flashes[0];for(const q of flashes)if(q.life<f.life)f=q;
  f.life=0.35;f.s=2+3*s;f.sp.position.set(x,y,z);f.sp.visible=true;
  for(let i=0;i<14;i++)spawnP(x,y,z,rnd(-9,9)*s,rnd(2,11)*s,rnd(-9,9)*s,1,rnd(0.7,0.95),rnd(0.3,0.6),rnd(0.3,0.6),2,12,0,0.55);
  for(let i=0;i<8;i++)spawnP(x,y-0.3,z,rnd(-3,3),rnd(0.5,2.5),rnd(-3,3),0.45,0.42,0.38,0.7,2,-1,0,2.4);
  const p=$('pop');p.textContent=POPS[(popIdx++)%POPS.length];p.classList.remove('go');void p.offsetWidth;p.classList.add('go');
}
function updateFlashes(dt){
  for(const f of flashes){
    if(f.life<=0){f.sp.visible=false;continue;}
    f.life-=dt;const k=Math.max(0,f.life/0.35);
    f.sp.material.opacity=k;const sc=f.s*(1.4-0.4*k);f.sp.scale.set(sc,sc,1);
  }
}
// Single entry point used by every collision (building, vehicle, obstacle, tree, lamp)
function registerImpact(impact,ix,iy,iz){
  contactNow=true;
  if(impact<3.2)return;                    // light scrape: no sound
  if(T-lastCrashT<0.9)return;              // cooldown between crash sounds
  if(contactPrev&&impact<12)return;        // still touching the same thing: don't repeat
  lastCrashT=T;
  const s=clamp(impact/18,0.25,1);
  playCrash(s);crashFx(ix,iy,iz,s);
  shake=Math.max(shake,0.15+0.45*s);P.hitT=0.4;
  damageVehicle(s);
}

/* ================= INPUT ================= */
const keys={};let started=false,shopOpen=false,talkOpen=false,camMode=0;
function isK(){for(let i=0;i<arguments.length;i++)if(keys[arguments[i]])return true;return false;}
window.addEventListener('keydown',function(e){
  if(kicked)return;
  const tg=e.target; // typing in an admin text box must not drive the car
  if(tg&&tg.tagName==='INPUT'&&(tg.type==='text'||tg.type==='password'||tg.type==='number')){
    if(e.code==='Enter'&&tg.id==='admPw')unlockAdmin();
    if(e.code==='Escape'||e.code==='Enter')tg.blur();
    return;
  }
  if(['Space','ArrowUp','ArrowDown','ArrowLeft','ArrowRight','Tab'].indexOf(e.code)>=0)e.preventDefault();
  if(!started){startGame();return;}
  if(e.code==='F2'||e.code==='Backquote'){e.preventDefault();toggleAdmin();return;}
  if(e.repeat){keys[e.code]=true;return;}
  keys[e.code]=true;
  if(e.code==='Escape'&&shopOpen){toggleShop(false);return;}
  if(e.code==='KeyE'){if(shopOpen)toggleShop(false);else if(nearShop())toggleShop(true);return;}
  if(talkOpen){ // talking to the shopkeeper: 1 = buy patakas, 2 = bye (number keys must not switch vehicles here)
    if(e.code==='Digit1'||e.code==='Numpad1')showList();
    else if(e.code==='Digit2'||e.code==='Numpad2')closeShop(true);
    return;
  }
  if(shopOpen)return;
  if(e.code==='KeyX'){toggleOnFoot();return;}
  if(e.code==='KeyC'&&WK.on){WK.cheerT=2.5;return;}
  const m=/^Digit([1-5])$/.exec(e.code);if(m){pickVehicle(ORDER[+m[1]-1]);return;}
  if(e.code==='KeyF')volley(false);
  else if(e.code==='KeyG')patakaChain();
  else if(e.code==='KeyV')camMode=(camMode+1)%3;
  else if(e.code==='KeyM'){muted=!muted;if(master)master.gain.value=muted?0:0.85;}
  else if(e.code==='KeyH'){const h=$('help');h.style.display=h.style.display==='none'?'block':'none';}
});
window.addEventListener('keyup',function(e){keys[e.code]=false;});
window.addEventListener('blur',function(){for(const k in keys)keys[k]=false;});
function startGame(){started=true;$('start').style.display='none';initAudio();toast('A Diwali Shop is right beside you. Press E to talk to the shopkeeper.',4500);}
$('start').addEventListener('click',startGame);
window.addEventListener('resize',function(){camera.aspect=window.innerWidth/window.innerHeight;camera.updateProjectionMatrix();renderer.setSize(window.innerWidth,window.innerHeight);pMat.uniforms.uScale.value=window.innerHeight*0.5*PR;});

/* ================= SHOP ================= */
const SHOP=[
  {id:'volley',name:'Rocket Packs',desc:'More rockets per F volley',max:3,cost:l=>25+l*20},
  {id:'auto',name:'Auto Fireworks Show',desc:'Faster automatic sky show',max:3,cost:l=>40+l*30},
  {id:'nitro',name:'Nitro Boost',desc:'Hold Shift to boost ground vehicles',max:1,cost:()=>50},
  {id:'gold',name:'Golden Paint',desc:'Gold finish for your vehicles',max:1,cost:()=>30},
  {id:'magnet',name:'Coin Magnet',desc:'Collect coins from farther away',max:2,cost:l=>35+l*25},
  {id:'repair',name:'Repair Vehicle',desc:'Fix your current vehicle to 100%',repair:true,cost:()=>20}
];
// one "Reinforce" upgrade per vehicle: each level cuts crash damage by 13% (max level 5 = 65% less damage)
ORDER.forEach(function(t){SHOP.push({id:'str_'+t,str:t,name:'Reinforce '+DEFS[t].name,desc:'Takes less crash damage',max:5,cost:function(l){return 30+l*25;}});});
function strMul(type){return 1-0.13*(save.str[type]|0);}
function nearShop(){const A=actor();if(A.y>=4)return false;for(let i=0;i<shopPts.length;i++)if(Math.hypot(A.x-shopPts[i].x,A.z-shopPts[i].z)<16)return true;return false;}
function renderShop(){
  $('shopcoins').textContent=save.coins;const box=$('shoplist');box.innerHTML='';
  SHOP.forEach(function(it){
    const rep=!!it.repair,lv=rep?0:(it.str?(save.str[it.str]|0):(save.up[it.id]||0)),maxed=rep?P.hp>=100:lv>=it.max,cost=it.cost(lv);
    const row=document.createElement('div');row.className='row';
    row.innerHTML='<div><b>'+it.name+(it.str===P.type?' (current)':'')+'</b> <small>'+(rep?'Condition '+Math.round(P.hp)+'%':'Lv '+lv+'/'+it.max)+' &middot; '+it.desc+'</small></div>';
    const b=document.createElement('button');b.textContent=maxed?(rep?'Not needed':'Owned'):(rep?'Fix ':'Buy ')+cost;b.disabled=maxed||save.coins<cost;
    b.addEventListener('click',function(){
      if(maxed||save.coins<cost)return;
      if(rep){save.coins-=cost;persist();P.hp=100;HPS[P.type]=100;refreshDamage();dismissCrowd();sfxBuy();updateHUDcoins();renderShop();toast('Vehicle repaired!',2000);return;}
      save.coins-=cost;if(it.str)save.str[it.str]=lv+1;else save.up[it.id]=lv+1;persist();sfxBuy();
      if(it.id==='gold'&&P.v)paintGold(P.v);
      updateHUDcoins();renderShop();
    });
    row.appendChild(b);box.appendChild(row);
  });
}
// Shopkeeper conversation: greeting -> [Buy patakas | Bye]. "Buy patakas" opens the shop list, "Bye" walks away.
function toggleShop(on){
  if(!on){closeShop(false);return;}
  shopOpen=true;talkOpen=true;for(const k in keys)keys[k]=false;
  $('talktxt').textContent='Namaste! Why are you here?';
  $('talk').style.display='block';$('shop').style.display='none';
}
function showList(){talkOpen=false;$('talk').style.display='none';$('shop').style.display='flex';renderShop();}
function closeShop(bye){
  shopOpen=false;talkOpen=false;$('talk').style.display='none';$('shop').style.display='none';
  if(bye)toast('Shopkeeper: Come again! Shubh Diwali!',2600);
}
$('optBuy').addEventListener('click',showList);
$('optBye').addEventListener('click',function(){closeShop(true);});
$('shopclose').addEventListener('click',function(){closeShop(true);});
function updateHUDcoins(){$('coins').textContent=save.coins;}

/* ================= PLAYER PHYSICS + COLLISIONS ================= */
function updatePlayer(dt){
  const def=DEFS[P.type],plane=P.type==='plane';
  if(WK.on){ // you are walking: the vehicle stays parked where you left it
    P.speed=0;const g0=P.v.group,f0=1-P.hp/100;
    g0.position.set(P.x,P.y,P.z);g0.rotation.y=P.h;g0.rotation.z=P.pull*f0*0.07;g0.rotation.x=plane?0:f0*0.05;
    return;
  }
  let thr=0,steer=0,hb=false,boost=false;
  if(started&&!shopOpen){
    thr=(isK('KeyW','ArrowUp')?1:0)-(isK('KeyS','ArrowDown')?1:0);
    steer=(isK('KeyA','ArrowLeft')?1:0)-(isK('KeyD','ArrowRight')?1:0);
    hb=isK('Space');boost=isK('ShiftLeft','ShiftRight');
  }
  const broken=P.hp<=0,dmgF=1-P.hp/100;
  if(broken){thr=0;steer=0;hb=false;boost=false;}
  let max=def.max*(1-0.4*dmgF)*ADM.speed,acc=def.acc*(1-0.3*dmgF)*ADM.speed;
  if(save.up.nitro&&boost&&!plane&&thr>0){max*=1.3;acc*=1.9;}
  if(thr>0)P.speed+=acc*dt;
  else if(thr<0)P.speed-=(P.speed>0?acc*1.6:acc*0.6)*dt;
  else P.speed*=Math.exp(-0.5*dt);
  if(plane){P.speed=clamp(P.speed,0,max);}else{P.speed=clamp(P.speed,-max*0.35,max);}
  if(broken)P.speed*=Math.exp(-2.5*dt);
  if(hb&&!plane)P.speed*=Math.exp(-2.8*dt);
  const sf=clamp(Math.abs(P.speed)/6,0,1)*(P.speed<0?-1:1);
  if(!broken&&Math.abs(P.speed)>5)steer=clamp(steer+P.pull*dmgF*0.3,-1,1); // damaged steering drifts to one side
  P.steer+=(steer-P.steer)*Math.min(1,dt*8);
  P.h+=P.steer*def.turn*dt*sf*(hb&&!plane?1.5:1);
  P.x+=Math.sin(P.h)*P.speed*dt;P.z+=Math.cos(P.h)*P.speed*dt;
  if(plane){
    const fly=P.speed>14,lift=clamp((P.speed-14)/10,0,1);
    if(hb&&fly)P.y+=14*lift*dt;
    if(boost&&P.y>0.95)P.y-=14*dt;
    if((P.y>1.5&&!fly)||(broken&&P.y>0.95))P.y-=(broken?18:10)*dt;
    P.y=clamp(P.y,0.95,150);
    if(P.v.prop)P.v.prop.rotation.z+=dt*(15+P.speed);
  }else P.y=0;
  P.x=clamp(P.x,-215,215);P.z=clamp(P.z,-215,215);
  collidePlayer();
  const g=P.v.group;g.position.set(P.x,P.y,P.z);g.rotation.y=P.h;
  let roll=-P.steer*clamp(P.speed/20,0,1)*(plane?0.5:(P.type==='bike'||P.type==='scooter'?0.35:0.06));
  if(P.hitT>0){P.hitT-=dt;roll+=Math.sin(P.hitT*40)*0.08*(P.hitT/0.4);}
  roll+=P.pull*dmgF*0.07;
  g.rotation.z=roll;
  g.rotation.x=plane?(hb&&P.speed>14?-0.2:(boost&&P.y>1?0.15:0)):dmgF*0.05+(broken?0.04:0);
  for(const w of P.v.wheels)w.rotation.x+=P.speed*dt/0.45;
}
function bounceFrom(nx,nz,ovx,ovz,k){
  const vx=Math.sin(P.h)*P.speed,vz=Math.cos(P.h)*P.speed;
  const rvx=vx-ovx,rvz=vz-ovz,vn=rvx*nx+rvz*nz;
  if(vn>=0)return 0;
  const impact=-vn,tx=rvx-vn*nx,tz=rvz-vn*nz,tm=Math.hypot(tx,tz);
  if(tm>1.5){
    const dirSign=P.speed>=0?1:-1,target=Math.atan2(tx*dirSign,tz*dirSign);
    P.h+=angDiff(P.h,target)*0.5;P.speed=dirSign*tm*0.85*k;
  }else P.speed*=-0.2;
  return impact;
}
function collidePlayer(){
  if(ADM.noclip)return;
  const r=DEFS[P.type].r,plane=P.type==='plane',hi=plane&&P.y>4;
  const fx=Math.sin(P.h),fz=Math.cos(P.h);
  const ix=P.x+fx*r*0.8,iy=P.y+1,iz=P.z+fz*r*0.8;
  for(let bi=0;bi<buildings.length;bi++){
    const b=buildings[bi];
    if(plane&&P.y>b.h+2)continue;
    if(Math.abs(P.x-b.x)>b.hw+r+1||Math.abs(P.z-b.z)>b.hd+r+1)continue;
    const cx=clamp(P.x,b.x-b.hw,b.x+b.hw),cz=clamp(P.z,b.z-b.hd,b.z+b.hd),dx=P.x-cx,dz=P.z-cz,d=Math.hypot(dx,dz);
    if(d>=r)continue;
    let nx,nz,pen;
    if(d<1e-4){
      const px=b.hw-Math.abs(P.x-b.x),pz=b.hd-Math.abs(P.z-b.z);
      if(px<pz){nx=Math.sign(P.x-b.x)||1;nz=0;pen=px+r;}else{nx=0;nz=Math.sign(P.z-b.z)||1;pen=pz+r;}
    }else{nx=dx/d;nz=dz/d;pen=r-d;}
    P.x+=nx*pen;P.z+=nz*pen;
    const imp=bounceFrom(nx,nz,0,0,1);
    if(imp>0)registerImpact(imp,ix,iy,iz);else contactNow=true;
  }
  for(let si=0;si<statics.length;si++){
    const s=statics[si];
    if(plane&&P.y>s.h)continue;
    const dx=P.x-s.x,dz=P.z-s.z;if(Math.abs(dx)>r+s.r+1||Math.abs(dz)>r+s.r+1)continue;
    const d=Math.hypot(dx,dz),rr=r+s.r;if(d>=rr)continue;
    const nx=d>1e-4?dx/d:1,nz=d>1e-4?dz/d:0;
    P.x+=nx*(rr-d);P.z+=nz*(rr-d);
    const imp=bounceFrom(nx,nz,0,0,1);
    if(imp>0)registerImpact(imp,ix,iy,iz);else contactNow=true;
  }
  if(hi)return;
  for(let ai=0;ai<ais.length;ai++){
    const a=ais[ai],dx=P.x-a.x,dz=P.z-a.z,rr=r+a.r;
    if(Math.abs(dx)>rr||Math.abs(dz)>rr)continue;
    const d=Math.hypot(dx,dz);if(d>=rr)continue;
    const nx=d>1e-4?dx/d:1,nz=d>1e-4?dz/d:0;
    P.x+=nx*(rr-d)*0.7;P.z+=nz*(rr-d)*0.7;
    const avx=a.horiz?a.dir*a.cur:0,avz=a.horiz?0:a.dir*a.cur;
    const imp=bounceFrom(nx,nz,avx,avz,0.8);
    // knock the traffic vehicle gently away and let it recover
    const kick=-nx,kickz=-nz,pushV=Math.max(imp,1)*0.6;
    if(a.horiz){a.kx+=kick*pushV;a.kz+=kickz*pushV;}else{a.kx+=kickz*pushV;a.kz+=kick*pushV;}
    a.pause=Math.max(a.pause,1.5);a.spin+=(Math.random()<0.5?-1:1)*Math.min(0.5,imp*0.04);
    if(imp>0)registerImpact(imp,ix,iy,iz);else contactNow=true;
  }
  for(let oi=0;oi<obstacles.length;oi++){
    const o=obstacles[oi],dx=P.x-o.x,dz=P.z-o.z,rr=r+o.r;
    if(Math.abs(dx)>rr||Math.abs(dz)>rr)continue;
    const d=Math.hypot(dx,dz);if(d>=rr)continue;
    const nx=d>1e-4?dx/d:1,nz=d>1e-4?dz/d:0;
    const closing=Math.max(0,-(Math.sin(P.h)*P.speed*nx+Math.cos(P.h)*P.speed*nz));
    P.x+=nx*(rr-d)*0.3;P.z+=nz*(rr-d)*0.3;
    o.vx=-nx*Math.max(closing,2)*1.1;o.vz=-nz*Math.max(closing,2)*1.1;o.spin=rnd(-8,8);
    o.x+=-nx*(rr-d)*0.7;o.z+=-nz*(rr-d)*0.7;
    P.speed*=closing>3?0.88:0.97;
    if(closing>0)registerImpact(closing*0.85,ix,iy,iz);else contactNow=true;
  }
}

/* ================= ON FOOT, DAMAGE & CROWD ================= */
const WK={on:false,x:0,y:0,z:0,h:0,sp:0,ph:0,mesh:null,hum:null,up:0,cheerT:0};
const crowd=[];
let toastUntil=0;
function actor(){return WK.on?WK:P;}
function toast(msg,ms){const t=$('toast');t.textContent=msg;t.style.display='block';toastUntil=T+(ms||3000)/1000;}
function makePerson(shirt,hero){
  const h=hero?buildHuman(0x6f9aa8,0x2b2b30,0xc9976a,0x201813)  // the grey-blue t-shirt man
              :buildHuman(shirt,SHORTS[(Math.random()*SHORTS.length)|0],SKINS[(Math.random()*SKINS.length)|0],HAIRS[(Math.random()*HAIRS.length)|0]);
  scene.add(h.g);return h;
}
function blockedAt(x,z,pad){
  for(let i=0;i<buildings.length;i++){const b=buildings[i];if(Math.abs(x-b.x)<b.hw+pad&&Math.abs(z-b.z)<b.hd+pad)return true;}
  for(let i=0;i<statics.length;i++){const s=statics[i];if(Math.hypot(x-s.x,z-s.z)<s.r+pad)return true;}
  return false;
}
// --- getting out / in ---
function exitVehicle(){
  if(WK.on)return;
  if(P.type==='plane'&&P.y>2.5){toast('Land the plane first, then press X',2500);return;}
  if(Math.abs(P.speed)>7){toast('Slow down before getting out',2200);return;}
  const r=DEFS[P.type].r+1.1,lx=Math.cos(P.h),lz=-Math.sin(P.h);
  const tries=[[lx,lz],[-lx,-lz],[-Math.sin(P.h),-Math.cos(P.h)],[Math.sin(P.h),Math.cos(P.h)]];
  let pos=null;
  for(let i=0;i<tries.length;i++){const x=P.x+tries[i][0]*r,z=P.z+tries[i][1]*r;if(!blockedAt(x,z,0.6)){pos=[x,z];break;}}
  if(!pos)pos=[P.x+lx*r,P.z+lz*r];
  if(!WK.mesh){const p=makePerson(0,true);WK.mesh=p.g;WK.hum=p;}
  WK.x=pos[0];WK.z=pos[1];WK.h=P.h;WK.sp=0;WK.on=true;WK.mesh.visible=true;P.speed=0;
  sfxDoor();toast('On foot: W/S walk, A/D turn, Shift run. Press X near your vehicle to get back in.',4200);
}
function boardVehicle(){
  if(!WK.on)return;
  if(Math.hypot(WK.x-P.x,WK.z-P.z)>DEFS[P.type].r+4){toast('Walk closer to your vehicle first',2200);return;}
  if(P.hp<=0){toast('This vehicle is broken. Repair it at the shop (E) or press 1-5 for another one.',4200);return;}
  WK.on=false;WK.mesh.visible=false;sfxDoor();
}
function toggleOnFoot(){if(WK.on)boardVehicle();else exitVehicle();}
function pickVehicle(type){
  if(WK.on){
    if(type===P.type){boardVehicle();return;}
    P.x=WK.x;P.z=WK.z;P.h=WK.h;WK.on=false;WK.mesh.visible=false;sfxDoor();
  }
  setVehicle(type);
}
function updateWalker(dt){
  if(!WK.on)return;
  let fwd=0,turn=0,run=false;
  if(started&&!shopOpen){
    fwd=(isK('KeyW','ArrowUp')?1:0)-(isK('KeyS','ArrowDown')?1:0);
    turn=(isK('KeyA','ArrowLeft')?1:0)-(isK('KeyD','ArrowRight')?1:0);
    run=isK('ShiftLeft','ShiftRight');
  }
  WK.h+=turn*2.8*dt;
  const target=(fwd>0?(run?9:5):(fwd<0?-2.5:0))*Math.min(ADM.speed,3);
  WK.sp+=(target-WK.sp)*Math.min(1,dt*8);
  WK.x=clamp(WK.x+Math.sin(WK.h)*WK.sp*dt,-215,215);WK.z=clamp(WK.z+Math.cos(WK.h)*WK.sp*dt,-215,215);
  const r=0.5;
  for(let i=0;i<buildings.length;i++){
    const b=buildings[i];
    if(Math.abs(WK.x-b.x)>b.hw+r+1||Math.abs(WK.z-b.z)>b.hd+r+1)continue;
    const cx=clamp(WK.x,b.x-b.hw,b.x+b.hw),cz=clamp(WK.z,b.z-b.hd,b.z+b.hd),dx=WK.x-cx,dz=WK.z-cz,d=Math.hypot(dx,dz);
    if(d>=r)continue;
    if(d<1e-4){
      const px=b.hw-Math.abs(WK.x-b.x),pz=b.hd-Math.abs(WK.z-b.z);
      if(px<pz)WK.x+=(Math.sign(WK.x-b.x)||1)*(px+r);else WK.z+=(Math.sign(WK.z-b.z)||1)*(pz+r);
    }else{WK.x+=dx/d*(r-d);WK.z+=dz/d*(r-d);}
  }
  function pushCircle(ox,oz,rr){const dx=WK.x-ox,dz=WK.z-oz,d=Math.hypot(dx,dz);if(d>=rr)return null;const nx=d>1e-4?dx/d:1,nz=d>1e-4?dz/d:0;WK.x+=nx*(rr-d);WK.z+=nz*(rr-d);return[nx,nz];}
  for(let i=0;i<statics.length;i++){const s=statics[i];if(Math.abs(WK.x-s.x)<3&&Math.abs(WK.z-s.z)<3)pushCircle(s.x,s.z,r+s.r);}
  pushCircle(P.x,P.z,r+DEFS[P.type].r*0.9);
  for(let i=0;i<ais.length;i++){const a=ais[i];if(pushCircle(a.x,a.z,r+a.r*0.9))a.pause=Math.max(a.pause,0.6);}
  for(let i=0;i<obstacles.length;i++){const o=obstacles[i],n=pushCircle(o.x,o.z,r+o.r);if(n){o.vx=-n[0]*2.5;o.vz=-n[1]*2.5;o.spin=rnd(-4,4);}}
  WK.ph+=dt*Math.abs(WK.sp)*1.8;
  WK.cheerT-=dt;WK.up+=((WK.cheerT>0?1:0)-WK.up)*Math.min(1,dt*9);
  const sw=Math.sin(WK.ph)*0.7*Math.min(1,Math.abs(WK.sp)/3);
  poseHuman(WK.hum,sw,WK.up);
  if(WK.up>0.05){WK.hum.a1.rotation.x=Math.sin(T*9)*0.25*WK.up;WK.hum.a2.rotation.x=-Math.sin(T*9)*0.25*WK.up;}
  WK.mesh.position.set(WK.x,0.3+Math.abs(Math.sin(T*8))*0.1*WK.up,WK.z);WK.mesh.rotation.y=WK.h;
}
// --- damage ---
function damageVehicle(s){
  if(ADM.god)return;
  const before=P.hp;
  P.hp=Math.max(0,P.hp-(14+22*s)*strMul(P.type));HPS[P.type]=P.hp;
  refreshDamage();
  if(P.hp<=0&&before>0){
    sfxBreak();toast('Your vehicle broke down! Press X to get out. Fix it at the shop (E) or press 1-5 for another vehicle.',5500);
    spawnCrowd(14,true);
  }else if(P.hp<75){
    toast(P.hp<45?'Your vehicle is badly damaged and smoking!':'Your vehicle is getting dented...',2200);
    spawnCrowd(Math.random()<0.5?2:1,false);
  }
}
let smokeT=0;
function updateDamageFx(dt){
  if(P.hp>=50)return;
  smokeT-=dt;if(smokeT>0)return;
  const broken=P.hp<=0;smokeT=broken?0.03:0.07;
  const def=DEFS[P.type],fx=Math.sin(P.h),fz=Math.cos(P.h);
  const x=P.x+fx*def.fz*0.7,z=P.z+fz*def.fz*0.7,y=P.y+def.sy,c=broken?0.2:0.32;
  spawnP(x+rnd(-0.2,0.2),y,z+rnd(-0.2,0.2),rnd(-0.6,0.6),rnd(1.5,3),rnd(-0.6,0.6),c,c,c,1.3,0.6,-0.4,0,broken?3.4:2.6);
  if(broken&&Math.random()<0.12)spawnP(x,y,z,rnd(-3,3),rnd(2,5),rnd(-3,3),1,0.55,0.15,0.4,1.5,8,2,0.6);
}
// --- bystanders gather around a crashed / broken vehicle (just watching, no one hurt) ---
function spawnCrowd(n,stay){
  const def=DEFS[P.type],cols=[0xe8567a,0x4a90e2,0xf5a623,0x7ed321,0xbd10e0,0xf8e71c,0xff6b6b,0x50e3c2];
  for(let k=0;k<n&&crowd.length<24;k++){
    let tx=0,tz=0,ok=false;
    for(let t=0;t<12&&!ok;t++){const a=Math.random()*TAU,rad=def.r+2.4+Math.random()*3.5;tx=P.x+Math.sin(a)*rad;tz=P.z+Math.cos(a)*rad;if(!blockedAt(tx,tz,0.8))ok=true;}
    if(!ok)continue;
    let sx=0,sz=0,ok2=false;
    for(let t=0;t<25&&!ok2;t++){
      const a=Math.random()*TAU,d=rnd(14,28);sx=P.x+Math.sin(a)*d;sz=P.z+Math.cos(a)*d;
      if(blockedAt(sx,sz,1))continue;
      let clear=true;
      for(let s=1;s<=8;s++){const q=s/8;if(blockedAt(sx+(tx-sx)*q,sz+(tz-sz)*q,0.5)){clear=false;break;}}
      if(clear)ok2=true;
    }
    if(!ok2){sx=tx;sz=tz;}
    const p=makePerson(cols[(Math.random()*cols.length)|0]);
    crowd.push({g:p.g,hum:p,up:0,x:sx,z:sz,tx:tx,tz:tz,state:'walk',ph:Math.random()*6,hop:0,hopT:rnd(1,3),away:0,leave:0,stay:!!stay,watchT:rnd(9,15),face:0});
  }
}
function dismissCrowd(){
  for(let i=0;i<crowd.length;i++){const c=crowd[i];c.state='leave';c.leave=5+Math.random()*2;c.away=Math.atan2(c.x-P.x,c.z-P.z)+rnd(-0.6,0.6);}
}
function updateCrowd(dt){
  for(let i=crowd.length-1;i>=0;i--){
    const c=crowd[i];let moving=false;
    if(c.state==='walk'){
      const dx=c.tx-c.x,dz=c.tz-c.z,d=Math.hypot(dx,dz);
      if(d<0.4)c.state='watch';
      else{const s=Math.min(3.2*dt,d);c.x+=dx/d*s;c.z+=dz/d*s;c.face=Math.atan2(dx,dz);moving=true;}
    }else if(c.state==='watch'){
      c.face=Math.atan2(P.x-c.x,P.z-c.z);c.hopT-=dt;
      if(c.hopT<=0){c.hop=0.5;c.hopT=rnd(1.2,3.5);}
      if(!c.stay){c.watchT-=dt;if(c.watchT<=0){c.state='leave';c.leave=5;c.away=Math.atan2(c.x-P.x,c.z-P.z)+rnd(-0.6,0.6);}}
    }else{
      const nx=c.x+Math.sin(c.away)*3*dt,nz=c.z+Math.cos(c.away)*3*dt;
      if(blockedAt(nx,nz,0.4))c.away+=1.2;else{c.x=nx;c.z=nz;}
      c.face=c.away;moving=true;c.leave-=dt;
      if(c.leave<=0){scene.remove(c.g);crowd.splice(i,1);continue;}
    }
    if(moving)c.ph+=dt*9;
    const sw=moving?Math.sin(c.ph)*0.6:0;c.up+=((c.hop>0?1:0)-c.up)*Math.min(1,dt*12);poseHuman(c.hum,sw,c.up);
    let y=0;if(c.hop>0){c.hop-=dt;y=Math.abs(Math.sin(c.hop*Math.PI/0.5))*0.35;}
    c.g.position.set(c.x,y,c.z);c.g.rotation.y=c.face;
  }
}

/* ================= CAMERA ================= */
let camDist=14,camHt=6;
function updateCamera(dt){
  const A=actor(),foot=WK.on;
  camH+=angDiff(camH,A.h)*Math.min(1,dt*3.5);
  const plane=!foot&&P.type==='plane';
  let td=foot?7:(plane?20:14),th=foot?3.4:(plane?8:6),look=foot?1.8:4,lo=foot?6:8;
  if(camMode===1){look=foot?16:(plane?26:24);lo=6;}
  else if(camMode===2){td+=foot?6:12;th+=foot?3:7;look=foot?1.5:3;}
  camDist+=(td-camDist)*Math.min(1,dt*3);camHt+=(th-camHt)*Math.min(1,dt*3);
  const sx=Math.sin(camH),sz=Math.cos(camH);
  let cx=A.x-sx*camDist,cy=A.y+camHt,cz=A.z-sz*camDist;
  if(shake>0){cx+=rnd(-1,1)*shake;cy+=rnd(-1,1)*shake*0.7;cz+=rnd(-1,1)*shake;shake=Math.max(0,shake-dt*1.6);}
  camera.position.set(cx,Math.max(cy,1),cz);
  camera.lookAt(A.x+sx*lo,A.y+look+(camMode===0&&!foot?6:0),A.z+sz*lo);
}

/* ================= MULTIPLAYER (same-browser tabs via BroadcastChannel) ================= */
const myId=Math.random().toString(36).slice(2,8),others=new Map();
let bc=null,netTimer=0;
function netSend(o){if(!bc||kicked)return;try{o.id=myId;bc.postMessage(o);}catch(e){}}
function netSendNow(){netSend({type:P.type,x:P.x,y:P.y,z:P.z,h:P.h,gold:save.up.gold?1:0});}
function makeLabel(text){
  const c=document.createElement('canvas');c.width=256;c.height=64;const g=c.getContext('2d');
  g.fillStyle='rgba(20,10,40,.7)';g.fillRect(0,8,256,48);g.fillStyle='#ffd36b';g.font='bold 30px Arial';g.textAlign='center';g.textBaseline='middle';g.fillText(text,128,33);
  const sp=new THREE.Sprite(new THREE.SpriteMaterial({map:new THREE.CanvasTexture(c),depthTest:false}));sp.scale.set(7,1.75,1);return sp;
}
try{
  bc=new BroadcastChannel('diwali-city-sim-v1');
  bc.onmessage=function(e){
    const m=e.data;if(!m||!m.id||m.id===myId)return;
    if(m.bye){const o=others.get(m.id);if(o){scene.remove(o.v.group);others.delete(m.id);}return;}
    if(m.adm){onAdminMsg(m);return;}
    if(m.fw){after(0,function(){for(let i=0;i<3;i++){const a=Math.random()*TAU,d=rnd(8,30);launchRocket(m.x+Math.sin(a)*d,m.z+Math.cos(a)*d);}});return;}
    let o=others.get(m.id);
    if(!o||o.type!==m.type){
      if(o)scene.remove(o.v.group);
      const v=buildVehicle(m.type,m.gold?0xe0b030:0x40e0d0);const lab=makeLabel('Player '+m.id.slice(0,3));lab.position.set(0,5,0);v.group.add(lab);scene.add(v.group);
      o={v:v,type:m.type,x:m.x,y:m.y,z:m.z,h:m.h,tx:m.x,ty:m.y,tz:m.z,th:m.h,last:T};others.set(m.id,o);
    }
    o.tx=m.x;o.ty=m.y;o.tz=m.z;o.th=m.h;o.last=T;
  };
  window.addEventListener('beforeunload',function(){netSend({bye:1});});
}catch(e){bc=null;}
function updateNet(dt){
  netTimer-=dt;if(netTimer<=0){netTimer=0.1;netSendNow();}
  others.forEach(function(o,id){
    if(T-o.last>3){scene.remove(o.v.group);others.delete(id);return;}
    const k=Math.min(1,dt*8);o.x+=(o.tx-o.x)*k;o.y+=(o.ty-o.y)*k;o.z+=(o.tz-o.z)*k;o.h+=angDiff(o.h,o.th)*k;
    o.v.group.position.set(o.x,o.y,o.z);o.v.group.rotation.y=o.h;
    if(o.v.prop)o.v.prop.rotation.z+=dt*30;
  });
  $('players').textContent=1+others.size;
}

/* ================= ADMIN PANEL ================= */
const undoStack=[];
const ZERO=new THREE.Matrix4().makeScale(0,0,0);
function hideLights(b){ // string lights are instanced, so shrink this building's bulbs to nothing
  const saved=[];
  for(let i=0;i<b.ls.length;i++){
    const im=b.ls[i][0]?window.__lightsB:window.__lightsA;if(!im)continue;
    const m=new THREE.Matrix4();im.getMatrixAt(b.ls[i][1],m);saved.push([im,b.ls[i][1],m]);
    im.setMatrixAt(b.ls[i][1],ZERO);im.instanceMatrix.needsUpdate=true;
  }
  return function(){for(let i=0;i<saved.length;i++){saved[i][0].setMatrixAt(saved[i][1],saved[i][2]);saved[i][0].instanceMatrix.needsUpdate=true;}};
}
// every remover returns an "undo" function (or null if there was nothing to remove)
function rmFrom(arr,o,obj3d){const i=arr.indexOf(o);if(i<0)return null;arr.splice(i,1);scene.remove(obj3d);return function(){scene.add(obj3d);arr.splice(Math.min(i,arr.length),0,o);};}
function rmBuilding(b){
  const i=buildings.indexOf(b);if(i<0)return null;
  buildings.splice(i,1);scene.remove(b.mesh);if(b.roof)scene.remove(b.roof);const back=hideLights(b);
  return function(){scene.add(b.mesh);if(b.roof)scene.add(b.roof);buildings.splice(Math.min(i,buildings.length),0,b);back();};
}
function rmTree(t){
  const j=trees.indexOf(t);if(j<0)return null;
  const i=statics.indexOf(t.st);trees.splice(j,1);if(i>=0)statics.splice(i,1);scene.remove(t.g);
  return function(){scene.add(t.g);trees.push(t);statics.push(t.st);};
}
function rmCoin(c){
  if(!c.alive&&c.respawn>1e8)return null;
  c.alive=false;c.g.visible=false;c.respawn=1e9;
  return function(){c.alive=true;c.g.visible=true;c.respawn=0;};
}
function kickPlayer(id){
  netSend({adm:'kick',to:id});
  const o=others.get(id);if(o){scene.remove(o.v.group);others.delete(id);}
  admKey='';toast('Player removed from the game',2000);
}
function removeDesc(d){
  if(!ADM.unlocked)return null;
  switch(d.k){
    case'building':return rmBuilding(d.r);
    case'obstacle':return rmFrom(obstacles,d.r,d.r.mesh);
    case'vehicle':return rmFrom(ais,d.r,d.r.v.group);
    case'person':return rmFrom(people,d.r,d.r.g)||rmFrom(crowd,d.r,d.r.g);
    case'shopkeeper':return rmFrom(keepers,d.r,d.r.h.g);
    case'tree':return rmTree(d.r);
    case'coin':return rmCoin(d.r);
  }
  return null;
}
function pushUndo(f){undoStack.push(f);if(undoStack.length>60)undoStack.shift();}
function massRemove(kinds){
  const lists={vehicle:ais,obstacle:obstacles,person:people,crowd:crowd,coin:coins,tree:trees,building:buildings,shopkeeper:keepers},undos=[];
  kinds.forEach(function(kind){
    lists[kind].slice().forEach(function(o){const u=removeDesc({k:kind==='crowd'?'person':kind,r:o});if(u)undos.push(u);});
  });
  if(undos.length){pushUndo(function(){for(let i=undos.length-1;i>=0;i--)undos[i]();});toast('Removed '+undos.length+' '+kinds[0]+(kinds.length>1?' (and more)':'')+(undos.length>1&&kinds.length===1?'s':''),2400);}
  else toast('Nothing to remove',1600);
  return undos.length;
}
function undoLast(){const f=undoStack.pop();if(f){f();toast('Restored',1500);return true;}toast('Nothing to undo',1500);return false;}

// delete tool: click any object in the 3D world
const ray=new THREE.Raycaster(),ndc=new THREE.Vector2();
function pickObject(ev){
  const rc=renderer.domElement.getBoundingClientRect();
  ndc.set(((ev.clientX-rc.left)/rc.width)*2-1,-((ev.clientY-rc.top)/rc.height)*2+1);
  ray.setFromCamera(ndc,camera);
  const desc=new Map(),objs=[];
  function add(o,d){objs.push(o);desc.set(o,d);}
  buildings.forEach(function(b){if(b.mesh)add(b.mesh,{k:'building',r:b});});
  obstacles.forEach(function(o){add(o.mesh,{k:'obstacle',r:o});});
  ais.forEach(function(a){add(a.v.group,{k:'vehicle',r:a});});
  people.forEach(function(p){add(p.g,{k:'person',r:p});});
  crowd.forEach(function(c){add(c.g,{k:'person',r:c});});
  keepers.forEach(function(k){add(k.h.g,{k:'shopkeeper',r:k});});
  trees.forEach(function(t){add(t.g,{k:'tree',r:t});});
  coins.forEach(function(c){if(c.alive)add(c.g,{k:'coin',r:c});});
  others.forEach(function(o,id){add(o.v.group,{k:'player',id:id});});
  const hits=ray.intersectObjects(objs,true);
  for(let i=0;i<hits.length;i++){let o=hits[i].object;while(o){if(desc.has(o))return desc.get(o);o=o.parent;}}
  return null;
}
renderer.domElement.addEventListener('click',function(ev){
  if(!ADM.unlocked||!ADM.del||shopOpen||!started)return;
  const d=pickObject(ev);
  if(!d){toast('Nothing there',800);return;}
  if(d.k==='player'){kickPlayer(d.id);return;}
  const u=removeDesc(d);
  if(u){pushUndo(u);toast('Removed '+d.k+'  (Undo in the admin panel)',1600);}
});

// money, players and messages between tabs
const amt=function(){return clamp(Math.floor(+$('admAmt').value)||0,-999999999,999999999);};
function giveSelf(n,set){save.coins=clamp(set?n:save.coins+n,0,999999999);persist();updateHUDcoins();sfxCoin();}
function onAdminMsg(m){
  if(m.adm==='coins'&&(m.to==='all'||m.to===myId)){
    const n=m.n|0;giveSelf(n,false);
    toast('The admin '+(n>=0?'gave you ':'took ')+Math.abs(n).toLocaleString()+' coins!',3500);
  }else if(m.adm==='kick'&&m.to===myId){
    netSend({bye:1});kicked=true;$('kicked').style.display='flex';
  }
}
let admKey='';
function renderAdminPlayers(){
  const ids=Array.from(others.keys()),key=ids.join(',');
  if(key===admKey)return;admKey=key;
  const box=$('admPlayers');box.innerHTML='';
  if(!ids.length){box.textContent='No other players online (open the game in another tab to test).';return;}
  ids.forEach(function(id){
    const row=document.createElement('div');row.className='arow aplayer';
    const nm=document.createElement('b');nm.textContent='Player '+id.slice(0,3);row.appendChild(nm);
    const btn=function(txt,cls,fn){const b=document.createElement('button');b.className='ab '+cls;b.textContent=txt;b.addEventListener('click',function(){fn();if(b.blur)b.blur();});row.appendChild(b);};
    btn('Give','',function(){netSend({adm:'coins',to:id,n:amt()});toast('Gave '+amt().toLocaleString()+' coins to Player '+id.slice(0,3),2200);});
    btn('Take','gray',function(){netSend({adm:'coins',to:id,n:-amt()});toast('Took '+amt().toLocaleString()+' coins from Player '+id.slice(0,3),2200);});
    btn('Go to','gray',function(){const o=others.get(id);if(o)teleport(o.x+4,o.z);});
    btn('Kick','red',function(){kickPlayer(id);});
    box.appendChild(row);
  });
}
function teleport(x,z){
  if(WK.on){WK.x=x;WK.z=z;}else{P.x=x;P.z=z;P.speed=0;if(P.type==='plane')P.y=Math.max(P.y,0.95);}
  const A=actor();camH=A.h;shake=0;
}
function repairNow(){P.hp=100;HPS[P.type]=100;refreshDamage();dismissCrowd();}

// panel open/close, password
function showAdminState(){
  $('admLock').style.display=ADM.unlocked?'none':'block';
  $('admBody').style.display=ADM.unlocked?'block':'none';
  $('admbadge').style.display=ADM.unlocked?'block':'none';
  $('admin').style.display=ADM.open?'block':'none';
  if(ADM.unlocked){admKey='';renderAdminPlayers();}
}
function toggleAdmin(){ADM.open=!ADM.open;showAdminState();for(const k in keys)keys[k]=false;}
function unlockAdmin(){
  if($('admPw').value===ADMIN_PASSWORD){
    ADM.unlocked=true;try{sessionStorage.setItem('dcs_admin','1');}catch(e){}
    $('admErr').textContent='';$('admPw').value='';showAdminState();toast('Admin mode unlocked',2000);
  }else $('admErr').textContent='Wrong password';
}
function lockAdmin(){
  ADM.unlocked=false;ADM.god=false;ADM.noclip=false;ADM.speed=1;ADM.del=false;ADM.open=false;
  try{sessionStorage.removeItem('dcs_admin');}catch(e){}
  $('admGod').checked=false;$('admNoclip').checked=false;$('admDel').checked=false;$('admSpeed').value=1;$('admSpeedV').textContent='1';
  renderer.domElement.style.cursor='';showAdminState();
}
(function(){
  const on=function(id,fn){$(id).addEventListener('click',function(){fn();if(this.blur)this.blur();});};
  on('admBtn',toggleAdmin);on('admClose',toggleAdmin);on('admUnlock',unlockAdmin);on('admLockBtn',lockAdmin);
  on('admAdd',function(){giveSelf(amt(),false);toast('You now have '+save.coins.toLocaleString()+' coins',2200);});
  on('admSet',function(){giveSelf(Math.max(0,amt()),true);toast('Coins set to '+save.coins.toLocaleString(),2200);});
  on('admAll',function(){netSend({adm:'coins',to:'all',n:amt()});toast('Sent '+amt().toLocaleString()+' coins to every other player',2400);});
  on('admRepair',function(){repairNow();toast('Vehicle repaired',1500);});
  on('admMax',function(){
    SHOP.forEach(function(it){if(it.str)save.str[it.str]=it.max;else if(!it.repair)save.up[it.id]=it.max;});
    persist();paintGold(P.v);HPS.car=HPS.truck=HPS.scooter=HPS.bike=HPS.plane=100;P.hp=100;refreshDamage();toast('Every upgrade unlocked and all vehicles repaired',2600);
  });
  on('admShow',function(){for(let i=0;i<24;i++)after(i*0.12,function(){const q=aroundPlayer(false);launchRocket(q[0],q[1]);});});
  on('admTpSpawn',function(){teleport(0,-6);});
  on('admTpShop1',function(){if(shopPts[0])teleport(shopPts[0].x,shopPts[0].z);});
  on('admTpShop2',function(){if(shopPts[1])teleport(shopPts[1].x,shopPts[1].z);});
  on('admCollect',function(){let n=0;coins.forEach(function(c){if(c.alive){c.alive=false;c.g.visible=false;c.respawn=25;save.coins+=5;n++;}});persist();updateHUDcoins();sfxCoin();toast('Collected '+n+' coins',1800);});
  on('admRespawn',function(){coins.forEach(function(c){c.alive=true;c.g.visible=true;c.respawn=0;});toast('All coins respawned',1500);});
  on('admCrowd',function(){spawnCrowd(24,false);});
  on('admRmVeh',function(){massRemove(['vehicle']);});
  on('admRmObs',function(){massRemove(['obstacle']);});
  on('admRmPpl',function(){massRemove(['person','crowd']);});
  on('admRmCoin',function(){massRemove(['coin']);});
  on('admRmTree',function(){massRemove(['tree']);});
  on('admRmBld',function(){massRemove(['building']);});
  on('admRmKeep',function(){massRemove(['shopkeeper']);});
  on('admUndo',undoLast);
  $('admGod').addEventListener('change',function(){ADM.god=ADM.unlocked&&this.checked;if(this.blur)this.blur();toast(ADM.god?'God mode ON':'God mode off',1200);});
  $('admNoclip').addEventListener('change',function(){ADM.noclip=ADM.unlocked&&this.checked;if(this.blur)this.blur();toast(ADM.noclip?'No-clip ON':'No-clip off',1200);});
  $('admDel').addEventListener('change',function(){ADM.del=ADM.unlocked&&this.checked;renderer.domElement.style.cursor=ADM.del?'crosshair':'';if(this.blur)this.blur();});
  $('admSpeed').addEventListener('input',function(){ADM.speed=ADM.unlocked?(+this.value||1):1;$('admSpeedV').textContent=String(ADM.speed);});
  $('admSpeed').addEventListener('change',function(){if(this.blur)this.blur();});
  showAdminState();
})();

/* ================= WEAPONS (Rivals-style loadout) =================
   PASTE THIS WHOLE BLOCK just above the line:  /* ================= MAIN LOOP ================= *​/
   Then in frame() change   updateWalker(dt);updateDamageFx(dt);
   to                       updateWalker(dt);updateWeapons(dt);updateDamageFx(dt);
   Controls: Left-click or J = fire, Q = next category, T = next weapon in category, Space = fists double-jump.
   Everything is cartoon / non-lethal: traffic gets disabled for a few seconds, pedestrians get startled and hop,
   obstacles get knocked around. Weapons are bought in the Diwali Shop (saved with your coins). Fists are free. */
const WCAT=['Primary','Secondary','Melee','Utility'];
// [id, category, name, cost, description, kind (h=hitscan p=projectile c=stream m=melee u=special), params]
// params: d damage, r range, c cooldown, a auto-fire, s spread, n pellets, kb knockback, v speed, g gravity, e blast radius, fx effect
const WDEF=[
['ar',0,'Assault Rifle',60,'Reliable full-auto, good everywhere','h',{d:9,r:90,c:.09,s:.025,a:1,kb:1.5}],
['flame',0,'Flamethrower',80,'Close-range fire stream, sets targets burning','c',{d:3,r:15,c:.05,a:1,fx:'fire',col:[1,.5,.1]}],
['perma',0,'Permafrost',80,'Freezing stream, slows and freezes traffic','c',{d:2,r:17,c:.05,a:1,fx:'ice',col:[.5,.9,1]}],
['gl',0,'Grenade Launcher',110,'Arcing grenades with splash damage','p',{d:40,v:34,g:14,vy:7,c:.9,e:8,life:3,kb:14,col:[1,.7,.3]}],
['dist',0,'Distortion',120,'Fires a black hole that drags everything to its centre','p',{d:0,v:40,c:2,life:1.2,fx:'hole',col:[.7,.3,1]}],
['bow',0,'Bow',50,'Arcing arrows','p',{d:32,v:55,g:10,vy:3,c:.7,life:3,kb:6,col:[.9,.8,.6]}],
['xbow',0,'Crossbow',70,'Flat, fast bolts','p',{d:36,v:85,g:2,c:.9,life:2,kb:8,col:[.8,.8,.9]}],
['shotgun',0,'Shotgun',90,'8 pellets, brutal up close','h',{d:6,n:8,r:35,c:.8,s:.12,kb:3}],
['sniper',0,'Sniper',130,'One long-range heavy shot','h',{d:85,r:300,c:1.4,cone:.05,kb:12,col:[1,1,1]}],
['gunblade',0,'Gunblade',100,'Shoots far, slashes automatically when close','h',{d:7,r:80,c:.14,s:.03,a:1,m:{d:28,r:4.5,arc:1.1,kb:6,c:.4}}],
['rpg',0,'RPG',150,'Huge rocket explosion','p',{d:70,v:48,c:1.8,e:11,life:3.5,kb:20,col:[1,.6,.2]}],
['uzi',1,'Uzi',40,'Fast, spraying, great tracking','h',{d:5,r:55,c:.055,s:.07,a:1}],
['handgun',1,'Handgun',30,'Accurate quick-draw','h',{d:13,r:70,c:.28,s:.02}],
['revolver',1,'Revolver',60,'Slow, hard-hitting burst taps','h',{d:38,r:90,c:.75,s:.01,kb:6}],
['warper',1,'Warper',80,'Shoot a portal and teleport to where it lands','p',{d:0,v:60,c:1.2,life:.6,fx:'warp',col:[.5,.6,1]}],
['exo',1,'Exogun',90,'Energy shot with massive knockback','h',{d:18,r:60,c:.5,kb:16,col:[.4,1,.8]}],
['daggers',1,'Daggers',55,'Throws a fan of 3 daggers','p',{d:12,v:60,g:2,c:.6,life:1.2,n:3,fan:.18,col:[.9,.9,1]}],
['shorty',1,'Shorty',45,'Mini shotgun','h',{d:6,n:5,r:20,c:.5,s:.14,kb:3}],
['epist',1,'Energy Pistols',60,'Twin auto energy pistols','h',{d:6,r:60,c:.12,s:.03,a:1,col:[.4,.6,1]}],
['sling',1,'Slingshot',25,'Cheap arcing shots','p',{d:10,v:45,g:12,vy:2,c:.45,life:2.5,col:[.8,.7,.5]}],
['flare',1,'Flare Gun',40,'Sets what it hits on fire','p',{d:5,v:50,g:3,c:1.2,life:2.5,fx:'ignite',col:[1,.3,.2]}],
['spray',1,'Spray',25,'Paints traffic random colours','c',{d:0,r:11,c:.03,a:1,fx:'paint'}],
['knife',2,'Knife',30,'Backstabs on traffic are instant knockouts','m',{d:25,r:3,c:.35,arc:.7,back:1}],
['scythe',2,'Scythe',90,'Each swing dashes you forward','m',{d:30,r:4,c:.7,arc:1.1,dash:9}],
['katana',2,'Katana',100,'Swing deflects nearby projectiles back','m',{d:28,r:4,c:.5,arc:1,kb:5}],
['axe',2,'Battle Axe',70,'Slow, heavy swings','m',{d:45,r:3.6,c:.9,arc:1,kb:10}],
['chainsaw',2,'Chainsaw',120,'Rapid continuous cutting','m',{d:6,r:3.4,c:.08,arc:.9,a:1,kb:1}],
['fists',2,'Fists',0,'Free. Press Space in the air to double jump (on foot)','m',{d:10,r:2.6,c:.3,arc:1}],
['shield',2,'Riot Shield',50,'Big shove with a wide arc','m',{d:2,r:3.5,c:.8,arc:1.3,kb:14}],
['trowel',2,'Trowel',20,'Whacks and drops a lit diya','m',{d:8,r:3,c:.5,arc:.9}],
['spear',2,'Spear',60,'Long reach, narrow thrust','m',{d:28,r:6.5,c:.7,arc:.35,kb:6}],
['maul',2,'Maul',110,'Ground slam damages everything around you','m',{d:35,r:7,c:1.4,aoe:1,kb:16}],
['medkit',3,'Medkit',30,'Repairs 40% of your vehicle','u',{c:1.5}],
['mine',3,'Subspace Tripmine',60,'Place it; blows when something gets close','u',{c:1}],
['warpstone',3,'Warpstone',70,'Teleport about 35 m forward','u',{c:3}],
['flash',3,'Flashbang',45,'Stuns everything nearby','u',{c:4}],
['freeze',3,'Freeze Ray',80,'Beam that freezes the target','h',{d:2,r:60,c:.5,fx:'ice',col:[.5,.9,1]}],
['grenade',3,'Grenade',40,'Bounces, then explodes on a timer','p',{d:45,v:22,g:22,vy:9,c:1.2,e:7,life:1.6,fuse:1,kb:14,col:[.6,1,.4]}],
['jpad',3,'Jump Pad',40,'Place a pad that launches you forward','u',{c:2}],
['molotov',3,'Molotov',45,'Leaves a burning patch of ground','p',{d:0,v:20,g:20,vy:8,c:1.5,life:2,fx:'molo',col:[1,.5,.1]}],
['satchel',3,'Satchel',80,'Place it, use again to detonate','u',{c:.4}],
['smoke',3,'Smoke Grenade',30,'Big smoke cloud','p',{d:0,v:20,g:20,vy:8,c:2,life:2,fx:'smoke',col:[.6,.6,.6]}]
];
WDEF.forEach(function(w){SHOP.push({id:'w_'+w[0],name:WCAT[w[1]]+': '+w[2],desc:w[4],max:1,cost:function(){return w[3];}});});
if(!save.up.w_fists){save.up.w_fists=1;persist();}
const WS={slot:2,idx:[0,0,0,0],cd:0,mouse:false,was:false,projs:[],holes:[],fires:[],mines:[],pads:[],sat:null,hy:0,hv:0,jumps:0,m:null,hudT:0};
function ownedIn(s){return WDEF.filter(function(w){return w[1]===s&&save.up['w_'+w[0]];});}
function cur(){const L=ownedIn(WS.slot);return L.length?L[WS.idx[WS.slot]%L.length]:null;}
const wHud=document.createElement('div');wHud.className='panel';wHud.style.cssText='right:12px;bottom:46px;text-align:right;font-size:13px';document.body.appendChild(wHud);
function announce(){const w=cur();toast(w?WCAT[w[1]]+': '+w[2]+' - '+w[4]:'Nothing owned in this category (buy at the Diwali Shop)',2200);}
window.addEventListener('keydown',function(e){
  if(!started||shopOpen||kicked||e.repeat)return;
  const tg=e.target;if(tg&&tg.tagName==='INPUT')return;
  if(e.code==='KeyQ'){for(let i=1;i<=4;i++){const s=(WS.slot+i)%4;if(ownedIn(s).length){WS.slot=s;break;}}announce();}
  else if(e.code==='KeyT'){const L=ownedIn(WS.slot);if(L.length){WS.idx[WS.slot]=(WS.idx[WS.slot]+1)%L.length;announce();}}
  else if(e.code==='Space'&&WK.on){const w=cur();if(w&&w[0]==='fists'&&WS.jumps<2){WS.hv=8;WS.hy=Math.max(WS.hy,.01);WS.jumps++;}}
});
renderer.domElement.addEventListener('mousedown',function(e){if(e.button===0&&!(ADM.unlocked&&ADM.del))WS.mouse=true;});
window.addEventListener('mouseup',function(){WS.mouse=false;});

/* --- helpers --- */
function tgts(){
  const a=[];
  ais.forEach(function(o){a.push({o:o,k:'v',x:o.x,z:o.z,r:o.r+.6});});
  obstacles.forEach(function(o){a.push({o:o,k:'o',x:o.x,z:o.z,r:o.r+.3});});
  people.forEach(function(o){a.push({o:o,k:'p',x:o.x,z:o.z,r:.7});});
  return a;
}
function lock(range,cone){ // soft auto-aim: nearest target inside a cone in front of you
  const A=actor(),fx=Math.sin(A.h),fz=Math.cos(A.h);let best=null,bd=1e9;
  for(const t of tgts()){const dx=t.x-A.x,dz=t.z-A.z,d=Math.hypot(dx,dz);if(d>range||d<.4)continue;if((dx*fx+dz*fz)/d<Math.cos(cone))continue;if(d<bd){bd=d;best=t;}}
  let dx=fx,dz=fz;if(best){dx=best.x-A.x;dz=best.z-A.z;const l=Math.hypot(dx,dz)||1;dx/=l;dz/=l;}
  return{best:best,dx:dx,dz:dz};
}
function muz(){const A=actor(),r=WK.on?.7:DEFS[P.type].fz;return{x:A.x+Math.sin(A.h)*r,y:A.y+(WK.on?1.4+WS.hy:DEFS[P.type].sy+.6),z:A.z+Math.cos(A.h)*r};}
function spk(x,y,z,c,n,sp){for(let i=0;i<n;i++)spawnP(x,y,z,rnd(-sp,sp),rnd(0,sp),rnd(-sp,sp),c[0],c[1],c[2],rnd(.2,.5),2,8,0,.8);}
function ring(x,z,c){for(let i=0;i<24;i++){const a=i/24*TAU;spawnP(x+Math.cos(a)*2,GY()+.8,z+Math.sin(a)*2,Math.cos(a)*5,3,Math.sin(a)*5,c[0],c[1],c[2],.7,2,0,0,1.2);}}
function sfxW(k){
  if(!AC)return;const t=AC.currentTime;
  if(k===0){const n=noiseSrc(t,.2),f=filt('bandpass',2200,1),g=envGain(t,.25,.01,.15,master);f.frequency.setValueAtTime(2200,t);f.frequency.exponentialRampToValueAtTime(500,t+.15);n.connect(f);f.connect(g);}
  else pataka(t,k===1?.3:(k===2?.55:.8),0,k>=2);
}
function hurt(t,d,nx,nz,kb){
  const o=t.o;
  if(t.k==='v'){o.hp=(o.hp==null?60:o.hp)-d;o.kx+=nx*kb;o.kz+=nz*kb;o.pause=Math.max(o.pause,.5);if(o.hp<=0){o.hp=60;o.dis=6;o.pause=Math.max(o.pause,6);}}
  else if(t.k==='o'){if(kb>0){o.vx+=nx*kb;o.vz+=nz*kb;o.spin=rnd(-8,8);}}
  else{o.cheer=rnd(.7,1.3);o.off=clamp(o.off+(o.horiz?nz:nx)*Math.min(kb,6)*.5,-5,5);} // pedestrians just get startled
}
function status(t,fx,amt){const o=t.o;if(t.k==='v'){if(fx==='fire')o.burn=4;else if(fx==='ice')o.frz=Math.min(4,(o.frz||0)+(amt||1));}else if(t.k==='p')o.cheer=1;}
function boom(x,y,z,rad,dmg,kb,col){
  for(const t of tgts()){const dx=t.x-x,dz=t.z-z,d=Math.hypot(dx,dz);if(d<rad+t.r){const f=1-d/(rad+t.r),l=d||1;hurt(t,dmg*f,dx/l,dz/l,kb*f);}}
  for(let i=0;i<50;i++)spawnP(x,y,z,rnd(-1,1)*rad*2.2,rnd(0,1)*rad*2,rnd(-1,1)*rad*2.2,col[0],col[1],col[2],rnd(.4,.9),2,5,2,1.4);
  spawnP(x,y,z,0,0,0,1,.9,.6,.3,1,0,0,rad*1.4);
  burstLight.position.set(x,y,z);burstLight.color.setRGB(col[0],col[1],col[2]);burstLightI=2.2;
  const A=actor();if(Math.hypot(A.x-x,A.z-z)<40)shake=Math.max(shake,.35);
  sfxW(3);if(AC)sfxBurst(x,y,z);
}
function dash(d){
  if(WK.on){const nx=WK.x+Math.sin(WK.h)*d,nz=WK.z+Math.cos(WK.h)*d;if(!blockedAt(nx,nz,.6)){WK.x=nx;WK.z=nz;}}
  else P.speed=Math.min(P.speed+d*2.5,DEFS[P.type].max*1.5);
}

/* --- firing --- */
function shootRay(p){
  const l=lock(p.r,p.cone||.3),m=muz(),c=p.col||[1,.9,.6],n=p.n||1;
  for(let i=0;i<n;i++){
    const sa=rnd(-1,1)*(p.s||0),cs=Math.cos(sa),sn=Math.sin(sa),ex=l.dx*cs-l.dz*sn,ez=l.dx*sn+l.dz*cs;
    let hit=null,hd=p.r;
    for(const t of tgts()){const tx=t.x-m.x,tz=t.z-m.z,al=tx*ex+tz*ez;if(al<0||al>hd)continue;if(Math.abs(tx*ez-tz*ex)<t.r){hit=t;hd=al;}}
    for(let s=2;s<hd;s+=2)if(blockedAt(m.x+ex*s,m.z+ez*s,0)){hd=s;hit=null;break;}
    const st=Math.max(1.5,hd/12);for(let s=2;s<hd;s+=st)spawnP(m.x+ex*s,m.y,m.z+ez*s,0,0,0,c[0],c[1],c[2],.1,0,0,0,.5);
    spk(m.x+ex*hd,m.y,m.z+ez*hd,c,hit?5:3,4);
    if(hit){hurt(hit,p.d,ex,ez,p.kb||2);if(p.fx)status(hit,p.fx,1.5);}
  }
  shake=Math.max(shake,Math.min(.12,.01+p.d/500));
}
function stream(p){
  const l=lock(p.r,.5),m=muz(),c=p.col||[1,1,1],sp=p.r*1.5;
  for(let i=0;i<(p.fx==='paint'?2:4);i++){
    const cc=p.fx==='paint'?[Math.random(),Math.random(),Math.random()]:c;
    spawnP(m.x,m.y-.2,m.z,(l.dx+rnd(-.15,.15))*sp,rnd(-.5,1.2),(l.dz+rnd(-.15,.15))*sp,cc[0],cc[1],cc[2],.6,0,p.fx==='fire'?-3:0,0,1.1);
  }
  const A=actor();
  for(const t of tgts()){
    const dx=t.x-A.x,dz=t.z-A.z,d=Math.hypot(dx,dz);if(d>p.r+t.r||d<.01)continue;if((dx*l.dx+dz*l.dz)/d<Math.cos(.45))continue;
    hurt(t,p.d,dx/d,dz/d,.5);
    if(p.fx==='paint'){if(t.k==='v'){t.o.v.baseHex=(Math.random()*0xffffff)|0;tintVehicle(t.o.v,0);}}
    else status(t,p.fx,.15);
  }
}
function throwP(p){
  const m=muz(),l=lock(80,.3),n=p.n||1;
  for(let i=0;i<n;i++){
    const sa=(i-(n-1)/2)*(p.fan||0),c=Math.cos(sa),s=Math.sin(sa),dx=l.dx*c-l.dz*s,dz=l.dx*s+l.dz*c;
    WS.projs.push({x:m.x,y:m.y,z:m.z,vx:dx*p.v,vy:p.vy||0,vz:dz*p.v,p:p,life:p.life||3});
  }
  sfxW(p.e?2:1);
}
function melee(id,p){
  const A=actor(),fx=Math.sin(A.h),fz=Math.cos(A.h),m=muz();sfxW(0);
  if(p.dash)dash(p.dash);
  let any=0;
  for(const t of tgts()){
    const dx=t.x-A.x,dz=t.z-A.z,d=Math.hypot(dx,dz);if(d>p.r+t.r||d<.01)continue;
    if(!p.aoe&&(dx*fx+dz*fz)/d<Math.cos(p.arc||1))continue;
    let dm=p.d;if(p.back&&t.k==='v'&&Math.abs(angDiff(t.o.h,A.h))<1.2)dm=999;
    hurt(t,dm,dx/d,dz/d,p.kb||4);any++;spk(t.x,m.y,t.z,[1,.9,.6],5,5);
  }
  if(p.aoe){ring(A.x,A.z,[1,.7,.3]);shake=Math.max(shake,.45);sfxW(3);}
  else for(let k=-4;k<=4;k++){const a=A.h+k*(p.arc||1)/4;spawnP(A.x+Math.sin(a)*p.r*.8,m.y-.3,A.z+Math.cos(a)*p.r*.8,0,0,0,.9,.9,1,.18,0,0,0,.8);}
  if(id==='katana')for(const q of WS.projs)if(Math.hypot(q.x-A.x,q.z-A.z)<7){q.vx*=-1.2;q.vz*=-1.2;spk(q.x,q.y,q.z,[1,1,1],6,5);}
  if(id==='trowel')spawnP(A.x+fx*2,GY()+.4,A.z+fz*2,0,0,0,1,.6,.1,6,0,0,2,1.6);
  if(id==='chainsaw'&&any)spk(A.x+fx*2.5,m.y,A.z+fz*2.5,[1,.8,.4],4,6);
}
function special(id,p){
  const A=actor(),fx=Math.sin(A.h),fz=Math.cos(A.h),m=muz();
  if(id==='medkit'){
    if(P.hp>=100){toast('Vehicle is already at full condition',1500);WS.cd=0;return;}
    P.hp=Math.min(100,P.hp+40);HPS[P.type]=P.hp;refreshDamage();spk(P.x,P.y+2,P.z,[.3,1,.4],20,4);sfxBuy();
  }else if(id==='mine'){WS.mines.push({x:A.x+fx*2.5,z:A.z+fz*2.5,arm:.8,b:0});toast('Tripmine placed',1200);sfxDoor();}
  else if(id==='warpstone'){
    let d=36;while(d>2&&blockedAt(A.x+fx*d,A.z+fz*d,1))d-=2;
    ring(A.x,A.z,[.7,.4,1]);teleport(A.x+fx*d,A.z+fz*d);ring(actor().x,actor().z,[.7,.4,1]);sfxW(1);
  }else if(id==='flash'){
    $('fade').style.opacity='.75';setTimeout(function(){$('fade').style.opacity='0';},120);
    for(const t of tgts())if(Math.hypot(t.x-A.x,t.z-A.z)<28){if(t.k==='v')t.o.pause=Math.max(t.o.pause,3.5);else if(t.k==='p')t.o.cheer=1.5;}
    spawnP(A.x,m.y,A.z,0,0,0,1,1,1,.4,1,0,0,24);burstLight.position.set(A.x,m.y,A.z);burstLight.color.setRGB(1,1,1);burstLightI=3.5;sfxW(3);
  }else if(id==='jpad'){WS.pads.push({x:A.x+fx*3,z:A.z+fz*3,t:40,cd:0,b:0});toast('Jump pad placed',1200);sfxDoor();}
  else if(id==='satchel'){
    if(WS.sat){boom(WS.sat.x,.8,WS.sat.z,10,70,18,[1,.5,.2]);WS.sat=null;WS.cd=.6;}
    else{WS.sat={x:A.x+fx*2,z:A.z+fz*2};toast('Satchel placed. Use it again to detonate.',2000);WS.cd=.4;sfxDoor();}
  }
}
function fire(){
  const w=cur();if(!w||WS.cd>0||shopOpen||talkOpen)return;
  const id=w[0],k=w[5],p=w[6];WS.cd=p.c||.4;
  if(k==='h'){
    if(p.m){const n=lock(p.m.r+1,1.1);if(n.best){melee('gb',p.m);WS.cd=p.m.c;return;}}
    shootRay(p);sfxW(p.d>=30?2:1);
  }else if(k==='p')throwP(p);
  else if(k==='c'){stream(p);if(Math.random()<.2)sfxW(0);}
  else if(k==='m')melee(id,p);
  else special(id,p);
}
function land(q,t){
  const p=q.p,fx=p.fx;
  if(fx==='warp'){ring(P.x,P.z,[.5,.6,1]);teleport(q.x,q.z);ring(q.x,q.z,[.5,.6,1]);return;}
  if(fx==='hole'){WS.holes.push({x:q.x,z:q.z,t:3.5});return;}
  if(fx==='molo'){WS.fires.push({x:q.x,z:q.z,t:5,k:0});spk(q.x,GY()+.5,q.z,[1,.5,.1],14,6);sfxW(1);return;}
  if(fx==='smoke'){for(let i=0;i<60;i++)spawnP(q.x+rnd(-3,3),GY()+rnd(.5,3),q.z+rnd(-3,3),rnd(-1,1),rnd(.2,1),rnd(-1,1),.45,.45,.5,rnd(5,8),.6,0,0,rnd(5,8));return;}
  if(p.e){boom(q.x,q.y,q.z,p.e,p.d,p.kb||10,p.col||[1,.7,.3]);return;}
  if(t){const l=Math.hypot(q.vx,q.vz)||1;hurt(t,p.d,q.vx/l,q.vz/l,p.kb||3);if(fx==='ignite')status(t,'fire');}
  if(fx==='ignite'){burstLight.position.set(q.x,q.y,q.z);burstLight.color.setRGB(1,.3,.2);burstLightI=1.4;}
  spk(q.x,q.y,q.z,p.col||[1,1,1],8,5);
}

/* --- per-frame --- */
function updateWeapons(dt){
  WS.cd-=dt;
  const down=WS.mouse||isK('KeyJ'),w=cur();
  if(started&&!shopOpen&&down&&w&&(!WS.was||w[6].a))fire();
  WS.was=down;
  // projectiles
  for(let i=WS.projs.length-1;i>=0;i--){
    const q=WS.projs[i],p=q.p;q.life-=dt;q.vy-=(p.g||0)*dt;q.x+=q.vx*dt;q.y+=q.vy*dt;q.z+=q.vz*dt;
    const c=p.col||[1,1,1];spawnP(q.x,q.y,q.z,0,0,0,c[0],c[1],c[2],.25,0,0,0,.6);
    let end=q.life<=0,hit=null;
    if(q.y<=GY()+.15){if(p.fuse){q.y=GY()+.15;q.vy=Math.abs(q.vy)*.3;q.vx*=.85;q.vz*=.85;}else end=true;}
    if(!end&&q.y<30&&blockedAt(q.x,q.z,0)){if(p.fuse){q.x-=q.vx*dt;q.z-=q.vz*dt;q.vx*=-.3;q.vz*=-.3;}else end=true;}
    if(!end&&!p.fuse)for(const t of tgts()){if(q.y<4&&Math.hypot(t.x-q.x,t.z-q.z)<t.r+.5){hit=t;end=true;break;}}
    if(end){WS.projs.splice(i,1);land(q,hit);}
  }
  // black holes
  for(let i=WS.holes.length-1;i>=0;i--){
    const h=WS.holes[i];h.t-=dt;
    for(let k=0;k<4;k++){const a=T*6+k*1.57,r=rnd(3,12);spawnP(h.x+Math.cos(a)*r,GY()+1.5+rnd(0,2),h.z+Math.sin(a)*r,-Math.sin(a)*8,0,Math.cos(a)*8,.6,.2,1,.5,1,0,0,1.2);}
    spawnP(h.x,GY()+2,h.z,0,0,0,.5,.1,1,.1,1,0,0,5);
    for(const t of tgts()){
      const dx=h.x-t.x,dz=h.z-t.z,d=Math.hypot(dx,dz);if(d>16||d<.5)continue;
      if(t.k==='v'){t.o.kx+=dx/d*40*dt;t.o.kz+=dz/d*40*dt;t.o.pause=Math.max(t.o.pause,.2);}
      else if(t.k==='o'){t.o.vx+=dx/d*40*dt;t.o.vz+=dz/d*40*dt;}
      else{t.o.off=clamp(t.o.off+(t.o.horiz?dz:dx)/d*4*dt,-5,5);t.o.cheer=.5;}
    }
    if(h.t<=0){WS.holes.splice(i,1);boom(h.x,2,h.z,6,10,6,[.6,.2,1]);}
  }
  // molotov fire patches
  for(let i=WS.fires.length-1;i>=0;i--){
    const f=WS.fires[i];f.t-=dt;f.k-=dt;
    for(let k=0;k<3;k++)spawnP(f.x+rnd(-4,4),GY()+.3,f.z+rnd(-4,4),0,rnd(2,5),0,1,rnd(.3,.6),.1,.7,1,-2,2,1.3);
    if(f.k<=0){f.k=.3;for(const t of tgts())if(Math.hypot(t.x-f.x,t.z-f.z)<5+t.r){hurt(t,4,0,0,0);status(t,'fire');}}
    if(f.t<=0)WS.fires.splice(i,1);
  }
  // tripmines
  for(let i=WS.mines.length-1;i>=0;i--){
    const m=WS.mines[i];m.arm-=dt;m.b-=dt;if(m.b<=0){m.b=.5;spawnP(m.x,GY()+.3,m.z,0,0,0,1,.1,.1,.3,0,0,0,1.2);}
    if(m.arm>0)continue;
    for(const t of tgts())if(Math.hypot(t.x-m.x,t.z-m.z)<4+t.r){WS.mines.splice(i,1);boom(m.x,.6,m.z,9,60,16,[1,.4,.2]);break;}
  }
  if(WS.sat)spawnP(WS.sat.x,GY()+.4,WS.sat.z,0,0,0,1,.8,.1,.15,0,0,0,1);
  // jump pads
  const A=actor();
  for(let i=WS.pads.length-1;i>=0;i--){
    const p=WS.pads[i];p.t-=dt;p.cd-=dt;p.b-=dt;
    if(p.b<=0){p.b=.2;ring(p.x,p.z,[1,.9,.2]);}
    if(p.cd<=0&&Math.hypot(A.x-p.x,A.z-p.z)<3.2){
      p.cd=1;
      if(WK.on){WS.hv=11;WS.hy=Math.max(WS.hy,.01);WS.jumps=1;dash(6);}
      else{P.speed=Math.min(P.speed+22,DEFS[P.type].max*1.7);if(P.type==='plane')P.y+=3;}
      sfxBuy();
    }
    if(p.t<=0)WS.pads.splice(i,1);
  }
  // status effects on traffic
  for(const a of ais){
    if(a.burn>0){a.burn-=dt;a.hp=(a.hp==null?60:a.hp)-6*dt;spawnP(a.x+rnd(-1,1),1.8,a.z+rnd(-1,1),0,rnd(1,3),0,1,rnd(.3,.6),.1,.5,1,-2,2,1.3);if(a.hp<=0){a.hp=60;a.dis=6;a.pause=Math.max(a.pause,6);}}
    if(a.frz>0){a.frz-=dt;a.pause=Math.max(a.pause,.15);if(Math.random()<dt*20)spawnP(a.x+rnd(-1,1),rnd(.5,2),a.z+rnd(-1,1),0,1,0,.5,.9,1,.5,1,0,2,1);}
    if(a.dis>0){a.dis-=dt;if(Math.random()<dt*15)spawnP(a.x+rnd(-.5,.5),1.8,a.z+rnd(-.5,.5),rnd(-.4,.4),rnd(1.5,3),rnd(-.4,.4),.3,.3,.3,1.3,.6,-.4,0,2.6);}
  }
  // double-jump hop (fists), held weapon model, HUD
  if(WS.hv||WS.hy){WS.hv-=24*dt;WS.hy=Math.max(0,WS.hy+WS.hv*dt);if(WS.hy===0&&WS.hv<0){WS.hv=0;WS.jumps=0;}if(WK.on)WK.mesh.position.y+=WS.hy;}
  if(WK.on&&WK.hum){
    if(!WS.m){WS.m=new THREE.Mesh(new THREE.BoxGeometry(.1,.1,1),new THREE.MeshLambertMaterial({color:0x333333}));WK.hum.body.add(WS.m);WS.m.position.set(.38,1.1,.45);}
    WS.m.visible=!!w;if(w){WS.m.material.color.setHex([0x333a44,0x55606e,0xc9ccd4,0xffa21f][w[1]]);WS.m.scale.set(1,1,[1.3,.5,1.5,.4][w[1]]);}
  }else if(WS.m)WS.m.visible=false;
  WS.hudT-=dt;
  if(WS.hudT<=0){
    WS.hudT=.15;
    wHud.innerHTML=w?'<b>'+WCAT[w[1]]+'</b>: '+w[2]+(WS.cd>0?' <span style="opacity:.6">...</span>':' <span style="color:#7dff6b">ready</span>')+'<br><small style="opacity:.75">Click/J fire &middot; Q category &middot; T weapon</small>':'<b>No weapon</b><br><small>Q switches category</small>';
  }
}

/* ================= MAIN LOOP ================= */
let last=0,blinkT=0,hudT=0;
setVehicle('car');updateHUDcoins();
P.x=0;P.z=-6;P.h=0;camH=0;
function frame(ts){
  requestAnimationFrame(frame);
  const dt=Math.min(0.05,Math.max(0.001,(ts-last)/1000||0.016));last=ts;T+=dt;
  contactNow=false;
  updatePlayer(dt);
  contactPrev=contactNow;
  updateWalker(dt);updateWeapons(dt);updateDamageFx(dt);updateCrowd(dt);updateKeepers(dt);
  updateAI(dt);updateObstacles(dt);updatePeople(dt);updateCoins(dt);
  updateFireworks(dt);updateFlashes(dt);updateNet(dt);updateEngine();
  blinkT+=dt;if(blinkT>0.6){blinkT=0;const on=(Math.floor(T/0.6)%2)===0;if(window.__lightsA)window.__lightsA.visible=on;if(window.__lightsB)window.__lightsB.visible=!on;}
  updateCamera(dt);
  hudT-=dt;
  if(hudT<=0){
    hudT=0.1;
    $('spd').textContent=WK.on?'Walking':(P.type==='plane'?'Alt '+Math.round(P.y)+' m · ':'')+Math.round(Math.abs(P.speed)*3.6)+' km/h';
    $('veh').textContent=WK.on?'On foot':DEFS[P.type].name+(P.hp<=0?' (broken)':'');
    $('armor').textContent=save.str[P.type]|0;
    $('cond').textContent=Math.round(P.hp)+'%';$('cond').style.color=P.hp>60?'#7dff6b':(P.hp>30?'#ffd36b':'#ff6b6b');
    let pt='';
    if(started&&!shopOpen){
      if(nearShop())pt='Press E to talk to the shopkeeper';
      else if(WK.on&&Math.hypot(WK.x-P.x,WK.z-P.z)<DEFS[P.type].r+4)pt='Press X to get back in';
      else if(!WK.on&&P.hp<=0)pt='Vehicle broken! Press X to get out, or 1-5 for another vehicle';
    }
    $('prompt').textContent=pt;$('prompt').style.display=pt?'block':'none';
    if(ADM.open&&ADM.unlocked)renderAdminPlayers();
    if(toastUntil>0&&T>toastUntil){toastUntil=0;$('toast').style.display='none';}
  }
  renderer.render(scene,camera);
}
requestAnimationFrame(frame);
window.__dcs={ADM:ADM,admin:{massRemove:massRemove,removeDesc:removeDesc,undo:undoLast,onMsg:onAdminMsg,unlock:unlockAdmin,lock:lockAdmin,toggle:toggleAdmin,teleport:teleport,giveSelf:giveSelf,undoCount:function(){return undoStack.length;},others:others,myId:myId},trees:trees,statics:statics,people:people,keepers:keepers,talk:{list:showList,bye:function(){closeShop(true);},open:function(){return talkOpen;}},shopPts:shopPts,SHOP:SHOP,WK:WK,crowd:crowd,HPS:HPS,toggleOnFoot:toggleOnFoot,P:P,rockets:rockets,ais:ais,buildings:buildings,obstacles:obstacles,coins:coins,explode:explode,launchRocket:launchRocket,registerImpact:registerImpact,save:save};
})();
</script>
</body>
</html>
