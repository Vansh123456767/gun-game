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
.gbox{text-align:left;width:min(640px,94vw)}
.gbox h2{text-align:center}.gbox h3{margin:14px 0 4px;color:#ffd36b;font-size:16px;letter-spacing:1px}
.gbox ul{margin:0;padding:0;list-style:none}.gbox li{margin:5px 0;font-size:14px;line-height:1.4;color:#efe6ff}
.gbox li b{display:inline-block;min-width:78px;background:#3a1f66;border:1px solid #8a63d6;border-radius:6px;padding:0 7px;margin-right:6px;color:#ffd36b;font-size:12px;text-align:center}
.gbox .goal{background:rgba(255,200,100,.12);border:1px solid rgba(255,200,100,.4);border-radius:10px;padding:8px 10px;font-size:14px}
#joy::after{content:'MOVE';position:absolute;left:0;right:0;top:-18px;text-align:center;font-size:11px;font-weight:800;letter-spacing:2px;color:rgba(255,255,255,.7);text-shadow:0 1px 2px #000}
.rotmsg{display:none;margin:8px 0 0;font-size:14px;color:#ffd36b;font-weight:700}
@media (orientation:portrait) and (pointer:coarse){.rotmsg{display:block}}
#joy{position:absolute;left:max(18px,env(safe-area-inset-left));bottom:max(18px,env(safe-area-inset-bottom));width:132px;height:132px;border-radius:50%;background:rgba(255,255,255,.1);border:2px solid rgba(255,255,255,.35);pointer-events:auto;touch-action:none}
#knob{position:absolute;left:50%;top:50%;width:56px;height:56px;margin:-28px 0 0 -28px;border-radius:50%;background:rgba(255,176,32,.7);border:2px solid #ffe3a0;pointer-events:none}
#tpad{position:absolute;right:max(14px,env(safe-area-inset-right));bottom:max(14px,env(safe-area-inset-bottom));width:190px;height:170px;pointer-events:none}
#tpad .tb{position:absolute;pointer-events:auto}
#tbar{position:absolute;right:max(8px,env(safe-area-inset-right));top:max(8px,env(safe-area-inset-top));display:flex;flex-wrap:wrap;justify-content:flex-end;gap:5px;max-width:min(330px,62vw);pointer-events:none}
.tb{display:flex;flex-direction:column;align-items:center;justify-content:center;box-sizing:border-box;background:rgba(30,14,60,.72);border:2px solid rgba(255,200,100,.55);border-radius:12px;color:#fff;font-weight:800;pointer-events:auto;touch-action:none;text-align:center;line-height:1.05}
.tb small{font-size:9px;font-weight:700;opacity:.85}
.tb.on{background:rgba(255,157,0,.85);color:#2a0a40}
.tb.tog{background:rgba(0,200,120,.88);color:#06281a;border-color:#b8ffdf}
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
.modes{display:grid;grid-template-columns:1fr 1fr;gap:6px;margin:8px 0}.modes button{margin:0;padding:8px 6px;font-size:13px;background:#3a2166;color:#fff;border:2px solid transparent}.modes button small{display:block;font-weight:400;font-size:10px;opacity:.75}
.modes button.sel,.sizes button.sel{border-color:#ffb020;background:#5a2fa0}.sizes{display:flex;justify-content:center;gap:4px}.sizes button{margin:2px;padding:6px 10px;font-size:14px;background:#3a2166;color:#fff;border:2px solid transparent}
#touch.match .nm{display:none}
#tkList .trow{display:flex;align-items:center;gap:10px;margin:7px 0;text-align:left;background:#2a1850;border-radius:10px;padding:8px 10px}
#tkList .tt{flex:1;font-size:13px}#tkList .tb2{height:8px;background:#160a2c;border-radius:4px;overflow:hidden;margin-top:5px}#tkList .tb2 i{display:block;height:100%;background:#ffb020}
#tkList button{padding:6px 10px;font-size:12px;margin:0}#tkList .done i{background:#7dff6b}
#mvCards{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:10px 0}
.mcard{background:#3a2166;border:3px solid transparent;border-radius:12px;padding:8px;cursor:pointer;color:#fff;text-align:center;transition:transform .12s}
.mcard:hover{transform:translateY(-3px)}.mcard.sel{border-color:#ffb020;background:#5a2fa0}.mcard.win{border-color:#7dff6b;box-shadow:0 0 18px #7dff6b;transform:scale(1.04)}
.mcard .th{height:78px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:40px;margin-bottom:6px}
.mcard b{display:block;font-size:15px}.mcard small{display:block;font-size:11px;opacity:.8;min-height:28px}
.mcard .vt{margin-top:6px;font-size:22px;font-weight:900;color:#ffd36b}.mcard .bar{height:6px;border-radius:3px;background:#ffb020;width:0;transition:width .2s;margin:4px auto 0}
#touch:not(.match) .mt{display:none}
</style>
</head>
<body>
<div id="game"></div>
<div id="hud" class="panel"><div id="admbadge" style="display:none;color:#ff6b8a;font-weight:800">ADMIN MODE</div>Coins: <b id="coins">0</b><br>Tasks (H): <b id="tkn">0</b> to claim<br>Vehicle: <b id="veh">Car</b><br>Condition: <b id="cond">100%</b> &middot; Armor <b id="armor">0</b>/5<br><span id="spd">0 km/h</span></div>
<div id="net" class="panel">Players here: <b id="players">1</b><br><small style="opacity:.75">(open a 2nd tab to test)</small></div>
<div id="help" class="panel">
<kbd>W A S D</kbd>/arrows drive &nbsp; <kbd>Space</kbd> handbrake (plane: climb) &nbsp; <kbd>Shift</kbd> nitro (plane: descend)<br>
<kbd>1</kbd> Car <kbd>2</kbd> Truck <kbd>3</kbd> Scooter <kbd>4</kbd> Bike <kbd>5</kbd> Plane &nbsp; <kbd>F</kbd> rocket volley &nbsp; <kbd>G</kbd> pataka chain<br>
<kbd>?</kbd> controls guide &nbsp; <kbd>X</kbd> get out / get in &nbsp; <kbd>C</kbd> slide (on foot) &nbsp; <kbd>B</kbd> cheer &nbsp; <kbd>E</kbd> talk to the shopkeeper (pink Diwali Shop, one is beside the start) &nbsp; <kbd>V</kbd> camera &nbsp; <kbd>M</kbd> mute &nbsp; <kbd>H</kbd> hide help<br>
Weapons (on foot, buy at the shop): <kbd>Z</kbd> switch &nbsp; <kbd>L</kbd> loadout &nbsp; <kbd>P</kbd> play a match (vs bots) &nbsp; <kbd>Click</kbd>/<kbd>J</kbd> fire &nbsp; hold <kbd>Right-click</kbd>/<kbd>Q</kbd> aim (Scythe: dash)
</div>
<div id="prompt" class="panel">Press E to open the Diwali Shop</div>
<div id="toast"></div>
<div id="pop">BONK!</div>
<div id="fade"></div>
<div id="mhud">
 <div id="mtop">
  <div class="tm"><small id="msYouL">YOU</small><span class="sc you" id="msYou">0</span></div>
  <div class="tm mid"><span class="hg">&#8987;</span><b id="mTime">3:00</b><small id="mGoal">FIRST TO 10</small></div>
  <div class="tm"><small id="msBotL">BOTS</small><span class="sc bot" id="msBot">0</span></div>
 </div>
 <div id="mfeed"></div>
 <div id="mhp"><small id="mName">YOU</small><div class="shield"><i></i></div><div class="bar"><i id="mhpBar"></i></div><b id="mhpTxt">100</b></div>
 <div id="mammo"><div class="amm"><b id="mwAmmo">0</b><span id="mwMag"></span></div><small id="mwName"></small></div>
 <div id="mslot"><em id="mslotN">1</em><div id="mslotIcon"></div></div>
 <div id="mswitch">Switch Loadout <kbd>E</kbd></div>
 <div id="mcenter"></div><div id="mhurt"></div><div id="mscope"></div>
</div>
<div id="guide" class="modal"><div class="mbox gbox"><h2>HOW TO PLAY</h2>
 <div class="goal"><b>Goal:</b> drive around the Diwali city, collect the yellow coins, light fireworks, and spend coins at the pink Diwali Shop on patakas, vehicle upgrades and weapons. You can also play <b>Diwali Duels</b> (a shooting match against bots).</div>
 <h3>MOVING (PHONE)</h3><ul>
  <li><b>Joystick</b> bottom-left. Push <u>up</u> to go forward, <u>down</u> to reverse or brake, <u>left / right</u> to steer.</li>
  <li><b>&#9650; JUMP</b> handbrake in a vehicle, climb in the plane.</li>
  <li><b>&#9889;</b> nitro boost in a vehicle, run when walking.</li>
  <li><b>OUT/IN</b> gets you out of the vehicle to walk, and back in when you stand next to it.</li></ul>
 <h3>TOP-RIGHT BUTTONS</h3><ul>
  <li><b>SHOP</b> appears when you are near a shop. Tap it to talk to the shopkeeper, then pick "Buy patakas".</li>
  <li><b>&#128663; &#128667; &#128757; &#127949; &#9992;&#65039;</b> switch between car, truck, scooter, bike and plane.</li>
  <li><b>ROCKETS</b> / <b>PATAKA</b> light fireworks and crackers around you.</li>
  <li><b>CAMERA</b> changes the view. <b>MUTE</b> turns sound off. <b>FULL</b> goes fullscreen.</li>
  <li><b>MATCH</b> opens Diwali Duels. <b>?</b> opens this guide again.</li></ul>
 <h3>WEAPONS (WALKING)</h3><ul>
  <li><b>Buy</b> weapons in the shop, then tap <b>OUT/IN</b> to get out of your vehicle.</li>
  <li><b>FIRE</b> shoots. <b>AIM</b> (hold) zooms in. <b>SWITCH</b> changes weapon. <b>LOADOUT</b> chooses which weapons you carry.</li>
  <li><b>SLIDE</b> a fast low glide. <b>CHEER</b> raises your arms.</li></ul>
 <h3>DIWALI DUELS</h3><ul>
  <li>Tap <b>MATCH</b>, then <b>PLAY</b>. You and 3 bots fight in a small town. First to 10 eliminations wins coins.</li>
  <li>Joystick walks and strafes. <b>Drag your finger on the right side of the screen</b> to look around and aim. Tap <b>FIRE</b> to shoot.</li>
  <li>You can also drive a vehicle in the match and run the bots over. Tap <b>MATCH</b> twice to leave.</li></ul>
 <h3>KEYBOARD (COMPUTER)</h3><ul>
  <li><b>W A S D</b> move or drive &nbsp; <b>Space</b> handbrake / climb &nbsp; <b>Shift</b> nitro / run</li>
  <li><b>X</b> get out / in &nbsp; <b>E</b> shop &nbsp; <b>1-5</b> vehicle &nbsp; <b>F</b> rockets &nbsp; <b>G</b> pataka &nbsp; <b>V</b> camera</li>
  <li><b>Click / J</b> fire &nbsp; <b>Right-click / Q</b> aim &nbsp; <b>Z</b> switch &nbsp; <b>L</b> loadout &nbsp; <b>C</b> slide &nbsp; <b>B</b> cheer &nbsp; <b>P</b> match</li>
  <li><b>O</b> play online (host a room / join with a code) &nbsp; <b>V</b> in a match: first-person / over-the-shoulder</li>
  <li><b>?</b> shows this guide</li></ul>
 <button id="gdClose">GOT IT, LET'S PLAY</button></div></div>
<div id="lobby" class="modal"><div class="mbox"><h2>DIWALI DUELS</h2><p style="font-size:10px;opacity:.6;margin:2px 0">Weapon models via poly.pizza (CC-BY): Quaternius, Zsky, Qubzy, dook, skony_3d, CreativeTrio.</p><div class="modes" id="lbModes"><button id="lbm_ffa" data-m="ffa">FREE-FOR-ALL<small>You vs 3 bots</small></button><button id="lbm_tdm" data-m="tdm">TEAM DEATHMATCH<small>1v1 up to 5v5</small></button><button id="lbm_gungame" data-m="gungame">GUN GAME<small>Every kill = new weapon</small></button><button id="lbm_melee" data-m="melee">MELEE ONLY<small>Katana, Knife, Scythe</small></button></div>
 <div id="lbTeams" style="display:none"><small style="opacity:.7">TEAM SIZE</small><div class="sizes" id="lbSizes"><button id="lbs_1" data-n="1">1v1</button><button id="lbs_2" data-n="2">2v2</button><button id="lbs_3" data-n="3">3v3</button><button id="lbs_4" data-n="4">4v4</button><button id="lbs_5" data-n="5">5v5</button></div></div>
 <p id="lbDesc"></p>
 <div class="lst" id="lbList"></div><p style="font-size:12px;opacity:.8">First-person view (V = over-the-shoulder). Move: W A S D &middot; Look: mouse (click to capture) or drag on a phone &middot; Fire: click / FIRE &middot; Aim: right-click / Q &middot; Switch: Z or 1-4 &middot; Win coins!</p>
 <button id="lbPlay">PLAY</button><button id="lbTasks" class="alt">TASKS</button><button id="lbLoadout" class="alt">LOADOUT</button><button id="lbOnline" class="alt">ONLINE</button><button id="lbClose" class="alt">CLOSE</button></div></div>
<div id="tasks" class="modal"><div class="mbox" style="width:min(520px,96vw)"><h2>TASKS</h2><p style="font-size:12px;opacity:.75;margin:2px 0 8px">Finish tasks to earn coins. They reset every day. Progress counts in Diwali Duels and the city.</p><div id="tkList"></div><button id="tkAll">CLAIM ALL</button><button id="tkClose" class="alt">CLOSE (H)</button></div></div>
<div id="mapvote" class="modal"><div class="mbox" style="width:min(640px,96vw)"><h2>VOTE FOR A MAP</h2><p id="mvTime" style="font-size:18px;color:#ffd36b;font-weight:800">10</p><div id="mvCards"></div><p style="font-size:12px;opacity:.75">Click a map (or press 1-3). Most votes wins, ties are random.</p><button id="mvBack" class="alt">BACK</button></div></div>
<div id="online" class="modal"><div class="mbox"><h2>PLAY ONLINE</h2>
 <p id="olStat">Not connected. Type any server code and press JOIN: if nobody has opened it yet you create it, and everyone else who types the same code joins you.</p>
 <div><button id="olHost">HOST A ROOM</button></div>
 <div id="olCode" style="display:none;font-size:34px;letter-spacing:8px;color:#ffd36b;font-weight:800;margin:6px 0"></div>
 <div style="margin:8px 0"><input id="olIn" type="text" maxlength="12" placeholder="SERVER CODE" autocomplete="off" style="width:190px;padding:9px;border-radius:8px;border:2px solid #59467f;background:#160a2c;color:#fff;font-size:18px;letter-spacing:4px;text-align:center;text-transform:uppercase"><button id="olJoin">JOIN</button></div>
 <div><button id="olServer" class="alt">JOIN OUR SERVER (code DIWALI)</button></div>
 <div class="lst" id="olList">No other players yet.</div>
 <button id="olLeave" class="alt" style="display:none">DISCONNECT</button><button id="olClose" class="alt">CLOSE</button></div></div>
<div id="chal" class="modal"><div class="mbox"><h2>DUEL!</h2><p id="chalTxt"></p><button id="chalYes">ACCEPT</button><button id="chalNo" class="alt">DECLINE</button></div></div>
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
  <div class="tb f" data-k="KeyC">&#127939;<small>SLIDE</small></div>
  <div class="tb nm f" data-k="KeyB">&#127881;<small>CHEER</small></div>
  <div class="tb nm" data-k="KeyH">&#128203;<small>TASKS</small></div>
  <div class="tb f mt" data-k="KeyK">&#128481;&#65039;<small>DEFLECT</small></div>
  <div class="tb f mt" data-k="KeyT">&#127919;<small>AIMBOT</small></div>
  <div class="tb f mt" data-k="KeyY">&#128299;<small>AUTO</small></div>
  <div class="tb nm" data-k="KeyV">&#127909;<small>CAMERA</small></div>
  <div class="tb mt" data-k="KeyV">&#128065;<small>VIEW</small></div>
  <div class="tb" data-k="KeyP">&#9876;&#65039;<small>MATCH</small></div>
  <div class="tb" data-gd="1">?<small>HELP</small></div>
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
    <h4>MATCH TIME</h4>
    <label>Minutes <input id="admMTime" type="number" min="0.1" max="999" step="0.5" value="3" style="width:80px"></label>
    <label><input type="checkbox" id="admMInf"> Infinite time (match only ends on the score)</label>
    <div class="arow"><button class="ab" id="admMApply">Apply to current match</button></div>
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
<div id="start"><h1>Diwali City Simulator</h1><p>Drive, fly, collect coins and light up the night sky.</p><p>Turn your sound on for the full firework + crash audio.</p><div class="btn">Tap, click or press any key to start</div><p class="rotmsg">&#128241; Turn your phone sideways (landscape) for the best view.</p></div>

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
const save={coins:0,up:{volley:0,auto:0,nitro:0,gold:0,magnet:0},str:{car:0,truck:0,scooter:0,bike:0,plane:0},wp:{rifle:0,shorty:0,scythe:0,nades:0,pistol:0,smg:0,sniper:0,knife:0,katana:0,rocket:0,glaunch:0,ammo:{rifle:0,shorty:0,pistol:0,smg:0,sniper:0,rocket:0,glaunch:0},slots:{primary:'rifle',secondary:'shorty',melee:'scythe',utility:'grenade'}}};
const MATCH={on:false,state:'off'}; // match mode state (filled in by the MATCH section)
try{const s=JSON.parse(localStorage.getItem('dcs_save_v2')||'null');if(s){save.coins=s.coins|0;Object.assign(save.up,s.up||{});Object.assign(save.str,s.str||{});if(s.wp){Object.assign(save.wp,s.wp);save.wp.ammo=Object.assign({rifle:0,shorty:0,pistol:0,smg:0,sniper:0,rocket:0,glaunch:0},s.wp.ammo||{});save.wp.slots=Object.assign({primary:'rifle',secondary:'shorty',melee:'scythe',utility:'grenade'},s.wp.slots||{});}}}catch(e){}
function persist(){if(MATCH.on)return;try{localStorage.setItem('dcs_save_v2',JSON.stringify(save));}catch(e){}}

/* ================= ADMIN SETTINGS ================= */
// Change this password! Press F2 (or the Admin button) in the game and enter it to become admin.
// Set it to '' (empty) to be admin automatically, with no password.
const ADMIN_PASSWORD='diwali-admin';
const ADM={unlocked:false,open:false,god:false,noclip:false,speed:1,del:false,mtime:180}; // mtime = match length in seconds (Infinity = no time limit)
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
const camera=new THREE.PerspectiveCamera(72,window.innerWidth/window.innerHeight,0.08,3000);
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
const SM={},RB={};
function smat(c){return SM[c]||(SM[c]=new THREE.MeshBasicMaterial({color:c,vertexColors:true}));} // true colours (not washed out by the daylight); soft shading is baked into the shape
const BL=new THREE.Vector3(-0.35,0.75,0.55).normalize();
function bake(g){ // bake a soft light-to-dark shade into the vertex colours
  if(g.attributes.color)return g;
  const nm=g.attributes.normal,n=nm.count,col=new Float32Array(n*3);
  for(let i=0;i<n;i++){const d=(nm.getX(i)*BL.x+nm.getY(i)*BL.y+nm.getZ(i)*BL.z)*0.5+0.5,sh=0.5+0.5*d;col[i*3]=col[i*3+1]=col[i*3+2]=sh;}
  g.setAttribute('color',new THREE.BufferAttribute(col,3));return g;
}
function rbox(w,h,d,r){ // box with rounded edges and smooth normals
  const k=w+'_'+h+'_'+d+'_'+r;if(RB[k])return RB[k];
  const g=new THREE.BoxGeometry(w,h,d,6,6,6),p=g.attributes.position,nm=g.attributes.normal,v=new THREE.Vector3(),c=new THREE.Vector3(),n=new THREE.Vector3();
  r=Math.min(r,w/2-1e-4,h/2-1e-4,d/2-1e-4);const hx=w/2-r,hy=h/2-r,hz=d/2-r;
  for(let i=0;i<p.count;i++){
    v.fromBufferAttribute(p,i);c.set(clamp(v.x,-hx,hx),clamp(v.y,-hy,hy),clamp(v.z,-hz,hz));n.copy(v).sub(c);
    if(n.lengthSq()>1e-12){n.normalize();v.copy(c).addScaledVector(n,r);p.setXYZ(i,v.x,v.y,v.z);nm.setXYZ(i,n.x,n.y,n.z);}
  }
  return RB[k]=bake(g);
}
function hmat(c){return HM[c]||(HM[c]=new THREE.MeshLambertMaterial({color:c,flatShading:true}));}
function hcyl(a,b,h,s){const k='c'+a+'_'+b+'_'+h+'_'+s;return HG[k]||(HG[k]=bake(new THREE.CylinderGeometry(a,b,h,s||6)));}
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
      if(dx*dx+dz*dz<mr*mr+reach){c.alive=false;c.g.visible=false;c.respawn=25;save.coins+=5;persist();sfxCoin();updateHUDcoins();taskAdd('coins',1);}
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
  if(GD.open){if(e.code==='Escape'||e.code==='Enter'||e.code==='Slash')closeGuide();return;}
  if(e.code==='Slash'){openGuide();return;}
  if(shopOpen)return;
  if(e.code==='KeyP'){matchKey();return;}
  if(!e.repeat&&(e.code==='KeyK'||e.code==='KeyT'||e.code==='KeyY')){toggleAssist(e.code==='KeyK'?'def':e.code==='KeyT'?'aim':'auto');return;}
  if(e.code==='KeyO'&&!MATCH.on){toggleOnline();return;}
  if(tsOpen){if(e.code==='Escape'||e.code==='KeyH')toggleTasks(false);return;}
  if(e.code==='KeyH'&&!e.repeat&&started&&!MATCH.on&&!LB.open&&!shopOpen&&!MV.open&&!(e.target&&e.target.tagName==='INPUT')){toggleTasks(true);return;}
  if(MV.open){const dg=/^Digit([1-3])$/.exec(e.code);if(dg&&MV.ids[+dg[1]-1])mvVote(MV.ids[+dg[1]-1]);else if(e.code==='Escape'){mvClose();openLobby();}return;}
  if(LB.open||(MATCH.on&&MATCH.state==='over')){if(LB.open){if(e.code==='Enter')lobbyPlay();else if(e.code==='Escape')closeLobby();}return;}
  if(MATCH.on){
    const dm=/^Digit([1-4])$/.exec(e.code);
    if(dm){if(WK.on){const l=MATCH.mode==='melee'?MELEEL:ownedList(),w=MATCH.mode==='gungame'?null:l[+dm[1]-1];if(w){WP.cur=w;attachModel();sfxEmpty();}}else{pickVehicle(ORDER[+dm[1]-1]);P.hp=100;HPS[P.type]=100;refreshDamage();}return;} // on foot: weapon slots; driving: switch vehicle
    if(e.code==='KeyE'){if(MATCH.mode==='ffa'||MATCH.mode==='tdm')toggleLoadout(true);return;}
    if(e.code==='KeyV'){MATCH.fpv=!MATCH.fpv;toast(MATCH.fpv?'First-person view':'Over-the-shoulder view',1200);return;}
    if(['KeyF','KeyG','KeyV','Digit5'].indexOf(e.code)>=0)return;
  }
  if(e.code==='KeyR'&&MOON.on){returnToEarth();return;}
  if(e.code==='KeyX'){toggleOnFoot();return;}
  if(e.code==='KeyC'&&WK.on){startSlide();return;}
  if(e.code==='KeyB'&&WK.on){WK.cheerT=2.5;return;}
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
function startGame(){started=true;$('start').style.display='none';initAudio();
  try{if(TOUCH_ON&&!localStorage.getItem('dcs_guide_seen'))setTimeout(openGuide,500);}catch(e){}
  if(TOUCH_ON){try{const el=document.documentElement,f=el.requestFullscreen||el.webkitRequestFullscreen;if(f){const r=f.call(el);if(r&&r.catch)r.catch(function(){});}}catch(e){}try{if(screen.orientation&&screen.orientation.lock)screen.orientation.lock('landscape').catch(function(){});}catch(e){}} // phones: fullscreen + landscape when allowed
  toast('A Diwali Shop is right beside you. Press E to talk to the shopkeeper.',4500);}
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
  add({id:'w_katana',wid:'katana',name:'Katana',done:function(){return'Owned';},info:function(){return W.katana?'Owned':'Melee · long reach slash · 42 damage';},cost:function(){return 120;},can:function(){return !W.katana;},buy:function(){W.katana=1;}});
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
const WK={on:false,x:0,y:0,z:0,h:0,sp:0,ph:0,mesh:null,hum:null,up:0,cheerT:0,jy:0,vy:0,dashT:0,dashDir:0,slideT:0,slideDir:0,slideSp:0,slideCd:0,slAmt:0};
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
function boardVehicle(){MATCH.pitch=0;
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
  const r=0.5,dsh=WK.dashT>0,sl=WK.slideT>0&&!dsh,hdg=dsh?WK.dashDir:(sl?WK.slideDir:WK.h),wsp=dsh?64:(sl?WK.slideSp*(0.25+0.75*WK.slideT/SLIDE_T):WK.sp),sv=(MATCH.on&&!dsh&&!sl)?stf*4.6*(WP.aim?0.5:1):0,wdx=Math.sin(hdg)*wsp*dt-Math.cos(WK.h)*sv*dt,wdz=Math.cos(hdg)*wsp*dt+Math.sin(WK.h)*sv*dt,wl=Math.hypot(wdx,wdz),wn=wl>1.2?clamp(Math.ceil(wl/0.8),1,200):1;
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
  if(WK.slideT>0){ // sliding: the direction follows your aim a little, dust kicks up behind you
    WK.slideT-=dt;WK.slideDir+=angDiff(WK.slideDir,WK.h)*Math.min(1,dt*1.5);
    const fx=Math.sin(WK.slideDir),fz=Math.cos(WK.slideDir);
    if(Math.random()<dt*40)spawnP(WK.x-fx*0.6+rnd(-0.3,0.3),WK.y+0.15,WK.z-fz*0.6+rnd(-0.3,0.3),-fx*rnd(0,2)+rnd(-1,1),rnd(0.3,1.4),-fz*rnd(0,2)+rnd(-1,1),0.75,0.7,0.62,0.6,1.5,0.5,0,1.8);
    if(WK.slideT<=0)WK.slideCd=0.6;
  }else WK.slideCd=Math.max(0,WK.slideCd-dt);
  WK.slAmt+=((WK.slideT>0?1:0)-WK.slAmt)*Math.min(1,dt*14);
  WK.ph+=dt*Math.min(Math.abs(WK.sp),14)*1.8;
  WK.cheerT-=dt;WK.up+=((WK.cheerT>0?1:0)-WK.up)*Math.min(1,dt*9);
  const sw=Math.sin(WK.ph)*0.7*Math.min(1,Math.abs(WK.sp)/3);
  poseHuman(WK.hum,sw,WK.up);
  if(WK.up>0.05){WK.hum.a1.rotation.x=Math.sin(T*9)*0.25*WK.up;WK.hum.a2.rotation.x=-Math.sin(T*9)*0.25*WK.up;}
  WK.mesh.rotation.order='YXZ';WK.mesh.position.set(WK.x,WK.y+0.3-0.12*WK.slAmt+Math.abs(Math.sin(T*8))*0.1*WK.up,WK.z);WK.mesh.rotation.y=WK.h;WK.mesh.rotation.x=-1.05*WK.slAmt;
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
const ORB={yaw:0,pitch:0,t:0}; // free orbit of the chase camera (mouse/trackpad drag) while driving or flying
function updateCamera(dt){
  if(MATCH.on&&WK.on){updateCameraFP(dt);return;}
  if(VM.g&&VM.g.visible)VM.g.visible=false;
  if(MATCH.on&&$('mscope').style.display!=='none')$('mscope').style.display='none';
  const A=actor(),foot=WK.on;
  if(performance.now()-ORB.t>2500||foot){const k=Math.min(1,dt*2.5);ORB.yaw*=1-k;ORB.pitch*=1-k;} // drifts back behind the vehicle when you let go
  camH+=angDiff(camH,A.h+ORB.yaw)*Math.min(1,dt*(ORB.t>performance.now()-150?12:3.5));
  const plane=!foot&&P.type==='plane';
  let td=foot?7:(plane?20:14),th=foot?3.4:(plane?8:6),look=foot?1.8:4,lo=foot?6:8;
  if(camMode===1){look=foot?16:(plane?26:24);lo=6;}
  else if(camMode===2){td+=foot?6:12;th+=foot?3:7;look=foot?1.5:3;}
  if(foot&&WP.aim){td=4.1;th=2.7;look=2.2;lo=30;} // aim down the sights: close behind the shoulder, looking far ahead
  const fovT=(foot&&WP.aim)?((WDEF[WP.cur]&&WDEF[WP.cur].zoom)||62):72;
  if(Math.abs(camera.fov-fovT)>0.05){camera.fov+=(fovT-camera.fov)*Math.min(1,dt*10);camera.updateProjectionMatrix();}
  camDist+=(td-camDist)*Math.min(1,dt*3);camHt+=(th-camHt)*Math.min(1,dt*3);
  const sx=Math.sin(camH),sz=Math.cos(camH);
  let cx=A.x-sx*camDist*Math.cos(ORB.pitch*0.5),cy=A.y+camHt+ORB.pitch*camDist,cz=A.z-sz*camDist*Math.cos(ORB.pitch*0.5);
  if(shake>0){cx+=rnd(-1,1)*shake;cy+=rnd(-1,1)*shake*0.7;cz+=rnd(-1,1)*shake;shake=Math.max(0,shake-dt*1.6);}
  camera.position.set(cx,Math.max(cy,1),cz);
  camera.lookAt(A.x+sx*lo,A.y+look+(camMode===0&&!foot?6:0)+(foot?Math.tan(MATCH.pitch)*lo:0),A.z+sz*lo);
}

/* ================= MULTIPLAYER (same-browser tabs via BroadcastChannel) ================= */
const myId=Math.random().toString(36).slice(2,8),others=new Map();
let bc=null,netTimer=0;
const NET={peer:null,conns:[],role:'',code:'',status:'',gen:0};
const SERVER_CODE='DIWALI'; // the always-the-same server code: the first person to type it opens the server, everyone after joins
function netPeerSend(o,except){NET.conns.forEach(function(c){if(c!==except&&c.open){try{c.send(o);}catch(e){}}});}
function netSend(o){if(kicked||(!bc&&!NET.conns.length))return;try{o.id=myId;if(bc)bc.postMessage(o);netPeerSend(o);}catch(e){}}
function netSendNow(){
  const o={type:P.type,x:P.x,y:P.y,z:P.z,h:P.h,gold:save.up.gold?1:0};
  if(MATCH.on&&MATCH.online&&WK.on){o.m=1;o.wx=WK.x;o.wz=WK.z;o.wh=WK.h;o.ws=WK.sp;o.ww=WP.cur;}
  netSend(o);
}
function makeLabel(text){
  const c=document.createElement('canvas');c.width=256;c.height=64;const g=c.getContext('2d');
  g.fillStyle='rgba(20,10,40,.7)';g.fillRect(0,8,256,48);g.fillStyle='#ffd36b';g.font='bold 30px Arial';g.textAlign='center';g.textBaseline='middle';g.fillText(text,128,33);
  const sp=new THREE.Sprite(new THREE.SpriteMaterial({map:new THREE.CanvasTexture(c),depthTest:false}));sp.scale.set(7,1.75,1);return sp;
}
try{
  bc=new BroadcastChannel('diwali-city-sim-v1');
  bc.onmessage=function(e){netRecv(e.data);};
  window.addEventListener('beforeunload',function(){netSend({bye:1});});
}catch(e){bc=null;}
function netRecv(m){
  {
    if(!m||!m.id||m.id===myId)return;
    if(m.bye){const o=others.get(m.id);if(o){scene.remove(o.v.group);others.delete(m.id);}return;}
    if(m.ch){if(!m.to||m.to===myId)onDuelMsg(m);return;}
    if(m.adm){onAdminMsg(m);return;}
    if(m.fw){after(0,function(){for(let i=0;i<3;i++){const a=Math.random()*TAU,d=rnd(8,30);launchRocket(m.x+Math.sin(a)*d,m.z+Math.cos(a)*d);}});return;}
    let o=others.get(m.id);
    if(!o||o.type!==m.type){
      if(o)scene.remove(o.v.group);
      const v=buildVehicle(m.type,m.gold?0xe0b030:0x40e0d0);const lab=makeLabel('Player '+m.id.slice(0,3));lab.position.set(0,5,0);v.group.add(lab);scene.add(v.group);
      o={v:v,type:m.type,x:m.x,y:m.y,z:m.z,h:m.h,tx:m.x,ty:m.y,tz:m.z,th:m.h,last:T};others.set(m.id,o);
    }
    o.tx=m.x;o.ty=m.y;o.tz=m.z;o.th=m.h;o.last=T;
    if(m.m){o.wm=1;o.wx=m.wx;o.wz=m.wz;o.wh=m.wh;o.ws=m.ws;o.ww=m.ww;o.wlast=T;}else o.wm=0;
  }
}
const NETM={opp:null,east:false};

// --- online play: a room code (PeerJS peer-to-peer). The host's browser is the hub; everything else already flows through netSend/netRecv ---
function netStatus(t){NET.status=t;$('olStat').textContent=t;}
function loadPeerLib(cb){
  if(window.Peer){cb(true);return;}
  const urls=['https://cdnjs.cloudflare.com/ajax/libs/peerjs/1.5.4/peerjs.min.js','https://cdn.jsdelivr.net/npm/peerjs@1.5.4/dist/peerjs.min.js','https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js'];
  let i=0;(function next(){
    if(i>=urls.length){cb(false);return;}
    const sc=document.createElement('script');sc.src=urls[i++];sc.onload=function(){if(window.Peer)cb(true);else next();};sc.onerror=next;document.head.appendChild(sc);
  })();
}
function netReset(){
  NET.gen++;
  NET.conns.forEach(function(c){try{c.close();}catch(e){}});NET.conns=[];
  if(NET.peer){try{NET.peer.destroy();}catch(e){}NET.peer=null;}
  NET.role='';NET.code='';$('olCode').style.display='none';$('olLeave').style.display='none';
  others.forEach(function(o,id){scene.remove(o.v.group);});others.clear();
}
function netWire(c){
  const gen=NET.gen;
  c.on('open',function(){if(gen!==NET.gen)return;
    if(NET.conns.indexOf(c)<0)NET.conns.push(c);
    netStatus(NET.role==='host'?'Hosting room '+NET.code+' · '+NET.conns.length+' connected':'Connected to room '+NET.code);
    $('olLeave').style.display='inline-block';netSendNow();
  });
  c.on('data',function(m){
    if(!m||typeof m!=='object'||!m.id||m.id===myId)return;
    netRecv(m);
    if(NET.role==='host')netPeerSend(m,c); // the host passes everything on to the other guests
  });
  const gone=function(){
    if(gen!==NET.gen)return;
    const k=NET.conns.indexOf(c);if(k>=0)NET.conns.splice(k,1);
    if(NET.role==='host')netStatus('Hosting room '+NET.code+' · '+NET.conns.length+' connected');
    else if(NET.role==='guest'){ // the host left: try to rejoin; whoever gets there first opens the server again, the rest join them
      const code=NET.code;NET.role='';netStatus('Host left - reconnecting to '+code+'...');
      setTimeout(function(){if(!NET.role&&!NET.conns.length)joinRoom(code);},rnd(800,2800));
    }
  };
  c.on('close',gone);c.on('error',gone);
}
function hostRoom(custom){
  custom=typeof custom==='string'?custom:'';
  netReset();netStatus('Starting room...');
  loadPeerLib(function(ok){
    if(!ok){netStatus('Could not load the online library. Check your internet connection.');return;}
    const al='ABCDEFGHJKMNPQRSTUVWXYZ23456789';let tries=0;
    (function open(){
      let code=custom;if(!code)for(let i=0;i<5;i++)code+=al[(Math.random()*al.length)|0];
      const peer=new window.Peer('diwali-'+code);NET.peer=peer;
      peer.on('open',function(){NET.role='host';NET.code=code;$('olCode').textContent=code;$('olCode').style.display='block';$('olLeave').style.display='inline-block';netStatus('Server '+code+' is open · anyone who types '+code+' joins you');});
      peer.on('connection',netWire);
      peer.on('error',function(e){
        if(e&&e.type==='unavailable-id'&&custom){try{peer.destroy();}catch(x){}joinRoom(custom,true);return;} // someone else opened this code a moment ago: join them instead
        if(e&&e.type==='unavailable-id'&&tries++<4){try{peer.destroy();}catch(x){}open();return;}
        netStatus('Online error: '+((e&&e.type)||'unknown'));
      });
    })();
  });
}
function joinRoom(code,noAuto){
  code=String(code||'').toUpperCase().replace(/[^A-Z0-9]/g,'');
  if(code.length<3){netStatus('Type a server code first (3-12 letters or numbers).');return;}
  netReset();netStatus('Joining '+code+'...');
  loadPeerLib(function(ok){
    if(!ok){netStatus('Could not load the online library. Check your internet connection.');return;}
    const peer=new window.Peer();NET.peer=peer;
    peer.on('open',function(){
      NET.role='guest';NET.code=code;
      const c=peer.connect('diwali-'+code,{reliable:true});netWire(c);
      setTimeout(function(){if(NET.role==='guest'&&!NET.conns.length&&!noAuto)hostRoom(code);},9000);
    });
    peer.on('error',function(e){
      if(e&&e.type==='peer-unavailable'){if(!noAuto){try{peer.destroy();}catch(x){}hostRoom(code);}else netStatus('No server found with code '+code);return;} // nobody has this code yet: open it ourselves
      netStatus('Online error: '+((e&&e.type)||'unknown'));
    });
  });
}
// --- duels: challenge another player to 1v1 ... 5v5 (the rest of each team is bots) ---
const CH={pending:null,from:null,n:1};
function challenge(id,n){
  if(MATCH.on){toast('Finish or leave your match first',1800);return;}
  CH.pending=id;CH.n=n;netSend({ch:'inv',to:id,n:n});toast('Duel '+n+'v'+n+' sent to Player '+id.slice(0,3)+'...',2500);
}
function startOnline(id,n,east){
  if(MATCH.on||!started)return;
  $('chal').style.display='none';$('online').style.display='none';olOpen=false;if(LB.open)closeLobby();
  LB.mode='tdm';LB.size=n;NETM.opp=id;NETM.east=!!east;startMatch();
}
function onDuelMsg(m){
  if(m.ch==='inv'){
    if(MATCH.on||!started||MOON.on||SPACE.busy||CH.from){netSend({ch:'busy',to:m.id});return;}
    CH.from=m.id;CH.n=Math.max(1,Math.min(5,m.n|0));
    $('chalTxt').textContent='Player '+m.id.slice(0,3)+' challenges you to a '+CH.n+'v'+CH.n+' duel!';$('chal').style.display='flex';
  }else if(m.ch==='acc'){if(CH.pending===m.id){CH.pending=null;startOnline(m.id,CH.n,false);}}
  else if(m.ch==='dec'){if(CH.pending===m.id){CH.pending=null;toast('Player '+m.id.slice(0,3)+' declined the duel',2500);}}
  else if(m.ch==='busy'){if(CH.pending===m.id){CH.pending=null;toast('Player '+m.id.slice(0,3)+' is busy',2500);}}
  else if(m.ch==='dmg'){
    if(!MATCH.on||!MATCH.online||MATCH.opp!==m.id)return;
    const b=MATCH.bots.find(function(x){return x.rid===m.id;});if(b)hurtPlayer(Math.min(100,+m.d||0),b);
  }else if(m.ch==='died'){
    if(!MATCH.on||!MATCH.online||MATCH.opp!==m.id)return;
    const b=MATCH.bots.find(function(x){return x.rid===m.id;});if(b)remoteKill(b);
  }else if(m.ch==='left'){
    if(MATCH.on&&MATCH.online&&MATCH.opp===m.id&&MATCH.state!=='over'){toast('Your opponent left - you win!',2500);MATCH.hk=MATCH.target;scoreCheck();}
  }
}
function remoteKill(b){
  if(MATCH.state!=='live')return;
  b.ko=2.8;b.g.rotation.order='YXZ';MATCH.kills++;MATCH.hk++;taskAdd('kills',1);addFeed('You','eliminated',b.name,WNAMES[WP.lastUsed]||'Weapon',true);sfxDing();scoreCheck();
}
let olOpen=false,olKey='';
function renderOnline(force){
  const ids=Array.from(others.keys()),key=ids.join(',')+'|'+NET.status;
  if(!force&&key===olKey)return;olKey=key;
  const box=$('olList');box.innerHTML='';
  if(!ids.length){box.textContent='No other players yet. Share your room code, or join a friend.';return;}
  ids.forEach(function(id){
    const row=document.createElement('div');row.style.margin='4px 0';
    const nm=document.createElement('b');nm.textContent='Player '+id.slice(0,3)+'  ';row.appendChild(nm);
    for(let n=1;n<=5;n++){const b=document.createElement('button');b.textContent=n+'v'+n;b.style.cssText='margin:2px;padding:5px 9px;font-size:13px';b.addEventListener('click',function(){challenge(id,n);});row.appendChild(b);}
    box.appendChild(row);
  });
}
function toggleOnline(v){
  if(!started||MATCH.on)return;
  olOpen=(v===undefined)?!olOpen:!!v;
  if(olOpen){for(const k in keys)keys[k]=false;if(LB.open)closeLobby();renderOnline(true);}
  $('online').style.display=olOpen?'flex':'none';
}
$('olHost').addEventListener('click',function(){hostRoom();});
$('olServer').addEventListener('click',function(){$('olIn').value=SERVER_CODE;joinRoom(SERVER_CODE);});
$('olJoin').addEventListener('click',function(){joinRoom($('olIn').value);});
$('olLeave').addEventListener('click',function(){netReset();netStatus('Disconnected.');});
$('olClose').addEventListener('click',function(){toggleOnline(false);});
$('lbOnline').addEventListener('click',function(){toggleOnline(true);});
$('chalYes').addEventListener('click',function(){const id=CH.from;CH.from=null;if(id){netSend({ch:'acc',to:id,n:CH.n});startOnline(id,CH.n,true);}});
$('chalNo').addEventListener('click',function(){const id=CH.from;CH.from=null;$('chal').style.display='none';if(id)netSend({ch:'dec',to:id});});
function updateNet(dt){
  netTimer-=dt;if(netTimer<=0){netTimer=0.1;netSendNow();}
  others.forEach(function(o,id){
    if(T-o.last>3){scene.remove(o.v.group);others.delete(id);return;}
    const k=Math.min(1,dt*8);o.x+=(o.tx-o.x)*k;o.y+=(o.ty-o.y)*k;o.z+=(o.tz-o.z)*k;o.h+=angDiff(o.h,o.th)*k;
    o.v.group.position.set(o.x,o.y,o.z);o.v.group.rotation.y=o.h;
    if(o.v.prop)o.v.prop.rotation.z+=dt*30;
  });
  $('players').textContent=1+others.size;
  if(olOpen)renderOnline(false);
}

/* ================= WEAPONS (held in the hands of the man on foot) ================= */
const WDEF={
  rifle:{dmg:6.6,rate:0.09,range:75,spreadHip:0.06,spreadAds:0.008,auto:true,mag:100,zoom:46},          // Assault Rifle: 100 bullets, 6.6 per bullet, full auto
  shorty:{dmg:11,pellets:6,rate:0.95,range:24,spreadHip:0.12,spreadAds:0.07,auto:true,mag:30,zoom:56}, // Shorty: 30 bullets, 6 pellets per shot
  pistol:{dmg:14,rate:0.24,range:60,spreadHip:0.035,spreadAds:0.006,auto:false,mag:48,zoom:58},        // Pistol: 48 bullets, semi-auto
  smg:{dmg:4.2,rate:0.055,range:45,spreadHip:0.09,spreadAds:0.03,auto:true,mag:120,zoom:60},           // SMG: 120 bullets, very fast fire
  sniper:{dmg:55,rate:1.1,range:180,spreadHip:0.18,spreadAds:0,auto:false,mag:12,zoom:20},             // Sniper: 12 bullets, 55 damage, scope
  katana:{dmg:42,range:3.6,rate:0.55},
  knife:{dmg:22,range:2.8,rate:0.32},                                                                  // Knife: very fast melee
  rocket:{dmg:90,radius:7,rate:1.5,speed:40,mag:4,zoom:62,range:130},                                  // Rocket Launcher: straight-flying rocket
  glaunch:{dmg:60,radius:5.5,rate:0.9,speed:22,mag:6,zoom:62},                                         // Grenade Launcher (RPG): arcing shell
  scythe:{dmg:35,range:3.7,rate:0.55,dashDmg:25},                             // Scythe: melee swing + dash
  grenade:{dmg:75,radius:6.5,rate:0.8,zoom:62}                                        // Grenade: 75 damage to people in the blast
};
const HEADMUL={rifle:2,smg:1.8,pistol:2.5,shorty:1.5,sniper:3.5}; // headshot damage multipliers
const WNAMES={rifle:'Assault Rifle',shorty:'Shorty',scythe:'Scythe',grenade:'Grenade',pistol:'Pistol',smg:'SMG',sniper:'Sniper',knife:'Knife',katana:'Katana',rocket:'Rocket Launcher',glaunch:'Grenade Launcher',vehicle:'Vehicle'};
const GUNS=['rifle','shorty','pistol','smg','sniper'],LAUNCHERS=['rocket','glaunch'],MELEE=['scythe','knife','katana'];
function canAim(id){return GUNS.indexOf(id)>=0||LAUNCHERS.indexOf(id)>=0||id==='grenade';}
const WP={cur:null,cd:0,fire:false,aimMouse:false,aim:false,swingT:0,swingHit:false,dashCd:0,dashHit:new Set(),recoil:0,throwT:0,hitFlash:0,dirty:0,model:null,modelFor:null,barKey:'',crossKey:''};
function owns(id){const w=save.wp;return id==='grenade'?w.nades>0:(w[id]|0)>0;}
function ownedAll(){return['rifle','smg','sniper','shorty','pistol','scythe','knife','katana','grenade','rocket','glaunch'].filter(owns);}
function inLoad(id){const q=save.wp.slots;return q.primary===id||q.secondary===id||q.melee===id||q.utility===id;}
function slotOf(id){const q=save.wp.slots;return q.primary===id?'PRIMARY':q.secondary===id?'SECONDARY':q.melee===id?'MELEE':'UTILITY';}
function autoSlot(id){const q=save.wp.slots,sl={rifle:'primary',smg:'primary',sniper:'primary',shorty:'secondary',pistol:'secondary',scythe:'melee',knife:'melee',katana:'melee',grenade:'utility',rocket:'utility',glaunch:'utility'}[id];if(!sl)return;if(!q[sl]||!owns(q[sl])||q[sl]===id)q[sl]=id;}
function ownedList(){if(MATCH.on&&MATCH.mode==='melee')return MELEEL.slice();const q=save.wp.slots;return[q.primary,q.secondary,q.melee,q.utility].filter(function(id,i,a){return id&&owns(id)&&a.indexOf(id)===i;});} // only the weapons chosen in your loadout
function eachTarget(fn){if(MATCH.on){const b=MATCH.bots;for(let i=0;i<b.length;i++)if(b[i].g.visible&&!(MATCH.mode==='tdm'&&b[i].team==='A'))fn(b[i]);return;}for(let i=0;i<people.length;i++)fn(people[i]);for(let i=0;i<crowd.length;i++)fn(crowd[i]);}

// --- the weapon in the man's hand (child of his right forearm: barrel runs along the forearm) ---
// --- real 3D weapon models (CC-BY, poly.pizza): Quaternius (pistol, knife), Zsky (sniper), Qubzy (scythe), dook (katana), skony_3d (grenade), CreativeTrio (grenade launcher) ---
const GLBW={"pistol":{"s":8.569597460860012e-06,"p":"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","i":"UQPwAVMDVANRA1MDVANSA1EDUgMwAy8DMANSA1QDLwP3AvYCaQL3AmoC9wIvAzAD9wJpAvYCagLaAWkCUgPZAVEDLwPZAVIDaQIvA/YCOwA9APABOwDwAdkB2gFqAvEB2gEnASYBOwA+AD0APgA7ADwAPgA8AGEAYABhADwAYQBgAJoAmQCaAGAAJgGaAJkAmgAmAScBJwHaAfEBYAAmAZkA2gHZAWkC2QEvA2kC2QFgADwA2QE8ADsAJgFgANkBJgHZAdoB8AFRA9kB3AIjAx8DwQCGAIMBhgDBAIIAgwEjAxoCIwPcAhoCIwODAYYAdwFyALUAtQByAG4AcgAOAg8DDgJyAHcBDwMOAtACDwPQAgsDIwOGACIDhQAiA4YAIgOFACEDhAAhA4UAIQOEACADgwAgA4QAIAOBAB4DgQAgA4MAHgOBABwDfwAcA4EAHAN/ABoDfQAaA38AGgN9ABgDewAYA30AGAN7ABYDeQAWA3sAFgN5ABQDdwAUA3kAFAN3ABIDcgAPA3UAdQASA3cAEgN1AA8DbgB1AG8AhAB8AIMAfgCEAIAAfQB0AHsAdgB9AHgAcAC4ALcAcAC2AG8AdgC6AHQAdgC8ALsAfgDAAL8AfgC+AHwAtwB4AbYAtwB6AXkBuwB8AboAuwB+AX0BvwCCAYEBvwCAAb4AgQEXAoABfQETAnwBDQPTAg4DEwPXAhUDGwPbAh0D0gIRAtMC0gINA9ECFAJ+ARUC1gITA9UC1gIVAtcCGAKCARkC2gIbA9kC2gIZAtsCdQBuAHIAcAB1AHEAcABvAHUAIQMdAyIDIQMZAxsDGgMRAxMDGgMVAxwDfACEAH4AgwB8AHoAeAC8AHYAeAC9ALwAcQC5ALgAcQC4AHAAcQB3AHMAcQB1AHcAdAB9AHYAewBzAHkAcwB3AHkAcwB7AHQAegB/AIEAegCBAIMAeAB/AHoAeAB9AH8AggCFAIYAgACFAIIAgACEAIUAgADAAH4AgADBAMAAeAG3AHkBtgBwALcAtQBvALYAtQBuAG8AdwG1ALYAdwG2AHgBuQBxAHMAuAB6AbcAuAB7AXoBugB2ALsAuQB0ALoAuQBzAHQAvQB4AHoAvAB+AbsAvAB/AX4BwQCAAIIAwACCAb8AwACDAYIBgAG/AIEBfwG+AIABfwG9AL4AgwHAAMEAgwEaAhkCggGDARkCggEYAoEBfwEWAhUCfwG8AL0AfgEUAn0BfgF/ARUCfAG7AH0BewG5ALoAewG6AHwBewESAhECewG4ALkAGQPYAtkCGQPZAhsDDAPQAtECDAPRAg0DEQPVAhMDEQPUAtUCDwJ5ARACDwIQAtIC0AIMAwsD0QIOAg8C0QIPAtICDgJ4AQ8CDgJ3AXgBDgLRAtAC1AIQAw4D0wINA9IC0wLUAg4DEgLUAtMCEwJ9ARQCEwIUAtYC1AIRAxAD1QISAhMC1QITAtYCEgLVAtQCEgJ7AXwBEgJ8ARMC2AIXAxUD1wITA9YC1wLYAhUDFgLYAtcCFQIWAtcCFQLWAhQC3AIfAx0D2wIbA9oC2wLcAh0DGgLcAtsCGQLaAhgCGQIaAtsCFwIYAtoCFwKBARgC2AIZAxcD2QIXAtoC2QIWAhcCFgJ/AYABFgKAARcCFgLZAtgCGQMhAyADIAMXAxkDHQMhAxsDHwMjAyIDHwMiAx0DFQMaAxMDFwMcAxUDFwMeAxwDFwMgAx4DEQMaAxgDGAMQAxEDDgMSAw0DEAMYAxYDEAMUAw4DEAMWAxQDvgB+AL8AvQB6AHwAvQB8AL4ADAMNAxIDCwMSAw8DEgMOAxQDEgMLAwwDEQISAtMCEQLSAhACeQEPAngBEAJ6ARECegEQAnkBegF7ARECRABIAEcAawBpAK0ArQCsAHEBIgIhAuYCTwNMA04DSwNOA0wD5QLmAiEC5QInA+YCJwMlA00DJwPlAiUDJwNNA1ADIQIiAq8BrACtAGkAaQBrAEYAUAPHAScDJwPHASICJwMiAuYCTwPHAVADSABEAEUASQBGAGsAcQFrAK0ASADGAUcAxgFPA04DxgFIAMcBSQDHAUgAxwFPA8YBxwFrAHEBawDHAUkAcAFxAawAxwGvASICrwFxAXABcQGvAccBcgNxAy4DqgCLALQAcQM/Ay0DPwMGAy0DPwP0AgYD9AI8At0C3QI8AhsC3QIGA/QCLgNxAy0DZAA5ADoAtABqAaoAiwCqAF0AOQBkAGMAXQBjAIsAYwBdADkANgAzADQALgArACwAJAAhACIAJgAjACQANQA3AFwANQBcAFsANQBaADMANQA4ADcALQBWACsALQAvAFgALQAwAC8ALQBYAFcAJQBUAFMAJQAoACcAJQAnAFQAJQBSACMAUwChAKAAUwCfAFIAVwClAKQAVwCjAFYAWwCpAKgAWwCnAFoAoABhAWABoABgAZ8ApABkAaMApABlAWQBqABpAWgBqABoAacAaAE7AjoCZAE3AjYCNQPrAjYDNQPpAuoCNQM0A+kCOQM4A+0COQPtAu4C7gI6AzkDNgLvAu4CNgLuAjUCNgJjAWQB8gI9A/EC8gI+Az0DOgJnAWgBOgLyAjkCOgLzAvICbQNrAzwDbQM8Az0DbQM+A28DbgNrA20DbgNtA3ADZQM6A2cDZQM4AzkDZQNjAzgDZgNjA2UDZgNlA2gDXQNbAzQDXQM0AzUDXQM2A18DXgNbA10DXgNdA2ADXANbA14DXANZA1sDUgAlAFMAIwBRACEAIwAmACUAOAA1ADYAOgA3ADgANAAxADIAMAAtAC4AMgAvADAALAApACoAKAAlACYAKgAnACgAIgAfACAAXACqAKkAXACpAFsAWQBaAKYAWQCmAKUAWAClAFcAWABZAKUAVQBWAKIAVAChAFMAVACiAKEAIQAkACMAHwBRAFAAHwAiACEAHwAhAFEAVAAnAFUAJwAqACkAVgAtAFcAKQBVACcAKQAsACsAVQArAFYAKwBVACkAKwAuAC0AWAAvAFkALwAyADEAWgA1AFsAMQA0ADMAMQBZAC8AWQAzAFoAMwA2ADUAMwBZADEAXAA3AF0ANwA6ADkAOQBdADcAXwEyAjECXwExAjACnwBgAV8BnwBfAV4BnwCeAFIAnwBTAKAAogBiAWEBogBUAFUAoQCiAGEBowBjAWIBowCiAFYAowBkAWMBowBXAKQAogCjAGIBpgBmAWUBpQBlAaQApQCmAGUBpwBoAWcBpwCmAFoApwBnAWYBpwBbAKgApgCnAGYBqgBqAWkBqgBcAF0AqQCqAGkBqQBpAagAagG0AHYBPAJqAXYBPAJ2ARsCagE8AjsCaQE7AmgBaQFqATsCZwE5AjgCZwE6AjkCZgFnATgCZgE4AjcCZQE3AmQBZQFmATcCYwE1AjQCYwE2AjUCYgFjATQCNgPrAuwCNgPsAjcDOgPwAjsDOgPvAvACPgP0Aj8DPgPzAvQCNgNdAzUDNwNjA2EDNAPoAukCOgNlAzkDOwNrA2kDOAPsAu0CPgNtAz0DPAPwAvECMQLpAugCMQLqAukC6wI1A+oC7AI4AzcDNQLuAu0CNQLtAuwCNAI1AuwC7wI6A+4COALwAu8CNwI4Au8CNwLvAjYC8QI9AzwD8AI8AzsDOQLyAvECOQLxAvACOAI5AvAC8wI+A/ICPAL0AvMCOwLzAjoCOwI8AvMCNALsAusCYgE0AjMCWwMzAzQDcANtA28DbwNyA3ADbwM/A3EDbwM+Az8DcgNvA3EDawM7AzwDbANpA2sDbANrA24DaANlA2cDaQNsA2oDZwM7A2kDZwNqA2gDZwM6AzsDagNnA2kDYwM3AzgDZANhA2MDZANjA2YDYANdA18DYQNkA2IDXwM3A2EDXwM2AzcDXwNiA2ADYgNfA2EDWANXA1kDWgNYA1kD6AIyA+cC6ALnAjAC6AI0AzMDMgPoAjMDWQNcA1oDMwNbA1kDMwNXAzIDVwMzA1kDUABRAJ0AUQBSAJ4AUQAjAFIAnQCeAF0BngCdAFEAngCfAF4BXQGxAJ0AIAAfAE4ALwIwAucCMAIxAugCXgFdAZ4AXgFfATACXQFeATACXQEwAi8CnQCKAFAALwJ1AV0BLwLnAuACVwMHAzIDWANCA1cD5wIyAwcD5wIHA+ACBwNXA0IDsQBdAXUBdQEvAhwCQgNYA0MD4AIcAi8CTgBPACAAigCdALEAHwCKAE4AHwBQAIoAMgLrAuoCMgLqAjECYAEyAl8BYAEzAjICMwLrAjICMwI0AusCYQEzAmABYQGgAKEAYQFiATMCowEnAkECSQERAR4BSQGPAaMBjwFJAVUB+gDLAOEA7AAjARYBkAFWAUoBpAEoApABKAKkAUICKAJ6Am0CfQKZAnACfQKnApkCcAIrAkUCHgFVAUkBEQH1AB4BEQHnAPUAIwHsAPoAFgFaAU4BegIoAkICbQKkApYCRQJ9AnACKwKTAacB5wDfAPUAWgEWASMBTgGUAagBHgFZAVUBIgFZAR4BcQIsAm0CcQKWApoCbQKWAnECbQIsAigC9QAiAR4B+QAiAfUAsQKVApkCmgKvArECrwKVArEClgKvApoC3wD5APUA4QD5AN8AmQJsAnAClQJsApkC9gDhAN8A+gDhAPYAHwH6APYAIwH6AB8BJwJwAmwCKwJwAicCVgEjAR8BWgEjAVYBjwErAicCVQGTAY8BkwErAo8BWQGTAVUBkAFaAVYBKAKUAZABlAFaAZABLAKUASgCSgGkAZABVgEfARIBxwKaArECpwKxApkCsQKnAscCJwKjAY8BQQJsAnkCywD6AOwA4QDrAPkAEgFKAVYBHwH2AOgAqAJ+AnECmgLHAqgCbAJBAicCeQKVAqMC6wDhAMsA+QAVASIB6AASAR8B3wDnAMkA9gDJAOgAyQD2AN8AlAFOAVoBqAEsAkYCfgJGAiwCcQKaAqgCLAKoAZQBLAJxAn4CpAJtAnoClgLFAq8CpwFFAisCkwFNAacBFQH5AOsAIgFNAVkBTQGTAVkBTQEiARUBxQKWAqQClQJ5AmwCowKvAsUCrwKjApUCFQHvABkBFQFRAU0BFQEZAVEBfgKCAkoCfgJKAkYCggKoAqwCggJ+AqgC7wAVAesA6wDNAO8AyQKnAqsCyQLHAqcCrALHAskCrAKoAscCzQDrAMsAqwJ9AoECqwKnAn0C7ADNAMsA7ADwAM0AfQJJAoECFgEaAfAAFgHwAOwAFgFSARoBSQJ9AkUCRQK2AUkCUgEWAU4BTgG3AVIBtgFFAqcBTQFRAbYBTQG2AacBtwFOAagBRgJKArcBRgK3AagBkgKdAsMCoAJmApICZgKgAnYCZgI+AiQCPgJmAnYCJAKgAZoBoAEkAj4CoAFsAZoBoAFGAWwBRgEOASoBKgFsAUYBKgHkAP4A5AAqAQ4B/gDHAPMAxwDjAP0AxwD9APMAxwD+AOQA4wApAf0A4wANASkBDQFFAWsBawEpAQ0BawGfAZcBnwFrAUUBnwEjApcBIwKfAT0CIwJ1AmUCdQIjAj0CZQKfApECwwKgApICnwJlAnUCnwKdApECnwLDAp0CXQIJAgUCXQJhAgkCCgEBAQMBCgEGAQEBhQJhAl0ChQKJAmECjQKJAoUCjQKPAokCAwEFAQkBAwEBAQUBhgKPAo0ChgKKAo8CCQEtATEBCQEFAS0BXgJiAooCXgIKAmICXgKKAoYCCgJeAgYCBgKxAQoChQExAS0BhQGJATEBsQEGApkBmQGKAbEBmAGJAYUBmAGwAYkBBQKwAZgBBQIJArABoAFKAUYBoAGkAUoBnwFBAj0CnwGjAUECRgESAQ4BRgFKARIBPQJ5AnUCPQJBAnkCDgESAegADgHoAOQA5ADoAMkA5ADJAMcAowJ1AnkCxQKfAqMCnwJ1AqMCwwKfAsUCxwDnAOMAxwDJAOcApALDAsUCoALDAqQC4wDnABEB4wARAQ0BPgJCAqQBPgKkAaABegKgAqQCdgKgAnoCdgJCAj4CdgJ6AkICigGZAYYBhgEyAYoBMgEuAQYBMgEGAQoBMgGGAS4BXgIkAgYCXgKSAmYCZgIkAl4CRQGjAZ8BRQFJAaMBDQFJAUUBDQERAUkBJAKaAQYChQEpAWsBawGYAYUBmgGGAZkBmgGZAQYCmAEjAgUCmAFrAZcBhgGaAWwBbAEuAYYBBQJlAl0CIwJlAgUCIwKYAZcBLgH+AAYBLgFsASoBZQKRAl0CkQKNAoUChQJdApECkgJeAoYCkgKNAp0CjQKRAp0CjQKSAoYC/gABAQYB/gAuASoBAQH+APMA8wD9AAEB/QAFAQEB/QApAQUBKQGFAS0BLQEFASkBpQEpAkMCSwGRAaUBSwETASABkQFLAVcB/ADMAOIA7gAlARgBkgFYAUwBpgEqApIBKgJ8Am8CKgKmAUQCfwKbAnICfwKpApsCcgItAkcCIAFXAUsBEwHpAPcAEwH3ACABJQHuAPwAGAFcAVABfAIqAkQCbwKmApgCRwJ/AnICLQKVAakB6QDgAPcAXAEYASUBUAGWAaoBIAFbAVcBJAFbASABbwIuAioCcwIuAm8CcwKYApwC9wAkASAB+wAkAfcAsgKXApsCnAKwArICsAKXArICmAKwApwCmAJzAm8C4AD7APcA4gD7AOAAmwJuAnIClwJuApsC+ADiAOAA/ADiAPgAIQH8APgAJQH8ACEBKQJyAm4CLQJyAikCWAElASEBXAElAVgBkQEtAikCVwGVAZEBlQEtApEBWwGVAVcBkgFcAVgBKgKWAZIBlgFcAZIBLgKWASoCTAGmAZIBWAEhARQByAKcArICqQKyApsCsgKpAsgCKQKlAZEBQwJuAnsCzAD8AO4A4gDtAPsAFAFMAVgBIQH4AOoAqgKAAnMCnALIAqoCbgJDAikCewKXAqUC7QDiAMwA+wAXASQB6gAUASEB4ADpAMoA+ADKAOoAygD4AOAAlgFQAVwBqgEuAkgCgAJIAi4CcwKcAqoCLgJzAoACLgKqAZYBpgJvAnwCmALGArACqQFHAi0ClQFPAakBFwH7AO0AJAFPAVsBTwEkARcBTwGVAVsBxgKYAqYClwJ7Am4CpQKwAsYCsAKlApcCFwFTAU8BFwEbAVMBFwHxABsBgAKEAkwCgAJMAkgChAKqAq4C8QAXAe0A7QDOAPEAygKpAq0CygLIAqkCqgKEAoACrgKqAsgCrgLIAsoCzgDtAMwArQJ/AoMC7gDyAM4A7gDOAMwAfwJLAoMCfwKtAqkCGAEcAfIAGAHyAO4AGAFUARwBSwJ/AkcCRwK4AUsCVAEYAVABUAG5AVQBuAFHAqkBTwG4AakBTwFTAbgBuQFQAaoBSAJMArkBSAK5AaoBogKeAsQClAJ4AmgCeAKUAqICaAJAAiYCQAJoAngCJgKiAZ4BogEmAkACogFuAZ4BogFIAW4BSAEQASwBLAFuAUgBAAEQAeYAAAEsARAB5gD0AAAB9ADmAMgAyAD/APQAyADlAP8A5QArAf8A5QAPASsBKwFHAW0BRwErAQ8BbQGhAZsBoQFtAUcBoQElApsBJQKhAT8CPwJnAiUCZwI/AncCdwKTAmcCdwKhApMCngKiApQCoQKeApMCoQLEAp4CXwJjAgsCXwILAgcCDAECAQQBDAEIAQIBhwJjAl8ChwKLAmMCjgKLAocCiwKOApACBAECAQcBBAEHAQsBiAKMApACiAKQAo4CCwEvATMBCwEHAS8BYAKMAogCYAJkAowCYAIMAmQCDAJgAggCCAKzAQwChwEzAS8BhwGLATMBswEIAp0BnQGMAbMBnAGLAYcBnAGyAYsBBwILArIBBwKyAZwBogGmAUwBogFMAUgBoQFDAj8CoQGlAUMCSAEUARABSAFMARQBPwJDAnsCPwJ7AncCEAEUAeoAEAHqAOYA5gDqAMoA5gDKAMgApQJ3AnsCxgKhAqUCoQJ3AqUCxAKhAsYCyADpAOUAyADKAOkApgLEAsYCogLEAqYC5QATAQ8B5QDpABMBQAKmAaIBQAJEAqYBfAKiAqYCeAJ8AkQCeAJEAkACeAKiAnwCjAGdAYgBiAE0AYwBNAEIAQwBNAGIATABNAEwAQgBaAIIAmACaAKIApQCRwFLAaUBRwGlAaEBDwFLAUcBDwETAUsBCAJoAiYCJgKeAQgCbQEvASsBbQGcAYcBngGdAQgCngGIAZ0BnAFtAZsBnAElAgcCiAGeAW4BiAEsATABJQKcAZsBJQJfAgcCLAGIAW4BLAEIATABZwKTAl8CkwKeAocCXwIlAmcChwKeAo4ChwJfApMCiAJoAmAClAKOAp4CjgKUAogCCAEsAQABAAECAQgBAgEAAfQA9AD/AAIB/wAHAQIB/wArAQcBLwFtAYcBLwEHASsBkQP5AfoBfwO/Ab4BgAPAAb8BgQPAAYADgQPBAcABggPBAYEDgwPEAcMBgwPCAYIDgwPDAcIBigOHA4kDiQOHA4YDKQMoA3QDKQN0A3UDKQPiAigDigPdAYsDjQOIA4cDjQOHA4wDdQN0A4gDdQOIA40DiQPLAYoDQQP2AQgDdAL2AfcBkQP6AY8D9QL2AXQCkQOPA44DkQOOA5ADjgOMA4sDjAOKA4sDjAOOA48DegN5A3gDegN9A3sDHwLiAuMCiAOFA4QDiAOEA4cDhQN0A3MDdAOFA4gDKAPhAiYDKAMmA3MDKANzA3QDHQIeAskBHQLJAcUBHgIfAt8BHgLfAckB4gIpA+MC4gIfAh4C4QIeAh0C4QIoA+IC4QLiAh4CegN4A7oBfQN+A70BfQO9AbsBewN9A7sBdQPeASkDjQPeAXUDHwLeAd8B4wLeAR8CdwOrAXYDcwPEAYUDhQPEAYQDJgPEAXMDHQLEAeECeQO1AawBeQN7A7UBdwN5A6wBdwN2A3gDeQN6A3sDeQN3A3gDiwPyAY4DjgP5AZADhgPKAYkDfAPIAYYDfAN9A3oDfgN9A3wDfgOGA38DgQOHA4IDhwODA4IDhwOEA4MDhwOKA4wDhgOAA38DhgOBA4ADhgOHA4EDhgN+A3wDjwP6AfMBjAPzAd4BjAOPA/MBGwD5ARoA+QGRA5AD+QEbAPoBvwEJAL4BvwF/A4ADvwEKAAkAwAEKAL8BCgDAAQsAwQGCA8IBwQELAMABCwDBAQwAwwENAMIBwgEMAMEBDADCAQ0AFAARABYAbQCvALAA3QGKA8sBywGJA8oBFAAVAN0BFADdAcsBFADLARMAHgAdAG0AFwASAB4AFwARABIAygGGA8gBEwDKARAAEwDLAcoBXwD2AV4A9gFBA0AD9gH1AggD9gEdAfcB9gGcAB0B+gEZAPMBiQD2AV8AnAD2AYkAGwAYABkAGAAbABoAGAAWABkAFgAYABUAFgAVABQABQAHAAQAdAGvAHMByQFyAcUB3wFzAckBEgAOAA8AHAAdAA8AHQAcAGwAHQAeABIAHQASAA8AagBsABwAbABtAB0AbACuAK8ArwB0AbAArwBtAGwArgBsAGoAcwGvAK4AcwHfAXQBcgFzAa4AcgHJAXMBugG8AXoDBAC8AboBBAAGALwBvgG9AX4DvgEJAAgAvgF+A38DvQEIAAcAvQG+AQgAuwEHAAUAuwG1AXsDuwG9AQcAbQDeAR4AFwDeARYAHgDeARcA3gGwAHQB3gGNA4wD3gHjAikD3gF0Ad8B3gFtALAAqwEBAAAAqwF3A6wBAQCrAawBrgDEAXIBagDEAa4AxQHEAR0CxAEPAA4AxAEcAA8AxAFqABwAxAEOAA0AxAENAMMBxAEmA+ECxAGDA4QDcgHEAcUBtQEFAAMAtQG7AQUArAEDAAEArAG1AQMAAQACAAAAAwACAAEAAwAFAAQAAwAEAAIA8gGLA90B8gEVABgAFQDyAd0B+QEYABoA+QGOA/IBGAD5AfIBqwG0AXYDtAG6AXgDtAF4A3YDAgC6AbQBAgAEALoBAAC0AasBAAACALQBvAF8A3oDyAF8A7wBEADKAcgBBgDIAbwBBgAQAMgBDgASABEACgAQAAkABwAGAAQABgAHAAgACAAQAAYAEQAMAA0AEQALAAwAEQANAA4AEQAXABYAEQAUABMACwAQAAoAEAAIAAkAEAARABMAEAALABEAGQD6ARsAFgDzARkA8wEWAN4B/gIAA/8CAAMEAwIDAgP/AgAD/wL6AvkC+gL/AgID+QLMAssCzAL5AvoCzAJbAloCWQL7AeYBWgJbAv8BWgLLAswCywJaAlkCAQP+AQMDAwMEAwEDzQJcAlsCzgLNAvwCzQLMAvsCBAMAAwEDBAMDAwUDBAP6AgIDBQP7AgQD+wLMAvoC+wL6AgQD/ALNAvsC/AL7AgUDXALNAs4CWwLMAs0CWwJcAgACAAP0AQED/gLtAQADjgCSAI8AjgCNAJAAjgCMAI0A+wH/ATYB+wFZAloC5gE2ATUBjQCWAJAAlgCNAJUAlQDEAJYAxACVAMMAxAA3AcUANgHDADUBNgH/ATcBNgHmAfsBwwA2AcQA/gEBA/QB9AEAA+0B9AGRAP4BjwCRAPQBkQCSAJMAxQA4AcYAmADFAMYAmACXAMUAkgCOAJAAkgCQAJYAkgCWAJcAkgCRAI8AkwCXAJgAlwDEAMUAlwCWAMQAlwCTAJIAOAHFADcB/wH7AVoCNwEAAjgBNwHEADYBAAL/AVsCAAI3Af8B7QGPAPQBjgDXAYwAjgCPAO0B1wGOAO0B1wHtAf4CuQK7AtQBuQLUAdIBuwK6AtMBswLNAcwB1QG8AtYBvQLWAbwCwgLBAu4BuALSAdEBvgLcAdgBwALcAb4CtwLRAdABtgLQAc8BugK8AtUBwgLAAsECvwLBAsACswK/ArQCuwK5ArgCuwK4AroCugK4ArcCugK3ArwCvgK1ArQCwAK0Ar8CtALAAr4CvQK2ArUCtQK+Ar0CvAK2Ar0CvAK3ArYCwALCAu8BwALvAdwBvQLYAdYBvgLYAb0CtQLPAc4BtALOAc0BzwDbAMwBzAG/ArMCzAHbAb8C1QDUAdcA1AHVANIB1wDTAdYA1AG7AtMB1AHTAdcA2ADVAdYB2ADWAdkA3gDuAd0A7wHCAu4B7wHuAd4A0QHTANAB0QG3ArgC0QHUANMA0gHVANQA0gHUANEB0gG4ArkC2gDcAdwA3AHaANgBzwHRAM4BzwHSANEAzwG1ArYC0AG2ArcC0AHSAM8B0AHTANIA2wDdANsB2wHuAcEC2wHBAr8C2wHMAdsA7gHbAd0A1gDVAdgA0wHVAdYA0wG6AtUB3QDcAN4A3ADdANsA0ADbAM8A0ADcANsA1QDXANQA1ADWANMA1gDUANcA2ADTANYA0QDaANAA3ADQANoA0gDZANEA2gDRANkA0wDYANIA2QDSANgA3ADvAd4A7wHcANwB2QDYAdoA2AHZANYBzQGzArQCzQHPAMwBzQHQAM8AzgG0ArUCzgHRANAAzgHQAM0BDQL9As8C/QKtASwDLANmACsDrQH9Ag0CrQFnACwDrQGUAGcAZgAsA2cAKwNlACoDZQArA2YAlACEAcIAlACtAYQBOgByA2QAZAByAy4DBgMbAi0DZAAtA2MAZAAuAy0DiwB2AbQAYwB2AYsAGwIGA90CdgEtAxsCdgFjAC0DsQBOAIoAdQFCA04AdQFOALEAQwNOAEIDQwNPAE4AQgN1ARwCQgPgAgcD4AJCAxwC+AIwAzED+AL3AjADMQNUA1YDVgNVA0ADawL3AfgBQAP1AfYBQANBA1YDQQMIAzEDdAL4AvUCVgNUA1MDVgNTA1UDVAMxAzAD9wL4AmsC9wJrAmoCagL4AfEBagJrAvgB+AJ0AmsCMQNWA0EDMQP1AvgC9QIxAwgDUwP1AVUDYgBAAD4AYgBhAJsA9QFAA1UD9QFeAPYBPwBAAF4AQABiAF8AKAH3AR0B9wFrAnQC9wEoAfgBXgD1AT8AXwBiAIkAXwBeAEAAHQGbACgBPQBAAD8AQAA9AD4AYQBiAD4AmgCbAGEAKAGbAJoAJwH4ASgBJwEoAZoA+AEnAfEBmwAdAZwAnABiAJsAYgCcAIkAPQD1AfAB9QFTA/ABPwD1AT0AQQBFAEQAQgBGAEUASgNJA0wDSQNIA0sDTANPA00DSwNMA0kD5QIhAiAC5ALlAiAC5AIlA+UCUANNA08DTQNKA0wDJQNKA00DJQMkA0oDJQPkAiQDIAIhAq4BrgFwAW8BbwFwAasAqwCsAGgAaQBoAKwAaQBGAEMAaABpAEMAZQBIAyoDSANlAEEASgP9AiwDbwHCAIQBQgBlAGYArgGEAa0BrgGtAQ0CIAKuAQ0CIAINAs8CSANJAyoDSQMsAysD5AIgAs8CJAPPAv0CwgBvAasAwgBoAJQAhAGuAW8BzwIkA+QC/QJKAyQDLANJA0oDKwMqA0kDZQBCAEEAZwBCAGYAlABDAGcAQwBCAGcAaADCAKsAaABDAJQARgBCAEMARQBBAEIARQBJAEgASQBFAEYAcAGuAa8BcAGsAKsArwGuASECAwP8AUQDAwNGAwUDRANGAwMDRAP8AUUDAwLeAgQCBAIBAgMC3gLfAgQC3wIJAwoDRQNGA0QDRgNFA0cDCQNHAwoDCQPfAt4CRwMJA0YDXALeAgMCAwIAAlwCRgP8AgUD3gJcAs4C/AJGAwkDzgIJA94CCQPOAvwC3wJHAwQCRwPfAgoDRwMCAgQC/AGRAEoA/AEDA/4BkQD8Af4BkQCTAEwA/QFFA/wB/AFKAEsA/AFLAP0BjQGOAbIAjgEBAgICAQIEAgICAQKOAY0BsgCOAbMASwBMAE0ATABLAEoAiACHALMAhwBNAEwAhwCyALMATQCHAIgAOAGyAMYAjQEAAgECAAIDAgECAAKNATgBTABKAJEATACYAIcAmABMAJMAsgA4AY0BxgCHAJgAhwDGALIA/QFHA0UDswBNAIgAAgJHA/0BTQCzAI4BTQD9AUsATQACAv0BTQCOAQICTwLgAU0C4gHgAU8CVgLpAeoBVQLoAekBUgLjAeUB4wFSAlACUQLlAeQBTwLkAeIB5AFPAlECVQJRAlQCUQJVAlICVgJSAlUCUgJXAlACUwJPAk0CTwJUAlECTwJTAlQCTgLsAeEBTgJYAuwBTgLjAVAC4QHjAU4CVwJSAlYCWAJQAlcCWAJOAlACVwLqAesBWAJXAusBVAJTAucBOwE5AeAB4gE7AeAB6gFXAlYCQgHpAUEBQgHqAekB6QFWAlUCQQHoAUABQQHpAegB4wE+AeUB4wE8AT4B5QE+AT0B5QFRAlIC5QE9AeQB5AE7AeIB5AE9ATsBPQFAATsBPQFBAUABQQE+AUIBPgFBAT0BPgFDAUIBOwE/ATkBPwE7AUABOgHsAUQB7AE6AeEBOgE8AeMB4wHhAToBQwE+ATwBPAFEAUMBRAE8AToBQwHrAeoBQwHqAUIBRAHrAUMB6wHsAVgC7AHrAUQB4AHnAVMC4AE/AecB4AFTAk0CPwHgATkBQAHnAT8B6AFUAucB6AFVAlQC6AHnAUAB","c":"AgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEB","pal":["444444","666666","aaaaaa","bb8844"]},"sniper":{"s":2.7436140018921472e-05,"p":"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","i":"pgAfAyADTQBKAEsAWABVAFYArwArAykDKgOyALAAIAOoAKYAXwBgAF4AqwAnAyUDegN5A3gDqQAlAyMDrQApAycDJAOsAKoAJgOuAKwAXQBeAFwAfAMrA2QAYQBiAGAAKAOwAK4AIgOqAKgAdgN1A3QDpwAjAyEDcwNwA28DpwAhA1cAeAN3A3YDZQNiA2QDWgBdAFwAdQNyA3QDZwNkA2YDYQNeA18DTwBMAE0AFwObAJ0AUQBOAE8AUwBQAFEAHQOhAKMApAAcAx4DHQOjAFUAawNoA2oDbgNrA2oDVQBUAFIAUgNRA1ADRgBEAEUAEgOVAJgAiQADA1ADjAAEAwYDOgA5ADsABQOJAIsAPQA6ADsABwOLAI0AjgAGAwgDVQNSA1QDQwBAAEEACwOPAJEAWwNYA1oDWANXA1YDQAA/AEEASQBGAEgAFAOYAJoAmQARAxMDlwAQAxEDVgNVA1QDYgNgA2EDkAAIAwoDCQONAI8ADwOTAJUAPgA9AD8ARQBCAEMAXQNaA1wDDQORAJMAlgAOAxADlAAMAw4DkgAKAwwDXgNcA18DGwOfAKEAaQNmA2gDogAaAxwDGQOdAJ8AoAAYAxoDpQAeAx8DngAWAxgDcANtA28DYgBlAGQAWgBXAFsAVwBxAFUAFQOaAJsAnAATAxYDSgBIAEsAbwNDA0QDbwNyAFcAVQBDA20DPgPHAj0DcQBJA0MDQwNIA0QDcQBsAG0AxwLqAOkAeADpAOoAPQPpAHcAPgPqAMgCbQA+A0kDSAN3AGwAbAB4AG0ASAM+Az0DRANsAHIAWgCnAFkAIQNyA3EDagMdA2wDowBSAFQAKQN8A3oDIANzAyIDsgBjALAAJQN0AyMDJgN5AygDrQBeAGAAqgBfAF0AJwN2AyUDrABhAF8AKAN7AyoDrwBgAGIAKQN4AycDrgBjAGEAKgN9AywDrwBkALEAIgN1AyQDpwBcAKkApgBbAFgAIQN0A3IDqABdAFsAJAN3AyYDqwBcAF4AGQNoA2YDnQBOAJ8ATQCgAE8AcAMfA24DVgCkAKUAaQMeAxwDGwNqA2gDUQCkAFMAoQBSAKMAZwMcAxoDXwMUA2EDSgCdAJsAngBLAJwAYwMYAxYDiwA8AI0AlwBFAJYACANTA1UDDwNfA1wDiwA4ADoAjAA5AIoABANTAwYDBQNQAwMDkQBCAJMAkgA/AJAADgNZA1sDCwNWAwkDlgBDAJQADQNcA1oDlQBHAJgAEQNdA14DjwA8AD4ACgNVA1cDjgA7AIwABQNUA1IDkwBEAJUAEANbA10DlABBAJIACwNaA1gDkAA9AI4ACQNUAwcDkQA+AEAADANXA1kDFwNmA2QDnwBQAKEATwCiAFEAZQMaAxgDYgMXA2QDSQCbAJoAnABIAJkAYAMWAxMDbgMeA2sDWAClAKYAYQMVA2IDRwCaAJgAmQBGAJcAXgMTAxEDfQN6A3wDpgClAB8DTQBMAEoAWABXAFUArwCxACsDKgMsA7IAIAMiA6gAXwBhAGAAqwCtACcDegN7A3kDqQCrACUDrQCvACkDJAMmA6wAJgMoA64AXQBfAF4AKwOxAGQAZABlAH0DZQCyAH0DsgAsA30DfQN8A2QAYQBjAGIAKAMqA7AAIgMkA6oAdgN3A3UDpwCpACMDbwNxA3IDcgNzA28DcQNvAyEDbwNXACEDVwBZAKcAeAN5A3cDZQNjA2IDWgBbAF0AdQNzA3IDZwNlA2QDYQNgA14DTwBOAEwAFwMVA5sAUQBQAE4AUwBSAFAAHQMbA6EApACiABwDVABVAKMAVQBtAx0DbQNsAx0DawNpA2gDbANtA2oDbQNuA2oDUwBWAFIAVgBVAFIAUgNTA1EDRgBHAEQAEgMPA5UAUANRAzkAUQMEAzkABAOKADkAOQA4AFADOACJAFADjACKAAQDOgA4ADkABQMDA4kAPQA8ADoABwMFA4sAjgCMAAYDVQNTA1IDQwBCAEAACwMJA48AWwNZA1gDWANZA1cDQAA+AD8ASQBHAEYAFAMSA5gAmQCXABEDlwCWABADVgNXA1UDYgNjA2ADkACOAAgDCQMHA40ADwMNA5MAPgA8AD0ARQBEAEIAXQNbA1oDDQMLA5EAlgCUAA4DlACSAAwDkgCQAAoDXgNdA1wDGwMZA58AaQNnA2YDogCgABoDGQMXA50AoACeABgDpQCkAB4DngCcABYDcANuA20DYgBjAGUAWQBXAFoAVwBYAFsAVwByAHEAFQMUA5oAnACZABMDSgBJAEgAbwNtA0MDbwNEA3IAVQBxAEMDPgPIAscCcQBtAEkDQwNJA0gDcQByAGwAxwLIAuoAeAB3AOkAPQPHAukAPgN4AOoAbQB4AD4DSAM9A3cAbAB3AHgASANJAz4DRANIA2wAKQMrA3wDIANwA3MDsgBlAGMAJQN2A3QDJgN3A3kDrQCrAF4AqgCsAF8AJwN4A3YDrACuAGEAKAN5A3sDrwCtAGAAKQN6A3gDrgCwAGMAKgN7A30DrwBiAGQAIgNzA3UDpwBaAFwApgCoAFsAIQMjA3QDqACqAF0AJAN1A3cDqwCpAFwAGQMbA2gDnQBMAE4ATQCeAKAAcAMgAx8DVgBTAKQAaQNrAx4DGwMdA2oDUQCiAKQAoQBQAFIAZwNpAxwDXwMSAxQDSgBMAJ0AngBNAEsAYwNlAxgDiwA6ADwAlwBGAEUACAMGA1MDDwMSA18DiwCJADgAjAA7ADkABANRA1MDBQNSA1ADkQBAAEIAkgBBAD8ADgMMA1kDCwNYA1YDlgBFAEMADQMPA1wDlQBEAEcAEQMQA10DjwCNADwACgMIA1UDjgA9ADsABQMHA1QDkwBCAEQAEAMOA1sDlABDAEEACwMNA1oDkAA/AD0ACQNWA1QDkQCPAD4ADAMKA1cDFwMZA2YDnwBOAFAATwCgAKIAZQNnAxoDYgMVAxcDSQBKAJsAnABLAEgAYANjAxYDbgMfAx4DWABWAKUAYQMUAxUDRwBJAJoAmQBIAEYAXgNgAxMDfQN7A3oDgAPYAjYAmAN+A4YD1wKBA9kCLQA3AC8AfgPYAoAD4AA2AOIAfwOJA4EDNQDjADcANAAuADYAhwOIA4kD4QDZAuMALAAvAC4AhgOAA4gD1gLiANgCHAAfAB4ALwPgANYC1wKNA38DHwA1AC0AjAPWAn4D4AAqADQAfwOZA4cDKwDhADUANAAeACwAmQOGA4cD4QAwA9cCLAAfAC0AHAAoAJYDlgOMA5gDLQOHAC8DLgONAzADHQArAB8AigMvA4wDhQAqAIcAiwOZA40DKQCIACsAKAAeACoAlwOYA5kDhgAwA4gAXQFsAmsCvwDCAMEAaAJbAVoB9gLzAvUCZgJXAWUCagJrAvUCbAJpAvYCZwLzAmUCaAJmAvQCXAHBAF0BWwFeAcIAWQFXAb8AWgHAAFgBZwL1AvMCbAL1AmsCaAL2AmkCZQL0AmYCZwJXAVkBWgFmAmgCwABXAVgBWQHBAFwBWwHAAFoBXgHBAMIAXAFrAmoCaQJeAVsBWQFqAmcCGgL9AhkCugCtAbkA+AIeAhwCggCDAIEA/AIXAvsCrAG3AKsBNAMxAzMDvACqAb4AuwCpAacB+QL9AjMD/gL6AjQD9wIxA/sC+AL8AjIDvQCDALkAvgC6AIQAtwCBALsAvACCALgAqwEbAhcCGAKoAawBGQKpAa0BrgEeAhoC+QIxA/cC/gIzA/0C+AI0A/oC+wIyA/wCGwL7AhcCqAG4AKwBgQC4AIIAvQCBAIMAvgCCALwAuQCEALoAqQG5AK0BHgL+AhoCpwG3ALsAHAL8AvgCHQL9AvkCqgG6AL4AHgKoARwCqQEbAqcBGQKuARoCqwEYAqwB9wIdAvkC0ALMAs8CzQLJAswC7wDxAO4A8gDPAvEA7QDOAssCzQLyAO8A8QDOAvAA7gDwAO0AzALtAMsCzwLLAs4CygLrAMkCzALrAO4A7wDKAs0C7gDsAO8ACwJsAQoCbQENAWwBDgFwAQ0BcQEOAnABDwJYAg4CWQKzAlgCCwK0Ag8CtAJUArMCVQIKAlQCUgKPA7ECQAOOA48DagEnAHQAVgIWAgwCUgICAkADagECAggCCwF2ACcAsQJCA1YCbgEWAnYAQgMVAhYCdgAmACcAdAABAgICAgI/A0ADjwNBA0IDFgJ1AHYAJwBzAHQAKgEBAnMA5ABzACYA5AB1ACsBEAJ1ABUCEAJBA5cC1QJBA44DlgKOAz8DBwI/AwECKgEGAgcCKwFvAOQAlwISAhAClgJHA9UC5AD8ACoBEAL+ACsB1QLAApcCBwK+ApYC/AAFAgYC/gBuAG8AwAIRAhICvgJGA0cDbwD7APwAEgL9AP4ARwO/AsACBgK9Ar4CUwIIAlICsgJSArECVwKxAlYCDQJWAgwCbwEMAm4BDAFuAQsBawELAWoBCQJqAQgCDQHoAGsAawFqAAwBcAEUAugADAHnAG8BWAIUAg4CbwETAg0CswLUAlgCVwITAtMC0gIDAtECSwPRAkoD1AJKA9MCFALTAhMC6AATAucAawDnAGoA5gBqAOUABALlAAMCawEDAuUAbAFrAOYAUwIDAgkCbAEEAgoCUwJKA9ECVAIEAtICsgLTAkoDVAJLA7MCSgJAAkECSQKEAUAChAF8AYUBfAFBAoUBiQE8AjsCSQJ8AXsBhQE8AooBhAGKAYkBOwKEAYkBPAJAAjsC+AGUAcwBlAFpAWcBaQE6AWcBOwFmAToBaAGTAWYBlQHLAZMB9wEvAssBMQJaAi8CXAKIAloCiQJbAogCXQJcAvcBXQIwAlsCMgLMATACjQE4AT8BsAEnArcBPwETASwBtAE+AjgCjgGyAbQBQAETATkBOAKLAoQCjgE5AYgBlQKLAqwClQKKAoMCNwKKAj0CswE9Aq8BjQGvAYcBsQGIAZoBegGIATkBegETAWEBYQE4AXkBeQGHAZkBsAGHAa8BKwKvAT0CKwKKAksCSwKsAl4CXgKLAkwCLAKLAj4CsQE+ArIBJwJCAigCeQGDAWEBTAIoAjoCsQGeAbgBSwInAisCeQGdAYsBsQEoAiwCegGeAZoBSwJCAjkCsAGdAZkBegGDAYwBTAJCAl4CywGbAZMBLwK5AcsBWgIpAi8CiAJHAloCWwJPAogCZwGcAZQBWwIqAkgCOgF+AWcBlAG6AcwBzAEqAjACZgF0AToBZgGbAX0BmwG1AZcBKQK1AbkBRwItAikCRwKHAlACSAKHAk8CnAFzAZgBSAIuAlECdAFzAX4BnAG2AboBKgK2AS4CdAFyATwBfQGXAXIBUAI3Ai0ChwKDAlACmAG0AbYBtgE4Ai4ClwE/AXIB9gEmAekBJwHZACYB5QKaAuMCmwLpAZoC3wLgAuEC2wDYANkAgQH1AegBLwGCAYEBRQKUApMC6AFGAkUCjQKRApICNAEwAS8BJAHJAcoBJwH1AYIB0QB/ASQB2wCCATAB5AKUAuUC3gKRAowC3gKNAuAC2gA0AdgA3wJDApEC5QJGApsCmALJAUMCmwL1AfYB2gAwATUB0AAtAdEA2AAzAdIA4gKPAuQCMwEvAS4BjwKTApQC2QFFAkQCRAKTApICLgGBAYABgAHoAdkB1gAIAdcA4wLkAuUCmgLaAZkC4wKZAuEC+gAoAfkA2gEmASUB2gEkAcoBJQHRACQB4QKYAt8CmQLKAZgC0wDQANEAfwHZAckBLQGAAX8BQwKSApECyQFEAkMCMgEuAS0B0gAyAdAA4AKOAuICjgKSApMC4QLiAuMC1gDZANgA+gDZANcA+gAlASYB1ADYANIA+QDTACUB1ADTANUA1QAFAdQA1AAHAdYA+gAIASkB+QAGAdUAnwKcAp4CnQIeAZwCIAEjASIBIwGgAiIBIAGgAp4CnwIjASEBnQIhAR8BHgGeApwCHgEhASABoQKeAqACIQL5AfsBJgIfAiECIgIlAiYC/AEgAiICpgH6AfwBoAGkAaYBnwGjAfkBpQGfAaAB+wGjAaUBIgImAiECHQGmAhwBEAERAQ8BpAIbARoBsAKtAq8CowIYAaICpwKlArACpAKjAq4CGwEdARIBGgEQARkBpwKvAqYCpAKwAqUCogKuAqMCGgGjAqQCDwEZARABGwEQARoBHAESAR0BpQIdARsBHAGmAq8CAAL0APAB9gAxAPQAMwAgADEAIQAwACAAMgDzADAA9QDtAfMA/QHDAu0BxQKCA8MChAOUA4IDlQODA5QDhQPEAoMDxgLwAcQCxQB5AHoAVgHwAfQAzgD0ADEAfgAxACAAzQAgADAAzQDzAFUBVQHtAe4B7gHDAnsCewKCA/EC8QKUAzwDPAODA/ICfAKDA8QCfALwAe8BbQLHAc8BcQLoAuoCSwHQAdMB6QJtAnACSgHDAMUA0wFuAnECxgBIAUsBOAPnAukC0gFHAUoBegDEAMYA6gI3AzgDcALPAdIBTgPBAccBwwAQAHkAbgKfA+gCSAHIAdAB5wJOA20CRwETAMMA0AFPA24CxABpAEgBNwOeA+cCRwHHAWgAeQAUAMQA6AKhAzcDEwAPABAATwOdA58DaQDEAcgBngNMA04DaAARABMAyAFNA08DFABnAGkAoQOcA54DxwFmAGgAEAASABQAnwOgA6ED8gF2Af8BPgH4AHYBcAD4ALQAswDPAHAAPQH3ALMA8QF1AT0BhQL+AfEBhQLBAk0CAQPmAsECAgPmAkUDhgLCAgID8gFOAoYCMQGRAV8BkAHyAfQBtACQAWABcABgATEBXwFwADEBPQFfAY8B8QGPAfMBNQLxAfMBYwKFAjUCRQNjApACZAJFA5ACNgICA2QC8gE2AvQBxQEjAs0BNAI2AmQCzgGQAfQBIwJjAjUCXwGhAY8BJAL0ATYCogFgAZABMwKQAmMCjwHNAfMBkgExAWABPwJkApAC8wEjAjUCwgHcAsUBGgCRAYYBkwMkAjQCxgGiAc4B3AIzAiMCIgChAZEB3QLOASQCogEjAJIBMwKbAz8CoQHFAc0BkgEaAIYBmwM0Aj8CGwAiABoAkwPbAt0CwwHdAMYB3AKQA5IDJADcACIA3QLDAcYB3QAlACMAkgOaA5sD3gDFAdwAIwAbABoAmgOTA5sD9gD/AXYB9gD4ADMAMwDPACEAMgDPAPcAMgB1AfUA9QD+Af0BxQL+AU0CxQLBAoQDhAPmApUDhQPmAsIChQNOAsYCxgL/AQACZwDDAcQBEgDfAGcAEgAbACUAEQAbAA8AEQDeACQAZgDCAd4ATAPCAcEBTAOQA9oCnAOaA5ADnQOaA6ADTQORA50DTQPDAdsC3AEUAdsBFAG2ALUAtgAWAbUAFwHhARYB4gGqAuEBqwL/AqoC4gEDAeMBAAOoAv8CqQLbAagC/wKoAhYBfwAEAQMBAAO3AqkCFQF/ALYA4gG5AqsC3AG3AtcB3AEBARUBFwF/AAMBqwI1AwADZAHgAd8B1wECAQEBtwLYAdcBuQI2AzUDAwHkAeMBAQGAAH8ANQO4ArcC4wG6ArkC3QECAdgBYgGAAAIBCQEEAYAA3wEEAWQBYQLkAd8BtQK6AmECXwI2A7UC3QG4Al8C3gG2AuABYgEKAQkBtQJgAl8C3wFiAmECCQFlAWQB3QFjAWIBXwLeAd0BYQK2ArUCewLrAe4B8gI7AzwDfgDMAM4A7gFTAVUBPAPvAvECzgBUAVYB7wF6AnwCVQHLAM0A8QJ5AnsCVgHsAe8BfALwAvICzQB9AH4AUwFPAVAB8AI6AzsDfQDKAMwA5gFPAeUBOwPtAu8CzABRAVQB7AF3AnoCUwHJAMsA7wJ2AnkCVAHnAewBegLuAvACywB8AH0AeALlAeoB6gFPAVIB6wFSAVMBdgJ4AnkC5gF1AnYC6wF4AuoBdgLVAeYB7gI5AzoDfADIAMoA5gFNAVABOgPrAu0CygBOAVEB5wF0AncCUAHHAMkA7QJzAnYCUQHWAecBdwLsAu4CyQB7AHwA0gFvAnAC7AI4AzkDewDGAMgAcAJyAnMCOQPpAusCyABLAU4B1gFxAnQCTQHFAMcA6wJwAnMCTgHTAdYBdALqAuwCxwB6AHsAcgLRAdQB1AFJAUwB1QFyAtQB0gFJAdEBSgFMAUkB1QFMAU0BNwF3AXgBGAAHAAkABwAKAAkAFwB4ARkACQAZABgANgEXABYABwAXAAgAdwEZAHgBCAAZAAoAdwEWABgABgACAAwAAgAGAAEABgAMABUABgAVAAsABgALAAEAAgABAAAADAACAAQAFQAMAA4ACwAVAA0AAQALAAMAAgAAAAQADAAEAA4AFQAOAA0ACwANAAMAAQADAAAABAAAAAUADgAEAAUADQAOAAUAAwANAAUAAAADAAUAwAFDAb0BRgH/AEMBAAFCAf8AvAFBAbsBvwGAAr4BgQK7An4CRQEAAbwCvAJ/ArsCggK9AX8CQgF+An8CRAG7AUEBvgF9ArsBfgKAAoECvAF9An4CvwFEAUUBRQFBAUIBRgO9AgUC2ALiADYANgAuAIgDLgAvAIgDLwA3AIEDNwDjAIED4wDZAoEDgQOJAy8AiQOIAy8AiAOAAzYAmAOMA34D1wJ/A4EDLQA1ADcAfgPWAtgC4AA0ADYAfwOHA4kDNQDhAOMANAAsAC4AhwOGA4gD4QDXAtkCLAAtAC8AhgN+A4AD1gLgAOIAHAAdAB8ALwOHAOAA1wIwA40DHwArADUAjAMvA9YC4ACHACoAfwONA5kDKwCIAOEANAAqAB4AmQOYA4YD4QCIADADLAAeAB8AKACFAC0DLQOKAygAigOWAygAlgOXAx0AlwOLAx0AiwMuA4YAhgApAIsDKQAdAIsDHQAcAJYDlgOKA4wDLQOFAIcALgOLA40DHQApACsAigMtAy8DhQAoACoAiwOXA5kDKQCGAIgAKAAcAB4AlwOWA5gDhgAuAzADXQFeAWwCvwDAAMIAaAJpAlsB9gL0AvMCZgJYAVcBZwJqAvUCbAL2AvUCaAL0AvYCZQLzAvQCZwJlAlcBWgFYAWYCwAC/AFcBWQG/AMEAWwHCAMAAXgFdAcEAXAFdAWsCaQJsAl4BWQFcAWoCGgL+Av0CugCuAa0B+AL6Ah4CggCEAIMA/AIYAhcCrAG4ALcANAMyAzEDvACoAaoBuwC9AKkBqwGnARsCGAIcAqgBGQIdAqkBrgGqAR4C+QIzAzED/gI0AzMD+AIyAzQD+wIxAzIDGwL3AvsCqAG8ALgAgQC3ALgAvQC7AIEAvgCEAIIAuQCDAIQAqQG9ALkAHgL6Av4CpwGrAbcAHAIYAvwCHQIZAv0CqgGuAboAHgKqAagBqQEdAhsCGQKtAa4BqwEXAhgC9wIbAh0C0ALNAswCzQLKAskC7wDyAPEA8gDQAs8C7QDwAM4CzQLQAvIA8QDPAs4C7gDxAPAAzALuAO0AzwLMAssCygLsAOsAzALJAusA7wDsAMoC7gDrAOwACwJtAWwBbQEOAQ0BDgFxAXABcQEPAg4CDwJZAlgCWQK0ArMCDgFtAQsCCwJVArQCtAJZAg8CDwJxAQ4BDgELAg8CtAJVAlQCVQILAgoCUgJAA48DQAM/A44DagELAScAVgJCAxYCUgIIAgICagF0AAICCwFuAXYAsQKPA0IDbgEMAhYCQgNBAxUCdgB1ACYAdABzAAECAgIBAj8DjwOOA0EDFgIVAnUAJwAmAHMAKgEHAgEC5AAqAXMA5AAmAHUAEAIrAXUAEAIVAkED1QKXAkEDlgLVAo4DBwKWAj8DKgH8AAYCKwH+AG8AlwLAAhIClgK+AkcD5ABvAPwAEAISAv4A1QJHA8ACBwIGAr4C/AD7AAUC/gD9AG4AwAK/AhECvgK9AkYDbwBuAPsAEgIRAv0ARwNGA78CBgIFAr0CUwIJAggCsgJTAlICVwKyArECDQJXAlYCbwENAgwCDAFvAW4BawEMAQsBCQJrAWoBDQFwAegAawHlAGoAcAEOAhQCDAFqAOcAWALUAhQCbwHnABMCswJLA9QCVwINAhMC0gIEAgMCSwPSAtEC1AJLA0oDFALUAtMC6AAUAhMCawDoAOcA5gBrAGoABALmAOUAawEJAgMCbAENAWsAUwLRAgMCbAHmAAQCUwKyAkoDVAIKAgQCsgJXAtMCVALSAksDSgJJAkACSQJ7AYQBhAF7AXwBfAFKAkECiQGKATwCSQJKAnwBhQFBAjwChAGFAYoBOwJAAoQBPAJBAkAC+AGWAZQBlAGWAWkBaQE7AToBOwFoAWYBaAGVAZMBlQH3AcsB9wExAi8CMQJcAloCXAKJAogCiQJdAlsCaQGWAfgB+AEyAl0CXQKJAlwCXAIxAvcB9wGVAWkBlQFoAWkBaAE7AWkBaQH4AfcB+AFdAvcBXQIyAjACMgL4AcwBjQGHATgBsAErAicCPwE4ARMBtAGyAT4CjgGIAbIBQAEsARMBOAI+AosCjgFAATkBlQKEAosClQKsAooCNwKDAooCswE3Aj0CjQGzAa8BsQGyAYgBegGaAYgBegE5ARMBYQETATgBeQE4AYcBsAGZAYcBKwKwAa8BKwI9AooCSwKKAqwCXgKsAosCLAJMAosCsQEsAj4CKAK4AZ4BngGMAYMBgwGLAScCiwGdAScCnQG3AScCJwI5AkICQgI6AigCKAKeAScCngGDAScCeQGLAYMBTAIsAigCsQGaAZ4BSwI5AicCeQGZAZ0BsQG4ASgCegGMAZ4BSwJeAkICsAG3AZ0BegFhAYMBTAI6AkICywG5AZsBLwIpArkBWgJHAikCiAJPAkcCWwJIAk8CZwF+AZwBWwIwAioCOgF0AX4BlAGcAboBzAG6ASoCZgF9AXQBZgGTAZsBmwG5AbUBKQItArUBRwJQAi0CRwJPAocCSAJRAocCnAF+AXMBSAIqAi4CdAE8AXMBnAGYAbYBKgK6AbYBdAF9AXIBfQGbAZcBUAKDAjcChwKVAoMCmAGOAbQBtgG0ATgClwGNAT8B9gEnASYBJwHbANkA5QKbApoCmwL2AekB3wLeAuAC2wDaANgAgQGCAfUBLwEwAYIBRQJGApQC6AH1AUYCjQKMApECNAE1ATABJAF/AckBJwH2AfUB0QAtAX8B2wAnAYIB5AKPApQC3gLfApEC3gKMAo0C2gA1ATQB3wKYAkMC5QKUAkYCmALKAckBmwJGAvUB2gDbADAB0AAyAS0B2AA0ATMB4gKOAo8CMwE0AS8BjwKOApMC2QHoAUUCRAJFApMCLgEvAYEBgAGBAegB1gAHAQgB4wLiAuQCmgLpAdoB4wKaApkC+gApASgB2gHpASYB2gElASQBJQHTANEA4QKZApgCmQLaAcoB0wDSANAAfwGAAdkBLQEuAYABQwJEApICyQHZAUQCMgEzAS4B0gAzATIB4AKNAo4CjgKNApIC4QLgAuIC1gDXANkA+gAmAdkA+gD5ACUB1ADWANgA+QDVANMA1ADSANMA1QAGAQUB1AAFAQcB+gDXAAgB+QAoAQYBnwKdApwCnQIfAR4BIAEhASMBIwGhAqACIAEiAaACnwKhAiMBnQKfAiEBHgEgAZ4CHgEfASEBoQKfAp4CIQIfAvkBJgIlAh8CIgIgAiUC/AH6ASACpgGkAfoBoAGfAaQB+QEfAvoBHwIlAvoBJQIgAvoB+gGkAfkBpAGfAfkBpQGjAZ8B+wH5AaMBIQL7AaYB+wGlAaYBpQGgAaYBpgH8ASEC/AEiAiECHQGnAqYCEAESAREBpAKlAhsBsAKuAq0CowIZARgBpwKwAq8CpAKuArACogKtAq4CGgEZAaMCDwEYARkBGwESARABHAERARIBpQKnAh0BrwKtAg8BrQKiAg8BogIYAQ8BDwERAa8CEQEcAa8CAAL2APQA9gAzADEAMwAhACAAIQAyADAAMgD1APMA9QD9Ae0B/QHFAsMCxQKEA4IDhAOVA5QDlQOFA4MDhQPGAsQCxgIAAvABxQDDAHkAVgHvAfABzgBWAfQAfgDOADEAzQB+ACAAzQAwAPMAVQHzAO0B7gHtAcMCewLDAoID8QKCA5QDPAOUA4MDfALyAoMDfALEAvABbQJOA8cBcQJuAugCSwFIAdAB6QLnAm0CSgFHAcMA0wHQAW4CxgDEAEgBOAM3A+cC0gHPAUcBegB5AMQA6gLoAjcDcAJtAs8BTgNMA8EBwwATABAAbgJPA58DSAFpAMgB5wKeA04DRwFoABMA0AHIAU8DxAAUAGkANwOhA54DRwHPAccBeQAQABQA6AKfA6EDEwARAA8ATwNNA50DaQBnAMQBngOcA0wDaABmABEAyAHEAU0DFAASAGcAoQOgA5wDxwHBAWYAEAAPABIAnwOdA6AD8gE+AXYBPgG0APgAcADPAPgAswD3AM8APQF1AfcA8QH+AXUBhQJNAv4BhQIBA8ECAQNFA+YCAgPCAuYChgJOAsIC8gH/AU4CMQGGAZEBkAE+AfIBtAA+AZABcAC0AGABXwGzAHAAPQGzAF8B8QE9AY8BNQKFAvEBYwIBA4UCRQMBA2MCZAICA0UDNgKGAgID8gGGAjYCxQHcAiMCNAIkAjYCzgGiAZABIwIzAmMCXwGRAaEBJALOAfQBogGSAWABMwI/ApACjwGhAc0BkgGGATEBPwI0AmQC8wHNASMCwgHaAtwCGgAiAJEBkwPdAiQCxgHdAKIB3AKSAzMCIgDcAKEB3QLGAc4BogHdACMAMwKSA5sDoQHcAMUBkgEjABoAmwOTAzQCGwAkACIAkwORA9sCwwHfAN0A3ALaApADJADeANwA3QLbAsMB3QDfACUAkgOQA5oD3gDCAcUBIwAlABsAmgORA5MD9gAAAv8B9gB2AfgAMwD4AM8AMgAhAM8AMgD3AHUB9QB1Af4BxQL9Af4BxQJNAsEChAPBAuYChQOVA+YChQPCAk4CxgJOAv8BZwDfAMMBEgAlAN8AEgAPABsAEQAkABsAEQBmAN4AZgDBAcIBTAPaAsIBTAOcA5ADnAOgA5oDnQORA5oDTQPbApEDTQPEAcMB3AEVARQBFAEVAbYAtgAXARYBFwHiAeEB4gGrAqoCqwIAA/8C4gEXAQMBAAOpAqgCqQLcAdsBqALbARQBFAG1AKgCtQAWAagCFgHhAaoCqgL/AhYBfwCAAAQBAAM1A7cCFQEBAX8A4gHjAbkC3AGpArcC3AHXAQEBFwG2AH8AqwK5AjUDZAFlAeAB1wHYAQIBtwK4AtgBuQK6AjYDAwEEAeQBAQECAYAANQM2A7gC4wHkAboC3QFiAQIBYgEJAYAACQFkAQQB3wHkAQQBYQK6AuQBtQI2A7oCXwK4AjYD3QHYAbgCCgFjAd4B3gFgArYCtgJiAuAB4AFlAQoBCgHeAeABYgFjAQoBtQK2AmAC3wHgAWICCQEKAWUB3QHeAWMBXwJgAt4BYQJiArYCewJ5AusB8gLwAjsDfgB9AMwA7gHrAVMBPAM7A+8CzgDMAFQB7wHsAXoCVQFTAcsA8QLvAnkCVgFUAewBfAJ6AvACzQDLAH0AUwFSAU8B8ALuAjoDfQB8AMoA5gFQAU8BOwM6A+0CzADKAFEB7AHnAXcCUwFQAckA7wLtAnYCVAFRAecBegJ3Au4CywDJAHwAeAJ1AuUB6gHlAU8B6wHqAVIBdgJ1AngC5gHlAXUC6wF5AngCdgJzAtUB7gLsAjkDfAB7AMgA5gHVAU0BOgM5A+sCygDIAE4B5wHWAXQCUAFNAccA7QLrAnMCUQFOAdYBdwJ0AuwCyQDHAHsA0gHRAW8C7ALqAjgDewB6AMYAcAJvAnICOQM4A+kCyADGAEsB1gHTAXECTQFKAcUA6wLpAnACTgFLAdMBdAJxAuoCxwDFAHoAcgJvAtEB1AHRAUkB1QFzAnIC0gFKAUkBSgFNAUwB1QHUAUwBNwE2AXcBGAAWAAcABwAIAAoAFwA3AXgBCQAKABkANgE3ARcABwAWABcAdwEYABkACAAXABkAdwE2ARYAwAFGAUMBRgEAAf8AAAFFAUIBvAFCAUEBvwGBAoACgQK8ArsCAAFGAcABwAGCAgABggK8AgABvAKBAr8BvwFFAbwCvAKCAn8CggLAAb0BfwK9AUMBQwH/AEIBQgG8AX4CfgK7An8CfwJDAUIBRAG+AbsBvgGAAn0CfgJ9AoACvAG7AX0CvwG+AUQBRQFEAUEBBQL7AG4AbgD9AAUC/QARAgUCEQK/AgUCvwJGAwUCCAEFAQYBKQEGASgBCAEHAQUBKQEIAQYB","c":"AQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACAgIC","pal":["333333","445533","bbbbbb"]},"glaunch":{"s":1.2207403790398877e-05,"p":"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","i":"swHfAuUC3wKzAZwBoACcAbMBnAGgAJoAawCaAKAAmgBrAGoAawCeAJwAsQGeAJ8AngCxAbABawCgALMB5AKwAbEBsAHkAuMCFAPeAuECnQEAA98CnQGaAIIA3wKcAZ0BgQBRAFIAUQCBAH8AUgCZAIEAmQBSAGoALgPeAhQDLgP/At4C/wKbAd4CLgPfAgAD3wIuAxQDmgCdAZwBgQCbAZoBmwH/ApoBmwGBAJkAagCCAJoAggBqAFIABgOLAq4BggCZAZ0BmQGCAIAAAAP+Ai4DLgP+Ai0DLgMtA/8C/wItA/0CmgGWAYEAgQCWAX8AUgCAAIIAgABSAFEAnQH+AgAD/gKdAZkB/wL9ApoBmgH9ApYBZgBTAFgAUwBmAGEA7QKTAO4CkwDtApIAYAKDAvwCYAL8AqACjADoAo0A6AKMAOcCVgBUAFcAVwBUAFUAKgMnAykDJwMqAygDVgBTAFQAJwMmAykDkQDrAuwC6wKRAJAAWABWAFkAWABTAFYAlwDzAvIC8wKXAJgAJgMrAykDKwMsAykD6wLpAhoD6QIYAxoDVQBgAGIAUwBhAF4AGQMWA+oC5wLqAhYD6wKQAI4AjgDpAusCKAMaAxgD6gKMAI8AjADqAucCJgMWAxkDHQPsAhsD7AIdA+4CVwBjAGUAkwDsAu4C7AKTAJEAKgMdAxsDaACUAGYAlABoAJcAKwMeAyAD7wKUAPIC8gKUAJcAWABoAGYAEwMsAyEDLAMTAx8DEwMhA/MC8wLxAhMDEwPxAvAC8AIfAxMDbABpAFkAWQBnAGwAbACVAJYAlQBsAGcAlgCYAGwAmABpAGwAXgBUAFMAVABeAF8AXwBeAIwAXwCMAI0AKAMXAycDFwMoAxgD6AIYA+kCGAPoAhcDGgMbA+wC7ALrAhoDKgMaAygDGgMqAxsDZgCPAGEAjwBmAJQA7wKPAJQAjwDvAuoCHQMcA+0CHQPtAu4CHAMdAyoDHAMqAykDZABXAGUAVwBkAFYAkgBlAJMAZQCSAGQAYwCQAJEAkABjAGIAVwBiAGMAYgBXAFUAHgMmAxkDJgMeAysDVQBUAF8AVQBfAGAAYABfAI0AYACNAI4AJwMWAyYDFgMnAxcD6ALnAhYD6AIWAxcDIQMgA/ICIQPyAvMCLAMgAyEDIAMsAysDaABZAGkAWQBoAFgAmACXAGgAmABoAGkAbQDbAG4A2wBtANoA2wDeAG4A3gDbAB4BHwPtAhwD7QIfA/ACLAMcAykDHAMsAx8DZABnAFkAWQBWAGQAkgCVAGcAZwBkAJIAjQDoAo4AjgDoAukCpAKlAhEDpQISAxEDowISA6ACowKgAl4CYAKlAYMCpQFgAqMBpQKmAacBpgGlAqQCIAFuAN4AbgAgAd0AIAGlAaMBpQEgAf0A2gCfAdsAnwHaAJ4BnwFeAqEBXgKfAaMCIAGDAP0AgwAgAd4AEQOjAqICowIRAxIDpwHdACABpwEgAaMBHgGDAN4AgwAeAfwApwHcAN0A3ACnAaYBYAKgAhIDYAISA6UCHgGgAfwAoAEeAaEBpQKnAaMBpQKjAWACHgGfAaEBnwEeAdsAoAGhAV4CoAFeAoICngGjAp8BowKeAaIC3ABuAN0AbgDcAG0A/AJeAqACXgL8AoIC/AKhAl8C/AJfAoICoAFfAqIBXwKgAYICHwH8AKABHwGgAaIBgwAfAd8AHwGDAPwAIQH9AIMAIQGDAN8ApQEhAaQBIQGlAf0AYQKlAaQBYQKDAqUBYQL8AoMC/AJhAqECHgPqAu8C6gIeAxkDZQCRAJMAkQBlAGMAjgBiAGAAYgCOAJAAjwBeAGEAXgCPAIwAIAPvAvIC7wIgAx4DBgP+AosC/gIGAy0D/gKZAZgB/gKYAYsCmAGAAPUAgACYAZkBgABRAHkAgAB5APUAeQB/APQAfwB5AFEA9ACWAZcBlgH0AH8AlgH9AooClgGKApcBLQOKAv0CigItAwYDigIGA64B9ACXAa4B9QB5AK4BiwKYAa4BlwGKAq4BeQD0AK4BmAH1AK4BZAN9A3wDZAN8A2IDHAC5AbIBHACyARoAugEdALsBugETAB0AugFlA2sDrQGJAggDiQKtAakBHQC0AbsBtAEdABsAAwAcABoAAwAaAAIAuQFkA2IDuQFiA7IBuwFjA2UDYwO7AbQBbAN/A34DbAN+A2oDagO5AbgBuQFqA2QDuQESALgBZAN+A30DZQO6AbsBfgNkA2oDEgC5ARwAAAASABwAHAADAAAAFADAAbgBFAC4ARIAwQFrA20DawPBAboBwAFsA2oDwAFqA7gBFAAAAAEAAAAUABIAFQC6AcEBugEVABMAAQAAAAMALwMHA2IDLwNiA3wDYgOsAbIBrAFiAwcDGgCsAXcArAEaALIBGgB3AAIAAgB3AFAAeAACAFAAAgB4ABsArQG0AXgAeAC0ARsAtAEIA2MDCAO0Aa0BfAMIAy8DCAN8A2MDUAD3AHgA9wBQAH4ArAH2AHcA9gCsAagBLwOIAgcDiAIvAwEDCAOJAi8DLwOJAgEDeAD3AK0BrQH3AKkBdwD2AFAAUAD2AH4ABwOIAqwBrAGIAqgBqAJoAmwCaAKoAqYCFgESAdgA2AASAdYABAOEAgIDhAIEA4YCcwDSANQA0gBzAG8AFQF8ANgAfAAVAeUAfAD6ABYBhgLDAYQCwwGGAssByQFnAmsCZwLJAbwBawIEA5sCBAM9AzwDBANrAqgCmwLJAWsCmwLIAckB+gDDAcsBwwH6APgAbAJoAsoBygFoAr0BFgHYAHwAfAD4APoA+AB8AHoAFQG8AckBvAEVAREB5QB6AHwAegDlAOMAawKmAqgCpgJrAmcCygH6AMsB+gDKARYB5QDCAeMAwgHlAMgBygGGAmwChgLKAcsBbAIEA6gCBANsAoYCyAGZAsIBmQLIAZsC2ADWABUBFQHWABEBvQEWAcoBFgG9ARIBmwICA5kCAgObAgQDyAEVAckByAHlABUBRgA/ADcANwA/ADMA0gHHAYsAiwDHAYkAIAAsADcALAAgABgALABGADcAGAA2ACgA+gCEAPgAhAD6AIYANgAyAEUARQAyAD4ARQBzAHwAfABEAEUATACEAIYAhABMAEoA1ABwAHQAcADUANIARgAsAEwATABPAFsATAB0AEYATACGAHQALABKAEwASgAsACoAIAAeADYANgAeADIARAA2AEUARAAoADYAGAAqACwAKgAYABYARQA+AHMAcwA+AG8AKAAWABgAFgAoACYAdAD6ANQA+gB0AIYAcwD6AHwA+gBzANQARAAmACgAJgBEAEIANwAzACAAIAAzAB4AdABwAEYARgBwAD8AfABCAEQAQgB8AHoANgAYACAAMAAuADwAPAAuADoAPQA7ADEAMQA7AC8AXQBcAIoAigBcAIgALQARAA0AEQAtADEATABaAEoAWgBMAFsACQAIABAAEAAIAA4ALAAMABAALAAQADAAWwBLAFoASwBbAE0APABOAE8ATgA8ADoALQA9ADEALQBNAD0ALQBLAE0ASwAtACsAEQAPAAkACQAPAAgADAAJABAACQAMAAcADQArAC0AKwANAAsAEAAOADAAMAAOAC4ATwA9AE0ATQBbAE8ABwALAA0ACwAHAAYATwBMADwATAAsADAATAAwADwADAAGAAcABgAMAAoAMQAvABEAEQAvAA8ATwA7AD0AOwBPAE4ALAAKAAwACgAsACoACQANABEADQAJAAcAOABAAEcAQAA4ADQA1QByAHYAcgDVANMA9gIiAyMDIgP2AvQCSAB9AHYAfQBIAEkAfQAXAfsAfQD7ANUATQCFAEsAhQBNAIcAOQA1ACEAIQA1AB8ALQAhADgAIQAtABkAhwD5AIUA+QCHAPsARwBAAHUAdQBAAHEA1QB2AH0A+wB1ANUA+wB7APkAewD7AH0AQQA5AEgAOQBBADUAOQAhABkAGQApADkASQB7AH0AewBJAEMAIQAfADgAOAAfADQAdQD7AIcAdQBNAEcASQAnAEMAJwBJACkATQB1AIcATQA4AEcATQAtADgAGQAnACkAJwAZABcAdgBBAEgAQQB2AHIAdQDTANUA0wB1AHEALQAXABkAFwAtACsASQA5ACkAOQBJAEgA2QDXABcBFwHXABMBbQKnAqkCpwJtAmkCigDGAdEBxgGKAIgAbgIFA6kCBQNuApwC+wDEAfkAxAH7AMwBzgEUARgBFAHOAb8BGAHZAH0AfQDmABgBzAGFAsQBhQLMAYcCFwG+Ac0BvgEXARMBbQIFA4cCBQNtAqkChwIDA4UCAwOHAgUDbgK/Ac4BvwFuAmoCzgHmAM8B5gDOARgBBQOaAgMDmgIFA5wCGAEUAdkA2QAUAdcAzQFtAocChwLMAc0BzwGaApwCmgLPAcUB+wDNAcwBzQH7ABcBFwF9ANkA5gDFAc8BxQHmAOQAqQJqAm4CagKpAqcCzQG+AW0CbQK+AWkCfQDkAOYA5AB9AHsAzwGcAm4CzwFuAs4BEAMMA60CrQIMA6sCPwM4A0wDTAM4A0gD9wL1AtIB0gH1AscBVQNhA2kDVQNxA1EDVQN1A3EDVQM/A0wDVQM1Az8DYQNVA0wD+wKFAocChQL7AvkCTQM5A0ADOQNNA0kDQAMFA0EDBQNAAxADQQNZA00DQQNNA0ADEAOtAocCNQP5AvsC+QI1AzMDrQKrAg8DDwOrAgsDNQNRA0UDVQMzAzUDMwNVA1MDYQNJA00DSQNhA18DVQNnA1MDZwNVA2kDQAM5AxADEAM5AwwDNQMPAz8DNQP7Ag8DaQNXA2cDVwNpA1kDDwOHAq0ChwIPA/sChwIFAxADQQNXA1kDVwNBAzsDTANfA2EDXwNMA0gDDwMLAz8DPwMLAzgDBQM7A0EDOwMFAwMDTQNpA2EDaQNNA1kDUQNPA0UDRQNPA0MDRANOA1ADTgNEA0ID0QH0AvYC9ALRAcYBVANwA3QDcANUA1ADJQMzAzUDMwMlAyQDdwNvA3EDbwN3A3YDUQM1A1UDNAMkAyUDJAM0AzIDRQNDAzEDMQNDAzADVAM0A0QDVANEA1ADVAMyAzQDMgNUA1IDcAN2A3cDdgNwA24DdwNxA3UDdQN5A3cDVANyA1IDcgNUA3QDcQNvA1EDUQNvA08DNAMxA0QDMQM0AyUDdAN4A3IDeAN0A3kDMQM1A0UDNQMxAyUDeQNzA3gDcwN5A3UDUANuA3ADbgNQA04DMQMwA0QDRAMwA0IDVQNzA3UDcwNVA1MDcAN5A3QDeQNwA3cDSwM3Az4DNwNLA0cDrAKqAg0DDQOqAgkDiwCJAF0AXQCJAFwAPQMEAw0DNAP4AjID+AI0A/oCSgNeA2ADXgNKA0YDYANLA1QDVANoA2ADaANKA2ADhgL4AvoC+AKGAoQCPgM3Aw4DDgM3AwoDDQOGAqwChgINAwQDIwP1AvcC9QIjAyIDPQNGA0oDRgM9AzYDSgNoA1gDBAM6AwIDOgMEAzwDYANHA0sDRwNgA14DDgOsAoYChgL6Ag4DPANWAzoDVgM8A1gDNAMOA/oCNANUA0sDNANLAz4DDgM0Az4DaANWA1gDVgNoA2YDDQM2Az0DNgMNAwkDDgMKA6wCrAIKA6oCaANSA2YDUgNoA1QDPANKA1gDSgM8Az0D0gFbA/cCWwPSAdMBIwDSAYsAIwDTAdIBWgP2AnoDegP2AiMDigAEAF0ABACKACIA9wJ6AyMDegP3AlsDXQAjAIsAIwBdAAQA9gLQAdEB0AH2AloD0AEiAIoA0AGKANEBXAPYAdABXAPQAVoDJQAEAAUABAAlACMA2AEkACIA2AEiANAB2QEjACUAIwDZAdMBXQN7A3oDXQN6A1sDXQPTAdkB0wFdA1sDewNcA1oDewNaA3oDBQAiACQAIgAFAAQAUAExAjICMQJQAU8BUgEnAjQCJwJSAUUBjgHzAfAB8wGOAYsBKgIpAkgBSAEpAkcBUwExAk8BMQJTATUCSQErAioCSQEqAkgBOAJVAVYBVQE4AjcCVAE1AlMBNQJUATYCTQFOAS8CMAIvAk4BUAE5AlcBOQJQATIChQGIAYYBiAGFAYcBNgJUAVIBNgJSATQCLQIsAksBLAJKAUsBLAIrAkoBKwJJAUoBOgI5Al0COQJ/Al0CfwJ+Al0CfgJ/ApgCXQI4AjoCOAJdAn4CfQIzAjcCMwJ9AlwCeQJcApcCfQKXAlwCXAJ5AjACXAIwAjMCdgJbAi8CLwJbAi4CdgKSAlsCkgJ1AlsCWwJ1Ai0CWwItAi4CJAFXAVgBJAEQAVcBJAFYAVYBJAFWAQ8BEAEPAfMADwEQASQBDgFRASMBUQEOAVUBUQFOASMBTgEKASMBCgHyACMB8gAOASMBBwFNASIBTQFMASIBTAFLASIBSwEGASIBBwEiAe0A7QAiAQYBcAIpAnECKAIpAnACcQKNAnACjQJxAo4CdwKVAngClQJ3ApQCMgIxAncCMgJ3AngCeAKYAn8CmAJ4ApUCOQIyAngCOQJ4An8CfQJ+ApgCmAKXAn0CNwI4An4CfgJ9AjcCdgKXApMClwJ2AnkCeQJ2Ai8CeQIvAjACkwKSAnYCNQJ3AjECdwI1AnsClgKUAncCdwJ7ApYCfAI1AjYCNQJ8AnsCewJ8ApYCfAI0AnoCNAJ8AjYCfAJ6ApYCegInAm8CJwJ6AjQCjAKWAnoCegJvAowCcgIpAioCcgJxAikCjwJxAnICjwKOAnECKwJyAioCcgIrAnMCcwKPAnICjwJzApACdAJzAisCdAIrAiwCkQKQAnMCcwJ0ApECdQJ0AiwCdQIsAi0CkgJ0AnUCkgKRAnQCAgEBAegA6ADpAAIBRwFGAQEBAQECAUcBCAFQAQkBUAEIAU8BCAEJAfAACAHwAO8ACQFXARABVwEJAVAB8wDwAAkBCQEQAfMADwFVAQ4BVQEPAVYB8wAOAfIADgHzAA8BTQEKAU4BCgFNAQcBBwHyAAoB8gAHAe4ABAKEAYYBhAEEAgICBwHtAO4ACAHxAAwB8QAIAe8ACAFTAU8BCAEMAVMB8QANAQwBDQFTAQwBUwENAVQB8QALAQ0BVAENAVIBDQELAVIBCwHnAAAB5wALAfEARQFSAQsBCwEAAUUB6gACAekAAgHqAAMBAwFHAQIBRwEDAUgB6gDrAAQB6gAEAQMBSQEDAQQBAwFJAUgB7AAEAesABAHsAAUBSQEFAUoBBQFJAQQB7QAFAewABQHtAAYBSgEGAUsBBgFKAQUBTAEuAi0CLQJLAUwBLwJMAU0BTAEvAi4COgJWAVgBVgE6AjgCVwE6AlgBOgJXATkCUQEzAjACUQEwAk4BVQE3AjMCVQEzAlEBRgEpAigCKQJGAUcBhgGIAQQCiAEGAgQCBAIGAgMCAwIGAgUCAwKHAYUBhwEDAgUCAgKAAYQBgAECAv4BAwICAgQCAgIDAgEChQEBAgMCAQKFAYMBhgGEAYUBhQGEAYMB/wH8Af4B/AH/AfsBAQL+AQIC/gEBAv8BhAGAAYMBgwGAAYEBgwH/AQEC/wGDAYEBewF9AXwBfQF+AXwBfgGBAYABgQF+AX0BgQH7Af8B+wGBAX0B/gF+AYABfgH+AfwB+wF7AfkB+wF9AXsB/AF8AX4BfAH8AfoB+wH6AfwB+gH7AfkB+QH3AfgB+QH4AfoBegF7AXwBewF6AXkBiwH9AfMB/QGLAX8BewH3AfkB9wF7AXkBjAH0AQACAAKCAYwB+gF6AXwBegH6AfgBfwGCAQACAAL9AX8BAALzAf0B8wEAAvQBggF/AYsBggGLAYwBjgGRAY8BkwGPAZEB9AHwAfMB8AH0AfEB8QH0AY8B9AGMAY8BjwGMAY4BjAGLAY4B7QGQAZEBkAHtAewB8AGRAY4BkQHwAe0B8QHtAfAB7QHxAe8BjwGTAfEB7wHxAZMB6gHsAesB7AHuAesB7gHtAe8B7QHuAewBkwGSAe8B7gHvAZIBkgGRAZABkQGSAZMBlQHqAesB6gGVAZQBkgGVAe4B6wHuAZUBlQGQAZQBkAGVAZIB7AGUAZABlAHsAeoBtwHiAuYC4gK3Aa8BnACbAZkAmwGcAKsBqwHeApsB3gKrAeECigH+AP8A/gCKAYkB4gLjAuAC4wLhAuAC4AKrAaoBqwHgAuECqwGbAKoBnQCwAa8BsAHiAq8BsAGdAJ4AmwCrAZwA4gKwAeMCnQCcAJ4AnACdAJsAoQCvAbcBrwGhAJ0A/gCbAJ0A4gLgAoACoQD/AJ0A/wD+AJ0AgQLiAoAC4gKBAuYCgQKAAvYB9gGAAvUB/wChALcBqgGbAP4AiQGKAfYB9gH1AYkBFQPfAhQD3wIVA+UCYwJMAmICTAJjAk8C1QEvATAB1QFOAk0CLwHVAdQB1gFMAk8CTALWAbYB1gEyATEB1gFPAlACMgEcATEBMgHWAdcBMQG2AdYBtgExAS4BZAJQAk8CUALXAdYBTwJjAmQCGwEuATEBLgEbARoBLwEaARsBGgEvAS0BLwG1AS0BtQEvAdQBTgJkAmMCTQLUAdUB1AFLArUBSwLUAU0CGwExARwBTQJiAksCYgJNAmMCUAJlAlICZQJQAmQC3QFQAlICUALdAdcBNAHXAd0B1wE0ATIBHQEyATQBMgEdARwBMwEcAR0BHAEzATABMwHVATAB1QEzAdoB2gFOAtUBTgLaAVECUQJkAk4CZAJRAmUCHAEwARsBLwEbATABYwJNAk4CPgLbAeAB2wE+AjsCUgJlAlMCUgJTAjwC3AFSAjwCUgLcAd0BNAHdAdwBNAHcAUIBQgErATQBHQE0ASsBHQFBATMBQQEdASsBMwHbAdoB2wEzAUEBOwJRAtsB2wFRAtoBZQJRAjsCOwJTAmUCRAHhAeAB4QFEASYBRAErASwBKwFEAUEBQwFCAd8B3wFCAdwBUwJUAjwCPQI8AlQCOwI+AlMCVAJTAj4C4AHbAUQBRAHbAUEBLAFCAUMBQgEsASsB3wE8Aj0CPALfAdwBnwJZAloCWQKfAp4C4ABDASUBQwHgACwB3gHfAVUC3wE9AlUC4QFWAj4C4QE+AuABJgEsAeAALAEmAUQB3wElAUMBJQHfAd4BnQI9AlQCnQJVAj0CVAI+AlYCVAJWAp0C4wHiAVgC4gFXAlgC4gEoAScBKAHiAeMBWgLlAecB5QFaAlkCKQHiACoB4gApAeEAWAJXAp8CVwKeAp8C5QEqAecBKgHlASkB4gAnASgBJwHiAOEAJwElAeIB4gElAd4BKQHgAOEA4AApASYBWQLhAeUB4QFZAlYC4gFVAlcCVQLiAd4B4QAlAScBJQHhAOAA5QHhASkBKQHhASYBVgJZAp0CngKdAlkCnQKeAlUCVwJVAp4CZgI/AlgCWAKfAmYCPwLyAeYBPwJmAvIB5AHjAT8C4wFYAj8C5AE/AuYBQAHjAeQB4wFAASgBQAHkAeYBGQEoAUABKAEZAeIAjQFAAeYBQAGNARkBPwHiABkB4gA/ASoB6AEZAY0BGQHoAT8B6QEqAT8BKgHpAecB6QE/AegBQAJaAukBWgLnAekBQALpAegBQAJmAp8CnwJaAkAC6AFmAkACZgLoAfIBjQHmAfIB6AGNAfIBOAFEAjkBOQFEAkUCRwJFAkYCRgJFAkQCRgI7AUcCOwFGAjoBOgE5ATsBOQE6ATgBSgI+AToBSgI6AUYCRAI4ATYBRAI2AUICOAE+ATYBPgE4AToBRgJEAkICQgJKAkYCSQI+AUoCPgFJAj0BCwJdAQ0CXwENAl0BNwE8AUgCSAJDAjcBQwI1ATcBNQFDAkECPQFJAkgCPQFIAjwBNgFBAkICQQI2ATUBBwJZAQkCWwEJAlkBXAFZAVoBWQFcAVsBWgEHAggCBwJaAVkBCQIKAgcCCgIIAgcCCQJcAQoCXAEJAlsBYAFdAV4BXQFgAV8BXgELAgwCCwJeAV0BDQIOAgsCDgIMAgsCDQJgAQ4CYAENAl8BDwJhARECYwERAmEBZAFhAWIBYQFkAWMBYgEPAhACDwJiAWEBEQISAg8CEgIQAg8CEQJkARICZAERAmMBEwJlARUCZwEVAmUBaAFlAWYBZQFoAWcBZgETAhQCEwJmAWUBFQIWAhMCFgIUAhMCFQJoARYCaAEVAmcBFwJpARkCawEZAmkBbAFpAWoBaQFsAWsBagEXAhgCFwJqAWkBGQIaAhcCGgIYAhcCGQJsARoCbAEZAmsBGwJtAR0CbwEdAm0BcAFtAW4BbQFwAW8BbgEbAhwCGwJuAW0BHQIeAhsCHgIcAhsCHQJwAR4CcAEdAm8BHwJxASECcwEhAnEBdAFxAXIBcQF0AXMBcgEfAiACHwJyAXEBIQIiAh8CIgIgAh8CIQJ0ASICdAEhAnMBIwJ1ASUCdwElAnUBeAF1AXYBdQF4AXcBdgEjAiQCIwJ2AXUBJQImAiMCJgIkAiMCJQJ4ASYCeAElAncBrgKiALACpACwAqIApQCiAKMAogClAKQAogCvAqMArwKiAK4CsAKxAq4CsQKvAq4CsQKkAKUApACxArACsgKmALQCqAC0AqYAqQCmAKcApgCpAKgApgCzAqcAswKmALICtAK1ArICtQKzArICtQKoAKkAqAC1ArQCtgKqALgCrAC4AqoArQCqAKsAqgCtAKwAqgC3AqsAtwKqALYCuAK5ArYCuQK3ArYCuQKsAK0ArAC5ArgCugKuALwCsAC8Aq4AsQCuAK8ArgCxALAArgC7Aq8AuwKuALoCvAK9AroCvQK7AroCvQKwALEAsAC9ArwCvgKyAMACtADAArIAtQCyALMAsgC1ALQAsgC/ArMAvwKyAL4CwALBAr4CwQK/Ar4CwQK0ALUAtADBAsACwgK2AMQCuADEArYAuQC2ALcAtgC5ALgAtgDDArcAwwK2AMICxALFAsICxQLDAsICxQK4ALkAuADFAsQCxgK6AMgCvADIAroAvQC6ALsAugC9ALwAugDHArsAxwK6AMYCyALJAsYCyQLHAsYCyQK8AL0AvADJAsgCygK+AMwCwADMAr4AwQC+AL8AvgDBAMAAvgDLAr8AywK+AMoCzALNAsoCzQLLAsoCzQLAAMEAwADNAswCzgLCANACxADQAsIAxQDCAMMAwgDFAMQAwgDPAsMAzwLCAM4C0ALRAs4C0QLPAs4C0QLEAMUAxADRAtAC0gLGANQCyADUAsYAyQDGAMcAxgDJAMgAxgDTAscA0wLGANIC1ALVAtIC1QLTAtIC1QLIAMkAyADVAtQC1gLKANgCzADYAsoAzQDKAMsAygDNAMwAygDXAssA1wLKANYC2ALZAtYC2QLXAtYC2QLMAM0AzADZAtgC2gLOANwC0ADcAs4A0QDOAM8AzgDRANAAzgDbAs8A2wLOANoC3ALdAtoC3QLbAtoC3QLQANEA0ADdAtwCmQBqAJwAagBrAJwAawCfAJ4AswHlAhUDFQPkArMB5AKxAbMBsQGfALMBnwBrALMB4QLjAhUD4wLkAhUD4QIVAxQDXwKhAmECYQKkAV8ClAKTApUCkwKUApECIQHfAB8BHwGiASEBwQFtA38DfwNsA8ABfwMBAMEBwAEUAAEAAQB/A8ABAQAVAMEBJAB7AwUAewNdAwUAEwADAAIAAgAbABMAawNlA2MDlQKXApgClwKVApMCGwAdABMAqQF+AAEDfgD2AKgBAQN+AIgCfQN+A38DFQABAAMAFQADABMAAQOJAqkBqQH3AH4AqAGIAn4AawN9A20DfwNtA30DXQPZASUAJQAFAF0DXwKkAaIBIQGiAaQBJADYAVwDXAN7AyQA6QDoAOcA5wDxAOoANgE+ATwBNgE8ATcB8QDrAOoAYwN8A2sDfQNrA3wD6gDpAOcA7gDtAOwAjAKNAo4CjAKOAo8C6wDvAOwA6wDxAO8AlgKMAo8CkAKWAo8C8ADyAO4A8gDwAPMA7wDuAOwA7gDvAPAAkQKSApMCtwHmAoECgQL2AbcB9gGKAbcBigH/ALcB/gCJAaoBiQH1AaoB9QGAAqoBgALgAqoBSQJKAkgClgKQApQClAKQApECQgJBAkMCNQE2ATcBSAJCAkMCQgJIAkoCPgE9ATwB","c":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA","pal":["3c4048"]},"knife":{"s":1.3428144169438766e-05,"p":"BPyU33ELBPxD4IP0BPwt4X0LBPzb4Y/0Qfz/4PUIQfyH4RT3Rfwj4y4IRfyf4/r3kvxH4VECkvxs50YCkvyO7D0Ckvwp7zgCkvwv9C8CkvyD+CcCkvwe+yICkvwl/xsCkvylBBECkvxBBwwCkvyADAMCkvykEjUBkvzhGGQAkvyHHkH9fv2I4cn5fv296Mz6fv197Pv3fv0Y7173fv3Z9P34fv0u+YL2fv3L+2H2fv0NAIb4fv2VBd/1fv0zCLz1fv20DfX3fv3UEzb4fv2hG6Lyfv0AIIz3N/6E34oNN/5T4GryN/4d4ZYNN/7r4XbyUv7y4JgKUv6T4XH1VP4Y46oJVP6q43727P4i4SQH7P4V5xoH7P527BAH7P4R7wsH7P778wIH7P5U+PoG7P7v+vYG7P7q/u4G7P5vBOQG7P4LB+AG7P48DNYG7P5dEgkG7P5/GDsF7P4oHhYCFv8/1vn2Fv9V4f72HP93ixwDHP9ekqIAHP+1kkwDHP/LkvoDHP9SnJT/HP+UnLkEHP+lnAcGHP8YpyYKHP8gp/sHHP9Ap3X/HP8UvjQKHP8pvgoIHP99voX/HP/iwDUKHP/4wAsIHP9SwYf/HP+vwzcKHP/Hww0IHP8nxIn/HP98xjkKHP+Wxg8IHP/8xov/HP9KyToKHP9lyREIHP/RyY3/HP8XzDwKHP80zBIIHP+mzI//HP/kzj4KHP8DzxQIHP97z5H/HP+y0T8KHP/S0RYIHP9Q0pP/HP9/1EEKHP+h1BcIHP8P1UEKHP8l1ZX/HP8x1RgIHP+31Zb/HP+i4EsKHP+24CEIHP8F4Z3/Sv8BgF0D4v9ZgbUB4v/GiG774v9RkVf44v/lmyv34v9hp9P24v/Rvub24v+swej24v+IxOv24v9jx+324v8+yu/24v8azfL24v/1z/T24v/Q0vb24v+s1fj2HQBZgbUBHQDGiG77HQBRkVf4HQDlmyv3HQBhp9P2HQDRvub2HQCswej2HQCIxOv2HQBjx+32HQA+yu/2HQAazfL2HQD1z/T2HQDQ0vb2HQCs1fj2tAABgF0D4gB3ixwD4gBekqIA4gC1kkwD4gDLkvoD4gBSnJT/4gCUnLkE4gClnAcG4gAYpyYK4gAgp/sH4gBAp3X/4gAUvjQK4gApvgoI4gB9voX/4gDiwDUK4gD4wAsI4gBSwYf/4gCvwzcK4gDHww0I4gAnxIn/4gB8xjkK4gCWxg8I4gD8xov/4gBKyToK4gBlyREI4gDRyY3/4gAXzDwK4gA0zBII4gCmzI//4gDkzj4K4gADzxQI4gB7z5H/4gCx0T8K4gDS0RYI4gBQ0pP/4gB/1EEK4gCh1BcI4gAP1UEK4gAl1ZX/4gAx1RgI4gC31Zb/4gCi4EsK4gC24CEI4gAF4Z3/6AA/1vn26ABV4f72FAEi4SQHFAEV5xoHFAF27BAHFAER7wsHFAH78wIHFAFU+PoGFAHv+vYGFAHq/u4GFAFvBOQGFAELB+AGFAE8DNYGFAFdEgkGFAF/GDsFFAEoHhYCrAEY46oJrAGq4372rgHy4JgKrgGT4XH1yQGE34oNyQFT4GryyQEd4ZYNyQHr4XbyggKI4cn5ggK96Mz6ggJ97Pv3ggIY7173ggLZ9P34ggIu+YL2ggLL+2H2ggINAIb4ggKVBd/1ggIzCLz1ggK0DfX3ggLUEzb4ggKhG6LyggIAIIz3bgNH4VECbgNs50YCbgOO7D0CbgMp7zgCbgMv9C8CbgOD+CcCbgMe+yICbgMl/xsCbgOlBBECbgNBBwwCbgOADAMCbgOkEjUBbgPhGGQAbgOHHkH9uwMj4y4IuwOf4/r3vwP/4PUIvwOH4RT3/AOU33EL/AND4IP0/AMt4X0L/APb4Y/0","i":"3wDSAOAA3wDRANIANAAPADMADwA0ABAA3QDeALoAuQDdALoANgARADUAEQA2ABIA3wDgALwAuwDfALwA2gDbALcAtgDaALcAMwAOADIADgAzAA8A3ADdALkAuADcALkAMQAMADAADAAxAA0A1wDYALQAswDXALQAMAALAC8ACwAwAAwA2QDaALYAtQDZALYALgAJAC0ACQAuAAoAuwDeAN8A3gC7ALoANQAQADQAEAA1ABEAuADbANwA2wC4ALcADQAyAA4AMgANADEAtQDYANkA2AC1ALQACgAvAAsALwAKAC4ALQAIACwACAAtAAkANwASADYAEgA3ABMAvgDhAOIA4QC+AL0AvwDiAOMAvgDiAL8AOAATADcAEwA4ABQAswDWANcA1gCzALIAvQDgAOEA4AC9ALwAFAA5ABUAOQAUADgA2ADJAMoAyQDYANcA0AAeAB8A0AAfANEA3wDQANEA0ADfAN4ANAC7ADUAugC7ADQAzQAcAM4AzQAbABwA3ADNAM4AzQDcANsAHgAQABEAHgARAB8AMQC4ADIAuAAxALcAygAZAMsAygAYABkA2QDKAMsAygDZANgAGwAOABwAGwANAA4ALgC1AC8AtAC1AC4A4QDSANMA0gDhAOAAIAASABMAIAATACEA0AAdAB4AHQDQAM8AHQAOAA8ADgAdABwA0gAfACAAHwDSANEA3gDPANAAzwDeAN0A3ADOAM8A3ADPAN0AGAAKAAsAGAALABkAMgC5ADMAuAC5ADIAzAAaABsAzAAbAM0AGgALAAwACwAaABkAEAAeAB0AHQAPABAAzgAcAB0AzgAdAM8A2QDLAMwA2QDMANoAvQA2ALwANgC9ADcAMwC6ADQAuQC6ADMALwC2ADAAtQC2AC8AyQAXABgAyQAYAMoACgAYABcAFwAJAAoADQAbABoAGgAMAA0AzAAZABoAGQDMAMsAMAC3ADEAtgC3ADAA2wDMAM0AzADbANoALgCzALQAswAuAC0AIQATABQAIQAUACIAIQDSACAA0gAhANMAOQC+AL8AvgA5ADgA4gDTANQA0wDiAOEAIgAVACMAIgAUABUAIgDTACEA0wAiANQAIwDjANUAOQDjABUA4wA5AL8A4wAjABUAyAAXAMkAyAAWABcAFgAIAAkAFgAJABcAFgDIANYALADWALIA1gAIABYACADWACwALQCyALMAsgAtACwAIAARABIAEQAgAB8A1QAiACMA1QDUACIANQC8ADYAuwC8ADUA4wDUANUA1ADjAOIA1wDIAMkAyADXANYAvgA3AL0ANwC+ADgARgCQAEcAkABGAI8ASQCTAJIAkwBJAEoAlQBNAEwAlgBNAJUAmQBPAFAATwCZAJgAmwBTAFIAnABTAJsAnwBVAFYAVQCfAJ4AWACiAFkAogBYAKEApABcAKUAXACkAFsAWQClAFwAogClAFkAUwCfAFYAnACfAFMATQCZAFAAlgCZAE0AkACTAEoARwCQAEoAkACOAJEAjgCQAI0AjgCNAIoAiQCOAIoAiQCKAIcAhgCJAIcARABCAEMAQgBEAEEAQQA/AEIAPwBBAD4ARABGAEcAQwBGAEQAXwBgAGIAXwBeAGAAqwCqAKwAqgCrAKgArgCsAK8ArACuAKsAYgBkAGUAYABkAGIAkwCRAJQAkQCTAJAAqACmAKoApgCoAKUApQCjAKYAowClAKIAogCgAKMAoACiAJ8AnwCdAKAAnQCfAJwAnACaAJ0AmgCcAJkAmQCXAJoAlwCZAJYAlgCUAJcAlACWAJMAXABeAF8AXABbAF4AVgBYAFkAVgBVAFgAUABPAFIAUABSAFMASgBMAE0ASgBJAEwASwBNAE4ASwBKAE0ATgBNAFAATgBQAFEAUQBQAFMAUQBTAFQAVABTAFYAVABWAFcAVwBWAFkAVwBZAFoAWgBcAF0AWgBZAFwAXQBcAF8AXQBfAGEASQCVAEwASQCSAJUATwCbAFIATwCYAJsAVQChAFgAVQCeAKEAWwCkAKcAWwCnAF4AlQCTAJYAkwCVAJIAmwCZAJwAmQCbAJgAoQCfAKIAnwChAJ4ApwClAKgApQCnAKQASABHAEoASABKAEsArwCwALEAsACvAKwAYwBiAGUAYwBlAGYAYACtAGQAqQCtAGAAOgBjAGYAOgBmADsAsQA6ADsAOgCxALAArQCrAK4AqwCtAKkAqQCoAKsAqACpAKcAOgCDAHUAOgCwAIMAdQBjADoAdQBhAGMAXgCpAGAAXgCnAKkAYQBiAGMAYQBfAGIArACDALAAgwCsAKoARQBHAEgARABHAEUAQAA+AEEAPgBAAD0ARQBBAEQAQQBFAEAAPwA+ADwAPQA8AD4APwCFAIgAhQA/ADwAhwCKAIgAiwCIAIoAQgCIAIsAiABCAD8AhwCIAIUAhQCGAIcAigCNAIsAjACLAI0AQwCLAIwAiwBDAEIAQwCPAEYAjACPAEMAjwCNAJAAjQCPAIwAxQAlAMcAJwDHACUAAwAlAAEAJQADACcAJgAAACQAAAAmAAIA6wDFAMcAxQDrAOkA6ADqAMYAxADoAMYAJADoAMQAxQABACUAAQDpAAAA6AAkAAAA6QABAMUAAADpAOgA6ADrAOoA6wDoAOkAAwDrACcAJwDrAMcAAgDrAAMAAgDqAOsAJgDqAAIA6gAmAMYAJADEACYAxgAmAMQAAQACAAMAAgABAAAAlwB8AH0AfACXAJQAmgB9AH4AfQCaAJcAnQB+AH8AfgCdAJoAoAB/AIAAfwCgAJ0AowCAAIEAgACjAKAApgCBAIIAgQCmAKMAqgCCAIMAggCqAKYAbgBOAG8AbgBLAE4AbwBOAFEAbwBRAHAAcABUAHEAcABRAFQAcQBXAHIAcQBUAFcAcgBaAHMAcgBXAFoAcwBaAF0AcwBdAHQAdABhAHUAdABdAGEAfABvAH0AbgBvAHwAfQBwAH4AbwBwAH0AfgBxAH8AcABxAH4AfwByAIAAcQByAH8AgABzAIEAcgBzAIAAgQB0AIIAcwB0AIEAggB1AIMAdAB1AIIAewBuAHwAbQBuAHsAbQBIAEsAbQBLAG4AlAB7AHwAewCUAJEAaAA8AGkAPABoAGcAkQB6AHsAegCRAI4AjgB5AHoAeQCOAIkAhgB3AHgAdwCGAIUAiQB4AHkAeACJAIYAdgBpAHcAaABpAHYAPACEAIUAhAA8AGcAhQB2AHcAdgCFAIQAPQBqAGkAPAA9AGkAdwBqAHgAaQBqAHcAZwBoAIQAdgCEAGgAeABrAHkAagBrAHgAawBAAEUAawBFAGwAagA9AEAAagBAAGsAbABIAG0ARQBIAGwAegBtAHsAbABtAHoAeQBsAHoAawBsAHkAwADmAOQA5gDAAMIABAAqAAYAKAAqAAQAwQApACsAKQDBAMMA5ADnAOUA5wDkAOYA5QDDAMEAwwDlAOcAKQAHACsABQAHACkABQAEAAYABQAGAAcAKADAACoAwAAoAMIAwQAHAOUABwDkAOUABwDBACsAKgDkAAYA5AAHAAYA5AAqAMAAwwAFACkAwgAEAOYABADCACgABQDDAOcA5gAEAAUA5wDmAAUA","c":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMCAgICAgICAgICAgICAgICAgICAgICAgICAgIC","pal":["776644","777777","887755","999999"]},"katana":{"s":4.1627246925260166e-05,"p":"yPvU74IEyPvP8IIEWPzU77MAWPzU71AIWPzP8LMAWPzP8FAIP/2P74IEnv2P7yEDnv2P7+IFqP3p8YIEwv3Y8IIE5P3U7+r95P3U7xoL5P3P8Or95P3P8BoL+f3p8XsC+f3p8YgGD/7Y8HwCD/7Y8IcGP/7p8YIEP/6HEIIEdv7CEIIEe/7p8SEDe/7p8eIFe/6HECEDe/6HEOIFoP6P7x8CoP6P7+QGo/50EYIEqv7CEEwDqv7CELcF0f50EWED0f50EaIF1f7p8QAB1f7p8QMI4f7Y8AEB4f7Y8AMIH//p8R8CH//p8eQGH/+HEB8CH/+HEOQGO//CEGkCO//CEJoGUf90EY4CUf90EXUGdP+rin/3dP+tlKn5dP+1npn7dP/FqE39dP/Zssb+dP/zvAQAdP8QxwYBdP8x0cwBdP9U21cCdP945acCdP9m7bcCdP+e77sCnv9XhnX2yf/8gWn19P+xgBr1AAABgOHzAACigUT0AAAAg3z4AAADhBL5AACThm/1AACkhpj6AAAYirz7AADQinD2AAAolOn9AADOlJn4AABAntz/AADTnoj6AABeqJMBAADeqDz8AACCsg4DAADvsrT9AACqvE0EAAAFvfH+AADXxlEFAAAfx/P/AAAG0RkGAAA80bkAAAA426QGAABb20QBAABs5fQGAAB75ZMBAABm7aMBAABm7QQHAACP78EBAACP74IEAACP70IHAACe76gBAACe7wkHAADU7+T8AADU7x8MAADP8OT8AADP8B8MAADY8HYAAADY8I0IAADp8XUAAADp8cEBAADp8UIHAADp8Y4IAACHEMEBAACHEEIHAADCEBYCAADCEO0GAAB0EUECAAB0EYIEAAB0EcIGDACxgBr1NwD8gWn1YgBXhnX2jACrin/3jACtlKn5jAC1npn7jADFqE39jADZssb+jADzvAQAjAAQxwYBjAAx0cwBjABU21cCjAB45acCjABm7bcCjACe77sCrwB0EY4CrwB0EXUGxQDCEGkCxQDCEJoG4QDp8R8C4QDp8eQG4QCHEB8C4QCHEOQGHwHY8AEBHwHY8AMIKwHp8QABKwHp8QMILwF0EWEDLwF0EaIFVgHCEEwDVgHCELcFXQF0EYIEYAGP7x8CYAGP7+QGhQHp8SEDhQHp8eIFhQGHECEDhQGHEOIFigHCEIIEwQHp8YIEwQGHEIIE8QHY8HwC8QHY8IcGBwLp8XsCBwLp8YgGHALU7+r9HALU7xoLHALP8Or9HALP8BoLPgLY8IIEWALp8YIEYgKP7yEDYgKP7+IFwQKP74IEqAPU77MAqAPU71AIqAPP8LMAqAPP8FAIOATU74IEOATP8IIE","i":"bgA7ADwAWwA4AHwAOQBAAD0APQA6ADkAcQBAAEMAQABxAHAAPgA7AG4AOQBBAEIAQgAtADkAfAA4AFwAbwBBAD8AQQBvAHAAVgBVADYANgA3AFYAfABWAFsAVgB8AHsAegBXAFQAVwB6AHsANwBXAFwAXAA4ADcAVQBTADUANQA2AFUAegBTAFUAUwB6AHkAeQBUAFIAVAB5AHoANQBSAFQAVAA2ADUAUwBRADQANAA1AFMAeQBRAFMAUQB5AHgAeABSAFAAUgB4AHkANABQAFIAUgA1ADQAUQBPADMAMwA0AFEAeABPAFEATwB4AHcAdwBQAE4AUAB3AHgAMwBOAFAAUAA0ADMATwBNADIAMgAzAE8AdwBNAE8ATQB3AHYAdgBOAEwATgB2AHcAMgBMAE4ATgAzADIATQBLADEAMQAyAE0AdgBLAE0ASwB2AHUAdQBMAEoATAB1AHYAMQBKAEwATAAyADEASwBJADAAMAAxAEsAdQBJAEsASQB1AHQAdABKAEgASgB0AHUAMABIAEoASgAxADAASQBHAC8ALwAwAEkAdABHAEkARwB0AHMAcwBIAEYASABzAHQALwBGAEgASAAwAC8ARwBFAC4ALgAvAEcAcwBFAEcARQBzAHIAcgBGAEQARgByAHMALgBEAEYARgAvAC4ARQBDAC0ALQAuAEUAcgBDAEUAQwByAHEAcQBEAEIARABxAHIALQBCAEQARAAuAC0ALQBDAEAAQAA5AC0AcAA9AEAAPQBwAG8AOgA/AEEAQQA5ADoAOABbAFYAVgA3ADgAewBVAFYAVQB7AHoAewBcAFcAXAB7AHwANgBUAFcAVwA3ADYAcABCAEEAQgBwAHEAPQA8ADsAOwA6AD0AOwA+AD8APwA6ADsAbwA+AG4APgBvAD8AbwA8AD0APABvAG4AJQAWABgAGAAnACUAZwAlACcAJQBnAGQAgwBkAGcAZACDAIEAkgCBAIMAgQCSAJAAlgCQAJIAkACWAJUAkwCVAJYAlQCTAJEAhACRAJMAkQCEAIIAZQCCAIQAhABoAGUAJgBlAGgAaAAoACYAFwAmACgAKAAZABcAEwAXABkAGQAUABMAFgATABQAFAAYABYAKwAfAGwAawArAGwAfQBrAGwAiQB9AGwAjQCJAGwAigCNAGwAfgCKAGwAbQB+AGwALABtAGwAIAAsAGwAHAAgAGwAHwAcAGwABwAaAFkAGgBYAFkAWACOAFkAjgChAFkAoQCjAFkAowCiAFkAogCPAFkAjwBaAFkAWgAbAFkAGwAIAFkACAAGAFkABgAHAFkACwACAAQABAANAAsAXwALAA0ACwBfAF0AnQBdAF8AXQCdAJsApgCbAJ0AmwCmAKQAqQCkAKYApACpAKgApwCoAKkAqACnAKUAngClAKcApQCeAJwAXgCcAJ4AngBgAF4ADABeAGAAYAAOAAwAAwAMAA4ADgAFAAMAAAADAAUABQABAAAAAgAAAAEAAQAEAAIAFgAlACEAIQAPABYABwALABoACwAHAAIAJQBkAGMAYwAhACUAGgBdAFgAXQAaAAsAZACBAIcAhwBjAGQAWACbAI4AmwBYAF0AgQCQAJkAmQCHAIEAjgCkAKEApACOAJsAkACVAKAAoACZAJAAoQCoAKMAqAChAKQAlQCRAJoAmgCgAJUAowClAKIApQCjAKgAkQCCAIgAiACaAJEAogCcAI8AnACiAKUAggBlAGYAZgCIAIIAjwBeAFoAXgCPAJwAZQAmACIAIgBmAGUAWgAMABsADABaAF4AJgAXABAAEAAiACYAGwADAAgAAwAbAAwAFwATAAkACQAQABcACAAAAAYAAAAIAAMACQATABYAFgAPAAkABgACAAcAAgAGAAAAHQApACcAJwAYAB0AKQBpAGcAZwAnACkAaQB/AIMAgwBnAGkAfwCLAJIAkgCDAH8AkgCUAJYAlACSAIsAlgCMAJMAjACWAJQAjACAAIQAhACTAIwAgABqAGgAaACEAIAAagAqACgAKABoAGoAKgAeABkAGQAoACoAHgAVABQAFAAZAB4AFQAdABgAGAAUABUAHwArACkAKQAdAB8AKQBrAGkAawApACsAaQB9AH8AfQBpAGsAfwCJAIsAiQB/AH0AiwCNAJQAjQCLAIkAlACKAIwAigCUAI0AjAB+AIAAfgCMAIoAfgBtAGoAagCAAH4AbQAsACoAKgBqAG0ALAAgAB4AHgAqACwAIAAcABUAFQAeACAAHAAfAB0AHQAVABwAEQAjAA0ADQAEABEAIwBhAF8AXwANACMAYQCFAJ0AnQBfAGEAhQCXAKYApgCdAIUAlwCfAKkAqQCmAJcAnwCYAKcApwCpAJ8AmACGAJ4AngCnAJgAhgBiAGAAYACeAIYAYgAkAA4ADgBgAGIAJAASAAUABQAOACQAEgAKAAEAAQAFABIACgARAAQABAABAAoADwAhACMAIwARAA8AIwBjAGEAYwAjACEAYQCHAIUAhwBhAGMAhQCZAJcAmQCFAIcAlwCgAJ8AoACXAJkAnwCaAJgAmgCfAKAAmACIAIYAiACYAJoAiABmAGIAYgCGAIgAZgAiACQAJABiAGYAIgAQABIAEgAkACIAEAAJAAoACgASABAACQAPABEAEQAKAAkA","c":"BgIDAwMDBgUDBAUDAwMBAgUDBAQDAwMDBQMFAwMDAwMFAwUDAwMDAwUDBQMDAwMDBQMFAwMDAwMFAwUDAwMDAwUDBQMDAwMDBQMFAwMDAwMFAwUDAwMDAwUDBQMDAwMDBQMBAgMDBAQFAwUDBgYGBgYGBgYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=","pal":["333333","666633","999922","cccccc","ddbb22","dddddd","eeeeee"]},"scythe":{"s":4.486220892971588e-05,"p":"+/wlyaX/+/yLytz8+/yvzLAC+/zbziAB+/w50LwF+/wq02MF+/x+2rL7+/wc29n4+/zL2775Kv14TsfhKv2kTuDhKv05TwDiKv21Tw3lKv3PTyLiKv0CUC3iKv0wUDfiKv08UCrlKv3BUE/lKv3FUDvoKv3zUDLlKv0bUTLlKv0/UVToKv2zUXzoKv3kUTfoKv0GUi3oKv0EVUjjKv1KVaPXKv1OVS/lKv12VbzXKv2YVRfnKv2oVeLXKv3aVQjYKv3rVRXYKv37VSHYKv2YV1zZKv2NWdraKv3cWpLkKv3uWjnlKv0AW+HlKv1TWzXcKv1PX6DnKv29X6/mKv0qYL7lK/0021D8K/3C22j7K/1Q3ID6Lv3WzGX6Lv2O0Kv/Lv1G1PEEQ/3vztX3Q/1B0jb+Q/2T1ZcEUf2X0N33Uf1T05b9Uf0P1k4DUv0G13D4Uv3Q1yr8Uv2a2OP/Yf0yUtLcYf1fUuvcYf25UgvdYf0TUyvdYf0yUzbdYf1OU0DdYf06VkveYf3CWY3fYf34XLLgYv31Wcf0Yv2WWj/2Yv06W733Yv1BW/HxYv0CXIrwYv2aXG7vYv2fXP76Yv3YXtL8Yv3/XhPzYv0TYHz4Yv2iYIH2Yv35YrrxYv10Y6f7Yv3TZBj4Yv10ZXb2aP2u2KgAgv3UXt38gv1vY7L7gv3PZCL4gv1wZYH2sP2X14kDsP0l3f79sP1m3mf8sP0N30T8sP0b3z79sP0o3zj+sP2n39D6wf3213kDwf3U3iUBwf1a3+AAyf0l2HIDyf2q37YCyf3+3wACzP1vXSQAzP2aXhf+zP1KYV7/zP0eZLH8zP3DZW74zP3jZRgBzP3mZRkBzP2EZnn2zP0caH4FzP27aGz+zP09abcHzP3NadQIzP1Valz6zP1davEJzP0Ra90AzP0Sa4D4zP1Xa5gDzP1sa4n8zP2zaz0HzP3iaxQJzP35awAKzP0EbJ36zP0QbOwKzP2IbMP+zP37bMb8zP2mbUwDzP3jbYcLzP3nbvcFzP2hbzkLzP1QcKAIzP1fcesK4v1USZr/4v1CSrL14v2HStz+4v0bS5n24v0cSw/14v0pS3X/4v25Sx3+4v30S3/34v1DTM384v1cTLf+4v2RTA784v25TKz74v3wTHT24v2OTfj94v3JTVr34v0YTqf84v1mTun74v2OTob77v2jL2r77v14Tsfh7v2kTuDh7v21Tw3l7v3FUDvo7v0/UVTo7v2zUXzo7v3kUTfo7v0GUi3o7v1KVaPX7v12VbzX7v2YVRfn7v2oVeLX7v2sVYzX7v3aVQjY7v3dVbLX7v3rVRXY7v37VSHY7v0QVtjX7v0hVubX7v0wVvHX7v2YV1zZ7v3OVy3Z7v2NWdra7v3CWava7v0AW+Hl7v1TWzXc7v2IWwXc7v1PX6Dn7v29X6/m7v0qYL7l7v1fYI/l8P2OMGT78/1I2qQA9P0ZM1L79f3cVYjX9f3fVYrX9f0PVq7X9f0RVrDX9f0gVrvX9f0vVsfX9f1DVtbX9f1UVuPX9f1kVu/X9f3NVwLZ9f0BWCrZ9f3BWYDa9f31Waja9f2HW9rb9v2YL1r69v3dM0379v2sWwvc9v1oYF3l+P2DMFX6/P0OM0X6/f3TM0H6//2eQKf3//2mQA74//2uQHX4//3pQHT7//3tQK77//32QB/8//0FQef8//0RQa/9//0dQXf+//0VQ631//0aQ1f2//0gQwL3//1IQ/f7//1LQ1j8//1RQxP9//1cQ13+//1jQzb///1qQw8A//1URIb1//1YRDb2//1cROf2//13RAf8//15RGv8//19RCz9//2ERIL+//2JRGL///2OREIA//3jRf3///3xRXz1//38RSr2//0GRtf2//0uRij///1URuL7//1aRkT8//1mRgL9//16RlL+//2uR5////0USKn1//0oSE72//09SPP2//1mSNn+//3VSMD7//3gSB78//33SNP8//0eSRP+//16SUL///1WShD2//2WSpH+//0gS+b2//2yS+D9//3pS7z3//0yTKf8//16TPf7//2gTJv7//0ATub+//0zTtH0//0cTzX+//2lT4r2//04UIT9//1FUJ/7//1NUI36//1RUP/5//1vUGD3//19UGj0//3SUKz+//00UVj2//06USP+//2iUZr9//0kUtD6//00UtgC//2UUmv4//2bUk8C//3LUuj4//0DU8YB//0+U7z3//2XU2QE//0cVMsD//2hVDMD//3EVv4G//0yV2sG//2hV9gF//0+W18N//2tW8wM//0cXDgM//04XO4N//3dXnIN//0HYeIRAf6rLyX8Av4x2wMCAv6VMB/8Bv4fMw38Bv7nWswMBv5UWzwMBv7BW6sLCP7kMwj8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","i":"+gg/B/QI9Ag/B0IH9AhLCfoIbwn6CEsJQQf7CPYIQQf2CEQHTAn2CPsI+whwCUwJSQfoCOoI6AhJB0oHKglrCSwJbQksCWsJ4QNlBl8GXwa4A+EDZQZCBz8HPwdfBmUGtwNjBuADZgbgA2MGRAdmBkEHYwZBB2YGvwMoBikGKAa/A8ADaAbLBmcGygZnBssGrwh7CkAKQAqrCK8IqwhvCHEIcQivCKsIfgrACGsKrAhrCsAIcAisCHIIwAhyCKwI5Qh6CnMKcwrkCOUIqgh5CK0IeQiqCHgInwVxCG8IcQifBZwFnwU1BZwFBwWcBTUFcgieBXAIcgiaBZ4FbAiiBaEFogVsCGsINAWeBZoFmgUGBTQFdQUQBQgFEAV1BXYFYQMHBWoDNQVqAwcFFwNhAzQDagM0A2EDaQM0BQYFBgVeA2kDjgRgA18DYAOOBJEEMwNpA14DXgMZAzMDEwM3AxQDNwMTAzYDPAIXA1wCNANcAhcDSAIJAjwCPAJcAkgCWwIzAxkDGQM6AlsC7gIWAhUC7gIVAvECCAJHAjoCWwI6AkcC/gENAv8BDQL+AQwCSAI7AgkCYAI7AkgCOwJgAlMCbQJTAmACRwI5Al8COQJHAggCDgLxAQ8C8QEOAvABbAJfAlcCOQJXAl8CQQITAkICEwJBAhQCTAtvCSELSwkhC28JIQtsC0wLlgtMC2wLcAlOCyMLIwtMCXAJdglPC1ILTwt2CXUJTguYC28LbwsjC04LaAuaC5kLmgtoC1ULbQJ7A3QDdANTAm0CewPhA7gDuAN0A3sDVwJxA2wCegNsAnEDTgJnA2YDTgJmA00CcQO3A3oD4AN6A7cDgQNzA4IDcwOBA3IDyAuWC7YLbAu2C5YLxAvVC8gLyAu2C8QLmAvLC7gLuAtvC5gLnwvNC84LzQufC50LxQu4C8sL1gvFC8sL1AvXC9gL1wvUC9ILxAu3C8kLyQvVC8QLtwuxC7oLyQu3C7oLzAvFC9YLxQvMC7kL2wvTC9EL0wvbC9kLuQvMC70LvQuyC7kLzwvHC9ALxwvPC8YLGwu6C7ELsQuoChsLqApACnsKewobC6gKvQsdC7ILqgqyCx0LSgvDC74LvgtHC0oLawqqCn4KHQt+CqoKHAueCh8LngocC4wKeQjACK0IwAh5CHIIygZjBmcGYwbKBkEHcAn7CCwJcAksCW0J+whJB+oISQf7CEEHmgVsCKEFbAiaBXIIXgOOBF8DXgMGBY4EOgIZA/EC8QIVAjoCCAIOAjkCDgIIAvABTgtwCXYJTgt2CVILcQNXAmYDTQJmA1cCYwa3AykGKQa3A78DywuYC58LnwvOC8sL0QvWC9sL1gvRC8wLHQu9C0oLwwtKC70LwAh+CuUI5Qh+CnoKBgV1BQgFBgWaBXUFGQNeAzYDGQM2AxMDDAL+AQgCCAI6AgwCFAJBAjkCVwI5AkECaAuZC5gLmAtOC2gLcgOBA3EDtwNxA4ED1AvYC9YL1gvLC9QL0AvHC8wLvQvMC8cLHwueCh0LfgodC54Kawl1CW0JdgltCXUJywZKB8oGSQfKBkoHeAhsCHkIbAh4CGsIEAWRBAgFjgQIBZEEFAPxAhMD8QIUA+4C8AH/AfEB/wHwAf4BTQJCAk4CQgJNAkECmgudC5kLnwuZC50LggPAA4EDvwOBA8AD2QvbC9gL2AvXC9kLxgvDC8cLwwvGC74LjAp6Cp4KegqMCnMKGwt7CowKGwuMChwLyQu6C8YLxgvPC8kLyAvVC9IL1wvSC9ULdAO4A4IDggNzA3QDVQuWC5oLTAuWC1ULOwJTAkICQgITAjsCPAIJAg0C/wENAgkCNwMXAxQDYQMXAzcDdgUHBRAFBwV2BZwFrwh4CKoIeAivCHEIXwbLBmgGywZfBj8H+ghvCSoJKglvCWsJewrkCHMK5Ah7Cq8IRwu+C7oLugsbC0cL1QvJC9ML0wvZC9ULzQudC8gLlgvIC50LuANfBigGuAMoBsADUwJ0A2cDZwNOAlMCdQlMC08LbwlMC3UJCQI7Ag8CCQIPAvEBFwM8Au4C7gI8AhYCBwVhA5EEkQRhA2ADawicBaIFnAVrCHEISgf6COgI+ghKBz8H6ggqCSwJKgnqCOgIKQZoBmcGaAYpBigG5AitCOUIrQjkCKoIoQV2BXUFdgWhBaIFNgNgAzcDYAM2A18DDAIWAg0CFgIMAhUCFAIPAhMCDwIUAg4CUgtVC2gLVQtSC08LcgNnA3MDZwNyA2YDzgvSC9QL0gvOC80L0wvQC9EL0AvTC88LRwsfC0oLHwtHCxwLbQl2CXAJygZJB0EHeQhsCHIIjgQGBQgF8QIZAxMD8AEIAv4BTQJXAkECnwuYC5kLvwO3A4ED2wvWC9gLwwu9C8cLegp+Cp4KcwqMCnsKvgvGC7oL2QvXC9ULnQuaC5YLwAOCA7gDTgJCAlMCdQlrCW8J8QH/AQkC7gIUAxcDkQQQBQcFawh4CHEISgfLBj8HKgnoCPoILAn7COoIaAYoBl8GZwZjBikGqgjkCK8IrQjACOUIdgWiBZwFoQV1BZoFNwNgA2EDXwM2A14DDQIWAjwCFQIMAjoCEwIPAjsCDgIUAjkCVQtPC0wLUgtoC04LcwNnA3QDZgNyA3ED0gvNC8gLzgvUC8sLzwvTC8kL0QvQC8wLHAtHCxsLSgsfCx0LcAirCKwIqwhwCG8IQgdlBmYGQgdmBkQHSwn0CPYISwn2CEwJawqoCqoKqAprCkAKsgu3C7kLtwuyC7ELuAvFC8QLxAu2C7gL4QN7A3oD4QN6A+ADIwtvC2wLbAshCyMLbAJgAl8CYAJsAm0CRwJcAlsCXAJHAkgCMwNqA2kDagMzAzQDNAWfBZ4FnwU0BTUFrAhACmsKQAqsCKsIqgqxC7ILsQuqCqgKxQu3C8QLtwvFC7kLtgtvC7gLbwu2C2wL4ANlBuEDZQbgA2YGbAJ6A20CbQJ6A3sDIQtMCSMLTAkhC0sJSAJfAmACRwJfAkgCMwNbAjQDNANbAlwCaQM1BTQFNQVpA2oDngVvCHAIbwieBZ8F9AhEB/YIRAf0CEIHLgnrB/MH8wdxCS4J+gl5CcAJwAkjCvoJrAl2CIcIhwj3CawJvwlKCX4JfgkOCr8JUAeEBFIEhARQB04HTwLzAqYCOgOmAvMCqgLrA7MDswNiAqoCnAsJC1QLhQpUCwkLOwMhBPAD8AP0AjsDlwurClMLhApTC6sKowE/ArgBuAEkAaMBJAuiC5ELkQunCiQL3wdpBd0H3QdpBXAFnwqPCyYLJgs/Cp8KiAHGAJMBxgCIAZUApAI5A/AC8AJJAqQCtQl7CHcJagh3CXsIMAmtCXMJ/glzCa0JRQFGArQBnQK0AUYCigkICvQJbAr0CQgKaAKqA7AC5wOwAqoDBgsTCngKeAqVCwYLygCVAUYBtgFGAZUBUQKxAcABKwHAAbEBoAtJC6cLSQugCyALiwG1ACYBJgGkAYsBlAHMAMcAogHMAJQBxAmdChUKagsVCp0KTwftB1IH9wdSB+0HrAJDAmMCuwFjAkMCvwGnAlACUAJgAb8BcQUkBGsF7wNrBSQEhgTqA14EtANeBOoD+Al5CiEKTQshCnkKbQjeB+MH4weACG0ISgnvBzIIMgh+CUoJkgulCmsLhgprC6UKrQk/Cv4JJgv+CT8KVgfwB3oHOwh6B/AHhgRZB1MHWQeGBF4E+wkeCzQKNAqMCfsJjwurCpcLqwqPC58KdwUrBG8F8wNvBSsEOQMpBPID8gPwAjkD6wdOB1AHUAfzB+sHhAr6CSMKIwpTC4QKdgjmB+oH6geHCHYI6QduBeIH4gduBXMFIQRwBWkFaQXwAyEEXgFPArwBpgK8AU8CZwnECYMJFQqDCcQJ9AmECIoJdAiKCYQI3QdqCN8HewjfB2oIkgHLAJcBywCSAbcAIAugC2oLagudCiALYwK0A6wC6gOsArQDrgFLAr4BvgEnAa4BpwtJC5ULBguVC0kLQwKkAbsBJgG7AaQBrgLpA7ADsANnAq4CRgLvAp0COAOdAu8CiwHKALUAlQHKAIsBjAkuCXEJcQn7CYwJeQltCIAIgAjACXkJEwqsCfcJ9wl4ChMKtgGkAkkCSQJGAbYBawuGCh4LNAoeC4YKawXjB94HawXeB3EFkwHGAEUBRQG0AZMBCAqnCmwKkQtsCqcKpQqSC00LTQt5CqUK8wLvAzoDJAQ6A+8DlQCIASQBowEkAYgBogskC5wLCQucCyQLpwI7A/QC9AJQAqcCUgdVBIUEUgeFBE8HPwKqAmICYgK4AT8CDgpUC4UKhQq/CQ4Kdwn4CbUJIQq1CfgJ7QcwCfcHcwn3BzAJ6wOHBF0EXQSzA+sD4gd0COkHhAjpB3QIzACiAWABvwFgAaIB6QOFBFUEVQSwA+kD8AdnCTsIgwk7CGcJKQRzBW4FbgXyAykElAHHACsBKwGxAZQBSwKuAmcCZwK+AUsCegddBIcEegeHBFYH7wdTB1kHWQcyCO8HsAJRAmgCwAFoAlECtwCSAScBrgEnAZIB7wLzAzgDKwQ4A/MDhATnA1IEqgNSBOcDlwHLAF4BXgG8AZcBbwXqB+YHbwXmB3cF8wdSB/cHUgfzB1AH8ANpBe8DawXvA2kF3wd7COMHgAjjB3sI+wlzCf4Jcwn7CXEJawsmC48LJgtrCx4LUwuSC5cLTQuSC1ML8wJQAvQCUALzAk8CwAkhCiMKtQkhCsAJXgHMAGABzABeAcsAJwHHALcAxwAnASsBZwLAAb4BwAFnAmgCsANSBKoDUgSwA1UE9wdzCXEJcQnzB/cHVQRSB1AHUAdSBFUE9ALwA+8D9ALvA/MCawXfB+MH3wdrBWkFgAi1CcAJtQmACHsIqgNoAmcCqgNnArADvgErAScBKwG+AcABywDHAMwAxwDLALcAYAFQAk8CYAFPAl4BIwpNC1MLTQsjCiEKlwtrC48LawuXC5IL/gkmCx4LHgv7Cf4JDgqDCRUKgwkOCn4JnAtqC6ALagucC1QLlQuiC6cLkQuiC5UL7wJJAvACSQLvAkYC9wlsCngK9AlsCvcJRQHKAEYBygBFAcYAJAG1AJUAtQAkASYBYgK7AbgBuwFiAmMCswNeBLQDXgSzA10EOwiDCX4JfgkyCDsIXQR6B1kHWQdeBF0E7wLyA/MD8gPvAvACbwXpB+oH6QdvBW4Fhwj0CfcJ9AmHCIQItANjAmICtANiArMDuAEmASQBJgG4AbsBxgC1AMoAtQDGAJUARgFJAkYCRgFGAkUBeAqRC5ULkQt4CmwKpwucC6ALnAunC6ILFQpqC1QLVAsOChUKMgh6BzsIegcyCFkH8gNuBfMDbwXzA24F6QeECOoHhwjqB4QITwfrB+0H6wdPB04HcAUhBCQEcAUkBHEFagjdB94HagjeB20IMAmMCa0JjAkwCS4JPwqGCp8Khgo/CjQKhAqrCqUKpQp5CoQKOgOnAqYCpwI6AzsDeQn6CfgJ+Al3CXkJvAGiAZcBogG8Ab8BrgGSAZQBlAGxAa4BUQKwAq4CUQKuAksC6QOEBIUEhATpA+cD7QcuCTAJLgntB+sHhQROB08HTgeFBIQEOgMhBDsDIQQ6AyQE3QdxBd4HcQXdB3AFdwltCHkJbQh3CWoIrgLnA+kD5wOuArACSwKxAVECsQFLAq4BlAGSAZcBlAGXAaIBpgK/AbwBvwGmAqcCeQr6CYQK+gl5CvgJqwqGCqUKhgqrCp8KrQk0Cj8KjAk0Cq0JZwm/CcQJvwlnCUoJnQoJCyALCQudCoUKBgtJCyQLJAunCgYLOAOkAp0CpAI4AzkDrAkTCggKCAqKCawJtAGVAZMBlQG0AbYBowGIAYsBiwGkAaMBQwKsAqoCQwKqAj8C6wOGBIcEhgTrA+oD8AdKCWcJSgnwB+8HhwRTB1YHUweHBIYEOAMpBDkDKQQ4AysE4gd3BeYHdwXiB3MFigl2CKwJdgiKCXQIqgLqA+sD6gOqAqwCPwKkAUMCpAE/AqMBiwGIAZMBiwGTAZUBnQK2AbQBtgGdAqQCpwoTCgYLEwqnCggKIAskC0kLCQskCyALxAmFCp0KvwmFCsQJVgfvB/AH7wdWB1MHcwUpBCsEcwUrBHcFdAjmB3YIdAjiB+YH3gTgBOEE4QTfBN4E/wapBv4GqgapBv8GqgaYBqkGfAaYBqoGfAZXBpgGWQZXBnwGWQYbBlcGHQYbBlkGGwaPBY0FjwUbBh0GjQVmBWUFZgWNBY8FZQUsBSsFLAVlBWYFKwXhBOAE4QQrBSwF2gTYBNkE2QTbBNoE3QTeBN8E3wTcBN0E2wTdBNwE3ATaBNsE1gTUBNcE1ATWBNUE1wTZBNgE2ATWBNcE1QTSBNQE0gTVBNME/Ab+BjEHLAcxB/4GJwcdB/kG9wb5Bh0H+wb8Bi4HMQcuB/wGLgcnB/sG+Qb7BicHDgcIB/UG8gb1BggHHQcOB/cG9Qb3Bg4HAwfyBggH8AbyBgMH8QbhBvMG3wbhBvEG9gb0BucG5AbnBvQG9AbzBuQG4QbkBvMG+gb4BusG6QbrBvgG6wbsBvoG/Qb6BuwG+Ab2BukG5wbpBvYG7AbuBv0G/wb9Bu4G5QTiBOQE4gTlBOME6ATnBOkE5wToBOYE5gTkBOcE5ATmBOUE6wTtBOwE7ATqBOsE7QTuBO8E7wTsBO0E6QTrBOoE6gToBOkE8QQtBS4FLQXxBPAEZwUuBS0FLgVnBWgFkAVoBWcFaAWQBZEFHgaRBZAFkQUeBh8GWwYeBloGHwYeBlsGgAZaBn8GWwZaBoAGgAZ/BqcG7wamBu4GpwZ/BqYGpwamBu8G7gTwBPEE8QTvBO4E3wThBPAE8ATuBN8E7gaqBv8GpgaqBu4GpgZ8BqoGfwZ8BqYGfwZZBnwGWgZZBn8GWgYdBlkGHgYdBloGHQaQBY8FkAUdBh4GjwVnBWYFZwWPBZAFZgUtBSwFLQVmBWcFLAXwBOEE8AQsBS0F2ATaBOsE6wTpBNgE3ATfBO4E7gTtBNwE2gTcBO0E7QTrBNoE5wTVBNYE1QTnBOQE6QTWBNgE1gTpBOcE5ATTBNUE0wTkBOIE8AbzBvIG8QbzBvAG9wb1BvYG9Ab2BvUG9QbyBvQG8wb0BvIG+wb5BvoG+Ab6BvkG+gb9BvsG/Ab7Bv0G+Qb3BvgG9gb4BvcG/Qb/BvwG/gb8Bv8G7QbdBu8G3AbdBu0G6AbmBtoG2QbaBuYG6gbcBu0G2wbcBuoG6gboBtsG2gbbBugG4wbiBtgG1wbYBuIG5gbjBtkG2AbZBuMG4AbXBuIG1gbXBuAG/wT1BPYE9QT/BP4EAQX3BPgE9wQBBQAFAAX2BPcE9gQABf8E+QT6BAMFAwUCBfkE+gT7BAQFBAUDBfoE+AT5BAIFAgUBBfgEMgUFBfwEBQUyBTYFNgVqBW0FagU2BTIFlgVtBWoFbQWWBZcFIAaXBZYFlwUgBiEGRQYgBkQGIQYgBkUGagZEBmwGRQZEBmoGpAZsBqUGagZsBqQG1QalBt0GpAalBtUGBQX7BPwE+wQFBQQF7wTxBPwE/AT7BO8E7QbvBuwG7gbsBu8G3QanBu8GpQanBt0GpQaABqcGbAaABqUGbAZbBoAGRAZbBmwGRAYfBlsGIAYfBkQGHwaWBZEFlgUfBiAGkQVqBWgFagWRBZYFaAUyBS4FMgVoBWoFLgX8BPEE/AQuBTIF6QbnBugG5gboBucG6ATqBPkE+QT4BOgE6gbtBusG7AbrBu0G7ATvBPsE+wT6BOwE6gTsBPoE+gT5BOoE6wbpBuoG6AbqBukG9wTlBOYE5QT3BPYE5AbhBuMG4gbjBuEG+ATmBOgE5gT4BPcE5wbkBuYG4wbmBuQG3wbiBuEG4AbiBt8G9gTjBOUE4wT2BPUERQfIBkYHxwbIBkUHxwagBsgGnwagBscGnwZiBqAGYQZiBp8GYQYMBmIG/wUMBmEG/wV6BQwGeQV6Bf8FeQU7BXoFOgU7BXkFOgUPBTsFDAUPBToFDAWNBA8FjASNBAwFBgb7BUsGBgZ9BfsFgQV9BQYGgQU9BX0FBgcKBwUHQQUWBT0FQQU9BYEFGgUWBUEFlgT+BJkE/wSZBP4EzgYEBwcHBwfPBs4GGgWVBBYFmgSVBBoFmwSWBJkElgSbBJcECAesBgMHsAasBggH1ASVBJoElQTUBNIEsAaGBqwGigaGBrAG1gbPBtcGzgbPBtYGigZHBoYGSwb7BUcGSwZHBooGBgexBgoHAgevBgYHSAZHBgAGrwaLBrEGqwaJBq8GqwavBgIHAAZHBvsF+wV+BQAGiQZMBosGiQaLBq8GhQZKBokGhQaJBqsGmASTBJcEfgV9BT4FfgX7BX0FSgaFBkYGSgYIBkwGRgYCBkoGAQcGBwUHAgcGBwEHPgV9BT0FFwU9BRYFPQUXBT4F/QT+BJYElgSSBP0EAgaCBQgGAgYIBkoG+gWABQIG+gUCBkYGzgbNBgQHAAcEB80GgAVCBYIFfAWABfoFfAVABYAFkwSYBJQElwSSBJYElwScBJgEkgSXBJMEBAcJBwcHAAcFBwQHBQcJBwQHBQcKBwkHAQcFBwAHQAVCBYAFQAUbBUIFPAVABXwFPAUZBUAFhwasBoYGrQasBocGGQWcBBsFGQUbBUAFGQU8BRUFFQWYBBkFmAScBBkFlASYBBUFSAaGBkcGhwaGBkgGkQZOBo0GUAZOBpEGHAWqBB4FnwSqBBwF2QbYBtEG0AbRBtgGtQaNBrMGkQaNBrUGQwUeBUoFHAUeBUMFCwcXBxYHDAcXBwsHoASvBNkE2QTXBKAEHQezBg4HtQazBh0HqQSdBKgEnQSpBJ4EgwVKBYUFQwVKBYMFHwWgBB0FrwSgBB8F0QbQBhYHCwcWB9AGCgaFBRMGgwWFBQoGAAUBBagEqASdBAAFTAUdBUQFHwUdBUwFDAcYBxcHDQcYBwwHTQYTBk8GCgYTBk0GhgVEBYQFTAVEBYYFqgSeBKkEngSqBJ8EjAZPBpAGTQZPBowGFAaEBQwGhgWEBRQGsgaQBrQGjAaQBrIGUAYMBk4GFAYMBlAGGAeyBrQGsgYYBw0HjQZLBooGTgZLBo0GGwWfBBwFnwQbBZwE2AbXBtAGzwbQBtcGswaKBrAGjQaKBrMGGwUcBUIFBwcJBwwHBwcMBwsH1wSaBKAEmgTXBNQEDgewBggHswawBg4HngSZBJ0EmQSeBJsEQgUcBUMFQgVDBYIFHQWaBBoFoASaBB0FzwYHBwsHCwfQBs8GggWDBQgGggUCBoAFggVDBYMFmQT/BJ0EAAWdBP8ERAUaBUEFHQUaBUQFCQcNBwwHCgcNBwkHCAaDBQoGCgZMBggGhAVBBYEFRAVBBYQFnASXBJsEnwSbBJ4EmwSfBJwETAYKBk0GTAaJBkoGTAZNBosGDAaBBQYGhAWBBQwGiwZNBowGsQYGB68GsQaMBrIGjAaxBosGTgYGBksGDAYGBk4GDQexBrIGsQYNBwoHlgZSBpMGIAW9BCIFtwS9BCAF2wbaBtMG0gbTBtoGugaTBrcGlgaTBroGUQUiBVQFIgVRBSAFIwcqBykHJAcqByMHuQTABN0E3QTbBLkELge3BicHuga3Bi4HtQS8BLYEhwVUBYkFVAWHBVEFJAW5BCEFwAS5BCQF0wbSBikHIwcpB9IGFQaJBRcGhwWJBRUGAgUDBbsEuwS1BAIFVwUhBVIFJAUhBVcFUQYXBlMGFQYXBlEGVwVSBYsFtgS9BLcEkgZTBpQGUQZTBpIGiwWIBRkGiwVSBYgFlAa2BpIGVQZSBpYGVQYWBlIGGQaIBRYGGQYWBlUGtgaUBrgGJQe4BisHuAYlB7YGKwe8BjUHuAa8BisHuAaZBrwGlAaZBrgGGgYZBlYGGgaLBRkGlAZYBpkGUwZYBpQGvQS2BLwExwS8BMYEjAWLBRoGjAVXBYsFWQVXBYwFUwYcBlgGFwYcBlMGNAcqBzUHKwc1ByoHWQUkBVcFJQUkBVkFAwUEBcUExQS7BAMFFwaOBRwGiQWOBRcG1AbTBjMHKQczB9MGJQXABCQFwwTABCUFiQVbBY4FVAVbBYkFvAS1BLsEvATHBL0EuwTGBLwEMQe6Bi4Huwa6BjEHwATDBN4E3gTdBMAEMwcpBzQHKgc0BykHVAUmBVsFIgUmBVQFuwaWBroGlwaWBrsG2wbUBtwG0wbUBtsGIgXHBCYFxwQiBb0ElwZVBpYGVgYZBlUGVgZVBpcGGAe2BiUHtgYYB7QGFgYUBlIGtAaSBrYGkAaSBrQGFgaGBRQGiAWGBRYGkAZRBpIGTwZRBpAGtwSpBLYEqQS3BKoEiAVMBYYFUgVMBYgFTwYVBlEGEwYVBk8GJQcrByQHGAclBxcHUgUfBUwFIQUfBVIFAQUCBbUEtQSoBAEFEwaHBRUGhwUTBoUF0gbRBiMHFgcjB9EGIQWvBB8FuQSvBCEFhQVRBYcFSgVRBYUFtgSoBLUEqAS2BKkEJwe1Bh0H2wTZBK8ErwS5BNsEFgckByMHJAcrByoHFwclByQHFwckBxYHSgUgBVEFIAVKBR4Ftwa1BicHtwaRBrUGkwaRBrcG2gbZBtIG0QbSBtkGHgW3BCAFqgS3BB4FkwZQBpEGUgYUBlAGUgZQBpMGKQXOBCcF0ATOBCkFYwUnBWEFKQUnBWMFlAVhBZIFYQWUBWMFlAWSBSQGJAaSBSIGIgZeBiQGXgYiBlwGXgZcBpwGnAZcBpoGnAaaBr8GPge9BjwHvwaaBr0Gvwa9Bj4HKAXQBCkFKAXPBNAEYgUpBWMFKQViBSgFkwVjBZQFkwViBWMFIwaUBSQGlAUjBpMFXQYkBl4GXQYjBiQGmwZeBpwGXgabBl0GvgacBr8GnAa+BpsGPQe/Bj4HPQe+Br8GIwXgBL4E4AQjBSsFVQUrBSMFKwVVBWUFigVlBVUFZQWKBY0FGAaNBYoFjQUYBhsGVwYYBlQGGwYYBlcGmAZUBpUGVwZUBpgGqQaVBrkGmAaVBqkG/ga5BiwHqQa5Bv4GzgQFBScFYQU2BW0FJwUFBTYFJwU2BWEFkgVtBZcFYQVtBZIFIgaXBSEGkgWXBSIGRQYiBiEGIgZFBlwGagZcBkUGXAZqBpoGpAaaBmoGmgakBr0G1Qa9BqQGvQbVBjwHlQZWBpcGVAZWBpUGxwTPBCYF3AbVBt0G1AbVBtwGuQaXBrsGlQaXBrkGWwUoBWIFJgXPBCgFJgUoBVsFPAczBz4HwwS+BOAE4ATeBMMELAe7BjEHuQa7BiwHxgS7BMUE0ATFBM4ExQTQBMYEjgViBZMFYgWOBVsFIwXDBCUFvgTDBCMF1QbUBjwHMwc8B9QGHAaTBSMGjgWTBRwGzgQEBQUFBAXOBMUEVQUlBVkFIwUlBVUFPgc0Bz0HNAc+BzMHNQc9BzQHWAYjBl0GHAYjBlgGigVZBYwFVQVZBYoFzwTGBNAExgTPBMcEmQZdBpsGWAZdBpkGGAaMBRoGigWMBRgGvAabBr4Gmwa8BpkGVAYaBlYGGAYaBlQGNQe+Bj0HvAa+BjUHrgadBsMGiAadBq4GiAZgBp0GSQZgBogGSQb7BWAGSQYBBvsF+wV/BXgFfwX7BQEGeAU/BTkFPwV4BX8FOQUYBRIFGAU5BT8FPgUYBT8FPgUXBRgFfgU/BX8FfgU+BT8FAAZ/BQEGAAZ+BX8FSAYABgEGSAYBBkkGhwZJBogGSQaHBkgGrQaIBq4GrQaHBogGQwfHBkUHQwfDBscGwwafBscGwwadBp8GnQZhBp8GnQZgBmEGYAb/BWEGYAb7Bf8F/wV4BXkFeAX/BfsFeQU5BToFOQV5BXgFOgUSBQwFEgU6BTkFDAWQBIwEkAQMBRIFLQc4BzYHMAc4By0HswVeBbQFXQVeBbMFLQd9BnkGMAc5BzgHOQcwBzcHfga0BYEGswW0BX4GXQXLBF4FyQTLBF0FNweBBjkHfgaBBjcHVgXIBL8EywTCBMoEwgTLBMkEsQTEBMEExASxBLgErAVLBakFTwVLBawFLwd7BjIHegZ7Bi8HWAXEBFoFwQTEBFgFegaxBXsGHwcyByYHMgcfBy8HSwWzBK0EswRLBU8FGwd2BnMGdgYbByEHsAWxBXoGsAVaBbEFWAVaBbAFGwcmByEHJgcbBx8HrQS4BLEEHAcSBxoHUwWtBVAFUwWuBa0FGgciBxwHKAccByIHrgV4BncGrgV3Bq0FugRTBVAFugRQBbQEeAYiB3cGeAYoByIHrgS6BLQErgSkBKYEugSuBLEEsQTBBLoEKAd6Bi8HegYoB3gGUwXBBFgFwQRTBboEeAawBXoGsAV4Bq4FKAcfBxwHHwcoBy8HSQWkBEcFSQWmBKQESwWmBEkFSwVJBakFrQSmBEsFFAdyBnAGFAdwBhIHGwdyBhQHrgVYBbAFWAWuBVMFFAcfBxsHHwcUBxwHpgSxBK4EsQSmBK0EcgamBXAGcgaoBaYFcwaoBXIGcwZyBhsHqAVJBUcFqAVHBaYFqQVJBagFqQWoBXMGdgapBXMGeQasBXYGrAWpBXYGrwWsBXkGvwTKBMIEuAStBLMEwgSzBL8EswTCBLgEIQcwBy0HMAchByYHsQVdBbMFWgVdBbEFIQd5BnYGeQYhBy0HTwW/BLMEvwRPBVYFJgc3BzAHNwcmBzIHewazBX4GsQWzBXsGWgXJBF0FxATJBFoFMgd+BjcHewZ+BjIHrwVPBawFVgVPBa8FyQS4BMIEuATJBMQEpASuBKwEEgccBxQHUAWrBU4FUAWtBasFcAalBW8GcAamBaUFEgdvBhEHEgdwBm8GIAcaBxkHGgcgByIHGgcRBxkHrQV1BqsFrQV3BnUGtARQBU4FtAROBbIEdwYiByAHdwYgB3UGRwWjBEYFRwWkBKMEpgVHBUYFpgVGBaUFsgSrBKwEtASsBK4ErASjBKQErAS0BLIEsASlBKcEoQSnBKUEEwd0Bh4HcQZ0BhMHSAWwBE0FpQSwBEgFcQaqBXQGpwWqBXEGEwcVBw8HpwVNBaoFSAVNBacFDwcQBwIHpwShBKIElASiBKEEowSsBKsEpwSiBKMEqwSnBKMEEAcPBxUHEQcVBxkHEQcaBxIHFQcRBxAHFQcTBx4HqgVNBU4FqgVOBasFEAdtBgIHbgajBW0GbgZtBhAHbwalBaQFbwakBW4GEQdvBm4GEQduBhAHogSUBEUFFQceBxkHIAcZBx4HdAaqBasFdAarBXUGTQWyBE4FsgRNBbAEHgd0BnUGHgd1BiAHpAU8BaMFpAWjBW4GRQWUBDwFRQU8BaQFRgWiBEUFRgWjBKIEpQVFBaQFpQVGBUUFsASnBKsEqwSyBLAEzATKBM0EygTMBMsEOQeCBjoHgQaCBjkHXgXMBF8FywTMBF4FYgXNBM8EzQRiBWAFtwVgBWIFYAW3BbYFhAa2BbcFtgWEBoMGPQeDBoQGgwY9BzsHOgeDBjsHggaDBjoHgga2BYMGtQW2BYIGtQVgBbYFXwVgBbUFXwXNBGAFzATNBF8FgQa1BYIGtAW1BYEGOwc4BzoHOQc6BzgHXAVWBbIFYgVcBbcFYgXIBFwFyARWBVwFzwTIBGIFPQd9BjYHtAVfBbUFXgVfBbQFPQc2BzsHOAc7BzYHygS/BMgEzQTIBM8EyATNBMoEfQavBXkGfQYtBzYHhAayBX0GhAZ9Bj0HsgVWBa8FsgWvBX0GtwWyBYQGtwVcBbIFCgkICQsJCwkNCQoJ4QH1AeAB9QHhAfYBsgK8Ar0CzwLlAuYC5QLPAs4CFwkaCRkJGgkXCRgJxwn9CMYJ/QjHCf4IxAHPAc4BNgkXCTUJFwk2CRgJuwnxCPIIuwnyCLwJ4wnhCeAJ4AniCeMJBgrgCeEJ4AkGCgUK1gnUCdMJ2AnaCdsJ2wGpAagBqQHbAdwBqwGoAakBqAGrAaoBDQkLCQ8JEgkUCREJEgkVCRQJDwkRCQ0JEQkPCRIJyQKgAsoCyQKfAqACogKgAp8CnwKhAqICyQLHAsYCvQKzArICvQK/AsACvwK9ArwCwALCAsQCEQndCdoJ3QkRCRQJ2AHJAsYCyQLYAdsB/QjQCcYJ0An9CAcJxAG8ArICvALEAc4BBwnTCdAJ0wkHCQoJzgG/ArwCvwLOAdEBCgnWCdMJ1gkKCQ0J0QHCAr8CwgLRAdQBDQnaCdYJ2gkNCREJ1AHGAsICxgLUAdgBqAGfAtsByQLbAZ8C3QkUCTMJMwkDCt0J4AHMAt4B4AHOAswCGQniCeAJGQngCRcJqgGhAp8CqgGfAqgBNQkDCjMJNQkFCgMKoQKqAd4B3gHMAqECNQngCQUK4Ak1CRcJzgLgAfUB9QHlAs4CGQm7CeIJuwkZCfEI7AtJCu4LSQrsC0YK8AtRCu0LRwrtC1EK7QuPCgIMjwrtC0cKAgyPCgEMjQoBDI8KjQrsCwEM7AuNCkYK7QvsC+4L7gvwC+0L8AvuC+8L7wvxC/AL7AsCDAEMAgzsC+0L8wtPCvULYgr1C08K9QtiCvYLZQr2C2IK9gtlCvQLVgr0C2UK9AvzC/UL9gv0C/ULCQMtAiwCCQMsAggD3wO7A94D3wO8A7sDdwR7BHoEewR3BHgERATSBUME0gVEBNMFfwEsAn4BfwEtAiwC9gIWAxUDFgP2AvcC2APVA9QD1APXA9gDLAjeCCsILAjfCN4ICwMPAw4DDwMLAwwD6QnrCeoJ6gnoCekJmwdUCJoHVAibB1UIEgZzBxEGEgZ0B3MHNAIwAi8CLwIzAjQCIQkgCR8J2wN9BNoDfQTbA34EpwmrCaoJqwmnCagJWghYCFkIHAnlCeQJHAnkCRsJNwIRAzYCEQM3AhID3wjbCN4I3wjcCNsIoAefB54HnwegB6EHkQnJCMgIkQnICJAJKAgsCCsILAgoCCkI3gXZBdoFFghdBxUIFgheB10HdAdwB3MHdAdxB3AHSQRIBEcESARJBEoE5QVnBOQFZwTlBWgEDgYSBhEGEgYOBg8GngOgA58DnwOdA54DfAmECXsJfAmFCYQJ5AnnCeYJxQP7AsQDxQP8AvsCfgR6BH0EfgR7BHoE1gLVAtQCwgipCKgIqQjCCMMIHgkcCRsJGwkdCR4JGwJpARoCaQEbAmoB6wl8CXsJewnqCesJ1wPbA9oD2wPXA9gD8gf0B/EH8gf1B/QHVAhXCFYIVwhUCFUIagF/AX4BfgFpAWoBqAgiCSEJIgmoCKkIEgMOAxEDEgMPAw4D5wnpCegJ5wnkCeUJ6AnmCecJVwdVB1QHVQdXB1gHnQebB5oHmgecB50HhQEzAoQBMwKFATQCWwjyB/EH8QdaCFsIMwI3AjYCNwIzAjQCIAkhCSIJHwkeCR0JHgkfCSAJJgbhBSUGJgbjBeEF0gXXBdYFagpnCmYKagpmCmkKVAehB6AHoQdUB1UHGwIsAi0CLAIbAhoCWQhbCFoIVwhZCFgIWAhWCFcIfwSCBIEEggR/BIAEQwRFBEYEgQGFAYQBhQGBAYIB3wUmBiUGJQbeBd8F/AIJAwgDCAP7AvwCngedB5wHnQeeB58H5APfA94D5APeA+MDmQOcA5sDMAKBAS8CgQEwAoIBgQRKBEkESgSBBIIExQPRA9ID0QPFA8QD2gXfBd4F1wXaBdkF1wXSBdMF2QXWBdcFFQMeAx0DHgMVAxYD0ALSAtMCZApgCmMKZAphCmAKoAPkA+MD4wOfA6ADaAR1BHQEdARnBGgERwRGBEUERgREBEMERgRHBEgEYQqhCWAKYQqiCaEJHQPXAtYC1wIdAx4D5QUDBgQGAwblBeQFnAOZA5oDnAOeA50DnQObA5wDqwlpCqoJqwlqCmkKXgdrB2oHagddB14H1QLWAtcC1ALTAtIC0wLRAtAC0wLUAtUCFggiCCMIIggWCBUIyQjWCNUI1QjICMkIkQmhCaIJoQmRCZAJDwMzAg4DDwM0AjMCDwPXAw4D1wMPA9gDewTYA9cDewTXA3oEewQOBnoEewQPBg4GcQcOBnAHcQcPBg4GcQcoCHAHcQcpCCgI2AgiCCUIIgjYCNUIbQcDBgcGAwZtB2oHdwTRA9QD0QN3BHQECAMvAgsDCAMsAi8CpAnYCNsIpAnbCKcJJQhtB3AHJQhwBygIBwZ6BA4GBwZ3BHoE1AMOA9cD1AMLAw4DLwKEATMCLwKBAYQB3ggoCCsIKAjeCNsIcwcOBhEGDgZzB3AHegTaA30EegTXA9oDDgM2AhEDDgMzAjYCxAMIA9EDxAP7AggD5AV0BAMG5AVnBHQEFQhqByIIFQhdB2oHkAnICNUIkAnVCKEJGgJ+ASwCfgEaAmkBZgqqCWkKZgqnCaoJYAqkCWMKYAqhCaQJewnCCKgIwgh7CYQJ8QdXB1QHVwfxB/QH4QWBBCUG4QV/BIEEHQPjA94D3gMVAx0DFQPeA7sDuwP2AhUD5wV/BOEF5wViBH8E9Ad2B1cHdgf0By4IhAnhCMII4QiECa8J5gkdCRsJ5gkbCeQJVgicB5oHVgiaB1QI1gVFBEME1gVDBNIFmwPSAtACmwPQApkD6gkfCegJ6gkhCR8JWgieB1gIWgigB54H3gVHBNkF3gVJBEcEnwPUAp0DnwPWAtQC6AkdCeYJ6AkfCR0JWAieB5wHWAicB1YI2QVFBNYF2QVHBEUEnQPSApsDnQPUAtICIQnqCXsJewmoCCEJoAdaCPEH8QdUB6AH3gWBBEkEgQTeBSUGnwMdA9YCHQOfA+MD5QX+BeIF5QUEBv4FcwR4BHYEeARzBHUEIwhpByEIIwhrB2kHewFqAWgBagF7AX8BaQdeB1wHXgdpB2sH1QPQA9MD1QPSA9ADBAZpB/4FBAZrB2kHFggjCCEIFgghCBQIBwMMAwoDDAMHAwkDBAZzBP4FBAZ1BHME1AjJCMcIyQjUCNYIMAItAikCMAIpAi4C0gN1BHME0gNzBNADNAKFAYMBNAKDATICkQmeCY8JkQmiCZ4JowmoCaYJqAmjCaUJ0gMJAwcD0gMHA9ADZQpqCmgKagplCmcK3AjZCNcI3AjXCNoILQIHAykCLQIJAwcDhQGCAYABhQGAAYMBJAgpCCcIKQgkCCYILQJ7ASkCLQJ/AXsBNAINAzICNAIPAw0DggEuAoABggEwAi4CcQdsB28HcQduB2wH2AMNA9YD2AMPAw0DyQiPCccIyQiRCY8JBQYPBg0GDwYFBgkG3wgqCN0I3wgsCCoI2AN5BNYD2AN7BHkEewR2BHkEewR4BHYEDwZ5BA0GDwZ7BHkEXgcWCBQIXgcUCFwH0wPYA9YD2APTA9UDdAcSBhAGdAcQBnIHDwZxB28HDwZvBw0GXQpkCmIKZApdCmEKDwMMAwoDDwMKAw0DKQhvBycIKQhxB28HogldCp4JoglhCl0KaATiBWYEaATlBeIFLgI0AjICNAIuAjACfgTZA3wEfgTbA9kDKQjcCNoIKQjaCCcIagqrCakJagqpCWgKqwmmCakJqwmoCaYJqAnaCKYJqAncCNoI/ALFA8MD/ALDA/oC2gjfCN0I3wjaCNwIEgM3AjUCEgM1AhADqAlnCmUKqAllCqYJLAgpCCcILAgnCCoIDAMwAi4CDAMuAgoDagEZAmgBagEbAhkCbwd0B3IHdAdvB3EHDAPVA9MDDAPTAwoDEgYPBg0GEgYNBhAGeATVA9MDeATTA3YEeQR+BHwEfgR5BHsEDwYJBgcGBwYOBg8GYgSABH8EgARiBGMECQPSA9EDCQPRAwgD0QKZA9ACmQPRApoDbQdxB3AHcQdtB24H4wXoBecF4wXnBeEFdQTSA9EDdQTRA3QEKQgmCCUIJQgoCCkIdgdYB1cHWAd2B3cHdQQDBnQEdQQEBgMG2AjcCNsI3AjYCNkI9QcuCPQH9QcvCC4IawcDBmoHAwZrBwQGqAmlCaQJpAmnCagJ4QjDCMIIwwjhCOIIawciCGoHawcjCCIILAIwAi8CMAIsAi0ChQmvCYQJhQmwCa8J1ggiCNUI1ggjCCIIDAMIAwsDDAMJAwgDcAS+A70DvgNwBHEEdwokCjgKdwo4CoMKkAFbAUQBWwGQAZYBmQGMAYcBjAGZAaUBOQoYCh8KOQofCkIKQgofCiQKQgokCncKlgGHAVsBhwGWAZkBjwGdAY4BnAGOAZ0BNQocCjYKHAo1ChsK1giiCaEJ1gihCdUIqQj1B/IH9QepCMMI0QPVA9QD1QPRA9IDVQfjBSYG4wVVB1gHpQljCqQJYwqlCWQK5AOCBIAEgATfA+QDeAR1BHQEeAR0BHcEVQIeAxYDFgNEAlUCmwFVAkQCRAKNAZsBsgGwAZgBsAGyAb0BHQoeCjcKHgodChYKGwqFCXwJhQkbCjUKFgoUCh4KFAoWCgEKNgodCjcKHQo2ChwKmAGdAY8BnQGYAbABjgGcAY0BmwGNAZwBWQGOAVgBjQFYAY4BjAGPAVoBjwGMAZgBdQo3CnYKNwp1CjYKHgoYCjkKGAoeChQKNQqwCYUJsAk1CnQKNwo5CnYKOQo3Ch4KpQGYAYwBmAGlAbIBjQFEAhACEAJYAY0BRAIWA/cC9wIQAkQCpQnYCKQJ2AilCdkI3wOABGMEYwS8A98DAwYJBgcGCQYDBgQGWAfoBeMF6AVYB3cHJgjYCCUI2AgmCNkIwwgvCPUHLwjDCOIIbgdqB20HbgdrB2oHdAo2CnUKNgp0CjUKWgGPAVkBjgFZAY8B2ArdCtsK2wrUCtgKswrNCbIKswrOCc0Jswq7CrwK3QDnAOYA5wDdAN4A1QDeAN0A3QDUANUA8QD6APUA6ADmAOcA7ADxAPUA8QDsAOgAzwrYCtQK1ArLCs8K5AruCuwK7AriCuQK2QpYC1YLWAvZCtsKiQD8AIUA/ACJAP4ABwEPARABBQEPAQcBkACJAIwAhQCMAIkAWwtWC1gLBwGQAAUBjAAFAZAA4gpfC+QKXwviClsLFgEPARQB7grzCuwK8QrsCvMKGgEcAR0BGQEcARoB+gr4CvsK9wrzCvgKFgEZARoBFAEZARYBGwr7ChwK+wobCvoKnQEdAZwBHAGcAR0BzAHTAMsBzAHUANMAigO5AokDigO6ArkCiwe/Bb4Fiwe+BYoHBQlFCEQIBQlECAQJ1AnWCdgJ0wnRCdAJTggVCRIJFQlOCFAIlAdQCE4IUAiUB5YHyAWWB5QHlgfIBcwFPQTMBcgFzAU9BD8EkwM/BD0EPwSTA5UDxwKVA5MDlQPHAsoCBgoECgMKAwoFCgYK8gDcAdkB3AHyAPsAxQLaAdcB2gHFAsgCkgPIAsUCyAKSA5QDPASUA5IDlAM8BD4ExwU+BDwEPgTHBcoFkwfKBccFygWTB5UHlQdNCE8ITQiVB5MHEAlPCE0ITwgQCRMJ2QkTCRAJEwnZCdwJ0ArcCdkJ3AnQCtkKzgkFCQQJzgkECc0JzwkFCc4JzwkGCQUJBglFCAUJBglGCEUIRQiKB0QIRQiLB4oHRgiLB0UIRgiMB4sHjAe/BYsHjAfABb8FvwU0BDMEvwUzBL4FwAU0BL8FwAU1BDQENASKA4kDNASJAzMENQSKAzQENQSLA4oDiwO6AooDiwO7AroCugLMAcsBugLLAbkCuwLMAboCuwLNAcwBzQHUAMwBzQHVANQAuwLQAc0BiwO+ArsCvgKLA40DNQSNA4sDjQM1BDcEwAU3BDUENwTABcIFjAfCBcAFwgWMB44HRgiOB4wHjgdGCEgIBglICEYISAgGCQkJzwkJCQYJCQnPCdIJtArSCc8J0gm0Cr0KBAreCd0J3QkDCgQKPggICf4ICAk+CEcIhAdHCD4IRwiEB40HuAWNB4QHjQe4BcEFwQUtBDYELQTBBbgFgwM2BC0ENgSDA4wDswKMA4MDjAOzAr0C2gnYCdYJ3QnbCdoJ2wndCd4JzQDPAcUBzwHNANYAvgLTAdAB0wG+AsECjQPBAr4CwQKNA48DNwSPA40DjwM3BDkEwgU5BDcEOQTCBcQFjgfEBcIFxAWOB5AHkAdICEoISAiQB44HCQlKCEgISggJCQwJ0gkMCQkJDAnSCdUJvQrVCdIJ1Qm9CsYK2QHbAdgB2QHcAdsB2AHWAdkBRwgLCQgJCwlHCEkIjQdJCEcISQiNB48HwQWPB40HjwfBBcMFwwU2BDgENgTDBcEFjAM4BDYEOASMA44DvQKOA4wDjgO9AsACNgkzCTQJMwk2CTUJ1gDSAc8B0gHWAN8AzQkDCcwJzQkECQMJBAlDCAMJBAlECEMIRAiJB0MIRAiKB4kHige9BYkHige+Bb0FvgUyBL0FvgUzBDIEMwSJA4gDMwSIAzIEiQO4AogDiQO5ArgCuQLLAcoBuQLKAbgCywHTANIAywHSAMoB1QHBAsMCwQLVAdMBjwPDAsECwwKPA5ADOQSQA48DkAM5BDoExAU6BDkEOgTEBcUFkAfFBcQFxQWQB5EHSgiRB5AHkQdKCEsIDAlLCEoI1QkOCQwJDgnVCdcJxgrXCdUJ1wnGCscKNAkUCRUJFAk0CTMJSQgPCQsJDwlJCEwIjwdMCEkITAiPB5IHwwWSB48HkgfDBcYFOATGBcMFxgU4BDsEjgM7BDgEOwSOA5EDwAKRA44DkQPAAsQCwgLAAr8CxwLJAsoCxgLEAsICxALGAscC3wDWAdIB1gHfAOkAwwLXAdUB1wHDAsUCkAPFAsMCxQKQA5IDOgSSA5ADkgM6BDwExQU8BDoEPATFBccFkQfHBcUFxwWRB5MHSwiTB5EHkwdLCE0ISwgMCQ4JDglNCEsITQgOCRAJ1wkQCQ4JEAnXCdkJxwrZCdcJ2QnHCtAKogLMAs0CzAKiAqECTAgSCQ8JEglMCE4IkgdOCEwITgiSB5QHxgWUB5IHlAfGBcgFOwTIBcYFyAU7BD0EkQM9BDsEPQSRA5MDxAKTA5EDkwPEAscC5QL2AeYC5QL1AfYB6QDZAdYB2QHpAPIAygkACckJygkBCQAJAQlACAAJAQlBCEAIQQiGB0AIQQiHB4YHhwe6BYYHhwe7BboFuwUvBLoFuwUwBC8EMASFAy8EMASGA4UDhgO1AoUDhgO2ArUCtgLHAbUCtgLIAccByAHPAMcByAHQAM8AyQn/CMgJyQkACf8IAAk/CP8IAAlACD8IQAiFBz8IQAiGB4UHhge5BYUHhge6BbkFugUuBLkFugUvBC4ELwSEAy4ELwSFA4QDhQO0AoQDhQO1ArQCtQLGAbQCtQLHAcYBxwHOAMYBxwHPAM4AyAn+CMcJyAn/CP4I/wg+CP4I/wg/CD4IPwiEBz4IPwiFB4QHhQe4BYQHhQe5BbgFuQUtBLgFuQUuBC0ELgSDAy0ELgSEA4MDhAOzAoMDhAO0ArMCtALFAbMCtALGAcUBxgHNAMUBxgHOAM0AHAlRCBYJHAlVCFEIVQiXB1EIVQibB5cHmwfPBZcHmwfTBc8F0wVABM8F0wVEBEAERASWA0AERASaA5YDmgPLApYDmgPRAssC0QLdAcsC0QLiAd0B4gEDAd0B4gENAQMBAgoyCdwJEwncCTIJMglmCBMJTwgTCWYIlQdPCKoHZgiqB08IlQfJBcoFyQWVB6oHPgTKBckFyQUlBD4ElAM+BCUEJQR1A5QDyAKUA3UDdQOeAsgC2gHIAp4CngKnAdoB+gDaAacBpwGDAPoAzwLNAswCzALOAs8CoAJ2A8oClQPKAnYDPwSVAyYEdgMmBJUDPwTLBcwFywU/BCYElgfMBcsFywWrB5YHUAiWB6sHqwdnCFAIFQlQCGcIZwg0CRUJqwHeAaoB3gGrAd8B2greCQQKBApXC9oKAgMhAiICAgMiAgMDywMCAwMDywMDA8wDzANuBMsDbgTMA28E9gVvBPkF9gVuBG8EZAf5BWUH+QVkB/YFHAhlBx0IHAhkB2UHHQjPCBwIzwgdCNAIlwnQCJgJ0AiXCc8IVQqYCVcKVQqXCZgJCAn9CP4ICAkKCQcJ/QgICQcJzwKXA80CzwKYA5cDmANBBJcDmANCBEEEQgTQBUEEQgTRBdAF0QWYB9AF0QWZB5gHmQdSCJgHmQdTCFIIUwgYCVIIUwgaCRgJswLEAbICswLFAcQB4wnjCuEJ4wnrCuMK3gHhAeAB4QHeAd8BogJ2A6ACogJ3A3YDdwMmBHYDdwMnBCYEJwTLBSYEJwTNBcsFzQWrB8sFzQWsB6sHrAdnCKsHrAdoCGcIaAg0CWcIaAg2CTQJ4wm7CbwJuwnjCeIJBgpXCwQKBgpaC1cLBwoyCQIKBwo3CTIJNwlmCDIJNwlpCGYIaQiqB2YIaQitB6oHrQfJBaoHrQfOBckFzgUlBMkFzgUoBCUEKAR1AyUEKAR4A3UDeAOeAnUDeAOjAp4CowKnAZ4CowKsAacBrAGDAKcBrAGIAIMA0QnHCcYJ0QnTCdQJxgnQCdEJdwOiAs0CzQKXA3cDJwR3A5cDlwNBBCcEQQTQBc0FzQWYB6wHzQUnBEEEmAfNBdAFrAdSCGgIUgisB5gHaAgYCTYJGAloCFIIGgnxCBkJ8QgaCfIIBgrjCloL4woGCuEJNwkHCt8J3wkWCTcJaQg3CRYJFglRCGkIrQdpCFEIUQiXB60HlwfPBc4FlwfOBa0HzgVABCgEQATOBc8FKASWA3gDlgMoBEAEeAPLAqMCywJ4A5YDowLdAawB3QGjAssCrAEDAYgAAwGsAd0BzAkCCcsJzAkDCQIJAwlCCAIJAwlDCEIIQwiIB0IIQwiJB4gHiQe8BYgHiQe9BbwFvQUxBLwFvQUyBDEEMgSHAzEEMgSIA4cDiAO3AocDiAO4ArcCuALJAbcCuALKAckBygHRAMkBygHSANEAywkBCcoJywkCCQEJAglBCAEJAglCCEEIQgiHB0EIQgiIB4cHiAe7BYcHiAe8BbsFvAUwBLsFvAUxBDAEMQSHA4YDMQSGAzAEhwO2AoYDhwO3ArYCtwLIAbYCtwLJAcgByQHQAMgByQHRANAAsAnhCK8J4QiwCeIIHglVCBwJHglXCFUIIggmCCUIJggiCCMInQfTBZsHnQfXBdMF2QjVCNgI2QjWCNUIRgSaA0QERgScA5oDLwh2By4ILwh3B3YH0wLiAdEC0wLjAeIB4wENAeIB4wESAQ0BAAMhAgIDAAMfAiECyQMCA8sDAgPJAwADbATLA24EbATJA8sD8wVuBPYF8wVsBG4EYgf2BWQHYgfzBfYFGghkBxwIZAcaCGIHzQgcCM8IzQgaCBwIlQnPCJcJlQnNCM8IUwqXCVUKlwlTCpUJ/QIdAv4C/QIcAh0CxgP+AscDxgP9Av4CaQTHA2oEaQTGA8cD6gVqBOsFagTqBWkEXwfrBWAH6wVfB+oFFwhgBxgIFwhfB2AHyggYCMsIyggXCBgIkgnLCJMJkgnKCMsITgqTCVAKTgqSCZMJ/wIfAgAD/wIeAh8CyAP/AgADyAMAA8kDawTJA2wEawTIA8kD7gVsBPMFbATuBWsEYQfzBWIHYQfuBfMFGQhiBxoIGQhhB2IHzAgaCM0IGgjMCBkIlAnNCJUJzQiUCcwIUgqVCVMKUgqUCZUJ/gIeAv8CxwP/AsgDxwP+Av8CagTIA2sEagTHA8gD6wVrBO4F6wVqBGsEYAfuBWEH7gVgB+sFGAhhBxkIGAhgB2EHywgZCMwIywgYCBkIkwnMCJQJkwnLCMwIUAqUCVIKlAlQCpMJoQmlCaQJpQmhCaIJIglZCCAJIglbCFkI6AViBOcFYgToBWMEoQfaBZ8HoQffBdoFJghtByUIJghuB20HSgSeA0gESgSgA54DCQZtBwcGCQZuB20H1wLkAdUC1wLlAeQB5QEYAeQB5QEbARgBvAP2ArsD9gK8A/cCIAlXCB4JIAlZCFcICQZ3BAcGCQZ4BHcEnwfXBZ0HnwfaBdcF1QN3BNQD1QN4BHcESAScA0YESASeA5wD1QMLA9QDCwPVAwwD1QLjAdMC1QLkAeMB5AESAeMB5AEYARIB1gHYAdQB1AHSAdYB5gKuA5gD5gKYA88CrgNCBJgDrgNZBEIEWQTRBUIEWQTbBdEF0QV+B5kHfgfRBdsFmQc4CFMIOAiZB34HUwjyCBoJ8ghTCDgI0gHUAdEBzwHRAc4BzwHEAcUB0QHPAdIB4wmQCusKkArjCbwJ9wHmAvYB9wHnAuYC5wKuA+YC5wKvA64DrwNZBK4DrwNaBFkEWgTbBVkEWgTdBdsF3QV+B9sF3QV/B34Hfwc4CH4Hfwc5CDgIOQjyCDgIOQjzCPII8wi8CfII8wi9CbwJvQmQCrwJvQmSCpAK4wLzARcCFwL4AuMCrAPjAvgC+ALBA6wDwQNXBKwDwQNkBFcEZATVBVcEZATcBdUF1QVaB3wHWgfVBdwFfAcSCDYIEgh8B1oHNgjFCO8IxQg2CBII7wiNCbkJjQnvCMUIuQlFCo4KRQq5CY0J8gHjAuIC4wLyAfMB4gKsA6sDrAPiAuMCqwNXBFYEVwSrA6wDVgTVBdQF1QVWBFcE1AV8B3sHfAc1CHsHfAfUBdUFfAc2CDUINgjuCDUINgjvCO4I7wi5CbgJ7wi4Ce4IuQmNCrgJuQmOCo0K5AL0AfIB5ALyAeICqwPkAuIC5AKrA60DVgStA6sDrQNWBFgE1AVYBFYEWATUBdgFewfYBdQF2AV7B30HNwh7BzUINwh9B3sHNQjwCDcI8Ag1CO4I7gi6CfAIugnuCLgJjwq6CbgJjwq4CY0K5AL3AfQB9wHkAucCrQPnAuQC5wKtA68DWASvA60DrwNYBFoE2AVaBFgEWgTYBd0FfQfdBdgF3QV9B38Hfwc3CDkINwh/B30H8wg3CPAI8wg5CDcIvQnzCPAIvQnwCLoJkgq6CY8Kkgq9CboJqQnTCJ0JqQndCNMI3QggCNMI3QgqCCAIKghoByAIKghyB2gHcgf9BWgHcgcQBv0FEAZyBP0FEAZ8BHIEfATZA88DfATPA3IE2QMGA88D2QMQAwYDEAMoAgYDEAM1AigCNQJ6ASgCNQKGAXoBGQL5AhgCGQL6AvkC+gLCA/kC+gLDA8IDwwNlBMIDwwNmBGUEZgTgBWUEZgTiBeAF4gVcB1sH4gVbB+AFXAcTCFsHXAcUCBMIFAjGCBMIFAjHCMYIxwiOCcYIxwiPCY4JjwlICo4JjwlMCkgKnQnOCJYJnQnTCM4I0wgbCM4I0wggCBsIGwhoB2MHaAcbCCAIYwf9BfUF/QVjB2gH9QVyBG0EcgT1Bf0FbQTPA8oDzwNtBHIEzwMBA8oDzwMGAwEDBgMgAgEDBgMoAiACKAJyASACKAJ6AXIBGAL4AhcCGAL5AvgC+QLBA/gC+QLCA8EDwgNkBMEDwgNlBGQEZQTcBWQEZQTgBdwF4AVaB9wF4AVbB1oHWwcSCFoHWwcTCBIIEwjFCBIIEwjGCMUIxgiNCcUIxgiOCY0JjglFCo0JjglICkUKownUCNcI1AijCZ4JIwjUCCEIIwjWCNQIJAhpB2wHaQckCCEICQb+BQUGCQYEBv4FBQZzBHYEcwQFBv4FcwRoBGYEaARzBHUE0wMHAwoDBwPTA9ADognWCNQIognUCJ4JKQKAAS4CKQJ7AYABaQduB2wHbgdpB2sH1wgkCCcI1wgnCNoIxQPQA8MDxQPSA9ADbAcNBm8HbAcFBg0GZAqlCaMJZAqjCWIKdgTWA3kEdgTTA9YDJgghCCQIJggjCCEICgMyAg0DCgMuAjICBwP8AvoC/AIHAwkDqQnaCN0I2gipCaYJ2QijCdcI2QilCaMJKghvB3IHbwcqCCcI1AjZCNcI2QjUCNYIDQZ8BBAGDQZ5BHwEGwItAikCGwIpAhkC1gMQA9kD1gMNAxAD2QgkCNcI2QgmCCQIMgKGATUCMgKDAYYB+gIpAgcD+gIZAikCpQmeCaMJpQmiCZ4JZgTQA3MEZgTDA9ADNwIyAjUCNwI0AjICXAf+BWkHXAfiBf4FbgckCGwHbgcmCCQIxwghCNQIxwgUCCEIDQMSAxADEgMNAw8DTAqeCV0KTAqPCZ4JAwMnAgUDJwIDAyICzAMDAwUDzAMFA84DbwTOA3EEbwTMA84D+QVxBPwFcQT5BW8EZQf8BWcHZQf5BfwFHQhnBx8IZwcdCGUH0AgfCNII0AgdCB8ImAnSCJwJmAnQCNIIVwqcCVsKVwqYCZwJnAnRCJsJnAnSCNEI0ggeCNEI0ggfCB4IHwhmBx4IHwhnB2YHZwf6BWYHZwf8BfoF/AVxBHAE/AVwBPoFzgMEA80DzgMFAwQDBQMlAgQDBQMnAiUCJwJ4ASUCJwJ5AXgBMAILAy8CCwMwAgwDWwgiCakIqQjyB1sIZwqnCWYKZwqoCacJ3wWhB1UHVQcmBt8F3AinCdsI3AioCacJSgTkA6AD5ANKBIIE3AgoCNsIKAjcCCkI1wJVAuUBVQLXAh4D5QGbARsBmwHlAVUCnAEcAZsBGwGbARwBsAE5AZ0BHQGdATkBHAr8Ch0K/AocCvsKWAoWCpoKFgpYCgEK6wn6ChsKGwp8CesJmgodCvwKHQqaChYKdgE5AbABvQF2AbABWgpVClcKWgpXClsKWwqbCVoKWwqcCZsJdQF4AXkBeAF1AXMBIgJ5AScCIgJ1AXkBYgpPCl0KTApdCk8K9AtPCvMLTwr0C1YKaApcCmUKVgplClwKbgcFBmwHbgcJBgUGegGDAXQBgwF6AYYB2wPWA9kD2wPYA9YDZQqjCaYJowllCmIKeAQFBnYEeAQJBgUGYwFlAWIBZQFjAW8BVApHClEKRwpUCk0KYgFkAWEBZAFiAWUBYwFyAW8BYwFrAXIBZAEXAmEBZAEYAhcC8QtWCvALUQrwC1YKRgpFCkgKSQpGCkgKSQpICkwKTwpJCkwKXAqWCVQKXAqdCZYJ7gtPCu8LTwruC0kKaAEYAmQBaAEZAhgCcgF0AW8BdAFyAXoBaAqdCVwKaAqpCZ0JdAGAAW0BgAF0AYMBbQF7AWgBewFtAYAB9AExAS4BMQH0AfcBRwqSCo8KkgpHCk0KMQFjAS4BawFjATEBZQFoAWQBaAFlAW0BVApRClYKVgpcClQKLgFiASwBYgEuAWMB7wtWCvELVgrvC08K9AEsAfIBLAH0AS4BLAHzAfIB8wEsAS0BRgqOCkUKRgqNCo4KLQFiAWEBYgEtASwB8wEtAWEBYQEXAvMBMQFmAWsBZgExAS8BMQH2AS8BMQH3AfYBSgqSCk0KkAqSCkoKbwFtAWUBbQFvAXQBFQEzATUBMwEVAREBkwr2CpcK9gqTCvIKDgEvAQwBLwEOATABFQEyARMBMgEVATUB7QrrCpAKkQrtCpAK4QEMAS8BLwH2AeEBFwEyATYBEwEyARcBlgr0CpMK8gqTCvQK9grtCpEKlwr2CpEKlAr0CpYK9AqUCvAK8AqVCu8KlQrwCpQKFwEwAQ4BMAEXATYBEQE0ATMBNAERARABcwE0ATcBNwF4AXMBlQr1Cu8K9QqVCpgKfAFEAToBRAF8AZABdgE4ATkBOAF2AXcBmQr8CvkK/AqZCpoKmAr5CvUK+QqYCpkKdwE3ATgBNwF3AXgBgwpeCpsKgwo4Cl4K9wrnCfEK9wrpCecJNwEWARoBGgE4ATcBFAEYARkBEgEYARQB8wr3CvEK9Qr4CvMKmApZCpkKmApaClkKmQpYCpoKmQpZClgK+Ar6CvcK+Ar1CvkK+wr5CvwK+Qr7CvgKGQEbARwBGAEbARkBOAEaAR0BOQE4AR0BVQqYCpUKmApVCloK7wr1Cu4K8wruCvUKDwEWARABFAENARIBDQEUAQ8BNAEQARYBFgE3ATQB+grpCfcK+grrCekJMwFzAXEBcwEzATQBNgFnATABZwE2AWwBlApVCpUKVQqUClMKUwqWClIKlgpTCpQKTgqXCksKkQpLCpcKUgqWClAKkwpQCpYKNgFuAWwBMgFuATYBTQpmAUoKTQprAWYBHgL+Ah0CHQJuAXABHQJwAR4CHgJwAXEBHgJxAR8CHAJuAR0CHAJsAW4BHwJzASECcwEfAnEBXgvgCmAL4ApeC98K2gDkAOMA5ADaANsA0gDbANoA0gDaANEA9wAAAQEBAAH3APgA8AroCvQK6ArwCucKYAtlC14L5wpmC+gKZgvnCmULCgGUAAkBkwAJAZQAjgCTAJQAjQCTAI4ACgEJARUBEwEKARUB8QrnCeUJ8QrlCewKjgACAY0AAQGNAAIB1grNCswK1grMCtUKzQrBCswKzQrCCsEK1QrhCuAK4ArWCtUKuAqwCq8KsAq4CrkKwgq4CsEKwgq5CrgK3QpeC1wLXgvdCt8KuQqxCrAKsArLCcoJsArKCa8KsQrLCbAKsQrMCcsJ3wrXCtYK1grgCt8K7wD5APgA7wDjAOQA5ADwAO8A2wDlAOQA0wDcANsA0wDbANIA+AD+AAAB+ADuAO8AjQABAYsAAAGLAAEBSwqRCkoKkApKCpEKFQEJAREB7wrnCvAK5wrvCuYKZQtgC2YLXgtjC1wLjQCSAJMAiwCSAI0AYwvnCuYK5wpjC2ULCQGTAAgBkgAIAZMAjwAEAYcABAGPAAYBlAALAZEACwGUAAoBBwpbC+IK4grfCQcKkQAGAY8ABgGRAAsB5ApjC+YKYwvkCl8LCAGQAAcBkAAIAZIAYgvjCuUK4wpiC1oLBQGMAAMBiAADAYwA6gpkC+UK5QpkC2ILqwGHAAQBBAHfAasBZwvqCukK6gpnC2QL6ApnC+kKZwvoCmYLhgCHAIQAhwCGAI8AlACKAI4AigCUAJEAWwsCClYLWwsHCgIKigCPAIYAjwCKAJEAYwteC2ULXAtjC18LWAtfC1sLXwtYC1wLiwCQAJIAiQCQAIsAWgtiC1kLWgtZC1cLjACFAIgAgwCIAIUAXQtZC2ILZAtdC2ILhwCrAakBhwCpAYQAXQtnC2ELZwtdC2QLZgthC2cLYQtmC2AL6Qr0CugK9ArpCvIK8grqCvYK6gryCukKDAHfAQQBDAHhAd8B6grlCu0K9grqCu0KBQENAQ8BAwENAQUB5QrjCusK6wrtCuUKEQEJAQgBEAERAQgBEAEIAQcBNQFuATIBbgE1AXABMAFmAS8BZgEwAWcBUAqXCk4KlwpQCpMKNQFxAXABcQE1ATMBIQJ1ASICdQEhAnMB+wCGAIQAhgD7AP0AhQD6AIMA+gCFAPwA3gphC+EKYQveCl0LhACpAfsA3AH7AKkB2wpcC1gLXAvbCt0KVgsCCtkK3AnZCgIKWQvaClcL2gpZC9wK7grmCu8K5gruCuQKCwEOAQYBDgELARcB5QkWCd8J5QkcCRYJ7ArlCd8J7ArfCeIKCwEKARMBCwETARcBBgEMAQQBDAEGAQ4B9AD/AP0A9AD9APMAtgqtCqwKtgqsCrUKiwD+AIkA/gCLAAABrQrICccJrQrHCawK2grSCtEKyQq/Cr4KyQq+CsgK9ADqAOsA9gD/APQA/wD2AAIB2ADOAM8AzwDZANgA2ADiAOEA4gDYANkA6wDgAOEA4QDXANgA4QDtAOsA6wD2APQA9gDrAO0Avwq1Cr4Kvwq2CrUKygq/CskKygrACr8K0graCtwK0grICtEK0grJCsgK3ArTCtIKrgrICa0KrgrJCcgJ/wCOAIoAjgD/AAIBtwquCq0KtwqtCrYKwAq2Cr8KwAq3CrYK9gABAQIBAQH2APcA0QDZANAA0QDaANkA2QDPANAA2QDjAOIAwQq3CsAKuAq3CsEKuAqvCq4KuAquCrcK/QCKAIYAigD9AP8ArwrJCa4KrwrKCckJ0wrKCskK0wrJCtIK0wrcCt4K3grVCtMK1QreCuEKzArACsoKwQrACswK7QDhAOIA4wDvAO4A4wDZANoA4gDuAO0A7gDiAOMA7gD4APcA1QrMCsoK1QrKCtMK7QD3APYA9wDtAO4A2AnRCsgK0QrYCdsJywrUCtAK0ArHCssK1QHxAOgA8QDVAdcB1wrNCtYK1wrOCs0K2ArOCtcK2ArPCs4K6QDzAPIA6gD0APMA6gDfAOAA1AnICr4KyArUCdgJywrFCsQKxwrGCsUK0wHoAOcA6ADTAdUBzgrCCs0KzgrDCsIKzwrDCs4KzwrECsMK8AD1APkA8ADkAOUA5gDoAOwA5QDsAPAA5QDbANwA7ADlAOYA9QD8APkA9QDwAOwA/AD1APoA+QDvAPAA/gD5APwA/gD4APkA1wrfCt0K3QrYCtcKsgrMCbEKsgrNCcwJXQvcClkL3ApdC94KsQq5CroKsgq6CrsK1ADdANwA3ADTANQAwwq5CsIKwwq6CrkKugqyCrEKuwqzCrIKxAq7CroKxAq6CsMKxArPCssK3ADmAOUA5gDcAN0A3wDqAOkA4ADrAOoA4ADWANcA0Qm+CrUKvgrRCdQJxQrLCscKxQq7CsQKxQq8CrsKxgq9CrwKxgq8CsUK0AHnAN4A5wDQAdMBzgDYANcA1gDgAN8A1wDhAOAA1wDNAM4AzQDXANYAxwm1CqwKtQrHCdEJvAq0CrMKtAq8Cr0K1QDQAd4A0AG7Ar4C0AHVAM0BYAvhCmEL4QpgC+AKtArOCbMKtArPCc4J1wH6APEA+gDXAdoB1ArbCtkK2QrQCtQK2wnaCtEK2grbCd4J8wD9APsA8wDpAOoA8wD7APIApQenB6YHpgekB6UHYAhfCF4IXwhgCGEIdwctCHUHLQh3By8IJgkoCScJJwklCSYJ6QIAAugCAALpAgEC8QnvCe4J7wnxCfIJ4giuCeAIrgniCLAJ6QKxA+gC6QKyA7EDXASyA7EDXASxA1sEXATtBVsE7QVcBO8F7QnvCe4J7gnsCe0JgQftBYAHgQfvBe0FJAklCSYJJQkkCSMJgQczCIAHgQc0CDMIXQhfCF4IXghcCF0I7QgzCOwI7Qg0CDMIowekB6UHpAejB6IHswnnCOYIswnmCLIJ7Qi2CewItgntCLcJ9AX4BfcF9wXyBfQFMQjnCOYIMQjmCDAITARNBE4ETQRMBEsEMQh4BzAIMQh5B3gHogOkA6MDowOhA6ID7AV5B3gH7AV4B+kF2QLaAtsC2gLZAtgC7AVfBOkFXwTsBWAEtgNfBLUDtgNgBF8EtgPsArUD7AK2A+0CBgLsAgUC7AIGAu0CBQvxCQQLBQvyCfEJBAsACwILtgmzCbIJswm2CbcJ5gjtCOwI7QjmCOcIKAlhCGAIKAlgCCcJNAgwCDMIMAg0CDEIeAeBB4AHgQd4B3kHpwcLBqYHpwcMBgsG7wXpBe0F6QXvBewFswl/CrIJfwqzCYAKXwRcBFsEXARfBGAEUASlA08EUASmA6UDwgmgCsEJoArCCaEKsgO1A7EDtQOyA7YD6QHaAugB6QHbAtoC7ALpAugC6QLsAu0CpAPaAqMD2gKkA9sC3QLrAdwC6wHdAuwBAAIGAgUCBgIAAgECpANNBKMDTQSkA04E+AVOBE0E+AVNBPcF+AWlB6QH+AWkB/cFsAmyCa4JswmyCbAJ7wHhAuAC7wHgAu4BXwikB14IpAdfCKUH5wjgCOYI4AjnCOIIqQPgAqgD4AKpA+ECXwglCV4IJQlfCCYJLQgxCDAIMQgtCC8IqQNTBKgDUwSpA1QE7wklCe4J7wkmCSUJeQd1B3gHdQd5B3cH8QVTBPAF8QVUBFME7wkAC+4J7wkCCwAL5gXsBekF7AXmBegF8QWCB/AFggfxBYMHYARhBF8EYQRgBGMEPQiCBzwIPQiDB4IH2QLmAdgC5gHZAucBugO2A7UDtgO6A7wDPQj3CDwIPQj4CPcI2QKhA9gCoQPZAqID7QL1AuwC9QLtAvcCwgn3CMEJ9wjCCfgITAShA0sEoQNMBKID7AnCCcEJwgnsCe0JTATyBUsE8gVMBPQF9wgkCSMJJAn3CPgIowfyBaIHowf0BfIFXQg8CFwIPAhdCD0IoQr9CqAKoQr+Cv0K6AKxA6gDqAPgAugC8AVbBO0F8AVTBFsEMwiCB4AHggczCDwItgn3COwI9wi2CcEJrgnmCOAI5giuCbIJLQh4B3UHeActCDAI6QVhBOYF6QVfBGEE9QK6A7UDtQPsAvUCfwq2CbIJtgl/CocKBQLsAugC6AIAAgUCtQNfBFsEWwSxA7UDeAftBekF7QV4B4AH5ggzCDAIMwjmCOwI9whcCDwIXAj3CCMJggfyBfAF8gWCB6IHqANTBEsESwShA6gD7gHgAtgC2ALmAe4B3ALoAdoC6wHoAdwCTwSjA00EpQOjA08Epgf3BaQHCwb3BaYHJwleCCUJYAheCCcJBAvuCQALBAvxCe4J7gkjCewJ7gklCSMJXgiiB1wIXgikB6IH9wVLBPIF9wVNBEsEowPYAqEDowPaAtgCoArsCcEJ7AmgCv0KZgnHB2UJZgnIB8cHIASHAh8EIASIAocCYwlmCWUJZgljCWQJyAfFB8cHyAfGB8UHRwnaB0gJRwnZB9oHHQQgBB8EIAQdBB4EAgSaAgMEAgSZApoCiAKFAocCiAKGAoUCYQljCWAJYQlkCWMJxQfDB8IHwwfFB8YHRQnYB0YJRQnXB9gHGwQdBBoEGwQeBB0EAASXApgCAASYAgEEhQKDAoICgwKFAoYCWwlaCVkJVglcCVsJXAlWCVgJvAe+B70HvAe9B7sHvge4B70Hvge6B7gHQQnQBz4JQQnTB9AHPQnPB9IHPQnSB0AJFQQUBBMEFAQVBBYEFgQQBBIE/AOQAvkDkAL8A5MC+AOPApIC+AOSAvsDfAJ+An0CfAJ9AnsCfgJ4An0CfgJ6AngCgwJ/AoICgwKBAn8CgQJ8AnsCgQJ7An8CFwQbBBoEGwQXBBkE/gOVApYC/gOWAv8DEwQZBBcEGQQTBBQE+gOUAv0D+gORApQCwwe/B8IHwwfBB78HwQe7B78HwQe8B7sHYQldCV8JQwnWB0QJQwnVB9YHXQlhCWAJWglbCVwJWQlfCV0JXwlZCVoJPwnUB0IJPwnRB9QHVAlYCVYJWAlUCVUJOwnOBzwJOwnNB84Huge3B7YHuge2B7gHEAQWBBUEEgQOBA8EegJ2AngCegJ3AnYC9gOOAvcD9gONAo4CTwlVCVQJTwlUCU4JtgexB7AHsQe2B7cHOQnMBzoJOQnLB8wHsQdOCbAHsQdPCU4JDgQSBBAECQQPBA4ECQQOBAgEdgJxAnACcQJ2AncC9QOLAowCiwL1A/QDcQIIBHACcQIJBAgEdwKLAnECiwJ3AowC9AP1Aw8EDwQJBPQDtwfLB7EHywe3B8wHOQk6CVUJVQlPCTkJfgKPAnoCjwJ+ApIC+AP7AxYEFgQSBPgDvgfPB7oHzwe+B9IHPQlACVwJXAlYCT0JgQKRAnwCkQKBApQC+gP9AxkEGQQUBPoDwQfRB7wH0QfBB9QHPwlCCV8JXwlaCT8JfgJ8ApACkAKTAn4C+QMWBPwDFgT5AxQEvge8B9AH0AfTB74HPglcCUEJXAk+CVoJegKNAncCjQJ6Ao4C9gP3AxIEEgQPBPYDugfNB7cHzQe6B84HOwk8CVgJWAlVCTsJgwKVAoEClQKDApYC/gP/AxsEGwQZBP4DwwfVB8EH1QfDB9YHQwlECWEJYQlfCUMJhgKXAoMClwKGApgCAAQBBB4EHgQbBAAExgfXB8MH1wfGB9gHRQlGCWQJZAlhCUUJiAKZAoYCmQKIApoCAgQDBCAEIAQeBAIEyAfZB8YH2QfIB9oHRwlICWYJZglkCUcJ2gdmCUgJZgnaB8gHmgIgBAMEIASaAogCRgnZB0cJRgnYB9kHAQSZAgIEAQSYApkCRAnXB0UJRAnWB9cH/wOXAgAE/wOWApcCPAnOB88HPAnPBz0J9wOOAo8C9wOPAvgDPgnRBz8JPgnQB9EH+QORAvoD+QOQApECQAnTB0EJQAnSB9MH+wOTAvwD+wOSApMC/QOVAv4DlQL9A5QCQgnVB0MJ1QdCCdQHOgnNBzsJOgnMB80H9gOMAo0CjAL2A/UDTwnLBzkJTwmxB8sHCQRxAosCCQSLAvQDRglHCWQJ2AfGB9kHAQQCBB4EmAKGApkCRAlFCWEJ1gfDB9cH/wMABBsElgKDApcCPQlYCTwJzwfOB7oH+AMSBPcDjwKOAnoCPwlaCT4J0QfQB7wH+gMUBPkDkQKQAnwCQAlBCVwJ0ge+B9MH+wP8AxYEkgJ+ApMClAKBApUC/QP+AxkE1AfBB9UHQglDCV8JOgk7CVUJzAe3B80H9QP2Aw8EjAJ3Ao0CsAcOBLYHsAcIBA4EdwtUCX8LdwtOCVQJuAcVBL0HuAcQBBUEhQtbCYgLhQtWCVsJuwcXBL8HFwS7BxMEhwtdCYkLXQmHC1kJvQcVBBMEvQcTBLsHiAtZCYcLiAtbCVkJXgm5B1cJXgnAB7kHwAcRBLkHwAcYBBEEGAR5AhEEGASAAnkCgAJ+AHoAgAJ6AHkCHASEAoACHASAAhgEjAteCYoLjAtiCV4JUwm0B1IJUwm1B7QHtQcMBLQHtQcNBAwEDQR0AgwEDQR1AnQCdQJvAHQCdQJxAG8AVwm5B7UHVwm1B1MJuQcNBLUHuQcRBA0EEQR5AnUCEQR1Ag0EeQJxAHUCeQJ6AHEAtgcQBLgHtgcOBBAEfwtWCYULVgl/C1QJvwcaBMIHvwcXBBoEiQtgCYsLiQtdCWAJCgRvAgcECgRyAm8CwgcdBMUHwgcaBB0EiwtjCY0LYwmLC2AJUAmyB68HUAmvB00JUQmyB1AJUQmzB7IHsgcHBK8HsgcKBAcEcgJpAG8CcgJsAGkAxQcdBB8ExQcfBMcHjQtlCY4LZQmNC2MJUgm0B7MHUgmzB1EJswcKBLIHswcLBAoECwRyAgoECwRzAnICtAcLBLMHtAcMBAsEDARzAgsEDAR0AnMCcwJsAHICcwJuAGwAdAJvAG4AdAJuAHMCHwSHAoQCHwSEAhwEHATHBx8ExAcYBMAHxAccBBgExwccBMQHZQnEB2IJZQnHB8QHjgtiCYwLYgnAB14JYgnEB8AHYgmOC2UJCgX4AfkB+AEKBQkFhwmOBoUGjgaHCYgJdAVkApkFZAJ0BVIC+gELBfkBCgX5AQsFiAmPBo4GjwaICYkJZAVSAnQFUgJkBT0CDQUHAjEFBwINBfwBtAnjCK0L/QGQBMEBkAT9AQ4F4wj2ByIL5Qa0CQMMiQTBAZAEwQGJBLkB9gdRB/8J/wkiC/YHMQURAjcFEQIxBQcC+AEJBWQCmQVkAgkFUQeVBTgJOAU9AmQFPQI4BSYCAwITBQICEQUCAhMFiQS6AbkBugGJBIoENwUmAjgFJgI3BRECBAIUBQMCEwUDAhQFiQTqAYoE7AGKBOoBAwzFCeUGxQkDDPIJ8glNB8UJrQsiC2EICwUlAgoFJAIKBSUCOAVWAjcFTAI3BVYCIgutC+MIIgv/CacHYQgoCa0LDgUrApAE+wGQBCsCEwUxAhEFJQIRBTECOAn/CVEH/wk4CQwGpwdhCCILDAanB/8JdAVqAmQFXQJkBWoCmQWJAnQFagJ0BYkCMQVFAg0FKgINBUUCZAVdAjgFVgI4BV0CNwVMAjEFRQIxBUwCkAT7AYkE6gGJBPsBFAU4AhMFMQITBTgCCgUkAgkFIwIJBSQCCQWJApkFiQIJBSMCrQsDDLQJAwytCygJKAnyCQMMhgnEBp8JxAaGCaMGQgZpCcAGmQnABmkJvglMB/AJTAe+CUMHmQnBBsAGwQaZCZoJaQlkBnIJZAZpCUIGeAmeBn0JngZ4CWsG8glMB00HTAfyCfAJfQmiBn8JogZ9CZ4GfwmjBoYJowZ/CaIGmgnCBsEGwgaaCZsJcglrBngJawZyCWQGoAlDB74JQwegCcUGRApxC0MKcAtDCnELPgE9ASgBKQE+ASgBRAE/ASoBPwFEAVsBogqKCqMKigqiCokKiArCCbcJwgmICqEKlgFFAkwCRQKWAZABigqDCqMKgwqKCncKJAp/CYYJhgk4CiQKPQEBAu8B7wEoAT0BAQLpAuEC4QLvAQECYwTmBWEE5gVjBOgFsgNcBFQEVASpA7IDCwb4BfcF+AULBgwGgQfxBe8F8QWBB4MHTgRQBE8ETwRNBE4E7Qg9CDQIPQjtCPgIiAq2CYcKtgmICrcJoQqJCqIKiQqhCogKPwE+ASkBKgE/ASkBWgFZAUIBQwFaAUIBgAp1CoEKdQqACnQK9wK6A/UC9wK8A7oD4ggxCC8IMQjiCOcI4QLoAuAC6ALhAukCdwfsBegF7AV3B3kHpQOkA6MDpAOlA6YDvANjBGAEYAS2A7wDsQOpA6gDqQOxA7IDEAL3Au0C7QIGAhACWAEQAgYCBgJBAVgBGAp4CX0JfQkfChgKdgpCCoIKQgp2CjkKpQFWAl0CVgKlAZkBdAqzCbAJswl0CoAKgQp2CoIKdgqBCnUKhwFaAUMBWgGHAYwBWQFYAUEBQgFZAUEBQgFBAT0BPgFCAT0BWwFDAT8BQwFbAYcBiQqCCooKggqJCoEK2wLdAtwC3ALaAtsCmQFMAlYCTAKZAZYBggp3CooKdwqCCkIKHwp9CX8JfwkkCh8KQQEGAgECAQI9AUEBVARbBFMEWwRUBFwE7QK2A7IDsgPpAu0C6wHpAegB6QHrAewB7wVgBOwF7wVcBGAE7QXxBfAF8QXtBe8FMQiBB3kHgQcxCDQIgweAB4IHgAeDB4EHswntCOcI7QizCbcJiAqBCokKgQqICoAKQwFCAT4BPwFDAT4BAQppCXIJcgkUCgEKvQFqAokCagK9AbIBsgFdAmoCXQKyAaUBFApyCXgJeAkYChQKKgEpAR8BIAEqAR8BAgsFCwQL/gr/CgILIgEfASMBHwEiASAB/gqiCv8Kogr+CqEK6gEjAewB6gEiASMBAwvyCQULAwvwCfIJWQqaCZkJWQqZCVgKWgqaCVkKWgqbCZoJwgkkCfgIJAnCCe0JgAqHCn8KgAqICocKPQijB4MHowc9CF0IMwg9CDwIPQgzCDQI9AVUBPEF9AVMBFQE5gHvAe4B7wHmAecB4QKpA6IDogPZAuEC+AjsCPcI7Aj4CO0IKAHvAecB5wEeASgBOAqGCZ8JnwleCjgK4ALZAtgC2QLgAuECpgPbAqQD3QLbAqYDtwnBCbYJwgnBCbcJDAZOBPgFUAROBAwGogOoA6EDqAOiA6kDYQilB18IpwelB2EI/grsCf0K/grtCewJ8gkmCe8JJgnyCSgJUwRMBEsETARTBFQE+wF9ATsBfQH7ASsCJAnsCSMJ7AkkCe0JJglfCF0IJgldCCQJ9AXwBfIF8AX0BfEFpQf4BfQFpQf0BaMHJAlcCCMJJAldCFwITgSiA0wETgSkA6IDggejB6IHoweCB4MH2wLpAecB2wLnAdkC6QEeAecB6QEhAR4B6gE7ASIBOwHqAfsBAQqZCWkJmQkBClgKIwK9AYkCdgG9ASMCowqbCgELowqDCpsK8AkDC5wKnAq+CfAJAwsBC5wKnAoBC5sKfQE6ATsBfAE6AX0BOwEgASIBIAE7AToBAgvtCf4KAgvvCe0JmwpfCpwKmwpeCl8KvgmcCl8KXwqgCb4J7AEhAekBIwEhAewBkAEqAkUCKgKQAXwBowdcCKIHXAijB10I/wqjCgELowr/CqIKIwEfASEBHgEhAR8BBQv/CgML/woFCwILAwv/CgELJQJ3ASQCJQJ4AXcBOgEqASABKgE6AUQBKQEoAR4BHwEpAR4BJAJ2ASMCJAJ3AXYBdwBuAG8AbwB4AHcAhwKAAIQChwKCAIAAewtRCXoLewtSCVEJgwt7C3oLewuDC4QLdwCAAIIAgAB3AHgAdgBsAG4AbgB3AHYAhQKBAIIAhQKCAIcCjQuDC4ILgwuNC44LegtRCVAJegtQCXgLgwuOC4wLeguCC4MLdwCCAIEAdwCBAHYAdABpAGwAbAB2AHQAggKBAIUCggJ/AIEAeAtNCXULeAtQCU0Jggt6C3gLgguLC40LeAuAC4ILdgCBAH8AdgB/AHQAcAuICYcJiAlwC3ILOAJqAGYAOAJmADECeQt1C3YLZQBmAGcAdguICXILdguJCYgJJQJmAGQAZgAlAjECaQByAG0AaQB0AHIAfwJ9AH8AfwJ/AIICiwuCC4ALiQuAC34LgAt4C3ULgAuJC4sLfgt1C3kLdQt+C4ALdAB/AH0AdAB9AHIAdgJ5AHgCdgJzAHkAeAB+AIAAfgB4AHoAdQBtAHIAbQB1AHAAfwuFC3wLeQt8C4ELeQuBC34LeQBwAHUAhgtTCX0LhgtXCVMJhAt9C3sLfQuEC4YLfQtTCVIJfQtSCXsLjAuEC4MLhAuMC4oLiguGC4QLhAKAAH4AhAJ+AIACigteCVcJigtXCYYLfAuFC4ELeQt0C3wLdAt5C3MLawBwAHMAcABrAGgAfQJ7AHsCewB9AnwAfAB1AHsAdQB8AHkAfguBC4cLiQt+C4cLgQuIC4cLhQuIC4ELewByAH0AcgB7AHUAeABvAHEAcQB6AHgAewJ9AH8CfQB7AnsAcAB5AHMAaABtAHAAcwt5C3YLcQtzC3ILcgtzC3YLcgtwC3ELfAt3C38Ldwt8C3QLeAJ5AHwAeAJ8AH0CcAJrAHMAcAJzAHYCbQBoAGcAZgBlAGQAZwBqAG0AagBnAGYAvwmFCj8KPwqtCb8JnwoJC6sKJAurCgkLCAqECqcKhAoICvoJvAGkArYBvAGmAqQClQGSAZcBkgGVAYsBQwKuAaQBrgFDAksC6QOuAqwC6QOsAuoDdAh5CYoJeQl0CG0IcwXeB+IH3gdzBXEFOQMkBCkEJAQ5AzoDhgRTB08HTweFBIYE7wdKCTAJMAntB+8H6gOFBOkDhQTqA4YErAJLAkMCSwKsAq4CpAGSAYsBkgGkAa4BlwG2AZUBtgGXAbwBign6CQgKeQn6CYoJOQOmAjoDpgI5A6QCpwqrCiQLhAqrCqcKnwqFCgkLhQqfCj8KrQlKCb8JSgmtCTAJ6AdOB+sHTgfoB0sHBgRsBSEEcAUhBGwF3AdkCN0HagjdB2QI3gdtCOIHdAjiB20IJARxBSkEcwUpBHEF7QdTB+8HUwftB08HxAmACQAKgAnECWcJIAtQC5MLUAsgC50KaQtJC5sLaQsGC0kLMAOdAjgDnQIwA5sC9gkTCj0KrAkTCvYJkwGmAV8BpgGTAbQBkQGIAUABiAGRAaMBpQI/AhICPwKlAqoC7QOHBOsDhwTtA4gEOgiACWcJZwnwBzoIiATJB1YHVgeHBIgEMAMrBFEEKwQwAzgDmAXmB+4H5geYBXcFhQisCfYJrAmFCHYI6wOlAu0D6wOqAqUCEgKjAZEBowESAj8CiAFfAUABkwFfAYgBmwKmAbQBtAGdApsCPQoGC2kLBgs9ChMKIAuTC0kLmwtJC5MLAApQC50KnQrECQAK3gIFBOsCIgTrAgUE7QHeAgsC6wILAt4CCwIKAu0BQAIKAgsCQAJcAzEDMQMKAkACDQpsCfwJSQn8CWwJ/An1Cf0J/QkNCvwJKQn9CfUJ9QnrCCkJjAk0ChcKFwp6CYwJCAuLCaQKggmkCosJPgqlCnEKpQo+CoYKCAsLC0gLCwsIC6QK+AkiCnkKIgr4CcMJ+QlICwsLCwvzCfkJwgGnAr8BwgGpAqcCtwEYA8MBIAPDARgDogGaAa8BmgGiAZQBtQG3AcMBtwG1Aa0BUQKzAbEBswFRAlkCtQGtAqgCqAKtAbUB5QOwAucD5QOxArACrQIsBCoEKgSoAq0CYwj5CfMJ8wliCGMIbAnnB0kJ5AdJCecHaghoCXcJaAlqCGQIKgViCDAFKgVjCGIIGAMqBSADMAUgAyoF5AePBNEEjwTkB+cHXAPRBI8EjwQxA1wDcAXcB90H3AdwBWwFOwMGBCEEBgQ7A10D4QcsBOAHLAThByoE4AeCCeEHiwnhB4IJBQSpByIEIgSpB6gHqQcpCesI6wioB6kHhAROB0sHSweDBIQE6wcuCfUI9QjoB+sHKQnoB/UIKQmpB+gHegkXCv0J/QkpCXoJcQoNCj4K/Qk+Cg0KMQMGBF0DMQOPBAYEbAXnB9wH5wdsBY8EbAkNCsMJIgrDCQ0KCgIxA6kCqQLCAQoCZAhsCWgJ5wdsCWQI7QGvAZoBCgKvAe0B3gLtAVkCWQLtAbMB5QPeArECBQTeAuUDSwcFBIMEBQRLB6kH5wODBOUDgwTnA4QEsAJZAlECWQKwArECsQGaAZQBmgGxAbMBrwG/AaIBvwGvAcIBdwnDCfgJaAnDCXcJOwOpAl0DqQI7A6cCeQoiCnEKeQpxCqUKPgo0CoYKNAo+ChcKegkuCYwJLgl6CfUI4QeLCToIOgiLCYAJUAsACggLiwkICwAKkwtIC5sLSAuTCwgLKgUYA1EEUQQYAzADYwiYBe4HmAVjCCoFPQppC0gLSAv5CT0KpgGbArcBGAO3AZsChQj2CfkJ+QljCIUIrQFfAbcBXwGtAUABEgKRAa0BrQGoAhICqAIqBO0DqALtA6UCKgTJB4gEyQcqBOEHdgiFCO4H5gd2CO4HdwWYBVEEKwR3BVEE8AfJBzoIyQfwB1YH5QODBAUEWQKxAt4CmgGzAe0BwgGvAQoCwwloCWwJXQOpAjEDcQoiCg0KFwo+Cv0J9Qh6CSkJAAqACYsJkwtQCwgLaQubC0gLmwIwAxgD9gk9CvkJXwGmAbcBkQFAAa0BpQISAqgCiATtAyoE7geFCGMI3AfnB2QIUQSYBSoFBgSPBGwFOgjJB+EH6AepB0sH4AfrCIIJ6wjgB6gHggn1CaQK9QmCCesIpAr1CfwJpAr8CQsLIAPRBFwD0QQgAzAF5AcwBWIIMAXkB9EE/AnzCQsL8wn8CUkJIANAAsMBQAIgA1wDSQliCPMJYghJCeQHCwLDAUACCwK1AcMBCwKtArUBrQILAusCrQIiBCwEIgStAusCLASoB+AHqAcsBCIERwf2CPwI9ghHB0QHcAidBXMIcAieBZ0FNAVrAzMFNAVpA2sDMwNeAjUDMwNbAl4CYQJHAl8CRwJhAkoCagkjC0YLIwtqCUwJbAJ/A24CbAJ6A38D4gNmBmkGZgbiA+ADkAu4C7wLuAuQC28Lygu5C7sLuQvKC8ULBwu0C7ILsguqCgcLrghyCmsKawqsCK4IMwWeBTQFngUzBZ0FNQNpAzMDaQM1A2sDSgJbAkcCWwJKAl4CbgJfAmwCXwJuAmECRgtvC5ALbwtGCyMLfwPgA+ID4AN/A3oDvAvFC8oLxQu8C7gLuQu0C7sLtAu5C7ILqgpyCgcLcgqqCmsKhghvCKsIbwiGCGUIzAZCB2UGZQZDBswGLwlLCfQI9AjBCC8JyQb5CN4GyQYtCfkIQgfBCPQIwQhCB8wGxAihBqgGoQbECOkIcgV1CJsFmwV1CG4IgQgnBnoIJwaBCHsFZQifBW8IZQigBZ8FaANyBYADmwWAA3IFpwMnBnsFewVwA6cDLwVqAzUFLwVlA2oDZQJoA6sCgAOrAmgDrwKnA3ADcANrAq8CHwNcAjQDHwNUAlwCqwKKAmUC3wKKAqsCrwKcAuoCnAKvAmsCYAI+AlgCPgJgAkgCpgrpCDwKxAg8CukILQklC4sKiwr5CC0JSwmpCiELqQpLCS8J3wLoA7kDuQOKAt8C6AOoBqEGoQa5A+gDnALdA+oC7APqAt0DZgJ7A20CZgJ5A3sD3QPJBuwD3gbsA8kG4QNDBmUGQwbhA9wDrAumCp4LPAqeC6YKJQu1C6kLqQuLCiULbQuvC7YLrwttC0sLrAueC6oLlAuqC54LsAupC7ULqQuwC6YLrgvEC7MLxAuuC7cLIAqqC5QLlAsMCiAKDApuCHUIdQggCgwKGgqmC30KsAt9CqYLqAqxC6sLqwt8CqgKeggaCoEIfQqBCBoKqwhBChkKGQqGCKsIewVzCJ0Fcwh7BYEIcAMzBWsDcAN7BTMFawJwAzUDNQNeAmsCawJhApwCYQJrAkoCJQstCWoJJQtqCUYL3QOcAn8DbgJ/A5wCtQslC5ALkAu8C7ULLQlHB/wIRwctCckGyQbdA2kGaQbdA+IDuwu1C8oLtQu7C7ALtAsHC7ALfQqwCwcLgQh9Cq4Irgh9CnIK9ghMCfwIagn8CEwJZgZEB2kGRwdpBkQHrAhzCK4IcwisCHAIIAp1CIYIIAqGCBkKfAqrC6oLqgsgCnwKrAuqC64LswusC64LuQNDBtwDuQOhBkMGzAbpCMEI6QjMBqEGrwtLC6wLpgqsC0sLigK5A3kDeQNmAooCLwmmCqkK6QimCi8JZQJYAj4CigJYAmUCaANlAh8DHwNlAlQCcgVoAy8FLwVoA2UDZQhyBaAFcgVlCHUInwUvBTUFLwWfBaAFNANlAx8DZQM0A2oDVAJIAlwCSAJUAj4CbQJYAmYCWAJtAmACIQtLC20LSwshC6kKewPcA+ED3AN7A3kDtguzC8QLswu2C68LrguxC7cLsQuuC6sLfApBCqgKQQp8ChkKagktCfwIRwfJBmkGcwiBCK4IzAZDBqEGLwnBCOkIZQiGCHUILwWgBXIFnQUzBXsFHwNlA2gDawM1A3ADPgJUAmUCXgJKAmsCZgJYAooCYQJuApwCSwupCqYKRguQCyUL3AN5A7kDfwPiA90DswuvC6wLvAvKC7ULqwuuC6oLuwu0C7ALGQp8CiAKBwtyCn0KeggMChoKDAp6CG4IGgqUC6YLlAsaCgwKqQuUC54LlAupC6YL7AOoBugDqAbsA94G3gbECKgGxAjeBvkInguLCqkLiwqeCzwK7APfAuoC3wLsA+gDPAr5CIsK+Qg8CsQIqwLqAt8CqwKvAuoCrwKAA6cDgAOvAqsCpwObBScGmwWnA4ADJwZuCHoIbggnBpsFbAM9A20DbAM8Az0DOgAKADsAOgAJAAoARANsA20DRANtA0UDlwBWAVcBVgGXAJYAngAcAJ8AngAaABwAMwZFAzQGMwZEA0UDVwGeAJ8AngBXAVYBKwY9AzwDKwY8AyoGiggqBokIKgaKCCsGkQg0BpIIkQgzBjQGlwAJAJYAlwAKAAkABQwnCwQMBQwoCycLKgZ8CIkIfAgqBjIGbAMqBjwDbAMyBioGOgCWAAkAOgBWAZYAMwZ8CDIGMwaRCHwIbAMzBjIGMwZsA0QDOgCeAFYBngA6ABoALwv4C20KLwsVDPgLJwv4CwQM+AsnC20KOQs3CzYLOgs3CzkLmQibCJgImwiZCJwIEwu3CBELtQgRC7cITgNMA0sDTwNMA04DKAO/ACYDvQAmA78ApgCoAKUAqACmAKkAOAs2CzQLOQs2CzgLNAs1CzgLNQs0CzILmAiaCJYImgiYCJsIlgiXCJQIlwiWCJoIEgu2CBALtAgQC7YIDwuzCA4LsggOC7MITQNLA0kDTgNLA00DSQNKA00DSgNJA0cDJwO+ACUDvAAlA74AJAO7ACMDugAjA7sApQCnAKMApwClAKgAowCkAKEApACjAKcAPAs+Cz0LPAs9CzsLOgs8CzsLOgs7CzcLFgu6CBcLuwgXC7oIFAu4CBULuQgVC7gIngifCKAInQicCJkInAidCJ4ITwNRA1ADTwNQA0wDUQNTA1IDUQNSA1ADrACrAKoAqwCsAK0AqgCpAKYAqQCqAKsAKwPCACwDwwAsA8IAKQPAACoDwQAqA8AAbwpFC0QLRQtvCnAKOwq/CBoLOwqDCL8IRAunCKYIpwhEC0ULpwh+CKYIpwh/CH4IbgNaA1kDWgNuA28DtABcAbMAtABdAVwBLwOKAckAigEvA2QDWgOzAFkDWgO0ALMAQAtwCm8KcApAC0ELPgtBC0ALPgtACz0LfwiiCH4IfwijCKIInwieCJ0IogigCJ8IoAiiCKMIOgq+CIIIvgg6CgoLGAu8CBkLvQgZC7wIVQNvA24DbwNVA1YDUwNWA1UDUwNVA1IDXQGvAFwBXQGwAK8ArwCtAKwArQCvALAAMgOJAWMDMgPIAIkBLQPEAC4DxQAuA8QAoQCiAJ8AogChAKQARwNIA0oDSANHA0UDIgO5ACEDuAAhA7kAnwBIA0UDSAOfAKIAlAiVCJIIlQiUCJcIlQgwC5IIlQgzCzALMgszCzULMwsyCzALDQuxCAwLsAgMC7EIGQs+CxgLPgsZC0ELvQigCKMIoAi9CLwILgNTAy0DUwMuA1YDxQCtALAArQDFAMQAXQG0AMkAyQCKAV0BZAMvA1oDWgNvA2QDpwiDCH8IgwinCL8IcAoaC0ULGgtwCjsKFQs6CxQLOgsVCzwLuQicCJ4InAi5CLgIKgNPAykDTwMqA1EDwQCpAKsAqQDBAMAAFws8CxYLPAsXCz4LuwieCKAIngi7CLoILANRAysDUQMsA1MDwwCrAK0AqwDDAMIADgs4CzULmgiyCJcIIwNNA0oDpwC6AKQADAs1CzMLlwiwCJUIIQNKA0gDpAC4AKIAEAs5CxILmwi0CLYIJQNOAycDqAC8AL4AEQs6CxMLnAi1CLcIJgNPAygDqQC9AL8AyABdAYkBXQHIALAAVgNjA28DYwNWAzIDowh/CIIIggi+CKMICgs6CnAKcApBCwoLOwqCCIMIggg7CjoKYwOKAWQDYwOJAYoBEQu4CBQLEQu1CLgIJgPAACkDJgO9AMAAEAu0CLcIEAu3CBMLKAO8AL8AvAAoAyUDDAuzCA8LDAuwCLMIIQO7ACQDIQO4ALsADgu2CBILDguyCLYIIwO+ACcDIwO6AL4AFwu8CBgLFwu7CLwILAPDAMQALAPEAC0DFQu6CBYLFQu5CLoIKgPCACsDKgPBAMIApwgaC78IGgunCEULyQBaAy8DWgPJALQACgu9CL4IvQgKCxkLLgPFAMgALgPIADIDSAO5ACIDSAOiALkAMwuxCA0LMwuVCLEIOwpwCjoKgwiCCH8IZANvA2MDigGJAV0BFAs6CxELuAi1CJwIKQNPAyYDwAC9AKkAEAsTCzkLtAibCLcIJQMoA04DvACoAL8ADws1CwwLswiwCJcIJANKAyEDuwC4AKQADgsSCzgLsgiaCLYIIwMnA00DugCnAL4AGAs+CxcLvAi7CKAILQNTAywDxADDAK0AFQsWCzwLuQieCLoIKgMrA1EDwQCrAMIAGQsKC0ELvQijCL4ILgMyA1YDxQCwAMgAnwg7BqIIPQaiCDsGOwZSAz0GVQM9BlIDrAAjAK8AJwCvACMArgBXA7EAVwOuAFQDPAZXA1QDVwM8Bj8GoQg/BjwGPwahCKQIPwukCKEIpAg/C0ILIQxCCz8LQgshDCMMKQuMCIsIjAgpCyoLiwgtBiwGLQaLCIwILAY/Az4DPwMsBi0GPgOZAJgAmQA+Az8DmAASAAwAEgCYAJkAsgBXA1gDVwOyALEAPwZYA1cDWAM/BkAGpAhABj8GQAakCKUIQgulCKQIpQhCC0MLIwxDC0ILQwsjDCQMQQZuA1kDbgNBBj4GfghBBqYIfgg+BkEGAAxECyUMAAxvCkQLighuCigLbgqKCH0IfQiRCJIIkQh9CHwIBQz4C/kL+AsFDAQMVwE9A5cAVwFtAz0D+QsVDBcMFQz5C/gLQwOgAJ0AoABDA0YDMQZGA0MDRgMxBjUGkAg1BjEGNQaQCJMIkAgxC5MIMQuQCC4LEwwxCy4LMQsTDBgMoABUA64AVAOgAEYDNQZUA0YDVAM1BjwGkwg8BjUGPAaTCKEIkwg/C6EIPwuTCDELGAw/CzELPwsYDCEMmQg5Bp0IOgadCDkGOQZMAzoGUAM6BkwDpgAhAKoAIgCqACEAnQg6Bp8IOwafCDoGOgZQAzsGUgM7BlADqgAiAKwAIwCsACIAlAg2BpYINwaWCDYGNgZHAzcGSQM3BkcDoQAeAKMAHwCjAB4AQAObAJoAmwBAA0EDQQMuBi8GLgZBA0ADjQgvBi4GLwaNCI4IKwuOCI0IjggrCywLEAwsCysLLAsQDBEMmQBAA5oAQAOZAD8DPwMuBkADLgY/Ay0GLQaNCC4GjQgtBowIjAgrC40IKwuMCCoLKgsQDCsLEAwqCw0Mkgg2BpQINgaSCDQGNAZHAzYGRwM0BkUDnwAeAKEAHgCfABwAlgg3BpgIOAaYCDcGNwZJAzgGSwM4BkkDowAfAKUAIAClAB8AQQOcAJsAnABBA0IDLwZCA0EDQgMvBjAGjggwBi8GMAaOCI8ILAuPCI4IjwgsCy0LEQwtCywLLQsRDBIMWQOyAFgDWQOzALIAQQZYA0AGQQZZA1gDpghABqUIQAamCEEGRAulCEMLRAumCKUIJQxDCyQMJQxEC0MLKQuKCCgLKQuLCIoIiwgrBooIiwgsBisGLAY9AysGLAY+Az0DPgOXAD0DPgOYAJcAmAAKAJcAmAAMAAoAmAg4BpkIOQaZCDgGOAZLAzkGTAM5BksDpQAgAKYAIQCmACAAQgOdAJwAnQBCA0MDMAZDA0IDQwMwBjEGjwgxBjAGMQaPCJAILQuQCI8IkAgtCy4LEgwuCy0LLgsSDBMMkghuCn0IkggwC24Kigh8CH0IfAiKCIkILwtuCjALLwttCm4KVwFFA20DRQNXAZ8AGgA7ABwAGgA6ADsAPQZuAz4GbgM9BlUDfgg9Bj4GPQZ+CKIIAAxAC28KQAsADCIMQQAqAEIAKgBBACQArwBCAFwBrwAnAEIAFQwwCxcMFQwvCzAL/wsiDAAMIgz/Cx4MHAwdDP4L/QscDP4L/gseDP8LHgz+Cx0MPwAZAEAAGQA/AA8AQAAkAEEAJABAABkAPAANAD0ADQA8AAsA+gsZDPsLGgz7CxkM+QsXDPoLGQz6CxcMOwALADwACwA7AAoAPQAOAD4ADgA9AA0A+wsaDPwLGwz8CxoMPgAPAD8ADwA+AA4A/AsbDP0LHAz9CxsMEwAPAA4ADwATABQACQz8CwoM/QsKDPwLnAAYABcAGACcAJ0ADwwSDA4MEgwPDBMMGww2CxwMNwscDDYLIAA/ACEAPwAgAD4ACgwODAkMDgwKDA8MFwAUABMAHwwlDCQMJAwgDB8MBwwoCwUMBwwpCygLswApALIAswAqACkAKAAlACYAFAAdABsAHQAUABgAGwAmACUAJgAbAB0AFAwPDAoMDwwUDBYMHwwWDBQMFgwfDCAMCAwLDAYMCwwIDAwMFQARABAAFgARABUAEgAQAAwAFQAQABIABgwHDAUMBwwGDAsMCQwMDAgMDAwJDA4MFwARABYAEQAXABMAEQAOAA0ADgARABMACAz7CwkM/AsJDPsLmwAXABYAFwCbAJwAEQwMDBIMEgwMDA4MGgw0CxsMNgsbDDQLHwA+ACAAPgAfAD0AHAA8AB4APAAcADsAFwwyCxkMMgsXDDALCwwNDAcMDQwLDBAMEgCaABUAmgASAJkABQz5CwYM+gsGDPkLDAALAAoAEAALAAwAEAANAAsAEQANABAABgz6CwgM+wsIDPoLmgAWABUAFgCaAJsADAwQDAsMEAwMDBEMGQwyCxoMNAsaDDILHgA9AB8APQAeADwAIgBAACMAIwBAAEEAIQBAACIAQAAhAD8AHQw7Cx4MPQseDDsLHAw3Cx0MOwsdDDcLIQwWDCAMIQwYDBYMGAwPDBYMDwwYDBMMHQCuACYArgAdAKAAnQAdABgAHQCdAKAAHwwUDP4LHwz+C/8L/Qv+CxQMCgz9CxQMGwAkABkAFAAZAA8AFAAXABgAGQAUABsAJQAoACkAJAAbACUAJAApACoAKQAkACUA/wslDB8MJQz/CwAMbQooC24KbQonCygLswBCACoAQgCzAFwBsQApACgAKQCxALIABwwqCykLKgsHDA0MJgCxACgAsQAmAK4AIAwkDCMMIwwhDCAMHgw9CyIMQAsiDD0LQQBCACcAIwBBACcAMQoRChAKEQoxCjIKLAoQCg8KEAosCjEKMgoSChEKEgoyCjMKTgGfAVMBnwFOAZ4BVAGhAVUBoQFUAaABUwGgAVQBoAFTAZ8BUgEqCkwBUgEwCioKSgElCkcBSgEoCiUKLwozCjIKMgouCi8KJwpIASYKJwpJAUgBUwFQAU4BTQFQAUkBTgFQAU0BJgouCicKJgovCi4KSgFPAUsBTwFKAUcBSgEpCigKKQpKAUsBKwouCiwKLgorCicKKwpOAU0BTgErCiwKSAFPAUcBSAFQAVEBUAFTAVQBUAFIAUkBJgotCi8KLwowCjMKJgpHASUKJgpIAUcBMApSAVUBMApVATMKUQFUAVUBVAFRAVABTwFIAVEBUQFSAU8BUgFRAVUBMAovCi0KKQotCigKLQomCiUKLQolCigKLQoqCjAKKgotCikKKwpJAScKKwpNAUkBLgoyCjEKMQosCi4KSwEqCikKKgpLAUwBTwFSAUwBSwFPAUwB8gT5B/oHfAP5B/IEpQv+BwsKCwr+B/0H8wR9A/IEfAPyBH0DCQr6B/kH+QejCwkK/QfzBPwH/Qf0BPME/AfyBPoH/AfzBPIECgr6BwkKCgr8B/oHCwr8BwoKCwr9B/wHNwAICOUHBwAICDcA9AR9A/MEfQP0BH4DPwwDCEIMAwg/DAQIMQD/By4A/wcxAAAIAAD7B/gHSwwCCAEIpQsBCP4HSwwBCKUL+Qd8A/gHAAD4B3wDNQwKCA8IRgwKCDUMNAAACDEAAAg0AOwHJgwQCAkIKgwQCCYMYQAGCAcIXgAGCGEAMwwNCDQMMwwMCA0IKwwOCBEIDggrDC4MEQhiABAIEQhjAGIANQwLCDIMCwg1DA8ILgwNCA4IDQguDDQMXgAFCAYIVwAFCF4ARgwICAoITgwICEYMVwAECAUIMwAECFcAJgwHCCkMBwgmDAkINADlB+wH5Qc0ADcALgD7BwEA+wcuAP8HQgwDCAIIAghMDEIMMgwLCAwIMgwMCDMMCQqkCwoKpAsJCqMLOAAGAAgABgA4ADkATgw4DDcMOAxODE8M/gf0BP0H/gd+A/QEMAxFDDEMRQwwDEQMQAw+DEEMPgxADD0MMwAvADAALwAzADIAAwAEAAUAAwACAAQASAxJDEcMSQykC6MLSQyjC0cMfgMCAH0DfgMEAAIAWQBbAFoAXQBZAFoAXQAtAFkALABZAC0APgw6DDsMOgw+DD0MNgAyADMAMgA2ADUANAwwDDMMMwwwDDEMCwhaAFsACwhbAAwILQAIACwACAAtAAcATQxFDEQMRQxNDE8MNww7DDoMOww3DDgMOQA1ADYANQA5ADgAMAADAAUAAwAwAC8ASAxBDEoMQAxBDEgMBQACCDAAMAACCAMISgxCDEwMQQxCDEoMSAz7B0AMQAz7B/8HLwABAAMAAQAvAC4ANQA3ADQANwA1ADgA7Ac3DDoMNwzsB+UHOww5DDwMOQw7DDgMNgwvDEMMLww2DCgMJwwmDCkMLAwnDCkMYQAHCDwBPAEHCAkIJQFiAF8AYgAlATwBtgBfAFgAXwC2ACUBLAwoDCcMLwwoDCwMNgxDDDkMVwBSADMAtgBSAFcAOQA2AFIAPww8DC8MBgA5ACsAUgArADkAWABSALYAUgBYACsALwwFCAQIBAg/DC8MOQxNDEQMRAw2DDkMRQxPDEYMRgxPDE4MLAAGACsABgAsAAgACAgtAAoICAgHAC0AKAwwDC0MLQwnDCgMLAwGCAUIBQgvDCwMtgBeACUBtgBXAF4ADQhgAA4IDQhcAGAAXwBcAFgAXABfAGAALQwrDCoMKwwtDC4MLQwwDDQMNAwuDC0MDwhaAAsIWgAPCF0AXwBiAGMAYwBgAF8AKwwRCBAIKwwQCCoMDghjABEIYwAOCGAADAhbAFwADAhcAA0IJQFeAGEAYQA8ASUBKQwHCAYIBggsDCkMMQw1DDIMMQwyDDMMPAEQCGIAEAg8AQkINAAyADUAMQAyADQAAAg6DD0M7Ac6DAAIPww+DDwMOww8DD4MXQAPCAoICggtAF0AWQAsAFgAKwBYACwAWwBYAFwAWABbAFkAJwwtDCoMKgwmDCcMfAMCAAAAAgB8A30D+AejC/kH+AdHDKMLSQylC6QLpQtJDEsMAQgEAH4DAQh+A/4HAghLDEwMAQgFAAQABQABCAIISQxIDEoMSwxKDEwMSgxLDEkM+wcAAAEA+AdIDEcMSAz4B/sHAQACAAMAAQAAAAIALgAyADEAMgAuAC8A/wc9DEAMPQz/BwAIQQw/DEIMPgw/DEEMMAADCDMAMwADCAQIMQxGDDUMRgwxDEUMpAsLCgoKCwqkC6ULKAw2DDAMRAwwDDYMTQw4DE8MTQw5DDgMCAhODDcMCAg3DOUHOAAHADcABwA4AAgAbggMCoIJ4AduCIIJDAqUC6QKggkMCqQKpAqeCwsLngukCpQL6AMwBagGMAXoAyADqAZiCMQIYgioBjAFPAoLC54LCws8CvMJ3wIgA+gDIAPfAsMBxAjzCTwK8wnECGIItQHfAqsCwwHfArUBqwKAA60CtQGrAq0CgAObBSwErQKAAywEmwVuCOAHLASbBeAHxgYrCXoIeggnBsYG5gPGBicGJwanA+YD8gLmA6cDrwLyAqcDrwIcA/IC6gIcA68CgQmLCqELiwqBCfkIHAPsA/ED7AMcA+oCoQupC/ILqQuhC4sKrgf5CIEJ+QiuB94G8QPeBq4H3gbxA+wDpgvyC6kL8gumC9oLUQvaC6YLGgpRC6YLKwlRCxoKeggrCRoKIgSoB0gHSAcEBCIEBATrAiIEBAQaA+sC6wJaAgsCWgLrAhoDWgJAAgsCaQJAAloC5AeICEkJiAjkB8oHXANAAmkCaQJiA1wDSQl0CfwJdAlJCYgI0QTKB+QHygfRBIsEXAOLBNEEiwRcA2IDbgn8CXQJ/AluCfUJ6wj1CW4JdwjrCG4JbgmICHcIiAhuCXQJdwjKB0gHdwiICMoHGgNpAloCYgNpAhoDYgMEBIsEBARiAxoDiwRIB8oHSAeLBAQEsQkxCdsHMQmxCW4L7gMbA1sDQAcjBNsHQAfbBzEJIwTuA1sDIwRAB+4DbguxCagL6wuoC/cLqAvrC24LIwSuB9sHrgcjBPEDWwPxAyME8QNbAxwDqAfrCHcIdwhIB6gHWwPyAhwD8gJbAxsD5gMbA+4DGwPmA/IC7gPGBuYDxgbuA0AHQAcrCcYGKwlABzEJMQlRCysJUQsxCW4LbgvaC1EL2gtuC+sL2gvrC/cL2gv3C/ILqAvyC/cL8guoC6ELsQmhC6gLoQuxCYEJ2weBCbEJgQnbB64H6AvBC8ALwQvoC+kL4wvAC78LwAvjC+gL6QvCC8ELwgvpC+oLSgBUAE8AVABKAFMAVgBQAFUAUABWAFEATwBVAFAAVQBPAFQATgDhC0gATgDnC+ELRgDcC0MARgDfC9wL5gvqC+kL6QvlC+YLTABPAFAATwBMAEoASQBMAEUASgBMAEkARgBLAEcARgDgC98L4AtGAEcA4gvlC+ML5QviC94L5AvcC98L4AvkC98LRABMAE0ARABLAEMATABEAEUA3QvmC+UL3QvlC94L3QvkC+YL5gvnC+oL5wtOAFEA5wtRAOoLTQBQAFEAUABNAEwASwBGAEMASwBEAE0ATQBOAEsATgBNAFEA5wvmC+QL5AvdC9wL5AvhC+cL4QvkC+AL5QvpC+gL6AvjC+ULRwDhC+AL4QtHAEgASwBOAEgARwBLAEgA","c":"AwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIC","pal":["222255","333344","335555","33ccff","555555"]},"grenade":{"s":2.5940733054597613e-06,"p":"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","i":"RQIrAkQCKwJFAiwCpQDLAPoAywClAJIAOgNAAx0DQAM6A18DGgAEAA8ABAAaAAYA/AIDA+cC5wLYAvwCRALmAdcB5gFEAisCDQHrADIBMgFiAQ0BXQNyA2kDaQM4A10DGAAdAHwAfABnABgANQMkAx4DHgM9AzUD0QCAASIBgAHRAF4BrgEsAkUCLAKuAdsB+QKVAqYClQL5ArQCywANAWIBYgH6AMsAHQNdAzgDXQMdA0ADDwAYAGcAGAAPAAQAAwM1Az0DPQPnAgMD1wHbAa4B2wHXAeYBdQElARUBJQF1AWwBXAM/AxwDHAM3A1wDbgAIABUACABuAB8ASQMJA+sC6wJFA0kDiAKoArwCvAJyAogCFQGDAZ8BgwEVASUBHANeAzkDXgMcAz8DFQATAD4AEwAVAAgACQMLA/EC8QLrAgkDqAK2AssCywK8AqgCkgFsAXUBdQHPAZIBcQNcAzcDNwNoA3EDOQAfAG4AbgCsADkATQNJA0UDRQNPA00DcwKIAnICiAJzAokCgwGSAc8BzwGfAYMBXgNxA2gDaAM5A14DPgA5AKwAOQA+ABMA8QJNA08DTQPxAgsDywKJAnMCiQLLArYC8AHnAcoB5wHwAQAC+gLzAscCxwLeAvoCLgBsANoA2gCAAC4AQwMHA+8C7wJLA0MDkAK+AtEC0QJqApAC5wEzAj4CPgLKAecBxwIWAxADFgPHAvMC2gA8AI0APADaAGwABwMFA+IC4gLvAgcDvgLAAtUC1QLRAr4CNAIAAvAB8AE/AjQCIgP6At4C3gIqAyIDIgAuAIAALgAiABIAMQNDA0sDSwMuAzEDawKQAmoCkAJrApECMwI0Aj8CPwI+AjMCFgMiAyoDKgMQAxYDjQASACIAEgCNADwA4gIxAy4DMQPiAgUD1QKRAmsCkQLVAsACfQJ8AngCeAJ5An0CggKDAnUCdQJ0AoICBwAeAH0AfQAbAAcApwKHAnECcQK7AqcCqAHxAeQB8QGoAcsBZALLAagBywFkAkECYgJBAmQCQQJiAkAC5AFAAmICQALkAfEBIQMBA/kC+QInAyEDSAMhAycDJwNXA0gDDwNIA1cDVwMNAw8DAQMPAw0DDQP5AgEDRwMOAw0DDQNXA0cDIANHA1cDVwMmAyADAAMgAyYDJgP4AgADDgMAA/gC+AINAw4DGAP2AvgC+AImAxgDzgIYAyYDJgPFAs4CrQLOAsUCxQKlAq0C9gKtAqUCpQL4AvYCrwKdAqUCpQLFAq8CeAKvAsUCxQJXAngCeQJ4AlcCVwJZAnkCnQJ5AlkCWQKlAp0CPgI/AlkCWQJXAj4CygE+AlcCVwKnAcoB8AHKAacBpwHjAfABPwLwAeMB4wFZAj8CnwHPAeMB4wGnAZ8B7gCfAacBnwHuABUBcwEVAe4AFQFzAXUB4wF1AXMBdQHjAc8B7gBiAXMBYgHuAPoAgQD6AO4A+gCBAKUAPgGlAIEApQA+ATIBcwEyAT4BMgFzAWIBgQAzAT4BMwGBAKYA7wCmAIEApgDvAPsAdAH7AO8A+wB0AWMBPgFjAXQBYwE+ATMB7wB2AXQBdgHvABYBqAEWAe8AFgGoAaAB5AGgAagBoAHkAdABdAHQAeQB0AF0AXYBZAJ6AmICegJkAnsCxgJ7AmQCewLGArACpgKwAsYCsAKmAp4CYgKeAqYCngJiAnoCxgKuAqYCrgLGAs8CJwPPAsYCzwInAxkD9wIZAycDJwP5AvcCrgL3AvkC+QKmAq4CIgHlAL0A5QAiATABuwEwASIBMAG7AaYBlwGmAbsBpgGXAY8BvQCPAZcBjwG9AOUA0QBfAD0AXwDRAMUAIgHFANEAxQAiAQoBvQAKASIBCgG9AMcAPQDHAL0AxwA9AF8AIQHGALwAxgAhAQkB0QAJASEBCQHRAMQAPQDEANEAxAA9AF4AvABeAD0AXgC8AMYApQGOAZYBlgG6AaUBIQGlAboBpQEhAS8BvAAvASEBLwG8AOQAjgHkALwAvACWAY4BQwJCAlYCVgJYAkMC1gFDAlgCWAK6AdYBrQHWAboBugGWAa0BQgKtAZYBlgFWAkICqwLBAtYC1gK3AqsCdQKrArcCtwJYAnUCdAJ1AlgCWAJWAnQCwQJ0AlYCVgLWAsECEAMqAzsDOwMUAxADxwIQAxQDFAO3AscC3gLHArcCtwLWAt4CKgPeAtYC1gI7AyoDOQNoA2wDbAMwAzkDHAM5AzADMAMUAxwDNwMcAxQDFAM7AzcDaAM3AzsDOwNsA2gDHQM4AzwDPAMVAx0DOgMdAxUDFQMwAzoDaQM6AzADMANsA2kDOANpA2wDbAM8AzgDuALfAtcC3wK4AsgCFQPIArgCyAIVAxEDKwMRAxUDFQM8AysD1wIrAzwDKwPXAt8CYwJ3AmUCdwJjAnYCuAJ2AmMCdgK4AqwC1wKsArgCrALXAsICZQLCAtcCwgJlAncCZQKqAZcBqgFlAkwCZwJMAmUCTAJnAk0CiQFNAmcCTQKJAZwBlwGcAYkBnAGXAaoBZwKaAYkBmgFnAlECZgJRAmcCUQJmAlACjQFQAmYCUAKNAZ4BiQGeAY0BngGJAZoBZgKjAY0BowFmAkkCZAJJAmYCSQJkAkgCqAFIAmQCSAKoAbcBjQG3AagBtwGNAaMBlwHVAL0A1QCXAYsBiQGLAZcBiwGJAX4BdAB+AYkBfgF0AJ4AvQCeAHQAngC9ANUAiQGXAHQAlwCJAXsBjQF7AYkBewGNAYIBiQCCAY0BggGJAKgAdACoAIkAqAB0AJcAjQG3AIkAtwCNAYYBqAGGAY0BhgGoAZUB7wCVAagBlQHvAP8AiQD/AO8A/wCJALcAvQA/AD0APwC9AK0AdACtAL0ArQB0AG8ADABvAHQAbwAMABYAPQAWAAwAFgA9AD8AdAAQAAwAEAB0AGgAiQBoAHQAaACJAH0AFwB9AIkAfQAXABsADAAbABcAGwAMABAAiQAiABcAIgCJAI0A7wCNAIkAjQDvANoAgQDaAO8A2gCBAIAAFwCAAIEAgAAXACIAPQCsALwArAA9AD4ADAA+AD0APgAMABUAcwAVAAwAFQBzAG4AvABuAHMAbgC8AKwADABnAHMAZwAMAA8AFwAPAAwADwAXABoAhwAaABcAGgCHAHwAcwB8AIcAfABzAGcAFwCMAIcAjAAXACEAgQAhABcAIQCBAH4A7gB+AIEAfgDuANkAhwDZAO4A2QCHAIwA1ACKAZYBlgG8ANQAcwDUALwA1ABzAJ0AiAGdAHMAnQCIAX0BigF9AYgBiAGWAYoBcwB6AYgBegFzAJYAhwCWAHMAlgCHAKcAgQGnAIcAhwCMAYEBegGBAYwBjAGIAXoBtgCFAYwBjAGHALYA7gC2AIcAtgDuAP4ApwH+AO4A/gCnAZQBhQGUAacBpwGMAYUBqQFLAlYCVgKWAakBmwGpAZYBlgGIAZsBSgKbAYgBiAFUAkoCSwJKAlQCVAJWAksCmQFOAlQCVAKIAZkBnQGZAYgBiAGMAZ0BTwKdAYwBjAFVAk8CTgJPAlUCVQJUAk4CogFGAlUCVQKMAaIBtgGiAYwBjAGnAbYBRwK2AacBpwFXAkcCRgJHAlcCVwJVAkYCbQLDAtYC1gJWAm0CbAJtAlYCVgJUAmwC0gJsAlQCVALkAtICwwLSAuQC5ALWAsMCaALUAuQC5AJUAmgCaQJoAlQCVAJVAmkC0AJpAlUCVQLgAtAC1ALQAuAC4ALkAtQCcALKAuAC4AJVAnACcQJwAlUCVQJXAnECuwJxAlcCVwLFArsCygK7AsUCxQLgAsoC4gIuAzsDOwPWAuIC7wLiAtYC1gLkAu8CSwPvAuQC5AJgA0sDLgNLA2ADYAM7Ay4D8QJPA2ADYAPkAvEC6wLxAuQC5ALgAusCRQPrAuAC4AJVA0UDTwNFA1UDVQNgA08D5wI9A1UDVQPgAucC2ALnAuAC4ALFAtgCHgPYAsUCxQImAx4DPQMeAyYDJgNVAz0DQQNqA2wDbAM7A0EDYgNBAzsDOwNgA2IDgANiA2ADYAOIA4ADagOAA4gDiANsA2oDZgOGA4gDiANgA2YDWgNmA2ADYANVA1oDewNaA1UDVQN/A3sDhgN7A38DfwOIA4YDUwN1A38DfwNVA1MDLANTA1UDVQMmAywDWAMsAyYDJgNXA1gDdQNYA1cDVwN/A3UDawNCAzwDPANsA2sDgQNrA2wDbAOIA4EDYwOBA4gDiANhA2MDQgNjA2EDYQM8A0IDhwNnA2EDYQOIA4cDfAOHA4gDiAN/A3wDWwN8A38DfwNWA1sDZwNbA1YDVgNhA2cDdgNUA1YDVgN/A3YDWQN2A38DfwNXA1kDLQNZA1cDVwMnAy0DVAMtAycDJwNWA1QDPAPjAtcC4wI8Ay8DTAMvAzwDPANhA0wD8AJMA2EDYQPlAvAC1wLwAuUC8ALXAuMCUAPyAuUC5QJhA1ADRgNQA2EDYQNWA0YD4QJGA1YDRgPhAuwC5QLsAuEC7ALlAvICVgPoAuEC6AJWAz4DHwM+A1YDVgMnAx8DxgIfAycDHwPGAtkC4QLZAsYC2QLhAugC1wJuAmUCbgLXAsQC5QLEAtcCxALlAtMCZwLTAuUC0wJnAm8CZQJvAmcCbwJlAm4C5QJrAmcCawLlAtUC4QLVAuUC1QLhAtECZgLRAuEC0QJmAmoCZwJqAmYCagJnAmsC4QJzAmYCcwLhAssCxgLLAuECywLGArwCZAK8AsYCvAJkAnICZgJyAmQCcgJmAnMCuwGuAZcBrgG7AdcBYwLXAbsB1wFjAkQCZQJEAmMCRAJlAkUClwFFAmUCRQKXAa4B6AE1AgECNQLoATYCUgMpAxsDGwM0A1IDMwMaAygDKANRAzMD6QLaAv4C/gISA+kCfAJ9ApkCmQKhAnwC5wEAAjQCNAIzAucBgwFsAZIBbAGDASUBywDrAA0B6wDLAJIAkwAOAewADgGTAMwAJgGTAW0BkwEmAYQBfwKaAn4CmgJ/AqIC6gL/AtsC/wLqAhMDGwFxAfYAcQEbAXkBeQB7ADgAewB5ALMAsgA2AHoANgCyAHgAGQH1AHABcAF4ARkB5QHaASkCKQIqAuUBgwKCAqkCqQKjAoMC8wL6AiIDIgMWA/MCPwNcA3EDcQNeAz8DXwNyA10DXQNAA18DFwMjA/sC+wL0AhcDhAKqAoUCqgKEAqQCIgK5AckBuQEiAiMCHwK1AbIBtQEfAh4CJwLdAcMB3QEnAiYCWgGqAM8AqgBaAUkBQwGkAJsApABDAUcBXAHyAMoA8gBcAW8BOgAJABQACQA6ACAAGQAHAAUABwAZAB4APAAuABIALgA8AGwAEwAfADkAHwATAAgABAAdABgAHQAEAAYAEQBrADsAawARAC0AzgBIAVkBSAHOAKkAmgBGAUIBRgGaAKMAyQBuAVsBbgHJAPEAuAEgAiECIQLIAbgBtAEdAhwCHAKxAbQB3AElAiQCJALCAdwBigK5ArECsQKLAooCjwK9Ar8CvwKOAo8ChwKnArUCtQKGAocCBwNDAzEDMQMFAwcDCQNJA00DTQMLAwkD/AIkAzUDNQMDA/wCdwOJA4IDggNvA3cDeQOLA40DjQN9A3kDZANzA4QDhANtA2QDigN4A3ADcAODA4oDjAN6A34DfgOOA4wDdANlA24DbgOFA3QDRAMIAwYDBgMyA0QDTgMKAwwDCgNOA0oDNgP9AgQD/QI2AyUDsgKNAowCjQKyAroCwAKQApECkALAAr4CtgKIAokCiAK2AqgC5gEsAtsBLALmASsCJAP8AtgC2AIeAyQDHQAGABoAGgB8AB0AcgNfAzoDOgNpA3IDMgGSAKUAkgAyAesAQQOCA2oDggNBA28DIQA7AIwAOwAhABEAyAL7At8C+wLIAvQCpgDsADMB7ACmAJMAggOJA4ADgANqA4IDOwBrANkA2QCMADsA+wIjAysDKwPfAvsC7AAOAWMBYwEzAewAdwNvA0EDQQNiA3cDfgARACEAEQB+AC0AEQP0AsgC9AIRAxcD+wCTAKYAkwD7AMwAiQN3A2IDYgOAA4kDawAtAH4AfgDZAGsAIwMXAxEDEQMrAyMDYwHMAPsAzABjAQ4BZgONA4YDjQNmA30D1ABZAYoBWQHUAM4AdgKFAncChQJ2AoQCFgFtAXYBbQEWASYBjQOLA3sDewOGA40DWQFIAX0BfQGKAVkBdwKqAsICqgJ3AoUCbQGTAdAB0AF2AW0BeQN9A2YDZgNaA3kDnQDOANQAzgCdAKkArAKEAnYChAKsAqQCoAEmARYBJgGgAYQBiwN5A1oDWgN7A4sDfQGpAJ0AqQB9AUgBqgKkAqwCrALCAqoC0AGEAaABhAHQAZMBbQOEA3UDdQNTA20DlgBCAXoBQgGWAJoATALJAaoByQFMAiICewJ+AnoCfgJ7An8ChANzA1gDWAN1A4QDQgFGAYEBgQF6AUIBqgG5AZwBuQGqAckBfgKaAp4CngJ6An4CZANtA1MDUwMsA2QDpwCaAJYAmgCnAKMATQIiAkwCIgJNAiMCsAJ/AnsCfwKwAqICcwNkAywDLANYA3MDRgGjAKcApwCBAUYBnAEjAk0CIwKcAbkBmgKiArACsAKeApoCawNwA0IDcANrA4MDtgBbAYUBWwG2AMkAUQKyAZoBsgFRAh8CzwLbAq4C2wLPAuoCQgN4A2MDeANCA3ADWwFuAZQBlAGFAVsBmgG1AZ4BtQGaAbIB2wL/AvcC9wKuAtsCigODA2sDawOBA4oD/gDJALYAyQD+APEAUAIfAlECHwJQAh4CGQPqAs8C6gIZAxMDeAOKA4EDgQNjA3gDbgHxAP4A/gCUAW4BtQEeAlACUAKeAbUB/wITAxkDGQP3Av8ChwN+A2cDfgOHA44DqQEhAksCIQKpAcgBSQLDAaMBwwFJAicCMAH2AOUA9gAwARsBZwN6A1sDegNnA34DIQIgAkoCSgJLAiECowHdAbcB3QGjAcMB5QBxAY8BcQHlAPYAjAOOA4cDhwN8A4wDmwHIAakByAGbAbgBSAInAkkCJwJIAiYCpgEbATABGwGmAXkBegOMA3wDfANbA3oDIAK4AZsBmwFKAiAC3QEmAkgCSAK3Ad0BjwF5AaYBeQGPAXEBhQNuA1QDVAN2A4UDmQEcAk4CHAKZAbEBiwHPANUAzwCLAVoBxQA4AF8AOADFAHkAywEBAvEBAQLLAegBVANlAy0DZQNUA24DHAIdAk8CTwJOAhwC1QCqAJ4AqgDVAM8AXwB7AMcAewBfADgAAQI1AkACQALxAQECdAOFA3YDdgNZA3QDnQGxAZkBsQGdAbQBfgFaAYsBWgF+AUkBCgF5AMUAeQAKAbMAQQLoAcsB6AFBAjYCZQN0A1kDWQMtA2UDHQK0AZ0BnQFPAh0CngBJAX4BSQGeAKoAxwCzAAoBswDHAHsAQAI2AkECNgJAAjUCLwMGA+MCBgMvAzIDwgEkAkYCRgKiAcIBewGbAJcAmwB7AUMBsgB6AMYAxgAJAbIAIQMbAwEDGwMhAzQD4wIIA/ACCAPjAgYDJAIlAkcCRwJGAiQClwCkAKgApACXAJsAxgA2AF4ANgDGAHoAGwMpAw8DDwMBAxsDRAMyAy8DLwNMA0QDtgHCAaIBwgG2AdwBggFDAXsBQwGCAUcBxACyAAkBsgDEAHgAUgM0AyEDIQNIA1IDCANEA0wDTAPwAggDJQLcAbYBtgFHAiUCpABHAYIBggGoAKQAXgB4AMQAeABeADYAKQNSA0gDSAMPAykDUAMMA/ICDANQA04DbQKxAsMCsQJtAosChgHKALcAygCGAVwBeAFwAY4BjgGlAXgBUQMoAw4DDgNHA1ED8gIKA+wCCgPyAgwDsQK5AtIC0gLDArECtwDyAP8A8gC3AMoAjgH1AOQA9QCOAXABKAMaAwADAAMOAygDSgNOA1ADUANGA0oDigKLAm0CbQJsAooClQFcAYYBXAGVAW8BLwF4AaUBeAEvARkBMwNRA0cDRwMgAzMDCgNKA0YDRgPsAgoDuQKKAmwCbALSArkC8gBvAZUBlQH/APIA5AAZAS8BGQHkAPUAGgMzAyADIAMAAxoDPgMEA+gCBAM+AzYDaAK/AtQCvwJoAo4CrQAUAD8AFACtADoAKgIpAkICQgJDAioCEgP+AvYC9gIYAxID6AL9AtkC/QLoAgQDvwK9AtAC0ALUAr8CPwAJABYACQA/ABQAKQLaAa0BrQFCAikC/gLaAq0CrQL2Av4CJQM2Az4DPgMfAyUDjwKOAmgCaAJpAo8CbwA6AK0AOgBvACAA1gEqAkMCKgLWAeUB6QISAxgDGAPOAukC/QIlAx8DHwPZAv0CvQKPAmkCaQLQAr0CFgAgAG8AIAAWAAkArQHlAdYB5QGtAdoB2gLpAs4CzgKtAtoCxAKMAm4CjALEArIChgK1AsoCygJwAoYCaAAFABAABQBoABkAowKpAsECwQKrAqMCoQKZAp0CnQKvAqECbgKNAm8CjQJuAowCtQKnArsCuwLKArUCEAAHABsABwAQAAUAqQKCAnQCdALBAqkCmQJ9AnkCeQKdApkCugKyAsQCxALTAroChwKGAnACcAJxAocCfQAZAGgAGQB9AB4AdQKjAqsCowJ1AoMCfAKhAq8CrwJ4AnwCbwK6AtMCugJvAo0CuALuAhUD7gK4AqACFAOfArcCnwIUA+0CWALrAboB6wFYAloCIQFeAdEAXgEhAX8BMAPtAhQD7QIwAwIDtwJaAlgCWgK3Ap8CFQMCAzADAgMVA+4CugF/ASEBfwG6AesBuwFhAmMCYQK7AewBgAEKAuwBCgKAAZABDQL4AJgB+AANAncBYwKgArgCoAJjAmECxAGkAcUBxQEMAmACYAKVArQCtALJArMCswKUAl0CXQILAsQBxAHFAWACYAK0ArMCswJdAsQBxAFgArMCPgHEAXMBxAE+AaQBswLJAg0DDQP4ArMC5AHFAXQBxQHkAQwCXQKUAqUCpQJZAl0CpgJgAmICYAKmApUCxAELAuMB4wFzAcQByQK0AvkC+QINA8kCdAGkAT4BpAF0AcUBlAKzAvgC+AKlApQCYgIMAuQBDAJiAmACCwJdAlkCWQLjAQsCAwCQAHIBcgHcAAMAaQHTAQoCCgKQAWkBwAFYAdwA3AByAcABVQHcAFgB3ACYAfgAmAFpAZABUQHcAFUBmAFrAWkBSwHcAFEBmAFqAWsBSgHcAEsBmAFkAWoBUAHcAEoBmAFfAWQBUAGYAdwAmAFUAV8BVAGYAVAB1QEKAtMBCgJ3AQ0CcgG8AcAB1AEKAtUBcgGwAbwB0QEKAtQBdwGwAXIBwQEKAtEBdwGsAbABswEKAsEBdwGrAawBswF3AQoCdwGvAasBrwF3AbMBkAAOAKAADgCQAAsAcgGgAHcBoAByAZAA+AADANwAAwD4AAoAdwEKAPgACgB3AaAAAAAOAAsADgAAAAEACgAAAAMAAAAKAAEAoAABAAoAAQCgAA4AAAANAAMADQAAAAIAIgHsAbsB7AEiAYAB3AC7AXIBuwHcACIBAgAcALkAuQANAAIAHAACAAAAAAALABwAAwC5AJAAuQADAA0AuQAcAAsACwCQALkAwAFVAVgBVQHAAbwBVQGwAVEBsAFVAbwBUQGwAawBrAFLAVEBSwGsAasBqwFKAUsBqwGvAVABUAFKAasBswFUAVABUAGvAbMBwQFfAVQBVAGzAcEB0QFkAV8BXwHBAdEB0QFqAWQBagHRAdQB1AFrAWoBawHUAdUBaQHVAdMB1QFpAWsBDwJfAjICXwKTAoECkwKSApYCkgJeAoACXgIOAjECDgIPAgkCDwKTAl8CkwJeApICXgIPAg4CDwJeApMCYQKcAqACnAJhAlMCUwKYApwCmAJTAlICUgKBApgCgQJSAl8CoALdAu4C3QKgApwCmALNAt0C3QKcApgCmAKTAs0CkwKYAoEC3QL1AgIDAgPuAt0CzQLmAvUC9QLdAs0CzQKWAuYClgLNApMC9QLcAu0C7QICA/UC5gLMAtwC3AL1AuYC5gKSAswCkgLmApYC3AKbAp8CnwLtAtwCzAKXApsCmwLcAswCzAKAApcCgALMApICmwJbAloCWgKfApsClwJcAlsCWwKbApcClwJeAlwCXgKXAoACWwL/AesB6wFaAlsCXAIIAv8B/wFbAlwCXAIxAggCMQJcAl4C6wGRAX8BkQHrAf8B/wGhAZEBoQH/AQgCCAIOAqEBDgIIAjECfwF8AV4BfAF/AZEBkQGHAXwBhwGRAaEBoQEJAocBCQKhAQ4CXgGQAYABkAFeAXwBfAGYAZABmAF8AYcBhwEPApgBDwKHAQkCmAEyAg0CMgKYAQ8C7AFTAmECUwLsAQoCCgJSAlMCUgIKAg0CDQJfAlICXwINAjICvgHGAcABcgG7AewB7AEKAtMB0gHHAc4BvQG+AcAB0gHOAdgBvQHAAXIB0wHSAdgBvwG9AXIB0wHYAd4BzQG/AXIB0wHeAeAB2QHNAXIB7AHTAeAB3wHZAXIB7AHgAeIB3wFyAewB7AHiAeEB4QHfAewBzAHOAccBxwHAAcYBxgHMAccBVwFYAWABYAFdAVcBYQFdAWABZgFpAZABkAGAASIBIgHcAFgBWAFXAVMBZwFhAWABYAFmAZABWAFTAU8BZwFgAZABIgFYAU8BaAFnAZABIgFPAUwBZQFoAZABIgFMAU0BZQGQASIBIgFNAU4BVgFlASIBIgFOAVIBUgFWASIBAgI4AjoCOgIEAgICBAI6AjwCPAIGAgQCBgI8Aj0CPQIHAgYCBwI9AjsCOwIFAgcCAwI7AjkCOwIDAgUC/gE5AjcCOQL+AQMC+QE3Ai8CNwL5Af4B9QEvAi0CLwL1AfkB9AEtAigCLQL0AfUB9wEoAi4CKAL3AfQB9wEuAjACMAL8AfcB/AEwAjgCOAICAvwBQAECAgQCBAJEAUABRAEEAgYCBgJFAUQBRQEGAgcCBwJBAUUBQQEHAgUCBQI/AUEBPAEFAgMCBQI8AT8BLgEDAv4BAwIuATwBKwH+AfkB/gErAS4BKQH5AfUB+QEpASsBLQH1AfQB9QEtASkBNwH0AfcB9AE3AS0BNwH3AfwB/AE9ATcBPQH8AQICAgJAAT0BMQFAAUQBRAE6ATEBOgFEAUUBRQE7AToBOwFFAUEBQQE1ATsBNQFBAT8BPwEoATUBKAE/ATwBPAEgASgBIAE8AS4BLgEYASABFAEuASsBLgEUARgBEwErASkBKwETARQBFwEpAS0BKQEXARMBHgEtATcBLQEeARcBJwE3AT0BNwEnAR4BMQE9AUABPQExAScBDwExAToBOgERAQ8BEQE6ATsBOwESAREBEgE7ATUBNQEQARIBDAE1ASgBNQEMARABCAEoASABKAEIAQwBBgEgARgBIAEGAQgBAwEYARQBGAEDAQYBAQEUARMBFAEBAQMBBQETARcBEwEFAQEBBQEXAR4BHgEHAQUBBwEeAScBJwELAQcBCwEnATEBMQEPAQsB4wAPAREBEQHqAOMA6gARARIBEgH0AOoA9AASARABEAHwAPQA8AAQAQwBDAHpAPAA4gAMAQgBDAHiAOkA4AAIAQYBCAHgAOIA3gAGAQMBBgHeAOAA2wADAQEBAwHbAN4A3QABAQUBAQHdANsA3wAFAQcBBQHfAN0A3wAHAQsBCwHhAN8A4QALAQ8BDwHjAOEAtADjAOoA6gC4ALQAuADqAPQA9ADAALgAwAD0APAA8ADCAMAAwgDwAOkA6QDBAMIAugDpAOIA6QC6AMEAtQDiAOAA4gC1ALoAsQDgAN4A4ACxALUArwDeANsA3gCvALEAqwDbAN0A2wCrAK8ArgDdAN8A3QCuAKsArgDfAOEA4QCwAK4AsADhAOMA4wC0ALAAfwC0ALgAuACDAH8AgwC4AMAAwACIAIMAiADAAMIAwgCPAIgAkQDCAMEAwgCRAI8AjgDBALoAwQCOAJEAhAC6ALUAugCEAI4AggC1ALEAtQCCAIQAdwCxAK8AsQB3AIIAcgCvAKsArwByAHcAcgCrAK4ArgBwAHIAcACuALAAsAB2AHAAdgCwALQAtAB/AHYATwB/AIMAgwBVAE8AVQCDAIgAiABcAFUAXACIAI8AjwBiAFwAYgCPAJEAkQBmAGIAZgCRAI4AjgBkAGYAZACOAIQAhABhAGQAWwCEAIIAhABbAGEAUwCCAHcAggBTAFsATgB3AHIAdwBOAFMARgByAHAAcgBGAE4ARwBwAHYAcABHAEYATwB2AH8AdgBPAEcAKQBPAFUAVQAxACkAMQBVAFwAXABCADEAQgBcAGIAYgBLAEIASwBiAGYAZgBQAEsAUABmAGQAZABRAFAAUQBkAGEAYQBNAFEAQwBhAFsAYQBDAE0AMwBbAFMAWwAzAEMAKgBTAE4AUwAqADMAKABOAEYATgAoACoAJwBGAEcARgAnACgAKQBHAE8ARwApACcAJQApADEAMQArACUAKwAxAEIAQgA1ACsANQBCAEsASwBEADUARABLAFAAUABIAEQASABQAFEAUQBJAEgASQBRAE0ATQBFAEkANwBNAEMATQA3AEUALABDADMAQwAsADcAJgAzACoAMwAmACwAJAAqACgAKgAkACYAIwAoACcAKAAjACQAJQAnACkAJwAlACMANAAlACsAKwBBADQAQQArADUANQBMAEEATAA1AEQARABUAEwAVABEAEgASABXAFQAVwBIAEkASQBWAFcAVgBJAEUARQBSAFYASgBFADcARQBKAFIAQAA3ACwANwBAAEoAMgAsACYALAAyAEAALwAmACQAJgAvADIAMAAkACMAJAAwAC8ANAAjACUAIwA0ADAAYwA0AEEANABjAF0AaQBBAEwAQQBpAGMAbQBMAFQATABtAGkAbQBUAFcAVwB1AG0AdQBXAFYAVgBxAHUAcQBWAFIAUgBqAHEAagBSAEoASgBlAGoAZQBKAEAAQABgAGUAYABAADIAMgBaAGAAWAAyAC8AMgBYAFoAWQAvADAALwBZAFgAXQAwADQAMABdAFkAmABdAGMAXQCYAJQAnABjAGkAYwCcAJgAnABpAG0AbQChAJwAoQBtAHUAdQCiAKEAogB1AHEAcQCfAKIAnwBxAGoAagCZAJ8AmQBqAGUAZQCVAJkAlQBlAGAAYACLAJUAhgBgAFoAYACGAIsAhQBaAFgAWgCFAIYAigBYAFkAWACKAIUAlABZAF0AWQCUAIoA0wCUAJgAlADTANAA1wCYAJwAmADXANMA1wCcAKEAoQDYANcA2AChAKIAogDWANgA1gCiAJ8AnwDSANYA0gCfAJkAmQDNANIAzQCZAJUAlQDDAM0AwwCVAIsAiwC+AMMAuwCLAIYAiwC7AL4AvwCGAIUAhgC/ALsAyACFAIoAhQDIAL8A0ACKAJQAigDQAMgAAAHQANMA0AAAAfwABAHTANcA0wAEAQABAgHXANgA1wACAQQBAgHYANYA1gD9AAIB/QDWANIA0gD5AP0A+QDSAM0AzQDzAPkA8wDNAMMAwwDoAPMA6ADDAL4AvgDmAOgA5gC+ALsAuwDnAOYA7QC7AL8AuwDtAOcA9wC/AMgAvwD3AO0A/ADIANAAyAD8APcAKgH8AAABAAE0ASoBNAEAAQQBBAE4ATQBOAEEAQIBAgE5ATgBOQECAf0A/QA2ATkBNgH9APkA+QAsATYBLAH5APMA8wAkASwBHwHzAOgA8wAfASQBHAHoAOYA6AAcAR8BGgHmAOcA5gAaARwBHQHnAO0A5wAdARoBIwHtAPcA7QAjAR0BKgH3APwA9wAqASMB8wEqATQBKgHzAe8B+AE0ATgBNAH4AfMB+AE4ATkBOQH7AfgB+wE5ATYBNgH9AfsB/QE2ASwBLAH6Af0B+gEsASQBJAH2AfoB9gEkAR8BHwHyAfYB8gEfARwBHAHuAfIB6gEcARoBHAHqAe4B6QEaAR0BGgHpAeoB7QEdASMBHQHtAekB7wEjASoBIwHvAe0BFQLvAfMB7wEVAhMCFwLzAfgB8wEXAhUCFwL4AfsB+wEZAhcCGQL7Af0B/QEbAhkCGwL9AfoB+gEaAhsCGgL6AfYB9gEYAhoCGAL2AfIB8gEWAhgCFgLyAe4B7gEUAhYCEgLuAeoB7gESAhQCEALqAekB6gEQAhICEQLpAe0B6QERAhACEwLtAe8B7QETAhEC","c":"AQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAw==","pal":["334433","445555","666655","ffffff"]}};
const GLBC={};
function glbModel(id){
  const d=GLBW[id];if(!d)return null;
  let geo=GLBC[id];
  if(!geo){
    const b64=function(t,T){const bin=atob(t),u=new Uint8Array(bin.length);for(let i=0;i<bin.length;i++)u[i]=bin.charCodeAt(i);return new T(u.buffer);};
    const P=b64(d.p,Int16Array),I=b64(d.i,Uint16Array),C=b64(d.c,Uint8Array),n=I.length/3;
    const pal=d.pal.map(function(h){return[parseInt(h.substr(0,2),16)/255,parseInt(h.substr(2,2),16)/255,parseInt(h.substr(4,2),16)/255];});
    const pos=new Float32Array(n*9),col=new Float32Array(n*9);
    for(let t=0;t<n;t++){const c=pal[C[t]]||pal[0];for(let k=0;k<3;k++){const v=I[t*3+k];for(let a=0;a<3;a++){pos[t*9+k*3+a]=P[v*3+a]*d.s;col[t*9+k*3+a]=c[a];}}}
    geo=new THREE.BufferGeometry();geo.setAttribute('position',new THREE.BufferAttribute(pos,3));geo.setAttribute('color',new THREE.BufferAttribute(col,3));geo.computeVertexNormals();
    GLBC[id]=geo;
  }
  if(!GLBC.mat)GLBC.mat=new THREE.MeshPhongMaterial({vertexColors:true,flatShading:true,shininess:25,specular:0x333333});
  const g=new THREE.Group();g.add(new THREE.Mesh(geo,GLBC.mat));return g;
}
function gunModel(id){
  const gl=glbModel(id);if(gl)return gl;
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
  }else if(MELEE.indexOf(id)>=0){
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
function hurtPerson(p,dmg,head){ // cartoon-style: a number pops up; at 0 health the person is knocked out for a few seconds, then gets up
  if(p.ko>0)return false;
  if(p.remote){netSend({ch:'dmg',to:p.rid,d:dmg,w:WP.lastUsed});floatText(head?'HEADSHOT '+Math.round(dmg*10)/10:String(Math.round(dmg*10)/10),p.x,p.g.position.y+2.5,p.z,head?'#ff4d4d':'#ffe066');WP.hitFlash=0.15;if(head)sfxHeadshot();else sfxHit();return true;} // a real player: their game applies the damage
  if(p.hp===undefined)p.hp=100;
  p.hp=Math.max(0,p.hp-dmg);
  floatText(head?'HEADSHOT '+Math.round(dmg*10)/10:String(Math.round(dmg*10)/10),p.x,p.g.position.y+2.5,p.z,head?'#ff4d4d':(p.hp<=0?'#ff6b6b':'#ffe066'));
  WP.hitFlash=0.15;if(head)sfxHeadshot();else sfxHit();
  if(p.hp<=0){p.ko=p.isBot?3.2:6.5;p.g.rotation.order='YXZ';floatText(p.isBot?'ELIMINATED':'KO!',p.x,p.g.position.y+3.1,p.z,'#7dff6b');if(p.isBot)matchKill(p);}
  return true;
}
const tracers=[];
(function(){
  const g=new THREE.BoxGeometry(0.035,0.035,1),m=new THREE.MeshBasicMaterial({color:0xffd84a,transparent:true,opacity:0.9,blending:THREE.AdditiveBlending,depthWrite:false});
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
function shootRay(ox,oz,dx,dz,range,oy,slope){
  const wall=wallDist(ox,oz,dx,dz,range);let best=null,bt=wall,head=false;
  eachTarget(function(p){
    if(p.ko>0)return;
    const rx=p.x-ox,rz=p.z-oz,t=rx*dx+rz*dz;
    if(t<0.3||t>=bt)return;
    if(Math.abs(rx*dz-rz*dx)<0.55){best=p;bt=t;const hy=(oy||0)+t*(slope||0)-(p.g.position.y-0.3);head=hy>1.8&&hy<2.45;}
  });
  return{p:best,t:bt,head:head};
}
function fireGun(id){
  const d=WDEF[id],shd=matchShoulder(),ox=WK.x+Math.sin(WK.h)*0.75-Math.cos(WK.h)*shd,oz=WK.z+Math.cos(WK.h)*0.75+Math.sin(WK.h)*shd,y=WK.y+1.4;
  const spread=WP.aim?d.spreadAds:d.spreadHip,pellets=d.pellets||1,hits=new Map(),heads=new Set(),eyeY=WK.y+1.62,slope=Math.tan(MATCH.pitch),hm=HEADMUL[id]||2;
  for(let k=0;k<pellets;k++){
    const a=WK.h+rnd(-spread,spread),dx=Math.sin(a),dz=Math.cos(a),r=shootRay(ox,oz,dx,dz,d.range,eyeY,slope);
    let dmg=d.dmg;if(pellets>1)dmg=d.dmg*(1-clamp(r.t/d.range,0,1)*0.75); // shotgun pellets weaken with distance
    if(r.p&&r.head){dmg*=hm;heads.add(r.p);}
    if(r.p)hits.set(r.p,(hits.get(r.p)||0)+dmg);
    const ey=y+r.t*slope,ex=ox+dx*r.t,ez=oz+dz*r.t;if(MATCH.on&&MATCH.fpv){const ca=Math.cos(WK.h),sa=Math.sin(WK.h);tracer(ox+sa*0.9-ca*0.18,y-0.2,oz+ca*0.9+sa*0.18,ex,ey,ez);}else tracer(ox,y,oz,ex,ey,ez);
    if(r.p){for(let s=0;s<3;s++)spawnP(ex,ey,ez,rnd(-3,3),rnd(0,3),rnd(-3,3),1,0.55,0.25,0.25,3,6,2,0.6);}
    else if(r.t<d.range-0.5){for(let s=0;s<3;s++)spawnP(ex,ey,ez,rnd(-2,2),rnd(0.5,2.5),rnd(-2,2),0.7,0.7,0.7,0.5,2,3,0,1.4);}
  }
  hits.forEach(function(dmg,p){hurtPerson(p,dmg,heads.has(p));if(heads.has(p))taskAdd('heads',1);});
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
function swingScythe(){if(WP.cd>0)return;WP.cd=WDEF[WP.cur].rate;WP.swingT=WP.cur==='knife'?0.2:WP.cur==='katana'?0.26:0.3;WP.swingHit=false;sfxSwing();}
function scytheHit(){
  const d=WDEF[WP.cur];let n=0;
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
const GD={open:false};
function openGuide(){GD.open=true;$('guide').style.display='flex';for(const k in keys)keys[k]=false;}
function closeGuide(){GD.open=false;$('guide').style.display='none';try{localStorage.setItem('dcs_guide_seen','1');}catch(e){}}
$('gdClose').addEventListener('click',closeGuide);
const SLIDE_T=0.95;
function startSlide(){ // C: slide like in Rivals, a fast low glide that keeps your momentum
  if(!WK.on||!started||shopOpen||matchFrozen()||WK.slideT>0||WK.slideCd>0||WK.dashT>0||WK.jy>0.05)return;
  WK.slideT=SLIDE_T;WK.slideDir=WK.h;WK.slideSp=Math.max(11,Math.abs(WK.sp)+5)*Math.min(ADM.speed,3);
  sfxSlide();shake=Math.max(shake,0.06);
}
function sfxSlide(){
  if(!AC)return;const t=AC.currentTime,n=noiseSrc(t,0.7),bp=filt('bandpass',900,0.7),g=envGain(t,0.22,0.03,0.6,master);n.connect(bp);bp.connect(g);
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
    const cnt=id==='grenade'?save.wp.nades:(id==='scythe'?'dash':(id==='knife'?'stab':id==='katana'?'slash':save.wp.ammo[id]));
    return'<div class="wslot'+(id===WP.cur?' sel':'')+'" data-w="'+id+'"><small style="display:block;opacity:.6;font-size:9px;letter-spacing:1px">'+slotOf(id)+'</small><b>'+WNAMES[id]+'</b><span>'+cnt+'</span></div>';
  }).join('');
}
// --- loadout screen (like Rivals): each slot has its own list of options, pick one per slot ---
const LO={open:false};
const LSLOTS=[
  {k:'primary',n:'PRIMARY',opts:['rifle','smg','sniper','shorty']},
  {k:'secondary',n:'SECONDARY',opts:['pistol','shorty','smg']},
  {k:'melee',n:'MELEE',opts:['scythe','knife','katana']},
  {k:'utility',n:'UTILITY',opts:['grenade','rocket','glaunch']}
];
const LINFO={rifle:'100 bullets, 6.6 dmg, full-auto',shorty:'30 bullets, 6 pellets, close range',scythe:'Swing 35 dmg, dash 25 dmg',grenade:'75 dmg blast',pistol:'48 bullets, 14 dmg, semi-auto',smg:'120 bullets, 4.2 dmg, very fast',sniper:'12 bullets, 55 dmg, scope',knife:'Fast stab, 22 dmg',katana:'Slash 42 dmg, long reach',rocket:'4 rockets, 90 dmg blast',glaunch:'6 shells, 60 dmg arcing blast'};
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
// --- Rivals-style helpers: katana deflect (K), aimbot (T), auto-shoot (Y) ---
const AS={aim:false,auto:false,def:false,defT:0,defCd:0,defTxt:''};
const DEF_TIME=15,DEF_CD=10; // deflect lasts 15 s, then a 10 s cooldown
const AS_KEY={def:'KeyK',aim:'KeyT',auto:'KeyY'},AS_NAME={def:'Katana deflect',aim:'Aimbot',auto:'Auto-shoot'};
function setTog(k,on){const els=document.querySelectorAll('.tb[data-k="'+AS_KEY[k]+'"]');for(let i=0;i<els.length;i++)els[i].classList.toggle('tog',on);}
function toggleAssist(k){
  if(k==='def'){
    if(AS.defT>0){AS.defT=0;AS.def=false;AS.defCd=DEF_CD;setTog('def',false);toast('Deflect off',1000);return;}
    if(AS.defCd>0){toast('Deflect recharging: '+Math.ceil(AS.defCd)+'s',1000);return;}
    if(!MATCH.on||!WK.on||WP.cur!=='katana'){toast('Hold the Katana in a match to deflect',1500);return;}
    AS.defT=DEF_TIME;AS.def=true;setTog('def',true);toast('DEFLECT ON for 15s - you cannot be hurt',1500);return;
  }
  AS[k]=!AS[k];toast(AS_NAME[k]+(AS[k]?' ON':' OFF')+'',1300);
  setTog(k,AS[k]);
}
function sfxDeflect(){if(!AC)return;const t=AC.currentTime,o=osc('triangle',2800,1500,t,0.16),o2=osc('square',4200,3000,t,0.08),g=envGain(t,0.22,0.001,0.16,master);o.connect(g);o2.connect(g);o.start(t);o2.start(t);o.stop(t+0.18);o2.stop(t+0.1);}
function tryDeflect(b){ // a bot's bullet that would hit you bounces back at the shooter when you hold the katana with deflect on (and face it)
  if(!MATCH.on||!WK.on||WP.cur!=='katana'||!(AS.defT>0))return false;
  if(Math.abs(angDiff(WK.h,Math.atan2(b.x-WK.x,b.z-WK.z)))>1.4)return false;
  tracer(WK.x,1.4,WK.z,b.x,1.35,b.z);
  for(let s=0;s<6;s++)spawnP(WK.x+Math.sin(WK.h)*0.8,1.4,WK.z+Math.cos(WK.h)*0.8,rnd(-4,4),rnd(0,4),rnd(-4,4),0.7,0.9,1,0.25,3,4,2,0.7);
  floatText('DEFLECTED!',WK.x,WK.y+2.4,WK.z,'#7fe8ff');if(defRing){defRing.style.transform='scale(1.35)';setTimeout(function(){defRing.style.transform='';},150);}sfxDeflect();WP.swingT=Math.max(WP.swingT,0.2);WP.swingHit=true;shake=Math.max(shake,0.05);
  hurtPerson(b,10);taskAdd('deflect',1); // the bullet flies back
  return true;
}
let defRing=null;
function defTick(dt){
  if(!MATCH.on||!WK.on){AS.defT=0;AS.defCd=0;if(AS.def){AS.def=false;setTog('def',false);}}
  if(AS.defT>0){AS.defT-=dt;if(AS.defT<=0){AS.defT=0;AS.def=false;AS.defCd=DEF_CD;setTog('def',false);toast('Deflect over',1000);}}
  else if(AS.defCd>0)AS.defCd=Math.max(0,AS.defCd-dt);
  const on=AS.defT>0&&WP.cur==='katana'&&MATCH.on&&WK.on;
  if(!defRing){defRing=document.createElement('div');defRing.style.cssText='position:fixed;left:50%;top:50%;width:170px;height:170px;margin:-85px 0 0 -85px;border-radius:50%;border:4px solid rgba(255,255,255,.9);box-shadow:0 0 30px 10px rgba(255,255,255,.55),inset 0 0 30px 6px rgba(160,230,255,.5);pointer-events:none;z-index:5;display:none;transition:transform .15s';defRing.innerHTML='<svg id="defStar" viewBox="-50 -50 100 100" style="position:absolute;inset:0;width:100%;height:100%;filter:drop-shadow(0 0 6px #fff) drop-shadow(0 0 14px #bfefff)"><polygon fill="#fff" points="0,-46 5,-5 46,0 5,5 0,46 -5,5 -46,0 -5,-5"/><polygon fill="#e8f9ff" transform="rotate(45)" points="0,-24 3,-3 24,0 3,3 0,24 -3,3 -24,0 -3,-3"/><circle r="5" fill="#fff"/></svg>';document.body.appendChild(defRing);}
  const st=defRing.firstChild;if(st&&on)st.style.transform='rotate('+((performance.now()/9)%360)+'deg)';
  defRing.style.display=on?'block':'none';
  const lbl=AS.defT>0?Math.ceil(AS.defT)+'s':AS.defCd>0?'wait '+Math.ceil(AS.defCd):'DEFLECT';
  if(lbl!==AS.defTxt){AS.defTxt=lbl;const sm=document.querySelectorAll('.tb[data-k="KeyK"] small');for(let i=0;i<sm.length;i++)sm[i].textContent=lbl;}
}
function findAssistTarget(){
  const ox=WK.x,oz=WK.z,rng=(WDEF[WP.cur]&&WDEF[WP.cur].range)||60;let best=null,bs=1e9;
  eachTarget(function(p){
    if(p.ko>0)return;
    const dx=p.x-ox,dz=p.z-oz,d=Math.hypot(dx,dz);if(d<1||d>rng)return;
    const ang=Math.abs(angDiff(WK.h,Math.atan2(dx,dz)));if(ang>1.0)return;
    if(wallDist(ox,oz,dx/d,dz/d,d)<d-0.6)return;
    const sc=ang*8+d*0.02;if(sc<bs){bs=sc;best=p;}
  });
  return best;
}
function assistTick(dt,active){ // aimbot swings you onto the nearest enemy in front; auto-shoot pulls the trigger when the crosshair is on one
  WP.autoOn=false;
  if(!MATCH.on||MATCH.state!=='live'||!active||GUNS.indexOf(WP.cur)<0||(!AS.aim&&!AS.auto))return;
  const t=findAssistTarget();if(!t)return;
  const dx=t.x-WK.x,dz=t.z-WK.z,d=Math.hypot(dx,dz),th=Math.atan2(dx,dz),hy=(t.g.position.y-0.3)+1.95-(WK.y+1.62),tp=clamp(Math.atan2(hy,d),-0.5,0.5);
  if(AS.aim){const k=Math.min(1,dt*14);WK.h+=angDiff(WK.h,th)*k;MATCH.pitch+=(tp-MATCH.pitch)*k;}
  if(AS.auto&&Math.abs(angDiff(WK.h,th))<Math.min(0.12,Math.atan2(0.4,d)))WP.autoOn=true;
}
function updateWeapons(dt){
  WP.cd=Math.max(0,WP.cd-dt);WP.dashCd=Math.max(0,WP.dashCd-dt);WP.hitFlash=Math.max(0,WP.hitFlash-dt);
  for(let i=0;i<tracers.length;i++){const t=tracers[i];if(t.life>0){t.life-=dt;if(t.life<=0)t.m.visible=false;}}
  updateNades(dt);
  const list=ownedList();
  if(WK.on&&list.length&&(!WP.cur||list.indexOf(WP.cur)<0)){WP.cur=list[0];attachModel();}
  if(WK.on&&WK.hum&&WP.cur&&WP.modelFor!==WP.cur)attachModel();
  const active=WK.on&&started&&!shopOpen&&!kicked&&list.length>0&&!!WP.cur&&!matchFrozen();
  WP.aim=active&&canAim(WP.cur)&&(WP.aimMouse||isK('KeyQ'));
  defTick(dt);assistTick(dt,active);
  const holdF=WP.fire||isK('KeyJ')||WP.autoOn,dF=WDEF[WP.cur];
  if(active&&holdF&&WP.cd<=0&&(!dF||dF.auto!==false||!WP.heldF)){WP.heldF=true;tryFire();}
  if(!holdF)WP.heldF=false;
  if(WP.swingT>0){WP.swingT-=dt;if(!WP.swingHit&&WP.swingT<(WP.cur==='knife'?0.1:WP.cur==='katana'?0.13:0.16)){WP.swingHit=true;scytheHit();}}
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
function sfxHeadshot(){if(!AC)return;const t=AC.currentTime,o=osc('square',1900,2600,t,0.09),g=envGain(t,0.2,0.001,0.1,master);o.connect(g);o.start(t);o.stop(t+0.12);}
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
const LB={open:false,mode:'ffa',size:2},LB_MODES=['ffa','tdm','gungame','melee'];
Object.assign(MATCH,{bots:[],hp:100,kills:0,deaths:0,time:180,target:10,t:0,pitch:0,lastHurt:-9,inv:0,snap:null,prev:null,sfxT:0,hurtFlash:0,leaveArm:0,vm:null,vmFor:null,hudKey:'',cnt:-1,lastBy:'',winner:'',reward:0,lookId:-1,mode:'ffa',size:2,fpv:true,opp:null,online:false,east:false,hk:0,hd:0});
const BOTNAMES=['Arjun','Priya','Rohan','Meera','Kabir','Diya','Zoya','Veer','Isha','Neel'];
const LADDER=['pistol','smg','shorty','rifle','sniper','glaunch','katana','knife']; // Gun Game: every kill moves you up one weapon
const MELEEL=['katana','knife','scythe'];
const SLOTOF={rifle:'primary',smg:'primary',sniper:'primary',shorty:'secondary',pistol:'secondary',scythe:'melee',knife:'melee',katana:'melee',glaunch:'utility',rocket:'utility',grenade:'utility'};
const MODEINFO={
  ffa:'Free-for-all vs 3 bots in the arena · first to 10 eliminations, or most kills in 3:00.',
  tdm:'Team Deathmatch · you + bot teammates (blue) vs bots (red) · first team to the score wins.',
  gungame:'Gun Game · every kill gives you the next weapon: Pistol, SMG, Shorty, Rifle, Sniper, Grenade Launcher, Katana, Knife. Finish the ladder first!',
  melee:'Melee Only · Katana, Knife and Scythe only (keys 1-3). Bots rush you with blades · first to 8.'
};
function matchFrozen(){return MATCH.on&&MATCH.state!=='live';}
function applyDay(on){ // the arena is a bright daytime map
  ambL.color.setHex(on?0xffffff:0x7a68b0);ambL.intensity=on?1.25:0.95;
  moonLight.color.setHex(on?0xfff6e0:0xaab4ff);moonLight.intensity=on?0.95:0.65;moonLight.position.set(on?60:-80,120,on?-40:60);
  skyObjs.forEach(function(o){o.visible=!on;});
}
function mapKit(root,boxes){
  const scene=root,ax=ARENA.x,az=ARENA.z,hs=ARENA.hs;
  function reg(x,z,w,d,h,m){const bd={x:ax+x,z:az+z,hw:w/2,hd:d/2,h:h,mesh:m,roof:null,ls:[]};boxes.push(bd);return bd;}
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
  function floor(col){const f=new THREE.Mesh(new THREE.PlaneGeometry(hs*2,hs*2),new THREE.MeshLambertMaterial({color:col}));f.rotation.x=-Math.PI/2;f.position.set(ax,0.03,az);scene.add(f);}
  function flat(x,z,w,d,col,y,basic){const m=new THREE.Mesh(new THREE.PlaneGeometry(w,d),basic?new THREE.MeshBasicMaterial({color:col}):new THREE.MeshLambertMaterial({color:col}));m.rotation.x=-Math.PI/2;m.position.set(ax+x,y||0.05,az+z);scene.add(m);}
  function perimeter(){fence(0,-hs-1,hs*2+4,2,7);fence(0,hs+1,hs*2+4,2,7);fence(-hs-1,0,2,hs*2+4,7);fence(hs+1,0,2,hs*2+4,7);}
  return{root:root,ax:ax,az:az,hs:hs,reg:reg,ab:ab,house:house,fence:fence,floor:floor,flat:flat,perimeter:perimeter};
}

const SPAWNS=[[-39,-26],[39,-26],[-39,26],[39,26],[-26,-39],[26,39],[-26,39],[26,-39]];
// --- several maps; they all share the same walled 84x84 square, only one is active (and collidable) at a time ---
const MAPS={
  streets:{name:'Diwali Streets',icon:'\u{1F3D9}️',desc:'Cross-roads, houses and market stalls',sky:[0.78,0.8,0.84],grad:'linear-gradient(135deg,#f3d9a4,#f0a8b8)',build:function(k){const scene=k.root,ax=k.ax,az=k.az,hs=k.hs,ab=k.ab,house=k.house;
  const fl=new THREE.Mesh(new THREE.PlaneGeometry(hs*2,hs*2),new THREE.MeshLambertMaterial({color:0x9c9ca0}));fl.rotation.x=-Math.PI/2;fl.position.set(ax,0.03,az);scene.add(fl);
  // asphalt cross-roads + paving
  const rm=new THREE.MeshLambertMaterial({color:0x3b3b42});
  [[0,0,hs*2,9],[0,0,9,hs*2]].forEach(function(r){const m=new THREE.Mesh(new THREE.PlaneGeometry(r[2],r[3]),rm);m.rotation.x=-Math.PI/2;m.position.set(ax,0.05,az);scene.add(m);});
  [[0,0,hs*2,0.5],[0,0,0.5,hs*2]].forEach(function(r){const m=new THREE.Mesh(new THREE.PlaneGeometry(r[2],r[3]),new THREE.MeshBasicMaterial({color:0xf2e6a0}));m.rotation.x=-Math.PI/2;m.position.set(ax,0.07,az);scene.add(m);});
  k.perimeter();
  // buildings around the cross-roads (roads run along the x and z axes through the middle)
  house(-17,-17,14,14,9,0xf3d9a4,0);house(17,-17,14,14,7,0xf0a8b8,1);house(-17,17,14,14,7,0x9fd4c6,2);house(17,17,14,14,9,0xf5c46b,3);
  house(-33,-33,10,10,6,0xd9b08c,1);house(33,-33,10,10,6,0xb8c8e8,2);house(-33,33,10,10,6,0xe8d28a,3);house(33,33,10,10,6,0xc9e0b0,0);
  house(-32,0,8,12,5,0xf0b98a,2);house(32,0,8,12,5,0xa8d0e0,1);house(0,-32,12,8,5,0xe8a8a0,3);house(0,32,12,8,5,0xd8d8a0,0);
  // crates / market stalls on the streets for cover
  [[8,8],[-8,-8],[8,-8],[-8,8],[22,0],[-22,0],[0,22],[0,-22]].forEach(function(q,k){ab(q[0],q[1],2.4,2.4,2.4,k%2?0xc9962a:0xd9a82a);});
  return SPAWNS;
  }},
  mela:{name:'Mela Grounds',icon:'\u{1F3A1}',desc:'Open fairground: stalls ring a big stage',sky:[0.95,0.78,0.6],grad:'linear-gradient(135deg,#7fb069,#f2c14e)',build:function(k){
    const ab=k.ab,hs=k.hs;k.floor(0x7fb069);k.flat(0,0,hs*2,8,0xc9a66b);k.flat(0,0,8,hs*2,0xc9a66b,0.06);
    ab(0,0,10,10,1.4,0xc0392b);ab(0,0,3,3,6,0xf2c14e);
    const cols=[0xe8473a,0xf2a62a,0x2aa66a,0xd63a9a];
    for(let i=0;i<8;i++){const a=i*Math.PI/4+Math.PI/8;ab(Math.cos(a)*20,Math.sin(a)*20,5,3.4,3.2,cols[i%4]);}
    [[-30,-30],[30,-30],[-30,30],[30,30]].forEach(function(q){ab(q[0],q[1],3,3,13,0x3a6ea5);});
    [[10,0],[-10,0],[0,10],[0,-10],[34,0],[-34,0]].forEach(function(q,i){ab(q[0],q[1],2.2,2.2,2.2,i%2?0xc9962a:0xd9a82a);});
    k.perimeter();return SPAWNS;}},
  fort:{name:'Rajasthan Fort',icon:'\u{1F3F0}',desc:'Sandstone walls, gates and a central keep',sky:[0.95,0.85,0.7],grad:'linear-gradient(135deg,#e0c08a,#c98f4f)',build:function(k){
    const ab=k.ab;k.floor(0xe0c08a);const wl=0xd9a96a,dk=0xc98f4f;
    ab(0,0,14,14,10,dk);
    [[-20,-22],[20,-22],[-20,22],[20,22]].forEach(function(q){ab(q[0],q[1],20,3,6,wl);});
    [[-22,-20],[22,-20],[-22,20],[22,20]].forEach(function(q){ab(q[0],q[1],3,20,6,wl);});
    [[-36,-36],[36,-36],[-36,36],[36,36]].forEach(function(q){ab(q[0],q[1],6,6,9,dk);});
    [[-10,-10],[10,-10],[-10,10],[10,10]].forEach(function(q){ab(q[0],q[1],2.4,2.4,2.4,0xb87a3a);});
    k.perimeter();return SPAWNS;}},
  yard:{name:'Container Yard',icon:'\u{1F4E6}',desc:'Long container lanes and a crane in the middle',sky:[0.7,0.78,0.88],grad:'linear-gradient(135deg,#6f7480,#e0b030)',build:function(k){
    const ab=k.ab;k.floor(0x77797f);k.flat(0,0,84,0.4,0xf2e6a0,0.06,true);
    const cc=[0xd0452c,0x2f6fb0,0x2aa66a,0xe0a02a,0x8a4fb0];
    [[-26,-26],[0,-26],[26,-26],[-26,26],[0,26],[26,26]].forEach(function(q,i){ab(q[0],q[1],14,2.8,3,cc[i%5]);});
    [[-16,-9],[16,-9],[-16,9],[16,9]].forEach(function(q,i){ab(q[0],q[1],14,2.8,i%2?6:3,cc[(i+2)%5]);});
    [[-34,0],[34,0]].forEach(function(q,i){ab(q[0],q[1],2.8,12,3,cc[(i+1)%5]);});
    ab(0,0,4,4,16,0xe0b030);[[8,0],[-8,0]].forEach(function(q){ab(q[0],q[1],2,2,2,0x4a4d55);});
    k.perimeter();return SPAWNS;}}
};
function activateMap(id){
  const M=MAPS[id]||MAPS.streets;id=MAPS[id]?id:'streets';
  if(ARENA.cur&&ARENA.cur!==id){const o=MAPS[ARENA.cur];if(o.root)o.root.visible=false;o.boxes.forEach(function(b){const i=buildings.indexOf(b);if(i>=0)buildings.splice(i,1);});ARENA.cur=null;}
  if(!M.root){const root=new THREE.Group(),boxes=[];scene.add(root);M.spawns=M.build(mapKit(root,boxes));M.root=root;M.boxes=boxes;}
  if(ARENA.cur!==id){M.root.visible=true;M.boxes.forEach(function(b){buildings.push(b);});}
  ARENA.cur=id;ARENA.boxes=M.boxes;ARENA.spawns=M.spawns;ARENA.built=true;
}
let MAPSEL='streets';
function buildArena(){activateMap(MAPSEL);}
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
function clearBots(){MATCH.bots.forEach(function(b){scene.remove(b.g);});MATCH.bots=[];}
function makeBots(){ // fresh bots for every match, because the mode decides how many and which team
  clearBots();
  const m=MATCH.mode,n=MATCH.size,spec=[];
  if(m==='tdm'){for(let i=0;i<n-1;i++)spec.push('A');for(let i=0;i<n;i++)spec.push('B');}
  else for(let i=0;i<3;i++)spec.push('F');
  const cols=[0xd6342b,0xe0872a,0xb02f8f];
  let remoteDone=false;
  spec.forEach(function(tm,i){
    const col=tm==='A'?0x2f7be0:(tm==='B'?0xd6342b:cols[i%3]);
    const h=buildHuman(col,0x23232a,SKINS[(i*2)%5],HAIRS[i%4]);
    const m2=gunModel(m==='melee'?'katana':'rifle');m2.position.y=-0.35;h.f2.add(m2);h.g.visible=false;scene.add(h.g);h.g.rotation.order='YXZ';
    const bo={g:h.g,hum:h,isBot:true,team:tm,name:BOTNAMES[i%BOTNAMES.length],x:0,z:0,h:0,hp:100,ko:0,ph:0,sp:0,cd:1,sdir:1,sT:0,dest:null,destT:0,seeT:0,kills:0,hurtT:0,swT:0,wm:m2,wid:'rifle'};
    if(MATCH.opp&&tm==='B'&&!remoteDone){remoteDone=true;bo.remote=true;bo.rid=MATCH.opp;bo.name='Player '+MATCH.opp.slice(0,3);bo.seen=T;}
    MATCH.bots.push(bo);
  });
}
function spawnPoint(avoidBots,team){ // spawn spot farthest from everyone else (in team matches: your own side of the arena)
  let best=null,bd=-1;
  ARENA.spawns.forEach(function(q){
    if(team&&MATCH.mode==='tdm'&&(((team==='A')!==!!MATCH.east)?q[0]>0:q[0]<0))return;
    const x=ARENA.x+q[0],z=ARENA.z+q[1];let md=1e9;
    if(avoidBots)MATCH.bots.forEach(function(b){if(b.g.visible&&b.ko<=0&&b.team!==team)md=Math.min(md,Math.hypot(b.x-x,b.z-z));});
    else md=Math.hypot((WK.on?WK.x:P.x)-x,(WK.on?WK.z:P.z)-z);
    md+=Math.random()*10;if(md>bd){bd=md;best=[x,z];}
  });
  return best;
}
function placeBot(b){const q=spawnPoint(false,b.team);b.x=q[0];b.z=q[1];b.hp=100;b.ko=0;b.g.visible=true;b.g.rotation.set(0,0,0);b.dest=null;b.seeT=0;b.cd=rnd(0.8,1.6);b.h=Math.atan2(ARENA.x-b.x,ARENA.z-b.z);}
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
  for(const k in keys)keys[k]=false;LB.open=true;lbRefresh();$('lobby').style.display='flex';
}
function lbRefresh(){
  LB_MODES.forEach(function(m){$('lbm_'+m).className=m===LB.mode?'sel':'';});
  for(let n=1;n<=5;n++)$('lbs_'+n).className=n===LB.size?'sel':'';
  $('lbTeams').style.display=LB.mode==='tdm'?'block':'none';
  let d=MODEINFO[LB.mode];if(LB.mode==='tdm')d='Team Deathmatch '+LB.size+'v'+LB.size+' · you + '+(LB.size-1)+' bot teammate'+(LB.size===2?'':'s')+' (blue) vs '+LB.size+' bots (red) · first team to '+(5+5*LB.size)+'.';
  $('lbDesc').textContent=d;
  const gm=LB.mode==='gungame'||LB.mode==='melee';
  $('lbList').style.display=gm?'none':'block';
}
function closeLobby(){LB.open=false;$('lobby').style.display='none';}
LB_MODES.forEach(function(m){$('lbm_'+m).addEventListener('click',function(){LB.mode=m;lbRefresh();});});
for(let n=1;n<=5;n++)(function(n){$('lbs_'+n).addEventListener('click',function(){LB.size=n;lbRefresh();});})(n);
function matchKey(){
  if(!started)return;
  if(MATCH.on){if(MATCH.state==='over')return;if(T<MATCH.leaveArm)leaveMatch();else{MATCH.leaveArm=T+2.5;toast('Press P (or MATCH) again to leave the match',2200);}return;}
  if(LB.open)closeLobby();else openLobby();
}
// --- tasks: finish them to earn coins (resets daily, kept in localStorage) ---
const TASKS=[
  {id:'kills',t:'Eliminate 10 opponents in Diwali Duels',goal:10,reward:150},
  {id:'heads',t:'Land 5 headshots',goal:5,reward:200},
  {id:'played',t:'Play 3 matches',goal:3,reward:120},
  {id:'wins',t:'Win 2 matches',goal:2,reward:300},
  {id:'deflect',t:'Deflect 5 bullets with the katana',goal:5,reward:200},
  {id:'coins',t:'Collect 20 coins in the city',goal:20,reward:100}
];
const TS={day:'',p:{},c:{}};let tsOpen=false;
function tsDay(){const d=new Date();return d.getFullYear()+'-'+(d.getMonth()+1)+'-'+d.getDate();}
function tsLoad(){try{const o=JSON.parse(localStorage.getItem('dcs_tasks_v1')||'null');if(o&&o.day===tsDay()){TS.day=o.day;TS.p=o.p||{};TS.c=o.c||{};return;}}catch(e){}TS.day=tsDay();TS.p={};TS.c={};}
function tsSave(){try{localStorage.setItem('dcs_tasks_v1',JSON.stringify(TS));}catch(e){}tsBadge();}
function tsReady(){return TASKS.filter(function(k){return (TS.p[k.id]||0)>=k.goal&&!TS.c[k.id];});}
function tsBadge(){const el=$('tkn');if(el)el.textContent=tsReady().length;}
function taskAdd(id,n){
  if(TS.day!==tsDay())tsLoad();
  const k=TASKS.find(function(x){return x.id===id;});if(!k||TS.c[id])return;
  const before=TS.p[id]||0,after=Math.min(k.goal,before+(n||1));if(after===before)return;
  TS.p[id]=after;tsSave();
  if(before<k.goal&&after>=k.goal){toast('Task complete: '+k.t+' · open Tasks (H) to claim +'+k.reward+' coins',3800);sfxDing();}
  if(tsOpen)renderTasks();
}
function claimTask(id){
  const k=TASKS.find(function(x){return x.id===id;});if(!k||TS.c[id]||(TS.p[id]||0)<k.goal)return 0;
  TS.c[id]=1;save.coins+=k.reward;persist();updateHUDcoins();tsSave();return k.reward;
}
function renderTasks(){
  const box=$('tkList');box.innerHTML='';
  TASKS.forEach(function(k){
    const p=Math.min(k.goal,TS.p[k.id]||0),done=p>=k.goal,row=document.createElement('div');row.className='trow'+(done?' done':'');
    row.innerHTML='<div class="tt"><b>'+k.t+'</b><div class="tb2"><i style="width:'+Math.round(p/k.goal*100)+'%"></i></div><small style="opacity:.75">'+p+' / '+k.goal+'</small></div>';
    const b=document.createElement('button');b.textContent=TS.c[k.id]?'CLAIMED':(done?'CLAIM +'+k.reward:'+'+k.reward);b.disabled=!!TS.c[k.id]||!done;
    b.addEventListener('click',function(){const r=claimTask(k.id);if(r){sfxCoin();toast('+'+r+' coins',1600);}renderTasks();});row.appendChild(b);box.appendChild(row);
  });
  $('tkAll').disabled=!tsReady().length;
}
function toggleTasks(v){
  if(!started||MATCH.on||MV.open)return;
  tsOpen=(v===undefined)?!tsOpen:!!v;
  if(tsOpen){for(const k in keys)keys[k]=false;if(LB.open)closeLobby();if(TS.day!==tsDay())tsLoad();renderTasks();}
  $('tasks').style.display=tsOpen?'flex':'none';
}
$('tkClose').addEventListener('click',function(){toggleTasks(false);});
$('lbTasks').addEventListener('click',function(){closeLobby();toggleTasks(true);});
$('tkAll').addEventListener('click',function(){let t=0;tsReady().forEach(function(k){t+=claimTask(k.id);});if(t){sfxCoin();toast('+'+t+' coins from tasks',2000);}renderTasks();});
tsLoad();tsBadge();
// --- map voting (like Rivals): three random maps, everyone votes, most votes wins ---
const MV={open:false,ids:[],votes:{},mine:null,t:0,sims:[],iv:0,done:false};
function mvRender(){
  const wrap=$('mvCards');wrap.innerHTML='';
  const mx=Math.max(1,...MV.ids.map(function(id){return MV.votes[id]||0;}));
  MV.ids.forEach(function(id,i){
    const M=MAPS[id],d=document.createElement('div');d.className='mcard'+(MV.mine===id?' sel':'')+(MV.winner===id?' win':'');
    d.innerHTML='<div class="th" style="background:'+M.grad+'">'+M.icon+'</div><b>'+(i+1)+'. '+M.name+'</b><small>'+M.desc+'</small><div class="vt">'+(MV.votes[id]||0)+'</div><div class="bar" style="width:'+Math.round((MV.votes[id]||0)/mx*100)+'%"></div>';
    d.addEventListener('click',function(){mvVote(id);});wrap.appendChild(d);
  });
  $('mvTime').textContent=MV.done?'Map: '+MAPS[MV.winner].name:Math.max(0,Math.ceil(MV.t))+'s';
}
function mvVote(id){if(!MV.open||MV.done)return;if(MV.mine)MV.votes[MV.mine]--;MV.mine=id;MV.votes[id]=(MV.votes[id]||0)+1;mvRender();}
function mvTick(){
  if(!MV.open)return;
  if(MV.done)return;
  MV.t-=0.1;
  MV.sims.forEach(function(v){if(!v.done&&MV.t<=v.at){v.done=true;MV.votes[v.id]=(MV.votes[v.id]||0)+1;}});
  mvRender();
  if(MV.t<=0)mvFinish();
}
function mvFinish(){
  MV.done=true;
  const mx=Math.max.apply(null,MV.ids.map(function(id){return MV.votes[id]||0;})),top=MV.ids.filter(function(id){return(MV.votes[id]||0)===mx;});
  MV.winner=top[(Math.random()*top.length)|0];mvRender();
  setTimeout(function(){mvClose();MAPSEL=MV.winner;startMatch();},1300);
}
function mvClose(){MV.open=false;clearInterval(MV.iv);$('mapvote').style.display='none';}
function openMapVote(){
  if(MATCH.on||MV.open)return;
  closeLobby();
  const all=Object.keys(MAPS).sort(function(){return Math.random()-0.5;});
  MV.ids=all.slice(0,3);MV.votes={};MV.mine=null;MV.t=10;MV.done=false;MV.winner=null;MV.open=true;
  MV.sims=[1,2,3].map(function(){return{at:rnd(1.5,9),id:MV.ids[(Math.random()*MV.ids.length)|0],done:false};}); // the other fighters vote too
  $('mapvote').style.display='flex';mvRender();
  clearInterval(MV.iv);MV.iv=setInterval(mvTick,100);
}
function lobbyPlay(){if(NETM.opp){MAPSEL='streets';startMatch();}else openMapVote();} // online duels keep the default map so both players see the same arena
$('mvBack').addEventListener('click',function(){mvClose();openLobby();});
function startMatch(){
  if(MATCH.on)return;
  MATCH.opp=NETM.opp;MATCH.online=!!NETM.opp;MATCH.east=NETM.east&&MATCH.online;NETM.opp=null;NETM.east=false;MATCH.hk=0;MATCH.hd=0;
  closeLobby();buildArena();MATCH.mode=LB.mode;MATCH.size=LB.mode==='tdm'?LB.size:1;makeBots();
  MATCH.target=MATCH.online?5:MATCH.mode==='tdm'?5+5*MATCH.size:(MATCH.mode==='gungame'?LADDER.length:(MATCH.mode==='melee'?8:10));
  const w=save.wp;
  MATCH.snap=JSON.parse(JSON.stringify(w));
  MATCH.prev={foot:WK.on,x:WK.x,z:WK.z,h:WK.h,p:{x:P.x,z:P.z,h:P.h,type:P.type,hp:P.hp}};
  // starter kit if you own nothing
  if(MATCH.mode==='gungame'||MATCH.mode==='melee'){w.katana=1;w.knife=1;w.scythe=1;w.slots.primary=w.slots.secondary=w.slots.utility='';w.slots.melee=MATCH.mode==='melee'?'katana':'';}
  else{
  if(!ownedList().some(function(id){return GUNS.indexOf(id)>=0||LAUNCHERS.indexOf(id)>=0;})){w.pistol=1;w.ammo.pistol=48;w.slots.secondary='pistol';if(!w.slots.primary||!owns(w.slots.primary))w.slots.primary='';}
  if(!ownedList().some(function(id){return MELEE.indexOf(id)>=0;})){w.knife=1;w.slots.melee='knife';}
  if(!ownedList().length){w.pistol=1;w.slots.secondary='pistol';}
  }
  if(!WK.mesh){const pp=makePerson(0,true);WK.mesh=pp.g;WK.hum=pp;}
  WK.on=true;WK.sp=0;WK.dashT=0;P.speed=0;
  MATCH.on=true;document.body.className=(TOUCH_ON?'touch ':'')+'matchon';
  if(!camera.parent)scene.add(camera);
  WK.mesh.visible=true;WP.cur=null;applyDay(true);
  MATCH.pitch=0;beginRound();toast('Map: '+MAPS[ARENA.cur].name,2200);
  try{if(!TOUCH_ON)renderer.domElement.requestPointerLock();}catch(e){}
}
function beginRound(){
  $('results').style.display='none';
  MATCH.kills=0;MATCH.deaths=0;MATCH.hk=0;MATCH.hd=0;MATCH.time=MATCH.online?180:ADM.mtime;MATCH.hp=100;MATCH.state='count';MATCH.t=3.6;MATCH.swT=0;MATCH.cnt=-1;MATCH.inv=0;MATCH.winner='';
  refillAmmo();
  const q=spawnPoint(false,'A');
  MATCH.bots.forEach(function(b){b.kills=0;b.g.visible=false;});
  WK.x=ARENA.x+rnd(-3,3);WK.z=ARENA.z+rnd(-3,3);WK.h=0;
  const wsp=MATCH.mode==='tdm'?ARENA.spawns.filter(function(q){return MATCH.east?q[0]>0:q[0]<0;}):ARENA.spawns,sp=wsp[(Math.random()*wsp.length)|0];WK.x=ARENA.x+sp[0];WK.z=ARENA.z+sp[1];WK.h=Math.atan2(ARENA.x-WK.x,ARENA.z-WK.z);
  placeVehicle();
  MATCH.bots.forEach(placeBot);
  if(MATCH.mode==='gungame')ggSet(LADDER[0]);else if(MATCH.mode==='melee'){WP.cur='katana';attachModel();}
  $('mfeed').innerHTML='';MATCH.hudKey='';
}
function addFeed(a,verb,b,wn,me){
  const f=$('mfeed'),d=document.createElement('div');if(me)d.className='me';
  d.innerHTML='<i>'+a+'</i> '+verb+' <i>'+b+'</i> <small style="opacity:.7">['+wn+']</small>';f.appendChild(d);
  setTimeout(function(){if(d.parentNode)d.parentNode.removeChild(d);},4500);
  while(f.childNodes.length>5)f.removeChild(f.firstChild);
}
function teamScores(){
  if(MATCH.online)return[MATCH.hk,MATCH.hd];
  if(MATCH.mode==='tdm'){let a=MATCH.kills,c=0;MATCH.bots.forEach(function(b){if(b.team==='A')a+=b.kills;else c+=b.kills;});return[a,c];}
  const best=MATCH.bots.reduce(function(m,b){return Math.max(m,b.kills);},0);return[MATCH.kills,best];
}
function scoreCheck(){const sc=teamScores();if(sc[0]>=MATCH.target||sc[1]>=MATCH.target)endMatch();}
function ggSet(id){ // Gun Game: you carry only the weapon of your current level
  const w=save.wp;w[id]=1;if(GUNS.concat(LAUNCHERS).indexOf(id)>=0)w.ammo[id]=WDEF[id].mag;
  const q=w.slots;q.primary=q.secondary=q.melee=q.utility='';q[SLOTOF[id]]=id;WP.cur=id;WP.cd=0.3;if(WK.on)attachModel();
}
function botHit(v,dmg,att){ // a bot shooting another bot (team matches)
  if(v.remote||v.ko>0||MATCH.state!=='live')return;
  v.hp-=dmg;
  if(v.hp<=0){v.ko=3.2;v.g.rotation.order='YXZ';att.kills++;addFeed(att.name,'eliminated',v.name,'Rifle',false);scoreCheck();}
}
function matchKill(b){
  if(!MATCH.on||MATCH.state!=='live')return;
  MATCH.kills++;taskAdd('kills',1);MATCH.lastKillT=T;addFeed('You','eliminated',b.name,WNAMES[WP.lastUsed]||'Weapon',true);sfxDing();
  if(MATCH.mode==='gungame'&&MATCH.kills<LADDER.length){ggSet(LADDER[MATCH.kills]);toast('LEVEL '+(MATCH.kills+1)+': '+WNAMES[LADDER[MATCH.kills]],1800);}
  scoreCheck();
}
function hurtPlayer(dmg,b){
  if(MATCH.state!=='live'||MATCH.inv>0||ADM.god||MATCH.hp<=0)return;
  if(AS.defT>0&&WK.on&&WP.cur==='katana'){if(T>(AS.blkT||0)){AS.blkT=T+0.4;floatText('BLOCKED',WK.x,WK.y+2.4,WK.z,'#7fe8ff');sfxDeflect();}return;} // deflect up: katana takes every hit, your health stays
  if(!WK.on)dmg*=0.5; // the vehicle takes some of the hits
  MATCH.hp=Math.max(0,MATCH.hp-dmg);MATCH.lastHurt=T;MATCH.hurtFlash=0.4;shake=Math.max(shake,0.12);sfxHurt();
  if(MATCH.hp<=0){
    if(!WK.on){WK.x=P.x;WK.z=P.z;WK.h=P.h;WK.on=true;WK.mesh.visible=true;WK.sp=0;P.speed=0;}
    MATCH.deaths++;if(b.remote){MATCH.hd++;netSend({ch:'died',to:b.rid});}MATCH.state='dead';MATCH.t=2.6;b.kills++;MATCH.lastBy=b.name;addFeed(b.name,'eliminated','You',MATCH.mode==='melee'?'Blade':'Assault Rifle',false);
    scoreCheck();
  }
}
function respawnPlayer(){
  const q=spawnPoint(true,'A');WK.on=true;WK.mesh.visible=true;WK.x=q[0];WK.z=q[1];WK.h=Math.atan2(ARENA.x-WK.x,ARENA.z-WK.z);MATCH.pitch=0;
  placeVehicle();
  MATCH.hp=100;MATCH.inv=1.8;MATCH.state='live';MATCH.swT=T+6;refillAmmo();WP.cd=0.2;WP.cur=null;if(MATCH.mode==='gungame')ggSet(LADDER[Math.min(MATCH.kills,LADDER.length-1)]);else if(MATCH.mode==='melee'){WP.cur='katana';attachModel();}
}
function endMatch(){
  if(MATCH.state==='over')return;
  const sc=teamScores(),best=sc[1],mine=sc[0];
  MATCH.state='over';
  const win=mine>best,draw=mine===best;
  MATCH.winner=win?'win':(draw?'draw':'lose');taskAdd('played',1);if(win)taskAdd('wins',1);
  MATCH.reward=Math.round((win?150:(draw?80:40))*(MATCH.mode==='tdm'?1+MATCH.size*0.2:1))+MATCH.kills*10;
  $('rsTitle').textContent=win?'VICTORY!':(draw?'DRAW':'DEFEAT');
  $('rsTitle').style.color=win?'#7dffa6':(draw?'#ffd36b':'#ff7a7a');
  $('rsSub').textContent=win?(MATCH.mode==='tdm'?'Your team won. Shubh Diwali!':'You out-fought them. Shubh Diwali!'):(draw?'Dead even.':'They won this one. Try again!');
  $('rsK').textContent=MATCH.kills;$('rsD').textContent=MATCH.deaths;$('rsC').textContent='+'+MATCH.reward;
  $('rsAgain').style.display=MATCH.online?'none':'';
  $('results').style.display='flex';WP.fire=false;WP.aimMouse=false;
  try{if(document.exitPointerLock)document.exitPointerLock();}catch(e){}
}
function leaveMatch(){
  if(!MATCH.on)return;
  const reward=MATCH.state==='over'?MATCH.reward:0;
  if(MATCH.online&&MATCH.opp){netSend({ch:'left',to:MATCH.opp});}
  MATCH.on=false;MATCH.online=false;MATCH.opp=null;MATCH.state='off';applyDay(false);$('results').style.display='none';document.body.className=TOUCH_ON?'touch':'';
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
function updateRemote(b,dt){ // the other human in an online duel
  const o=others.get(b.rid),h=b.hum;
  if(!o||!o.wm||T-(o.wlast||0)>2.5){
    b.g.visible=false;
    if(MATCH.on&&MATCH.state==='live'&&T-b.seen>8){b.seen=T+1e9;MATCH.hk=MATCH.target;toast('Your opponent disconnected - you win!',2500);scoreCheck();}
    return;
  }
  b.seen=T;b.g.visible=true;
  const k=Math.min(1,dt*10),ox=b.x,oz=b.z;
  if(!b.x&&!b.z){b.x=o.wx;b.z=o.wz;}
  b.x+=(o.wx-b.x)*k;b.z+=(o.wz-b.z)*k;b.h+=angDiff(b.h,o.wh)*k;b.sp=Math.hypot(b.x-ox,b.z-oz)/Math.max(dt,0.001);
  const wid=o.ww||'rifle';
  if(b.wid!==wid){if(b.wm)h.f2.remove(b.wm);b.wm=gunModel(wid);b.wm.position.y=-0.35;h.f2.add(b.wm);b.wid=wid;}
  if(b.ko>0){b.ko-=dt;b.g.rotation.x+=(-1.45-b.g.rotation.x)*Math.min(1,dt*10);b.g.position.set(b.x,0.18,b.z);b.g.rotation.y=b.h;return;}
  b.g.rotation.x=0;b.ph+=dt*Math.min(b.sp,7)*1.8;poseHuman(h,Math.sin(b.ph)*0.7*Math.min(1,b.sp/3),0);
  if(MELEE.indexOf(wid)>=0){h.a2.rotation.set(-0.5,0,-0.15);h.f2.rotation.x=-0.4;h.a1.rotation.set(-0.35,0,0.25);}
  else{h.a2.rotation.set(-1.25,0,0);h.f2.rotation.x=-0.08;h.a1.rotation.set(-1.15,0,0.32);h.f1.rotation.x=-0.1;}
  b.g.position.set(b.x,0.3,b.z);b.g.rotation.y=b.h;
}
function botTarget(b){ // nearest enemy this bot can fight (players, and in team matches the other team's bots)
  const tdm=MATCH.mode==='tdm';let best=null,bd=1e9;
  const pl=WK.on?WK:P,plOk=MATCH.state==='live'&&MATCH.hp>0&&(!tdm||b.team==='B');
  if(plOk){bd=Math.hypot(pl.x-b.x,pl.z-b.z);best={x:pl.x,z:pl.z,sp:WK.on?WK.sp:P.speed,isP:true};}
  if(tdm)MATCH.bots.forEach(function(o){
    if(o===b||o.team===b.team||!o.g.visible||o.ko>0)return;
    const d=Math.hypot(o.x-b.x,o.z-b.z);if(d<bd){bd=d;best={x:o.x,z:o.z,sp:o.sp,isP:false,b:o};}
  });
  return best;
}
function updateBots(dt){
  const live=MATCH.state==='live',melee=MATCH.mode==='melee';
  MATCH.bots.forEach(function(b){
    if(b.remote){updateRemote(b,dt);return;}
    if(!b.g.visible)return;
    const h=b.hum;
    if(b.ko>0){ // knocked down, then respawns
      b.ko-=dt;b.g.rotation.x+=(-1.45-b.g.rotation.x)*Math.min(1,dt*10);b.g.position.set(b.x,0.18,b.z);b.g.rotation.y=b.h;
      if(b.ko<=0){if(MATCH.state==='over')b.ko=0.01;else placeBot(b);}
      return;
    }
    if(MATCH.state==='over'||MATCH.state==='count'){poseHuman(h,0,0);h.a2.rotation.set(-1.25,0,0);h.f2.rotation.x=-0.08;h.a1.rotation.set(-1.15,0,0.32);h.f1.rotation.x=-0.1;b.g.position.set(b.x,0.3,b.z);b.g.rotation.y=b.h;return;}
    const A=botTarget(b)||{x:b.x+1,z:b.z,sp:0,isP:false,none:true},aspd=A.sp,alive=live&&!A.none;
    const dx=A.x-b.x,dz=A.z-b.z,d=Math.hypot(dx,dz)||0.01;
    const los=wallDist(b.x,b.z,dx/d,dz/d,d)>=d-0.05;
    let vx=0,vz=0,face=b.h;
    const fx=dx/d,fz=dz/d,rx=-fz,rz=fx;
    if(alive&&los&&d<42){
      b.seeT+=dt;b.sT-=dt;if(b.sT<=0){b.sdir=Math.random()<0.5?-1:1;b.sT=rnd(0.7,1.8);}
      const ap=melee?(d>1.7?1:0):(d>22?1:(d<10?-1:0)),sdr=melee?(d>6?0.4:0):3.6,spd=melee?5.4:3.4;vx=fx*ap*spd+rx*b.sdir*sdr;vz=fz*ap*spd+rz*b.sdir*sdr;face=Math.atan2(dx,dz);
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
    if(melee){ // blade fighters: close in and slash
      b.swT=Math.max(0,b.swT-dt);
      if(alive&&d<2.5&&Math.abs(angDiff(b.h,Math.atan2(dx,dz)))<0.8&&b.cd<=0){b.cd=0.8;b.swT=0.28;sfxSwing();if(A.isP)hurtPlayer(16,b);else botHit(A.b,16,b);}
    }else if(alive&&los&&d<42&&b.seeT>0.45&&Math.abs(angDiff(b.h,Math.atan2(dx,dz)))<0.3&&b.cd<=0){
      b.cd=rnd(0.3,0.55);
      const mv=Math.min(1,Math.abs(aspd)/5),acc=clamp(0.34-d*0.0055,0.08,0.34)*(1-mv*0.28)*0.85*((A.isP&&WK.on&&WK.slideT>0)?0.5:1);
      const hit=Math.random()<acc,mx=b.x+Math.sin(b.h)*0.8,mz=b.z+Math.cos(b.h)*0.8;
      if(hit){tracer(mx,1.35,mz,A.x,1.35,A.z);if(A.isP){if(!tryDeflect(b))hurtPlayer(5.5,b);}else botHit(A.b,5.5,b);}
      else{const a=rnd(-0.12,0.12),mm=d+rnd(-4,8);tracer(mx,1.35,mz,b.x+Math.sin(b.h+a)*mm,rnd(0.6,2),b.z+Math.cos(b.h+a)*mm);}
      for(let s=0;s<3;s++)spawnP(mx,1.35,mz,Math.sin(b.h)*6+rnd(-2,2),rnd(-1,1),Math.cos(b.h)*6+rnd(-2,2),1,0.85,0.4,0.08,3,0,0,1.6);
      if(d<30&&T>MATCH.sfxT){MATCH.sfxT=T+0.12;sfxShot('smg');}
    }
    b.ph+=dt*Math.min(b.sp,7)*1.8;const sw=Math.sin(b.ph)*0.7*Math.min(1,b.sp/3);
    poseHuman(h,sw,0);if(melee){const sv=b.swT>0?Math.sin((1-b.swT/0.28)*Math.PI):0;h.a2.rotation.set(-0.5-sv*1.9,0,-0.15+sv*0.6);h.f2.rotation.x=-0.4;h.a1.rotation.set(-0.35,0,0.25);}else{h.a2.rotation.set(-1.25,0,0);h.f2.rotation.x=-0.08;h.a1.rotation.set(-1.15,0,0.32);h.f1.rotation.x=-0.1;}
    b.g.position.set(b.x,0.3,b.z);b.g.rotation.y=b.h;b.g.rotation.x=0;
  });
}
// --- first-person camera + the gun in front of you ---
// --- first-person arms + weapon (like Rivals): the real weapon model sits in your hands in front of the camera ---
const VM={g:null,hold:null,id:null,ads:0,bob:0,lh:null};
const VMC={ // holder position (hip), aim position, left-hand spot along the barrel, scale, one-handed?
  rifle:{p:[0.1,-0.3,-0.66],a:[0,-0.145,-0.32],lh:-0.34,s:1},smg:{p:[0.2,-0.2,-0.48],a:[0,-0.1,-0.3],lh:-0.26,s:1},
  shorty:{p:[0.2,-0.2,-0.5],a:[0,-0.085,-0.3],lh:-0.34,s:1},sniper:{p:[0.22,-0.2,-0.5],a:[0,-0.1,-0.26],lh:-0.5,s:1},
  pistol:{p:[0.16,-0.18,-0.42],a:[0,-0.085,-0.28],lh:0.0,s:1},rocket:{p:[0.22,-0.26,-0.45],a:[0,-0.2,-0.4],lh:-0.4,s:1},glaunch:{p:[0.22,-0.26,-0.45],a:[0,-0.2,-0.4],lh:-0.28,s:1},
  knife:{p:[0.3,-0.28,-0.5],a:[0.3,-0.28,-0.5],lh:null,s:1.5,melee:1},katana:{p:[0.22,0.0,-0.72],a:[0.22,0.0,-0.72],lh:null,s:1,melee:1,kat:1},
  scythe:{p:[0.34,-0.34,-0.4],a:[0.34,-0.34,-0.4],lh:null,s:0.8,melee:1,rev:1},grenade:{p:[0.26,-0.24,-0.5],a:[0.26,-0.24,-0.5],lh:null,s:2,melee:1,nade:1}
};
function vmLimb(x1,y1,z1,x2,y2,z2,w,mat,par){
  const dx=x2-x1,dy=y2-y1,dz=z2-z1,L=Math.hypot(dx,dy,dz)||0.01;
  const me=hpart(hbox(w,w,L),mat,(x1+x2)/2,(y1+y2)/2,(z1+z2)/2,par);me.rotation.order='YXZ';me.rotation.y=Math.atan2(dx,dz);me.rotation.x=-Math.asin(clamp(dy/L,-1,1));return me;
}
function buildKatanaVM(hold,c,sk,sl,sl2){ // Rivals-style katana: long black curved blade held sideways across the screen, green guard + wrapped hilt, fist at the right edge
  const blk=hmat(0x0c0c0f),edge=hmat(0x2a2a32),grn=hmat(0x35d63a),grn2=hmat(0x1f8f25),dk=hmat(0x16161a);
  const L=1.0,N=24,drop=0.15,pt=function(t){return[-t*L,-drop*Math.pow(t,2.1)];};
  for(let i=0;i<N;i++){
    const t0=i/N,t1=(i+1)/N,a=pt(t0),b=pt(t1),len=Math.hypot(b[0]-a[0],b[1]-a[1]),ang=Math.atan2(a[1]-b[1],a[0]-b[0]);
    const th=t1>0.94?0.03*(1-(t1-0.94)/0.06*0.8)+0.003:0.03-0.008*t1; // blade height narrows toward the tip
    const seg=hpart(hbox(len*1.06,th,0.012),blk,(a[0]+b[0])/2,(a[1]+b[1])/2,0,hold);seg.rotation.z=ang;
    const ed=hpart(hbox(len*1.06,th*0.16,0.014),edge,(a[0]+b[0])/2,(a[1]+b[1])/2-th*0.42*Math.cos(ang),0,hold);ed.rotation.z=ang;
  }
  const tb=hpart(hcyl(0.042,0.042,0.012,18),grn,0.01,0,0,hold);tb.rotation.z=Math.PI/2;        // round green guard
  const tb2=hpart(hcyl(0.03,0.03,0.018,14),grn2,0.022,0,0,hold);tb2.rotation.z=Math.PI/2;
  const hl=hpart(hcyl(0.02,0.02,0.34,10),dk,0.2,0,0,hold);hl.rotation.z=Math.PI/2;           // hilt
  for(let k=0;k<5;k++){const r=hpart(hcyl(0.0225,0.0225,0.018,10),grn2,0.08+k*0.065,0,0,hold);r.rotation.z=Math.PI/2;} // green wrap
  hpart(hsph(0.028,8,6),grn,0.385,0,0,hold);                                                  // pommel
  // right hand gripping the hilt (fingers wrap round it), forearm leaves the screen at the bottom right
  hpart(hbox(0.1,0.085,0.085),sk,0.21,0,0.0,hold);
  for(let f=0;f<4;f++)hpart(hbox(0.021,0.075,0.03),sk,0.176+f*0.024,-0.003,0.058,hold);
  hpart(hbox(0.05,0.03,0.03),sk,0.19,0.055,0.025,hold);
  vmLimb(0.25,-0.035,0.06,0.55,-0.3,0.5,0.085,sl,hold);vmLimb(0.55,-0.3,0.5,0.75,-0.55,0.95,0.11,sl2,hold);
}
let flashTex=null;
function mkFlash(pv,z){ // yellow star that pops on every shot
  if(!flashTex){const cv=document.createElement('canvas');cv.width=cv.height=128;const g=cv.getContext('2d'),gd=g.createRadialGradient(64,64,2,64,64,62);gd.addColorStop(0,'rgba(255,255,230,1)');gd.addColorStop(0.35,'rgba(255,210,60,.9)');gd.addColorStop(1,'rgba(255,150,0,0)');g.fillStyle=gd;g.fillRect(0,0,128,128);
    g.fillStyle='rgba(255,235,120,.95)';g.beginPath();for(let i=0;i<10;i++){const a=i*Math.PI/5-Math.PI/2,r=i%2?14:62;g.lineTo(64+Math.cos(a)*r,64+Math.sin(a)*r);}g.closePath();g.fill();flashTex=new THREE.CanvasTexture(cv);}
  const fl=new THREE.Sprite(new THREE.SpriteMaterial({map:flashTex,transparent:true,depthTest:false,blending:THREE.AdditiveBlending}));fl.scale.set(0.42,0.42,1);fl.position.set(0,0.012,z);fl.visible=false;pv.add(fl);VM.mflash=fl;
}
function vmCapsule(pv,a,b,r,mat){ // rounded finger / limb between two points
  const d=b.clone().sub(a),L=d.length()||0.001,Y=new THREE.Vector3(0,1,0);
  const m=new THREE.Mesh(bake(new THREE.CylinderGeometry(r,r,L,12)),mat);m.quaternion.setFromUnitVectors(Y,d.clone().normalize());m.position.copy(a).addScaledVector(d,0.5);pv.add(m);
  [a,b].forEach(function(p){const sp=new THREE.Mesh(bake(new THREE.SphereGeometry(r,12,8)),mat);sp.position.copy(p);pv.add(sp);});
}
function vmHand(pv,cx,cy,cz,sg){ // hand (back of hand, four fingers wrapped round the gun, thumb) + wide faceted sleeve with black cuff and peach band
  const skin=smat(0xdea078),skin2=smat(0xc98a62),V=function(x,y,z){return new THREE.Vector3(x,y,z);};
  hpart(rbox(0.07,0.034,0.09,0.014),skin,cx,cy,cz,pv);                                  // back of the hand
  for(let f=0;f<4;f++){
    const z=cz-0.033+f*0.022,len=f===0||f===3?0.06:0.07;
    vmCapsule(pv,V(cx-sg*0.02,cy,z),V(cx-sg*0.032,cy-0.012,z),0.0115,skin);
    vmCapsule(pv,V(cx-sg*0.032,cy-0.012,z),V(cx-sg*0.03,cy-0.012-len,z),0.0115,skin);
  }
  vmCapsule(pv,V(cx-sg*0.01,cy+0.006,cz-0.04),V(cx-sg*0.03,cy+0.012,cz-0.115),0.0135,skin2); // thumb
  // sleeve (same wide faceted shape as before, darker so the daylight doesn't wash it out)
  const blk=hmat(0x0c0d10),pe=hmat(0xd99a6a),g1=hmat(0x1f6a1f),g2=hmat(0x14451a),g3=hmat(0x2f8a2c),Y=new THREE.Vector3(0,1,0);
  const p=V(cx+sg*0.01,cy-0.03,cz+0.03),dir=V(sg*0.12,-0.24,0.28).normalize(),up=V(-sg*0.1,0.9,0.1).normalize();
  const along=function(r0,r1,len,t,mat,sx){const m=new THREE.Mesh(new THREE.CylinderGeometry(r0,r1,len,6,1),mat);m.scale.x=sx;m.quaternion.setFromUnitVectors(Y,dir.clone().negate());m.position.copy(p).addScaledVector(dir,t+len/2);pv.add(m);return m;};
  const facet=function(off,sx,sy,sz,mat){const m=new THREE.Mesh(new THREE.SphereGeometry(1,5,3),mat);m.scale.set(sx,sy,sz);m.quaternion.setFromUnitVectors(new THREE.Vector3(0,0,1),dir);m.position.copy(p).add(off);pv.add(m);return m;};
  along(0.075,0.08,0.085,0.0,blk,1.6);                    // black cuff
  along(0.08,0.084,0.07,0.085,pe,1.6);                    // peach band of wrist
  along(0.09,0.17,0.36,0.155,g1,1.6);                     // wide sleeve, widening toward the camera
  facet(dir.clone().multiplyScalar(0.36).addScaledVector(up,0.08),0.13,0.05,0.2,g3);
  facet(dir.clone().multiplyScalar(0.4).addScaledVector(up,-0.07).add(V(sg*0.05,0,0)),0.14,0.06,0.2,g2);
}
function rivalsArms(pv,L,R){vmHand(pv,L.x,L.y,L.z,-1);vmHand(pv,R.x,R.y,R.z,1);}
const GSPEC={ // simple chunky Rivals-style guns: [kind,x,y,z,size...,color,rotX]; b = box(w,h,d), c = cylinder along the barrel(r,len), t = ring(r,tube)
  pistol:{fz:-0.31,L:{x:-0.062,y:-0.1,z:0.05},R:{x:0.062,y:-0.1,z:0.05},parts:[
    ['b',0,0.035,-0.1,0.05,0.06,0.32,0xd9dde4],['b',0,-0.02,-0.09,0.048,0.04,0.3,0x2b2e36],['b',0,0.047,-0.1,0.054,0.012,0.12,0xf28a1c],
    ['c',0,0.035,-0.27,0.013,0.06,0x15161b],['b',0,-0.1,0.05,0.045,0.14,0.06,0x2b2e36,0.2],['b',0,0.075,0.04,0.03,0.02,0.01,0x15161b],['b',0,0.075,-0.25,0.01,0.025,0.01,0x15161b]]},
  smg:{fz:-0.52,L:{x:-0.075,y:-0.04,z:-0.3},R:{x:0.058,y:-0.1,z:0.1},parts:[
    ['b',0,0,-0.1,0.065,0.09,0.4,0x2d3a8c],['b',0,0.058,-0.1,0.04,0.02,0.4,0x15161b],['t',0,0.1,-0.28,0.022,0.005,0x15161b],['t',0,0.1,0.06,0.018,0.005,0x15161b],
    ['c',0,0.01,-0.42,0.012,0.18,0x15161b],['c',0,0.01,-0.5,0.019,0.06,0x15161b],['b',0,-0.17,-0.06,0.045,0.24,0.06,0xf2b01c,-0.1],['b',0,-0.11,0.11,0.045,0.13,0.05,0x15161b,0.25],['b',0,-0.01,0.2,0.05,0.08,0.1,0x2d3a8c]]},
  shorty:{fz:-0.64,L:{x:-0.07,y:-0.06,z:-0.3},R:{x:0.058,y:-0.1,z:0.1},parts:[
    ['b',0,0,-0.05,0.06,0.09,0.34,0x9a5f2c],['b',0,-0.045,-0.3,0.078,0.065,0.16,0xd0342c],['c',-0.02,0.025,-0.45,0.016,0.42,0x1b1c22],['c',0.02,0.025,-0.45,0.016,0.42,0x1b1c22],
    ['b',0,0.06,-0.62,0.012,0.03,0.012,0xffd24a],['b',0,0.055,-0.02,0.03,0.015,0.02,0x15161b],['b',0,-0.03,0.25,0.06,0.12,0.22,0x9a5f2c],['b',0,-0.1,0.1,0.045,0.12,0.05,0x6e411c,0.25]]},
  sniper:{fz:-1.03,L:{x:-0.075,y:-0.05,z:-0.35},R:{x:0.058,y:-0.1,z:0.1},parts:[
    ['b',0,0,-0.1,0.06,0.09,0.55,0x35493a],['c',0,0.012,-0.66,0.014,0.7,0x15161b],['c',0,0.012,-1.0,0.022,0.07,0x15161b],
    ['c',0,0.1,-0.12,0.04,0.34,0x15161b],['c',0,0.1,-0.3,0.052,0.05,0x15161b],['c',0,0.1,0.06,0.048,0.05,0x15161b],['c',0,0.1,-0.325,0.045,0.01,0x5ac8ff],
    ['b',0,0.06,-0.2,0.03,0.04,0.03,0x15161b],['b',0,0.06,0.0,0.03,0.04,0.03,0x15161b],['b',0,-0.02,0.3,0.065,0.12,0.26,0x35493a],['b',0,-0.11,0.12,0.045,0.13,0.05,0x15161b,0.25],['b',0,-0.1,-0.02,0.04,0.1,0.06,0x15161b]]}
};
function buildSpecVM(id,hold){
  const sp=GSPEC[id],pv=new THREE.Group();hold.add(pv);VM.pv=pv;
  sp.parts.forEach(function(q){
    let me;
    if(q[0]==='b')me=hpart(rbox(q[4],q[5],q[6],Math.min(q[4],q[5],q[6])*0.32),smat(q[7]),q[1],q[2],q[3],pv);
    else if(q[0]==='c'){me=hpart(hcyl(q[4],q[4],q[5],16),smat(q[6]),q[1],q[2],q[3],pv);me.rotation.x=Math.PI/2;}
    else me=hpart(bake(new THREE.TorusGeometry(q[4],q[5],10,24)),smat(q[6]),q[1],q[2],q[3],pv);
    if(q[0]==='b'&&q[8])me.rotation.x=q[8];
  });
  rivalsArms(pv,sp.L,sp.R);mkFlash(pv,sp.fz);
}

function buildRifleVM(hold,c,sk){ // the golden assault rifle: gold body, black rail, ring sights, green sleeves, yellow muzzle flash
  const pv=new THREE.Group();hold.add(pv);VM.pv=pv;
  const gold=smat(0xd48a0c),gold2=smat(0xe89c12),gold3=smat(0xa86a08),dk=smat(0x0e0f12),gy=smat(0x2a2d36),wh=smat(0xdcd6c8),pe=smat(0xd9976a);
  hpart(rbox(0.08,0.12,0.42,0.035),gold,0,0,-0.02,pv);                       // receiver
  hpart(rbox(0.074,0.09,0.34,0.035),gold2,0,-0.01,-0.4,pv);                  // handguard
  hpart(rbox(0.078,0.03,0.22,0.012),dk,0,-0.047,-0.4,pv);                    // handguard underside
  hpart(rbox(0.046,0.026,0.64,0.01),dk,0,0.07,-0.18,pv);                     // top rail
  for(let i=0;i<13;i++)hpart(rbox(0.05,0.014,0.018,0.005),gy,0,0.085,0.13-i*0.047,pv); // rail ridges
  hpart(rbox(0.014,0.04,0.014,0.005),dk,0,0.1,0.12,pv);hpart(bake(new THREE.TorusGeometry(0.03,0.007,10,28)),dk,0,0.14,0.12,pv);   // rear ring sight
  hpart(rbox(0.014,0.05,0.014,0.005),dk,0,0.1,-0.58,pv);hpart(bake(new THREE.TorusGeometry(0.036,0.008,10,28)),dk,0,0.145,-0.58,pv); // front ring sight
  const br=hpart(hcyl(0.014,0.014,0.2,16),dk,0,0.012,-0.66,pv);br.rotation.x=Math.PI/2;                // barrel
  const fh=hpart(hcyl(0.022,0.022,0.07,16),dk,0,0.012,-0.78,pv);fh.rotation.x=Math.PI/2;               // flash hider
  hpart(rbox(0.062,0.12,0.2,0.025),gold,0,-0.02,0.28,pv);hpart(rbox(0.064,0.13,0.03,0.012),dk,0,-0.02,0.395,pv);   // stock
  hpart(rbox(0.046,0.13,0.052,0.018),dk,0,-0.12,0.14,pv).rotation.x=0.25;                                      // grip
  hpart(rbox(0.052,0.2,0.074,0.02),gold3,0,-0.15,-0.06,pv).rotation.x=-0.15;                                  // magazine
  [1,-1].forEach(function(sx){const lg=hpart(rbox(0.006,0.052,0.052,0.014),wh,sx*0.041,-0.005,-0.1,pv);lg.rotation.x=0.785;const lg2=hpart(rbox(0.008,0.026,0.026,0.008),pe,sx*0.042,-0.005,-0.1,pv);lg2.rotation.x=0.785;}); // white square mark
  rivalsArms(pv,{x:-0.07,y:0.0,z:-0.28},{x:0.075,y:-0.09,z:0.14});
  mkFlash(pv,-0.88);
}
function buildVM(id){
  if(VM.g){camera.remove(VM.g);VM.g=null;}
  VM.id=id;VM.lh=null;VM.pv=null;VM.mflash=null;if(!id)return;
  const c=VMC[id]||VMC.rifle,hold=new THREE.Group(),sk=hmat(0xc9976a),sl=hmat(0x2f6f8f),sl2=hmat(0x25566f);
  if(id==='katana'){buildKatanaVM(hold,c,sk,sl,sl2);hold.position.set(c.p[0],c.p[1],c.p[2]);camera.add(hold);VM.g=hold;VM.hold=c;return;}
  if(GSPEC[id]){buildSpecVM(id,hold);hold.position.set(c.p[0],c.p[1],c.p[2]);camera.add(hold);VM.g=hold;VM.hold=c;return;}
  if(id==='rifle'){buildRifleVM(hold,c,sk);hold.position.set(c.p[0],c.p[1],c.p[2]);camera.add(hold);VM.g=hold;VM.hold=c;return;}
  const gm=gunModel(id),m=new THREE.Matrix4();
  if(c.rev)m.set(1,0,0,0, 0,0,1,0, 0,-1,0,0, 0,0,0,1); else m.set(-1,0,0,0, 0,0,1,0, 0,1,0,0, 0,0,0,1); // game weapon axes -> camera axes (barrel forward, sights up)
  const mount=new THREE.Group();mount.add(gm);gm.quaternion.setFromRotationMatrix(m);mount.scale.set(c.s,c.s,c.s);hold.add(mount);
  // right hand + forearm (always)
  hpart(hbox(0.085,0.085,0.1),sk,0,-0.02,0.02,hold);
  vmLimb(0.02,-0.03,0.05,0.2,-0.3,0.55,0.1,sl,hold);vmLimb(0.2,-0.3,0.55,0.34,-0.5,0.95,0.13,sl2,hold);
  if(c.lh!==null&&!c.melee){ // left hand supports the front of the gun
    const lx=c.lx===undefined?-0.02:c.lx,ly=c.ly===undefined?-0.075:c.ly;
    hpart(hbox(0.085,0.085,0.1),sk,lx,ly,c.lh,hold);
    vmLimb(lx,ly-0.015,c.lh+0.05,lx-0.24,-0.32,c.lh+0.55,0.1,sl,hold);vmLimb(lx-0.24,-0.32,c.lh+0.55,lx-0.38,-0.5,c.lh+0.95,0.13,sl2,hold);
  }
  hold.position.set(c.p[0],c.p[1],c.p[2]);camera.add(hold);VM.g=hold;VM.hold=c;
}
function updateVM(dt,show){
  if(!show||!WP.cur){if(VM.g)VM.g.visible=false;return;}
  if(VM.id!==WP.cur)buildVM(WP.cur);
  const g=VM.g,c=VM.hold;if(!g)return;g.visible=true;
  const aim=(WP.aim&&canAim(WP.cur)&&!c.melee)?1:0;VM.ads+=(aim-VM.ads)*Math.min(1,dt*14);
  VM.bob+=dt*Math.min(WK.sp,8)*1.6;const bs=Math.min(1,WK.sp/5)*(1-VM.ads*0.8);
  let x=c.p[0]+(c.a[0]-c.p[0])*VM.ads+Math.sin(VM.bob)*0.012*bs,y=c.p[1]+(c.a[1]-c.p[1])*VM.ads+Math.abs(Math.cos(VM.bob))*0.012*bs-WK.slAmt*0.12,z=c.p[2]+(c.a[2]-c.p[2])*VM.ads+WP.recoil*0.07;
  let rx=WP.recoil*0.06,ry=0,rz=0;
  if(c.melee){
    rx=c.rev?0.7:0.5;rz=0;if(c.kat){rx=0;rz=0;if(AS.defT>0){y+=0.05;rz=0.22;}} // katana: held level like Rivals, raised guard while deflecting
    if(c.nade){rx=0;if(WP.throwT>0){const s=Math.sin((1-WP.throwT/0.35)*Math.PI);rx=0.8*s;y+=0.18*s;z-=0.25*s;}}
    else if(WP.swingT>0){const dur=WP.cur==='knife'?0.2:WP.cur==='katana'?0.26:0.3,s=Math.sin((1-WP.swingT/dur)*Math.PI);rx-=1.1*s;rz=-0.9*s;x-=0.25*s;z-=0.12*s;}
    if(WK.dashT>0){x=0;y=-0.5;}
  }
  g.position.set(x,y,z);g.rotation.set(rx,ry,rz);
  if(VM.pv){const k=1-VM.ads,rf=VM.id==='rifle'; // rifle: big and angled across the screen like in Rivals, straight when aiming
    VM.pv.scale.setScalar(1+(rf?0.15:0)*k);VM.pv.rotation.set((rf?0.2:0.07)*k,(rf?0.42:0.2)*k,(rf?-0.22:0)*k);}
  if(VM.mflash)VM.mflash.visible=WP.recoil>0.6;
}
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
function matchShoulder(){return MATCH.on?(MATCH.fpv&&WK.on?0:((WP.aim&&WP.cur==='sniper')?0:(WP.aim?0.45:0.7))):0;} // how far right of you the camera (and the bullets) sit
function updateCameraFP(dt){ // over-the-shoulder camera like Rivals; the sniper scope switches to first person
  const dead=MATCH.state==='dead',scoped=WP.aim&&WP.cur==='sniper'&&!dead;
  const sx=Math.sin(WK.h),sz=Math.cos(WK.h),rx=-Math.cos(WK.h),rz=Math.sin(WK.h);
  const cp=Math.cos(MATCH.pitch),spn=Math.sin(MATCH.pitch);
  const dx=sx*cp,dy=spn,dz=sz*cp;
  let cam={x:0,z:0},cy;const fp=MATCH.fpv&&WK.on&&!dead;
  if(scoped||fp){cam.x=WK.x;cam.z=WK.z;cy=WK.y+1.62-0.8*WK.slAmt;}
  else{
    const dist=dead?6.5:(WP.aim?2.4:3.7),sh=dead?0:matchShoulder(),py=WK.y+(dead?1.2:1.65)-0.7*WK.slAmt;
    cam.x=WK.x-dx*dist+rx*sh;cam.z=WK.z-dz*dist+rz*sh;cy=py-dy*dist+(dead?2.5:0.35);
    pushBoxes(cam,0.35);
  }
  if(shake>0){cam.x+=rnd(-1,1)*shake*0.5;cy+=rnd(-1,1)*shake*0.35;cam.z+=rnd(-1,1)*shake*0.5;shake=Math.max(0,shake-dt*1.6);}
  cy=Math.max(cy,0.45);
  camera.position.set(cam.x,cy,cam.z);
  if(dead)camera.lookAt(WK.x,WK.y+1,WK.z);else camera.lookAt(cam.x+dx*30,cy+dy*30,cam.z+dz*30);
  WK.mesh.visible=!(scoped||fp);updateVM(dt,fp&&!scoped);
  const fovT=WP.aim?(scoped?20:Math.max(48,(WDEF[WP.cur]&&WDEF[WP.cur].zoom)||62)):(fp?75:70);
  if(Math.abs(camera.fov-fovT)>0.05){camera.fov+=(fovT-camera.fov)*Math.min(1,dt*12);camera.updateProjectionMatrix();}
  const sc=$('mscope');const want=scoped?'block':'none';if(sc.style.display!==want)sc.style.display=want;
}
// --- per-frame match logic + HUD ---
function wIcon(id){ // little weapon silhouette for the HUD slot
  const f='#f2f2f2';let b;
  if(id==='katana')b='<polygon points="4,14 52,9 58,11 52,13" fill="'+f+'"/><rect x="2" y="12" width="12" height="4" fill="#aaa"/>';
  else if(id==='knife')b='<polygon points="8,12 40,6 54,10 40,14" fill="'+f+'"/><rect x="4" y="10" width="10" height="5" fill="#aaa"/>';
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
  const tsc=teamScores(),best=tsc[1];
  const wid=WK.on?WP.cur:null,curAmmo=!wid?'':(wid==='grenade'?save.wp.nades:(MELEE.indexOf(wid)>=0?'∞':save.wp.ammo[wid])),magTxt=(wid&&GUNS.concat(LAUNCHERS).indexOf(wid)>=0)?'/'+WDEF[wid].mag:'',ammo=curAmmo+magTxt;
  const secs=Math.ceil(MATCH.time),cn=MATCH.state==='count'?Math.max(0,Math.ceil(MATCH.t-0.6)):-1;
  const key=[Math.ceil(MATCH.hp),MATCH.kills,tsc[0],best,secs,MATCH.state,wid,ammo,cn,Math.ceil(MATCH.t)].join('|');
  if(key!==MATCH.hudKey){
    MATCH.hudKey=key;
    $('msYou').textContent=tsc[0];$('msBot').textContent=best;$('msYouL').textContent=MATCH.mode==='tdm'?'TEAM':'YOU';$('msBotL').textContent=MATCH.mode==='tdm'?'ENEMY':'BOTS';
    $('mTime').textContent=isFinite(secs)?Math.floor(secs/60)+':'+('0'+(secs%60)).slice(-2):'\u221E';
    $('mGoal').textContent=MATCH.mode==='gungame'?'LEVEL '+Math.min(MATCH.kills+1,LADDER.length)+'/'+LADDER.length:'FIRST TO '+MATCH.target;
    const hp=Math.ceil(MATCH.hp);$('mhpTxt').textContent=hp;$('mhpBar').style.width=hp+'%';$('mhpBar').className=hp<35?'low':'';
    $('mwName').textContent=wid?WNAMES[wid].toUpperCase():'';$('mwAmmo').textContent=wid?curAmmo:'';$('mwMag').textContent=magTxt;
    const li=MATCH.mode==='melee'?MELEEL.indexOf(wid):ownedList().indexOf(wid);$('mslotN').textContent=li+1;$('mslotIcon').innerHTML=wid?wIcon(wid):'';
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
window.addEventListener('mousemove',function(e){
  if(MATCH.on&&MATCH.state==='live'){const lk=document.pointerLockElement===renderer.domElement;lookBy(e.movementX||0,e.movementY||0,lk?0.0024:0.0032);}});
// free look outside matches: works on window so no overlay can swallow it; mouse drag / pointer lock / trackpad / touch drag
function freeLook(dx,dy,sens){
  if(MATCH.on||!started||shopOpen||talkOpen||LO.open||kicked||LB.open||GD.open)return;
  if(WK.on)lookBy(dx,dy,sens);
  else{ORB.yaw-=dx*sens*2.4;ORB.pitch=clamp(ORB.pitch+dy*sens*2,-0.25,1.1);ORB.t=performance.now();} // vehicle: orbit the camera all the way round
}
window.addEventListener('mousemove',function(e){
  if(e.target&&e.target!==renderer.domElement&&document.pointerLockElement!==renderer.domElement)return;
  const lk=document.pointerLockElement===renderer.domElement;
  if(lk||(e.buttons&1))freeLook(e.movementX||0,e.movementY||0,lk?0.0024:0.0035);
});
const FL={id:-1,x:0,y:0};
window.addEventListener('pointerdown',function(e){if(e.pointerType==='touch'&&e.target===renderer.domElement&&FL.id<0){FL.id=e.pointerId;FL.x=e.clientX;FL.y=e.clientY;}});
window.addEventListener('pointermove',function(e){if(e.pointerId!==FL.id)return;freeLook(e.clientX-FL.x,e.clientY-FL.y,0.0055);FL.x=e.clientX;FL.y=e.clientY;});
const endFL=function(e){if(e.pointerId===FL.id)FL.id=-1;};window.addEventListener('pointerup',endFL);window.addEventListener('pointercancel',endFL);
renderer.domElement.addEventListener('pointerdown',function(e){
  if(e.pointerType!=='touch'||!MATCH.on)return;e.preventDefault();if(MATCH.lookId<0)MATCH.lookId=e.pointerId,MATCH.lx=e.clientX,MATCH.ly=e.clientY;
});
window.addEventListener('pointermove',function(e){
  if(e.pointerId!==MATCH.lookId||!MATCH.on)return;
  if(MATCH.state==='live')lookBy(e.clientX-MATCH.lx,e.clientY-MATCH.ly,0.0055);MATCH.lx=e.clientX;MATCH.ly=e.clientY;
});
const endLook=function(e){if(e.pointerId===MATCH.lookId)MATCH.lookId=-1;};
window.addEventListener('pointerup',endLook);window.addEventListener('pointercancel',endLook);
renderer.domElement.addEventListener('mousedown',function(){if(((MATCH.on&&MATCH.state!=='over')||(!MATCH.on&&WK.on&&started&&!shopOpen))&&document.pointerLockElement!==renderer.domElement){try{renderer.domElement.requestPointerLock();}catch(e){}}});
$('lbPlay').addEventListener('click',lobbyPlay);$('lbClose').addEventListener('click',closeLobby);
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
  if(MATCH.on){const sk=(MAPS[ARENA.cur]||MAPS.streets).sky;scene.background.setRGB(sk[0],sk[1],sk[2]);scene.fog.color.setRGB(sk[0],sk[1],sk[2]);scene.fog.density=0.0035;}
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
    for(let n=1;n<=5;n++)(function(n){btn(n+'v'+n,'gray',function(){challenge(id,n);});})(n);
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
  ADM.unlocked=false;ADM.god=false;ADM.noclip=false;ADM.speed=1;ADM.del=false;ADM.open=false;ADM.mtime=180;$('admMTime').value=3;$('admMTime').disabled=false;$('admMInf').checked=false;
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
  const mtSync=function(){const inf=$('admMInf').checked;$('admMTime').disabled=inf;const m=+$('admMTime').value;ADM.mtime=ADM.unlocked?(inf?Infinity:(m>0?Math.min(m,999)*60:180)):180;};
  $('admMInf').addEventListener('change',function(){mtSync();if(this.blur)this.blur();toast(ADM.mtime===Infinity?'Matches: infinite time (next round)':'Match time set (next round)',1500);});
  $('admMTime').addEventListener('input',mtSync);$('admMTime').addEventListener('change',function(){mtSync();toast('Match time: '+(ADM.mtime/60)+' min (next round)',1500);});
  on('admMApply',function(){mtSync();if(MATCH.on&&MATCH.state!=='over'&&!MATCH.online){MATCH.time=ADM.mtime;MATCH.hudKey='';toast(ADM.mtime===Infinity?'Time limit removed':'Timer set to '+(ADM.mtime/60)+' min',1600);}else toast('Applies when a match starts',1500);});
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
function btnOf(t){while(t&&t.dataset&&t.dataset.k===undefined&&t.dataset.fs===undefined&&t.dataset.gd===undefined)t=t.parentNode;return(t&&t.dataset&&(t.dataset.k!==undefined||t.dataset.fs!==undefined||t.dataset.gd!==undefined))?t:null;}
function enableTouch(){
  if(TOUCH_ON)return;TOUCH_ON=true;document.body.className='touch';
  const root=$('touch');
  $('joy').addEventListener('pointerdown',function(e){e.preventDefault();TJ.id=e.pointerId;joyMove(e);});
  window.addEventListener('pointermove',function(e){if(e.pointerId===TJ.id)joyMove(e);});
  const endJ=function(e){if(e.pointerId===TJ.id){TJ.id=-1;setJoy(0,0);}};
  window.addEventListener('pointerup',endJ);window.addEventListener('pointercancel',endJ);
  root.addEventListener('pointerdown',function(e){
    const b=btnOf(e.target);if(!b)return;e.preventDefault();
    if(b.dataset.gd!==undefined){openGuide();return;}
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
  const hide=!started||shopOpen||talkOpen||LO.open||kicked||LB.open||GD.open||(MATCH.on&&MATCH.state==='over');
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
window.__dcs={net:{NET:NET,CH:CH,challenge:challenge,recv:netRecv},slide:{start:startSlide,SLIDE_T:SLIDE_T},match:{VM:VM,teamScores:teamScores,LADDER:LADDER,botHit:botHit,MATCH:MATCH,ARENA:ARENA,start:startMatch,leave:leaveMatch,open:openLobby,hurt:hurtPlayer,end:endMatch,lobby:LB,matchKill:matchKill,projs:projs,autoSlot:autoSlot},touch:{enable:enableTouch,setJoy:setJoy,fireKey:fireKey,TJ:TJ,on:function(){return TOUCH_ON;}},weap:{LO:LO,setSlot:setSlot,toggleLoadout:toggleLoadout,ownedList:ownedList,WP:WP,WDEF:WDEF,owns:owns,cycle:cycleWeapon,tryFire:tryFire,hurt:hurtPerson,startDash:startDash,nades:nades,explode:explodeNade,throwGrenade:throwGrenade,scytheHit:scytheHit,swing:swingScythe},moon:{MOON:MOON,SPACE:SPACE,start:startMoonTrip,back:returnToEarth},ADM:ADM,admin:{massRemove:massRemove,removeDesc:removeDesc,undo:undoLast,onMsg:onAdminMsg,unlock:unlockAdmin,lock:lockAdmin,toggle:toggleAdmin,teleport:teleport,giveSelf:giveSelf,undoCount:function(){return undoStack.length;},others:others,myId:myId},trees:trees,statics:statics,people:people,keepers:keepers,talk:{list:showList,bye:function(){closeShop(true);},open:function(){return talkOpen;}},shopPts:shopPts,SHOP:SHOP,WK:WK,crowd:crowd,HPS:HPS,toggleOnFoot:toggleOnFoot,P:P,rockets:rockets,ais:ais,buildings:buildings,obstacles:obstacles,coins:coins,explode:explode,launchRocket:launchRocket,registerImpact:registerImpact,save:save};
})();
</script>
</body>
</html>
