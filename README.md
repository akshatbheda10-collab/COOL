<!DOCTYPE html>
<html lang="en"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="robots" content="noindex"><title>Teacher Code Generator</title>
<style>
body{margin:0;min-height:100vh;display:grid;place-items:center;background:#070514;color:#f4f2ff;font-family:system-ui,Segoe UI,Arial,sans-serif;text-align:center;padding:16px}
.card{background:#14102e;border:1px solid #ffffff22;border-radius:22px;padding:32px 28px;max-width:420px;width:100%}
.code{font:800 76px ui-monospace,Consolas,monospace;letter-spacing:.18em;color:#fde68a;text-shadow:0 0 30px #fbbf2480;margin:18px 0 6px;padding-left:.18em}
p{color:#a9a4d3;font-size:14px;margin:8px 0}
button{margin-top:14px;padding:10px 18px;border:0;border-radius:12px;background:#6c5ce7;color:#fff;font-weight:700;font-size:15px;cursor:pointer}
#st{font-size:12px}
</style></head><body><div class="card">
<div style="font-size:40px">🔑</div><h1 style="margin:6px 0">Teacher Code</h1>
<div class="code" id="code">----</div>
<p>Type this into the Teacher Lock on Star Champions along with the password.</p>
<p>It works <b>once</b>. After it's used, a new code appears here automatically.</p>
<button onclick="newCode()">Make a new code now</button>
<p id="st">Connecting…</p></div>
<script>
const URL_="https://wtqqyadgolqbtoqizfvd.supabase.co",KEY="sb_publishable_TnD-uSOe2lD40FXzP52OKQ_7hjfaC0v",ROOM="TEACHER-ACCESS";
let cur=null;
const H={apikey:KEY,Authorization:"Bearer "+KEY,"Content-Type":"application/json"};
const T=URL_+"/rest/v1/star_champions_rooms";
const show=c=>{cur=c;document.getElementById('code').textContent=c;};
const st=t=>document.getElementById('st').textContent=t;
async function read(){const r=await fetch(T+"?room=eq."+ROOM+"&select=json",{headers:H});if(!r.ok)throw new Error(await r.text());const d=await r.json();return d[0]&&d[0].json&&d[0].json.code;}
async function newCode(){try{const c=String(Math.floor(Math.random()*10000)).padStart(4,'0'),t=Date.now();
 const r=await fetch(T+"?on_conflict=room",{method:"POST",headers:Object.assign({Prefer:"resolution=merge-duplicates,return=minimal"},H),body:JSON.stringify({room:ROOM,json:{code:c,savedAt:t},saved_at:t})});
 if(!r.ok)throw new Error(await r.text());show(c);st('New code ready.');}catch(e){st('Could not save code: '+e.message);}}
async function poll(){try{const c=await read();if(!c)return newCode();if(c!==cur){show(c);st('Code updated.');}else st('Live • waiting for use');}catch(e){st('Connection problem: '+e.message);}}
window.addEventListener('load',()=>{poll();setInterval(poll,3000);});
</script></body></html>
