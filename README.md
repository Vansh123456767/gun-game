<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>Diwali City Simulator</title>
<style>
html,body{margin:0;height:100%;overflow:hidden;overscroll-behavior:none;-webkit-touch-callout:none;-webkit-tap-highlight-color:transparent;background:#0a0618;font-family:system-ui,"Segoe UI",Arial,sans-serif;color:#fff;user-select:none;-webkit-user-select:none}
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
#cross{position:fixed;left:50%;top:50%;width:0;height:0;pointer-events:none;z-index:4;display:none}
#cross::before,#cross::after{content:'';position:absolute;background:#fff;box-shadow:0 0 3px #000}
#cross::before{width:2px;height:18px;left:-1px;top:-9px}
#cross::after{width:18px;height:2px;left:-9px;top:-1px}
#cross i{position:absolute;left:-2px;top:-2px;width:4px;height:4px;background:#ff3b3b;border-radius:50%;box-shadow:0 0 4px #000;transition:transform .08s}
#cross.aim::before{height:8px;top:-4px}
#cross.aim::after{width:8px;left:-4px}
#cross.hit i{background:#ffe600;transform:scale(3)}
#wpbar{position:fixed;left:50%;bottom:14px;transform:translateX(-50%);display:none;gap:8px;z-index:4}
.wslot{background:rgba(20,10,40,.78);border:2px solid #6e58a8;border-radius:10px;padding:5px 12px;min-width:88px;text-align:center;cursor:pointer;font-size:12px;line-height:1.3}
.wslot.sel{border-color:#ffb020;background:rgba(90,44,10,.88)}
.wslot b{display:block}
.wslot span{color:#ffd36b;font-weight:800;font-size:15px}
#loadout{position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);display:none;z-index:8;background:rgba(22,10,44,.96);border:2px solid #ffb020;border-radius:16px;padding:16px 18px;width:min(560px,94vw);max-height:90vh;overflow:auto;box-shadow:0 10px 40px rgba(0,0,0,.6)}
#loadout h4{margin:0 0 2px;color:#ffd36b;font-size:20px;letter-spacing:2px}
#loadout p{margin:0 0 10px;font-size:12px;opacity:.8}
.lslot{margin-bottom:10px}
.lname{font-size:11px;font-weight:800;letter-spacing:1.5px;color:#b9a6ff;margin-bottom:4px}
.lopts{display:flex;gap:8px;flex-wrap:wrap}
.lopt{flex:1 1 140px;background:rgba(255,255,255,.07);border:2px solid #6e58a8;border-radius:10px;padding:8px 10px;cursor:pointer;font-size:13px}
.lopt b{display:block}.lopt small{display:block;opacity:.75;font-size:11px;margin-top:2px}
.lopt.on{border-color:#5fe08a;background:rgba(40,120,70,.4)}
.lopt.lock{opacity:.45}
#loadout .lclose{width:100%;margin-top:4px;padding:8px;border-radius:9px;border:0;background:#ffb020;color:#2a1100;font-weight:800;cursor:pointer}
#floaters{position:fixed;inset:0;pointer-events:none;z-index:6;overflow:hidden}
.dmg{position:absolute;font-weight:900;font-size:21px;text-shadow:0 2px 0 #000,0 0 6px #000;transform:translate(-50%,-50%);animation:dmgup .9s ease-out forwards;font-family:Impact,"Arial Black",sans-serif}
@keyframes dmgup{0%{opacity:1;margin-top:0}100%{opacity:0;margin-top:-52px}}
#fade{position:fixed;inset:0;background:#fff;opacity:0;pointer-events:none;transition:opacity .8s;z-index:15}
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
#game{touch-action:none}
/* ---- phone / tablet touch controls ---- */
#touch{position:fixed;inset:0;display:none;pointer-events:none;z-index:7;touch-action:none}
body.touch #touch{display:block}
body.touch #help,body.touch #net{display:none!important}
body.touch #hud{font-size:12px;padding:5px 9px;min-width:0;line-height:1.35;left:max(8px,env(safe-area-inset-left));top:max(8px,env(safe-area-inset-top))}
body.touch #start h1{font-size:34px}
body.touch #prompt{bottom:auto;top:12px;font-size:14px;white-space:nowrap}
body.touch #toast{bottom:auto;top:46px}
body.touch .wslot{min-width:62px;padding:3px 8px;font-size:11px}
body.touch #wpbar{bottom:max(8px,env(safe-area-inset-bottom))}
body.touch #loadout{width:min(520px,96vw);padding:10px 12px}
#touch.hide{display:none!important}
#joy{position:absolute;left:max(18px,env(safe-area-inset-left));bottom:max(18px,env(safe-area-inset-bottom));width:132px;height:132px;border-radius:50%;background:rgba(255,255,255,.1);border:2px solid rgba(255,255,255,.35);pointer-events:auto;touch-action:none}
#knob{position:absolute;left:50%;top:50%;width:56px;height:56px;margin:-28px 0 0 -28px;border-radius:50%;background:rgba(255,176,32,.7);border:2px solid #ffe3a0;pointer-events:none}
#tpad{position:absolute;right:max(14px,env(safe-area-inset-right));bottom:max(14px,env(safe-area-inset-bottom));width:190px;height:170px;pointer-events:none}
#tpad .tb{position:absolute;pointer-events:auto}
#tbar{position:absolute;right:max(8px,env(safe-area-inset-right));top:max(8px,env(safe-area-inset-top));display:flex;flex-wrap:wrap;justify-content:flex-end;gap:5px;max-width:min(330px,62vw);pointer-events:none}
.tb{display:flex;flex-direction:column;align-items:center;justify-content:center;box-sizing:border-box;background:rgba(30,14,60,.72);border:2px solid rgba(255,200,100,.55);border-radius:12px;color:#fff;font-weight:800;pointer-events:auto;touch-action:none;text-align:center;line-height:1.05}
.tb small{font-size:9px;font-weight:700;opacity:.85}
.tb.on{background:rgba(255,157,0,.85);color:#2a0a40}
#tbar .tb{width:42px;height:42px;font-size:17px}
#tpad .tb{border-radius:50%;font-size:22px}
.tb.fire{right:0;bottom:36px;width:84px;height:84px;background:rgba(210,40,40,.75);border-color:#ffb0b0}
.tb.aim{right:92px;bottom:96px;width:62px;height:62px}
.tb.jump{right:92px;bottom:20px;width:62px;height:62px}
.tb.boost{right:8px;bottom:128px;width:56px;height:46px;border-radius:12px!important;font-size:15px}
#touch:not(.foot) .f{display:none}
#touch:not(.moon) .m{display:none}
#touch:not(.shop) .s{display:none}
@media (orientation:portrait){body.touch #tbar{max-width:56vw}}
/* ---- match mode (Rivals-style HUD) ---- */
#mhud{position:fixed;inset:0;display:none;pointer-events:none;z-index:5;font-family:Impact,"Arial Black",system-ui,sans-serif}
body.matchon #mhud{display:block}
body.matchon #help,body.matchon #net,body.matchon #hud,body.matchon #wpbar{display:none!important}
#mtop{position:absolute;left:50%;top:8px;transform:translateX(-50%);display:flex;align-items:flex-end;gap:8px;text-align:center}
#mtop .tm{display:flex;flex-direction:column;align-items:center;gap:2px}
#mtop .tm small{font-size:10px;letter-spacing:1.5px;font-family:system-ui,sans-serif;font-weight:800;opacity:.9;text-shadow:0 1px 2px #000}
#mtop .sc{display:block;min-width:44px;height:44px;line-height:44px;border-radius:8px;font-size:28px;color:#fff;border:2px solid rgba(255,255,255,.85);box-shadow:0 2px 6px rgba(0,0,0,.5)}
#mtop .you{background:#d63a3a}#mtop .bot{background:#2f6df0}
#mtop .mid{background:rgba(20,20,26,.82);border:2px solid rgba(255,255,255,.7);border-radius:10px;padding:2px 14px 3px;min-width:96px;flex-direction:row;flex-wrap:wrap;justify-content:center;gap:0 6px}
#mtop .mid b{font-size:24px;letter-spacing:1px}#mtop .mid .hg{font-size:20px;align-self:center}#mtop .mid small{flex-basis:100%;margin-top:-2px}
#mfeed{position:absolute;right:12px;top:84px;display:flex;flex-direction:column;align-items:flex-end;gap:4px}
#mfeed div{background:rgba(15,8,30,.7);border-radius:7px;padding:3px 9px;font-size:13px;font-family:system-ui,sans-serif;font-weight:700;animation:feedin .2s}
#mfeed .me{border:1px solid #7dffa6}#mfeed .me i{color:#7dffa6}#mfeed i{font-style:normal;color:#ffb3b3}
@keyframes feedin{from{opacity:0;transform:translateX(30px)}to{opacity:1;transform:none}}
#mhp{position:absolute;left:14px;bottom:12px;width:min(300px,46vw);font-family:system-ui,sans-serif}
#mhp small{display:block;font-size:13px;font-weight:800;letter-spacing:1px;text-shadow:0 1px 3px #000;margin-bottom:2px}
#mhp .shield{height:6px;background:rgba(0,0,0,.6);border:2px solid #000;border-bottom:0;border-radius:5px 5px 0 0;overflow:hidden}
#mhp .shield i{display:block;height:100%;width:100%;background:#2fd0e6}
#mhp .bar{height:20px;background:rgba(0,0,0,.6);border:2px solid #000;border-radius:0 0 8px 8px;overflow:hidden}
#mhp .bar i{display:block;height:100%;width:100%;background:#22c533;transition:width .12s}
#mhp .bar i.low{background:#e0392b}
#mhp b{position:absolute;right:8px;bottom:2px;font-size:13px;text-shadow:0 1px 2px #000}
#mammo{position:absolute;right:92px;bottom:10px;text-align:right;text-shadow:0 2px 0 #000,0 0 6px #000;font-family:system-ui,sans-serif}
#mammo .amm b{font-size:38px;font-weight:900}#mammo .amm span{font-size:17px;font-weight:800;opacity:.75;margin-left:3px}
#mammo small{display:block;font-size:12px;font-weight:800;letter-spacing:1px;margin-top:-4px}
#mslot{position:absolute;right:12px;bottom:12px;width:70px;height:48px;background:rgba(25,25,30,.85);border:2px solid rgba(255,255,255,.55);border-radius:8px}
#mslot em{position:absolute;left:5px;top:1px;font-style:normal;font-size:11px;font-weight:800;font-family:system-ui,sans-serif;opacity:.85}
#mslotIcon{position:absolute;inset:8px 4px 4px 4px}#mslotIcon svg{width:100%;height:100%}
#mswitch{position:absolute;left:50%;bottom:92px;transform:translateX(-50%);background:#1e63e6;border:2px solid #7fb0ff;border-radius:6px;padding:6px 14px;font-family:system-ui,sans-serif;font-weight:800;font-size:14px;display:none;pointer-events:auto;cursor:pointer;box-shadow:0 2px 8px rgba(0,0,0,.5)}
#mswitch kbd{background:#0f3a96;border-radius:4px;padding:0 6px;margin-left:6px;font-size:12px}
#mcenter{position:absolute;left:0;right:0;top:26%;text-align:center;text-shadow:0 3px 0 #000,0 0 12px #000;font-size:64px;color:#ffd36b}
#mcenter small{display:block;font-size:20px;color:#fff;font-family:system-ui,sans-serif;font-weight:700}
#mhurt{position:absolute;inset:0;background:radial-gradient(circle,transparent 45%,rgba(230,20,20,.75) 100%);opacity:0;transition:opacity .25s}
#mscope{position:absolute;inset:0;display:none;background:radial-gradient(circle at 50% 50%,transparent 0,transparent min(34vh,34vw),#000 calc(min(34vh,34vw) + 2px))}
#mscope::before,#mscope::after{content:'';position:absolute;background:#000}
#mscope::before{left:0;right:0;top:50%;height:2px}#mscope::after{top:0;bottom:0;left:50%;width:2px}
body.touch #mhp{bottom:168px;left:14px}
body.touch #mammo{right:auto;left:50%;transform:translateX(-50%);bottom:8px;text-align:center}
body.touch #mslot{display:none}
body.touch #mswitch{bottom:60px}
body.touch #mfeed{right:auto;left:8px;top:96px;align-items:flex-start}
body.touch #mtop{top:4px;padding:2px 10px}body.touch #mtop .sc{font-size:20px}
body.touch #mcenter{font-size:44px}
.modal{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(8,2,20,.78);z-index:9}
.mbox{background:#241042;border:2px solid #ffb020;border-radius:16px;padding:18px 22px;width:min(440px,92vw);text-align:center;max-height:92vh;overflow:auto}
.mbox h2{margin:0 0 4px;color:#ffd36b;font-size:28px;letter-spacing:2px}
.mbox p{margin:6px 0;font-size:14px;color:#e8dcff}
.mbox .lst{background:rgba(255,255,255,.07);border-radius:10px;padding:8px;margin:10px 0;font-size:13px;line-height:1.6}
.mbox .stat{display:flex;justify-content:space-around;margin:12px 0}.mbox .stat b{display:block;font-size:26px;color:#ffd36b}
.mbox button{margin:4px;padding:10px 20px;border:0;border-radius:10px;font-weight:800;font-size:16px;cursor:pointer;background:#ff9d00;color:#2a0a40}
.mbox button.alt{background:#59467f;color:#fff}
#touch.match .nm{display:none}
#touch:not(.match) .mt{display:none}
</style>
</head>
<body>
<div id="game"></div>
<div id="hud" class="panel"><div id="admbadge" style="display:none;color:#ff6b8a;font-weight:800">ADMIN MODE</div>Coins: <b id="coins">0</b><br>Vehicle: <b id="veh">Car</b><br>Condition: <b id="cond">100%</b> &middot; Armor <b id="armor">0</b>/5<br><span id="spd">0 km/h</span></div>
<div id="net" class="panel">Players here: <b id="players">1</b><br><small style="opacity:.75">(open a 2nd tab to test)</small></div>
<div id="help" class="panel">
<kbd>W A S D</kbd>/arrows drive &nbsp; <kbd>Space</kbd> handbrake (plane: climb) &nbsp; <kbd>Shift</kbd> nitro (plane: descend)<br>
<kbd>1</kbd> Car <kbd>2</kbd> Truck <kbd>3</kbd> Scooter <kbd>4</kbd> Bike <kbd>5</kbd> Plane &nbsp; <kbd>F</kbd> rocket volley &nbsp; <kbd>G</kbd> pataka chain<br>
<kbd>X</kbd> get out / get in &nbsp; <kbd>C</kbd> cheer (on foot) &nbsp; <kbd>E</kbd> talk to the shopkeeper (pink Diwali Shop, one is beside the start) &nbsp; <kbd>V</kbd> camera &nbsp; <kbd>M</kbd> mute &nbsp; <kbd>H</kbd> hide help<br>
Weapons (on foot, buy at the shop): <kbd>Z</kbd> switch &nbsp; <kbd>L</kbd> loadout &nbsp; <kbd>P</kbd> play a match (vs bots) &nbsp; <kbd>Click</kbd>/<kbd>J</kbd> fire &nbsp; hold <kbd>Right-click</kbd>/<kbd>Q</kbd> aim (Scythe: dash)
</div>
<div id="prompt" class="panel">Press E to open the Diwali Shop</div>
<div id="toast"></div>
<div id="pop">BONK!</div>
<div id="fade"></div>
<div id="mhud">
 <div id="mtop">
  <div class="tm"><small>YOU</small><span class="sc you" id="msYou">0</span></div>
  <div class="tm mid"><span class="hg">&#8987;</span><b id="mTime">3:00</b><small id="mGoal">FIRST TO 10</small></div>
  <div class="tm"><small>BOTS</small><span class="sc bot" id="msBot">0</span></div>
 </div>
 <div id="mfeed"></div>
 <div id="mhp"><small id="mName">YOU</small><div class="shield"><i></i></div><div class="bar"><i id="mhpBar"></i></div><b id="mhpTxt">100</b></div>
 <div id="mammo"><div class="amm"><b id="mwAmmo">0</b><span id="mwMag"></span></div><small id="mwName"></small></div>
 <div id="mslot"><em id="mslotN">1</em><div id="mslotIcon"></div></div>
 <div id="mswitch">Switch Loadout <kbd>E</kbd></div>
 <div id="mcenter"></div><div id="mhurt"></div><div id="mscope"></div>
</div>
<div id="lobby" class="modal"><div class="mbox"><h2>DIWALI DUELS</h2><p>Free-for-all vs 3 bots in the arena &middot; first to 10 eliminations, or most kills in 3:00.</p>
 <div class="lst" id="lbList"></div><p style="font-size:12px;opacity:.8">Over-the-shoulder view. Move: W A S D &middot; Look: mouse (click to capture) or drag on a phone &middot; Fire: click / FIRE &middot; Aim: right-click / Q &middot; Switch: Z or 1-4 &middot; Win coins!</p>
 <button id="lbPlay">PLAY</button><button id="lbLoadout" class="alt">LOADOUT</button><button id="lbClose" class="alt">CLOSE</button></div></div>
<div id="results" class="modal"><div class="mbox"><h2 id="rsTitle">VICTORY</h2><p id="rsSub"></p>
 <div class="stat"><div><b id="rsK">0</b>KILLS</div><div><b id="rsD">0</b>DEATHS</div><div><b id="rsC">0</b>COINS</div></div>
 <button id="rsAgain">REMATCH</button><button id="rsLeave" class="alt">LEAVE</button></div></div>
<div id="cross"><i></i></div>
<div id="touch">
 <div id="joy"><i id="knob"></i></div>
 <div id="tbar">
  <div class="tb" data-k="KeyX">&#128694;<small>OUT/IN</small></div>
  <div class="tb nm s" data-k="KeyE">&#128722;<small>SHOP</small></div>
  <div class="tb f" data-k="KeyZ">&#128260;<small>SWITCH</small></div>
  <div class="tb f" data-k="KeyL">&#127890;<small>LOADOUT</small></div>
  <div class="tb m" data-k="KeyR">&#127757;<small>BACK</small></div>
  <div class="tb nm" data-k="Digit1">&#128663;</div><div class="tb nm" data-k="Digit2">&#128667;</div><div class="tb nm" data-k="Digit3">&#128757;</div><div class="tb nm" data-k="Digit4">&#127949;</div><div class="tb nm" data-k="Digit5">&#9992;&#65039;</div>
  <div class="tb nm" data-k="KeyF">&#127878;<small>ROCKETS</small></div>
  <div class="tb nm" data-k="KeyG">&#128165;<small>PATAKA</small></div>
  <div class="tb nm f" data-k="KeyC">&#127881;<small>CHEER</small></div>
  <div class="tb nm" data-k="KeyV">&#127909;<small>CAMERA</small></div>
  <div class="tb" data-k="KeyP">&#9876;&#65039;<small>MATCH</small></div>
  <div class="tb" data-k="KeyM">&#128266;<small>MUTE</small></div>
  <div class="tb nm" data-k="Backquote">&#9881;&#65039;<small>ADMIN</small></div>
  <div class="tb" data-fs="1">&#9974;<small>FULL</small></div>
 </div>
 <div id="tpad">
  <div class="tb fire f" data-k="KeyJ">FIRE</div>
  <div class="tb aim f" data-k="KeyQ">AIM</div>
  <div class="tb jump nm" data-k="Space">&#9650;<small>JUMP</small></div>
  <div class="tb boost" data-k="ShiftLeft">&#9889;</div>
 </div>
</div>
<div id="wpbar"></div>
<div id="loadout"></div>
<div id="floaters"></div>
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
    <div class="arow"><button class="ab" id="admRepair">Repair vehicle</button><button class="ab" id="admMax">Unlock ALL upgrades</button><button class="ab" id="admShow">Mega fireworks</button><button class="ab" id="admWeapons">All weapons</button></div>
    <div class="arow"><button class="ab gray" id="admTpSpawn">Go to spawn</button><button class="ab gray" id="admTpShop1">Spawn shop</button><button class="ab gray" id="admTpShop2">Main shop</button><button class="ab gray" id="admMoon">Moon trip</button></div>
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
<div id="start"><h1>Diwali City Simulator</h1><p>Drive, fly, collect coins and light up the night sky.</p><p>Turn your sound on for the full firework + crash audio.</p><div class="btn">Tap, click or press any key to start</div></div>

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
const save={coins:0,up:{volley:0,auto:0,nitro:0,gold:0,magnet:0},str:{car:0,truck:0,scooter:0,bike:0,plane:0},wp:{rifle:0,shorty:0,scythe:0,nades:0,pistol:0,smg:0,sniper:0,knife:0,rocket:0,glaunch:0,ammo:{rifle:0,shorty:0,pistol:0,smg:0,sniper:0,rocket:0,glaunch:0},slots:{primary:'rifle',secondary:'shorty',melee:'scythe',utility:'grenade'}}};
const MATCH={on:false,state:'off'}; // match mode state (filled in by the MATCH section)
try{const s=JSON.parse(localStorage.getItem('dcs_save_v2')||'null');if(s){save.coins=s.coins|0;Object.assign(save.up,s.up||{});Object.assign(save.str,s.str||{});if(s.wp){Object.assign(save.wp,s.wp);save.wp.ammo=Object.assign({rifle:0,shorty:0,pistol:0,smg:0,sniper:0,rocket:0,glaunch:0},s.wp.ammo||{});save.wp.slots=Object.assign({primary:'rifle',secondary:'shorty',melee:'scythe',utility:'grenade'},s.wp.slots||{});}}}catch(e){}
function persist(){if(MATCH.on)return;try{localStorage.setItem('dcs_save_v2',JSON.stringify(save));}catch(e){}}

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
const PR=Math.min(window.devicePixelRatio||1,(typeof matchMedia!=='undefined'&&matchMedia('(pointer:coarse)').matches)?1.5:2);
const renderer=new THREE.WebGLRenderer({antialias:true});
renderer.setPixelRatio(PR);
renderer.setSize(window.innerWidth,window.innerHeight);
$('game').appendChild(renderer.domElement);
const scene=new THREE.Scene();
scene.background=new THREE.Color(0x0a0618);
scene.fog=new THREE.FogExp2(0x120a2a,0.0055);
const camera=new THREE.PerspectiveCamera(72,window.innerWidth/window.innerHeight,0.5,3000);
const ambL=new THREE.AmbientLight(0x7a68b0,0.95);scene.add(ambL);
const skyObjs=[]; // stars + moon (hidden in the daytime arena)
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
  const stars=new THREE.Points(g,new THREE.PointsMaterial({color:0xffffff,size:2,sizeAttenuation:false,fog:false}));scene.add(stars);skyObjs.push(stars);
  const moon=new THREE.Mesh(new THREE.SphereGeometry(38,16,12),new THREE.MeshBasicMaterial({color:0xfff2c8,fog:false}));
  moon.position.set(-450,380,-700);scene.add(moon);skyObjs.push(moon);
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
    if(p.ko>0){ // knocked out (cartoon-style): lies down with dizzy stars, then gets back up
      p.ko-=dt;const kf=clamp((6.5-p.ko)*5,0,1);
      p.g.rotation.x=-Math.PI/2*kf;p.g.position.set(p.x,0.3+0.25*kf,p.z);poseHuman(p.h,0,0);
      if(Math.random()<dt*3)spawnP(p.x+rnd(-0.4,0.4),2.4,p.z+rnd(-0.4,0.4),0,0.5,0,1,0.9,0.3,0.6,1,0,0,1.4);
      if(p.ko<=0){p.hp=100;p.g.rotation.x=0;}
      continue;
    }
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
function applyVolume(){if(master)master.gain.value=muted?0:(MOON.on?0.2:0.85);} // thin air on the moon: everything is muffled
function sfxLiftoff(){
  if(!AC)return;const t=AC.currentTime;
  const n=noiseSrc(t,2.4),lp=filt('lowpass',300);lp.frequency.setValueAtTime(300,t);lp.frequency.exponentialRampToValueAtTime(1800,t+2);
  const g=AC.createGain();g.gain.setValueAtTime(0.0001,t);g.gain.linearRampToValueAtTime(0.5,t+0.5);g.gain.linearRampToValueAtTime(0.0001,t+2.4);
  n.connect(lp);lp.connect(g);g.connect(master);
  const o=osc('sawtooth',45,140,t,2.2),og=AC.createGain();og.gain.setValueAtTime(0.0001,t);og.gain.linearRampToValueAtTime(0.18,t+0.6);og.gain.linearRampToValueAtTime(0.0001,t+2.3);
  o.connect(og);og.connect(master);o.start(t);o.stop(t+2.4);
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
  if(!AC||!engOsc)return;const t=AC.currentTime,sp=Math.min(Math.abs(P.speed),90),def=DEFS[P.type];
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
    for(let k=0;k<12;k++)spawnP(x,GY()+0.4,z,rnd(-6,6),rnd(5,12),rnd(-6,6),1,0.8,0.35,0.4+Math.random()*0.3,1.5,14,2,0.7);
    spawnP(x,GY()+0.6,z,0,0,0,1,0.9,0.6,0.08,1,0,0,5);
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
let onKeyDown=null,onKeyUp=null;
window.addEventListener('keydown',onKeyDown=function(e){
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
  if(e.code==='KeyP'){matchKey();return;}
  if(LB.open||(MATCH.on&&MATCH.state==='over')){if(LB.open){if(e.code==='Enter')startMatch();else if(e.code==='Escape')closeLobby();}return;}
  if(MATCH.on){
    const dm=/^Digit([1-4])$/.exec(e.code);
    if(dm){if(WK.on){const l=ownedList(),w=l[+dm[1]-1];if(w){WP.cur=w;attachModel();sfxEmpty();}}else{pickVehicle(ORDER[+dm[1]-1]);P.hp=100;HPS[P.type]=100;refreshDamage();}return;} // on foot: weapon slots; driving: switch vehicle
    if(e.code==='KeyE'){toggleLoadout(true);return;}
    if(['KeyF','KeyG','KeyC','KeyV','Digit5'].indexOf(e.code)>=0)return;
  }
  if(e.code==='KeyR'&&MOON.on){returnToEarth();return;}
  if(e.code==='KeyX'){toggleOnFoot();return;}
  if(e.code==='KeyC'&&WK.on){WK.cheerT=2.5;return;}
  if(e.code==='KeyZ'){cycleWeapon();return;}
  if(e.code==='KeyL'){toggleLoadout();return;}
  if(e.code==='Escape'&&LO.open){toggleLoadout(false);return;}
  if(e.code==='KeyQ'&&WK.on&&WP.cur==='scythe'){startDash();return;}
  const m=/^Digit([1-5])$/.exec(e.code);if(m){pickVehicle(ORDER[+m[1]-1]);return;}
  if(e.code==='KeyF')volley(false);
  else if(e.code==='KeyG')patakaChain();
  else if(e.code==='KeyV')camMode=(camMode+1)%3;
  else if(e.code==='KeyM'){muted=!muted;applyVolume();}
  else if(e.code==='KeyH'){const h=$('help');h.style.display=h.style.display==='none'?'block':'none';}
});
window.addEventListener('keyup',onKeyUp=function(e){keys[e.code]=false;});
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
// weapons: held in the hands of the man on foot (get out of the vehicle with X)
(function(){
  const W=save.wp,add=function(o){o.custom=true;SHOP.push(o);};
  add({id:'w_rifle',wid:'rifle',name:'Assault Rifle',done:function(){return'Owned';},info:function(){return W.rifle?'Owned · '+W.ammo.rifle+'/100 bullets':'100 bullets · 6.6 damage per bullet · aim to zoom';},cost:function(){return 150;},can:function(){return !W.rifle;},buy:function(){W.rifle=1;W.ammo.rifle=100;}});
  add({id:'a_rifle',name:'Rifle Ammo',done:function(){return W.rifle?'Full':'Need rifle';},info:function(){return W.ammo.rifle+'/100 bullets';},cost:function(){return 40;},can:function(){return W.rifle>0&&W.ammo.rifle<100;},buy:function(){W.ammo.rifle=100;}});
  add({id:'w_shorty',wid:'shorty',name:'Shorty (shotgun)',done:function(){return'Owned';},info:function(){return W.shorty?'Owned · '+W.ammo.shorty+'/30 bullets':'30 bullets · 6 pellets per shot · deadly up close · aim to zoom';},cost:function(){return 120;},can:function(){return !W.shorty;},buy:function(){W.shorty=1;W.ammo.shorty=30;}});
  add({id:'a_shorty',name:'Shorty Ammo',done:function(){return W.shorty?'Full':'Need Shorty';},info:function(){return W.ammo.shorty+'/30 bullets';},cost:function(){return 35;},can:function(){return W.shorty>0&&W.ammo.shorty<30;},buy:function(){W.ammo.shorty=30;}});
  add({id:'w_scythe',wid:'scythe',name:'Scythe',done:function(){return'Owned';},info:function(){return W.scythe?'Owned':'Melee · 35 damage · right-click / Q to dash';},cost:function(){return 100;},can:function(){return !W.scythe;},buy:function(){W.scythe=1;}});
  add({id:'w_nade',wid:'grenade',name:'Grenades x5',done:function(){return'Max 15';},info:function(){return'You have '+W.nades+' · 75 damage to people nearby';},cost:function(){return 60;},can:function(){return W.nades<15;},buy:function(){W.nades=Math.min(15,W.nades+5);}});
  [ // more weapons: id, name, price, ammo, ammo price, description, ammo noun
    ['pistol','Pistol',80,48,25,'48 bullets · 14 damage · accurate semi-auto','bullets'],
    ['smg','SMG',180,120,45,'120 bullets · 4.2 damage · very fast fire','bullets'],
    ['sniper','Sniper Rifle',260,12,60,'12 bullets · 55 damage · hold aim to scope in','bullets'],
    ['rocket','Rocket Launcher',220,4,70,'4 rockets · 90 damage blast · straight flight','rockets'],
    ['glaunch','Grenade Launcher (RPG)',200,6,50,'6 shells · 60 damage blast · arcing shot','shells']
  ].forEach(function(w){
    const id=w[0],mag=w[3];
    add({id:'w_'+id,wid:id,name:w[1],done:function(){return'Owned';},info:function(){return W[id]?'Owned · '+W.ammo[id]+'/'+mag+' '+w[6]:w[5];},cost:function(){return w[2];},can:function(){return !W[id];},buy:function(){W[id]=1;W.ammo[id]=mag;}});
    add({id:'a_'+id,name:w[1]+' Ammo',done:function(){return W[id]?'Full':'Need '+w[1];},info:function(){return W.ammo[id]+'/'+mag+' '+w[6];},cost:function(){return w[4];},can:function(){return W[id]>0&&W.ammo[id]<mag;},buy:function(){W.ammo[id]=mag;}});
  });
  add({id:'w_knife',wid:'knife',name:'Knife',done:function(){return'Owned';},info:function(){return W.knife?'Owned':'Melee · very fast stab · 22 damage';},cost:function(){return 70;},can:function(){return !W.knife;},buy:function(){W.knife=1;}});
})();
function nearShop(){const A=actor();if(A.y>=4)return false;for(let i=0;i<shopPts.length;i++)if(Math.hypot(A.x-shopPts[i].x,A.z-shopPts[i].z)<16)return true;return false;}
function renderShop(){
  $('shopcoins').textContent=save.coins;const box=$('shoplist');box.innerHTML='';
  SHOP.forEach(function(it){
    if(it.custom){ // weapons and ammo
      const c=it.cost(),can=it.can(),crow=document.createElement('div');crow.className='row';
      crow.innerHTML='<div><b>'+it.name+'</b> <small>'+it.info()+'</small></div>';
      const cb=document.createElement('button');cb.textContent=can?'Buy '+c:it.done();cb.disabled=!can||save.coins<c;
      cb.addEventListener('click',function(){
        if(!it.can()||save.coins<it.cost())return;
        save.coins-=it.cost();it.buy();if(it.wid)autoSlot(it.wid);persist();sfxBuy();updateHUDcoins();renderShop();
        if(it.id.charAt(0)==='w')toast(it.name+' ready! Get out of the vehicle (X) to hold it.',3000);
      });
      crow.appendChild(cb);box.appendChild(crow);return;
    }
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
  if(started&&!shopOpen&&!matchFrozen()){
    thr=(isK('KeyW','ArrowUp')?1:0)-(isK('KeyS','ArrowDown')?1:0);
    steer=(isK('KeyA','ArrowLeft')?1:0)-(isK('KeyD','ArrowRight')?1:0);
    hb=isK('Space');boost=isK('ShiftLeft','ShiftRight');
  }
  const broken=P.hp<=0,dmgF=1-P.hp/100;
  if(broken){thr=0;steer=0;hb=false;boost=false;}
  let max=def.max*(1-0.4*dmgF)*ADM.speed,acc=def.acc*(1-0.3*dmgF)*ADM.speed;
  if(save.up.nitro&&boost&&!plane&&thr>0){max*=1.3;acc*=1.9;}
  if(plane&&SPACE.open&&P.y>140){max*=1.6;acc*=1.5;}
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
  if(plane){
    const gy=GY(),inSpace=SPACE.open&&P.y-gy>140,fly=P.speed>14,lift=clamp((P.speed-14)/10,0,1);
    if(hb&&fly)P.y+=(inSpace?40:14)*lift*dt;
    if(boost&&P.y>0.95+gy)P.y-=(inSpace?40:14)*dt;
    if((P.y>1.5+gy&&!fly)||(broken&&P.y>0.95+gy))P.y-=(broken?18:10)*dt;
    updateCeiling(dt,hb&&fly&&!broken);
    P.y=clamp(P.y,0.95+gy,SPACE.open?100000:gy+150);
    if(SPACE.open&&P.y<100)SPACE.open=false;
    if(P.v.prop)P.v.prop.rotation.z+=dt*(15+Math.min(P.speed,120));
  }else P.y=GY();
  // move in small sub-steps when going very fast (admin 10x / 100x) so nothing is tunnelled through
  const mvDist=Math.abs(P.speed)*dt,nSub=mvDist>2.5?clamp(Math.ceil(mvDist/1.5),1,200):1;
  for(let s=0;s<nSub;s++){
    P.x+=Math.sin(P.h)*P.speed*dt/nSub;P.z+=Math.cos(P.h)*P.speed*dt/nSub;
    if(MOON.on){const d=Math.hypot(P.x,P.z);if(d>MOON.r){P.x*=MOON.r/d;P.z*=MOON.r/d;}}
    else if(MATCH.on){const ah=ARENA.hs-DEFS[P.type].r-0.2;P.x=clamp(P.x,ARENA.x-ah,ARENA.x+ah);P.z=clamp(P.z,ARENA.z-ah,ARENA.z+ah);}
    else if(SPACE.open){P.x=clamp(P.x,-2600,2600);P.z=clamp(P.z,-2600,2600);}
    else{P.x=clamp(P.x,-215,215);P.z=clamp(P.z,-215,215);}
    collidePlayer();
  }
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
function collideMoon(){ // rocks are solid on the moon; everything else is open surface
  const r=DEFS[P.type].r,fx=Math.sin(P.h),fz=Math.cos(P.h),ix=P.x+fx*r*0.8,iy=P.y+1,iz=P.z+fz*r*0.8;
  if(P.type==='plane'&&P.y-MOON.y>6)return;
  for(let i=0;i<MOON.rocks.length;i++){
    const s=MOON.rocks[i],dx=P.x-s.x,dz=P.z-s.z,rr=r+s.r;
    if(Math.abs(dx)>rr||Math.abs(dz)>rr)continue;
    const d=Math.hypot(dx,dz);if(d>=rr)continue;
    const nx=d>1e-4?dx/d:1,nz=d>1e-4?dz/d:0;
    P.x+=nx*(rr-d);P.z+=nz*(rr-d);
    const imp=bounceFrom(nx,nz,0,0,1);
    if(imp>0)registerImpact(imp,ix,iy,iz);else contactNow=true;
  }
}
function collidePlayer(){
  if(ADM.noclip)return;
  if(MOON.on){collideMoon();return;}
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
    const kick=-nx,kickz=-nz,pushV=Math.min(Math.max(imp,1)*0.6,12);
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
    const cl=Math.min(Math.max(closing,2),30);o.vx=-nx*cl*1.1;o.vz=-nz*cl*1.1;o.spin=rnd(-8,8);
    o.x+=-nx*(rr-d)*0.7;o.z+=-nz*(rr-d)*0.7;
    P.speed*=closing>3?0.88:0.97;
    if(closing>0)registerImpact(closing*0.85,ix,iy,iz);else contactNow=true;
  }
}

/* ================= ON FOOT, DAMAGE & CROWD ================= */
const WK={on:false,x:0,y:0,z:0,h:0,sp:0,ph:0,mesh:null,hum:null,up:0,cheerT:0,jy:0,vy:0,dashT:0,dashDir:0};
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
  if(P.type==='plane'&&P.y-GY()>2.5){toast('Land the plane first, then press X',2500);return;}
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
  let stf=0;
  if(started&&!shopOpen&&!matchFrozen()){
    fwd=(isK('KeyW','ArrowUp')?1:0)-(isK('KeyS','ArrowDown')?1:0);
    if(MATCH.on){turn=(isK('ArrowLeft')?1:0)-(isK('ArrowRight')?1:0);stf=(isK('KeyD')?1:0)-(isK('KeyA')?1:0);} // first person: A/D strafe
    else turn=(isK('KeyA','ArrowLeft')?1:0)-(isK('KeyD','ArrowRight')?1:0);
    run=isK('ShiftLeft','ShiftRight');
  }
  WK.h+=turn*2.8*dt;
  const target=(fwd>0?(run?9:5):(fwd<0?-2.5:0))*ADM.speed*(WP.aim?0.45:1); // aiming slows you down
  WK.sp+=(target-WK.sp)*Math.min(1,dt*8);
  const r=0.5,dsh=WK.dashT>0,hdg=dsh?WK.dashDir:WK.h,wsp=dsh?64:WK.sp,sv=(MATCH.on&&!dsh)?stf*4.6*(WP.aim?0.5:1):0,wdx=Math.sin(hdg)*wsp*dt-Math.cos(WK.h)*sv*dt,wdz=Math.cos(hdg)*wsp*dt+Math.sin(WK.h)*sv*dt,wl=Math.hypot(wdx,wdz),wn=wl>1.2?clamp(Math.ceil(wl/0.8),1,200):1;
  function pushCircle(ox,oz,rr){const dx=WK.x-ox,dz=WK.z-oz,d=Math.hypot(dx,dz);if(d>=rr)return null;const nx=d>1e-4?dx/d:1,nz=d>1e-4?dz/d:0;WK.x+=nx*(rr-d);WK.z+=nz*(rr-d);return[nx,nz];}
  for(let s=0;s<wn;s++){ // sub-steps keep a very fast (admin) walker from slipping through walls
    if(MOON.on){
      WK.x+=wdx/wn;WK.z+=wdz/wn;const d=Math.hypot(WK.x,WK.z);if(d>MOON.r){WK.x*=MOON.r/d;WK.z*=MOON.r/d;}
      for(let i=0;i<MOON.rocks.length;i++){const k=MOON.rocks[i];if(Math.abs(WK.x-k.x)<k.r+3&&Math.abs(WK.z-k.z)<k.r+3)pushCircle(k.x,k.z,r+k.r);}
      pushCircle(P.x,P.z,r+DEFS[P.type].r*0.9);
      continue;
    }
    if(MATCH.on){const ah=ARENA.hs-0.7;WK.x=clamp(WK.x+wdx/wn,ARENA.x-ah,ARENA.x+ah);WK.z=clamp(WK.z+wdz/wn,ARENA.z-ah,ARENA.z+ah);}else{WK.x=clamp(WK.x+wdx/wn,-215,215);WK.z=clamp(WK.z+wdz/wn,-215,215);}
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
    for(let i=0;i<statics.length;i++){const s2=statics[i];if(Math.abs(WK.x-s2.x)<3&&Math.abs(WK.z-s2.z)<3)pushCircle(s2.x,s2.z,r+s2.r);}
    pushCircle(P.x,P.z,r+DEFS[P.type].r*0.9);
    for(let i=0;i<ais.length;i++){const a=ais[i];if(pushCircle(a.x,a.z,r+a.r*0.9))a.pause=Math.max(a.pause,0.6);}
    for(let i=0;i<obstacles.length;i++){const o=obstacles[i],n=pushCircle(o.x,o.z,r+o.r);if(n){o.vx=-n[0]*2.5;o.vz=-n[1]*2.5;o.spin=rnd(-4,4);}}
  }
  if(MOON.on){ // low gravity: Space jumps, and you float back down slowly
    if(started&&!shopOpen&&isK('Space')&&WK.jy<0.02)WK.vy=4.2;
    WK.vy-=2.2*dt;WK.jy=Math.max(0,WK.jy+WK.vy*dt);if(WK.jy===0&&WK.vy<0)WK.vy=0;
  }else{WK.jy=0;WK.vy=0;}
  WK.y=GY()+WK.jy;if(WK.dashT>0)WK.dashT-=dt;
  WK.ph+=dt*Math.min(Math.abs(WK.sp),14)*1.8;
  WK.cheerT-=dt;WK.up+=((WK.cheerT>0?1:0)-WK.up)*Math.min(1,dt*9);
  const sw=Math.sin(WK.ph)*0.7*Math.min(1,Math.abs(WK.sp)/3);
  poseHuman(WK.hum,sw,WK.up);
  if(WK.up>0.05){WK.hum.a1.rotation.x=Math.sin(T*9)*0.25*WK.up;WK.hum.a2.rotation.x=-Math.sin(T*9)*0.25*WK.up;}
  WK.mesh.position.set(WK.x,WK.y+0.3+Math.abs(Math.sin(T*8))*0.1*WK.up,WK.z);WK.mesh.rotation.y=WK.h;
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
  if(MOON.on)return;
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
    if(c.ko>0){c.ko-=dt;const kf=clamp((6.5-c.ko)*5,0,1);c.g.rotation.x=-Math.PI/2*kf;c.g.position.set(c.x,0.25*kf,c.z);poseHuman(c.hum,0,0);if(c.ko<=0){c.hp=100;c.g.rotation.x=0;}continue;}
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
  if(MATCH.on&&WK.on){updateCameraFP(dt);return;}
  if(MATCH.on&&$('mscope').style.display!=='none')$('mscope').style.display='none';
  const A=actor(),foot=WK.on;
  camH+=angDiff(camH,A.h)*Math.min(1,dt*3.5);
  const plane=!foot&&P.type==='plane';
  let td=foot?7:(plane?20:14),th=foot?3.4:(plane?8:6),look=foot?1.8:4,lo=foot?6:8;
  if(camMode===1){look=foot?16:(plane?26:24);lo=6;}
  else if(camMode===2){td+=foot?6:12;th+=foot?3:7;look=foot?1.5:3;}
  if(foot&&WP.aim){td=4.1;th=2.7;look=2.2;lo=30;} // aim down the sights: close behind the shoulder, looking far ahead
  const fovT=(foot&&WP.aim)?((WDEF[WP.cur]&&WDEF[WP.cur].zoom)||62):72;
  if(Math.abs(camera.fov-fovT)>0.05){camera.fov+=(fovT-camera.fov)*Math.min(1,dt*10);camera.updateProjectionMatrix();}
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

/* ================= WEAPONS (held in the hands of the man on foot) ================= */
const WDEF={
  rifle:{dmg:6.6,rate:0.09,range:75,spreadHip:0.06,spreadAds:0.008,auto:true,mag:100,zoom:46},          // Assault Rifle: 100 bullets, 6.6 per bullet, full auto
  shorty:{dmg:11,pellets:6,rate:0.95,range:24,spreadHip:0.12,spreadAds:0.07,auto:true,mag:30,zoom:56}, // Shorty: 30 bullets, 6 pellets per shot
  pistol:{dmg:14,rate:0.24,range:60,spreadHip:0.035,spreadAds:0.006,auto:false,mag:48,zoom:58},        // Pistol: 48 bullets, semi-auto
  smg:{dmg:4.2,rate:0.055,range:45,spreadHip:0.09,spreadAds:0.03,auto:true,mag:120,zoom:60},           // SMG: 120 bullets, very fast fire
  sniper:{dmg:55,rate:1.1,range:180,spreadHip:0.18,spreadAds:0,auto:false,mag:12,zoom:20},             // Sniper: 12 bullets, 55 damage, scope
  knife:{dmg:22,range:2.8,rate:0.32},                                                                  // Knife: very fast melee
  rocket:{dmg:90,radius:7,rate:1.5,speed:40,mag:4,zoom:62,range:130},                                  // Rocket Launcher: straight-flying rocket
  glaunch:{dmg:60,radius:5.5,rate:0.9,speed:22,mag:6,zoom:62},                                         // Grenade Launcher (RPG): arcing shell
  scythe:{dmg:35,range:3.7,rate:0.55,dashDmg:25},                             // Scythe: melee swing + dash
  grenade:{dmg:75,radius:6.5,rate:0.8,zoom:62}                                        // Grenade: 75 damage to people in the blast
};
const WNAMES={rifle:'Assault Rifle',shorty:'Shorty',scythe:'Scythe',grenade:'Grenade',pistol:'Pistol',smg:'SMG',sniper:'Sniper',knife:'Knife',rocket:'Rocket Launcher',glaunch:'Grenade Launcher',vehicle:'Vehicle'};
const GUNS=['rifle','shorty','pistol','smg','sniper'],LAUNCHERS=['rocket','glaunch'],MELEE=['scythe','knife'];
function canAim(id){return GUNS.indexOf(id)>=0||LAUNCHERS.indexOf(id)>=0||id==='grenade';}
const WP={cur:null,cd:0,fire:false,aimMouse:false,aim:false,swingT:0,swingHit:false,dashCd:0,dashHit:new Set(),recoil:0,throwT:0,hitFlash:0,dirty:0,model:null,modelFor:null,barKey:'',crossKey:''};
function owns(id){const w=save.wp;return id==='grenade'?w.nades>0:(w[id]|0)>0;}
function ownedAll(){return['rifle','smg','sniper','shorty','pistol','scythe','knife','grenade','rocket','glaunch'].filter(owns);}
function inLoad(id){const q=save.wp.slots;return q.primary===id||q.secondary===id||q.melee===id||q.utility===id;}
function slotOf(id){const q=save.wp.slots;return q.primary===id?'PRIMARY':q.secondary===id?'SECONDARY':q.melee===id?'MELEE':'UTILITY';}
function autoSlot(id){const q=save.wp.slots,sl={rifle:'primary',smg:'primary',sniper:'primary',shorty:'secondary',pistol:'secondary',scythe:'melee',knife:'melee',grenade:'utility',rocket:'utility',glaunch:'utility'}[id];if(!sl)return;if(!q[sl]||!owns(q[sl])||q[sl]===id)q[sl]=id;}
function ownedList(){const q=save.wp.slots;return[q.primary,q.secondary,q.melee,q.utility].filter(function(id,i,a){return id&&owns(id)&&a.indexOf(id)===i;});} // only the weapons chosen in your loadout
function eachTarget(fn){if(MATCH.on){const b=MATCH.bots;for(let i=0;i<b.length;i++)if(b[i].g.visible)fn(b[i]);return;}for(let i=0;i<people.length;i++)fn(people[i]);for(let i=0;i<crowd.length;i++)fn(crowd[i]);}

// --- the weapon in the man's hand (child of his right forearm: barrel runs along the forearm) ---
function gunModel(id){
  const g=new THREE.Group(),dk=hmat(0x24262e),gy=hmat(0x6a6d78),wd=hmat(0x7a4f2a);
  if(id==='rifle'){
    hpart(hbox(0.07,0.5,0.1),dk,0,-0.1,0,g);hpart(hbox(0.03,0.32,0.03),gy,0,-0.5,0.02,g);
    hpart(hbox(0.05,0.1,0.15),dk,0,-0.14,-0.11,g);hpart(hbox(0.02,0.08,0.04),gy,0,-0.2,0.07,g);hpart(hbox(0.05,0.22,0.08),wd,0,0.2,-0.01,g);
  }else if(id==='shorty'){
    hpart(hbox(0.08,0.4,0.1),wd,0,-0.02,0,g);hpart(hbox(0.055,0.42,0.055),gy,0,-0.44,0.015,g);
    hpart(hbox(0.09,0.15,0.07),dk,0,-0.28,-0.05,g);hpart(hbox(0.05,0.2,0.08),wd,0,0.22,-0.01,g);
  }else if(id==='scythe'){
    hpart(hcyl(0.025,0.025,1.7,6),wd,0,0.35,0,g);
    hpart(hbox(0.035,0.08,0.55),gy,0,1.22,0.25,g).rotation.x=-0.15;
    hpart(hbox(0.03,0.07,0.4),gy,0,1.15,0.62,g).rotation.x=-0.7;
    hpart(hbox(0.025,0.06,0.25),hmat(0xd9dce6),0,0.95,0.82,g).rotation.x=-1.1;
  }else if(id==='pistol'){
    hpart(hbox(0.055,0.3,0.075),dk,0,-0.2,0,g);hpart(hbox(0.05,0.15,0.065),dk,0,0.0,-0.035,g).rotation.x=0.25;hpart(hbox(0.02,0.04,0.02),gy,0,-0.1,0.05,g);
  }else if(id==='smg'){
    hpart(hbox(0.075,0.36,0.1),dk,0,-0.12,0,g);hpart(hbox(0.03,0.16,0.03),gy,0,-0.4,0.01,g);
    hpart(hbox(0.045,0.2,0.06),gy,0,-0.1,-0.13,g);hpart(hbox(0.05,0.16,0.07),dk,0,0.14,-0.02,g);hpart(hbox(0.04,0.06,0.04),dk,0,-0.04,0.08,g);
  }else if(id==='sniper'){
    hpart(hbox(0.07,0.6,0.09),dk,0,-0.18,0,g);hpart(hbox(0.028,0.7,0.028),gy,0,-0.85,0.01,g);
    hpart(hcyl(0.045,0.045,0.32,8),hmat(0x15161b),0,-0.28,0.09,g);hpart(hcyl(0.055,0.055,0.04,8),hmat(0x5ac8ff),0,-0.44,0.09,g);
    hpart(hbox(0.055,0.34,0.085),wd,0,0.3,-0.01,g);hpart(hbox(0.02,0.14,0.07),gy,0,-0.5,-0.07,g);
  }else if(id==='knife'){
    hpart(hbox(0.045,0.15,0.055),dk,0,-0.05,0,g);hpart(hbox(0.09,0.03,0.07),gy,0,-0.14,0,g);hpart(hbox(0.02,0.34,0.07),hmat(0xe3e7f2),0,-0.33,0,g);
  }else if(id==='rocket'){
    hpart(hcyl(0.09,0.09,0.95,8),hmat(0x4a5a3a),0,-0.25,0.04,g);hpart(hcyl(0.001,0.075,0.24,8),hmat(0xd0342c),0,-0.84,0.04,g);
    hpart(hcyl(0.11,0.09,0.12,8),hmat(0x2e3a24),0,0.26,0.04,g);hpart(hbox(0.05,0.14,0.07),dk,0,-0.1,-0.12,g);
  }else if(id==='glaunch'){
    hpart(hcyl(0.07,0.07,0.55,8),dk,0,-0.3,0.02,g);hpart(hcyl(0.1,0.1,0.16,8),gy,0,-0.08,0.02,g);hpart(hcyl(0.085,0.085,0.1,8),dk,0,-0.62,0.02,g);
    hpart(hbox(0.05,0.14,0.07),dk,0,-0.04,-0.12,g);hpart(hbox(0.05,0.22,0.07),wd,0,0.2,0,g);
  }else{
    hpart(hsph(0.075,6,5),hmat(0x3c8a3c),0,-0.4,0,g);hpart(hbox(0.03,0.04,0.03),gy,0,-0.34,0.07,g);
  }
  return g;
}
function attachModel(){
  if(!WK.hum)return;
  if(WP.model){WK.hum.f2.remove(WP.model);WP.model=null;}
  WP.modelFor=WP.cur;if(!WP.cur)return;
  WP.model=gunModel(WP.cur);WP.model.position.y=-0.35;WK.hum.f2.add(WP.model); // in his hand
}
function poseWeapon(dt){ // arms wrap around whatever is held (runs after the normal walk pose)
  const h=WK.hum,id=WP.cur;if(!h||!id)return;
  WP.recoil=Math.max(0,WP.recoil-dt*8);
  if(GUNS.indexOf(id)>=0||LAUNCHERS.indexOf(id)>=0){
    const a=WP.aim?1:0;
    h.a2.rotation.set(-(1.25+0.25*a)+WP.recoil*0.25,0,0);h.f2.rotation.x=-0.08;
    h.a1.rotation.set(-(1.15+0.3*a)+WP.recoil*0.2,0,0.32);h.f1.rotation.x=-0.1;
  }else if(id==='scythe'||id==='knife'){
    const s=WP.swingT>0?Math.sin((1-WP.swingT/0.3)*Math.PI):0;
    h.a2.rotation.set(-0.5-s*1.9,0,-0.15+s*0.6);h.f2.rotation.x=-0.4;h.a1.rotation.set(-0.35,0,0.25);
    if(WK.dashT>0){h.a2.rotation.x=-1.6;h.a1.rotation.x=-1.4;}
  }else{
    const s=WP.throwT>0?Math.sin((1-WP.throwT/0.35)*Math.PI):0;
    h.a2.rotation.set(-0.5-s*2.2,0,0);h.f2.rotation.x=-0.6;
  }
}
function cycleWeapon(){
  const l=ownedList();
  if(!l.length){toast('No weapons yet. Buy some at the Diwali Shop (L = loadout).',2200);return;}
  if(!WK.on){toast('Get out of the vehicle (X) to hold a weapon',2000);return;}
  WP.cur=l[(l.indexOf(WP.cur)+1)%l.length];attachModel();sfxEmpty();
}

// --- hits, damage numbers, tracers ---
const _wv=new THREE.Vector3();
function floatText(txt,x,y,z,color){
  _wv.set(x,y,z).project(camera);
  if(_wv.z>1||Math.abs(_wv.x)>1.2||Math.abs(_wv.y)>1.2)return;
  const d=document.createElement('div');d.className='dmg';d.textContent=txt;d.style.color=color||'#fff';
  d.style.left=((_wv.x*0.5+0.5)*window.innerWidth)+'px';d.style.top=((-_wv.y*0.5+0.5)*window.innerHeight)+'px';
  $('floaters').appendChild(d);setTimeout(function(){if(d.parentNode)d.parentNode.removeChild(d);},900);
}
function hurtPerson(p,dmg){ // cartoon-style: a number pops up; at 0 health the person is knocked out for a few seconds, then gets up
  if(p.ko>0)return false;
  if(p.hp===undefined)p.hp=100;
  p.hp=Math.max(0,p.hp-dmg);
  floatText(String(Math.round(dmg*10)/10),p.x,p.g.position.y+2.5,p.z,p.hp<=0?'#ff6b6b':'#ffe066');
  WP.hitFlash=0.15;sfxHit();
  if(p.hp<=0){p.ko=p.isBot?3.2:6.5;p.g.rotation.order='YXZ';floatText(p.isBot?'ELIMINATED':'KO!',p.x,p.g.position.y+3.1,p.z,'#7dff6b');if(p.isBot)matchKill(p);}
  return true;
}
const tracers=[];
(function(){
  const g=new THREE.BoxGeometry(0.05,0.05,1),m=new THREE.MeshBasicMaterial({color:0xfff0a0,transparent:true,opacity:0.9,blending:THREE.AdditiveBlending,depthWrite:false});
  for(let i=0;i<24;i++){const me=new THREE.Mesh(g,m);me.visible=false;me.userData.keep=true;scene.add(me);tracers.push({m:me,life:0});}
})();
function tracer(x0,y0,z0,x1,y1,z1){
  let t=tracers[0];for(let i=1;i<tracers.length;i++)if(tracers[i].life<t.life)t=tracers[i];
  const len=Math.hypot(x1-x0,y1-y0,z1-z0)||0.01;
  t.m.position.set((x0+x1)/2,(y0+y1)/2,(z0+z1)/2);t.m.scale.set(1,1,len);t.m.lookAt(x1,y1,z1);t.m.visible=true;t.life=0.07;
}
function flashAt(x,y,z,size){let f=flashes[0];for(let i=1;i<flashes.length;i++)if(flashes[i].life<f.life)f=flashes[i];f.life=0.35;f.s=size;f.sp.position.set(x,y,z);f.sp.visible=true;}
function wallDist(ox,oz,dx,dz,maxD){ // how far a shot travels before it hits a building
  let best=maxD;
  for(let i=0;i<buildings.length;i++){
    const b=buildings[i];
    if(Math.abs(b.x-ox)>best+b.hw||Math.abs(b.z-oz)>best+b.hd)continue;
    let t0=0,t1=best,ok=true;
    for(let ax=0;ax<2&&ok;ax++){
      const o=ax?oz:ox,d=ax?dz:dx,lo=ax?b.z-b.hd:b.x-b.hw,hi=ax?b.z+b.hd:b.x+b.hw;
      if(Math.abs(d)<1e-6){if(o<lo||o>hi)ok=false;}
      else{let a=(lo-o)/d,c=(hi-o)/d;if(a>c){const q=a;a=c;c=q;}t0=Math.max(t0,a);t1=Math.min(t1,c);if(t0>t1)ok=false;}
    }
    if(ok&&t0<best)best=t0;
  }
  return best;
}
function shootRay(ox,oz,dx,dz,range){
  const wall=wallDist(ox,oz,dx,dz,range);let best=null,bt=wall;
  eachTarget(function(p){
    if(p.ko>0)return;
    const rx=p.x-ox,rz=p.z-oz,t=rx*dx+rz*dz;
    if(t<0.3||t>=bt)return;
    if(Math.abs(rx*dz-rz*dx)<0.55){best=p;bt=t;}
  });
  return{p:best,t:bt};
}
function fireGun(id){
  const d=WDEF[id],shd=matchShoulder(),ox=WK.x+Math.sin(WK.h)*0.75-Math.cos(WK.h)*shd,oz=WK.z+Math.cos(WK.h)*0.75+Math.sin(WK.h)*shd,y=WK.y+1.4;
  const spread=WP.aim?d.spreadAds:d.spreadHip,pellets=d.pellets||1,hits=new Map();
  for(let k=0;k<pellets;k++){
    const a=WK.h+rnd(-spread,spread),dx=Math.sin(a),dz=Math.cos(a),r=shootRay(ox,oz,dx,dz,d.range);
    let dmg=d.dmg;if(pellets>1)dmg=d.dmg*(1-clamp(r.t/d.range,0,1)*0.75); // shotgun pellets weaken with distance
    if(r.p)hits.set(r.p,(hits.get(r.p)||0)+dmg);
    const ex=ox+dx*r.t,ez=oz+dz*r.t;tracer(ox,y,oz,ex,y,ez);
    if(r.p){for(let s=0;s<3;s++)spawnP(ex,y,ez,rnd(-3,3),rnd(0,3),rnd(-3,3),1,0.55,0.25,0.25,3,6,2,0.6);}
    else if(r.t<d.range-0.5){for(let s=0;s<3;s++)spawnP(ex,y,ez,rnd(-2,2),rnd(0.5,2.5),rnd(-2,2),0.7,0.7,0.7,0.5,2,3,0,1.4);}
  }
  hits.forEach(function(dmg,p){hurtPerson(p,dmg);});
  const mx=ox+Math.sin(WK.h)*0.9,mz=oz+Math.cos(WK.h)*0.9;
  for(let s=0;s<4;s++)spawnP(mx,y,mz,Math.sin(WK.h)*7+rnd(-2,2),rnd(-1,1),Math.cos(WK.h)*7+rnd(-2,2),1,0.85,0.4,0.09,3,0,0,1.8);
  WP.recoil=1;shake=Math.max(shake,id==='shorty'?0.14:0.035);sfxShot(id);
}
function tryFire(){
  const id=WP.cur;if(!id)return;
  WP.lastUsed=id;
  if(GUNS.indexOf(id)>=0){
    if(save.wp.ammo[id]<=0){WP.cd=0.3;sfxEmpty();toast('Out of bullets! Buy more at the Diwali Shop.',1800);return;}
    WP.cd=WDEF[id].rate;save.wp.ammo[id]--;WP.dirty=1;fireGun(id);
  }else if(LAUNCHERS.indexOf(id)>=0)launchProj(id);
  else if(MELEE.indexOf(id)>=0)swingScythe();
  else if(id==='grenade')throwGrenade();
}

// --- scythe: swing + dash ---
function swingScythe(){if(WP.cd>0)return;WP.cd=WDEF[WP.cur==='knife'?'knife':'scythe'].rate;WP.swingT=WP.cur==='knife'?0.2:0.3;WP.swingHit=false;sfxSwing();}
function scytheHit(){
  const d=WDEF[WP.cur==='knife'?'knife':'scythe'];let n=0;
  eachTarget(function(p){
    if(p.ko>0)return;
    const dx=p.x-WK.x,dz=p.z-WK.z;
    if(Math.hypot(dx,dz)>d.range||Math.abs(angDiff(WK.h,Math.atan2(dx,dz)))>1.25)return;
    if(hurtPerson(p,d.dmg)){n++;for(let s=0;s<5;s++)spawnP(p.x,WK.y+1.3,p.z,rnd(-4,4),rnd(0,4),rnd(-4,4),0.7,0.85,1,0.3,3,4,2,0.8);}
  });
  for(let i=-6;i<=6;i++){const a=WK.h+i*0.16;spawnP(WK.x+Math.sin(a)*2.4,WK.y+1.2,WK.z+Math.cos(a)*2.4,0,0,0,0.7,0.8,1,0.18,1,0,0,2.4);}
  if(n)shake=Math.max(shake,0.08);
  return n;
}
function startDash(){
  if(!WK.on||WP.cur!=='scythe'||WK.dashT>0||WP.dashCd>0)return;
  WK.dashT=0.2;WK.dashDir=WK.h;WP.dashCd=1.4;WP.dashHit=new Set();sfxDash();shake=Math.max(shake,0.08);
}

// --- grenades ---
const nades=[];
function throwGrenade(){
  if(save.wp.nades<=0){WP.cd=0.4;sfxEmpty();toast('No grenades left. Buy more at the Diwali Shop.',1800);return;}
  WP.cd=WDEF.grenade.rate;WP.throwT=0.35;save.wp.nades--;WP.dirty=1;
  after(0.16,function(){
    const sp=WP.aim?19:13,h=WK.h,m=new THREE.Mesh(hsph(0.12,6,5),hmat(0x3c8a3c));m.userData.keep=true;scene.add(m);
    nades.push({m:m,x:WK.x+Math.sin(h)*0.8-Math.cos(h)*matchShoulder(),y:WK.y+1.6,z:WK.z+Math.cos(h)*0.8+Math.sin(h)*matchShoulder(),vx:Math.sin(h)*sp,vz:Math.cos(h)*sp,vy:WP.aim?8:6.2,t:0,fuse:1.7});
    sfxSwing();
  });
}
function explodeNade(n){
  const R=n.R||WDEF.grenade.radius,DM=n.dmg||WDEF.grenade.dmg;let hit=0;
  eachTarget(function(p){if(p.ko>0)return;if(Math.hypot(p.x-n.x,p.z-n.z)<=R&&hurtPerson(p,DM))hit++;});
  obstacles.forEach(function(o){const dx=o.x-n.x,dz=o.z-n.z,d=Math.hypot(dx,dz);if(d<9&&d>0.01){o.vx=dx/d*12;o.vz=dz/d*12;o.spin=rnd(-8,8);}});
  ais.forEach(function(a){if(Math.hypot(a.x-n.x,a.z-n.z)<8)a.pause=Math.max(a.pause,1.6);});
  for(let i=0;i<70;i++){const u=Math.random()*2-1,a=Math.random()*TAU,s=Math.sqrt(1-u*u),sp=rnd(5,16);spawnP(n.x,n.y+0.3,n.z,s*Math.cos(a)*sp,Math.abs(u)*sp*0.8+2,s*Math.sin(a)*sp,1,rnd(0.45,0.8),0.15,rnd(0.5,0.9),1.8,8,2,rnd(1.4,2.6));}
  for(let i=0;i<24;i++)spawnP(n.x+rnd(-1,1),n.y+0.5,n.z+rnd(-1,1),rnd(-3,3),rnd(1,4),rnd(-3,3),0.3,0.3,0.32,rnd(1.2,2),1.2,-0.5,0,rnd(3,5));
  flashAt(n.x,n.y+0.8,n.z,9);burstLight.position.set(n.x,n.y+1,n.z);burstLight.color.setRGB(1,0.6,0.25);burstLightI=2.6;
  shake=Math.max(shake,clamp(1.1-Math.hypot(n.x-WK.x,n.z-WK.z)*0.04,0.08,0.9));sfxGrenade();
  scene.remove(n.m);
  return hit;
}
// --- rocket launcher + grenade launcher (RPG): projectiles ---
const projs=[];
function launchProj(id){
  const d=WDEF[id];
  if(save.wp.ammo[id]<=0){WP.cd=0.4;sfxEmpty();toast(id==='rocket'?'No rockets left. Buy more at the Diwali Shop.':'No grenade shells left. Buy more at the Diwali Shop.',1800);return;}
  WP.cd=d.rate;save.wp.ammo[id]--;WP.dirty=1;WP.recoil=1.6;
  const h=WK.h,m=new THREE.Mesh(id==='rocket'?hbox(0.12,0.12,0.55):hsph(0.11,6,5),hmat(id==='rocket'?0xd0342c:0x3a3f33));m.userData.keep=true;m.rotation.y=h;scene.add(m);
  const sp=d.speed;
  projs.push({id:id,m:m,x:WK.x+Math.sin(h)*0.9-Math.cos(h)*matchShoulder(),y:WK.y+1.45,z:WK.z+Math.cos(h)*0.9+Math.sin(h)*matchShoulder(),vx:Math.sin(h)*sp,vz:Math.cos(h)*sp,vy:id==='rocket'?0:(WP.aim?7:5),t:0,life:id==='rocket'?d.range/sp:3.2});
  sfxShot(id);shake=Math.max(shake,0.2);
  for(let s=0;s<8;s++)spawnP(WK.x+Math.sin(h)*1.1,WK.y+1.4,WK.z+Math.cos(h)*1.1,rnd(-2,2),rnd(-1,1),rnd(-2,2),0.9,0.8,0.7,0.5,2,0,0,2.2);
}
function updateProjs(dt){
  const g=MOON.on?5:24;
  for(let i=projs.length-1;i>=0;i--){
    const n=projs[i],d=WDEF[n.id];let boom=false;
    if(n.id==='glaunch')n.vy-=g*dt;
    const nx=n.x+n.vx*dt,nz=n.z+n.vz*dt,ny=n.y+n.vy*dt;
    for(let k=0;k<buildings.length;k++){const b=buildings[k];if(ny<b.h&&Math.abs(nx-b.x)<b.hw+0.2&&Math.abs(nz-b.z)<b.hd+0.2){boom=true;break;}}
    if(!boom)eachTarget(function(p){if(!boom&&p.ko<=0&&Math.hypot(p.x-nx,p.z-nz)<0.9&&ny<GY()+2.3)boom=true;});
    if(!boom&&ny<=GY()+0.12)boom=true;
    n.t+=dt;if(n.t>=n.life)boom=true;
    if(!boom){n.x=nx;n.y=ny;n.z=nz;}
    n.m.position.set(n.x,n.y,n.z);
    if(n.id==='rocket'){n.m.rotation.y=Math.atan2(n.vx,n.vz);spawnP(n.x-n.vx*0.02,n.y,n.z-n.vz*0.02,rnd(-0.5,0.5),rnd(-0.3,0.5),rnd(-0.5,0.5),1,0.6,0.2,0.4,2,0,0,1.4);}
    if(boom){explodeNade({x:n.x,y:Math.max(n.y,GY()+0.4),z:n.z,m:n.m,R:d.radius,dmg:d.dmg});projs.splice(i,1);}
  }
}
function updateNades(dt){
  updateProjs(dt);
  const g=MOON.on?5:24;
  for(let i=nades.length-1;i>=0;i--){
    const n=nades[i];n.vy-=g*dt;
    const nx=n.x+n.vx*dt,nz=n.z+n.vz*dt;let hitB=false;
    for(let k=0;k<buildings.length;k++){const b=buildings[k];if(n.y<b.h&&Math.abs(nx-b.x)<b.hw+0.15&&Math.abs(nz-b.z)<b.hd+0.15){hitB=true;break;}}
    if(hitB){n.vx*=-0.35;n.vz*=-0.35;}else{n.x=nx;n.z=nz;}
    n.y+=n.vy*dt;const gnd=GY()+0.12;
    if(n.y<gnd){n.y=gnd;if(n.vy<-2)sfxClink();n.vy*=-0.4;n.vx*=0.72;n.vz*=0.72;if(Math.abs(n.vy)<0.8)n.vy=0;}
    n.t+=dt;n.m.scale.setScalar(1+0.15*Math.sin(n.t*30*(n.t/n.fuse+0.6)));n.m.position.set(n.x,n.y,n.z);
    if(n.t>=n.fuse){explodeNade(n);nades.splice(i,1);}
  }
}

// --- weapon bar, crosshair, per-frame update ---
function refreshWpBar(){
  const bar=$('wpbar'),list=ownedList();
  if(!WK.on||!list.length){bar.style.display='none';return;}
  bar.style.display='flex';
  bar.innerHTML=list.map(function(id){
    const cnt=id==='grenade'?save.wp.nades:(id==='scythe'?'dash':(id==='knife'?'stab':save.wp.ammo[id]));
    return'<div class="wslot'+(id===WP.cur?' sel':'')+'" data-w="'+id+'"><small style="display:block;opacity:.6;font-size:9px;letter-spacing:1px">'+slotOf(id)+'</small><b>'+WNAMES[id]+'</b><span>'+cnt+'</span></div>';
  }).join('');
}
// --- loadout screen (like Rivals): each slot has its own list of options, pick one per slot ---
const LO={open:false};
const LSLOTS=[
  {k:'primary',n:'PRIMARY',opts:['rifle','smg','sniper','shorty']},
  {k:'secondary',n:'SECONDARY',opts:['pistol','shorty','smg']},
  {k:'melee',n:'MELEE',opts:['scythe','knife']},
  {k:'utility',n:'UTILITY',opts:['grenade','rocket','glaunch']}
];
const LINFO={rifle:'100 bullets, 6.6 dmg, full-auto',shorty:'30 bullets, 6 pellets, close range',scythe:'Swing 35 dmg, dash 25 dmg',grenade:'75 dmg blast',pistol:'48 bullets, 14 dmg, semi-auto',smg:'120 bullets, 4.2 dmg, very fast',sniper:'12 bullets, 55 dmg, scope',knife:'Fast stab, 22 dmg',rocket:'4 rockets, 90 dmg blast',glaunch:'6 shells, 60 dmg arcing blast'};
function refreshLoadout(){
  const box=$('loadout');
  if(!LO.open){box.style.display='none';return;}
  box.style.display='block';
  box.innerHTML='<h4>LOADOUT</h4><p>Pick one weapon for each slot. Z cycles through your slots. Buy more at the Diwali Shop.</p>'+
    LSLOTS.map(function(sl){
      const cur=save.wp.slots[sl.k];
      return'<div class="lslot"><div class="lname">'+sl.n+'</div><div class="lopts">'+
        sl.opts.concat(['']).map(function(id){
          if(!id)return'<div class="lopt'+(!cur?' on':'')+'" data-ls="'+sl.k+'" data-lo="">Empty</div>';
          const have=owns(id);
          return'<div class="lopt'+(cur===id?' on':'')+(have?'':' lock')+'" data-ls="'+sl.k+'" data-lo="'+id+'"><b>'+WNAMES[id]+'</b><small>'+(have?LINFO[id]:'Buy at the shop')+'</small></div>';
        }).join('')+'</div></div>';
    }).join('')+'<button class="lclose" data-lc="x">Done (L)</button>';
}
function toggleLoadout(v){
  LO.open=(v===undefined)?!LO.open:!!v;
  if(LO.open&&!started)LO.open=false;
  refreshLoadout();
}
function setSlot(slot,id){
  const q=save.wp.slots;
  if(id&&!owns(id)){toast('You do not own that yet. Buy it at the Diwali Shop.',1800);return false;}
  const old=q[slot];
  if(!id){const rest=['primary','secondary','melee','utility'].filter(function(k){return k!==slot&&q[k]&&owns(q[k]);});if(!rest.length){toast('Keep at least one weapon in your loadout',1600);return false;}}
  q[slot]=id;
  if(id)['primary','secondary'].forEach(function(k){if(k!==slot&&q[k]===id)q[k]=(slot==='primary'||slot==='secondary')?old:'';});
  WP.dirty=1;
  const l=ownedList();if(WP.cur&&l.indexOf(WP.cur)<0){WP.cur=l[0];attachModel();}
  refreshLoadout();return true;
}
$('loadout').addEventListener('click',function(e){
  let t=e.target;while(t&&t!==$('loadout')&&!(t.dataset&&(t.dataset.lo!==undefined||t.dataset.lc)))t=t.parentNode;
  if(!t||!t.dataset)return;
  if(t.dataset.lc){toggleLoadout(false);return;}
  setSlot(t.dataset.ls,t.dataset.lo);
});
$('wpbar').addEventListener('click',function(e){
  let t=e.target;while(t&&!(t.dataset&&t.dataset.w))t=t.parentNode;
  if(t&&owns(t.dataset.w)){WP.cur=t.dataset.w;attachModel();}
});
function updateWeapons(dt){
  WP.cd=Math.max(0,WP.cd-dt);WP.dashCd=Math.max(0,WP.dashCd-dt);WP.hitFlash=Math.max(0,WP.hitFlash-dt);
  for(let i=0;i<tracers.length;i++){const t=tracers[i];if(t.life>0){t.life-=dt;if(t.life<=0)t.m.visible=false;}}
  updateNades(dt);
  const list=ownedList();
  if(WK.on&&list.length&&(!WP.cur||list.indexOf(WP.cur)<0)){WP.cur=list[0];attachModel();}
  if(WK.on&&WK.hum&&WP.cur&&WP.modelFor!==WP.cur)attachModel();
  const active=WK.on&&started&&!shopOpen&&!kicked&&list.length>0&&!!WP.cur&&!matchFrozen();
  WP.aim=active&&canAim(WP.cur)&&(WP.aimMouse||isK('KeyQ'));
  const holdF=WP.fire||isK('KeyJ'),dF=WDEF[WP.cur];
  if(active&&holdF&&WP.cd<=0&&(!dF||dF.auto!==false||!WP.heldF)){WP.heldF=true;tryFire();}
  if(!holdF)WP.heldF=false;
  if(WP.swingT>0){WP.swingT-=dt;if(!WP.swingHit&&WP.swingT<(WP.cur==='knife'?0.1:0.16)){WP.swingHit=true;scytheHit();}}
  if(WP.throwT>0)WP.throwT-=dt;
  if(WK.dashT>0&&WK.on){ // dash trail + dash damage
    for(let k=0;k<3;k++)spawnP(WK.x+rnd(-0.4,0.4),WK.y+rnd(0.3,1.8),WK.z+rnd(-0.4,0.4),0,0.3,0,0.5,0.75,1,0.35,2,0,0,2.2);
    eachTarget(function(p){if(p.ko>0||WP.dashHit.has(p))return;if(Math.hypot(p.x-WK.x,p.z-WK.z)<1.9){WP.dashHit.add(p);hurtPerson(p,WDEF.scythe.dashDmg);}});
  }
  if(WK.on&&WP.cur&&WK.hum)poseWeapon(dt);
  touchUI();
  const bk=String(WK.on)+list.join()+WP.cur+JSON.stringify(save.wp.ammo)+'/'+save.wp.nades;
  if(bk!==WP.barKey){WP.barKey=bk;refreshWpBar();if(LO.open)refreshLoadout();}
  const show=WK.on&&!shopOpen&&list.length>0&&canAim(WP.cur);
  const ck=show?('on'+(WP.aim?' aim':'')+(WP.hitFlash>0?' hit':'')):'off';
  if(ck!==WP.crossKey){WP.crossKey=ck;const c=$('cross');c.style.display=show?'block':'none';c.className=show?((WP.aim?'aim':'')+(WP.hitFlash>0?' hit':'')):'';}
}
// mouse: left = fire, hold right = aim (Scythe: right-click dashes). Q aims too (laptop friendly).
renderer.domElement.addEventListener('mousedown',function(e){
  if(!started||shopOpen||ADM.del||!WK.on||kicked)return;
  if(e.button===0)WP.fire=true;
  else if(e.button===2){WP.aimMouse=true;startDash();}
});
window.addEventListener('mouseup',function(e){if(e.button===0)WP.fire=false;else if(e.button===2)WP.aimMouse=false;});
renderer.domElement.addEventListener('contextmenu',function(e){e.preventDefault();});
window.addEventListener('blur',function(){WP.fire=false;WP.aimMouse=false;});
// --- weapon sounds (Web Audio, no files) ---
function sfxShot(id){
  if(!AC)return;const t=AC.currentTime,big=id==='shorty'||id==='sniper'||id==='rocket'||id==='glaunch';
  const n=noiseSrc(t,0.3),bp=filt('bandpass',big?900:(id==='smg'?2800:2200),0.8),g=envGain(t,big?0.7:(id==='smg'?0.3:0.4),0.001,big?0.2:0.07,master);n.connect(bp);bp.connect(g);
  const o=osc('sine',big?170:230,big?45:70,t,0.2),og=envGain(t,big?0.8:0.45,0.002,big?0.22:0.1,master);o.connect(og);o.start(t);o.stop(t+0.3);
}
function sfxHit(){if(!AC)return;const t=AC.currentTime,o=osc('square',1400,900,t,0.05),g=envGain(t,0.1,0.001,0.05,master);o.connect(g);o.start(t);o.stop(t+0.08);}
function sfxEmpty(){if(!AC)return;const t=AC.currentTime,o=osc('square',190,150,t,0.04),g=envGain(t,0.1,0.001,0.04,master);o.connect(g);o.start(t);o.stop(t+0.07);}
function sfxClink(){if(!AC)return;const t=AC.currentTime,o=osc('triangle',2600,2000,t,0.08),g=envGain(t,0.12,0.001,0.08,master);o.connect(g);o.start(t);o.stop(t+0.1);}
function sfxSwing(){
  if(!AC)return;const t=AC.currentTime,n=noiseSrc(t,0.3),bp=filt('bandpass',500,1.2);
  bp.frequency.setValueAtTime(500,t);bp.frequency.exponentialRampToValueAtTime(2600,t+0.25);
  const g=AC.createGain();g.gain.setValueAtTime(0.0001,t);g.gain.linearRampToValueAtTime(0.3,t+0.1);g.gain.linearRampToValueAtTime(0.0001,t+0.28);n.connect(bp);bp.connect(g);g.connect(master);
}
function sfxDash(){
  if(!AC)return;const t=AC.currentTime,n=noiseSrc(t,0.3),hp=filt('highpass',1500),g=envGain(t,0.3,0.02,0.25,master);n.connect(hp);hp.connect(g);
  const o=osc('sine',200,900,t,0.2),og=envGain(t,0.12,0.01,0.2,master);o.connect(og);o.start(t);o.stop(t+0.25);
}
function sfxGrenade(){
  if(!AC)return;const t=AC.currentTime;
  const o=osc('sine',150,30,t,0.7),og=envGain(t,1,0.003,0.7,master);o.connect(og);o.start(t);o.stop(t+0.8);
  const n=noiseSrc(t,0.6),lp=filt('lowpass',900),ng=envGain(t,0.8,0.003,0.5,master);n.connect(lp);lp.connect(ng);
  const c=noiseSrc(t+0.05,0.3),ch=filt('highpass',1800),cg=envGain(t+0.05,0.35,0.002,0.2,master);c.connect(ch);ch.connect(cg);
}

/* ================= MATCH MODE: a Rivals-style duel in a walled arena vs 3 bots, first person ================= */
const ARENA={x:1300,z:1300,hs:42,built:false,boxes:[],spawns:[]};
const LB={open:false};
Object.assign(MATCH,{bots:[],hp:100,kills:0,deaths:0,time:180,target:10,t:0,pitch:0,lastHurt:-9,inv:0,snap:null,prev:null,sfxT:0,hurtFlash:0,leaveArm:0,vm:null,vmFor:null,hudKey:'',cnt:-1,lastBy:'',winner:'',reward:0,lookId:-1});
const BOTNAMES=['Arjun','Priya','Rohan','Meera','Kabir','Diya'];
function matchFrozen(){return MATCH.on&&MATCH.state!=='live';}
function applyDay(on){ // the arena is a bright daytime map
  ambL.color.setHex(on?0xffffff:0x7a68b0);ambL.intensity=on?1.25:0.95;
  moonLight.color.setHex(on?0xfff6e0:0xaab4ff);moonLight.intensity=on?0.95:0.65;moonLight.position.set(on?60:-80,120,on?-40:60);
  skyObjs.forEach(function(o){o.visible=!on;});
}
function buildArena(){
  if(ARENA.built)return;ARENA.built=true;
  const ax=ARENA.x,az=ARENA.z,hs=ARENA.hs;
  const fl=new THREE.Mesh(new THREE.PlaneGeometry(hs*2,hs*2),new THREE.MeshLambertMaterial({color:0x9c9ca0}));fl.rotation.x=-Math.PI/2;fl.position.set(ax,0.03,az);scene.add(fl);
  // asphalt cross-roads + paving
  const rm=new THREE.MeshLambertMaterial({color:0x3b3b42});
  [[0,0,hs*2,9],[0,0,9,hs*2]].forEach(function(r){const m=new THREE.Mesh(new THREE.PlaneGeometry(r[2],r[3]),rm);m.rotation.x=-Math.PI/2;m.position.set(ax,0.05,az);scene.add(m);});
  [[0,0,hs*2,0.5],[0,0,0.5,hs*2]].forEach(function(r){const m=new THREE.Mesh(new THREE.PlaneGeometry(r[2],r[3]),new THREE.MeshBasicMaterial({color:0xf2e6a0}));m.rotation.x=-Math.PI/2;m.position.set(ax,0.07,az);scene.add(m);});
  function reg(x,z,w,d,h,m){const bd={x:ax+x,z:az+z,hw:w/2,hd:d/2,h:h,mesh:m,roof:null,ls:[]};buildings.push(bd);ARENA.boxes.push(bd);return bd;}
  function ab(x,z,w,d,h,col){const m=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),new THREE.MeshLambertMaterial({color:col,flatShading:true}));m.position.set(ax+x,h/2,az+z);scene.add(m);reg(x,z,w,d,h,m);}
  // window textures for real-looking buildings
  function winTex(base,floors){
    const c=document.createElement('canvas');c.width=128;c.height=128;const g=c.getContext('2d');
    g.fillStyle=base;g.fillRect(0,0,128,128);
    g.fillStyle='rgba(0,0,0,.08)';g.fillRect(0,0,128,6);g.fillRect(0,122,128,6);
    for(let fy=0;fy<2;fy++)for(let fx=0;fx<2;fx++){
      const x=14+fx*60,y=14+fy*60;
      g.fillStyle='#5b3a24';g.fillRect(x-3,y-3,42,46);                       // frame
      g.fillStyle=(fx+fy)%2?'#ffd98a':'#8fc4e8';g.fillRect(x,y,36,40);        // lit / sky glass
      g.fillStyle='#5b3a24';g.fillRect(x+16,y,3,40);g.fillRect(x,y+18,36,3);   // mullions
      g.fillStyle='rgba(0,0,0,.18)';g.fillRect(x-3,y+43,42,5);                 // sill
    }
    const t=new THREE.CanvasTexture(c);t.wrapS=t.wrapT=THREE.RepeatWrapping;return t;
  }
  const doorM=new THREE.MeshLambertMaterial({color:0x4a2a1a}),roofC=[0xa8452c,0x7d3b2a,0x3f5a8a,0x5a7d3b];
  function house(x,z,w,d,h,col,k){ // a proper building: windows, roof edge, door, rooftop tank, diya lights
    const fl=Math.max(1,Math.round(h/3.2)),cs='#'+('000000'+col.toString(16)).slice(-6);
    const tx=winTex(cs),tz=winTex(cs);tx.repeat.set(Math.max(1,Math.round(d/3.2)),fl);tz.repeat.set(Math.max(1,Math.round(w/3.2)),fl);
    const roof=new THREE.MeshLambertMaterial({color:0x6b6b72}),mats=[new THREE.MeshLambertMaterial({map:tx}),new THREE.MeshLambertMaterial({map:tx}),roof,roof,new THREE.MeshLambertMaterial({map:tz}),new THREE.MeshLambertMaterial({map:tz})];
    const grp=new THREE.Group(),body=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),mats);body.position.y=h/2;grp.add(body);
    const par=new THREE.MeshLambertMaterial({color:roofC[k%4]});
    [[0,d/2-0.15,w,0.3],[0,-d/2+0.15,w,0.3]].forEach(function(q){const e=new THREE.Mesh(new THREE.BoxGeometry(q[2],0.7,q[3]),par);e.position.set(q[0],h+0.35,q[1]);grp.add(e);});
    [[w/2-0.15,0],[-w/2+0.15,0]].forEach(function(q){const e=new THREE.Mesh(new THREE.BoxGeometry(0.3,0.7,d),par);e.position.set(q[0],h+0.35,q[1]);grp.add(e);});
    const tank=new THREE.Mesh(new THREE.CylinderGeometry(0.9,0.9,1.4,10),new THREE.MeshLambertMaterial({color:0x2b6cc4}));tank.position.set(w*0.2,h+1.4,-d*0.15);grp.add(tank);
    const door=new THREE.Mesh(new THREE.BoxGeometry(1.6,2.4,0.15),doorM);door.position.set(0,1.2,-d/2-0.05);grp.add(door);
    const awn=new THREE.Mesh(new THREE.BoxGeometry(3,0.15,1.2),new THREE.MeshLambertMaterial({color:[0xe8473a,0xf2a62a,0x2aa66a,0xd63a9a][k%4]}));awn.position.set(0,2.7,-d/2-0.6);grp.add(awn);
    const lm=new THREE.MeshBasicMaterial({color:0xffb84a,fog:false});
    for(let q=-w/2+0.8;q<=w/2-0.4;q+=1.6){const l=new THREE.Mesh(new THREE.BoxGeometry(0.22,0.22,0.22),lm);l.position.set(q,h+0.9,d/2+0.12);grp.add(l);}
    grp.position.set(ax+x,0,az+z);scene.add(grp);reg(x,z,w,d,h,grp);
  }
  // chain-link fence on all four sides
  const c=document.createElement('canvas');c.width=c.height=64;const g=c.getContext('2d');g.strokeStyle='#7f858d';g.lineWidth=3;
  g.beginPath();g.moveTo(0,0);g.lineTo(64,64);g.moveTo(64,0);g.lineTo(0,64);g.moveTo(-32,32);g.lineTo(32,96);g.moveTo(32,-32);g.lineTo(96,32);g.moveTo(32,96);g.lineTo(96,32);g.moveTo(-32,32);g.lineTo(32,-32);g.stroke();
  const ft=new THREE.CanvasTexture(c);ft.wrapS=ft.wrapT=THREE.RepeatWrapping;
  function fence(x,z,w,d,h){
    const long=Math.max(w,d),grp=new THREE.Group(),tx=ft.clone();tx.needsUpdate=true;tx.repeat.set(long/3,h/3);
    const pl=new THREE.Mesh(new THREE.PlaneGeometry(long,h),new THREE.MeshBasicMaterial({map:tx,transparent:true,alphaTest:0.3,side:THREE.DoubleSide}));pl.position.y=h/2;grp.add(pl);
    const pm=new THREE.MeshLambertMaterial({color:0x6d727a}),rail=new THREE.Mesh(new THREE.BoxGeometry(long,0.18,0.18),pm);rail.position.y=h;grp.add(rail);
    for(let q=-long/2;q<=long/2+0.1;q+=long/6){const po=new THREE.Mesh(new THREE.BoxGeometry(0.22,h,0.22),pm);po.position.set(q,h/2,0);grp.add(po);}
    if(d>w)grp.rotation.y=Math.PI/2;grp.position.set(ax+x,0,az+z);scene.add(grp);reg(x,z,w,d,h,grp);
  }
  fence(0,-hs-1,hs*2+4,2,7);fence(0,hs+1,hs*2+4,2,7);fence(-hs-1,0,2,hs*2+4,7);fence(hs+1,0,2,hs*2+4,7);
  // buildings around the cross-roads (roads run along the x and z axes through the middle)
  house(-17,-17,14,14,9,0xf3d9a4,0);house(17,-17,14,14,7,0xf0a8b8,1);house(-17,17,14,14,7,0x9fd4c6,2);house(17,17,14,14,9,0xf5c46b,3);
  house(-33,-33,10,10,6,0xd9b08c,1);house(33,-33,10,10,6,0xb8c8e8,2);house(-33,33,10,10,6,0xe8d28a,3);house(33,33,10,10,6,0xc9e0b0,0);
  house(-32,0,8,12,5,0xf0b98a,2);house(32,0,8,12,5,0xa8d0e0,1);house(0,-32,12,8,5,0xe8a8a0,3);house(0,32,12,8,5,0xd8d8a0,0);
  // crates / market stalls on the streets for cover
  [[8,8],[-8,-8],[8,-8],[-8,8],[22,0],[-22,0],[0,22],[0,-22]].forEach(function(q,k){ab(q[0],q[1],2.4,2.4,2.4,k%2?0xc9962a:0xd9a82a);});
  ARENA.spawns=[[-39,-26],[39,-26],[-39,26],[39,26],[-26,-39],[26,39],[-26,39],[26,-39]];
}
function arenaBlocked(x,z,pad){for(let i=0;i<ARENA.boxes.length;i++){const b=ARENA.boxes[i];if(Math.abs(x-b.x)<b.hw+pad&&Math.abs(z-b.z)<b.hd+pad)return true;}return false;}
function pushBoxes(o,r){
  for(let i=0;i<ARENA.boxes.length;i++){
    const b=ARENA.boxes[i];if(Math.abs(o.x-b.x)>b.hw+r+1||Math.abs(o.z-b.z)>b.hd+r+1)continue;
    const cx=clamp(o.x,b.x-b.hw,b.x+b.hw),cz=clamp(o.z,b.z-b.hd,b.z+b.hd),dx=o.x-cx,dz=o.z-cz,d=Math.hypot(dx,dz);
    if(d>=r)continue;
    if(d<1e-4){const px=b.hw-Math.abs(o.x-b.x),pz=b.hd-Math.abs(o.z-b.z);if(px<pz)o.x+=(Math.sign(o.x-b.x)||1)*(px+r);else o.z+=(Math.sign(o.z-b.z)||1)*(pz+r);}
    else{o.x+=dx/d*(r-d);o.z+=dz/d*(r-d);}
  }
  const ah=ARENA.hs-0.7;o.x=clamp(o.x,ARENA.x-ah,ARENA.x+ah);o.z=clamp(o.z,ARENA.z-ah,ARENA.z+ah);
}
function makeBots(){
  if(MATCH.bots.length)return;
  const cols=[0xd6342b,0xe0872a,0xb02f8f];
  for(let i=0;i<3;i++){
    const h=buildHuman(cols[i],0x23232a,SKINS[(i*2)%5],HAIRS[i%4]);
    const m=gunModel('rifle');m.position.y=-0.35;h.f2.add(m);h.g.visible=false;scene.add(h.g);h.g.rotation.order='YXZ';
    MATCH.bots.push({g:h.g,hum:h,isBot:true,name:BOTNAMES[i],x:0,z:0,h:0,hp:100,ko:0,ph:0,sp:0,cd:1,sdir:1,sT:0,dest:null,destT:0,seeT:0,kills:0,hurtT:0});
  }
}
function spawnPoint(avoidBots){ // spawn spot farthest from everyone else
  let best=null,bd=-1;
  ARENA.spawns.forEach(function(q){
    const x=ARENA.x+q[0],z=ARENA.z+q[1];let md=1e9;
    if(avoidBots)MATCH.bots.forEach(function(b){if(b.g.visible&&b.ko<=0)md=Math.min(md,Math.hypot(b.x-x,b.z-z));});
    else md=Math.hypot((WK.on?WK.x:P.x)-x,(WK.on?WK.z:P.z)-z);
    md+=Math.random()*10;if(md>bd){bd=md;best=[x,z];}
  });
  return best;
}
function placeBot(b){const q=spawnPoint(false);b.x=q[0];b.z=q[1];b.hp=100;b.ko=0;b.g.visible=true;b.g.rotation.set(0,0,0);b.dest=null;b.seeT=0;b.cd=rnd(0.8,1.6);b.h=Math.atan2(ARENA.x-b.x,ARENA.z-b.z);}
function refillAmmo(){
  const w=save.wp;GUNS.concat(LAUNCHERS).forEach(function(id){if(owns(id))w.ammo[id]=WDEF[id].mag;});if(w.nades>0)w.nades=Math.max(w.nades,5);
}
function placeVehicle(){ // your vehicle waits in the arena next to your spawn
  if(P.type==='plane')setVehicle('car');
  const dx=ARENA.x-WK.x,dz=ARENA.z-WK.z,d=Math.hypot(dx,dz)||1,fx=dx/d,fz=dz/d;
  let x=WK.x+fx*5+(-fz)*2,z=WK.z+fz*5+fx*2;
  for(let k=0;k<12&&arenaBlocked(x,z,DEFS[P.type].r+1);k++){x=WK.x+fx*(5+k)+(-fz)*(2+k*0.5);z=WK.z+fz*(5+k)+fx*(2+k*0.5);}
  P.x=x;P.z=z;P.h=Math.atan2(fx,fz);P.speed=0;P.y=GY();P.hp=100;HPS[P.type]=100;refreshDamage();
}
function openLobby(){
  if(!started||MATCH.on||LB.open)return;
  if(MOON.on||SPACE.busy){toast('Match is only available on Earth',2000);return;}
  if(!WK.on&&P.type==='plane'&&P.y-GY()>2.5){toast('Land the plane first',2000);return;}
  const l=ownedList();
  $('lbList').innerHTML=l.length?'<b>Your loadout</b><br>'+l.map(function(id){return slotOf(id)+': '+WNAMES[id];}).join('<br>'):'<b>You own no weapons yet</b><br>You will get a free Pistol and Knife for the match. Buy more at the Diwali Shop!';
  for(const k in keys)keys[k]=false;LB.open=true;$('lobby').style.display='flex';
}
function closeLobby(){LB.open=false;$('lobby').style.display='none';}
function matchKey(){
  if(!started)return;
  if(MATCH.on){if(MATCH.state==='over')return;if(T<MATCH.leaveArm)leaveMatch();else{MATCH.leaveArm=T+2.5;toast('Press P (or MATCH) again to leave the match',2200);}return;}
  if(LB.open)closeLobby();else openLobby();
}
function startMatch(){
  if(MATCH.on)return;
  closeLobby();buildArena();makeBots();
  const w=save.wp;
  MATCH.snap=JSON.parse(JSON.stringify(w));
  MATCH.prev={foot:WK.on,x:WK.x,z:WK.z,h:WK.h,p:{x:P.x,z:P.z,h:P.h,type:P.type,hp:P.hp}};
  // starter kit if you own nothing
  if(!ownedList().some(function(id){return GUNS.indexOf(id)>=0||LAUNCHERS.indexOf(id)>=0;})){w.pistol=1;w.ammo.pistol=48;w.slots.secondary='pistol';if(!w.slots.primary||!owns(w.slots.primary))w.slots.primary='';}
  if(!ownedList().some(function(id){return MELEE.indexOf(id)>=0;})){w.knife=1;w.slots.melee='knife';}
  if(!ownedList().length){w.pistol=1;w.slots.secondary='pistol';}
  if(!WK.mesh){const pp=makePerson(0,true);WK.mesh=pp.g;WK.hum=pp;}
  WK.on=true;WK.sp=0;WK.dashT=0;P.speed=0;
  MATCH.on=true;document.body.className=(TOUCH_ON?'touch ':'')+'matchon';
  if(!camera.parent)scene.add(camera);
  WK.mesh.visible=true;WP.cur=null;applyDay(true);
  MATCH.pitch=0;beginRound();
  try{if(!TOUCH_ON)renderer.domElement.requestPointerLock();}catch(e){}
}
function beginRound(){
  $('results').style.display='none';
  MATCH.kills=0;MATCH.deaths=0;MATCH.time=180;MATCH.hp=100;MATCH.state='count';MATCH.t=3.6;MATCH.swT=0;MATCH.cnt=-1;MATCH.inv=0;MATCH.winner='';
  refillAmmo();
  const q=spawnPoint(false);
  MATCH.bots.forEach(function(b){b.kills=0;b.g.visible=false;});
  WK.x=ARENA.x+rnd(-3,3);WK.z=ARENA.z+rnd(-3,3);WK.h=0;
  const sp=ARENA.spawns[(Math.random()*ARENA.spawns.length)|0];WK.x=ARENA.x+sp[0];WK.z=ARENA.z+sp[1];WK.h=Math.atan2(ARENA.x-WK.x,ARENA.z-WK.z);
  placeVehicle();
  MATCH.bots.forEach(placeBot);
  $('mfeed').innerHTML='';MATCH.hudKey='';
}
function addFeed(a,verb,b,wn,me){
  const f=$('mfeed'),d=document.createElement('div');if(me)d.className='me';
  d.innerHTML='<i>'+a+'</i> '+verb+' <i>'+b+'</i> <small style="opacity:.7">['+wn+']</small>';f.appendChild(d);
  setTimeout(function(){if(d.parentNode)d.parentNode.removeChild(d);},4500);
  while(f.childNodes.length>5)f.removeChild(f.firstChild);
}
function matchKill(b){
  if(!MATCH.on||MATCH.state!=='live')return;
  MATCH.kills++;MATCH.lastKillT=T;addFeed('You','eliminated',b.name,WNAMES[WP.lastUsed]||'Weapon',true);sfxDing();
  if(MATCH.kills>=MATCH.target)endMatch();
}
function hurtPlayer(dmg,b){
  if(MATCH.state!=='live'||MATCH.inv>0||ADM.god||MATCH.hp<=0)return;
  if(!WK.on)dmg*=0.5; // the vehicle takes some of the hits
  MATCH.hp=Math.max(0,MATCH.hp-dmg);MATCH.lastHurt=T;MATCH.hurtFlash=0.4;shake=Math.max(shake,0.12);sfxHurt();
  if(MATCH.hp<=0){
    if(!WK.on){WK.x=P.x;WK.z=P.z;WK.h=P.h;WK.on=true;WK.mesh.visible=true;WK.sp=0;P.speed=0;}
    MATCH.deaths++;MATCH.state='dead';MATCH.t=2.6;b.kills++;MATCH.lastBy=b.name;addFeed(b.name,'eliminated','You','Assault Rifle',false);
    if(b.kills>=MATCH.target)endMatch();
  }
}
function respawnPlayer(){
  const q=spawnPoint(true);WK.on=true;WK.mesh.visible=true;WK.x=q[0];WK.z=q[1];WK.h=Math.atan2(ARENA.x-WK.x,ARENA.z-WK.z);MATCH.pitch=0;
  placeVehicle();
  MATCH.hp=100;MATCH.inv=1.8;MATCH.state='live';MATCH.swT=T+6;refillAmmo();WP.cd=0.2;WP.cur=null;
}
function endMatch(){
  if(MATCH.state==='over')return;
  const best=MATCH.bots.reduce(function(m,b){return Math.max(m,b.kills);},0);
  MATCH.state='over';
  const win=MATCH.kills>best,draw=MATCH.kills===best;
  MATCH.winner=win?'win':(draw?'draw':'lose');
  MATCH.reward=(win?150:(draw?80:40))+MATCH.kills*10;
  $('rsTitle').textContent=win?'VICTORY!':(draw?'DRAW':'DEFEAT');
  $('rsTitle').style.color=win?'#7dffa6':(draw?'#ffd36b':'#ff7a7a');
  $('rsSub').textContent=win?'You out-fought the bots. Shubh Diwali!':(draw?'Dead even.':'The bots won this one. Try again!');
  $('rsK').textContent=MATCH.kills;$('rsD').textContent=MATCH.deaths;$('rsC').textContent='+'+MATCH.reward;
  $('results').style.display='flex';WP.fire=false;WP.aimMouse=false;
  try{if(document.exitPointerLock)document.exitPointerLock();}catch(e){}
}
function leaveMatch(){
  if(!MATCH.on)return;
  const reward=MATCH.state==='over'?MATCH.reward:0;
  MATCH.on=false;MATCH.state='off';applyDay(false);$('results').style.display='none';document.body.className=TOUCH_ON?'touch':'';
  const w=save.wp,sn=MATCH.snap;
  Object.keys(sn).forEach(function(k){if(k==='ammo'||k==='slots')Object.assign(w[k],sn[k]);else w[k]=sn[k];});
  MATCH.bots.forEach(function(b){b.g.visible=false;});
  const pr=MATCH.prev;if(P.type!==pr.p.type)setVehicle(pr.p.type);P.x=pr.p.x;P.z=pr.p.z;P.h=pr.p.h;P.speed=0;P.y=GY();P.hp=pr.p.hp;HPS[P.type]=pr.p.hp;refreshDamage();
  WK.x=pr.x;WK.z=pr.z;WK.h=pr.h;WK.on=pr.foot;WK.mesh.visible=pr.foot;WK.sp=0;
  if(!pr.foot)WP.cur=null;
  WP.fire=false;WP.aimMouse=false;MATCH.hudKey='';camH=WK.h;
  try{if(document.exitPointerLock)document.exitPointerLock();}catch(e){}
  syncVM();$('mscope').style.display='none';$('mhurt').style.opacity=0;
  if(reward){save.coins+=reward;updateHUDcoins();toast('Match reward: +'+reward+' coins',3000);}
  persist();
}
// --- bots ---
function updateBots(dt){
  const live=MATCH.state==='live',A=WK.on?WK:P,aspd=WK.on?WK.sp:P.speed;
  MATCH.bots.forEach(function(b){
    if(!b.g.visible)return;
    const h=b.hum;
    if(b.ko>0){ // knocked down, then respawns
      b.ko-=dt;b.g.rotation.x+=(-1.45-b.g.rotation.x)*Math.min(1,dt*10);b.g.position.set(b.x,0.18,b.z);b.g.rotation.y=b.h;
      if(b.ko<=0){if(MATCH.state==='over')b.ko=0.01;else placeBot(b);}
      return;
    }
    if(MATCH.state==='over'||MATCH.state==='count'){poseHuman(h,0,0);h.a2.rotation.set(-1.25,0,0);h.f2.rotation.x=-0.08;h.a1.rotation.set(-1.15,0,0.32);h.f1.rotation.x=-0.1;b.g.position.set(b.x,0.3,b.z);b.g.rotation.y=b.h;return;}
    const dx=A.x-b.x,dz=A.z-b.z,d=Math.hypot(dx,dz)||0.01,alive=MATCH.state==='live'&&MATCH.hp>0;
    const los=wallDist(b.x,b.z,dx/d,dz/d,d)>=d-0.05;
    let vx=0,vz=0,face=b.h;
    const fx=dx/d,fz=dz/d,rx=-fz,rz=fx;
    if(alive&&los&&d<42){
      b.seeT+=dt;b.sT-=dt;if(b.sT<=0){b.sdir=Math.random()<0.5?-1:1;b.sT=rnd(0.7,1.8);}
      const ap=d>22?1:(d<10?-1:0);vx=fx*ap*3.4+rx*b.sdir*3.6;vz=fz*ap*3.4+rz*b.sdir*3.6;face=Math.atan2(dx,dz);
    }else{
      b.seeT=0;b.destT-=dt;
      if(!b.dest||b.destT<=0||Math.hypot(b.dest.x-b.x,b.dest.z-b.z)<2){
        if(Math.random()<0.6&&alive)b.dest={x:A.x+rnd(-6,6),z:A.z+rnd(-6,6)};
        else{let tries=0,x,z;do{x=ARENA.x+rnd(-36,36);z=ARENA.z+rnd(-36,36);}while(arenaBlocked(x,z,1.2)&&tries++<12);b.dest={x:x,z:z};}
        b.destT=rnd(2.5,5);
      }
      const ex=b.dest.x-b.x,ez=b.dest.z-b.z,ed=Math.hypot(ex,ez)||1;vx=ex/ed*4.8;vz=ez/ed*4.8;face=Math.atan2(vx,vz);
    }
    const ox=b.x,oz=b.z;b.x+=vx*dt;b.z+=vz*dt;pushBoxes(b,0.55);
    MATCH.bots.forEach(function(o){if(o!==b&&o.g.visible&&o.ko<=0){const ddx=b.x-o.x,ddz=b.z-o.z,dd=Math.hypot(ddx,ddz);if(dd<1.1&&dd>0.001){b.x+=ddx/dd*(1.1-dd)*0.5;b.z+=ddz/dd*(1.1-dd)*0.5;}}});
    b.sp=Math.hypot(b.x-ox,b.z-oz)/Math.max(dt,0.001);
    b.h+=angDiff(b.h,face)*Math.min(1,dt*7);
    // shooting
    b.cd-=dt;
    if(alive&&los&&d<42&&b.seeT>0.45&&Math.abs(angDiff(b.h,Math.atan2(dx,dz)))<0.3&&b.cd<=0){
      b.cd=rnd(0.3,0.55);
      const mv=Math.min(1,Math.abs(aspd)/5),acc=clamp(0.34-d*0.0055,0.08,0.34)*(1-mv*0.28)*0.85;
      const hit=Math.random()<acc,mx=b.x+Math.sin(b.h)*0.8,mz=b.z+Math.cos(b.h)*0.8;
      if(hit){tracer(mx,1.35,mz,A.x,1.35,A.z);hurtPlayer(5.5,b);}
      else{const a=rnd(-0.12,0.12),mm=d+rnd(-4,8);tracer(mx,1.35,mz,b.x+Math.sin(b.h+a)*mm,rnd(0.6,2),b.z+Math.cos(b.h+a)*mm);}
      for(let s=0;s<3;s++)spawnP(mx,1.35,mz,Math.sin(b.h)*6+rnd(-2,2),rnd(-1,1),Math.cos(b.h)*6+rnd(-2,2),1,0.85,0.4,0.08,3,0,0,1.6);
      if(d<30&&T>MATCH.sfxT){MATCH.sfxT=T+0.12;sfxShot('smg');}
    }
    b.ph+=dt*Math.min(b.sp,7)*1.8;const sw=Math.sin(b.ph)*0.7*Math.min(1,b.sp/3);
    poseHuman(h,sw,0);h.a2.rotation.set(-1.25,0,0);h.f2.rotation.x=-0.08;h.a1.rotation.set(-1.15,0,0.32);h.f1.rotation.x=-0.1;
    b.g.position.set(b.x,0.3,b.z);b.g.rotation.y=b.h;b.g.rotation.x=0;
  });
}
// --- first-person camera + the gun in front of you ---
function syncVM(){
  const id=null; // over-the-shoulder view: no separate first-person gun
  if(MATCH.vmFor!==id){
    if(MATCH.vm){camera.remove(MATCH.vm);MATCH.vm=null;}
    MATCH.vmFor=id;
    if(id){
      const w=new THREE.Group(),gm=gunModel(id);gm.rotation.order='YXZ';gm.rotation.set(-Math.PI/2,Math.PI,0);w.add(gm);
      const skin=hmat(0xc9976a),sh=hmat(0x6f9aa8);
      hpart(hbox(0.1,0.1,0.55),skin,0.0,-0.13,0.36,w).rotation.x=0.18;
      hpart(hbox(0.13,0.13,0.3),sh,0.02,-0.2,0.7,w).rotation.x=0.18;
      hpart(hbox(0.11,0.07,0.13),skin,-0.03,-0.03,0.04,w);
      w.position.set(0.24,-0.27,-0.5);camera.add(w);MATCH.vm=w;
    }
  }
}
function matchShoulder(){return MATCH.on?((WP.aim&&WP.cur==='sniper')?0:(WP.aim?0.45:0.7)):0;} // how far right of you the camera (and the bullets) sit
function updateCameraFP(dt){ // over-the-shoulder camera like Rivals; the sniper scope switches to first person
  const dead=MATCH.state==='dead',scoped=WP.aim&&WP.cur==='sniper'&&!dead;
  const sx=Math.sin(WK.h),sz=Math.cos(WK.h),rx=-Math.cos(WK.h),rz=Math.sin(WK.h);
  const cp=Math.cos(MATCH.pitch),spn=Math.sin(MATCH.pitch);
  const dx=sx*cp,dy=spn,dz=sz*cp;
  let cam={x:0,z:0},cy;
  if(scoped){cam.x=WK.x;cam.z=WK.z;cy=WK.y+1.62;}
  else{
    const dist=dead?6.5:(WP.aim?2.4:3.7),sh=dead?0:matchShoulder(),py=WK.y+(dead?1.2:1.65);
    cam.x=WK.x-dx*dist+rx*sh;cam.z=WK.z-dz*dist+rz*sh;cy=py-dy*dist+(dead?2.5:0.35);
    pushBoxes(cam,0.35);
  }
  if(shake>0){cam.x+=rnd(-1,1)*shake*0.5;cy+=rnd(-1,1)*shake*0.35;cam.z+=rnd(-1,1)*shake*0.5;shake=Math.max(0,shake-dt*1.6);}
  cy=Math.max(cy,0.45);
  camera.position.set(cam.x,cy,cam.z);
  if(dead)camera.lookAt(WK.x,WK.y+1,WK.z);else camera.lookAt(cam.x+dx*30,cy+dy*30,cam.z+dz*30);
  WK.mesh.visible=!scoped;
  const fovT=WP.aim?(scoped?20:Math.max(48,(WDEF[WP.cur]&&WDEF[WP.cur].zoom)||62)):70;
  if(Math.abs(camera.fov-fovT)>0.05){camera.fov+=(fovT-camera.fov)*Math.min(1,dt*12);camera.updateProjectionMatrix();}
  const sc=$('mscope');const want=scoped?'block':'none';if(sc.style.display!==want)sc.style.display=want;
}
// --- per-frame match logic + HUD ---
function wIcon(id){ // little weapon silhouette for the HUD slot
  const f='#f2f2f2';let b;
  if(id==='knife')b='<polygon points="8,12 40,6 54,10 40,14" fill="'+f+'"/><rect x="4" y="10" width="10" height="5" fill="#aaa"/>';
  else if(id==='scythe')b='<rect x="6" y="14" width="46" height="2.5" fill="'+f+'"/><path d="M40,14 Q56,2 58,12 Q50,8 42,16Z" fill="'+f+'"/>';
  else if(id==='grenade')b='<circle cx="30" cy="14" r="9" fill="'+f+'"/><rect x="27" y="2" width="6" height="5" fill="#aaa"/>';
  else if(id==='rocket'||id==='glaunch')b='<rect x="4" y="9" width="46" height="9" rx="3" fill="'+f+'"/><polygon points="50,9 58,13.5 50,18" fill="#e55"/><rect x="18" y="18" width="5" height="7" fill="#aaa"/>';
  else if(id==='pistol')b='<rect x="14" y="6" width="30" height="8" fill="'+f+'"/><rect x="14" y="14" width="8" height="12" fill="#aaa"/>';
  else if(id==='sniper')b='<rect x="2" y="12" width="58" height="4" fill="'+f+'"/><rect x="14" y="7" width="16" height="5" fill="#9cf"/><rect x="8" y="16" width="10" height="8" fill="#aaa"/>';
  else b='<rect x="6" y="9" width="40" height="7" fill="'+f+'"/><rect x="44" y="11" width="14" height="3" fill="#bbb"/><rect x="16" y="16" width="6" height="9" fill="#aaa"/><rect x="30" y="16" width="5" height="8" fill="#aaa"/>';
  return'<svg viewBox="0 0 60 28">'+b+'</svg>';
}
function updateMatch(dt){
  if(!MATCH.on)return;
  MATCH.inv=Math.max(0,MATCH.inv-dt);MATCH.hurtFlash=Math.max(0,MATCH.hurtFlash-dt);
  if(MATCH.state==='count'){
    MATCH.t-=dt;const n=Math.ceil(MATCH.t-0.6);
    if(n!==MATCH.cnt){MATCH.cnt=n;if(n>0)sfxBeep(520);else if(n===0)sfxBeep(900);}
    if(MATCH.t<=0.6&&MATCH.t>0)MATCH.cnt=0;
    if(MATCH.t<=0)MATCH.state='live';
  }else if(MATCH.state==='live'){
    MATCH.time-=dt;
    if(MATCH.hp<100&&T-MATCH.lastHurt>4)MATCH.hp=Math.min(100,MATCH.hp+14*dt);
    if(MATCH.time<=0){MATCH.time=0;endMatch();}
  }else if(MATCH.state==='dead'){
    MATCH.t-=dt;if(MATCH.t<=0)respawnPlayer();
  }
  updateBots(dt);
  if(!WK.on&&MATCH.state==='live'&&Math.abs(P.speed)>6){ // run the bots over
    const rr=DEFS[P.type].r+0.8;
    MATCH.bots.forEach(function(b){
      if(b.g.visible&&b.ko<=0&&Math.hypot(b.x-P.x,b.z-P.z)<rr){WP.lastUsed='vehicle';hurtPerson(b,clamp(Math.abs(P.speed)*4,35,100));shake=Math.max(shake,0.25);sfxHit();for(let k=0;k<8;k++)spawnP(b.x,1,b.z,rnd(-4,4),rnd(1,5),rnd(-4,4),1,0.7,0.3,0.5,2,8,2,0.8);}
    });
  }
  // HUD
  const best=MATCH.bots.reduce(function(m,b){return Math.max(m,b.kills);},0);
  const wid=WK.on?WP.cur:null,curAmmo=!wid?'':(wid==='grenade'?save.wp.nades:(MELEE.indexOf(wid)>=0?'∞':save.wp.ammo[wid])),magTxt=(wid&&GUNS.concat(LAUNCHERS).indexOf(wid)>=0)?'/'+WDEF[wid].mag:'',ammo=curAmmo+magTxt;
  const secs=Math.ceil(MATCH.time),cn=MATCH.state==='count'?Math.max(0,Math.ceil(MATCH.t-0.6)):-1;
  const key=[Math.ceil(MATCH.hp),MATCH.kills,best,secs,MATCH.state,wid,ammo,cn,Math.ceil(MATCH.t)].join('|');
  if(key!==MATCH.hudKey){
    MATCH.hudKey=key;
    $('msYou').textContent=MATCH.kills;$('msBot').textContent=best;
    $('mTime').textContent=Math.floor(secs/60)+':'+('0'+(secs%60)).slice(-2);
    $('mGoal').textContent='FIRST TO '+MATCH.target;
    const hp=Math.ceil(MATCH.hp);$('mhpTxt').textContent=hp;$('mhpBar').style.width=hp+'%';$('mhpBar').className=hp<35?'low':'';
    $('mwName').textContent=wid?WNAMES[wid].toUpperCase():'';$('mwAmmo').textContent=wid?curAmmo:'';$('mwMag').textContent=magTxt;
    const li=ownedList().indexOf(wid);$('mslotN').textContent=li+1;$('mslotIcon').innerHTML=wid?wIcon(wid):'';
    let c='';
    if(MATCH.state==='count')c=cn>0?cn:'FIGHT!';
    else if(MATCH.state==='dead')c='<span style="color:#ff7a7a">ELIMINATED</span><small>by '+MATCH.lastBy+' &middot; respawning in '+Math.ceil(MATCH.t)+'</small>';
    else if(MATCH.state==='live'&&T-(MATCH.fightT||0)<0)c='';
    $('mcenter').innerHTML=c;
  }
  if(MATCH.state==='live'&&MATCH.t<=0&&!MATCH.fightShown){MATCH.fightShown=true;$('mcenter').innerHTML='FIGHT!';setTimeout(function(){if(MATCH.state==='live')$('mcenter').innerHTML='';},700);}
  if(MATCH.state==='count')MATCH.fightShown=false;
  $('mhurt').style.opacity=Math.min(1,MATCH.hurtFlash*2.2+(MATCH.hp<30&&MATCH.state==='live'?0.25:0));
  const sw=(MATCH.state==='count'||T<MATCH.swT)?'block':'none';if(sw!==MATCH.swShown){MATCH.swShown=sw;$('mswitch').style.display=sw;}
}
// --- mouse look (pointer lock) and touch look ---
function lookBy(dx,dy,sens){const z=WP.aim?(WP.cur==='sniper'?0.3:0.65):1;WK.h-=dx*sens*z;MATCH.pitch=clamp(MATCH.pitch-dy*sens*z,-0.5,0.5);}
renderer.domElement.addEventListener('mousemove',function(e){if(MATCH.on&&MATCH.state==='live'&&document.pointerLockElement===renderer.domElement)lookBy(e.movementX||0,e.movementY||0,0.0024);});
renderer.domElement.addEventListener('pointerdown',function(e){
  if(e.pointerType!=='touch'||!MATCH.on)return;e.preventDefault();if(MATCH.lookId<0)MATCH.lookId=e.pointerId,MATCH.lx=e.clientX,MATCH.ly=e.clientY;
});
window.addEventListener('pointermove',function(e){
  if(e.pointerId!==MATCH.lookId||!MATCH.on)return;
  if(MATCH.state==='live')lookBy(e.clientX-MATCH.lx,e.clientY-MATCH.ly,0.0055);MATCH.lx=e.clientX;MATCH.ly=e.clientY;
});
const endLook=function(e){if(e.pointerId===MATCH.lookId)MATCH.lookId=-1;};
window.addEventListener('pointerup',endLook);window.addEventListener('pointercancel',endLook);
renderer.domElement.addEventListener('mousedown',function(){if(MATCH.on&&!TOUCH_ON&&document.pointerLockElement!==renderer.domElement&&MATCH.state!=='over'){try{renderer.domElement.requestPointerLock();}catch(e){}}});
$('lbPlay').addEventListener('click',startMatch);$('lbClose').addEventListener('click',closeLobby);
$('lbLoadout').addEventListener('click',function(){toggleLoadout(true);});
$('mswitch').addEventListener('click',function(){toggleLoadout(true);});
$('rsAgain').addEventListener('click',function(){beginRound();try{if(!TOUCH_ON)renderer.domElement.requestPointerLock();}catch(e){}});
$('rsLeave').addEventListener('click',leaveMatch);
// sounds
function sfxDing(){if(!AC)return;const t=AC.currentTime,o=osc('sine',880,1320,t,0.18),g=envGain(t,0.25,0.002,0.25,master);o.connect(g);o.start(t);o.stop(t+0.3);}
function sfxHurt(){if(!AC)return;const t=AC.currentTime,o=osc('sawtooth',160,70,t,0.12),g=envGain(t,0.2,0.002,0.14,master);o.connect(g);o.start(t);o.stop(t+0.18);}
function sfxBeep(f){if(!AC)return;const t=AC.currentTime,o=osc('square',f,f,t,0.18),g=envGain(t,0.14,0.002,0.18,master);o.connect(g);o.start(t);o.stop(t+0.22);}

/* ================= THE MOON (hidden easter egg: nothing in the game mentions it) ================= */
// Secret: in the plane, hold climb at the 150 m ceiling for a few seconds. It rumbles, then breaks through into space.
function updateCeiling(dt,climbing){
  if(MOON.on||SPACE.open||SPACE.busy)return;
  if(P.y>=149&&climbing){
    SPACE.hold+=dt;
    if(SPACE.hold>1.2)shake=Math.max(shake,Math.min(0.3,(SPACE.hold-1.2)*0.15)); // the only hint: the plane starts to shake
    if(SPACE.hold>3.5){SPACE.open=true;SPACE.hold=0;shake=0.9;sfxLiftoff();}
  }else SPACE.hold=Math.max(0,SPACE.hold-dt*2);
}
const MOON_AT={x:-450,y:380,z:-700}; // where the moon hangs in the Earth sky
function updateSpace(dt){
  let f=0;
  if(MOON.on)f=1;else if(SPACE.open)f=clamp((P.y-170)/450,0,1); // the sky goes black as you leave the atmosphere
  if(MATCH.on){scene.background.setRGB(0.78,0.8,0.84);scene.fog.color.setRGB(0.78,0.8,0.84);scene.fog.density=0.0035;}
  else{scene.background.setRGB(0.039*(1-f),0.024*(1-f),0.094*(1-f));scene.fog.color.setHex(0x120a2a);scene.fog.density=0.0055*(1-f)+0.00004*f;}
  if(MOON.on){
    if(!WK.on&&Math.abs(P.speed)>3&&P.y-MOON.y<3&&Math.random()<dt*50){ // moon dust behind the wheels
      const fx=Math.sin(P.h),fz=Math.cos(P.h);
      spawnP(P.x-fx*2+rnd(-0.6,0.6),MOON.y+0.3,P.z-fz*2+rnd(-0.6,0.6),rnd(-1.5,1.5),rnd(0.5,2.5),rnd(-1.5,1.5),0.5,0.5,0.55,1.4,1.2,0.4,0,2.4);
    }
    return;
  }
  if(SPACE.open&&!SPACE.busy&&!WK.on&&Math.hypot(P.x-MOON_AT.x,P.y-MOON_AT.y,P.z-MOON_AT.z)<95)startMoonTrip();
}
function hideEarth(on){ // while you are on the moon the whole Earth world is switched off
  if(on){
    MOON.hidden=[];
    scene.children.slice().forEach(function(o){
      if(o===MOON.world||o.isLight||o.isPoints||o.isSprite||o===P.v.group||o===WK.mesh||(o.userData&&o.userData.keep))return;
      if(o.visible){o.visible=false;MOON.hidden.push(o);}
    });
  }else{MOON.hidden.forEach(function(o){o.visible=true;});MOON.hidden=[];}
}
function startMoonTrip(){
  if(SPACE.busy||MOON.on||MATCH.on)return;SPACE.busy=true;
  $('fade').style.opacity='1';sfxLiftoff();
  after(0.95,function(){enterMoon();$('fade').style.opacity='0';SPACE.busy=false;});
}
function enterMoon(){
  MOON.on=true;SPACE.open=false;SPACE.hold=0;
  rockets.forEach(function(r){scene.remove(r.mesh);});rockets.length=0;
  crowd.forEach(function(c){scene.remove(c.g);});crowd.length=0;
  hideEarth(true);MOON.world.visible=true;
  P.x=0;P.z=0;P.h=0;P.speed=0;P.steer=0;P.y=MOON.y+(P.type==='plane'?0.95:0);
  if(WK.on){WK.x=4;WK.z=0;WK.sp=0;WK.h=0;WK.jy=0;WK.vy=0;}
  camH=0;shake=0;applyVolume();
  toast('One small step... Happy Diwali from the Moon!',5000);
}
function returnToEarth(){
  if(!MOON.on||SPACE.busy)return;SPACE.busy=true;
  $('fade').style.opacity='1';
  after(0.95,function(){
    MOON.on=false;MOON.world.visible=false;hideEarth(false);
    rockets.forEach(function(r){scene.remove(r.mesh);});rockets.length=0;
    P.x=0;P.z=-6;P.h=0;P.speed=0;P.steer=0;P.y=(P.type==='plane')?0.95:0;
    if(WK.on){WK.x=3;WK.z=-6;WK.sp=0;WK.h=0;WK.jy=0;WK.vy=0;}
    camH=0;shake=0;applyVolume();
    $('fade').style.opacity='0';SPACE.busy=false;toast('Welcome back to Earth!',2500);
  });
}
(function(){ // the moon surface, built once, switched on only while you are there
  const w=new THREE.Group();w.position.y=MOON.y;w.visible=false;scene.add(w);MOON.world=w;
  const gray=mat(0x8f8f98),dark=mat(0x65656e),light=mat(0xaaaab3);
  const disc=new THREE.Mesh(new THREE.CircleGeometry(MOON.r+60,56),gray);disc.rotation.x=-Math.PI/2;w.add(disc);
  for(let i=0;i<30;i++){
    const a=Math.random()*TAU,d=rnd(50,MOON.r-10),r=rnd(6,24),x=Math.cos(a)*d,z=Math.sin(a)*d;
    const rim=new THREE.Mesh(new THREE.RingGeometry(r*0.82,r,18),light);rim.rotation.x=-Math.PI/2;rim.position.set(x,0.06,z);w.add(rim);
    const pit=new THREE.Mesh(new THREE.CircleGeometry(r*0.82,18),dark);pit.rotation.x=-Math.PI/2;pit.position.set(x,0.04,z);w.add(pit);
  }
  const rockG=new THREE.DodecahedronGeometry(1,0),rockM=mat(0x777780);
  for(let i=0;i<46;i++){
    const a=Math.random()*TAU,d=rnd(22,MOON.r-6),s=rnd(0.8,3.6),x=Math.cos(a)*d,z=Math.sin(a)*d;
    if(Math.hypot(x,z-34)<9)continue; // keep the flag area clear
    const k=new THREE.Mesh(rockG,rockM);k.scale.set(s,s*0.7,s*rnd(0.8,1.2));k.position.set(x,s*0.3,z);k.rotation.y=rnd(0,TAU);w.add(k);
    if(s>1.4)MOON.rocks.push({x:x,z:z,r:s*0.85});
  }
  const pad=new THREE.Mesh(new THREE.RingGeometry(9,10.5,40),new THREE.MeshBasicMaterial({color:0xffffff,transparent:true,opacity:0.45}));pad.rotation.x=-Math.PI/2;pad.position.y=0.08;w.add(pad);
  // a Diwali flag with a ring of diyas
  box(0.2,11,0.2,mat(0xdddddd),0,5.5,34,w);
  const fc=document.createElement('canvas');fc.width=256;fc.height=150;const g=fc.getContext('2d');
  const gr=g.createLinearGradient(0,0,256,150);gr.addColorStop(0,'#ff8a1f');gr.addColorStop(1,'#d6246e');g.fillStyle=gr;g.fillRect(0,0,256,150);
  g.fillStyle='#fff3c4';g.font='bold 40px Arial';g.textAlign='center';g.textBaseline='middle';g.fillText('HAPPY',128,40);g.fillText('DIWALI!',128,86);g.font='bold 18px Arial';g.fillText('from the Moon',128,126);
  const flag=new THREE.Mesh(new THREE.PlaneGeometry(6.4,3.75),new THREE.MeshBasicMaterial({map:new THREE.CanvasTexture(fc),side:THREE.DoubleSide}));
  flag.position.set(3.4,8.8,34);w.add(flag);
  const dg=new THREE.CylinderGeometry(0.45,0.3,0.3,8),dm=basic(0xff9a1a),fg=new THREE.SphereGeometry(0.2,6,5),fm=basic(0xffe066);
  for(let i=0;i<14;i++){
    const a=i/14*TAU,x=Math.sin(a)*5.5,z=34+Math.cos(a)*5.5;
    const d1=new THREE.Mesh(dg,dm);d1.position.set(x,0.2,z);w.add(d1);const f1=new THREE.Mesh(fg,fm);f1.position.set(x,0.6,z);w.add(f1);
  }
  // Earth hangs in the black sky
  const ec=document.createElement('canvas');ec.width=256;ec.height=128;const e=ec.getContext('2d');
  e.fillStyle='#1d5fb8';e.fillRect(0,0,256,128);
  for(let i=0;i<14;i++){e.fillStyle='#3aa356';e.beginPath();e.ellipse(rnd(0,256),rnd(10,118),rnd(12,34),rnd(8,20),rnd(0,3),0,TAU);e.fill();}
  for(let i=0;i<18;i++){e.fillStyle='rgba(255,255,255,0.75)';e.beginPath();e.ellipse(rnd(0,256),rnd(0,128),rnd(10,30),rnd(3,7),rnd(0,3),0,TAU);e.fill();}
  const earth=new THREE.Mesh(new THREE.SphereGeometry(70,18,12),new THREE.MeshBasicMaterial({map:new THREE.CanvasTexture(ec),fog:false}));
  earth.position.set(-380,300,-760);w.add(earth);
})();

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
  on('admWeapons',function(){const w=save.wp;w.rifle=1;w.shorty=1;w.scythe=1;w.nades=15;w.ammo.rifle=100;w.ammo.shorty=30;persist();toast('All weapons and full ammo. Get out (X) to hold them.',2600);});
  on('admMoon',function(){if(MOON.on)returnToEarth();else startMoonTrip();});
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

/* ================= TOUCH CONTROLS (phones and tablets): an on-screen joystick and buttons that press the same keys ================= */
let TOUCH_ON=false;
function fireKey(code,down){const f=down?onKeyDown:onKeyUp;if(f)f({code:code,repeat:false,target:null,preventDefault:function(){}});}
const TJ={id:-1,up:false,dn:false,lf:false,rt:false};
const TB=new Map(); // pointerId -> key code held by that finger
function setJoy(dx,dy){ // dx,dy in -1..1 (up is negative dy)
  const n={up:dy<-0.35,dn:dy>0.35,lf:dx<-0.35,rt:dx>0.35};
  if(n.up!==TJ.up){TJ.up=n.up;fireKey('KeyW',n.up);}
  if(n.dn!==TJ.dn){TJ.dn=n.dn;fireKey('KeyS',n.dn);}
  if(n.lf!==TJ.lf){TJ.lf=n.lf;fireKey('KeyA',n.lf);}
  if(n.rt!==TJ.rt){TJ.rt=n.rt;fireKey('KeyD',n.rt);}
  const k=$('knob');k.style.transform='translate('+(dx*38)+'px,'+(dy*38)+'px)';
}
function joyMove(e){
  const r=$('joy').getBoundingClientRect(),R=r.width/2;
  let dx=(e.clientX-(r.left+R))/R,dy=(e.clientY-(r.top+R))/R;const l=Math.hypot(dx,dy);if(l>1){dx/=l;dy/=l;}
  setJoy(dx,dy);
}
function btnOf(t){while(t&&t.dataset&&t.dataset.k===undefined&&t.dataset.fs===undefined)t=t.parentNode;return(t&&t.dataset&&(t.dataset.k!==undefined||t.dataset.fs!==undefined))?t:null;}
function enableTouch(){
  if(TOUCH_ON)return;TOUCH_ON=true;document.body.className='touch';
  const root=$('touch');
  $('joy').addEventListener('pointerdown',function(e){e.preventDefault();TJ.id=e.pointerId;joyMove(e);});
  window.addEventListener('pointermove',function(e){if(e.pointerId===TJ.id)joyMove(e);});
  const endJ=function(e){if(e.pointerId===TJ.id){TJ.id=-1;setJoy(0,0);}};
  window.addEventListener('pointerup',endJ);window.addEventListener('pointercancel',endJ);
  root.addEventListener('pointerdown',function(e){
    const b=btnOf(e.target);if(!b)return;e.preventDefault();
    if(b.dataset.fs!==undefined){try{const el=document.documentElement;(el.requestFullscreen||el.webkitRequestFullscreen).call(el);}catch(x){}return;}
    TB.set(e.pointerId,b.dataset.k);b.className+=' on';fireKey(b.dataset.k,true);
  });
  const endB=function(e){
    const k=TB.get(e.pointerId);if(k===undefined)return;TB.delete(e.pointerId);fireKey(k,false);
    const els=root.querySelectorAll('.tb.on');for(let i=0;i<els.length;i++)if(els[i].dataset.k===k)els[i].className=els[i].className.replace(' on','');
  };
  window.addEventListener('pointerup',endB);window.addEventListener('pointercancel',endB);
  touchUI(true);
}
let touchKey='';
function touchUI(force){
  if(!TOUCH_ON)return;
  const hide=!started||shopOpen||talkOpen||LO.open||kicked||LB.open||(MATCH.on&&MATCH.state==='over');
  const k=[hide,WK.on,MOON.on,MATCH.on,!MOON.on&&started&&nearShop()].join();
  if(k===touchKey&&!force)return;touchKey=k;
  $('touch').className=(hide?'hide':'')+(MATCH.on?' match':'')+(WK.on?' foot':'')+(MOON.on?' moon':'')+((!MOON.on&&started&&nearShop())?' shop':'');
}
(function(){
  const coarse=typeof matchMedia!=='undefined'&&matchMedia('(pointer:coarse)').matches;
  const q=typeof location!=='undefined'&&/[?&]touch/.test(location.search);
  if(coarse||q)enableTouch();
  else window.addEventListener('touchstart',function(){enableTouch();},{once:true,passive:true});
  document.addEventListener&&document.addEventListener('contextmenu',function(e){if(TOUCH_ON)e.preventDefault();});
})();

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
  updateWalker(dt);updateDamageFx(dt);updateCrowd(dt);updateKeepers(dt);updateSpace(dt);updateWeapons(dt);updateMatch(dt);
  updateAI(dt);updateObstacles(dt);updatePeople(dt);updateCoins(dt);
  updateFireworks(dt);updateFlashes(dt);updateNet(dt);updateEngine();
  blinkT+=dt;if(blinkT>0.6&&!MOON.on){blinkT=0;const on=(Math.floor(T/0.6)%2)===0;if(window.__lightsA)window.__lightsA.visible=on;if(window.__lightsB)window.__lightsB.visible=!on;}
  updateCamera(dt);
  hudT-=dt;
  if(hudT<=0){
    hudT=0.1;
    $('spd').textContent=WK.on?'Walking':(P.type==='plane'?'Alt '+Math.round(P.y-GY())+' m · ':'')+Math.round(Math.abs(P.speed)*3.6)+' km/h';
    $('veh').textContent=WK.on?'On foot':DEFS[P.type].name+(P.hp<=0?' (broken)':'');
    $('armor').textContent=save.str[P.type]|0;
    $('cond').textContent=Math.round(P.hp)+'%';$('cond').style.color=P.hp>60?'#7dff6b':(P.hp>30?'#ffd36b':'#ff6b6b');
    let pt='';
    if(started&&!shopOpen){
      if(MOON.on)pt='Press R to fly back to Earth';
      else if(nearShop())pt='Press E to talk to the shopkeeper';
      else if(WK.on&&Math.hypot(WK.x-P.x,WK.z-P.z)<DEFS[P.type].r+4)pt='Press X to get back in';
      else if(!WK.on&&P.hp<=0)pt='Vehicle broken! Press X to get out, or 1-5 for another vehicle';
    }
    if(TOUCH_ON)pt=pt.replace('Press E to talk','Tap SHOP to talk').replace('Press X to get back in','Tap OUT/IN to get back in').replace('Press R to fly','Tap BACK to fly').replace('Press X to get out, or 1-5 for another vehicle','Tap OUT/IN to get out, or pick another vehicle');
    $('prompt').textContent=pt;$('prompt').style.display=pt?'block':'none';
    if(ADM.open&&ADM.unlocked)renderAdminPlayers();
    if(WP.dirty){WP.dirty=0;persist();} // ammo is saved a few times a second, not on every bullet
    if(toastUntil>0&&T>toastUntil){toastUntil=0;$('toast').style.display='none';}
  }
  renderer.render(scene,camera);
}
requestAnimationFrame(frame);
window.__dcs={match:{MATCH:MATCH,ARENA:ARENA,start:startMatch,leave:leaveMatch,open:openLobby,hurt:hurtPlayer,end:endMatch,lobby:LB,matchKill:matchKill,projs:projs,autoSlot:autoSlot},touch:{enable:enableTouch,setJoy:setJoy,fireKey:fireKey,TJ:TJ,on:function(){return TOUCH_ON;}},weap:{LO:LO,setSlot:setSlot,toggleLoadout:toggleLoadout,ownedList:ownedList,WP:WP,WDEF:WDEF,owns:owns,cycle:cycleWeapon,tryFire:tryFire,hurt:hurtPerson,startDash:startDash,nades:nades,explode:explodeNade,throwGrenade:throwGrenade,scytheHit:scytheHit,swing:swingScythe},moon:{MOON:MOON,SPACE:SPACE,start:startMoonTrip,back:returnToEarth},ADM:ADM,admin:{massRemove:massRemove,removeDesc:removeDesc,undo:undoLast,onMsg:onAdminMsg,unlock:unlockAdmin,lock:lockAdmin,toggle:toggleAdmin,teleport:teleport,giveSelf:giveSelf,undoCount:function(){return undoStack.length;},others:others,myId:myId},trees:trees,statics:statics,people:people,keepers:keepers,talk:{list:showList,bye:function(){closeShop(true);},open:function(){return talkOpen;}},shopPts:shopPts,SHOP:SHOP,WK:WK,crowd:crowd,HPS:HPS,toggleOnFoot:toggleOnFoot,P:P,rockets:rockets,ais:ais,buildings:buildings,obstacles:obstacles,coins:coins,explode:explode,launchRocket:launchRocket,registerImpact:registerImpact,save:save};
})();
</script>
</body>
</html>
