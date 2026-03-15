<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>🌌 NEBULA POP — Galactic Overlord</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@400;500;700&display=swap" rel="stylesheet">
<style>
:root{--neon:#00d2ff;--neon2:#3a7bd5;--bg:#02020a;--panel:rgba(3,8,24,0.96);--red:#ff004c;--gold:#ffd700;--green:#00ff88;--purple:#b44fff;--orange:#ff8c00;--pink:#ff6eb4}
*{box-sizing:border-box;margin:0;padding:0}
html,body{width:100%;height:100%;overflow:hidden;background:var(--bg);font-family:'Rajdhani',sans-serif;color:#fff;user-select:none;cursor:crosshair}
canvas#bg{position:fixed;inset:0;z-index:0;pointer-events:none}
::-webkit-scrollbar{width:3px}::-webkit-scrollbar-track{background:transparent}::-webkit-scrollbar-thumb{background:rgba(0,210,255,.2);border-radius:2px}
#layout{position:fixed;inset:0;display:grid;grid-template-columns:268px 1fr 256px;z-index:10;pointer-events:none}
#layout>*{pointer-events:auto}
#left-col,#right-col{display:flex;flex-direction:column;gap:7px;padding:10px;overflow:hidden}
#center-col{position:relative;pointer-events:none}
.pnl{background:var(--panel);border-radius:11px;backdrop-filter:blur(16px);border:1px solid rgba(255,255,255,.07);overflow:hidden;flex-shrink:0}
.pi{padding:10px 12px}
.pt{font-family:'Orbitron',sans-serif;font-size:8px;letter-spacing:2px;color:#3a3a3a;margin-bottom:7px;display:flex;justify-content:space-between;align-items:center}
.sep{border:0;border-top:1px solid rgba(255,255,255,.06);margin:7px 0}
#hud-pnl{border-color:rgba(0,210,255,.3);box-shadow:0 0 18px rgba(0,210,255,.07)}
.hud-name{font-family:'Orbitron',sans-serif;font-size:10px;color:var(--neon);cursor:pointer;display:flex;align-items:center;gap:5px;margin-bottom:2px}
.hud-score{font-family:'Orbitron',sans-serif;font-size:28px;font-weight:900;background:linear-gradient(90deg,var(--neon),#fff);-webkit-background-clip:text;-webkit-text-fill-color:transparent;line-height:1.1}
.hud-score em{-webkit-text-fill-color:#888;font-style:normal;font-size:14px;margin-right:3px}
.hud-grid{display:grid;grid-template-columns:1fr 1fr;gap:2px 8px;font-size:10px;color:#444;margin:5px 0}
.hud-grid .v{color:var(--neon);font-weight:700}
.pi-row{display:flex;justify-content:space-between;font-size:10px;color:#444}
.pi-row .g{color:var(--gold)}
.pb{display:flex;gap:2px;margin:4px 0}
.pb-seg{flex:1;height:12px;border-radius:3px;border:1px solid rgba(255,255,255,.08);background:rgba(255,255,255,.02);display:flex;align-items:center;justify-content:center;font-size:6px;letter-spacing:.5px;font-family:'Orbitron',sans-serif;color:#2a2a2a;transition:all .3s}
.pb-seg.act{background:linear-gradient(135deg,rgba(255,0,76,.25),rgba(255,140,0,.25));border-color:var(--orange);color:var(--orange)}
.pb-seg.dn{background:linear-gradient(135deg,rgba(255,215,0,.15),rgba(0,210,255,.15));border-color:var(--gold);color:var(--gold)}
#ach-pnl{flex:0 0 175px;border-color:rgba(255,215,0,.16)}
#ach-list{overflow-y:auto;max-height:140px;scrollbar-width:thin;scrollbar-color:rgba(255,215,0,.12) transparent}
.ach-row{display:flex;align-items:center;gap:5px;padding:3px 0;border-bottom:1px solid rgba(255,255,255,.04);font-size:10px}
.ach-ico{font-size:12px;width:16px;text-align:center;flex-shrink:0}
.ach-name{font-weight:700;font-size:10px}
.ach-sub{font-size:8px;color:#3a3a3a}
.ach-row.locked{opacity:.18;filter:grayscale(1)}
.ach-row.done .ach-name{color:var(--gold)}
#upg-pnl{flex:1;display:flex;flex-direction:column;min-height:0}
#upg-scroll{overflow-y:auto;flex:1;padding:0 1px}
.ucat{font-family:'Orbitron',sans-serif;font-size:7px;letter-spacing:2px;color:#252525;padding:5px 2px 2px}
.urow{display:flex;align-items:center;gap:7px;padding:5px 7px;margin-bottom:3px;border-radius:7px;border:1px solid rgba(255,255,255,.05);background:rgba(255,255,255,.02);cursor:pointer;transition:all .12s}
.urow:hover:not(.dim){background:rgba(0,210,255,.06);border-color:rgba(0,210,255,.22)}
.urow.dim{opacity:.22;cursor:not-allowed}
.u-ico{font-size:16px;width:20px;text-align:center;flex-shrink:0}
.u-info{flex:1;min-width:0}
.u-name{font-size:10px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.u-desc{font-size:8px;color:#444}
.u-r{text-align:right;flex-shrink:0}
.u-lvl{font-size:7px;color:var(--green)}
.u-cost{font-family:'Orbitron',sans-serif;font-size:8px;color:var(--gold)}
#lb-pnl{flex-shrink:0;border-color:rgba(255,215,0,.2)}
.lb-row{display:flex;align-items:center;gap:6px;padding:4px 2px;border-bottom:1px solid rgba(255,255,255,.04);font-size:10px}
.lb-rank{font-family:'Orbitron',sans-serif;font-size:8px;width:16px;text-align:right;color:#3a3a3a;flex-shrink:0}
.lb-rank.r1{color:var(--gold)}.lb-rank.r2{color:#c0c0c0}.lb-rank.r3{color:#cd7f32}
.lb-name{flex:1;font-weight:700;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.lb-name.me{color:var(--neon)}
.lb-score{font-family:'Orbitron',sans-serif;font-size:8px;color:#555}
.online-dot{width:5px;height:5px;border-radius:50%;background:var(--green);display:inline-block;margin-right:3px;animation:pulse 1.5s infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.3}}
#mp-pnl{flex:0 0 auto;border-color:rgba(0,255,136,.15)}
.mp-feed{font-size:9px;color:#444;overflow-y:auto;max-height:90px;scrollbar-width:thin}
.mp-entry{padding:2px 0;border-bottom:1px solid rgba(255,255,255,.03);display:flex;gap:5px;align-items:flex-start}
.mp-entry.me-ev{color:rgba(0,210,255,.6)}.mp-entry.boss-ev{color:rgba(255,0,76,.5)}.mp-entry.pres-ev{color:rgba(255,215,0,.6)}
.mp-ts{color:#2a2a2a;flex-shrink:0;font-size:8px;margin-top:1px}
.mp-msg{flex:1}
#gift-pnl{flex:0 0 auto;border-color:rgba(0,255,136,.18)}
.gift-days{display:flex;gap:3px;margin-bottom:6px}
.gd{flex:1;height:26px;border-radius:5px;border:1px solid rgba(255,255,255,.07);background:rgba(255,255,255,.02);display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:9px;transition:all .3s}
.gd.done{background:rgba(0,255,136,.1);border-color:var(--green)}
.gd.today{background:rgba(0,210,255,.12);border-color:var(--neon);animation:glow .8s infinite alternate}
@keyframes glow{from{box-shadow:0 0 4px rgba(0,210,255,.3)}to{box-shadow:0 0 12px rgba(0,210,255,.7)}}
.gd-lbl{font-size:6px;color:#333;font-family:'Orbitron',sans-serif}
#pets-pnl{flex:0 0 auto;border-color:rgba(180,79,255,.18)}
.egg-row{display:flex;gap:4px;margin-bottom:5px}
.egg-btn{flex:1;padding:5px 3px;border:none;border-radius:6px;cursor:pointer;font-family:'Rajdhani',sans-serif;font-size:10px;font-weight:700;color:#fff;display:flex;flex-direction:column;align-items:center;gap:1px;transition:all .14s}
.egg-btn:hover{filter:brightness(1.2);transform:translateY(-1px)}
.egg-btn:disabled{opacity:.28;cursor:not-allowed;transform:none;filter:none}
.egg-btn.b1{background:linear-gradient(135deg,var(--neon),var(--neon2))}
.egg-btn.b2{background:linear-gradient(135deg,#7b2ff7,var(--purple))}
.egg-btn.b3{background:linear-gradient(135deg,#b8860b,var(--gold));color:#111}
.egg-cost{font-size:7px;opacity:.8;font-family:'Orbitron',sans-serif}
.pet-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:2px}
.ps{width:29px;height:29px;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.07);border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:14px;position:relative}
.ps .pb2{position:absolute;bottom:-2px;right:-2px;font-size:6px;background:#000;border-radius:2px;padding:0 1px;line-height:11px;color:var(--gold)}
#skin-pnl{flex:0 0 auto;border-color:rgba(255,110,180,.13)}
.sk-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;margin-bottom:5px}
.sk{width:29px;height:29px;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.07);border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:14px;cursor:pointer;transition:all .11s}
.sk:hover{border-color:var(--neon);background:rgba(0,210,255,.08)}
.sk.act{border-color:var(--neon);box-shadow:0 0 7px rgba(0,210,255,.35);background:rgba(0,210,255,.1)}
.buy-row{display:flex;gap:4px}
.btn{border:none;color:#fff;padding:6px 10px;border-radius:7px;cursor:pointer;font-weight:700;font-family:'Rajdhani',sans-serif;font-size:11px;letter-spacing:1px;transition:all .13s;width:100%}
.btn:hover{filter:brightness(1.15);transform:translateY(-1px)}
.btn:active{transform:translateY(0)}
.btn:disabled{opacity:.26;cursor:not-allowed;transform:none;filter:none}
.btn-n{background:linear-gradient(135deg,var(--neon),var(--neon2))}
.btn-p{background:linear-gradient(135deg,#ff004c,#ff8c00);box-shadow:0 0 12px rgba(255,0,76,.25)}
.btn-p:hover:not(:disabled){box-shadow:0 0 24px rgba(255,0,76,.5)}
.btn-g{background:linear-gradient(135deg,#00c853,#00695c)}
.btn-pk{background:linear-gradient(135deg,#c2185b,var(--pink))}
.btn-sm{padding:4px 7px;font-size:10px}
#center-hud{position:absolute;top:10px;left:50%;transform:translateX(-50%);z-index:20;display:flex;flex-direction:column;align-items:center;gap:4px;pointer-events:none;min-width:180px}
#combo-txt{font-family:'Orbitron',sans-serif;font-size:22px;font-weight:900;color:var(--gold);text-shadow:0 0 14px var(--gold);opacity:0;min-height:28px;text-align:center}
#combo-txt.on{opacity:1;animation:cpop .25s ease-out}
@keyframes cpop{0%{transform:scale(1.6)}100%{transform:scale(1)}}
#fever-wrap{display:flex;flex-direction:column;align-items:center;gap:2px}
#fever-label{font-family:'Orbitron',sans-serif;font-size:7px;letter-spacing:2px;color:var(--orange)}
#fever-track{width:140px;height:5px;background:rgba(255,255,255,.06);border-radius:3px;overflow:hidden;border:1px solid rgba(255,140,0,.18)}
#fever-fill{height:100%;width:0%;background:linear-gradient(90deg,#ff6a00,#ffd700);border-radius:3px;transition:width .12s}
#fever-on{font-family:'Orbitron',sans-serif;font-size:11px;color:var(--orange);text-shadow:0 0 8px var(--orange);opacity:0;min-height:15px}
#fever-on.on{opacity:1;animation:fpop .3s ease-out}
@keyframes fpop{0%{transform:scale(1.3)}100%{transform:scale(1)}}
#ev-chip{background:rgba(255,215,0,.09);border:1px solid var(--gold);border-radius:16px;padding:3px 12px;font-family:'Orbitron',sans-serif;font-size:8px;color:var(--gold);display:none;white-space:nowrap;animation:chipG .8s infinite alternate}
@keyframes chipG{from{box-shadow:0 0 5px rgba(255,215,0,.2)}to{box-shadow:0 0 16px rgba(255,215,0,.5)}}
#boss-zone{position:absolute;left:50%;top:50%;transform:translate(-50%,-52%);display:none;flex-direction:column;align-items:center;pointer-events:auto}
#boss-spr{font-size:100px;cursor:crosshair;filter:drop-shadow(0 0 24px #fff);animation:bflt 3s ease-in-out infinite;transition:transform .05s}
@keyframes bflt{0%,100%{transform:translateY(0)}50%{transform:translateY(-11px)}}
#boss-spr:hover{filter:drop-shadow(0 0 40px var(--red))}
#boss-spr:active{transform:scale(.85)!important}
#boss-nm{font-family:'Orbitron',sans-serif;font-size:15px;font-weight:900;letter-spacing:2px;color:var(--red);text-shadow:0 0 11px var(--red);margin-top:9px;text-transform:uppercase}
#boss-sub{font-size:10px;color:#444;margin-top:2px}
.hp-wrap{width:290px;margin-top:7px}
.hp-track{width:100%;height:12px;background:rgba(255,255,255,.06);border-radius:6px;overflow:hidden;border:1px solid rgba(255,0,76,.28)}
#hp-fill{height:100%;background:linear-gradient(90deg,#ff004c,#ff6a00,#ffd700);background-size:200%;animation:hpsh 2s linear infinite;transition:width .1s;border-radius:6px}
@keyframes hpsh{0%{background-position:0%}100%{background-position:200%}}
.hp-info{display:flex;justify-content:space-between;font-size:8px;color:#444;margin-top:2px;font-family:'Orbitron',sans-serif}
#boss-enrage{font-size:9px;color:var(--orange);text-align:center;margin-top:3px;font-weight:700;min-height:13px}
#boss-timer{position:absolute;bottom:8px;left:50%;transform:translateX(-50%);background:var(--panel);padding:7px 18px;border-radius:9px;border:1px solid rgba(255,0,76,.2);display:flex;align-items:center;gap:12px;pointer-events:none;backdrop-filter:blur(10px)}
.bt-time{font-family:'Orbitron',sans-serif;font-size:18px;color:var(--red);text-shadow:0 0 8px var(--red)}
.bt-div{width:1px;height:24px;background:rgba(255,255,255,.07)}
.bt-info{font-size:9px;color:#3a3a3a;text-align:center}
.bt-info b{color:var(--orange);font-family:'Orbitron',sans-serif}
#boss-timer.imm{animation:timP .5s infinite alternate}
@keyframes timP{from{box-shadow:0 0 7px rgba(255,0,76,.3)}to{box-shadow:0 0 24px rgba(255,0,76,.8)}}
#boss-intro{position:fixed;inset:0;z-index:500;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.87);backdrop-filter:blur(4px)}
#boss-intro.on{display:flex;animation:fi .3s}
@keyframes fi{from{opacity:0}to{opacity:1}}
.bi-card{text-align:center;animation:biS .4s cubic-bezier(.175,.885,.32,1.275)}
@keyframes biS{from{transform:scale(.4);opacity:0}to{transform:scale(1);opacity:1}}
.bi-warn{font-family:'Orbitron',sans-serif;font-size:11px;letter-spacing:4px;color:var(--red);margin-bottom:10px;animation:blnk .5s infinite alternate}
@keyframes blnk{from{opacity:1}to{opacity:.2}}
.bi-ico{font-size:80px;display:block;margin:7px 0;filter:drop-shadow(0 0 32px var(--red))}
.bi-name{font-family:'Orbitron',sans-serif;font-size:20px;font-weight:900;color:#fff;text-shadow:0 0 16px #fff}
.bi-sub{font-size:11px;color:#555;margin-top:4px}
.modal{position:fixed;inset:0;z-index:800;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.8);backdrop-filter:blur(5px)}
.modal.on{display:flex;animation:fi .25s}
.mbox{background:var(--panel);border-radius:14px;padding:24px 28px;border:1px solid rgba(0,210,255,.25);box-shadow:0 0 32px rgba(0,210,255,.08);min-width:300px;max-width:460px;text-align:center;animation:biS .3s cubic-bezier(.175,.885,.32,1.275);position:relative}
.mclose{position:absolute;top:12px;right:14px;font-size:16px;cursor:pointer;color:#333;transition:color .14s}
.mclose:hover{color:#fff}
.m-title{font-family:'Orbitron',sans-serif;font-size:14px;font-weight:900;margin-bottom:10px}
.m-input{width:100%;background:rgba(255,255,255,.06);border:1px solid rgba(0,210,255,.28);border-radius:7px;padding:9px 12px;color:#fff;font-family:'Orbitron',sans-serif;font-size:13px;outline:none;text-align:center;margin:8px 0}
.m-input:focus{border-color:var(--neon);box-shadow:0 0 8px rgba(0,210,255,.18)}
.pm-opt{padding:9px 12px;border-radius:8px;border:1px solid rgba(255,255,255,.08);background:rgba(255,255,255,.03);cursor:pointer;transition:all .13s;text-align:left;display:flex;align-items:center;gap:10px;margin-bottom:6px}
.pm-opt:hover:not(.dim){background:rgba(255,140,0,.08);border-color:var(--orange)}
.pm-opt.dim{opacity:.27;cursor:not-allowed}
.pm-ico{font-size:24px}
.pm-nm{font-weight:700;font-size:12px}
.pm-d{font-size:9px;color:#555}
.pm-c{font-family:'Orbitron',sans-serif;font-size:8px;color:var(--gold);margin-top:2px}
.pm-meta-div{border-top:1px solid rgba(180,79,255,.25);margin:8px 0;padding-top:6px;font-family:'Orbitron',sans-serif;font-size:7px;color:var(--purple);letter-spacing:2px;text-align:center}
#ev-modal .mbox{border-color:var(--gold);box-shadow:0 0 32px rgba(255,215,0,.12)}
.ev-tag{font-family:'Orbitron',sans-serif;font-size:8px;letter-spacing:3px;color:var(--gold);margin-bottom:5px}
.ev-ico2{font-size:44px;display:block;margin:5px 0}
.ev-nm{font-family:'Orbitron',sans-serif;font-size:18px;font-weight:900;margin-bottom:4px}
.ev-desc2{font-size:12px;color:#888;margin-bottom:14px}
#gift-modal .mbox{border-color:var(--green);box-shadow:0 0 32px rgba(0,255,136,.1)}
.gift-big{font-size:54px;display:block;margin:8px 0;animation:gbounce .6s cubic-bezier(.175,.885,.32,1.275)}
@keyframes gbounce{0%{transform:scale(0) rotate(-20deg)}100%{transform:scale(1) rotate(0)}}
.ftxt{position:fixed;pointer-events:none;font-family:'Rajdhani',sans-serif;font-weight:700;font-size:14px;z-index:999;animation:fup .9s ease-out forwards}
@keyframes fup{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-65px) scale(.5)}}
.ftxt.big{font-size:20px}
.rp{position:fixed;pointer-events:none;border-radius:50%;border:2px solid rgba(0,210,255,.65);animation:rexp .4s ease-out forwards;z-index:998}
@keyframes rexp{0%{width:8px;height:8px;opacity:.8;transform:translate(-50%,-50%)}100%{width:65px;height:65px;opacity:0;transform:translate(-50%,-50%)}}
.bubble{position:fixed;cursor:pointer;z-index:9;animation:bpop .12s ease-out}
.bubble:hover{transform:scale(1.2)!important}.bubble:active{transform:scale(.84)!important}
@keyframes bpop{from{opacity:0;transform:scale(.3)}to{opacity:1;transform:scale(1)}}
.b-g{filter:drop-shadow(0 0 11px var(--gold))!important;animation:bpop .12s ease-out,bobG .6s ease-in-out infinite alternate}
@keyframes bobG{from{transform:scale(1) rotate(-4deg)}to{transform:scale(1.1) rotate(4deg)}}
.b-p{filter:drop-shadow(0 0 11px var(--purple))!important;animation:bpop .12s ease-out,bobP .5s ease-in-out infinite alternate}
@keyframes bobP{from{transform:scale(1) rotate(3deg)}to{transform:scale(1.07) rotate(-3deg)}}
.b-r{filter:drop-shadow(0 0 11px var(--red))!important;animation:bpop .12s ease-out,bobR .4s ease-in-out infinite alternate}
@keyframes bobR{from{transform:scale(1)}to{transform:scale(1.13)}}
.b-pt{filter:drop-shadow(0 0 11px var(--green))!important;animation:bpop .12s ease-out,bobPt .45s ease-in-out infinite alternate}
@keyframes bobPt{from{transform:scale(1)}to{transform:scale(1.1) rotate(5deg)}}
.pet-minion{position:fixed;font-size:18px;pointer-events:none;z-index:15;transition:all .7s cubic-bezier(.4,0,.2,1);filter:drop-shadow(0 0 5px var(--green))}
#pflash{position:fixed;inset:0;z-index:1000;pointer-events:none;opacity:0}
#pflash.fire{background:radial-gradient(ellipse,rgba(255,140,0,.85),transparent);animation:pflA 1.2s ease-out forwards}
#pflash.void{background:radial-gradient(ellipse,rgba(180,79,255,.9),transparent);animation:pflA 1.4s ease-out forwards}
@keyframes pflA{0%{opacity:1}100%{opacity:0}}
@keyframes shk{0%,100%{transform:translate(0,0)}20%{transform:translate(-5px,2px)}40%{transform:translate(5px,-3px)}60%{transform:translate(-3px,4px)}80%{transform:translate(4px,-2px)}}
body.shk{animation:shk .18s ease-out}
#toasts{position:fixed;bottom:55px;left:50%;transform:translateX(-50%);z-index:999;display:flex;flex-direction:column;align-items:center;gap:4px;pointer-events:none}
.toast{background:linear-gradient(135deg,rgba(255,215,0,.12),rgba(255,215,0,.03));border:1px solid rgba(255,215,0,.35);border-radius:8px;padding:6px 12px;display:flex;align-items:center;gap:7px;font-size:10px;white-space:nowrap;animation:tin .32s cubic-bezier(.175,.885,.32,1.275) forwards}
.toast.out{animation:tout .28s ease-in forwards}
@keyframes tin{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}
@keyframes tout{from{opacity:1;transform:translateY(0)}to{opacity:0;transform:translateY(-8px)}}
.t-i{font-size:14px}.t-tl{font-family:'Orbitron',sans-serif;font-size:7px;color:var(--gold);letter-spacing:1px}.t-m{font-weight:700;font-size:10px}
#online-badge{position:fixed;top:10px;left:50%;transform:translateX(-50%);z-index:102;background:rgba(0,0,0,.7);border:1px solid rgba(0,255,136,.3);border-radius:14px;padding:3px 12px;font-family:'Orbitron',sans-serif;font-size:8px;color:var(--green);display:flex;align-items:center;gap:5px;backdrop-filter:blur(6px)}
</style>
</head>
<body>
<canvas id="bg"></canvas>
<div id="pflash"></div>
<div id="online-badge"><span class="online-dot"></span><span id="online-count">Connexion en cours...</span></div>
<div id="layout">
<div id="left-col">
  <div class="pnl" id="hud-pnl"><div class="pi"><div class="hud-name" onclick="openModal('name-modal')"><span id="pname">🚀 Choisir un pseudo</span><span style="font-size:8px;color:#2a2a2a">✏️</span></div><div class="hud-score"><em>✨</em><span id="score">0</span></div><div class="hud-grid"><span>×<span id="mult" class="v">1.00</span></span><span>⚡<span id="dps" class="v">0</span>/s</span><span>🖱️<span id="cps" class="v">0</span>/s</span><span>🐾<span id="pet-dps" class="v">0</span>/s</span></div><div class="sep"></div><div class="pi-row"><span>Prestige <span class="g" id="p-lvl">0</span></span><span>Méta <span class="g" id="mp-lvl" style="color:var(--purple)">0</span></span><span style="font-size:9px">Coût <span class="g" id="p-cost">5K</span></span></div><div class="pb" id="pb"></div><button class="btn btn-p" id="p-btn" onclick="openPrestigeModal()" disabled style="margin-top:5px">💥 PRESTIGE</button></div></div>
  <div class="pnl" id="ach-pnl"><div class="pi" style="height:100%;display:flex;flex-direction:column"><div class="pt">🏅 SUCCÈS <span id="ach-cnt">0/0</span></div><div id="ach-list"></div></div></div>
  <div class="pnl" id="upg-pnl"><div class="pi" style="height:100%;display:flex;flex-direction:column;padding-bottom:5px"><div class="pt">⚡ AMÉLIORATIONS</div><div id="upg-scroll"></div></div></div>
</div>
<div id="center-col">
  <div id="center-hud"><div id="combo-txt"></div><div id="fever-wrap"><div id="fever-label">FEVER METER</div><div id="fever-track"><div id="fever-fill"></div></div><div id="fever-on">🔥 FEVER ×3!</div></div><div id="ev-chip"></div></div>
  <div id="boss-zone"><div id="boss-spr">👾</div><div id="boss-nm">—</div><div id="boss-sub">—</div><div class="hp-wrap"><div class="hp-track"><div id="hp-fill"></div></div><div class="hp-info"><span id="hp-txt">0/0</span><span id="hp-pct">100%</span></div></div><div id="boss-enrage"></div></div>
  <div id="boss-timer"><div><div style="font-size:7px;letter-spacing:1px;color:#2a2a2a;margin-bottom:1px">BOSS DANS</div><div class="bt-time" id="bcd">5:00</div></div><div class="bt-div"></div><div class="bt-info">VAGUE<br><b id="b-wave">1</b></div><div class="bt-div"></div><div class="bt-info">TUÉS<br><b id="b-kills" style="color:var(--red)">0</b></div></div>
</div>
<div id="right-col">
  <div class="pnl" id="lb-pnl" style="flex-shrink:0"><div class="pi"><div class="pt" style="color:var(--gold)">🌍 TOP MONDIAL <span id="lb-players" style="color:#3a3a3a;font-size:8px"></span></div><div id="lb-list"><div style="font-size:9px;color:#2a2a2a;text-align:center;padding:10px 0">Connexion en cours...</div></div><div style="text-align:center;margin-top:4px;font-size:8px;color:#2a2a2a;display:flex;align-items:center;justify-content:center;gap:4px"><span class="online-dot"></span>Classement en temps réel</div></div></div>
  <div class="pnl" id="mp-pnl" style="flex-shrink:0"><div class="pi"><div class="pt" style="color:var(--green)">📡 ACTIVITÉ EN DIRECT</div><div class="mp-feed" id="mp-feed"><div style="font-size:9px;color:#2a2a2a;padding:4px">Aucune activité...</div></div></div></div>
  <div class="pnl" id="gift-pnl" style="flex-shrink:0"><div class="pi"><div class="pt" style="color:var(--green)">🎁 CADEAU <span id="streak-lbl" style="color:#3a3a3a"></span></div><div class="gift-days" id="gift-days"></div><button class="btn btn-g btn-sm" id="gift-btn" onclick="claimGift()" style="width:100%">🎁 RÉCLAMER</button></div></div>
  <div class="pnl" id="pets-pnl" style="flex-shrink:0"><div class="pi"><div class="pt">🐾 COMPAGNONS (<span id="pet-cnt">0</span>) <span id="pet-pw" style="color:var(--green);font-size:8px">+0/s</span></div><div class="egg-row"><button class="egg-btn b1" onclick="buyEgg(1)">🥚<span class="egg-cost" id="e1c">500</span></button><button class="egg-btn b2" onclick="buyEgg(2)">💜<span class="egg-cost" id="e2c">5K</span></button><button class="egg-btn b3" onclick="buyEgg(3)">👑<span class="egg-cost" id="e3c">50K</span></button></div><div class="pet-grid" id="pet-list"></div></div></div>
  <div class="pnl" id="skin-pnl" style="flex-shrink:0"><div class="pi"><div class="pt">🎨 ARMURERIE (<span id="skin-cnt">3</span>/48)</div><div class="sk-grid" id="skin-list"></div><div class="buy-row"><button class="btn btn-g btn-sm" onclick="buySkin()" style="flex:1">🎲 <span id="rc1">1K</span>✨</button><button class="btn btn-pk btn-sm" onclick="buySkin5()" style="flex:1">×5 <span id="rc5">4K</span>✨</button></div></div></div>
</div>
</div>
<div id="boss-intro"><div class="bi-card"><div class="bi-warn">⚠ ALERTE BOSS ⚠</div><span class="bi-ico" id="bi-ico">👾</span><div class="bi-name" id="bi-nm">—</div><div class="bi-sub" id="bi-sb">—</div></div></div>
<div class="modal" id="name-modal"><div class="mbox"><div class="mclose" onclick="closeModal('name-modal')">✕</div><div class="m-title" style="color:var(--neon)">🚀 TON PSEUDO</div><p style="font-size:11px;color:#666;margin-bottom:6px">Visible dans le classement mondial</p><input class="m-input" id="name-inp" placeholder="Ex: NovaMaster_99" maxlength="16" autocomplete="off"><button class="btn btn-n" onclick="saveName()" style="margin-top:5px">✅ CONFIRMER</button></div></div>
<div class="modal" id="prestige-modal"><div class="mbox"><div class="mclose" onclick="closeModal('prestige-modal')">✕</div><div class="m-title" style="color:var(--orange)">💥 PRESTIGE</div><p style="font-size:10px;color:#555;margin-bottom:7px">Recommence avec des bonus permanents.</p><div id="pm-opts"></div></div></div>
<div class="modal" id="ev-modal"><div class="mbox"><div class="mclose" onclick="closeModal('ev-modal')">✕</div><div class="ev-tag">⚡ ÉVÉNEMENT COSMIQUE</div><span class="ev-ico2" id="ev-ico">⭐</span><div class="ev-nm" id="ev-nm">—</div><div class="ev-desc2" id="ev-desc">—</div><button class="btn btn-n" onclick="closeModal('ev-modal')" style="max-width:180px;margin:0 auto">🎮 JOUER !</button></div></div>
<div class="modal" id="gift-modal"><div class="mbox"><div class="mclose" onclick="closeModal('gift-modal')">✕</div><div class="m-title" style="color:var(--green)">🎁 CADEAU REÇU !</div><span class="gift-big" id="gift-ico">🎁</span><div id="gift-desc" style="font-size:13px;font-weight:700;margin-top:5px">—</div><div id="gift-streak-msg" style="font-size:10px;color:#555;margin-top:3px"></div><button class="btn btn-g" onclick="closeModal('gift-modal')" style="max-width:160px;margin:8px auto 0">🚀 CONTINUER</button></div></div>
<div id="toasts"></div>
<script>
const cv=document.getElementById('bg'),cx=cv.getContext('2d');
let stars=[],neb=[],shoots=[];
function rBg(){cv.width=innerWidth;cv.height=innerHeight;stars=Array.from({length:220},()=>({x:Math.random()*cv.width,y:Math.random()*cv.height,r:Math.random()*1.5+.2,spd:Math.random()*.3+.04,op:Math.random()*.8+.2,tw:Math.random()*Math.PI*2}));neb=[{x:cv.width*.15,y:cv.height*.35,rx:260,ry:180,h:200,a:.022},{x:cv.width*.78,y:cv.height*.55,rx:240,ry:165,h:280,a:.018},{x:cv.width*.5,y:cv.height*.82,rx:180,ry:125,h:30,a:.016}];}
rBg();window.addEventListener('resize',rBg);
setInterval(()=>shoots.push({x:Math.random()*cv.width,y:0,len:100+Math.random()*80,spd:8+Math.random()*6,op:1,a:.52+Math.random()*.35}),4000);
(function draw(){cx.clearRect(0,0,cv.width,cv.height);neb.forEach(n=>{const g=cx.createRadialGradient(n.x,n.y,0,n.x,n.y,Math.max(n.rx,n.ry));g.addColorStop(0,`hsla(${n.h},80%,55%,${n.a})`);g.addColorStop(1,'transparent');cx.save();cx.scale(1,n.ry/n.rx);cx.beginPath();cx.arc(n.x,n.y*n.rx/n.ry,n.rx,0,Math.PI*2);cx.fillStyle=g;cx.fill();cx.restore();});stars.forEach(s=>{s.y+=s.spd;if(s.y>cv.height){s.y=0;s.x=Math.random()*cv.width}s.tw+=.024;cx.beginPath();cx.arc(s.x,s.y,s.r,0,Math.PI*2);cx.fillStyle=`rgba(255,255,255,${s.op*(.6+.4*Math.sin(s.tw))})`;cx.fill();});shoots.forEach((s,i)=>{cx.save();cx.globalAlpha=s.op;cx.strokeStyle=`rgba(255,255,255,${s.op})`;cx.lineWidth=1.5;cx.beginPath();cx.moveTo(s.x,s.y);cx.lineTo(s.x-Math.cos(s.a)*s.len,s.y-Math.sin(s.a)*s.len);cx.stroke();cx.restore();s.x+=Math.cos(s.a)*s.spd;s.y+=Math.sin(s.a)*s.spd;s.op-=.016;if(s.op<=0)shoots.splice(i,1);});requestAnimationFrame(draw);})();
let _ac;const ac=()=>{if(!_ac)_ac=new(window.AudioContext||window.webkitAudioContext)();return _ac};
function tone(f,d,t,v){try{const o=ac().createOscillator(),g=ac().createGain();o.connect(g);g.connect(ac().destination);o.type=t||'sine';o.frequency.value=f||440;g.gain.setValueAtTime(v||.09,ac().currentTime);g.gain.exponentialRampToValueAtTime(.001,ac().currentTime+(d||.08));o.start();o.stop(ac().currentTime+(d||.08));}catch(e){}}
const sBub=()=>tone(500+Math.random()*500,.06,'sine',.07);
const sBH=()=>tone(180,.1,'sawtooth',.08);
const sBK=()=>[300,420,560,720,1000].forEach((f,i)=>setTimeout(()=>tone(f,.18,'square',.09),i*55));
const sCrit=()=>{tone(880,.12,'square',.12);setTimeout(()=>tone(1200,.1,'square',.09),80)};
const sAch=()=>[523,659,784,1047].forEach((f,i)=>setTimeout(()=>tone(f,.2,'sine',.1),i*75));
const sPres=()=>[200,150,100,280,560,900,1400].forEach((f,i)=>setTimeout(()=>tone(f,.22,'sawtooth',.13),i*75));
const sFvr=()=>[400,600,800,1100].forEach((f,i)=>setTimeout(()=>tone(f,.15,'sine',.11),i*60));
const sEgg=()=>[300,500,700,900].forEach((f,i)=>setTimeout(()=>tone(f,.18,'sine',.11),i*80));
const sEv=()=>[440,550,660,880].forEach((f,i)=>setTimeout(()=>tone(f,.2,'sine',.11),i*100));
const sGift=()=>[523,784,1047,1568].forEach((f,i)=>setTimeout(()=>tone(f,.25,'sine',.12),i*90));
const LS=k=>localStorage.getItem(k);
const SS=(k,v)=>localStorage.setItem(k,typeof v==='object'?JSON.stringify(v):v);
const PLAYER_ID=LS('nb_pid')||(()=>{const id='P'+Date.now().toString(36).toUpperCase();SS('nb_pid',id);return id;})();
const SB_URL = "https://trtgtpdkidhgyxzxfria.supabase.co";
const SB_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRydGd0cGRraWRoZ3l4enhmcmlhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzM1ODc3MzMsImV4cCI6MjA4OTE2MzczM30.tsffAnsI2IF04oxDz5Q5PQ4ZM0nmolvMBXOqUYQT8rY";
let sbReady=false;
function initSupabase(){sbReady=true;if(pname)mpPushScore();mpFetchLeaderboard();mpFetchFeed();mpPing();setInterval(()=>{mpFetchLeaderboard();mpFetchFeed();mpPing();},8000);window.addEventListener('beforeunload',mpGoOffline);}
async function sb(table,method='GET',body=null,query=''){const url=`${SB_URL}/rest/v1/${table}${query}`;const opts={method,headers:{'apikey':SB_KEY,'Authorization':'Bearer '+SB_KEY,'Content-Type':'application/json','Prefer':method==='POST'?'resolution=merge-duplicates':''}};if(body)opts.body=JSON.stringify(body);const r=await fetch(url,opts);if(!r.ok)throw new Error(await r.text());return r.status===204?null:r.json();}
async function mpPing(){if(!sbReady||!pname)return;try{await sb('players','POST',{id:PLAYER_ID,name:pname,score:Math.floor(allTime),prestige,meta:metaP,wave:bossWave,kills:bossKills,ts:Date.now()});}catch(e){}}
async function mpGoOffline(){if(!sbReady||!pname)return;try{await sb('players','POST',{id:PLAYER_ID,name:pname,score:Math.floor(allTime),prestige,meta:metaP,wave:bossWave,kills:bossKills,ts:0});}catch(e){}}
async function mpPushScore(){if(!sbReady||!pname)return;try{await sb('players','POST',{id:PLAYER_ID,name:pname,score:Math.floor(allTime),prestige,meta:metaP,wave:bossWave,kills:bossKills,ts:Date.now()});}catch(e){console.warn('pushScore:',e);}}
async function mpPushActivity(msg,type){if(!sbReady||!pname)return;try{await sb('feed','POST',{pid:PLAYER_ID,name:pname,msg,type,ts:Date.now()});const old=Date.now()-120000;await sb('feed','DELETE',null,`?ts=lt.${old}`);}catch(e){}}
async function mpFetchLeaderboard(){if(!sbReady)return;try{const cutoff=Date.now()-300000;const players=await sb('players','GET',null,`?ts=gt.${cutoff}&order=score.desc&limit=10`);if(Array.isArray(players))renderLeaderboard(players);const active=Date.now()-120000;const all=await sb('players','GET',null,`?ts=gt.${active}&select=id`);if(Array.isArray(all)){const c=all.length;document.getElementById('lb-players').textContent=c+' en ligne';document.getElementById('online-count').textContent=c+' joueur'+(c>1?'s':'')+' en ligne';}}catch(e){console.warn('fetchLB:',e);}}
async function mpFetchFeed(){if(!sbReady)return;try{const cutoff=Date.now()-120000;const entries=await sb('feed','GET',null,`?ts=gt.${cutoff}&order=ts.asc&limit=15`);if(Array.isArray(entries))renderFeed(entries);}catch(e){}}
function renderLeaderboard(players){const list=document.getElementById('lb-list');list.innerHTML='';if(!players.length){list.innerHTML='<div style="font-size:9px;color:#2a2a2a;text-align:center;padding:10px 0">Personne en ligne...<br>Partage le lien !</div>';return;}players.slice(0,10).forEach((p,i)=>{const isMe=p.id===PLAYER_ID;const r=document.createElement('div');r.className='lb-row';const rc=i===0?'r1':i===1?'r2':i===2?'r3':'';const badge=p.meta>0?'🔮':p.prestige>0?'💥':'';r.innerHTML=`<span class="lb-rank ${rc}">${i===0?'🥇':i===1?'🥈':i===2?'🥉':i+1}</span><span class="lb-name${isMe?' me':''}">${(p.name||'?').replace(/</g,'&lt;')}${badge}</span><span class="lb-score">${fmt(p.score||0)}</span>`;list.appendChild(r);});}
function renderFeed(entries){const feed=document.getElementById('mp-feed');feed.innerHTML='';const now=Date.now();const seen=new Set();entries.forEach(e=>{const k=e.pid+e.msg;if(seen.has(k))return;seen.add(k);const ago=Math.round((now-e.ts)/1000);const isMe=e.pid===PLAYER_ID;const el=document.createElement('div');el.className='mp-entry '+(isMe?'me-ev':e.type==='boss'?'boss-ev':e.type==='prestige'?'pres-ev':'');el.innerHTML=`<span class="mp-ts">${ago<60?ago+'s':'1m+'}</span><span class="mp-msg"><b>${isMe?'Toi':(e.name||'?').replace(/</g,'&lt;')}</b> ${e.msg}</span>`;feed.appendChild(el);});if(!entries.length)feed.innerHTML='<div style="font-size:9px;color:#2a2a2a;padding:4px">Aucune activité...</div>';feed.scrollTop=feed.scrollHeight;}
setInterval(()=>mpPushScore(),20000);
let pname=LS('nb_name')||'';
let score=parseFloat(LS('nb_score'))||0;
let multi=parseFloat(LS('nb_multi'))||1;
let prestige=parseInt(LS('nb_pres'))||0;
let metaP=parseInt(LS('nb_meta'))||0;
let bossWave=parseInt(LS('nb_wave'))||1;
let bossKills=parseInt(LS('nb_bk'))||0;
let maxCombo=parseInt(LS('nb_mc'))||0;
let allTime=parseFloat(LS('nb_at'))||0;
let totalClick=parseInt(LS('nb_tc'))||0;
let totalBub=parseInt(LS('nb_tb'))||0;
let critCount=parseInt(LS('nb_cc'))||0;
let dayStreak=parseInt(LS('nb_streak'))||0;
let lastGift=LS('nb_lastgift')||'';
let skins=JSON.parse(LS('nb_skins'))||['🔵','☄️','🛸'];
let pets=JSON.parse(LS('nb_pets'))||[];
let curSkin=skins[0];let isBoss=false;
let bubVal=10,bossDmg=1,critChance=.05,critMult=2,autoDps=0,petPMult=1;
let fFillB=0,fDurB=0,fvM=0,fvOn=false,fvTimer=null;
let comboN=0,comboT=null,lastClic=0;
let evMult=1,evActive=false,bossEvM=1,critEv=false;
let dpsA=0,cpsA=0,petDA=0;
setInterval(()=>{document.getElementById('dps').textContent=fmt(Math.round(dpsA));dpsA=0;},1000);
setInterval(()=>{document.getElementById('cps').textContent=cpsA;cpsA=0;},1000);
setInterval(()=>{document.getElementById('pet-dps').textContent=fmt(Math.round(petDA));petDA=0;},1000);
const cAt=(b,l)=>Math.floor(b*Math.pow(1.65,l));
const UPGS=[{id:1,n:'Propulseur Quantum',i:'🚀',c:'mult',b:500,v:.1,d:'+0.10× mult'},{id:2,n:'Cristaux d\'Orion',i:'💎',c:'mult',b:3500,v:.25,d:'+0.25× mult'},{id:3,n:'Réacteur Antimatière',i:'☢️',c:'mult',b:10000,v:.6,d:'+0.60× mult'},{id:4,n:'Hyperespace Drive',i:'🌌',c:'mult',b:40000,v:1.5,d:'+1.5× mult'},{id:5,n:'Singularité Noire',i:'🕳️',c:'mult',b:120000,v:4.0,d:'+4.0× mult'},{id:6,n:'Turbine Galactique',i:'🌀',c:'mult',b:400000,v:8.0,d:'+8.0× mult'},{id:7,n:'Capteurs Lunaires',i:'🌙',c:'auto',b:1200,v:5,d:'+5 pts/s'},{id:8,n:'Drones Automatiques',i:'🤖',c:'auto',b:4500,v:20,d:'+20 pts/s'},{id:9,n:'Armée de Nanobots',i:'🦠',c:'auto',b:14000,v:60,d:'+60 pts/s'},{id:10,n:'Flotte Galactique',i:'🛸',c:'auto',b:50000,v:200,d:'+200 pts/s'},{id:11,n:'Matrice IA Cosmique',i:'🧠',c:'auto',b:160000,v:800,d:'+800 pts/s'},{id:12,n:'Codex Universel',i:'📖',c:'auto',b:500000,v:2000,d:'+2000 pts/s'},{id:13,n:'Sérum d\'Étoiles',i:'💉',c:'bub',b:800,v:5,d:'+5 pts/bulle'},{id:14,n:'Champ de Force',i:'🛡️',c:'bub',b:2500,v:15,d:'+15 pts/bulle'},{id:15,n:'Plasma Stellaire',i:'🔆',c:'bub',b:8000,v:50,d:'+50 pts/bulle'},{id:16,n:'Atome Condensé',i:'⚛️',c:'bub',b:30000,v:200,d:'+200 pts/bulle'},{id:17,n:'Laser Nébulaire',i:'🔫',c:'boss',b:2000,v:1,d:'+1 dégât boss'},{id:18,n:'Canon Photonique',i:'⚡',c:'boss',b:6000,v:3,d:'+3 dégât boss'},{id:19,n:'Supernova Condensée',i:'💥',c:'boss',b:20000,v:8,d:'+8 dégât boss'},{id:20,n:'Marteau d\'Étoile',i:'🔨',c:'boss',b:70000,v:25,d:'+25 dégât boss'},{id:21,n:'Œil du Tireur',i:'👁️',c:'crit',b:3000,v:.03,d:'+3% crit'},{id:22,n:'Amplificateur Crit',i:'🎯',c:'crit',b:12000,v:.5,d:'+0.5× crit mult'},{id:23,n:'Overdrive Temporel',i:'⏱️',c:'fvr',b:5000,v:10,d:'Fever +10% vitesse'},{id:24,n:'Noyau de Fusion',i:'🌡️',c:'fvr',b:18000,v:20,d:'Fever +20s'},{id:25,n:'Bond de Pet',i:'🐾',c:'pet',b:8000,v:.3,d:'Pets +30%'},{id:26,n:'Nourriture Cosmique',i:'🍖',c:'pet',b:25000,v:.8,d:'Pets +80%'}];
let upgLvl=JSON.parse(LS('nb_upg'))||{};UPGS.forEach(u=>{if(!upgLvl[u.id])upgLvl[u.id]=0;});
function initUpgrades(){const mb=1+metaP*.5;multi=1+prestige*1.5+metaP*3;bubVal=10*(1+metaP);bossDmg=1;critChance=.05;critMult=2+metaP*.5;fFillB=0;fDurB=0;autoDps=0;petPMult=1+metaP;UPGS.forEach(u=>{const l=upgLvl[u.id]||0;if(!l)return;switch(u.c){case'mult':multi+=u.v*l*mb;break;case'bub':bubVal+=u.v*l*mb;break;case'boss':bossDmg+=u.v*l;break;case'crit':u.id===21?critChance=Math.min(.85,critChance+u.v*l):critMult+=u.v*l;break;case'fvr':u.id===23?fFillB+=u.v*l:fDurB+=u.v*l;break;case'auto':autoDps+=u.v*l*mb;break;case'pet':petPMult+=u.v*l;break;}});setupAuto();}
let autoInt=null;
function setupAuto(){if(autoInt)clearInterval(autoInt);autoDps=0;UPGS.forEach(u=>{if(u.c==='auto')autoDps+=u.v*(upgLvl[u.id]||0)*(1+metaP);});if(autoDps>0){autoInt=setInterval(()=>{const e=autoDps*multi*evMult*(fvOn?3:1);score+=e;allTime+=e;dpsA+=e;updateUI();},1000);}}
function buyUpgrade(id){const u=UPGS.find(u=>u.id===id);if(!u)return;const l=upgLvl[u.id]||0,c=cAt(u.b,l);if(score<c)return;score-=c;upgLvl[u.id]=l+1;SS('nb_upg',upgLvl);switch(u.c){case'mult':multi+=u.v;break;case'bub':bubVal+=u.v;break;case'boss':bossDmg+=u.v;break;case'auto':setupAuto();break;case'crit':u.id===21?critChance=Math.min(.85,critChance+u.v):critMult+=u.v;break;case'fvr':u.id===23?fFillB+=u.v:fDurB+=u.v;break;case'pet':petPMult+=u.v;break;}sAch();toast(u.i,`AMÉLIORATION Niv.${upgLvl[u.id]}`,`${u.n} — ${u.d}`);checkAchs();updateUI();}
const PT={1:{icons:['🐱','🐶','🐭','🐹','🐰','🐸'],p:1,n:'Commun'},2:{icons:['🦊','🐼','🐯','🐉','🦁','🐺'],p:5,n:'Rare'},3:{icons:['🦄','👽','👑','🌟','💎','🔱','🧿'],p:20,n:'Divin'}};
function petPow(){return Math.max(0,pets.reduce((s,p)=>s+(p.power||1),0))*petPMult;}
function petDps2(){return petPow()*multi*evMult*(fvOn?2:1);}
setInterval(()=>{if(!pets.length)return;const e=petDps2();score+=e;allTime+=e;dpsA+=e;petDA+=e;updateUI();},1000);
setInterval(()=>{if(!pets.length)return;const bubs=[...document.querySelectorAll('.bubble')];if(!bubs.length)return;pets.forEach(pet=>{if(Math.random()<Math.min(.9,pet.power*.04)){const t=bubs[Math.floor(Math.random()*bubs.length)];if(!t||!t.parentNode)return;const p=document.createElement('div');p.className='pet-minion';p.textContent=pet.icon;p.style.cssText='left:50%;top:50%';document.body.appendChild(p);const r=t.getBoundingClientRect();setTimeout(()=>{p.style.left=r.left+r.width/2+'px';p.style.top=r.top+r.height/2+'px';},30);setTimeout(()=>{if(t.parentNode){const e=Math.round(bubVal*pet.power*.5*multi*evMult*(fvOn?3:1));score+=e;allTime+=e;dpsA+=e;petDA+=e;floatTxt(r.left+r.width/2,r.top+r.height/2,'+'+fmt(e),'#00ff88');t.remove();checkAchs();updateUI();}p.remove();},750);}});},1800);
function buyEgg(tier){const m=Math.max(1,pets.length*.5+1);const costs={1:Math.floor(500*m),2:Math.floor(5000*m),3:Math.floor(50000*m)};if(score<costs[tier])return;score-=costs[tier];const td=PT[tier],icon=td.icons[Math.floor(Math.random()*td.icons.length)];pets.push({icon,power:td.p,tier});SS('nb_pets',pets);sEgg();toast('🥚',`PET ÉCLOS [${td.n}]`,`${icon} +${td.p} puissance`);checkAchs();updateUI();}
const EVENTS=[{ico:'🌪️',n:'TEMPÊTE COSMIQUE',d:'Points ×3 pendant 30s!',mult:3,dur:30000,bx:1,cr:false},{ico:'⭐',n:'PLUIE D\'ÉTOILES',d:'Points ×2!',mult:2,dur:25000,bx:1,cr:false},{ico:'💎',n:'CRISTAL TEMPOREL',d:'Auto-DPS ×5!',mult:1,dur:20000,bx:1,cr:false,ax:5},{ico:'🔥',n:'RAGE COSMIQUE',d:'Dégâts boss ×4!',mult:1,dur:40000,bx:4,cr:false},{ico:'🌈',n:'FESTIVAL GALACTIQUE',d:'TOUT ×2!',mult:2,dur:20000,bx:2,cr:false},{ico:'🍀',n:'CHANCE QUANTIQUE',d:'Crits garantis 15s!',mult:1,dur:15000,bx:1,cr:true},{ico:'🦄',n:'MIRACLE COSMIQUE',d:'Bulles ×10 pendant 10s!',mult:10,dur:10000,bx:1,cr:false},{ico:'⚡',n:'FOUDRE STELLAIRE',d:'Auto+Pet ×8 pendant 18s!',mult:1,dur:18000,bx:1,cr:false,ax:8}];
let evEndT=null,evChipInt=null;
function triggerEvent(){if(evActive)return;const ev=EVENTS[Math.floor(Math.random()*EVENTS.length)];evMult=ev.mult;bossEvM=ev.bx||1;critEv=ev.cr||false;evActive=true;evEndT=Date.now()+ev.dur;SS('nb_ev','1');sEv();document.getElementById('ev-ico').textContent=ev.ico;document.getElementById('ev-nm').textContent=ev.ico+' '+ev.n;document.getElementById('ev-desc').textContent=ev.d;openModal('ev-modal');const chip=document.getElementById('ev-chip');chip.style.display='block';if(evChipInt)clearInterval(evChipInt);evChipInt=setInterval(()=>{const r=Math.ceil((evEndT-Date.now())/1000);chip.textContent=`${ev.ico} ${ev.n} — ${r}s`;if(r<=0){clearInterval(evChipInt);chip.style.display='none';}},1000);setTimeout(()=>{evMult=1;bossEvM=1;critEv=false;evActive=false;if(ev.ax)setupAuto();toast('⏰','ÉVÉNEMENT TERMINÉ','Retour à la normale.');},ev.dur);mpPushActivity(`joue l'événement ${ev.ico} ${ev.n}!`,'event');checkAchs();}
setInterval(()=>{if(!evActive&&Math.random()<.28)triggerEvent();},60000);
setTimeout(()=>{if(!evActive)triggerEvent();},75000);
const BOSSES=[{n:'Xylos le Destructeur',s:'Terreur de la nébuleuse',i:'🛸',hp:50,c:'#ff004c',r:500,er:30},{n:'L\'Anomalie Nébulique',s:'Entité de dimension zéro',i:'🌀',hp:80,c:'#9d00ff',r:800,er:40},{n:'Gardien d\'Orion',s:'Sentinelle éternelle',i:'🛡️',hp:120,c:'#00ffcc',r:1200,er:50},{n:'Star-Eater Prime',s:'Dévoreur d\'étoiles',i:'👹',hp:200,c:'#ff8000',r:2000,er:35},{n:'Spectre de Pluton',s:'Fantôme de la zone froide',i:'👻',hp:60,c:'#c8e6ff',r:600,er:25},{n:'Général Robotonique',s:'Commandant de l\'IA',i:'🤖',hp:150,c:'#8888aa',r:1500,er:45},{n:'Dragon de l\'Espace',s:'Seigneur du vide',i:'🐲',hp:300,c:'#22ff00',r:3000,er:60},{n:'Sa Majesté Cosmique',s:'Règne depuis la nuit',i:'👑',hp:100,c:'#ffd700',r:1000,er:30},{n:'Trou Noir Vivant',s:'L\'abîme vous observe',i:'🕳️',hp:500,c:'#7700ff',r:5000,er:70},{n:'Œil de la Galaxie',s:'Tout-voyant cosmique',i:'👁️',hp:250,c:'#00d2ff',r:2500,er:55},{n:'Kraken Stellaire',s:'Monstruosité des abysses',i:'🦑',hp:400,c:'#0055ff',r:4000,er:65},{n:'Comète Vivante',s:'Vitesse absolue',i:'🌠',hp:180,c:'#ffee00',r:1800,er:40},{n:'Démon d\'Andromède',s:'Exilé de la galaxie mère',i:'😈',hp:450,c:'#ff3300',r:4500,er:70},{n:'Oracle Brisé',s:'Prophète des fins',i:'🔮',hp:350,c:'#cc00ff',r:3500,er:60},{n:'Phénix du Néant',s:'Il renaît sans cesse',i:'🦅',hp:280,c:'#ff6600',r:2800,er:50},{n:'Golem Métallique',s:'Construit pour l\'éternité',i:'🗿',hp:600,c:'#aaaaaa',r:6000,er:80},{n:'Araignée Cosmique',s:'Tisse des pièges dans l\'espace',i:'🕷️',hp:320,c:'#550055',r:3200,er:55},{n:'Ouragan Solaire',s:'Tempête venue du soleil',i:'🌪️',hp:220,c:'#ffaa00',r:2200,er:45},{n:'Nova l\'Impitoyable',s:'La dernière étoile mourante',i:'💫',hp:700,c:'#ff99ff',r:7000,er:85},{n:'Seigneur du Néant',s:'Maître de l\'obscurité',i:'🌑',hp:900,c:'#330033',r:9000,er:90}];
let bossCD=300;
setInterval(()=>{if(isBoss)return;bossCD--;if(bossCD<=0){bossCD=300;spawnBoss();}const m=Math.floor(bossCD/60),s=bossCD%60;document.getElementById('bcd').textContent=`${m}:${s.toString().padStart(2,'0')}`;document.getElementById('boss-timer').classList.toggle('imm',bossCD<=30);},1000);
function spawnBoss(){if(isBoss)return;isBoss=true;const bd=BOSSES[Math.floor(Math.random()*BOSSES.length)];const maxHp=Math.floor(bd.hp*(1+bossWave*.6)+prestige*90+metaP*500);document.getElementById('bi-ico').textContent=bd.i;document.getElementById('bi-nm').textContent=bd.n;document.getElementById('bi-sb').textContent=bd.s;document.getElementById('boss-intro').classList.add('on');tone(120,.6,'sawtooth',.12);setTimeout(()=>{document.getElementById('boss-intro').classList.remove('on');startBoss(bd,maxHp);},2200);}
function startBoss(bd,maxHp){let hp=maxHp,enraged=false;const spr=document.getElementById('boss-spr'),nm=document.getElementById('boss-nm'),sub=document.getElementById('boss-sub'),fill=document.getElementById('hp-fill'),hpTx=document.getElementById('hp-txt'),hpPct=document.getElementById('hp-pct'),enrEl=document.getElementById('boss-enrage'),zone=document.getElementById('boss-zone');spr.textContent=bd.i;spr.style.filter=`drop-shadow(0 0 26px ${bd.c})`;nm.textContent=bd.n;nm.style.color=bd.c;nm.style.textShadow=`0 0 11px ${bd.c}`;sub.textContent=bd.s;fill.style.width='100%';hpTx.textContent=fmt(maxHp)+'/'+fmt(maxHp);hpPct.textContent='100%';enrEl.textContent='';zone.style.display='flex';spr.onclick=e=>{if(hp<=0)return;totalClick++;cpsA++;SS('nb_tc',totalClick);const isCrit=(critEv||Math.random()<critChance),fm=fvOn?3:1;let dmg=Math.round(bossDmg*bossEvM*(isCrit?critMult:1)*fm)+Math.floor(petPow()*.2);if(enraged)dmg=Math.round(dmg*.6);hp-=dmg;if(hp<0)hp=0;const earned=Math.round(10*multi*evMult*fm);score+=earned;allTime+=earned;const pct=hp/maxHp;fill.style.width=(pct*100)+'%';hpTx.textContent=fmt(Math.max(0,hp))+'/'+fmt(maxHp);hpPct.textContent=Math.round(pct*100)+'%';spr.style.transform='scale(.84)';setTimeout(()=>spr.style.transform='',55);floatTxt(e.clientX,e.clientY,isCrit?`⚡${fmt(dmg)}`:`-${fmt(dmg)}`,isCrit?'#ff004c':'#ff6688',isCrit);ring(e.clientX,e.clientY,'rgba(255,0,76,.7)');isCrit?sCrit():sBH();shake();doCombo();if(!enraged&&pct<=(bd.er/100)){enraged=true;enrEl.textContent='⚠ ENRAGÉ — Défense ×1.5!';spr.style.animation='bflt .7s ease-in-out infinite';toast('⚠️','BOSS ENRAGÉ!','Résistance accrue!');}if(hp<=0){const bonus=Math.round(bd.r*(bossWave+prestige+metaP*5)*evMult);score+=bonus;allTime+=bonus;bossKills++;bossWave++;SS('nb_bk',bossKills);SS('nb_wave',bossWave);isBoss=false;zone.style.display='none';spr.style.animation='bflt 3s ease-in-out infinite';sBK();floatTxt(innerWidth/2,innerHeight/2,`BOSS VAINCU! +${fmt(bonus)}✨`,'#ffd700',true);toast('💀','BOSS VAINCU!',`${bd.n} — +${fmt(bonus)} ✨`);document.getElementById('b-wave').textContent=bossWave;document.getElementById('b-kills').textContent=bossKills;mpPushActivity(`a vaincu ${bd.i} ${bd.n}!`,'boss');mpPushScore();checkAchs();updateUI();}else updateUI();};}
const PTIERS=[{lbl:'NOVA',cost:n=>5000*Math.pow(2,n),r:'×1.5 mult, reset score'},{lbl:'PULSAR',cost:n=>50000*Math.pow(3,n),r:'×3 mult, boss dmg×2'},{lbl:'QUASAR',cost:n=>500000*Math.pow(4,n),r:'×6 mult, crit×1.5'}];
const MPTIERS=[{lbl:'MÉTA I',cost:5,r:'Pets×3, +10% crit'},{lbl:'MÉTA II',cost:15,r:'Auto DPS×5, bubble×3'},{lbl:'MÉTA III',cost:30,r:'TOUT×10 — Singularité'}];
function openPrestigeModal(){const opts=document.getElementById('pm-opts');opts.innerHTML='';PTIERS.forEach((t,i)=>{const cost=t.cost(prestige);const can=score>=cost;const el=document.createElement('div');el.className='pm-opt'+(can?'':' dim');el.innerHTML=`<div class="pm-ico">${['💥','🌟','⚡'][i]}</div><div><div class="pm-nm">${t.lbl}</div><div class="pm-d">${t.r}</div><div class="pm-c">Coût: ${fmt(cost)} ✨</div></div>`;if(can)el.onclick=()=>doPrestige(i);opts.appendChild(el);});const div=document.createElement('div');div.className='pm-meta-div';div.textContent='✦ MÉTA-PRESTIGE ✦';opts.appendChild(div);MPTIERS.forEach((t,i)=>{const can=prestige>=t.cost;const el=document.createElement('div');el.className='pm-opt'+(can?'':' dim');el.style.borderColor=can?'rgba(180,79,255,.35)':'rgba(255,255,255,.06)';el.innerHTML=`<div class="pm-ico">🔮</div><div><div class="pm-nm" style="color:var(--purple)">${t.lbl}</div><div class="pm-d">${t.r}</div><div class="pm-c" style="color:var(--purple)">Requiert prestige ${t.cost}</div></div>`;if(can)el.onclick=()=>doMeta(i);opts.appendChild(el);});openModal('prestige-modal');}
function doPrestige(ti){const t=PTIERS[ti];const cost=t.cost(prestige);if(score<cost)return;closeModal('prestige-modal');score=0;prestige++;SS('nb_pres',prestige);initUpgrades();flash('fire');sPres();toast('💥','PRESTIGE!',`${t.lbl} — ${t.r}`);mpPushActivity(`a effectué un prestige ${t.lbl}!`,'prestige');mpPushScore();checkAchs();updateUI();}
function doMeta(ti){const t=MPTIERS[ti];if(prestige<t.cost)return;closeModal('prestige-modal');metaP++;prestige=0;score=0;upgLvl={};UPGS.forEach(u=>upgLvl[u.id]=0);SS('nb_upg',upgLvl);SS('nb_meta',metaP);SS('nb_pres',0);initUpgrades();flash('void');sPres();toast('🔮','MÉTA-PRESTIGE!',`${t.lbl} — ${t.r}`);mpPushActivity(`a atteint le MÉTA-PRESTIGE ${t.lbl}! 🔮`,'prestige');mpPushScore();checkAchs();updateUI();}
const SKIN_POOL=['💎','🔥','🌈','🍕','🍔','🍦','🍭','🎁','🦄','⚡','💯','💰','🐉','🌟','🦋','🎮','🏆','🎯','🌙','☀️','🦊','🐬','🌊','🎆','🧊','🍀','🎸','🧿','🪐','🌺','🦁','🐯','🐸','🦜','🧸','🎃','👽','🤡','🦾','🎵','🧲','🔱','⚜️','🌀','🎴'];
const skC=(n=1)=>Math.floor(1000*Math.pow(1.1,skins.length)*n*.92);
function buySkin(){const c=skC();if(score<c)return;score-=c;const av=SKIN_POOL.filter(s=>!skins.includes(s));if(av.length){const s=av[Math.floor(Math.random()*av.length)];skins.push(s);toast('🎲','SKIN!',s+' débloqué !');}SS('nb_skins',skins);checkAchs();updateUI();}
function buySkin5(){const c=skC(5);if(score<c)return;score-=c;const av=SKIN_POOL.filter(s=>!skins.includes(s));const n=Math.min(5,av.length);for(let i=0;i<n;i++){const idx=Math.floor(Math.random()*av.length);skins.push(av[idx]);av.splice(idx,1);}n?toast('🎲','PACK ×5!',n+' skins !'):toast('ℹ️','Complet','Tout débloqué!');SS('nb_skins',skins);checkAchs();updateUI();}
const GIFTS=[{i:'💰',d:'500 ✨ offertes!',a:()=>{score+=500;}},{i:'⚡',d:'Mult +0.5× pendant 5min!',a:()=>{multi+=.5;setTimeout(()=>multi-=.5,300000);}},{i:'🐾',d:'Compagnon commun gratuit!',a:()=>{const td=PT[1];pets.push({icon:td.icons[Math.floor(Math.random()*td.icons.length)],power:td.p,tier:1});SS('nb_pets',pets);}},{i:'🎯',d:'Crit 100% pendant 60s!',a:()=>{critEv=true;setTimeout(()=>critEv=false,60000);}},{i:'🔥',d:'Fever instantané!',a:()=>{if(!fvOn)activateFever(20000);}},{i:'💎',d:'5 000 ✨ offertes!',a:()=>{score+=5000;}},{i:'👑',d:'50 000 ✨ — Cadeau Royal!',a:()=>{score+=50000;}}];
function buildGiftDays(){const el=document.getElementById('gift-days');el.innerHTML='';const DLBLS=['L','M','M','J','V','S','D'];DLBLS.forEach((d,i)=>{const s=dayStreak%7;const div=document.createElement('div');div.className='gd';if(i<s)div.classList.add('done');if(i===s)div.classList.add('today');div.innerHTML=`<span>${i===s?'🎁':i<s?'✅':GIFTS[i%GIFTS.length].i}</span><span class="gd-lbl">${d}</span>`;el.appendChild(div);});const today=new Date().toDateString();document.getElementById('gift-btn').disabled=lastGift===today;document.getElementById('gift-btn').textContent=lastGift===today?'✅ REÇU':'🎁 RÉCLAMER';document.getElementById('streak-lbl').textContent=`Série: ${dayStreak}j`;}
function claimGift(){const today=new Date().toDateString();if(lastGift===today)return;const i=Math.min(Math.floor(dayStreak/7),GIFTS.length-1);const gift=GIFTS[i];gift.a();lastGift=today;dayStreak++;SS('nb_lastgift',today);SS('nb_streak',dayStreak);document.getElementById('gift-ico').textContent=gift.i;document.getElementById('gift-desc').textContent=gift.d;document.getElementById('gift-streak-msg').textContent=dayStreak>=3?`🔥 ${dayStreak} jours consécutifs!`:'Reviens demain pour un meilleur cadeau!';openModal('gift-modal');sGift();buildGiftDays();mpPushActivity(`a réclamé son cadeau ${gift.i}`,'gift');updateUI();checkAchs();}
function doCombo(){const now=Date.now();now-lastClic<500?comboN++:comboN=1;lastClic=now;if(comboN>maxCombo){maxCombo=comboN;SS('nb_mc',maxCombo);}if(comboT)clearTimeout(comboT);comboT=setTimeout(()=>{comboN=0;document.getElementById('combo-txt').classList.remove('on');},1100);if(comboN>=3){const el=document.getElementById('combo-txt');el.textContent=`COMBO ×${comboN}!`;el.classList.remove('on');void el.offsetWidth;el.classList.add('on');}if(!fvOn){fvM=Math.min(100,fvM+(2+fFillB*.1));document.getElementById('fever-fill').style.width=fvM+'%';if(fvM>=100)activateFever();}return comboN>=3?1+(comboN-2)*.08:1;}
function activateFever(dur){if(fvOn)return;fvOn=true;SS('nb_fu','1');sFvr();document.getElementById('fever-on').classList.add('on');toast('🔥','FEVER MODE!','×3 sur tous les gains!');checkAchs();if(fvTimer)clearTimeout(fvTimer);fvTimer=setTimeout(()=>{fvOn=false;fvM=0;document.getElementById('fever-fill').style.width='0%';document.getElementById('fever-on').classList.remove('on');},dur||(30000+fDurB*1000));}
let bList=[];
const BT=[{p:.56,sz:35,lf:2500,cl:'',m:1,lb:null},{p:.15,sz:44,lf:3200,cl:'',m:1.5,lb:null},{p:.10,sz:27,lf:1600,cl:'',m:.8,lb:null},{p:.07,sz:40,lf:4000,cl:'b-g',m:5,lb:'⭐'},{p:.05,sz:42,lf:3500,cl:'b-p',m:10,lb:'💜'},{p:.025,sz:38,lf:4500,cl:'b-pt',m:8,lb:null,pet:true},{p:.015,sz:46,lf:5000,cl:'b-r',m:20,lb:'🔴'}];
function createBubble(){if(isBoss||bList.length>14)return;const r=Math.random();let c=0,bt=BT[0];for(const t of BT){c+=t.p;if(r<=c){bt=t;break;}}const el=document.createElement('div');el.className='bubble '+bt.cl;const icon=bt.pet&&pets.length?pets[Math.floor(Math.random()*pets.length)].icon:(bt.lb||curSkin);const lx=278,rx=innerWidth-264,ty=155,by2=innerHeight-65;el.style.cssText=`left:${lx+Math.random()*(rx-lx)}px;top:${ty+Math.random()*(by2-ty)}px;font-size:${bt.sz}px`;el.textContent=icon;el.onclick=ex=>{ex.stopPropagation();totalClick++;cpsA++;totalBub++;SS('nb_tc',totalClick);SS('nb_tb',totalBub);const cm=doCombo(),fm=fvOn?3:1,em=evMult;let base=bubVal*bt.m*multi*cm*fm*em;if(bt.pet)base*=(1+petPMult*.5);const isCrit=(critEv||Math.random()<critChance);if(isCrit){base*=critMult;critCount++;SS('nb_cc',critCount);}const earned=Math.round(base);score+=earned;allTime+=earned;dpsA+=earned;const bx=el.getBoundingClientRect().left+el.offsetWidth/2,by3=el.getBoundingClientRect().top+el.offsetHeight/2;const col=isCrit?'#ff004c':bt.cl==='b-g'?'#ffd700':bt.cl==='b-p'?'#b44fff':bt.cl==='b-r'?'#ff4466':bt.pet?'#00ff88':'#00d2ff';floatTxt(bx,by3,(isCrit?'⚡CRIT! ':'+')+fmt(earned),col,isCrit);ring(bx,by3);isCrit?sCrit():sBub();el.remove();bList=bList.filter(b=>b!==el);checkAchs();updateUI();};document.body.appendChild(el);bList.push(el);setTimeout(()=>{el.remove();bList=bList.filter(b=>b!==el);},bt.lf);}
setInterval(createBubble,680);
const ACHS=[{id:'a1',i:'👶',n:'Premier Clic',d:'Cliquer pour la 1ère fois',c:()=>totalClick>=1},{id:'a2',i:'💰',n:'Millionnaire',d:'1 000 ✨',c:()=>allTime>=1000},{id:'a3',i:'💎',n:'Multimillionnaire',d:'100 000 ✨',c:()=>allTime>=100000},{id:'a4',i:'🏆',n:'Milliardaire',d:'1 000 000 ✨',c:()=>allTime>=1000000},{id:'a5',i:'🌌',n:'Transcendance',d:'1 Milliard ✨',c:()=>allTime>=1e9},{id:'a6',i:'♾️',n:'Infini',d:'1 Trillion ✨',c:()=>allTime>=1e12},{id:'a7',i:'🎯',n:'Chasseur de Boss',d:'1er boss vaincu',c:()=>bossKills>=1},{id:'a8',i:'💀',n:'Tueur de Boss',d:'5 boss vaincus',c:()=>bossKills>=5},{id:'a9',i:'☠️',n:'Légendaire',d:'20 boss vaincus',c:()=>bossKills>=20},{id:'a10',i:'👑',n:'Exterminateur',d:'50 boss vaincus',c:()=>bossKills>=50},{id:'a11',i:'🔥',n:'Combo ×5',d:'Combo ×5',c:()=>maxCombo>=5},{id:'a12',i:'🌪️',n:'Combo Extrême',d:'Combo ×15',c:()=>maxCombo>=15},{id:'a13',i:'⚡',n:'Combo Légendaire',d:'Combo ×30',c:()=>maxCombo>=30},{id:'a14',i:'💥',n:'Big Bang',d:'1er prestige',c:()=>prestige>=1},{id:'a15',i:'⭐',n:'Multivers',d:'5 prestiges',c:()=>prestige>=5},{id:'a16',i:'🔮',n:'Méta-Prestige',d:'Atteindre Méta I',c:()=>metaP>=1},{id:'a17',i:'🌠',n:'Transcendance II',d:'Atteindre Méta III',c:()=>metaP>=3},{id:'a18',i:'🎨',n:'Collectionneur',d:'5 skins débloqués',c:()=>skins.length>=5},{id:'a19',i:'🌈',n:'Arc-en-Ciel',d:'15 skins débloqués',c:()=>skins.length>=15},{id:'a20',i:'⚡',n:'Upgradé',d:'5 améliorations',c:()=>Object.values(upgLvl).filter(v=>v>=1).length>=5},{id:'a21',i:'🧙',n:'Maître Upgrades',d:'Niv.10 sur une upgrade',c:()=>Object.values(upgLvl).some(v=>v>=10)},{id:'a22',i:'💥',n:'Critique!',d:'1er crit',c:()=>critCount>=1},{id:'a23',i:'🤖',n:'Auto-pilote',d:'Acheter des drones',c:()=>(upgLvl[8]||0)>=1},{id:'a24',i:'🖱️',n:'Cliqueur Fou',d:'1 000 clics',c:()=>totalClick>=1000},{id:'a25',i:'🔥',n:'Fever!',d:'Activer Fever',c:()=>LS('nb_fu')==='1'},{id:'a26',i:'🌟',n:'Légende',d:'Score actif de 1M',c:()=>score>=1000000},{id:'a27',i:'🥚',n:'Éleveur',d:'3 compagnons',c:()=>pets.length>=3},{id:'a28',i:'🦁',n:'Maître des Bêtes',d:'10 compagnons',c:()=>pets.length>=10},{id:'a29',i:'⚡',n:'Survivant',d:'Vivre un événement',c:()=>LS('nb_ev')==='1'},{id:'a30',i:'📅',n:'Fidèle',d:'7 jours de connexion',c:()=>dayStreak>=7},{id:'a31',i:'🌍',n:'Compétiteur',d:'Choisir un pseudo',c:()=>!!pname}];
let unlockedAchs=JSON.parse(LS('nb_achs'))||[];
function checkAchs(){ACHS.forEach(a=>{if(!unlockedAchs.includes(a.id)&&a.c()){unlockedAchs.push(a.id);SS('nb_achs',unlockedAchs);toast(a.i,'SUCCÈS DÉBLOQUÉ!',a.n);sAch();}});renderAchs();}
const PBLS=['NOVA','PULSAR','QUASAR','NEBULA','COSMIC','DIVINE'];
function fmt(n){if(n>=1e15)return(n/1e15).toFixed(1)+'Qa';if(n>=1e12)return(n/1e12).toFixed(1)+'T';if(n>=1e9)return(n/1e9).toFixed(1)+'B';if(n>=1e6)return(n/1e6).toFixed(1)+'M';if(n>=1e3)return(n/1e3).toFixed(1)+'K';return Math.floor(n).toLocaleString();}
function renderPb(){const bar=document.getElementById('pb');bar.innerHTML='';PBLS.forEach((l,i)=>{const d=document.createElement('div');d.className='pb-seg';if(i<prestige%6)d.classList.add('dn');if(i===prestige%6)d.classList.add('act');d.textContent=l;bar.appendChild(d);});}
function renderAchs(){const l=document.getElementById('ach-list');l.innerHTML='';ACHS.forEach(a=>{const ok=unlockedAchs.includes(a.id);const d=document.createElement('div');d.className='ach-row'+(ok?' done':' locked');d.innerHTML=`<span class="ach-ico">${a.i}</span><div><div class="ach-name">${a.n}</div><div class="ach-sub">${a.d}</div></div>`;l.appendChild(d);});document.getElementById('ach-cnt').textContent=`${unlockedAchs.length}/${ACHS.length}`;}
function renderUpgrades(){const s=document.getElementById('upg-scroll');s.innerHTML='';const cats={mult:'✨ Multiplicateur',auto:'🤖 Auto-Click',bub:'🫧 Bulle',boss:'💀 Boss',crit:'⚡ Critique',fvr:'🔥 Fever',pet:'🐾 Compagnons'};Object.entries(cats).forEach(([cat,label])=>{const ups=UPGS.filter(u=>u.c===cat);const hdr=document.createElement('div');hdr.className='ucat';hdr.textContent=label;s.appendChild(hdr);ups.forEach(u=>{const l=upgLvl[u.id]||0,c=cAt(u.b,l),af=score>=c;const el=document.createElement('div');el.className='urow'+(af?'':' dim');el.innerHTML=`<div class="u-ico">${u.i}</div><div class="u-info"><div class="u-name">${u.n}</div><div class="u-desc">${u.d}</div></div><div class="u-r">${l>0?`<div class="u-lvl">niv.${l}</div>`:''}<div class="u-cost">${fmt(c)}✨</div></div>`;el.onclick=()=>buyUpgrade(u.id);s.appendChild(el);});});}
function renderPets(){const l=document.getElementById('pet-list');l.innerHTML='';pets.forEach(p=>{const d=document.createElement('div');d.className='ps';d.innerHTML=`${p.icon}<span class="pb2">${p.tier===3?'👑':p.tier===2?'⭐':'·'}</span>`;l.appendChild(d);});document.getElementById('pet-cnt').textContent=pets.length;document.getElementById('pet-pw').textContent=`+${fmt(Math.round(petPow()*multi))}/s`;}
function renderSkins(){const g=document.getElementById('skin-list');g.innerHTML='';skins.forEach(s=>{const b=document.createElement('div');b.className='sk'+(curSkin===s?' act':'');b.textContent=s;b.onclick=()=>{curSkin=s;updateUI();};g.appendChild(b);});document.getElementById('skin-cnt').textContent=skins.length;}
function save(){SS('nb_score',score);SS('nb_multi',multi);SS('nb_skins',skins);SS('nb_at',allTime);}
function floatTxt(x,y,txt,color,big){const el=document.createElement('div');el.className='ftxt'+(big?' big':'');el.style.cssText=`left:${x}px;top:${y}px;color:${color||'#ffd700'};text-shadow:0 0 8px ${color||'#ffd700'}`;el.textContent=txt;document.body.appendChild(el);setTimeout(()=>el.remove(),950);}
function ring(x,y){const r=document.createElement('div');r.className='rp';r.style.cssText=`left:${x}px;top:${y}px`;document.body.appendChild(r);setTimeout(()=>r.remove(),420);}
function shake(){document.body.classList.add('shk');setTimeout(()=>document.body.classList.remove('shk'),200);}
function flash(type){const f=document.getElementById('pflash');f.className='';void f.offsetWidth;f.classList.add(type);}
function toast(ico,title,msg){const c=document.getElementById('toasts'),t=document.createElement('div');t.className='toast';t.innerHTML=`<div class="t-i">${ico}</div><div><div class="t-tl">${title}</div><div class="t-m">${msg}</div></div>`;c.appendChild(t);setTimeout(()=>{t.classList.add('out');setTimeout(()=>t.remove(),320);},3500);}
function openModal(id){document.getElementById(id).classList.add('on');}
function closeModal(id){document.getElementById(id).classList.remove('on');}
function saveName(){const v=document.getElementById('name-inp').value.trim().replace(/[<>]/g,'');if(!v)return;pname=v;SS('nb_name',v);closeModal('name-modal');if(sbReady){mpPushScore();}else{initSupabase();}updateUI();}
document.getElementById('name-inp').addEventListener('keydown',e=>{if(e.key==='Enter')saveName();});
function updateUI(){document.getElementById('pname').textContent=pname||'🚀 Choisir un pseudo';document.getElementById('score').textContent=fmt(score);document.getElementById('mult').textContent=(multi*evMult).toFixed(2);document.getElementById('p-lvl').textContent=prestige;document.getElementById('mp-lvl').textContent=metaP>0?metaP+'★':0;const pcost=PTIERS[0].cost(prestige);document.getElementById('p-cost').textContent=fmt(pcost);document.getElementById('p-btn').disabled=score<pcost;document.getElementById('b-wave').textContent=bossWave;document.getElementById('b-kills').textContent=bossKills;const em=Math.max(1,pets.length*.5+1);document.getElementById('e1c').textContent=fmt(Math.floor(500*em));document.getElementById('e2c').textContent=fmt(Math.floor(5000*em));document.getElementById('e3c').textContent=fmt(Math.floor(50000*em));document.getElementById('rc1').textContent=fmt(skC());document.getElementById('rc5').textContent=fmt(skC(5));renderSkins();renderPets();renderUpgrades();renderAchs();renderPb();save();}
initUpgrades();renderAchs();buildGiftDays();renderLeaderboard([]);updateUI();
initSupabase();
if(!pname)setTimeout(()=>openModal('name-modal'),800);
setInterval(checkAchs,4000);setInterval(buildGiftDays,60000);
const tod=new Date().toDateString();
if(lastGift!==tod)setTimeout(()=>toast('🎁','CADEAU QUOTIDIEN!','Clique pour le réclamer!'),5000);
</script>
</body>
</html>
