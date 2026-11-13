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
#cross{transform:scale(var(--chs,1))}
#cross::before,#cross::after{content:'';position:absolute;background:var(--chc,#fff);box-shadow:0 0 3px #000}
#cross[data-st=dot]::before,#cross[data-st=dot]::after,#cross[data-st=circle]::after,#cross[data-st=none]::before,#cross[data-st=none]::after,#cross[data-st=none] i{display:none}
#cross[data-st=circle]::before{width:22px;height:22px;left:-11px;top:-11px;background:none;border:2px solid var(--chc,#fff);border-radius:50%}
#cross[data-st=dot] i{background:var(--chc,#fff)}
#rwBody .trow{display:flex;align-items:center;gap:10px;margin:6px 0;background:#2a1850;border-radius:10px;padding:8px 10px;text-align:left;font-size:13px}#rwBody .trow .tt{flex:1}#rwBody button,#rwTabs button{padding:6px 10px;font-size:12px;margin:2px}#rwTabs button.sel{border-color:#ffb020;background:#5a2fa0}
#rwBody .rar{font-weight:800}#rwBody .me{background:#5a2fa0}#rwBody .big{font-size:18px;text-align:center;padding:16px;justify-content:center}
#settings label{display:flex;align-items:center;justify-content:space-between;gap:10px;margin:8px 0;font-size:14px;text-align:left}#settings input[type=range]{width:55%}
#cross::before{width:2px;height:18px;left:-1px;top:-9px}
#cross::after{width:18px;height:2px;left:-9px;top:-1px}
