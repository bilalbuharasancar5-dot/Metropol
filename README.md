# Metropol
<!DOCTYPE html>
<html lang="tr"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>Metropol</title>
<style>
html,body{margin:0;height:100%;overflow:hidden;background:#101820;font-family:system-ui,sans-serif;touch-action:none;user-select:none;-webkit-user-select:none;color:#fff}
canvas{display:block;width:100%;height:100%}
.ab{position:fixed;pointer-events:none;text-shadow:0 1px 4px #000}
#hud{left:0;right:0;top:0;padding:calc(env(safe-area-inset-top,0px) + 8px) calc(env(safe-area-inset-right,0px) + 12px) 0 calc(env(safe-area-inset-left,0px) + 12px);display:flex;justify-content:space-between}
#sub{font-size:12px;letter-spacing:1px}#dist{font-size:21px;font-weight:700}#mny{font-size:14px;color:#9be37a;font-weight:600}
#hint{font-size:11px;opacity:.75;text-align:right;max-width:38%}
#wp,#sb{left:50%;transform:translateX(-50%);font-size:13px;background:rgba(0,0,0,.5);padding:4px 10px;border-radius:10px;display:none;white-space:nowrap}
#wp{top:calc(env(safe-area-inset-top,0px) + 8px)}#sb{top:calc(env(safe-area-inset-top,0px) + 40px)}
#toast{top:24%;left:50%;transform:translateX(-50%);background:rgba(0,0,0,.72);padding:8px 14px;border-radius:12px;font-size:14px;opacity:0;transition:opacity .3s;text-align:center;max-width:80vw}
.btn{padding:12px 16px;border:0;border-radius:24px;background:rgba(20,20,20,.72);color:#fff;font-size:15px;font-weight:600}
#bt{position:fixed;right:calc(env(safe-area-inset-right,0px) + 14px);bottom:calc(env(safe-area-inset-bottom,0px) + 20px);display:flex;flex-direction:column;gap:8px;align-items:flex-end}
#ctx{background:#e67e22;display:none}
#fs{position:fixed;right:calc(env(safe-area-inset-right,0px) + 12px);top:calc(env(safe-area-inset-top,0px) + 58px);padding:6px 10px;border-radius:10px}
#joy{position:fixed;left:calc(env(safe-area-inset-left,0px) + 20px);bottom:calc(env(safe-area-inset-bottom,0px) + 24px);width:120px;height:120px;border-radius:50%;background:rgba(255,255,255,.15);border:2px solid rgba(255,255,255,.35)}
#knob{position:absolute;left:35px;top:35px;width:50px;height:50px;border-radius:50%;background:rgba(255,255,255,.55)}
#chat{position:fixed;left:0;right:0;bottom:0;display:none;flex-direction:column;padding:12px 12px calc(env(safe-area-inset-bottom,0px) + 12px);background:rgba(15,18,24,.94);border-radius:16px 16px 0 0}
#chat h4{margin:0 0 6px;font-size:15px;display:flex;justify-content:space-between;align-items:center}
#x{background:none;border:0;color:#fff;font-size:20px}
#log{max-height:28vh;overflow:auto;font-size:15px;line-height:1.4;touch-action:pan-y}#log p{margin:4px 0}.me{color:#8fd3ff}.npc{color:#ffd88a}
#row{display:flex;gap:8px;margin-top:8px}
#msg{flex:1;padding:10px;border-radius:10px;border:0;font-size:16px;min-width:0}
#send{padding:10px 16px;border:0;border-radius:10px;background:#e67e22;color:#fff;font-weight:700;font-size:15px}
#ph{position:fixed;left:50%;top:50%;transform:translate(-50%,-50%);width:min(360px,94vw);height:min(600px,90vh);background:#12161c;border:3px solid #333;border-radius:26px;display:none;flex-direction:column;overflow:hidden;box-shadow:0 8px 40px #000}
.phh{display:flex;justify-content:space-between;align-items:center;padding:10px 14px;background:#0b0e12;font-size:14px}
.grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;padding:16px;align-content:start}
.grid button{background:#232a33;border:0;border-radius:16px;color:#fff;padding:12px 4px;font-size:12px}
.grid i{display:block;font-style:normal;font-size:28px}
.bar{display:flex;gap:10px;align-items:center;padding:8px 12px;background:#1a2028}
.pc{overflow:auto;padding:10px;flex:1;font-size:14px;touch-action:pan-y}
.po,.it{background:#1d232b;border-radius:10px;padding:8px;margin:6px 0}
.sm{font-size:12px;opacity:.7}
#ph button.a{background:#e67e22;border:0;color:#fff;border-radius:8px;padding:6px 10px;font-size:13px;margin:3px 3px 0 0}
.prow{display:flex;gap:6px}.prow input{flex:1;min-width:0;border-radius:8px;border:0;padding:8px;font-size:16px}
@media (orientation:landscape) and (max-height:520px){
#chat{left:auto;top:0;width:46%;border-radius:16px 0 0 16px}#log{max-height:none;flex:1}
#hint{display:none}.btn{padding:9px 14px;font-size:14px}#joy{width:100px;height:100px}#knob{left:25px;top:25px}}
body.menu #hud,body.menu #joy,body.menu #bt,body.menu #fs,body.menu #wp,body.menu #sb,body.menu #mb{display:none!important}
#vig{position:fixed;inset:0;pointer-events:none;background:radial-gradient(ellipse at center,transparent 55%,rgba(0,0,0,.45) 100%)}
.lb{position:fixed;left:0;right:0;height:0;background:#000;pointer-events:none;transition:height 1.2s}
#lt{top:0}#lbb{bottom:0}
#fade{position:fixed;inset:0;background:#000;opacity:0;pointer-events:none;transition:opacity .5s}
#menu{position:fixed;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;text-align:center;background:linear-gradient(180deg,rgba(0,0,0,.4),rgba(0,0,0,.1) 45%,rgba(0,0,0,.65))}
#menu h1{margin:0;font-size:clamp(38px,11vw,84px);letter-spacing:.18em;font-weight:800;text-shadow:0 4px 30px #000}
#menu .sb2{font-size:13px;letter-spacing:.4em;opacity:.85;margin-bottom:12px}
#menu button{min-width:230px;padding:13px 20px;border:1px solid rgba(255,255,255,.4);border-radius:6px;background:rgba(0,0,0,.5);color:#fff;font-size:17px;letter-spacing:.06em}
#menu button.p{background:#c0392b;border-color:#e74c3c}#menu input{min-width:210px;padding:10px;border-radius:6px;border:1px solid rgba(255,255,255,.4);background:rgba(0,0,0,.5);color:#fff;font-size:15px;text-align:center}
#help{max-width:520px;margin:0 12px;font-size:13px;line-height:1.5;background:rgba(0,0,0,.6);padding:10px 14px;border-radius:8px;display:none}
#mb{position:fixed;right:calc(env(safe-area-inset-right,0px) + 12px);top:calc(env(safe-area-inset-top,0px) + 98px);padding:6px 10px;border-radius:10px}
@media (max-height:520px){#menu{gap:6px}#menu h1{font-size:38px}#menu .sb2{margin-bottom:2px}#menu button{padding:8px 16px;font-size:15px}}
#mc{width:100%;border-radius:8px;display:block}#cact{display:flex;gap:6px;flex-wrap:wrap;margin-top:6px}#cact button{background:#e67e22;border:0;color:#fff;border-radius:8px;padding:6px 10px;font-size:13px}
</style></head><body><div id="vig"></div>
<div class="ab" id="hud"><div><div id="sub"></div><div id="dist">İstanbul</div><div id="mny"></div></div><div id="hint">WASD yürü/sür · Shift koş · E etkileşim · sürükle: kamera</div></div>
<div class="ab" id="wp"></div><div class="ab" id="sb"></div><div class="ab" id="toast"></div>
<button class="btn" id="fs">⛶</button><button class="btn" id="mb">☰</button>
<div id="joy"><div id="knob"></div></div>
<div id="bt"><button class="btn" id="ctx"></button><button class="btn" id="mapb">🗺️ Harita</button><button class="btn" id="run">Koş: Kapalı</button><button class="btn" id="pb">📱 Telefon</button></div>
<div id="ph"></div>
<div class="lb" id="lt"></div><div class="lb" id="lbb"></div>
<div id="menu"><h1>METROPOL</h1><div class="sb2">İSTANBUL · AÇIK DÜNYA</div><input id="nick" maxlength="14" placeholder="Takma adın"><input id="roomc" maxlength="24" placeholder="Oda kodu (online, isteğe bağlı)"><button class="p" id="mstart">▶ Oyuna Başla</button><button id="mmus">🎵 Müzik: Açık</button><button id="msnd">🔊 Ses: Açık</button><button id="mhelp">❓ Nasıl Oynanır</button><div id="help">Yürü: sol joystick / WASD · Koş: Shift · Kamera: sürükle · Etkileşim: turuncu düğme / E · Telefon: 📱 · Arabaya yaklaşıp bin · Telefondan bilet al, stada gir, ev satın al, iş bul · Banka ve ATM'ler Merkez'de ve sahilde · Hız yaparsan polis ceza keser! · ✈️ Havalimanından Amerika (3 dk), İngiltere (2 dk) ve İspanya'ya (1 dk) uç · 🌐 Online: aynı oda kodunu yazan arkadaşınla birlikte oyna!</div></div>
<div id="chat"><h4><span id="cn"></span><button id="x">✕</button></h4><div id="log"></div><div id="cact"></div><div id="row"><input id="msg" placeholder="Bir şey yaz..." autocomplete="off"><button id="send">Gönder</button></div></div>
<div id="fade"></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
const $=id=>document.getElementById(id),T=THREE,PI=Math.PI,rnd=(a,b)=>a+Math.random()*(b-a),pick=a=>a[Math.floor(Math.random()*a.length)],cl=(v,a,b)=>Math.max(a,Math.min(b,v)),rr=x=>{x=Math.sin(x)*43758.5453;return x-Math.floor(x)},fmt=n=>Math.round(n).toLocaleString('tr-TR')+' ₺',esc=s=>String(s).replace(/[<>&"]/g,'');
const S={money:1e8,bank:0,wa:{},wu:0,friends:[],msel:-1,hap:70,rel:{},spouse:null,kids:[],kidDue:false,following:[],dm:[],unread:0,mine:[],tix:{},job:null,ms:null,fol:120,sp:0,feed:[],wp:null,day:0,ls:[]};
let gt=170,LF=1,ct=0,cxm=0,lastCt=0,flight=null,curFlag=null,curReg='',rainI=0,mode='menu',started=false,itime=0,fineT=0,barsOn=true,chatOpen=false,phoneOpen=false,inside=null,car=null,watch=null,chatNpc=null,yaw=0,pitch=.35,dist=7,run=false,spd=0,wph=0,app='';
const scene=new T.Scene(),sky=new T.Color(0x9ec9e8);scene.background=sky;scene.fog=new T.Fog(0x9ec9e8,70,330);
const cam=new T.PerspectiveCamera(60,innerWidth/innerHeight,.1,600);
const ren=new T.WebGLRenderer({antialias:true});ren.setPixelRatio(Math.min(devicePixelRatio||1,2));ren.setSize(innerWidth,innerHeight);document.body.prepend(ren.domElement);document.body.className='menu';
const hemi=new T.HemisphereLight(0xffffff,0x556655,.85),sun=new T.DirectionalLight(0xfff2d0,.8);sun.position.set(80,120,40);scene.add(hemi,sun,sun.target);sun.castShadow=true;sun.shadow.mapSize.set(2048,2048);{const q=sun.shadow.camera;q.left=q.bottom=-70;q.right=q.top=70;q.near=10;q.far=400}sun.shadow.bias=-.0008;
const mc={},MT=c=>mc[c]||(mc[c]=new T.MeshLambertMaterial({color:c}));
const bgc={},BG=(w,h,d)=>bgc[[w,h,d]]||(bgc[[w,h,d]]=new T.BoxGeometry(w,h,d));
const box=(w,h,d,c,x,y,z,p)=>{const m=new T.Mesh(BG(w,h,d),MT(c));m.position.set(x,y,z);(p||scene).add(m);return m};
const cyl=(r1,r2,h,c,x,y,z,s)=>{const m=new T.Mesh(new T.CylinderGeometry(r1,r2,h,s||14),MT(c));m.position.set(x,y,z);scene.add(m);return m};
const flat=(w,d,c,x,y,z,mt)=>{const m=new T.Mesh(new T.PlaneGeometry(w,d),mt||MT(c));m.rotation.x=-PI/2;m.position.set(x,y,z);scene.add(m);return m};
const sh=o=>o.traverse(m=>{if(m.isMesh){m.castShadow=m.geometry.type!='PlaneGeometry';m.receiveShadow=true}});
function nt(base,rep,w,h){const c=document.createElement('canvas');c.width=c.height=128;const g=c.getContext('2d');g.fillStyle=base;g.fillRect(0,0,128,128);for(let i=0;i<1100;i++){g.fillStyle=`rgba(${Math.random()<.5?'255,255,255':'0,0,0'},${Math.random()*.14})`;g.fillRect(Math.random()*128,Math.random()*128,2+Math.random()*3,2+Math.random()*3)}const t=new T.CanvasTexture(c);t.wrapS=t.wrapT=T.RepeatWrapping;t.repeat.set(w/rep,h/rep);t.anisotropy=8;return t}
function roadM(h){const c=document.createElement('canvas');c.width=c.height=128;const g=c.getContext('2d');g.fillStyle='#3a3a3d';g.fillRect(0,0,128,128);for(let i=0;i<700;i++){g.fillStyle=`rgba(${Math.random()<.5?'255,255,255':'0,0,0'},${Math.random()*.12})`;g.fillRect(Math.random()*128,Math.random()*128,2,2)}g.fillStyle='#e8e8e8';if(h)g.fillRect(0,62,64,4);else g.fillRect(62,0,4,64);const t=new T.CanvasTexture(c);t.wrapS=t.wrapT=T.RepeatWrapping;t.anisotropy=8;t.repeat.set(h?800/12:1,h?1:640/12);return new T.MeshStandardMaterial({map:t,roughness:.95})}
const flagTex=(()=>{const c=document.createElement('canvas');c.width=300;c.height=200;const g=c.getContext('2d');g.fillStyle='#e30a17';g.fillRect(0,0,300,200);g.fillStyle='#fff';g.beginPath();g.arc(112,100,50,0,PI*2);g.fill();g.fillStyle='#e30a17';g.beginPath();g.arc(128,100,40,0,PI*2);g.fill();g.fillStyle='#fff';g.beginPath();for(let i=0;i<10;i++){const a=-PI/2+i*PI/5,r=i%2?9:22;g.lineTo(168+Math.cos(a)*r,100+Math.sin(a)*r)}g.closePath();g.fill();return new T.CanvasTexture(c)})();
const flagM=new T.MeshBasicMaterial({map:flagTex,side:T.DoubleSide}),flagG=new T.PlaneGeometry(3,2),FL=[],PRM=new T.MeshBasicMaterial({color:0xff2020}),PBM=new T.MeshBasicMaterial({color:0x2060ff}),PLR=[],PLB=[];
function flagPole(x,y,z,h){cyl(.07,.09,h,0xcccccc,x,y+h/2,z,6);const f=new T.Mesh(flagG,curFlag||flagM);f.position.set(x+1.5,y+h-1,z);scene.add(f);FL.push(f)}
function lab(t,w,h,fs){const c=document.createElement('canvas');c.width=512;c.height=Math.round(512*h/w);const g=c.getContext('2d');g.fillStyle='#fff';g.font='bold '+fs+'px sans-serif';g.textAlign='center';g.textBaseline='middle';g.shadowColor='#000';g.shadowBlur=8;g.fillText(t,256,c.height/2);const s=new T.Sprite(new T.SpriteMaterial({map:new T.CanvasTexture(c),transparent:true,fog:false}));s.scale.set(w,h,1);return s}
const lampMat=new T.MeshBasicMaterial({color:0x3a3a3a});
const lm=(w,h,d,x,y,z)=>{const m=new T.Mesh(BG(w,h,d),lampMat);m.position.set(x,y,z);scene.add(m);return m};
// pencere dokuları
const wc=document.createElement('canvas'),ec=document.createElement('canvas');wc.width=wc.height=ec.width=ec.height=256;
{const g=wc.getContext('2d'),e=ec.getContext('2d');g.fillStyle='#fff';g.fillRect(0,0,256,256);g.fillStyle='#d8d8d8';g.fillRect(0,236,256,6);e.fillStyle='#000';e.fillRect(0,0,256,256);
[[24,40],[148,40]].forEach(([x,y])=>{g.fillStyle='#444c55';g.fillRect(x-6,y-6,92,148);const gr=g.createLinearGradient(x,y,x+80,y+136);gr.addColorStop(0,'#9fc3e6');gr.addColorStop(.5,'#4f6f90');gr.addColorStop(1,'#1f2b3a');g.fillStyle=gr;g.fillRect(x,y,80,136);g.fillStyle='rgba(255,255,255,.28)';g.beginPath();g.moveTo(x+8,y);g.lineTo(x+34,y);g.lineTo(x+8,y+70);g.closePath();g.fill();g.fillStyle='#444c55';g.fillRect(x+37,y,6,136);g.fillRect(x,y+60,80,5);g.fillStyle='#bbb';g.fillRect(x-10,y+142,100,8);
e.fillStyle='#ffd27a';e.fillRect(x,y,80,136);e.fillStyle='#000';e.fillRect(x+37,y,6,136);e.fillRect(x,y+60,80,5)});
for(let i=0;i<2000;i++){g.fillStyle=`rgba(0,0,0,${Math.random()*.05})`;g.fillRect(Math.random()*256,Math.random()*256,2,2)}}
const wt=new T.CanvasTexture(wc),et=new T.CanvasTexture(ec);[wt,et].forEach(t=>{t.wrapS=t.wrapT=T.RepeatWrapping;t.anisotropy=8});
const bm={},BM=c=>bm[c]||(bm[c]=new T.MeshStandardMaterial({color:c,map:wt,roughness:.8,metalness:.05,emissive:0xffffff,emissiveMap:et,emissiveIntensity:0}));const cols=[],H=[],trees=[];
function bld(x,z,w,d,h,c){const g=new T.BoxGeometry(w,h,d),u=g.attributes.uv,rx=Math.max(1,Math.round(w/4)),ry=Math.max(1,Math.round(h/4));for(let i=0;i<u.count;i++)u.setXY(i,u.getX(i)*rx,u.getY(i)*ry);const m=new T.Mesh(g,BM(c));m.position.set(x,h/2,z);scene.add(m);cols.push({x,z,hw:w/2,hd:d/2})}
// ---------- dünya ----------
const HR=[-20,60,195],VR=[-70,0,80];
const GRM=new T.MeshStandardMaterial({map:nt('#6f8a55',8,800,640),roughness:1}),PVM=new T.MeshStandardMaterial({map:nt('#9b9b9b',2,800,2),roughness:.9}),SEAM=new T.MeshStandardMaterial({map:nt('#2a6f9e',16,800,200),roughness:.25,metalness:.2}),RDH=roadM(1),RDV=roadM(0);
flat(800,640,0,0,0,-90,GRM);flat(800,200,0,0,-.3,330,SEAM);
HR.forEach(z=>{flat(800,12,0,0,.05,z,RDH);flat(800,2,0,0,.055,z-7,PVM);flat(800,2,0,0,.055,z+7,PVM)});
VR.forEach(x=>{flat(12,640,0,x,.06,-90,RDV);flat(2,640,0,x-7,.065,-90,PVM);flat(2,640,0,x+7,.065,-90,PVM)});
// Boğaz ve köprüler
const BRX=[-70,0,80],PIERS=[-60,225,250];
flat(800,24,0,0,.03,20,SEAM);
[[-400,-77],[-63,-7],[7,73],[87,400]].forEach(q=>[8,32].forEach(zz=>box(q[1]-q[0],.5,1.2,0x7a7a7a,(q[0]+q[1])/2,.25,zz)));
BRX.forEach(x=>{box(16,.7,26,0x8e8e8e,x,-.3,20);[-8,8].forEach(sx=>box(.25,1.1,26,0xbbbbbb,x+sx,.6,20))});
[-9.5,9.5].forEach(sx=>{[6,34].forEach(zz=>{box(1.4,42,1.4,0xc0392b,sx,21,zz);cols.push({x:sx,z:zz,r:1.2})});
const cv=new T.QuadraticBezierCurve3(new T.Vector3(sx,40,6),new T.Vector3(sx,-30,20),new T.Vector3(sx,40,34));scene.add(new T.Mesh(new T.TubeGeometry(cv,24,.25,6),MT(0xdddddd)));
for(let i=1;i<12;i++){const pt=cv.getPoint(i/12);box(.1,pt.y,.1,0xcccccc,pt.x,pt.y/2,pt.z)}});
[6,34].forEach(zz=>box(20,1,1,0xc0392b,0,38,zz));
const bl_=lab('BOĞAZ KÖPRÜSÜ',30,6,48);bl_.position.set(0,46,20);scene.add(bl_);
const nr=(x,z,m)=>HR.some(r=>Math.abs(z-r)<6+m)||VR.some(r=>Math.abs(x-r)<6+m);
const pastel=[0xe8d5b7,0xd9b8a0,0xc9d6e8,0xe6c3c3,0xd5e3c0,0xf0e0a8],low=[0xf2ece0,0xd9a88a,0xe8d9b0,0xcfd8dc];
const D=[{n:'Seyrantepe',x:-150,z:-130,k:'seyrantepe|rams|galatasaray stad'},{n:'Tüpraş',x:150,z:-130,k:'tüpraş|tupras|rafineri|baca'},{n:'Kadıköy',x:-120,z:100,k:'kadıköy|kadikoy|iskele|boğa'},{n:'Maltepe',x:150,z:100,k:'maltepe|sahil'},{n:'Şükrü Saracoğlu',x:-245,z:95,k:'şükrü|saracoğlu|fener stad|fb stad'},{n:'Banka',x:40,z:-50,k:'banka|atm',nl:1},{n:'Karakol',x:-35,z:-100,k:'karakol|polis',nl:1},{n:'Pendik',x:235,z:115,k:'pendik|marina|sabiha|havalimanı|havaalanı'},{n:'Boğaz Köprüsü',x:0,z:20,k:'köprü|boğaz',nl:1}];
D.forEach(d=>{if(d.nl)return;const s=lab(d.n.toUpperCase(),60,15,d.n.length>11?46:70);s.position.set(d.x,48,d.z);scene.add(s)});
const zones=[[-150,-130,92],[150,-130,78],[-245,95,52],[150,115,24],[-170,150,22],[-35,-55,22],[40,-50,20],[-35,-100,20],[-245,155,30],[-215,-52,30],[50,-107,30],[-120,148,30],[130,166,30],[220,158,30],[112,100,16],[196,100,12]],inZ=(x,z)=>zones.some(q=>Math.hypot(x-q[0],z-q[1])<q[2])||(x>240&&z>36&&z<192);
const dn=(x,z)=>x<-5&&z>36?'Kadıköy':x>205&&z>36?'Pendik':x>40&&z>36?'Maltepe':'Merkez';
const GEO={Kadıköy:[12,34,pastel],Pendik:[8,18,[0xf4f4f4,0xcfe3f2,0xe9dfc9,0x9fc3e6]],Merkez:[14,40,[0x9fb3c8,0xb8c4d0,0x8fa3b5,0xc9ced3]]};
function suburb(x,z,w,d,h){const c=pick([0xf2ece0,0xd9c9a8,0xb7c9d9,0xe3d6c4,0xcfd8c0,0xf4e4e0]);bld(x,z,w,d,h,c);
const r=(w+.8)/Math.sqrt(3),rf=new T.Mesh(new T.CylinderGeometry(r,r,d+1,3),MT(pick([0x7a3b2e,0x4a4f55,0x5b4636])));rf.rotation.x=-PI/2;rf.scale.z=.4;rf.position.set(x,h+r*.2,z);scene.add(rf);
box(w*.5,.25,2.2,0xcfc7b6,x,.12,z+d/2+1.1);const gx=x+w/2+2.4;box(4.4,3,d*.8,c,gx,1.5,z-d*.1);cols.push({x:gx,z:z-d*.1,hw:2.2,hd:d*.4});box(3.4,2.3,.12,0xe8e8e8,gx,1.2,z+d*.3+.07);flat(3.6,11,0x8a8a8a,gx,.05,z+d/2+4);
flat(w+9,12,0x5f9e45,x,.038,z+d/2+6.5);box(w+9,.9,.1,0xffffff,x,.45,z+d/2+12.5);box(.1,.9,12,0xffffff,x-(w+9)/2,.45,z+d/2+6.5);box(.1,.9,12,0xffffff,x+(w+9)/2,.45,z+d/2+6.5);
box(.12,1,.12,0x555555,x-w/2-1.5,.5,z+d/2+11.5);box(.35,.3,.5,0x2255aa,x-w/2-1.5,1.1,z+d/2+11.5);trees.push([x-w/2-3,z+d/2+4])}
for(let gx=-270;gx<=270;gx+=24)for(let gz=-280;gz<=175;gz+=24){
const x=gx+rnd(-3,3),z=gz+rnd(-3,3);if(inZ(x,z)||Math.abs(z-20)<30||Math.random()<.12)continue;
const k=dn(x,z),sb=k=='Maltepe',w=sb?rnd(9,11):rnd(10,15),d=sb?rnd(8,9):rnd(10,15);if(nr(x,z,Math.max(w,d)/2+1)||(sb&&nr(x,z+8,13)))continue;
const cfg=GEO[k]||GEO.Merkez,h=sb?rnd(3.6,4.4):rnd(cfg[0],cfg[1]);
if(sb)suburb(x,z,w,d,h);else bld(x,z,w,d,h,pick(cfg[2]));if(h>16&&Math.random()<.22)flagPole(x+w/3,h,z,5);
if(H.length<150&&(sb||Math.random()<.3)){const id=H.length,ty=h<10?'Müstakil Ev':'Daire';H.push({id,x,z,w,d,dx:x,dz:z+d/2+1.4,dn:k,type:ty,name:k+' '+ty+' #'+(id+1),price:Math.round(rnd(k=='Maltepe'?5e6:3.5e6,k=='Maltepe'?14e6:12e6)/5e4)*5e4,own:false})}
if(!sb&&Math.random()<.6)trees.push([x+w/2+3,z+d/2+3])}
for(let i=0;i<50;i++){const x=rnd(70,235),z=rnd(40,175);if(!nr(x,z,3)&&Math.hypot(x-150,z-115)>22)trees.push([x,z])}
// Kadıköy
flat(220,26,0x9a9a9a,-120,.04,217);
box(3,1.5,5,0x777777,-95,.75,208);box(1.8,1.2,3.2,0x4a3a2a,-95,2.1,208);box(1,1,1.2,0x4a3a2a,-95,2.7,209.8);
const bs=lab('Boğa Heykeli',8,2,52);bs.position.set(-95,6,208);scene.add(bs);
box(7,.6,36,0x8a6a3a,-60,.3,246);box(7,5,8,0xd8d0c0,-60,2.5,232);cols.push({x:-60,z:232,hw:3.5,hd:4});
// Maltepe
flat(190,26,0xd9c9a0,150,.04,217);
function mosque(x,z){box(14,9,14,0xeeeae0,x,4.5,z);cols.push({x,z,hw:7,hd:7});box(18,.4,18,0xcfcabb,x,.2,z);
const d=new T.Mesh(new T.SphereGeometry(6,16,10,0,PI*2,0,PI/2),MT(0x7f9bb8));d.position.set(x,9,z);scene.add(d);
[[-1,-1],[1,-1]].forEach(([sx,sz])=>{const mx=x+sx*9.5,mz=z+sz*9.5;cyl(.8,1,26,0xf5f5f5,mx,13,mz,12);const b=new T.Mesh(new T.TorusGeometry(1.3,.25,6,12),MT(0xdddddd));b.rotation.x=PI/2;b.position.set(mx,20,mz);scene.add(b);cyl(0,1.2,6,0x7f9bb8,mx,29,mz,12);cyl(.05,.05,2,0xffd24a,mx,33,mz,5);cols.push({x:mx,z:mz,r:1.2})});
cyl(.1,.1,3,0xffd24a,x,15.5,z,5);flagPole(x+10,0,z+9,10)}
mosque(150,115);mosque(-170,150);mosque(-35,-55);
function diner(x,z){box(14,4,9,0xd9d9d9,x,2,z);cols.push({x,z,hw:7,hd:4.5});box(14.2,.8,9.2,0xc0392b,x,4.4,z);box(14.2,.5,.3,0xc0392b,x,.9,z+4.6);const q=lab('DINER',9,2.6,80);q.position.set(x,7,z+4.6);scene.add(q);flat(26,16,0x3a3d40,x,.035,z+12)}
function wtower(x,z){[[-2,-2],[2,-2],[-2,2],[2,2]].forEach(q=>cyl(.25,.25,14,0x888888,x+q[0],7,z+q[1],6));cyl(4,4,5,0xeeeeee,x,16.5,z,16);cyl(0,4.4,2.5,0xc0392b,x,20,z,16);const q=lab('MALTEPE',8,2,60);q.position.set(x,16.5,z+4.2);scene.add(q);cols.push({x,z,r:3})}
diner(112,100);wtower(196,100);
for(let i=0;i<5;i++)flagPole(-215+i*45,0,208,9);for(let i=0;i<4;i++)flagPole(80+i*45,0,208,9);flagPole(95,0,-75,12);
// Tüpraş
const TX=150,TZ=-130;flat(130,110,0x555a5f,TX,.03,TZ);
[[-25,-20],[0,-25],[25,-20],[-22,15],[5,10],[30,15]].forEach(([a,b])=>{cyl(9,9,12,0xe6e6e6,TX+a,6,TZ+b,18);cyl(9.4,9.4,.8,0xbbbbbb,TX+a,12.4,TZ+b,18);cols.push({x:TX+a,z:TZ+b,r:9.6})});
box(60,.6,.6,0x777777,TX,5,TZ-2);box(.6,.6,40,0x777777,TX-10,5,TZ);
for(let i=0;i<6;i++)cyl(1.5,1.8,10,i%2?0xffffff:0xcc2222,TX+45,5+i*10,TZ-5);cols.push({x:TX+45,z:TZ-5,r:2.4});
cyl(.3,.4,40,0x666666,TX-45,20,TZ-5,8);
const fl=new T.Mesh(new T.SphereGeometry(1,8,6),new T.MeshBasicMaterial({color:0xff8a33}));fl.position.set(TX-45,41,TZ-5);scene.add(fl);
const fli=new T.PointLight(0xff8833,1.2,90);fli.position.copy(fl.position);scene.add(fli);
// banka, karakol, ATM, uçaklar
function bank(x,z){box(22,13,14,0xd8d2c4,x,6.5,z);cols.push({x,z,hw:11,hd:7});box(24,1.2,16,0xb9b2a0,x,13.6,z);
for(let i=0;i<5;i++)cyl(.8,.8,11,0xeeeeee,x-8+i*4,5.5,z+7.8,10);box(24,.8,2.5,0xc9c2b0,x,11.4,z+7.8);
const s=lab('BANKA',16,4,150);s.position.set(x,16,z+8);scene.add(s);box(4,6,.3,0x223344,x,3,z+7.05);flagPole(x-9,13.6,z,6);flagPole(x+9,13.6,z,6)}
function police(x,z){box(18,8,12,0xf0f0f0,x,4,z);cols.push({x,z,hw:9,hd:6});box(18.2,1.6,12.2,0x1f4aa8,x,6.6,z);box(4,5,.3,0x223344,x,2.5,z+6.05);const s=lab('POLİS',12,3,130);s.position.set(x,11,z+6.5);scene.add(s);
const a=new T.Mesh(BG(1.4,.8,1.4),PRM),b=new T.Mesh(BG(1.4,.8,1.4),PBM);a.position.set(x-1.2,8.4,z);b.position.set(x+1.2,8.4,z);scene.add(a,b);PLR.push(a);PLB.push(b);flagPole(x+8,8,z-4,6)}
const ATM=[],BK={x:40,z:-40};
function atm(x,z){box(1,2.1,.9,0x555c66,x,1.05,z);const q=new T.Mesh(BG(.7,.5,.05),new T.MeshBasicMaterial({color:0x66ccff}));q.position.set(x,1.5,z+.48);scene.add(q);const l=lab('ATM',2,.7,60);l.position.set(x,2.7,z);scene.add(l);cols.push({x,z,r:.7});ATM.push({x,z})}
bank(40,-50);police(-35,-100);[[54,-43],[-125,208],[130,208],[-8.5,40],[95,-70],[-26,-92]].forEach(q=>atm(q[0],q[1]));
const PLN=[],pm=new T.MeshStandardMaterial({color:0xf4f4f4,fog:false,roughness:.5}),pmb=new T.MeshStandardMaterial({color:0x1f4aa8,fog:false});
function mkPlane(){const g=new T.Group(),add=(ge,m,x,y,z,rx)=>{const q=new T.Mesh(ge,m);q.position.set(x,y,z);if(rx)q.rotation.x=rx;g.add(q);return q},lt=c=>new T.MeshBasicMaterial({color:c,fog:false});
add(new T.CylinderGeometry(1.5,1.2,22,10),pm,0,0,0,PI/2);add(new T.SphereGeometry(1.5,10,8),pm,0,0,11);add(new T.CylinderGeometry(1.2,.4,5,10),pm,0,.3,-12.5,PI/2);
add(BG(26,.4,5),pm,0,-.4,1);add(BG(9,.3,2.5),pm,0,.2,-12.5);add(BG(.4,5,3),pmb,0,3,-12);[-6,6].forEach(x=>add(new T.CylinderGeometry(.8,.8,3.5,8),pm,x,-1.2,2,PI/2));
add(new T.SphereGeometry(.5,6,4),lt(0xff2020),-13,-.2,.5);add(new T.SphereGeometry(.5,6,4),lt(0x20ff40),13,-.2,.5);const ls=add(new T.SphereGeometry(.7,6,4),lt(0xffffff),0,3.8,-12);
scene.add(g);return{g,ls,x:rnd(-400,400),z:rnd(-400,400),a:rnd(0,PI*2),y:rnd(110,170),v:rnd(45,60)}}
for(let i=0;i<3;i++)PLN.push(mkPlane());
// Pendik
flat(60,26,0xd9c9a0,262,.04,217);const YT=[];
[225,250].forEach(x=>box(5,.6,34,0x8a6a3a,x,.3,245));
function yacht(x,z){const y=new T.Group();y.position.set(x,.15,z);box(3,1.2,8,0xf2f2f2,0,.4,0,y);box(2,1,3,0x9fb7c9,0,1.5,-.5,y);const m=new T.Mesh(new T.CylinderGeometry(.1,.1,9,6),MT(0xcccccc));m.position.set(0,5.5,1);y.add(m);box(.1,6,3.2,0xffffff,0,6,2.8,y);scene.add(y);YT.push(y)}
[225,250].forEach(px=>[[-5.5,236],[5.5,240],[-5.5,250],[5.5,254]].forEach(q=>yacht(px+q[0],q[1])));
for(let i=0;i<3;i++)flagPole(215+i*25,0,208,9);
flat(14,150,0x2f3236,282,.05,115);for(let z=48;z<185;z+=14)flat(.6,6,0xeeeeee,282,.06,z);flat(18,110,0x4a4e52,266,.045,115);
box(12,10,60,0xd8dde2,252,5,115);box(12.2,3,60.2,0x335577,252,6.5,115);cols.push({x:252,z:115,hw:6,hd:30});
const ts_=lab('SABİHA GÖKÇEN',22,4.3,50);ts_.position.set(252,14,115);scene.add(ts_);flagPole(244,0,85,12);
{const a=mkPlane();a.g.position.set(268,2.4,85);a.g.rotation.y=PI;const b=mkPlane();b.g.position.set(268,2.4,145)}
// sahil / plaj
flat(103,32,0xe6d5a8,166,.045,218);
{const um=new T.InstancedMesh(new T.ConeGeometry(1.8,.7,10),new T.MeshLambertMaterial({color:0xffffff}),26),pl_=new T.InstancedMesh(new T.CylinderGeometry(.05,.05,2.4,5),MT(0xeeeeee),26),lg=new T.InstancedMesh(BG(.7,.25,1.8),new T.MeshLambertMaterial({color:0xffffff}),26),ob=new T.Object3D(),c3=new T.Color();um.frustumCulled=pl_.frustumCulled=lg.frustumCulled=false;
for(let i=0;i<26;i++){const x=rnd(122,212),z=rnd(208,230);ob.position.set(x,2.4,z);ob.updateMatrix();um.setMatrixAt(i,ob.matrix);um.setColorAt(i,c3.setHex(pick([0xe74c3c,0xf1c40f,0x3498db,0xffffff,0x2ecc71])));ob.position.set(x,1.2,z);ob.updateMatrix();pl_.setMatrixAt(i,ob.matrix);ob.position.set(x+1.3,.15,z+1.2);ob.updateMatrix();lg.setMatrixAt(i,ob.matrix);lg.setColorAt(i,c3.setHex(pick([0xffffff,0x3498db,0xe67e22])))}scene.add(um,pl_,lg)}
box(2.4,.2,2.4,0xffffff,170,3.2,230);[[-1,-1],[1,-1],[-1,1],[1,1]].forEach(q=>box(.2,3.2,.2,0xdddddd,170+q[0],1.6,230+q[1]));box(2.8,1.6,2.8,0xe74c3c,170,4.2,230);const lgl=lab('CANKURTARAN',5,1.2,48);lgl.position.set(170,6.4,230);scene.add(lgl);
box(.1,2.4,.1,0xdddddd,140,1.2,224);box(.1,2.4,.1,0xdddddd,148,1.2,224);box(8,1,.05,0xffffff,144,1.9,224);
// stad otoparkları
[[-150,-60],[-245,155]].forEach(q=>{flat(70,34,0x3a3d40,q[0],.035,q[1]);for(let i=0;i<11;i++){flat(.25,5,0xffffff,q[0]-27.5+i*5.5,.04,q[1]-9);flat(.25,5,0xffffff,q[0]-27.5+i*5.5,.04,q[1]+9)}box(.3,.6,.3,0x555555,q[0]-33,.3,q[1]+15);const ps=lab('P · OTOPARK',9,2.4,52);ps.position.set(q[0]-33,3.6,q[1]+15);scene.add(ps)});
// okullar
const SCH=[['Seyrantepe İlkokulu',-215,-60],['Merkez Anadolu Lisesi',50,-115],['Kadıköy Fen Lisesi',-120,140],['Maltepe Koleji',130,158],['Pendik Ortaokulu',220,150]];
SCH.forEach(q=>{const x=q[1],z=q[2];bld(x,z,30,11,10,0xe8d9b0);flat(36,22,0x8f9a86,x,.04,z+17);box(36,.9,.12,0xffffff,x,.45,z+28);box(.12,.9,22,0xffffff,x-18,.45,z+17);box(.12,.9,22,0xffffff,x+18,.45,z+17);box(5,4,.4,0x6b3e1f,x,2,z+5.7);const sl=lab(q[0],24,4,34);sl.position.set(x,12.6,z);scene.add(sl);flagPole(x-14,10,z+4,5);flagPole(x+14,10,z+4,5)});
// metro & otobüs durakları
const MET=[['Seyrantepe',-100,-29],['Merkez · Banka',9,-60],['Tüpraş',89,-65],['Kadıköy',-61,100],['Maltepe',89,90],['Pendik',230,69],['Sabiha Gökçen',246,172]];
MET.forEach(q=>{const x=q[1],z=q[2];box(3.5,3.2,3.5,0xdde3ea,x,1.6,z);box(4.2,.3,4.2,0xc0392b,x,3.4,z);box(2.4,2.4,.2,0xc0392b,x,5.4,z);const m=lab('M',2,2,150);m.position.set(x,5.4,z+.2);scene.add(m);const l=lab(q[0],7,1.4,40);l.position.set(x,7.6,z);scene.add(l);cols.push({x,z,hw:2,hd:2})});
const BUS=[['x',60,1,[-190,-120,40,130,215],['Kadıköy Batı','Kadıköy Merkez','Boğaz Güney','Maltepe Merkez','Pendik Çarşı']],['x',-20,-1,[210,120,-60,-170],['Pendik Kuzey','Tüpraş Girişi','Merkez Çarşı','Seyrantepe']],['z',0,1,[-150,-60,70,150],['Seyrantepe Kuzey','Merkez Banka','Köprü Güney','Maltepe Batı']]],BST=[];
BUS.forEach(b=>b[3].forEach((q,i)=>{const x=b[0]=='x'?q:b[1]-9*b[2],z=b[0]=='x'?b[1]+9*b[2]:q;BST.push([b[4][i],x,z]);box(3.2,.15,1.4,0x335577,x,2.4,z);[-1.4,1.4].forEach(dx=>box(.1,2.4,.1,0x888888,x+dx,1.2,z));box(2.4,.3,.5,0x8a6a3a,x,.5,z);box(.7,.5,.1,0x1f4aa8,x+1.8,1.9,z);box(.15,1.6,.15,0x555555,x+1.8,.8,z);cols.push({x,z,hw:1.7,hd:.8})}));
const TRN=[...MET.map(q=>({x:q[1],z:q[2],t:'m'})),...BST.map(q=>({x:q[1],z:q[2],t:'b'}))];
// ---------- stadlar & maçlar ----------
const KIT={Galatasaray:[0xa00000,0xffb000],Fenerbahçe:[0xffd400,0x0a1f5c],Beşiktaş:[0x111111,0xffffff],Trabzonspor:[0x7a1f3d,0x2a6fcf],Kasımpaşa:[0x1a2e6c,0xeeeeee],Başakşehir:[0xf57c00,0x1a2e6c],Antalyaspor:[0xd32f2f,0xeeeeee],Rizespor:[0x2e7d32,0x1565c0],Konyaspor:[0x2e7d32,0xeeeeee],Samsunspor:[0xd32f2f,0xeeeeee],"Real Madrid":[0xf5f5f5,0x1a2e6c],"Bayern München":[0xdc052d,0xffffff],"Manchester City":[0x6cabdd,0xffffff],Barcelona:[0xa50044,0x004d98],"Manchester United":[0xda291c,0xffffff],Liverpool:[0xc8102e,0xffffff],Arsenal:[0xef0107,0xffffff],Chelsea:[0x034694,0xffffff],"Atlético Madrid":[0xcb3524,0xffffff],Sevilla:[0xffffff,0xd32f2f],Valencia:[0xffffff,0x111111]};
const NM_=new T.MeshBasicMaterial({color:0xffffff,transparent:true,opacity:.25,side:T.DoubleSide}),STD=[],pg1=new T.CylinderGeometry(.22,.18,1.1,6),pg2=new T.SphereGeometry(.14,6,5),bgm=new T.SphereGeometry(.3,8,6);
const FXx=[-.9,-.6,-.6,-.6,-.6,-.3,-.3,-.3,-.3,-.05,-.05],FXz=[0,-.8,-.28,.28,.8,-.85,-.3,.3,.85,-.3,.3];
function mkStd(o){const{cx,cz,R}=o,k=KIT[o.home],gh=7/R,L=2*PI-2*gh;
flat(R*3.2,R*3.2,0x3a3d40,cx,.03,cz);
for(let i=0;i<4;i++){const m=new T.Mesh(new T.CylinderGeometry(R-2,R,R*.48,16,1,true,gh+i*L/4,L/4),new T.MeshLambertMaterial({color:i%2?k[1]:k[0],side:T.DoubleSide}));m.position.set(cx,R*.24,cz);scene.add(m)}
const rg=new T.Mesh(new T.TorusGeometry(R-1,1.4,6,32),MT(k[1]));rg.rotation.x=PI/2;rg.position.set(cx,R*.49,cz);scene.add(rg);
o.pw=R*1.2;o.pH=R*.7;flat(o.pw,o.pH,0x2e8b3a,cx,.08,cz);
const o3=new T.Object3D(),r0=R*.74,sw=(R-3-r0)/4,pf=[new T.Vector2(r0,0)];
for(let j=0;j<4;j++)pf.push(new T.Vector2(r0+j*sw,(j+1)*1.3),new T.Vector2(r0+(j+1)*sw,(j+1)*1.3));pf.push(new T.Vector2(R-2.5,0));
const bw=new T.Mesh(new T.LatheGeometry(pf,40),new T.MeshStandardMaterial({color:0x8a8d92,side:T.DoubleSide,roughness:.9}));bw.position.set(cx,0,cz);scene.add(bw);
const sp=[];for(let j=0;j<4;j++){const rad=r0+j*sw+sw*.5,n=Math.floor(2*PI*rad/.85);for(let i=0;i<n;i++){const a=i/n*2*PI;if(Math.cos(a)>0&&Math.abs(Math.sin(a))*rad<6.5)continue;sp.push([cx+Math.sin(a)*rad,(j+1)*1.3,cz+Math.cos(a)*rad,a])}}
const LM=()=>new T.MeshLambertMaterial({color:0xffffff}),mkI=(g,m)=>{const q=new T.InstancedMesh(g,m,sp.length);q.frustumCulled=false;return q};
const sI=mkI(BG(.55,.45,.5),LM()),fI=mkI(BG(.45,.7,.35),LM()),hI=mkI(pg2,LM()),cc2=new T.Color(),sk2=[0xf0c8a0,0xe0b090,0xc68642,0x8d5524],av=[k[0],k[1],k[0],k[1],0xeeeeee];
sp.forEach((q,i)=>{o3.rotation.set(0,q[3],0);o3.position.set(q[0],q[1]+.22,q[2]);o3.updateMatrix();sI.setMatrixAt(i,o3.matrix);sI.setColorAt(i,cc2.setHex(pick([k[1],k[1],k[0],0x777777])));
o3.position.y=q[1]+.7;o3.updateMatrix();fI.setMatrixAt(i,o3.matrix);fI.setColorAt(i,cc2.setHex(pick(av)));o3.position.y=q[1]+1.2;o3.updateMatrix();hI.setMatrixAt(i,o3.matrix);hI.setColorAt(i,cc2.setHex(pick(sk2)))});
o.F=new T.Group();o.F.add(fI,hI);o.F.visible=false;scene.add(sI,o.F);
[[o.pw,.3,0,-o.pH/2],[o.pw,.3,0,o.pH/2],[.3,o.pH,-o.pw/2,0],[.3,o.pH,o.pw/2,0],[.3,o.pH,0,0]].forEach(q=>flat(q[0],q[1],0xffffff,cx+q[2],.09,cz+q[3]));
[-1,1].forEach(sg=>{const gx=cx+sg*o.pw/2;[-3.66,3.66].forEach(zz=>{cyl(.08,.08,2.4,0xffffff,gx,1.2,cz+zz,6);const sn=new T.Mesh(new T.PlaneGeometry(1.8,2.4),NM_);sn.position.set(gx+sg*.9,1.2,cz+zz);scene.add(sn)});box(.1,.1,7.4,0xffffff,gx,2.4,cz);const bn=new T.Mesh(new T.PlaneGeometry(7.4,2.4),NM_);bn.rotation.y=PI/2;bn.position.set(gx+sg*1.8,1.2,cz);scene.add(bn)});
cols.push({x:cx,z:cz,ring:1,a:R-3,b:R+.5});
o.gate={open:false};cols.push({x:cx,z:cz+R-1.5,hw:5.5,hd:.8,gate:o.gate});const br=new T.Group();br.position.set(cx,0,cz+R-1.5);scene.add(br);[-3.4,0,3.4].forEach(x=>{box(.4,1.2,.4,0x555555,x,.6,0,br);box(1.6,.12,.12,0xcccc33,x,1,0,br);box(.12,.12,1.6,0xcccc33,x,1,0,br)});box(11,.9,.15,0x555566,0,.5,.5,br);o.bar=br;
box(1.6,9,2.4,0x777777,cx-7.5,4.5,cz+R-1);box(1.6,9,2.4,0x777777,cx+7.5,4.5,cz+R-1);box(16.6,1.6,2.6,k[0],cx,9.2,cz+R-1);
const gs=lab('TARAFTAR GİRİŞİ',15,2.6,44);gs.position.set(cx,11.6,cz+R-1);scene.add(gs);flagPole(cx-10,0,cz+R+2,10);flagPole(cx+10,0,cz+R+2,10);
for(let i=0;i<4;i++){const a=PI/4+i*PI/2,x=cx+Math.cos(a)*(R+8),z=cz+Math.sin(a)*(R+8);cyl(.5,.6,30,0x999999,x,15,z,8);lm(6,1.5,1,x,30.5,z);cols.push({x,z,r:.8})}
o.mA=new T.MeshLambertMaterial({color:0xffffff});o.mB=new T.MeshLambertMaterial({color:0xffffff});
o.G=new T.Group();o.G.visible=false;scene.add(o.G);o.P=[];
for(let i=0;i<22;i++){const g=new T.Group(),b=new T.Mesh(pg1,i==0?MT(0x27ae60):i==11?MT(0xe67e22):i<11?o.mA:o.mB),h=new T.Mesh(pg2,MT(0xe0b090));b.position.y=.8;h.position.y=1.65;g.add(b,h);o.G.add(g);o.P.push(g)}
o.ball=new T.Mesh(bgm,MT(0xffffff));o.G.add(o.ball);o.bs={x:cx,z:cz,t:0,tm:0};o.k=-1;o.last=0;STD.push(o)}
mkStd({id:0,name:'Rams Park',home:'Galatasaray',cx:-150,cz:-130,R:46,off:0,fx:[['Kasımpaşa',1000],['Trabzonspor',6500],['Real Madrid',45000,1],['Fenerbahçe',20000],['Beşiktaş',14000],['Bayern München',38000,1],['Başakşehir',2500],['Antalyaspor',1200]]});
mkStd({id:1,name:'Şükrü Saracoğlu',home:'Fenerbahçe',cx:-245,cz:95,R:36,off:120,fx:[['Rizespor',900],['Galatasaray',20000],['Beşiktaş',13000],['Barcelona',42000,1],['Trabzonspor',6000],['Konyaspor',1500],['Manchester City',40000,1],['Samsunspor',2200]]});
function fixture(st,k){const f=st.fx[k%st.fx.length],price=Math.round(f[1]*(.9+.25*rr(k*3.7+st.id*11))/50)*50,gl=[],hg=Math.floor(rr(k+st.id)*4),ag=Math.floor(rr(k*2+st.id*5)*3);
for(let i=0;i<hg;i++)gl.push({m:5+Math.floor(rr(k*7+i*3+st.id)*84),s:0});for(let i=0;i<ag;i++)gl.push({m:5+Math.floor(rr(k*9+i*5+st.id*2)*84),s:1});return{home:st.home,away:f[0],price,gl,ucl:!!f[2]}}
function mst(st){const u=gt+st.off,k=Math.floor(u/240),tt=u%240,f=fixture(st,k),ph=tt<110?0:tt<210?1:2,min=ph==1?Math.min(90,Math.floor((tt-110)*.9)):ph?90:0,sc=[0,0];f.gl.forEach(g=>{if(g.m<=min)sc[g.s]++});return{k,tt,f,ph,min,sc}}
const ab=n=>n.slice(0,3).toUpperCase();
// ---------- instanced: kapılar, lambalar, ağaçlar ----------
const O3=new T.Object3D(),DC=new T.Color(0x6b3e1f),OC=new T.Color(0x2ecc71);
const dm=new T.InstancedMesh(new T.BoxGeometry(1.4,2.2,.3),new T.MeshLambertMaterial({color:0xffffff}),Math.max(1,H.length));dm.frustumCulled=false;
H.forEach((h,i)=>{O3.position.set(h.x,1.1,h.z+h.d/2+.1);O3.updateMatrix();dm.setMatrixAt(i,O3.matrix);dm.setColorAt(i,DC)});scene.add(dm);
const setDoor=h=>{dm.setColorAt(h.id,h.own?OC:DC);dm.instanceColor.needsUpdate=true};
const lp=[];HR.forEach(z=>{for(let x=-280;x<=280;x+=40)lp.push([x,z+(x%80?7.5:-7.5)])});VR.forEach(x=>{for(let z=-280;z<=220;z+=40)lp.push([x+(z%80?7.5:-7.5),z])});
const lpo=new T.InstancedMesh(new T.CylinderGeometry(.1,.12,6,5),MT(0x444444),lp.length),lph=new T.InstancedMesh(BG(.7,.3,.7),lampMat,lp.length);lpo.frustumCulled=lph.frustumCulled=false;
lp.forEach((p,i)=>{O3.position.set(p[0],3,p[1]);O3.updateMatrix();lpo.setMatrixAt(i,O3.matrix);O3.position.set(p[0],6.1,p[1]);O3.updateMatrix();lph.setMatrixAt(i,O3.matrix)});scene.add(lpo,lph);
const tr1=new T.InstancedMesh(new T.CylinderGeometry(.2,.25,2,6),MT(0x6b4a2a),trees.length),tr2=new T.InstancedMesh(new T.SphereGeometry(1.8,8,6),MT(0x2f7d32),trees.length);tr1.frustumCulled=tr2.frustumCulled=false;
trees.forEach((p,i)=>{O3.position.set(p[0],1,p[1]);O3.updateMatrix();tr1.setMatrixAt(i,O3.matrix);O3.position.set(p[0],3.4,p[1]);O3.updateMatrix();tr2.setMatrixAt(i,O3.matrix)});scene.add(tr1,tr2);
// ---------- insanlar ----------
const gT=new T.CylinderGeometry(.17,.14,.62,8),gH=new T.SphereGeometry(.115,10,8),gL=new T.CylinderGeometry(.08,.06,.85,6),gA=new T.CylinderGeometry(.05,.045,.62,6),gS=new T.BoxGeometry(.13,.07,.28),gSk=new T.CylinderGeometry(.13,.27,.45,8),gHr=new T.SphereGeometry(.125,10,6,0,PI*2,0,PI/2),gHl=new T.CylinderGeometry(.12,.08,.5,6);
function person(s,p,k,h,f){const g=new T.Group(),m=(ge,c,x,y,z,par,sc)=>{const q=new T.Mesh(ge,MT(c));q.position.set(x,y,z);(par||g).add(q);if(sc)q.scale.set(sc[0],sc[1],sc[2]);return q};
m(gT,s,0,1.2,0,0,[1.35,1,.85]);m(gH,k,0,1.64,0,0,[1,1.18,1]);m(gHr,h,0,1.665,-.01);[-.04,.04].forEach(x=>m(BG(.025,.02,.02),0x111111,x,1.655,.108));
if(f){m(gHl,h,0,1.5,-.09);if(Math.random()<.5)m(gSk,s,0,.85,0)}
const lg=x=>{const q=new T.Group();q.position.set(x,.9,0);m(gL,p,0,-.43,0,q);m(gS,0x222222,0,-.86,.06,q);g.add(q);return q},ar=x=>{const q=new T.Group();q.position.set(x,1.46,0);m(gA,Math.random()<.5?s:k,0,-.3,0,q);g.add(q);return q};
g.userData={L:lg(-.1),R:lg(.1),AL:ar(-.26),AR:ar(.26)};g.scale.setScalar(rnd(.96,1.04));return g}
function anim(g,ph,a){const u=g.userData,s=Math.sin(ph)*a;u.L.rotation.x=s;u.R.rotation.x=-s;u.AL.rotation.x=-s*.8;u.AR.rotation.x=s*.8}
function face(g,dx,dz,dt){const t=Math.atan2(dx,dz),df=((t-g.rotation.y+PI)%(2*PI)+2*PI)%(2*PI)-PI;g.rotation.y+=df*Math.min(1,dt*8)}
const me=person(0x2b4a8a,0x23232f,0xe0b090,0x2a1a10,false);me.scale.setScalar(1);me.position.set(6,0,-8);me.rotation.y=PI;scene.add(me);const mp=me.position;
const SH=[0xc0392b,0x2980b9,0x27ae60,0x8e44ad,0xf39c12,0x16a085,0xd35400,0x7f8c8d,0xecf0f1,0x34495e],PN=[0x222222,0x34495e,0x5d4037,0x455a64,0x1f3a5f],SK=[0xf0c8a0,0xe0b090,0xc68642,0x8d5524],HC=[0x222222,0x4a3020,0x777777,0x111111,0x8a5a2b];
// ---------- konuşma ----------
const FBK=["Hmm, ilginç. Biraz daha anlat.","Valla düşünmem lazım.","Haklısın bence.","Bunu hiç böyle düşünmemiştim.","İstanbul'da her şey olur."];
const RAW=[
["Hasan Usta","Simitçi",-110,75,"Sıcak simit! Buyur, taze çıktı.",[["simit|fiyat","Simit sıcakken yenir, çayı da iskelede iç."],["kadıköy|mahalle","Kadıköy her şeyin merkezi: çarşı, Moda, vapur, stat..."],["fener|maç","Fener maçı günü simit satışım üç katına çıkar!"]]],
["Nazlı","Üniversite öğrencisi",-70,130,"Selam! Moda'ya kahve içmeye mi gidiyorsun?",[["kahve|moda|kafe","Moda sahilinde deniz izleyerek kahve içmek en iyisi."],["ders|okul|sınav","Final haftası yaklaşıyor, hiç çalışmadım, sorma!"],["bar|konser","Kadıköy'de her akşam bir yerde canlı müzik var."]]],
["Kaptan Cemal","Vapur kaptanı",-70,215,"Hoş geldin evlat. Denizin havasını içine çek.",[["vapur|boğaz|sefer","Yıllardır Kadıköy-Eminönü hattını sürerim. Martılar beni tanır."],["boğa|heykel","Boğa heykeli buluşma noktamızdır."],["martı","Simit atarsan peşini bırakmazlar!"]]],
["Deniz","Sokak müzisyeni",-150,85,"Selam! Bir parça çalayım mı?",[["çal|gitar|bağlama","Gitar, bağlama, ne istersen. Sokak müziği bir yaşam biçimi."],["para|kazanç","Şapka dolarsa akşam yemeği bizden, dolmazsa makarna."],["rock|rap|caz","Kadıköy rock'ın başkentidir, hâlâ öyle."]]],
["İsmail Amca","Balıkçı",150,206,"Hoş geldin! Oltan var mı yanında?",[["balık|olta|tut|istavrit|palamut","Bu mevsimde istavrit çıkar. Sabırlı olmak yeter."],["maltepe","Maltepe sahili yıllardır burada. Yürüyüş yolu, çay bahçesi, ne ararsan."],["gece","Gece balık daha çok vurur, ışıklara bak."]]],
["Fatma Teyze","Mahalle sakini",130,95,"Gel evladım, çay demledim, otur biraz.",[["börek|mantı|yemek","Evde mantı açtım, akşam gel yiyelim."],["mahalle|komşu","Maltepe eskiden daha sakindi ama komşularım altın gibi."],["cami","Şu camide cuma günleri bayram havası olur."]]],
["Mehmet Amca","Emekli",190,60,"Buyur gençlik, bank boş, oturabilirsin.",[["eski|geçmiş|yıl|çocuk","Ben çocukken burada sadece bostan vardı. Şimdi apartman ormanı."],["park|bank","Bankların hepsi benim ofisim sayılır."],["gazete|haber","Hep aynı haber... trafik, zam, hava."]]],
["Burak","Galatasaray taraftarı",-130,-95,"Aslan gibi geldin! Rams Park'a hoş geldin.",[["galatasaray|gs|takım|aslan","Cim Bom! Maç günü tribünde ses duvarı oluşur."],["kombine|tribün","Biletler hep tükeniyor, telefondan erken al."],["derbi|fener","Derbi demek kalp krizi demek, ama değer!"]]],
["Selim","Güvenlik görevlisi",-190,-115,"Durun bakalım... yok yok, buyurun, kolay gelsin.",[["giriş|içeri|kapı","Bileti olan girer, bileti olmayan dışarıdan bakar."],["vardiya|iş","Vardiya uzun ama atmosfer harika."],["park","Otopark maç günü tıka basa dolar."]]],
["Yusuf","Çaycı",-150,-75,"Taze çay var, demli! Buyur abi.",[["çay|demli|şeker","İki şeker mi atayım, açık mı? Çayın abisi benim."],["taraftar","Maç günü yüz bardak satıyorum, galibiyette iki yüz!"],["kahve","Kahve de var ama çay benim işim."]]],
["Elif","Proses mühendisi",128,-98,"Merhaba! Tesise girmek yasak ama sohbete açığım.",[["rafineri|petrol|tüpraş|üretim","Burada ham petrolü benzin, motorin ve daha fazlasına çeviriyoruz."],["alev|flare","O alev bir güvenlik sistemi; fazla gazı güvenle yakar."],["kask|güvenlik","Burada kask ve güvenlik her şeyden önce gelir."]]],
["Rıza Usta","Vardiya şefi",180,-95,"Vardiya değişimi yaklaşıyor, çabuk ol evlat.",[["vardiya|çalış","Üç vardiya dönüyoruz, gece bile tesis uyumaz."],["tank|depo","Şu beyaz tanklar dev gibi, her biri koca bir havuz."],["çorba|kantin","Kantinin çorbası meşhurdur."]]],
["Kemal","Taksici",22,-10,"Abi nereye? Taksi var, yürümek yorar!",[["taksi|ücret|lira","Taksimetre açık ama bu şehir yürüyerek keşfedilir derler."],["istanbul|şehir","Otuz yıldır bu şehirde taksi sürerim, her sokağı bilirim."],["meslek|iş","Taksici olmak istiyorsan telefondan Meslek'e gir, araba da lazım."]]],
["Rehber Zeynep","Turist rehberi",-120,182,"Merhaba! Kadıköy turuna katılmak ister misin?",[["tur|gez|görül","Boğa heykeli, iskele, çarşı, Moda... Kadıköy turu bir günlük."],["tarih|haydarpaşa","Haydarpaşa yakın; bu şehrin tarihi çok eski."],["yemek|balık","Çarşıda ve sahilde yiyecek harika yerler var."]]]];
RAW.push(["Kaptan Barış","Yat kaptanı",226,222,"Pendik Marina'ya hoş geldin! Yatım hazır.",[["yat|tekne|marina","Pendik Marina'da yüzlerce tekne bağlanır."],["deniz|rüzgar","Rüzgâr bugün poyraz, denize açılmak için iyi."],["havalimanı|sabiha","Sabiha Gökçen hemen yakınımızda, uçaklar tepemizden geçer."]]],["Pilot Aslı","Pilot",246,95,"Merhaba! Uçuşa hazırlanıyorum.",[["uçak|uçuş|pilot","Havalimanı Pendik'te, her gün onlarca uçuş yapıyoruz."],["bilet","Uçak bileti telefondan alınmıyor ama şehir turu atabilirsin!"],["gök|bulut","Bulutların üstü hep güneşli."]]]);
const RP={Öğrenci:["Selam! Dersten kaçıp gezmeye çıktım.",[["ders|okul|sınav","Final haftası, kafayı yiyeceğim!"],["kafe|kahve","Moda'da güzel bir yer biliyorum."]]],Esnaf:["Hayırlı işler! Bir şey lazım mı?",[["dükkan|satış|müşteri","Bugün müşteri az, akşama doğru açılır."],["kira","Dükkân kirası belimizi büktü."]]],Memur:["İyi günler. Mesaiden çıktım yeni.",[["mesai|iş","Mesai uzun, maaş kısa, ama idare."],["otobüs|metro","Metrobüste sıkışmadan eve gitmek hayal."]]],Emekli:["Selamün aleyküm evladım.",[["eski|zaman","Bizim zamanımızda İstanbul daha sakindi."],["torun|çocuk","Torunlar gelince ev şenleniyor."]]],Turist:["Hello! Eh, merhaba! İstanbul çok güzel.",[["gez|görül|tarih","Ayasofya, Boğaz, Kadıköy... çok yer var!"],["yemek|baklava","Baklava ve Türk kahvesi favorim!"]]],Sporcu:["Selam! Antrenmandan dönüyorum.",[["antrenman|spor|koşu","Sabah koşusu şehri tanımanın en iyi yolu."],["maç|lig","Lig bu sene çok çekişmeli."]]],Garson:["Hoş geldiniz! Masa mı bakıyordunuz?",[["menü|yemek|masa","Günün balığı çok taze, tavsiye ederim."],["bahşiş|para","Bahşiş garsonun ekmeği."]]],Müzisyen:["Selam! Bu akşam sahnedeyim.",[["müzik|konser|sahne","Akşam Kadıköy'de çalıyoruz, gel."],["gitar|davul","Davulcumuz hep geç gelir."]]]};
const GEN=[
["nasılsın|naber|ne haber|nasıl gidiyor",["İyiyim, sağ ol! Sen nasılsın?","Şükür, idare ediyoruz. Sende ne var ne yok?","Bugün işler yolunda, sen anlat."]],
["adın|ismin|kimsin",d=>"Ben "+d.n+", "+d.r.toLowerCase()+"."],
["ne iş|mesleğin|ne yapıyorsun|işin",d=>"Mesleğim: "+d.r.toLowerCase()+". Sen de telefondan Meslek uygulamasına girip iş bulabilirsin."],
["yaşın|kaç yaş",["Yaş sorulmaz ama gönlüm hep genç!","Otuzlarda sayılırım."]],
["nerelisin|memleket",["İstanbulluyum, doğma büyüme.","Anadolu'dan geldim, bu şehre âşık oldum."]],
["saat|vakit",()=>"Saat şu an "+clk()+"."],
["gece|akşam|karanlık",["Gece İstanbul başka güzel ama dikkatli ol.","Geceleri sokak lambaları yanıyor, şehir ışıl ışıl."]],
["gündüz|sabah|güneş",["Gündüz şehir daha canlı, esnaf kapıyı açar.","Sabahın güneşi Boğaz'a vurunca bambaşka olur."]],
["hava|yağmur",["Bugün İstanbul'a göre fena değil. Yine de şemsiyeni unutma.","Hava güzel, yürümelik."]],
["trafik|araba|araç|taksi|sürmek",["Trafikte sabır lazım. Sürmek istersen bir arabaya yaklaş ve bin.","Telefondan taksi çağırabilirsin: Ara → Taksi."]],
["ev|kira|daire|ilan|satılık",()=>{const h=pick(H);return "Telefondaki İlanlar'a bak. Mesela "+h.name+" "+fmt(h.price)+"."}],
["bilet|maç|stad|derbi",()=>stInfo()],
["galatasaray|gs|cim bom|aslan",["Cim Bom Bom! Rams Park'ta atmosfer görülmeye değer.","Sarı-kırmızı kalpte yaşar."]],
["fener|fenerbahçe|fb|kanarya",["Kadıköy'de Şükrü Saracoğlu'nun atmosferi bir başka!","Sarı-lacivert tutku!"]],
["beşiktaş|bjk|kartal",["Kartal yuvası derler, sağlam taraftarı var.","Çarşı'yı bilirsin, çok şey söyler."]],
["para|zengin|milyon|fiyat|kaç lira",["Para gelir geçer ama ev almak iyi yatırım.","Cebin doluysa ilanlara göz at."]],
["meslek|iş bul|çalış|iş ara",["Telefondan Meslek uygulamasına gir: kurye, taksici, güvenlik...","İş çok, çalışan az. Telefona bak."]],
["telefon|instagram|gram|sosyal|takipçi",["Gram'da paylaşım yap, takipçi kazan; sponsor bile gelir.","Ben pek telefona bakmam ama herkes Gram'da."]],
["yemek|aç|simit|kebap|balık|çay|kahve",["Simit, çay, balık-ekmek... İstanbul'da yemek sorun olmaz.","Kadıköy'de her sokak başı bir lezzet."]],
["müzik|şarkı|konser",["Kadıköy'de her akşam bir yerde konser var.","Müzik olmasa şehir sessiz kalır."]],
["deniz|vapur|sahil|boğaz",["Vapurla deniz izlemek en güzel terapi.","Maltepe sahilinde yürümek ruha iyi gelir."]],
["şaka|fıkra|güldür",["İstanbul'da trafik yok diyen turist ilk günündeymiş.","Bir adam simitçiye sormuş: 'Susam çok mu?' Simitçi: 'Sen yeter ki iste.'"]],
["tavsiye|ne yapayım|nereye gideyim|gezilecek|öner",["Gündüzse Kadıköy'de yürü, akşam Maltepe sahiline git, maç varsa stada uğra.","Önce bir arabaya bin, şehri gez."]],
["aşk|sevgili|evlen",["Gönül işi karışık ama İstanbul'da her köşe romantik.","Önce ev, sonra evlilik derler!"]],
["polis|tehlike|güvenli",["Şehir güvenli sayılır, yine de geceleri dikkat.","Trafikte yayalara dikkat edersen sorun yok."]],
["üzgün|kötü|moralim|yorgun",["Üzülme, İstanbul'da her gün yeni bir şans.","Bir çay iç, geçer."]],
["tamam|olur|peki|iyi",["Güzel.","Tamamdır."]],
["teşekkür|sağ ol|sağol|eyvallah|saol",["Rica ederim, ne demek!","Estağfurullah."]],
["görüşürüz|hoşça|bay bay|bye",["Görüşürüz, kendine iyi bak!","Yolun açık olsun!"]],
["yardım|ne yapabilirim",["Sokakları gez, insanlarla konuş, telefona bak; ev al, maça git, iş bul.","Bana mahalleleri sorabilirsin."]],
["metropol",["Metropol mü? İstanbul'a bundan başka ne yakışır ki."]]].map(a=>[new RegExp(a[0]),a[1]]);
const clk=()=>{const t=gt%300;let h=(t<180?6+12*t/180:18+12*(t-180)/120)%24;return String(Math.floor(h)).padStart(2,'0')+':'+String(Math.floor(h%1*60)).padStart(2,'0')};
const stInfo=()=>{let o=STD[0],bd=1e9;STD.forEach(x=>{const d=Math.hypot(x.cx-mp.x,x.cz-mp.z);if(d<bd){bd=d;o=x}});const s=mst(o),f=fixture(o,s.ph==2?s.k+1:s.k);return o.name+"'ta "+f.home+"–"+f.away+" maçı var"+(f.ucl?" (Şampiyonlar Ligi!)":"")+", bilet "+fmt(f.price)+". Telefondan Biletler'e bak."};
function reply(n,s){s=s.toLocaleLowerCase('tr');const d=n.d;
if(/nerede|nasıl gid|yol|tarif|git|uzak|nereye/.test(s))for(const x of D)if(new RegExp(x.k).test(s)){const dx=x.x-mp.x,dz=x.z-mp.z,dd=Math.round(Math.hypot(dx,dz)),dr=(dz<-20?'kuzey':dz>20?'güney':'')+(dx>20?'doğu':dx<-20?'batı':'');return dd<45?x.n+" burası zaten, hemen yanındasın!":x.n+" mı? "+(dr||'çok yakın')+" yönünde, yaklaşık "+dd+" metre var."}
if(/selam|merhaba|mrb|hey|günaydın|iyi akşamlar|iyi günler/.test(s))return d.hi;
for(const[k,a]of d.t)if(new RegExp(k).test(s))return typeof a=='function'?a(d):a;
if(d.fl){for(const q of GENL[d.fl])if(q[0].test(s))return typeof q[1]=='function'?q[1](d):Array.isArray(q[1])?pick(q[1]):q[1];return pick(d.f)}for(const[re,a]of GEN)if(re.test(s))return typeof a=='function'?a(d):Array.isArray(a)?pick(a):a;
return pick(d.f||FBK)}
// ---------- NPC'ler ----------
const N=[],FEM=/Nazlı|Fatma|Elif|Zeynep|Ayşe|Selin|Ece|Burcu|Merve|Gül|Emily|Megan|Ashley|Jessica|Charlotte|Amelia|Poppy|Lucía|Carmen|Marta|Elena/;
function mkNpc(d,x,z,sh_,pol,hat,pn){const g=person(sh_||pick(SH),pol?0x1a2a40:pn?pick(pn):pick(PN),pick(SK),pol?0x111111:pick(HC),FEM.test(d.n));if(pol||hat){const cp=new T.Mesh(new T.CylinderGeometry(.14,.15,.1,10),MT(pol?0x1f3a5f:hat));cp.position.y=1.8;g.add(cp)}g.position.set(x,0,z);const lb=lab(d.n,3.4,.85,54);lb.position.y=2.3;g.add(lb);scene.add(g);const e={d,g,lb,id:N.length,hx:x,hz:z,tx:x,tz:z,w:rnd(0,3),ph:0,t:0,pol:!!pol};N.push(e);return e}
RAW.forEach(a=>mkNpc({n:a[0],r:a[1],hi:a[4],t:a[5],f:FBK},a[2],a[3]));
const NMS=["Ahmet","Ayşe","Mehmet","Zeynep","Can","Elif","Emre","Selin","Kerem","Ece","Oğuz","Burcu","Murat","Merve","Tolga","Gül","Okan"],RL=Object.keys(RP);
const DR={
Kadıköy:{r:[["Sanatçı","Selam! Şu duvardaki grafitiyi ben yaptım.",[["sanat|resim|grafiti","Kadıköy'de her duvar bir tuval."],["moda|kafe","Moda'da gün batımını izlemek şart."]]],["Barista","Hoş geldin! Filtre kahve mi, türk kahvesi mi?",[["kahve","Tek kökenli çekirdek çalışıyoruz, denemelisin."],["müzik","Dükkânda hep caz çalar."]]],["Kitapçı","Hoş geldiniz, kitaplara bakmak ister misiniz?",[["kitap|roman","Bu sene en çok edebiyat satıyor."],["sahaf","Sahaflarda hazine var."]]],["Vapur yolcusu","Vapuru kaçırmamak lazım!",[["vapur|iskele","Kadıköy vapuru en güzel yolculuk."],["kedi","Kadıköy'ün kedileri şehrin sahibi."]]]],f:["Kadıköy'de her şey olur, kafana takma.","Şu an Moda'ya yürüyesim var.","Bunu konuşmak için kahve içelim."],sh:[0x222222,0x8e44ad,0xc0392b,0x16a085,0x2c3e50],hat:[0x222222],hp:.2},
Seyrantepe:{r:[["Galatasaraylı","Cim Bom! Maç var mı bugün?",[["maç|skor|şampiyon","Bu sene şampiyonluk bizim, yazıp koy!"],["fener|derbi","Derbi günü uyku yok."]]],["Fenerbahçeli","Buralar kırmızı-sarı ama kalbim sarı-lacivert!",[["fener|kadıköy","Kadıköy'de Şükrü Saracoğlu'nda görüşürüz."],["gala|cim bom","Rakibe saygı, ama kupa bizde."]]],["Bilet satıcısı","Bilet var abi, bilet! Telefondan da alabilirsin.",[["bilet|fiyat","Derbi biletleri uçuyor, erken al."],["tribün|koltuk","Kuzey tribünü en gürültülüsü."]]],["Gazeteci","Maç öncesi röportaj yapıyorum, birkaç sorum var.",[["röportaj|haber","Spor dünyasında her gün yeni haber var."],["transfer","Bu transferi bilen yok ama duyumlar var."]]]],f:["Maç konuşalım abi!","Bu sene lig çok çekişmeli."],sh:[0xa00000,0xffb000,0xa00000,0xffffff,0x0a1f5c]},
Tüpraş:{r:[["Rafineri işçisi","Vardiya bitti, yorgunum ama bu iş ekmek kapısı.",[["vardiya|rafineri","Gece vardiyası zordur, alev hiç sönmez."],["kask|güvenlik","Kasksız kimse içeri girmez."]]],["Mühendis","Basınç değerleri normal, günaydın.",[["proses|ürün|petrol","Ham petrolü benzine, mazota, jet yakıtına çeviriyoruz."],["tank|boru","Boru hatları kilometrelerce uzar."]]],["Tır şoförü","Tanker dolacak, yola çıkacağım.",[["tanker|yol|yakıt","İstanbul'un benzinini biz taşırız."],["trafik","Köprüde sıra bekliyorum."]]],["Güvenlik","Burası kısıtlı alan, ama sohbet serbest.",[["giriş|kimlik","Kimliksiz içeri girilmez."],["gece","Gece alev şehri aydınlatır."]]]],f:["Tesis çok büyük, kaybolma.","Burada güvenlik her şeydir."],sh:[0xff7a00,0xff7a00,0xf1c40f,0xecf0f1],hat:[0xffd400,0xffffff],hp:.85,pn:[0x2c3e50]},
Maltepe:{r:[["Komşu","Hey komşu! Hoş geldin!",[["çim|bahçe","Çimleri her hafta biçerim, mahalle birinciyim."],["mangal|barbekü","Hafta sonu bahçede mangal yapıyoruz, gel."]]],["Postacı","Postam var mı bakalım! Günaydın!",[["posta|kargo","Kapı kapı dolaşırım, herkesi tanırım."],["köpek","Mahallenin köpekleri beni sever."]]],["Aile babası","Çocukları okula bırakacağım, geç kaldık!",[["çocuk|okul","Maltepe Koleji çok iyi, çocuklar mutlu."],["ev|müstakil","Müstakil ev, bahçe, huzur... başka ne ister insan?"]]],["Bahçıvan","Gül fidanları bu mevsim çok güzel açıyor.",[["gül|çiçek|bahçe","Her bahçeye ayrı bir fidan lazım."],["mahalle","Burası küçük bir kasaba gibi, herkes birbirini tanır."]]]],f:["Mahallemiz çok sakin, çimler tertemiz.","Barbekü yapacağız, gelsene!"],sh:[0xecf0f1,0xf1c40f,0x5dade2,0xe8b4b8,0xa8d8a0],hat:[0xffffff,0x3498db],hp:.3},
Pendik:{r:[["Denizci","Rüzgâr dönüyor, tekneyi bağlayayım.",[["tekne|yat|marina","Pendik Marina'da yüzlerce tekne bağlanır."],["rüzgar|dalga","Poyraz çıkarsa denize açılmak zor."]]],["Havayolu çalışanı","Uçuşa yetişmem lazım, hoşça kal!",[["uçak|uçuş|havalimanı","Sabiha Gökçen'den her gün yüzlerce sefer var."],["bavul|yolcu","Yolcular bavul fazlalığından hep şikayetçi."]]],["Turist","Wow, Pendik çok güzel! Marina süper.",[["marina|deniz","Marina çok güzel, fotoğraf çektim."],["yemek|balık","Balık ekmek yedim, harika!"]]],["Balıkçı","Olta hazır, balık bekliyorum.",[["balık|olta","Bu mevsimde çipura çıkar."],["marina","Marina kıyısında balık tutmak yasak ama kimse bilmez."]]]],f:["Denizin kokusu başka, değil mi?","Uçak sesine alıştık burada."],sh:[0x1a2e6c,0xffffff,0x1a2e6c,0xe74c3c],hat:[0xffffff],hp:.5},
Merkez:{r:[["Banka çalışanı","Mesaim bitti, şimdi kahveye gidiyorum.",[["banka|kredi|faiz","Faiz oranları sürekli değişiyor, hesap açmak lazım."],["atm|para","ATM'ler 7/24 çalışır."]]],["Memur","İyi günler. Mesaiden yeni çıktım.",[["mesai|iş","Mesai uzun, maaş kısa, ama idare."],["otobüs|metro","Metro olmasa işe yetişemem."]]],["Yazılımcı","Kod derleniyor, bir kahve molası...",[["kod|yazılım|bilgisayar","Bugün çok bug çıktı, kafayı yedim."],["startup|girişim","Herkes girişim kuruyor ama zor iş."]]],["İş insanı","Toplantım var, vakit nakittir.",[["toplantı|iş|anlaşma","Bugün büyük bir anlaşma yapacağız."],["borsa|yatırım","Borsa hareketli, dikkatli ol."]]]],f:["Toplantıya yetişmem lazım.","İş dünyası acımasız, bilirsin."],sh:[0x2c3e50,0x34495e,0xffffff,0x7f8c8d],pn:[0x222222,0x1a1a1a]}};
function distAt(x,z){let b='Merkez',bd=1e9;[['Seyrantepe',-150,-130,110],['Tüpraş',150,-130,100],['Kadıköy',-120,100,120],['Maltepe',150,100,95],['Pendik',235,115,70]].forEach(q=>{const d=Math.hypot(x-q[1],z-q[2]);if(d<q[3]&&d<bd){bd=d;b=q[0]}});return b}
for(let i=0;i<64;i++){let x,z;do{if(Math.random()<.5){z=pick(HR)+pick([-9,9]);x=rnd(-270,270)}else{x=pick(VR)+pick([-9,9]);z=rnd(-270,200)}}while(z>6&&z<34);const pk=DR[distAt(x,z)],ro=pick(pk.r);mkNpc({n:pick(NMS),r:ro[0],hi:ro[1],t:ro[2],f:pk.f},x,z,pick(pk.sh),0,pk.hat&&Math.random()<pk.hp?pick(pk.hat):0,pk.pn)}
[[-40,-90],[-30,-90],[8,-25],[-62,66]].forEach(q=>mkNpc({n:'Polis Memuru',r:'Polis',hi:'Buyur vatandaş, bir sorun mu var?',t:[['trafik|hız|ceza|radar','Hız limitine uy, polis her yerde olabilir. Sürat cezası 5.000 ₺!'],['hırsız|suç|soygun','Şehir güvenli, ama dikkatli olmakta fayda var.'],['karakol|şikayet|ihbar','Karakol kuzeybatıda, Merkez mahallesinde.'],['banka|atm','Banka Merkez\'de, ATM\'ler sahilde ve sokak başlarında.']],f:FBK},q[0],q[1],0x1f3a5f,true));
STD.forEach(o=>{const hv=KIT[o.home];for(let i=0;i<6;i++)mkNpc({n:'Taraftar',r:'Taraftar',hi:'Maça geldim, bilet aldın mı?',t:[['maç|bilet|skor','Bileti telefondan al, kapıya gel, içeri geç!'],['takım|kupa','Bu takım benim hayatım!']],f:FBK},o.cx+rnd(-9,9),o.cz+o.R+rnd(5,12),hv[0])});
// okul heykelleri, öğrenciler, plajcılar
const KT=[['okul|ders|öğretmen','Okulu seviyorum ama sınavlardan korkuyorum!'],['atatürk|heykel','Atatürk\'ü çok severiz, her sabah ona saygı duruşu yaparız.'],['oyun|top|arkadaş','Teneffüste top oynuyoruz!']],KNN=["Ali","Ayşe","Emir","Zeynep","Can","Elif","Mert","Defne"];
SCH.forEach(q=>{const x=q[1],z=q[2];box(3,.8,3,0xcccccc,x,.4,z+10);box(1.4,2.4,1.4,0xdddddd,x,2,z+10);const st=person(0x6b5a3a,0x6b5a3a,0x8a7a58,0x4a3a2a,false);st.scale.setScalar(1.3);st.position.set(x,3.2,z+10);scene.add(st);cols.push({x,z:z+10,r:1.7});
const al=lab('ATATÜRK',4,1,56);al.position.set(x,7.2,z+10);scene.add(al);
mkNpc({n:'Öğretmen',r:'Öğretmen',hi:'Günaydın! Okulumuza hoş geldiniz.',t:[['okul|ders|öğrenci','Çocuklarımız geleceğimiz, her gün onlarla gurur duyuyorum.'],['atatürk|heykel','Atatürk bizim yol göstericimizdir, heykeli her öğrenciye ilham verir.']],f:FBK},x+8,z+14,0x8e44ad);
for(let i=0;i<6;i++){const a=rnd(0,PI*2),r=rnd(25,50),hx=cl(x+Math.cos(a)*r,-280,280),hz=z+Math.sin(a)*r,e=mkNpc({n:pick(KNN),r:'Öğrenci',hi:pick(['Merhaba abi/abla!','Selam! Okula gidiyorum.','Hey! Top oynar mısın?']),t:KT,f:['Hihi!','Bilmiyorum ki!','Öğretmenim kızar!']},hx,hz,0x1a2e6c,0,0,[0x555555]);e.g.scale.setScalar(.62);e.sc={x,z:z+17};e.sh={x:hx,z:hz};e.sp=1.8}});
for(let i=0;i<7;i++)mkNpc({n:pick(NMS),r:'Plajcı',hi:pick(['Plaj keyfi başlasın!','Güneş çok güzel, değil mi?','Selam! Denize girdin mi?']),t:[['deniz|yüz|dalga','Deniz bugün tam kıvamında, girmelisin!'],['güneş|bronz|krem','Güneş kremi şart, yoksa kıpkırmızı olursun.'],['dondurma|içecek','Plajda dondurmacı hep dolaşıyor.']],f:['Tatil modundayım, kafamı yorma.','Ah şu deniz kokusu...']},rnd(125,210),rnd(208,228),pick([0xff6b6b,0x48dbfb,0xfeca57,0x1dd1a1]),0,0,[0xff6b6b,0x48dbfb,0xfeca57]);
// ---------- arabalar ----------
const CARS=[],gW=new T.CylinderGeometry(.38,.38,.25,10);
function mkCar(color,pol){const g=new T.Group();box(1.9,.7,4.2,color,0,.65,0,g);box(1.7,.6,2.2,0x223344,0,1.25,-.2,g);
[[-1,1.3],[1,1.3],[-1,-1.3],[1,-1.3]].forEach(([sx,sz])=>{const w=new T.Mesh(gW,MT(0x111111));w.rotation.z=PI/2;w.position.set(sx*.95,.38,sz);g.add(w)});
[-.6,.6].forEach(x=>{const h=new T.Mesh(BG(.4,.2,.1),lampMat);h.position.set(x,.7,2.1);g.add(h);box(.4,.2,.1,0xaa0000,x,.7,-2.1,g)});if(color==0xf1c40f||color==0x111111)box(.8,.25,.4,0xffffff,0,1.7,-.2,g);if(pol){box(1.95,.22,4.25,0x1f4aa8,0,.62,0,g);const a=new T.Mesh(BG(.5,.2,.4),PRM),b=new T.Mesh(BG(.5,.2,.4),PBM);a.position.set(-.35,1.65,-.2);b.position.set(.35,1.65,-.2);g.add(a,b);PLR.push(a);PLB.push(b)}scene.add(g);return g}
function spawnCar(x,z,a,color,ai,pol){const c={g:mkCar(color,pol),x,z,a,v:0,ai,pol};col(c,2.1);CARS.push(c);sh(c.g);return c}
const CC=[0xc0392b,0x2c3e50,0xecf0f1,0x27ae60,0x8e44ad,0x2980b9,0x7f8c8d];
for(let i=0;i<16;i++){const hz=i%2==0,r=pick(hz?HR:VR),dir=pick([1,-1]),c=spawnCar(hz?rnd(-250,250):r,hz?r:rnd(-250,190),0,i>13?0xf2f2f2:i>9?0xf1c40f:pick(CC),{ax:hz?'x':'z',r,dir,sp:rnd(7,11)},i>13);c.x=hz?c.x:r-dir*3;c.z=hz?r+dir*3:c.z;c.taxi=i>9&&i<14}
for(let i=0;i<12;i++){const hz=Math.random()<.5,r=pick(hz?HR:VR),sd=pick([-5,5]);spawnCar(hz?rnd(-250,250):r+sd,hz?r+sd:rnd(-250,190),hz?PI/2:0,pick(CC),null)}
spawnCar(12,-15,PI/2,0x1abc9c,null);
function mkBus(kind){const bc=kind=='dd'?0xcc1a1a:kind=='sb'?0xf6b800:kind=='es'?0xd32f2f:0x1f4aa8,g=new T.Group();box(2.6,2.4,10.5,bc,0,1.7,0,g);if(kind=='dd'){box(2.6,2,10.5,bc,0,3.9,0,g);box(2.66,.7,10.2,0x223344,0,4.1,0,g)}box(2.62,.8,10.52,0xffffff,0,.8,0,g);box(2.66,.9,10.2,0x223344,0,2.4,0,g);box(1.2,.35,.1,0xffd24a,0,3.1,5.3,g);
[[-1.3,3.5],[1.3,3.5],[-1.3,-3.5],[1.3,-3.5]].forEach(q=>{const w=new T.Mesh(gW,MT(0x111111));w.rotation.z=PI/2;w.scale.setScalar(1.2);w.position.set(q[0],.45,q[1]);g.add(w)});
[-.9,.9].forEach(x=>{const h=new T.Mesh(BG(.4,.25,.1),lampMat);h.position.set(x,.9,5.3);g.add(h)});scene.add(g);return g}
BUS.forEach(b=>{const hz=b[0]=='x',c={g:mkBus(),x:hz?rnd(-200,200):b[1]-b[2]*3,z:hz?b[1]+b[2]*3:rnd(-150,150),a:0,v:0,ai:{ax:b[0],r:b[1],dir:b[2],sp:8,stops:b[3]},bus:1};CARS.push(c);sh(c.g)});
[[-150,-60],[-245,155]].forEach(q=>{for(let i=0;i<10;i++)if(Math.random()<.7)spawnCar(q[0]-24.75+i*5.5,q[1]+(Math.random()<.5?-9:9),Math.random()<.5?0:PI,pick(CC),null)});
// ---------- ev içi ----------
const IR=new T.Group();IR.position.set(1000,0,1000);scene.add(IR);
{const r=new T.Mesh(new T.BoxGeometry(10,3.6,10),new T.MeshLambertMaterial({color:0xe8dcc8,side:T.BackSide}));r.position.y=1.8;IR.add(r);
box(10,.2,10,0x8a5a2b,0,.05,0,IR);box(2,.6,3,0x3355aa,-3.5,.4,-3.2,IR);box(3,.7,1.2,0x884444,2.5,.4,-4,IR);box(1.8,1,.1,0x111111,2.5,1.6,-4.8,IR);box(1.8,.1,1.2,0x6a4a2a,0,.8,-1.5,IR);box(1.8,.2,.1,0x553311,0,1.1,4.9,IR)}
const il=new T.PointLight(0xffe0b0,0,18);il.position.set(1000,3,1000);scene.add(il);
// ---------- çarpışma ----------
function col(p,r){for(const c of cols){
if(c.gate&&c.gate.open)continue;
if(c.hw){const cx=Math.max(c.x-c.hw,Math.min(p.x,c.x+c.hw)),cz=Math.max(c.z-c.hd,Math.min(p.z,c.z+c.hd)),dx=p.x-cx,dz=p.z-cz,d=Math.hypot(dx,dz);if(d<r){if(d>1e-4){p.x=cx+dx/d*r;p.z=cz+dz/d*r}else p.x+=r}}
else if(c.ring){const dx=p.x-c.x,dz=p.z-c.z,d=Math.hypot(dx,dz);if(d>c.a-r&&d<c.b+r&&!(dz>0&&Math.abs(dx)<5)&&d>1e-4){const t=d<(c.a+c.b)/2?c.a-r:c.b+r;p.x=c.x+dx/d*t;p.z=c.z+dz/d*t}}
else{const dx=p.x-c.x,dz=p.z-c.z,d=Math.hypot(dx,dz),m=c.r+r;if(d<m&&d>1e-4){p.x=c.x+dx/d*m;p.z=c.z+dz/d*m}}}
const kk=Math.round(p.x/3000);if(kk>0&&kk<4){colC(p,kk);return}p.x=cl(p.x,-290,290);p.z=Math.max(-290,p.z);if(p.z>228){if(PIERS.some(q=>Math.abs(p.x-q)<3.2))p.z=Math.min(p.z,262);else p.z=(p.x>115&&p.x<218)?Math.min(p.z,234):228}if(p.z>8&&p.z<32&&!BRX.some(b=>Math.abs(p.x-b)<8.2))p.z=p.z<20?8:32}
// ---------- gece / gündüz ----------
const SKYU={top:{value:new T.Color(0x3d7fc4)},bot:{value:new T.Color(0xb8d8f0)}},NT_=new T.Color(0x02040c),DT_=new T.Color(0x3d7fc4),OR_=new T.Color(0xff9a5a),SD=new T.Vector3(),
skyD=new T.Mesh(new T.SphereGeometry(480,24,12),new T.ShaderMaterial({uniforms:SKYU,side:T.BackSide,depthWrite:false,fog:false,vertexShader:'varying float h;void main(){h=normalize(position).y;gl_Position=projectionMatrix*modelViewMatrix*vec4(position,1.);}',fragmentShader:'uniform vec3 top;uniform vec3 bot;varying float h;void main(){gl_FragColor=vec4(mix(bot,top,pow(max(h,0.),.6)),1.);}'}));
skyD.renderOrder=-10;scene.add(skyD);
const sunM=new T.Mesh(new T.SphereGeometry(16,12,8),new T.MeshBasicMaterial({color:0xfff2c0,fog:false})),moonM=new T.Mesh(new T.SphereGeometry(12,12,8),new T.MeshBasicMaterial({color:0xe8ecff,fog:false}));
const stp=[];for(let i=0;i<500;i++){const a=rnd(0,PI*2),e=Math.acos(rnd(.05,1));stp.push(Math.sin(e)*Math.cos(a)*450,Math.cos(e)*450,Math.sin(e)*Math.sin(a)*450)}
const sg=new T.BufferGeometry();sg.setAttribute('position',new T.Float32BufferAttribute(stp,3));
const stars=new T.Points(sg,new T.PointsMaterial({color:0xffffff,size:1.6,sizeAttenuation:false,transparent:true,opacity:0,depthWrite:false,fog:false}));skyD.add(sunM,moonM,stars);
const DS=new T.Color(0x9ec9e8),NS=new T.Color(0x0b1026),SC=new T.Color(0xfff2d0),MC=new T.Color(0x6a7aa8),LD=new T.Color(0x3a3a3a),LN=new T.Color(0xffd98a);
function env(){const t=gt%300,f=t<165?1:t<180?1-(t-165)/15:t<288?0:(t-288)/12;
LF=f;const ce=CE[ct]||CE[0],tn=f*(1-f)*4;sky.copy(NS).lerp(DS,f).lerp(OR_,tn*.5).lerp(GRY,ce.g*f);scene.fog.color.copy(sky);SKYU.bot.value.copy(sky);SKYU.top.value.copy(NT_).lerp(DT_,f).lerp(OR_,tn*.12);stars.material.opacity=1-f;FLMS.forEach(m=>m.color.setScalar(.3+.7*f));
const dy=t<180,an=dy?PI*t/180:PI*(t-180)/120;SD.set(Math.cos(an),Math.max(.15,Math.sin(an)),.35).normalize();sunM.visible=dy;moonM.visible=!dy;(dy?sunM:moonM).position.set(Math.cos(an)*420,Math.max(15,Math.sin(an)*320),-200);scene.fog.near=(30+40*f)*ce.fs;scene.fog.far=(190+140*f)*ce.fs;
hemi.intensity=.32+.55*f;sun.intensity=(.22+.6*f)*ce.ss;sun.color.copy(MC).lerp(SC,f);
const e=(1-f)*.95;for(const k in bm)bm[k].emissiveIntensity=e;lampMat.color.copy(LD).lerp(LN,1-f);
const s='METROPOL · '+clk()+(f>.5?' ☀️':' 🌙')+' · Gün '+(S.day+1);if($('sub').textContent!==s)$('sub').textContent=s}
// ---------- girdi ----------
const keys={},joy={x:0,y:0};
addEventListener('keydown',e=>{if(e.key==='Escape'){closeChat();closePhone();return}if(chatOpen||phoneOpen)return;keys[e.code]=1;if(e.code==='KeyE'||e.code==='KeyF')ctxGo()});
addEventListener('keyup',e=>{delete keys[e.code]});
let dr=null;const cv=ren.domElement;
cv.addEventListener('pointerdown',e=>{if(dr)return;dr={id:e.pointerId,x:e.clientX,y:e.clientY};cv.setPointerCapture(e.pointerId)});
cv.addEventListener('pointermove',e=>{if(!dr||e.pointerId!==dr.id)return;yaw-=(e.clientX-dr.x)*.005;pitch=cl(pitch+(e.clientY-dr.y)*.005,.05,1.1);dr.x=e.clientX;dr.y=e.clientY});
const dend=e=>{if(dr&&e.pointerId===dr.id)dr=null};cv.addEventListener('pointerup',dend);cv.addEventListener('pointercancel',dend);
const J=$('joy'),K=$('knob');let jid=null;
function jm(e){const r=J.getBoundingClientRect();let dx=e.clientX-(r.left+r.width/2),dy=e.clientY-(r.top+r.height/2);const m=Math.hypot(dx,dy),mx=r.width*.42;if(m>mx){dx*=mx/m;dy*=mx/m}K.style.transform=`translate(${dx}px,${dy}px)`;joy.x=dx/mx;joy.y=-dy/mx}
J.addEventListener('pointerdown',e=>{jid=e.pointerId;J.setPointerCapture(jid);jm(e)});J.addEventListener('pointermove',e=>{if(e.pointerId===jid)jm(e)});
const jend=e=>{if(e.pointerId===jid){jid=null;joy.x=joy.y=0;K.style.transform=''}};J.addEventListener('pointerup',jend);J.addEventListener('pointercancel',jend);
$('run').onclick=()=>{run=!run;$('run').style.background=run?'#27ae60':'';$('run').textContent=run?'Koş: Açık':'Koş: Kapalı'};
$('fs').onclick=()=>{const e=document.documentElement;try{(e.requestFullscreen||e.webkitRequestFullscreen).call(e)}catch(_){}Promise.resolve().then(()=>screen.orientation.lock('landscape')).catch(()=>{})};
let tt_;function toast(t){const e=$('toast');e.textContent=t;e.style.opacity=1;clearTimeout(tt_);tt_=setTimeout(()=>e.style.opacity=0,2800)}
// ---------- sohbet ----------
const add=(c,t)=>{const p=document.createElement('p');p.className=c;p.textContent=t;$('log').appendChild(p);$('log').scrollTop=1e5};
function openChat(n,call){chatOpen=true;chatNpc=n;for(const k in keys)delete keys[k];$('chat').style.display='flex';chatCall=!!call;renderAct(n);$('log').innerHTML='';add('npc',n.d.n+': '+n.d.hi);if(!matchMedia('(pointer:coarse)').matches)$('msg').focus()}
function closeChat(){chatOpen=false;chatNpc=null;$('chat').style.display='none';$('msg').blur()}
function send(){const v=$('msg').value.trim();if(!v||!chatNpc)return;$('msg').value='';add('me','Sen: '+v);const n=chatNpc;if(n.id!=null&&!n.pol){S.rel[n.id]=(S.rel[n.id]||0)+(/sev|güzel|aşk|kahve|çiçek|canım/.test(v.toLocaleLowerCase('tr'))?3:1);renderAct(n)}setTimeout(()=>{if(chatNpc===n)add('npc',n.d.n+': '+reply(n,v))},450)}
$('send').onclick=send;$('msg').addEventListener('keydown',e=>{if(e.key==='Enter')send()});$('x').onclick=closeChat;
const knock=h=>openChat({d:{n:'Ev sahibi',r:h.name,hi:'Kim o? Buyur, ne istemiştin?',t:[['satılık|ilan|fiyat|kaç|satın',()=>'Bu ev '+fmt(h.price)+'. Telefonundan İlanlar\'a gir, beğenirsen satın al.'],['kim|oturan','Bu mahallede yıllardır oturuyoruz.']],f:['Şimdi müsait değilim.','Başka bir şey var mı?']}});
// ---------- ev / araba / stad ----------
function enterHouse(h){inside=h;mp.set(1000,0,1001);yaw=0;pitch=.4;dist=2.8;me.rotation.y=PI}
function exitHouse(){const h=inside;inside=null;dist=7;mp.set(h.dx,0,h.dz+1.5)}
function sleep(){if(MP.sync)return toast('Online modda zaman atlanamaz');gt=(Math.floor(gt/300)+1)*300;toast('💤 Yeni güne uyandın ☀️')}
function enter(c){car=c;c.ai=null;me.visible=false;dist=9;yaw=c.a+PI}
function exitCar(){const c=car;car=null;me.visible=true;dist=7;c.v=0;mp.set(c.x-Math.cos(c.a)*2.4,0,c.z+Math.sin(c.a)*2.4);col(mp,.4)}
function watchStd(o){watch=o;me.visible=false;yaw=PI}
function exitStd(){const o=watch;watch=null;me.visible=true;mp.set(o.cx,0,o.cz+o.R+3)}
function drive(c,fx,fy,dt){if(fy>.1)c.v+=fy*13*dt;else if(fy<-.1)c.v-=(c.v>0?26:9)*-fy*dt;else c.v-=Math.sign(c.v)*Math.min(Math.abs(c.v),5*dt);
c.v=cl(c.v,-6,22);c.a-=fx*dt*1.7*cl(c.v/5,-1,1);c.x+=Math.sin(c.a)*c.v*dt;c.z+=Math.cos(c.a)*c.v*dt;
const px=c.x,pz=c.z;col(c,2.1);if(Math.hypot(c.x-px,c.z-pz)>.02)c.v*=.4;
if(c.v>2){const df=((c.a+PI-yaw+PI)%(2*PI)+2*PI)%(2*PI)-PI;yaw+=df*Math.min(1,dt*2)}}
function aiCar(c,dt){const a=c.ai;let sp=a.sp;const fx=Math.sin(c.a),fz=Math.cos(c.a);if(a.stops){const q=a.ax=='x'?c.x:c.z;if(c.wait>0){c.wait-=dt;sp=0}else for(const st of a.stops)if(Math.abs(q-st)<1.5&&c.ls!==st){c.wait=6;c.ls=st}if(c.ls!=null&&Math.abs(q-c.ls)>4)c.ls=null}
for(const o of CARS)if(o!==c){const dx=o.x-c.x,dz=o.z-c.z,al=dx*fx+dz*fz,lt=Math.abs(dx*fz-dz*fx);if(al>0&&al<9&&lt<2.4){sp=0;break}}
if(sp&&!car){const dx=mp.x-c.x,dz=mp.z-c.z,al=dx*fx+dz*fz,lt=Math.abs(dx*fz-dz*fx);if(al>0&&al<8&&lt<2)sp=0}
c.v+=(sp-c.v)*Math.min(1,dt*2);
{const o=a.o||0,b=a.b||285,ld=a.ld||1;if(a.ax=='x'){c.x+=a.dir*c.v*dt;if(c.x>b+o)c.x=-b+o;if(c.x<-b+o)c.x=b+o;c.z=a.r+a.dir*3*ld;c.a=a.dir>0?PI/2:-PI/2}
else{c.z+=a.dir*c.v*dt;if(c.z>215)c.z=-285;if(c.z<-285)c.z=215;c.x=a.r-a.dir*3*ld;c.a=a.dir>0?0:PI}}}
// ---------- telefon ----------
const NM=["ahmet","ayse","mehmet","zeynep","can","elif","emre","deniz","burcu","kerem","selin","murat"];
const TP=[()=>"Saat "+clk()+" ve İstanbul "+(gt%300<180?"ışıl ışıl ☀️":"gece ışıkları içinde 🌃"),"Rams Park'ta maç var mı? Bilet bulan?","Kadıköy'de simit + çay = mutluluk 🥯","Trafik yine felaket 😩 #İstanbul","Maltepe sahilinde yürüyüş şart 🌊","Fener–Gala biletleri uçtu!","Yeni ev ilanlarına baktınız mı? 🏠","Tüpraş'ın alevi gece uzaktan görünüyor 🔥","Taksiciye sordum: 'Trafik hafif' dedi. Yalan 😂"];
function genPost(f){const t=pick(TP);S.feed.unshift({u:f||pick(NM),c:[],t:typeof t=='function'?t():t,l:Math.floor(rnd(2,400))});if(S.feed.length>30)S.feed.pop()}
for(let i=0;i<8;i++)genPost();S.ls=H.slice().sort(()=>Math.random()-.5).slice(0,12);
const rd=()=>{const h=pick(H);return{x:h.dx,z:h.dz}},rp=()=>Math.random()<.5?{x:rnd(-250,250),z:pick(HR)}:{x:pick(VR),z:rnd(-250,190)};
const JB=[{n:"Kurye",pay:16000,car:0,g:()=>[["📦 Paketi al",rd()],["🏠 Teslim et",rd()]]},{n:"Taksici",pay:28000,car:1,g:()=>[["🧍 Yolcuyu al",rp()],["📍 Yolcuyu bırak",rp()]]},{n:"Stad Güvenliği",pay:20000,car:0,g:()=>[0,1,2].map(i=>["🔦 Kontrol noktası "+(i+1),{x:-150+Math.cos(i*2.1)*46,z:-130+Math.sin(i*2.1)*46}])},{n:"Rafineri Operatörü",pay:34000,car:0,g:()=>[[-25,-20],[25,-20],[5,10]].map((q,i)=>["🔧 Vana kontrolü "+(i+1),{x:150+q[0],z:-130+q[1]+12}])},{n:"Emlak Danışmanı",pay:40000,car:0,g:()=>[["🏠 Evi göster",rd()],["🤝 İkinci evi göster",rd()]]}];
function startMs(){const j=S.job;if(!j)return;const pts=j.g();let tot=0,px=mp.x,pz=mp.z;pts.forEach(p=>{tot+=Math.hypot(p[1].x-px,p[1].z-pz);px=p[1].x;pz=p[1].z});S.ms={pts,i:0,pay:Math.round(j.pay*(1+tot/400))};toast('💼 Yeni görev: '+pts[0][0])}
const PT={ucus:'Uçuş',online:'Online',wa:'WhatsApp',map:'Harita',aile:'Aile',atm:'ATM · Banka',gram:'Gram',call:'Ara',ev:'İlanlar',bilet:'Biletler',job:'Meslek'},PA=[['gram','📸'],['call','📞'],['ev','🏠'],['bilet','🎟️'],['job','💼'],['aile','👨‍👩‍👧'],['wa','💬'],['map','🗺️'],['ucus','✈️'],['online','🌐']];
const RA={
gram:()=>{const tabs=[['feed','Akış'],['exp','Keşfet'],['pro','Profil'],['dm','Mesaj'+(S.unread?' ('+S.unread+')':'')]];let h='<div>'+tabs.map(t=>`<button class="a" data-a="gtab" data-g="${t[0]}"${gtab==t[0]?' style="background:#c0392b"':''}>${t[1]}</button>`).join('')+'</div>';
if(gtab=='feed')h+=`<div class="prow"><input id="pi" placeholder="Ne düşünüyorsun?" maxlength="120"><button class="a" data-a="post">Paylaş</button></div>`+S.feed.slice(0,10).map((p,i)=>`<div class="po"><b>@${p.u}${p.m?' (sen)':''}</b> ${esc(p.t)}<div class="sm">${(p.c||[]).slice(-2).map(esc).join(' · ')}</div><button class="a" data-a="like" data-g="${i}">${p.liked?'❤️':'🤍'} ${p.l}</button></div>`).join('');
else if(gtab=='exp')h+=N.slice(0,RAW.length).map((n,i)=>`<div class="it"><b>${n.d.n}</b> <span class="sm">${n.d.r}</span><br><button class="a" data-a="fol" data-g="${i}">${S.following.includes(i)?'✓ Takiptesin':'Takip et'}</button></div>`).join('');
else if(gtab=='pro')h+=`<div class="it"><b>@sen</b><br>👥 ${S.fol} takipçi · ➡️ ${S.following.length} takip · 📝 ${S.mine.length} gönderi<div class="sm">Sponsorluk: her 1.000 takipçide +25.000 ₺. Maçta ve arabadayken paylaşım daha çok ilgi görür.</div></div>`;
else{S.unread=0;h+=S.dm.length?S.dm.slice(-8).reverse().map((d,i)=>{const ix=S.dm.length-1-i;return `<div class="it"><b>${N[d.id].d.n}</b>: ${d.t}<br>${d.done?'<span class="sm">Yanıtlandı</span>':['😊 Olur!','🙂 Belki','🚫 Şimdi değil'].map((x,o)=>`<button class="a" data-a="dmr" data-g="${ix}:${o}">${x}</button>`).join('')}</div>`}).join(''):'<div class="sm">Henüz mesaj yok.</div>'}
return h},
ucus:()=>{const pp=car||mp,ap=AIRP.find(a=>Math.hypot(a.x-pp.x,a.z-pp.z)<70);let h=ap?'<div class="sm">✈️ '+ap.n+'</div>':'<div class="it sm">Uçmak için bir havalimanı terminaline git (Harita → taksi!).</div>';
CTY.forEach((c,k)=>{if(ap&&k!=ap.k){const f=FLT(ap.k,k);h+=`<div class="it"><b>${c.fl} ${c.nm}</b><div class="sm">Uçuş süresi: ${f[0]/60} dk · ${fmt(f[1])}</div><button class="a" data-a="fly" data-g="${k}">✈️ Bilet al ve uç</button></div>`}});return h},
online:()=>{const rm=MP.rm,ps=rm?rm.peers().filter(q=>q.kind=='viewer'):[];return `<div class="it">🌐 ${esc(MP.st)}<br><span class="sm">Adın: <b>${esc(MP.name)}</b> · Aynı oda kodunu yazan arkadaşların seninle oynar.</span></div>`+(rm?`<div class="sm">Çevrimiçi: ${ps.length} kişi</div>`+ps.map(q=>`<div class="it">${q.isMe?'⭐ ':''}${esc(String((q.presence&&q.presence.n)||'Oyuncu'))}${q.isMe?' (sen)':''}</div>`).join('')+MP.chat.slice(-8).map(m=>`<div class="po"><b>${esc(m.n)}</b>: ${esc(m.t)}</div>`).join('')+`<div class="prow"><input id="ci" placeholder="Mesaj yaz..." maxlength="120"><button class="a" data-a="csend">Gönder</button></div>`:'<div class="it sm">Çok oyunculu, claude.ai içinde giriş yapmış oyuncularla çalışır. Genel web bağlantısında tek başına oynarsın.</div>')},
wa:()=>{const cs=waAll();if(!wk){S.wu=0;return '<button class="a" data-a="wopen" data-g="aile">👨‍👩‍👧 Aile grubu</button>'+cs.map(c=>`<button class="a" data-a="wopen" data-g="${c.k}">${c.l}</button>`).join('')+'<div class="sm">Arkadaş eklemek için biriyle sohbet edip yakınlaş (❤️ 12) ve "Arkadaş ol"a bas.</div>'}
const th=(S.wa[wk]||[]).slice(-12);return `<button class="a" data-a="wback">◀ Kişiler</button> <b>${wk=='aile'?'Aile grubu':(cs.find(c=>c.k==wk)||{l:wk}).l}</b>`+th.map(m=>`<div class="po" style="${m.m?'text-align:right;background:#0d4d3a':''}">${esc(m.t)}</div>`).join('')+`<div class="prow"><input id="wi" placeholder="Mesaj yaz..." maxlength="120"><button class="a" data-a="wsend">Gönder</button></div>`},
map:()=>{const q=MAPP[S.msel];let h='<canvas id="mc" width="320" height="300" data-a="mapc"></canvas><div class="sm">🟠 Yer · 🟢 Okul · 🔴 Metro · 🔵 Durak · ⚪ Sen — dokunarak seç</div>';
if(q){const pp=car||mp,d=Math.hypot(q[1]-pp.x,q[2]-pp.z),fare=Math.max(3000,Math.round(d*25)),near=TRN.find(t=>Math.hypot(t.x-pp.x,t.z-pp.z)<25&&t.t==q[3]);
h+=`<div class="it"><b>${q[0]}</b><div class="sm">${Math.round(d)} m uzakta</div><button class="a" data-a="mwp">📍 İşaretle</button><button class="a" data-a="mtx">🚕 Taksi (${fmt(fare)})</button>${near?`<button class="a" data-a="mtr">${q[3]=='m'?'🚇 Metro (20 ₺)':'🚌 Otobüs (15 ₺)'}</button>`:'<div class="sm">Toplu taşıma: önce bir durağa/istasyona git, hedefi de durak/istasyon seç.</div>'}</div>`}
return h},
aile:()=>{let h=`<div class="it">Mutluluk: <b>${Math.round(S.hap)}%</b></div><div class="it">👩 <b>Anne</b> · 👨 <b>Baba</b><br><button class="a" data-a="fcall" data-g="anne">📞 Anneyi ara</button><button class="a" data-a="fcall" data-g="baba">📞 Babayı ara</button><button class="a" data-a="fdine">🍽️ Aileyle yemek (20.000 ₺)</button><button class="a" data-a="fgift">🎁 Anneye hediye (50.000 ₺)</button></div>`;
const top=Object.entries(S.rel).filter(e=>e[1]>0).sort((a,b)=>b[1]-a[1]).slice(0,5);if(top.length)h+='<div class="sm">Yakın çevre</div>'+top.map(e=>`<div class="it">❤️ ${e[1]} · ${N[e[0]].d.n}</div>`).join('');return h},
call:()=>N.slice(0,RAW.length).map((n,i)=>`<button class="a" data-a="call" data-g="${i}">📞 ${n.d.n}</button>`).join('')+'<button class="a" data-a="call" data-g="t">🚕 Taksi çağır</button><button class="a" data-a="call" data-g="e">🏢 Emlakçı Kenan</button>',
ev:()=>[...H.filter(h=>h.own),...S.ls.filter(h=>!h.own)].map(h=>`<div class="it"><b>${h.name}</b><div class="sm">${h.type} · ${h.dn}</div><div>${h.own?'✅ Sahipsin · kira ~'+fmt(h.price*.0025)+'/gün':fmt(h.price)}</div><button class="a" data-a="pin" data-g="${h.id}">📍 Göster</button>${h.own?`<button class="a" data-a="sell" data-g="${h.id}">Sat</button>`:`<button class="a" data-a="buy" data-g="${h.id}">Satın al</button>`}</div>`).join(''),
atm:()=>`<div class="it">💵 Nakit: <b>${fmt(S.money)}</b><br>🏦 Hesap: <b>${fmt(S.bank)}</b><div class="sm">Günlük faiz %0,5</div></div><div class="it">Yatır<br>${[1e4,1e5,1e6,1e7].map(v=>`<button class="a" data-a="dep" data-g="${v}">${fmt(v)}</button>`).join('')}<button class="a" data-a="dep" data-g="all">Tümü</button></div><div class="it">Çek<br>${[1e4,1e5,1e6,1e7].map(v=>`<button class="a" data-a="wd" data-g="${v}">${fmt(v)}</button>`).join('')}<button class="a" data-a="wd" data-g="all">Tümü</button></div>`,
bilet:()=>STD.map((o,i)=>{const s=mst(o),kk=s.ph==2?s.k+1:s.k,f=fixture(o,kk),has=S.tix[o.id+':'+kk],st=s.ph==0?'Başlamasına '+(110-(s.tt|0))+' sn':s.ph==1?'🔴 CANLI '+s.min+"' · "+s.sc[0]+'-'+s.sc[1]:'Maç bitti · sonraki maç yakında';return `<div class="it"><b>${o.name}</b><div>${f.ucl?'⭐ Şampiyonlar Ligi · ':''}${f.home} – ${f.away}</div><div class="sm">${st}</div><div>Bilet: <b>${fmt(f.price)}</b></div>${has?'✅ Biletin var':`<button class="a" data-a="tix" data-g="${i}">Satın al</button>`}</div>`}).join(''),
job:()=>(S.job?`<div class="it">Aktif: <b>${S.job.n}</b><br><button class="a" data-a="quit">İşi bırak</button></div>`:'')+JB.map((j,i)=>`<div class="it"><b>${j.n}</b> · ${fmt(j.pay)}+ / görev ${j.car?'· 🚗 araba gerekli':''}<br><button class="a" data-a="job" data-g="${i}">Başla</button></div>`).join('')};
function render(){let h=`<div class="phh"><b>${clk()}</b><span>${fmt(S.money)}</span><button class="a" data-a="x">✕</button></div>`;
if(!app)h+='<div class="grid">'+PA.map(a=>`<button data-a="open" data-g="${a[0]}"><i>${a[1]}</i>${PT[a[0]]}</button>`).join('')+'</div>';
else h+=`<div class="bar"><button class="a" data-a="home">◀</button><b>${PT[app]}</b></div><div class="pc">${RA[app]()}</div>`;
$('ph').innerHTML=h;if(app=='map')drawMap()}
function openPhone(){phoneOpen=true;for(const k in keys)delete keys[k];$('ph').style.display='flex';render()}
function closePhone(){phoneOpen=false;if(app=='atm')app='';$('ph').style.display='none'}
$('pb').onclick=()=>phoneOpen?closePhone():openPhone();
const AC={fly:g=>startFlight(+g),csend:()=>{const v=$('ci').value.trim();if(!v||!MP.rm)return;MP.rm.emit('chat',{n:MP.name,t:v.slice(0,120)}).catch(()=>toast('Mesaj gönderilemedi'));$('ci').value=''},wopen:g=>{wk=g;render()},wback:()=>{wk='';render()},
wsend:()=>{const v=esc($('wi').value.trim());if(!v)return;const k=wk;(S.wa[k]=S.wa[k]||[]).push({m:1,t:v});const cs=waAll(),tg=k=='aile'?cs.filter(c=>c.f).slice(0,3):[cs.find(c=>c.k==k)];
tg.forEach((c,i)=>setTimeout(()=>{if(!c)return;(S.wa[k]=S.wa[k]||[]).push({t:(k=='aile'?c.l+': ':'')+reply(c.o,v)});if(c.o.id!=null)S.rel[c.o.id]=(S.rel[c.o.id]||0)+1;if(phoneOpen&&app=='wa'&&wk==k)render()},700+i*800));render()},
mapc:(g,e)=>{const r=e.target.getBoundingClientRect(),cx=(e.clientX-r.left)*320/r.width,cy=(e.clientY-r.top)*300/r.height;let bi=-1,bd=18;MAPP.forEach((q,i)=>{const d=Math.hypot((q[1]-cxm+300)/600*320-cx,(q[2]+300)/560*300-cy);if(d<bd){bd=d;bi=i}});S.msel=bi;render()},
mwp:()=>{const q=MAPP[S.msel];if(!q)return;S.wp={x:q[1],z:q[2],t:'📍 '+q[0]};closePhone();toast('Rota işaretlendi')},
mtx:()=>{const q=MAPP[S.msel];if(!q)return;const pp=car||mp;goTo(q,Math.max(3000,Math.round(Math.hypot(q[1]-pp.x,q[2]-pp.z)*25)),'🚕 Taksiyle vardın')},
mtr:()=>{const q=MAPP[S.msel];if(!q)return;goTo(q,q[3]=='m'?20:15,q[3]=='m'?'🚇 Metroyla vardın':'🚌 Otobüsle vardın')},
dep:g=>{const v=g=='all'?S.money:+g;if(v>S.money||v<=0)return toast('Yetersiz nakit');S.money-=v;S.bank+=v;render()},wd:g=>{const v=g=='all'?S.bank:+g;if(v>S.bank||v<=0)return toast('Hesapta yeterli para yok');S.bank-=v;S.money+=v;render()},x:closePhone,open:g=>{app=g;render()},home:()=>{if(app=='atm')closePhone();else{app='';render()}},
post:()=>{const v=esc($('pi').value.trim());if(!v)return;const mul=(watch?3:1)*(car?1.5:1),p={u:'sen',t:v,l:Math.floor(rnd(5,40)),m:1,c:[]};S.feed.unshift(p);S.mine.push(p);S.fol+=Math.floor(rnd(5,60)*mul);sponsor();render()},
gtab:g=>{gtab=g;render()},like:g=>{const p=S.feed[+g];if(!p)return;p.liked=!p.liked;p.l+=p.liked?1:-1;render()},
fol:g=>{const i=+g,x=S.following.indexOf(i);if(x>=0)S.following.splice(x,1);else S.following.push(i);render()},
dmr:g=>{const a=g.split(':').map(Number),d=S.dm[a[0]];if(!d||d.done)return;d.done=1;S.rel[d.id]=(S.rel[d.id]||0)+[6,2,-3][a[1]];toast(a[1]==2?'Mesajı reddettin':'💬 Yanıt gönderildi');render()},
fcall:g=>{closePhone();openChat(PAR[g],1)},
fdine:()=>{if(S.money<2e4)return toast('Yetersiz nakit');S.money-=2e4;S.hap=Math.min(100,S.hap+12);toast('🍽️ Güzel bir aile yemeği! Mutluluk arttı');render()},
fgift:()=>{if(S.money<5e4)return toast('Yetersiz nakit');S.money-=5e4;S.hap=Math.min(100,S.hap+8);toast('🎁 Annen çok mutlu oldu');render()},
call:g=>{closePhone();if(g=='t'){const a=yaw+PI;spawnCar(mp.x+Math.sin(a)*8,mp.z+Math.cos(a)*8,a,0xf1c40f,null).taxi=true;openChat({d:{n:'Taksi Durağı',r:'Çağrı merkezi',hi:'Taksi mi? Hemen geliyoruz, olduğun yerde bekle! (Sarı araç yanında)',t:[],f:['Adresi söyle, geliyoruz.','Hat yoğun, birazdan orada.']}},1)}
else if(g=='e')openChat({d:{n:'Emlakçı Kenan',r:'Emlak danışmanı',hi:'Buyur, fırsat ev mi arıyorsun?',t:[['ilan|ev|fiyat|fırsat|ucuz',()=>{const h=H.reduce((a,b)=>a.price<b.price?a:b);return 'En uygun fiyat: '+h.name+' '+fmt(h.price)+'.'}]],f:FBK}},1);else openChat(N[g],1)},
pin:g=>{const h=H[g];S.wp={x:h.dx,z:h.dz,t:'📍 '+h.name};toast('Harita işaretlendi');},
buy:g=>{const h=H[g];if(S.money<h.price)return toast('Yeterli paran yok');S.money-=h.price;h.own=true;setDoor(h);toast('🏠 Satın aldın: '+h.name);render()},
sell:g=>{const h=H[g];S.money+=h.price*.92;h.own=false;setDoor(h);toast('Evi sattın');render()},
tix:g=>{const o=STD[g],s=mst(o),kk=s.ph==2?s.k+1:s.k,f=fixture(o,kk);if(S.money<f.price)return toast('Yeterli paran yok');S.money-=f.price;S.tix[o.id+':'+kk]=1;toast('🎟️ Bilet alındı: '+f.home+'–'+f.away);render()},
job:g=>{S.job=JB[g];startMs();render()},quit:()=>{S.job=null;S.ms=null;render()}};
$('ph').addEventListener('click',e=>{const b=e.target.closest('[data-a]');if(b&&AC[b.dataset.a])AC[b.dataset.a](b.dataset.g,e)});
// ---------- bağlam düğmesi ----------
const ctxB=$('ctx');let ctxF=null;const ctxGo=()=>{if(ctxF&&!chatOpen&&!phoneOpen)ctxF()};ctxB.onclick=ctxGo;
// ---------- sosyal medya / aile / evlilik ----------
let gtab='feed',chatCall=false;const FW=[];
const CM=["Harika! 🔥","Çok güzel olmuş","Helal olsun 👏","Hangi mahalle burası?","Ben de oradaydım!"],DMT=["Selam! Müsait misin?","Bu akşam maça gidiyor musun?","Gram'daki paylaşımını gördüm, çok güzel!","Seninle kahve içmek isterim ☕","Şehirde yeni bir mekan açıldı, gidelim mi?"],KN=["Ali","Ayşe","Emir","Zeynep","Can","Elif","Mert","Defne"];
function sponsor(){const k=Math.floor(S.fol/1000);if(k>S.sp){S.money+=25000*(k-S.sp);S.sp=k;toast('🤝 Sponsor geliri +'+fmt(25000*k))}}
function tick14(){genPost(S.following.length&&Math.random()<.5?N[pick(S.following)].d.n:null);
S.mine.slice(-6).forEach(p=>{const g=Math.floor(rnd(0,S.fol*.05+3));p.l+=g;S.fol+=Math.floor(g*.2);if(p.l>10&&p.c.length<3&&Math.random()<.5)p.c.push(pick(NM)+': '+pick(CM))});sponsor();
if(mode=='play'&&Math.random()<.3){const id=Math.floor(rnd(0,RAW.length));S.dm.push({id,t:pick(DMT),done:0});S.unread++;toast('📩 '+N[id].d.n+' sana mesaj gönderdi')}
waGen();if(phoneOpen&&(app=='gram'||app=='wa'))render()}
function renderAct(n){const c=$('cact');c.innerHTML='';const rl=n.id!=null?(S.rel[n.id]||0):0,ok=n.id!=null&&!n.pol&&!n.kid;$('cn').textContent=(chatCall?'📞 ':'')+n.d.n+' · '+n.d.r+(ok?' · ❤️ '+rl:'');if(!ok)return;
const b=(t,fn)=>{const e=document.createElement('button');e.textContent=t;e.onclick=fn;c.appendChild(e)};
b('☕ Kahve (5.000 ₺)',()=>{if(S.money<5e3)return toast('Yetersiz nakit');S.money-=5e3;S.rel[n.id]=rl+4;add('npc',n.d.n+': Ne kadar naziksin, teşekkürler!');renderAct(n)});
b('🎁 Hediye (50.000 ₺)',()=>{if(S.money<5e4)return toast('Yetersiz nakit');S.money-=5e4;S.rel[n.id]=rl+9;add('npc',n.d.n+': Bu çok güzel bir hediye, sağ ol!');renderAct(n)});
if(rl>=12&&!S.friends.includes(n.id))b('🤝 Arkadaş ol',()=>{S.friends.push(n.id);toast('🤝 Artık arkadaşsınız! WhatsApp\'tan yazabilirsin');renderAct(n)})}
function propose(n){const rl=S.rel[n.id]||0;if(S.money<5e5)return toast('Yüzük için yeterli paran yok');S.money-=5e5;
if(Math.random()<(rl>=60?1:(rl-30)/40)){S.spouse=n.id;n.fol=true;S.hap=85;add('npc',n.d.n+': Evet! Seninle evlenmek istiyorum! 💍');toast('💒 Evlendiniz! Eşin artık seni takip ediyor');n.g.position.set(mp.x+1.5,0,mp.z+1.5);fire(mp.x,mp.z)}
else{S.rel[n.id]=Math.max(0,rl-6);add('npc',n.d.n+': Henüz hazır değilim... Biraz zamana ihtiyacım var.')}renderAct(n)}
function addKid(){const nm=pick(KN),e=mkNpc({n:nm,r:'Çocuğun',hi:'Anne/Baba! 😄',t:[['oyun|oyuncak','Oyun oynayalım!'],['sev','Ben de seni seviyorum!']],f:['Hihi!','Aaa!','Beni parka götür!']},mp.x+1,mp.z+1);e.fol=true;e.kid=true;e.g.scale.setScalar(.55);S.kids.push(nm);S.hap=Math.min(100,S.hap+15);toast('👶 '+nm+' doğdu! Tebrikler!');fire(mp.x,mp.z)}
function folUpd(n,dt){const p=n.g.position;if(car||inside||watch){n.g.visible=false;p.set(mp.x+1.2,0,mp.z+1.2);return}n.g.visible=true;n.lb.visible=true;
const o=n.kid?-1.4:1.4,dx=mp.x+o-p.x,dz=mp.z+1.2-p.z,d=Math.hypot(dx,dz);let mv=0;
if(chatOpen&&chatNpc===n)face(n.g,mp.x-p.x,mp.z-p.z,dt);else if(d>2.4){const sp=cl(d*.9,0,d>8?6:3.2);p.x+=dx/d*sp*dt;p.z+=dz/d*sp*dt;mv=sp/3.2;face(n.g,dx,dz,dt)}
if(d>45)p.set(mp.x+o,0,mp.z+1.2);col(p,.4);n.ph+=mv*dt*5;anim(n.g,n.ph,Math.min(.8,mv*.5))}
function fire(x,z){for(let k=0;k<4;k++){const nn=60,g=new T.BufferGeometry(),pos=new Float32Array(nn*3),v=[],cx=x+rnd(-12,12),cz=z+rnd(-12,12),cy=rnd(18,30);
for(let i=0;i<nn;i++){pos.set([cx,cy,cz],i*3);const a=rnd(0,PI*2),e=rnd(-1,1),r=rnd(4,9),q=Math.sqrt(1-e*e);v.push([Math.cos(a)*q*r,e*r,Math.sin(a)*q*r])}
g.setAttribute('position',new T.BufferAttribute(pos,3));const m=new T.Points(g,new T.PointsMaterial({color:pick([0xff4444,0xffd24a,0x66ccff,0xff66ff]),size:3,sizeAttenuation:false,transparent:true,fog:false}));m.frustumCulled=false;scene.add(m);FW.push({m,v,t:0,d:rnd(0,.8)})}}
// ---------- ÜLKELER ----------
const SCLS=[[0,1,4,5,7,8,10,12,13,16],[0,2,4,7,9,12,14,16,19,21],[0,2,3,5,7,9,10,12,14,15],[0,1,4,5,7,8,10,12,13,16]],
CE=[{g:0,fs:1,ss:1},{g:.05,fs:1.1,ss:1.05},{g:.5,fs:.6,ss:.75},{g:0,fs:1.25,ss:1.2}],GRY=new T.Color(0x8a9097),FLMS=[flagM],
NEON=[0xff4fd8,0x38f9ff,0xffe14a,0x7cff4f,0xff5a4f],SPIN=[],BAL=[],WTM=new T.MeshStandardMaterial({color:0x3a7fb5,roughness:.2,metalness:.25});
function mkFlag(fn){const c=document.createElement('canvas');c.width=300;c.height=200;fn(c.getContext('2d'));const m=new T.MeshBasicMaterial({map:new T.CanvasTexture(c),side:T.DoubleSide});FLMS.push(m);return m}
const CTY=[
{nm:'Türkiye',fl:'🇹🇷',cx:0,ap:{x:244,z:150},apn:'İstanbul Sabiha Gökçen',W:[],HR:HR,VR:VR,regs:D},
{nm:'Amerika',fl:'🇺🇸',cx:3000,g:'#6f9a55',gc:'#6f9a55',tk:'o',beach:[-250,120],ap:{x:228,z:60},apn:'John F. Kennedy Havalimanı',HR:[-170,-45,35,115,195],VL:[-190,-70,50,150],W:[[160,240,-285,-190]],ld:1,taxi:0xf1c40f,cc:[0x2c3e50,0xc0392b,0xecf0f1,0x7f8c8d,0x1f4aa8],bus:'sb',lang:'en',names:["Jake","Emily","Tyler","Megan","Brandon","Ashley","Chris","Jessica"],
ff:g=>{for(let i=0;i<13;i++){g.fillStyle=i%2?'#fff':'#b22234';g.fillRect(0,i*200/13,300,200/13+1)}g.fillStyle='#3c3b6e';g.fillRect(0,0,120,108);g.fillStyle='#fff';for(let r=0;r<5;r++)for(let c=0;c<6;c++)g.fillRect(10+c*19,8+r*20,4,4)},
zones:[[-10,-110,18],[24,-145,18],[100,-108,52],[-130,75,32],[-10,75,24],[112,75,26],[-130,150,28],[-10,176,40],[94,176,30],[250,60,80]],
rg:[{n:'New York',x:20,z:-110,h:[30,85],pal:[0xb8c4d0,0x9fb3c8,0xa88b7a,0x7f8c9a,0xc9ced3],tk:'o',tp:.2,up:1},{n:'Washington',x:0,z:75,h:[10,18],pal:[0xf4f1ea,0xe8e2d0,0xd8cfc0,0xf0ebe0],tk:'c',tp:.5},{n:'Florida',x:-30,z:165,h:[6,13],pal:[0xf8b6c8,0x9be3d6,0xfff0a8,0xffc9a0,0xb6d4ff],tk:'p',tp:.8}],
po:[['Empire State Binası',-10,-84],['Times Meydanı',24,-135],['Central Park',100,-52],['Özgürlük Heykeli (liman)',200,-182],['ABD Kongre Binası',-130,92],['Beyaz Saray',-10,92],['Washington Anıtı',102,58],['Ocean Drive (Miami)',36,190],['NASA Roketi',-130,168],['Miami Plajı',-60,215]]},
{nm:'İngiltere',fl:'🇬🇧',cx:6000,g:'#4f8a4a',gc:'#4f8a4a',tk:'o',ap:{x:228,z:-60},apn:'Heathrow Havalimanı',HR:[-230,-90,-10,50,170],VL:[-140,0,60,150],W:[[-290,290,90,112,[-140,0,60,150]]],ld:-1,taxi:0x111111,cc:[0x2c3e50,0xc0392b,0xecf0f1,0x1f4aa8,0x2e7d32],bus:'dd',lang:'en',names:["Oliver","Charlotte","Harry","Amelia","Jack","Emily","George","Poppy"],
ff:g=>{g.fillStyle='#012169';g.fillRect(0,0,300,200);g.strokeStyle='#fff';g.lineWidth=34;g.beginPath();g.moveTo(0,0);g.lineTo(300,200);g.moveTo(300,0);g.lineTo(0,200);g.stroke();g.strokeStyle='#c8102e';g.lineWidth=14;g.stroke();g.strokeStyle='#fff';g.lineWidth=56;g.beginPath();g.moveTo(150,0);g.lineTo(150,200);g.moveTo(0,100);g.lineTo(300,100);g.stroke();g.strokeStyle='#c8102e';g.lineWidth=32;g.stroke()},
zones:[[26,82,30],[50,72,12],[105,72,30],[-70,-160,64],[250,-60,80],[-90,-45,30]],
rg:[{n:'Manchester',x:-70,z:-100,h:[10,22],pal:[0x9b4a3c,0x7a3b2e,0xa0522d,0x8b4a3a],tk:'o',tp:.2},{n:'London',x:110,z:90,h:[14,30],pal:[0x8b5a3c,0xa9856a,0xc9b79c,0x6b6b6b,0xb89a7a],roof:1,rc:[0x4a4f55,0x5b4636],tk:'o',tp:.25},{n:'Bristol',x:-190,z:130,h:[8,14],pal:[0xf4a6a6,0xa6c8f4,0xf4e0a6,0xa6f4c8,0xf4a6e0,0xe9c46a],roof:1,rc:[0x6b4a3a,0x555555],tk:'o',tp:.4}],
po:[['Big Ben',50,60],['London Eye',105,50],['Tower Bridge',150,86],['Clifton Köprüsü',-140,86],['Old Trafford',-70,-110],['Bristol Limanı',-200,118],['Manchester Merkez',-70,-50]]},
{nm:'İspanya',fl:'🇪🇸',cx:9000,g:'#c2a36a',gc:'#c2a36a',tk:'p',beach:[-50,170],ap:{x:228,z:90},apn:'Madrid-Barajas Havalimanı',HR:[-230,-110,-30,70,160,200],VL:[-190,-80,10,100,190],W:[],ld:1,taxi:0,cc:[0xecf0f1,0xc0392b,0x2980b9,0x95a5a6,0x16a085],bus:'es',lang:'es',names:["Carlos","Lucía","Pablo","Carmen","Javier","Marta","Diego","Elena"],
ff:g=>{g.fillStyle='#aa151b';g.fillRect(0,0,300,200);g.fillStyle='#f1bf00';g.fillRect(0,50,300,100)},
zones:[[-135,-70,52],[-135,-170,48],[-35,-70,16],[-35,-140,14],[55,-180,52],[145,-170,52],[55,30,40],[145,30,14],[145,115,34],[250,90,80]],
rg:[{n:'Madrid',x:-110,z:-110,h:[14,26],pal:[0xe3b27a,0xd9926a,0xf0d9a6,0xc97b5a,0xdcc08a],tk:'o',tp:.25},{n:'Barcelona',x:110,z:80,h:[14,24],pal:[0xf0e2b8,0xe8d09a,0xd9c9a8,0xf4e4c4,0xe9c9a0],tk:'p',tp:.5}],
po:[['Plaza Mayor',-135,-40],['Kraliyet Sarayı',-135,-145],['Cibeles Çeşmesi',-35,-50],['Puerta de Alcalá',-35,-120],['Santiago Bernabéu',55,-140],['Camp Nou',145,-130],['Sagrada Familia',55,-5],['Torre Glòries',145,10],['Park Güell',145,90],['Barceloneta Plajı',60,215]]}];
CTY.forEach((C,k)=>{if(!k){C.fm=flagM;return}C.VR=C.VL.map(x=>x+C.cx);C.regs=C.rg.map(r=>({...r,x:r.x+C.cx}));C.fm=mkFlag(C.ff);C.gm=new T.MeshStandardMaterial({map:nt(C.g,8,800,640),roughness:1});C.mapp=[...C.po.map(q=>[q[0],q[1]+C.cx,q[2],'p']),[C.apn,C.cx+C.ap.x,C.ap.z,'p']]});
const AIRP=CTY.map((c,k)=>({k,n:c.apn,x:c.cx+c.ap.x,z:c.ap.z})),FLD={'0-1':[180,350000],'0-2':[120,220000],'0-3':[60,140000],'2-3':[60,120000],'1-2':[120,300000],'1-3':[120,320000]},FLT=(a,b)=>FLD[a<b?a+'-'+b:b+'-'+a];
const RG={
'Florida':{r:[["Lifeguard","Beach is open! Stay between the flags! (Plaj açık! Bayrakların arasında kal!)",[["beach|sea|plaj|deniz","The water's warm today, perfect for swimming. (Su sıcak, yüzmek için ideal.)"],["shark|köpekbalığı","Haha, sharks are rare here. (Köpekbalığı nadirdir.)"]]],["NASA Engineer","We launch rockets near here! (Yakında roket fırlatıyoruz!)",[["rocket|space|roket|uzay","Next launch is soon, watch the sky! (Yakında fırlatma var!)"],["moon|ay|mars","Mars is next! (Sırada Mars var!)"]]],["Cuban café owner","¡Hola! Cuban coffee? (Küba kahvesi ister misin?)",[["coffee|kahve|cafe","Strong and sweet, that's how we make it. (Güçlü ve tatlı.)"],["miami|florida","Miami never sleeps. (Miami uyumaz.)"]]]],f:["Sun's out, life's good! (Güneş açık, hayat güzel!)","Grab an iced tea, friend! (Buzlu çay al dostum!)"],sh:[0xff6b6b,0x48dbfb,0xfeca57,0xffffff,0x1dd1a1],hat:[0xffffff]},
'New York':{r:[["Hot dog vendor","Hot dogs! Get your hot dogs! (Sosisli!)",[["hot dog|food|yemek","Two bucks, best dog in Manhattan. (İki dolar, Manhattan'ın en iyisi.)"],["subway|metro","Take the subway, it's faster. (Metro daha hızlı.)"]]],["Broadway actor","Break a leg tonight! (Bu akşam bol şans!)",[["show|broadway|theatre|tiyatro","Tickets are sold out, my friend. (Biletler tükendi.)"],["times square","Times Square never goes dark. (Times Square hep ışıl ışıl.)"]]],["Taxi driver","Where to, buddy? (Nereye dostum?)",[["taxi|cab|taksi","Yellow cabs are everywhere. (Sarı taksiler her yerde.)"],["traffic|trafik","Traffic is crazy today! (Trafik çılgın!)"]]],["Banker","Wall Street is busy as ever. (Wall Street hep yoğun.)",[["stock|borsa|wall street","Markets are up today. (Borsa yükselişte.)"],["coffee|kahve","I need my coffee. (Kahveye ihtiyacım var.)"]]]],f:["Hey, I'm walkin' here! (Hey, yürüyorum burada!)","Only in New York. (Sadece New York'ta.)"],sh:[0x222222,0x34495e,0x7f8c8d,0xc0392b,0xffffff],hat:[0x222222]},
'Washington':{r:[["Tour guide","Welcome to the Capital! (Başkente hoş geldin!)",[["white house|beyaz saray","The President lives there. (Başkan orada yaşar.)"],["monument|anıt","It's 169 meters tall. (169 metre yüksekliğinde.)"]]],["Senator's aide","Busy day on Capitol Hill. (Yoğun bir gün.)",[["congress|kongre|capitol","Congress is in session. (Kongre toplantıda.)"],["politics|siyaset","I can't talk politics. (Siyaset konuşamam.)"]]],["Jogger","Running by the reflecting pool! (Yansıma havuzunda koşuyorum!)",[["cherry|kiraz","Cherry blossoms are beautiful in spring. (Kiraz çiçekleri baharda harika.)"],["run|koş","Best view for a morning run. (Sabah koşusu için en iyi manzara.)"]]]],f:["Lovely day in D.C. (D.C.'de güzel bir gün.)","Have you seen the monument? (Anıtı gördün mü?)"],sh:[0x2c3e50,0xffffff,0x34495e,0xa8d8a0],hat:[]},
'London':{r:[["Bobby (Police)","Evening, sir! All fine? (İyi akşamlar! Her şey yolunda mı?)",[["big ben|saat","Big Ben is just there. (Big Ben hemen orada.)"],["lost|kayıp","Use the Tube, mate. (Metroya bin.)"]]],["Pub landlord","Pint of lager? (Bira ister misin?)",[["pub|beer|bira","Best pub in town, guv'nor. (Şehrin en iyi pub'ı.)"],["football|maç","Premier League night! (Premier Lig gecesi!)"]]],["Tourist guide","Mind the gap! (Boşluğa dikkat!)",[["tower bridge|köprü","Tower Bridge opens for big ships. (Büyük gemiler için açılır.)"],["london eye","The Eye gives a lovely view. (London Eye'dan manzara harika.)"]]],["Rainy commuter","Typical, it's raining again. (Yine yağmur.)",[["rain|yağmur","It rains every other day. (Gün aşırı yağar.)"],["tea|çay","A cuppa fixes everything. (Bir fincan çay her şeyi düzeltir.)"]]]],f:["Cheers, mate! (Sağ ol dostum!)","Lovely weather for ducks. (Ördekler için harika hava.)"],sh:[0x2c3e50,0x34495e,0x7a3b2e,0x1f4aa8,0xecf0f1],hat:[0x222222]},
'Manchester':{r:[["Football fan","Come on United! (Haydi United!)",[["united|football|maç","Old Trafford on matchday is unreal. (Maç günü Old Trafford inanılmaz.)"],["city|derby","Derby day, ay? (Derbi günü!)"]]],["Mill historian","Mills made this city. (Bu şehri fabrikalar yaptı.)",[["mill|cotton|fabrika","Cotton built Manchester. (Pamuk Manchester'ı kurdu.)"],["canal|kanal","The canals carried goods. (Kanallar mal taşırdı.)"]]],["Musician","Manchester has the best bands. (En iyi gruplar burada.)",[["band|music|müzik","Music runs in our veins. (Müzik damarlarımızda.)"],["rain|yağmur","It always rains, we're used to it. (Hep yağar, alıştık.)"]]]],f:["Eh up! (Selam!)","It's grim up north, but we love it. (Kuzey sert ama severiz.)"],sh:[0xda291c,0x34495e,0x7f8c8d,0xffffff],hat:[0x222222]},
'Bristol':{r:[["Balloonist","Fiesta season! (Balon festivali!)",[["balloon|balon","Bristol hosts the big balloon fiesta. (Büyük balon festivali burada.)"],["sky|gök","The view's amazing up there. (Orada manzara muhteşem.)"]]],["Street artist","Banksy was from here! (Banksy buralı!)",[["banksy|art|sanat","Graffiti is everywhere, it's art. (Her yerde grafiti, bu sanat.)"],["colour|renk","The colourful houses are famous. (Renkli evler ünlü.)"]]],["Sailor","Harbour's calm today. (Liman sakin.)",[["ship|gemi|brunel","The SS Great Britain is a legend. (SS Great Britain bir efsane.)"],["bridge|köprü","Clifton Bridge looks stunning. (Clifton Köprüsü harika.)"]]]],f:["Alright, my lover? (Nasılsın, tatlım? — Bristol usulü!)","Gert lush, innit. (Çok güzel!)"],sh:[0xf4a6a6,0xa6c8f4,0xf4e0a6,0xa6f4c8,0xe9c46a],hat:[0x3498db]},
'Madrid':{r:[["Tapas waiter","¡Hola! ¿Una mesa? (Masa mı?)",[["tapas|comer|yemek","Prueba las patatas bravas. (Patatas bravas dene.)"],["vino|wine","Un vino tinto, claro. (Tabii ki kırmızı şarap.)"]]],["Real Madrid fan","¡Hala Madrid! (Haydi Madrid!)",[["real|madrid|fútbol|maç","El Bernabéu es un templo. (Bernabéu bir tapınak.)"],["barça|barcelona","El Clásico es guerra. (El Clásico savaştır.)"]]],["Flamenco dancer","¡Olé! Vamos a bailar. (Dans edelim.)",[["flamenco|baile|dans","El flamenco es pasión. (Flamenko tutkudur.)"],["guitarra|gitar","La guitarra es el alma. (Gitar ruhtur.)"]]],["Siesta lover","Es hora de la siesta... (Siesta zamanı...)",[["siesta|dormir|uyku","Después de comer, siesta sagrada. (Yemekten sonra kutsal siesta.)"]]]],f:["¡Qué calor! (Ne sıcak!)","Mañana, mañana... (Yarın, yarın...)"],sh:[0xc0392b,0xf1c40f,0xecf0f1,0x2c3e50],hat:[0x222222]},
'Barcelona':{r:[["Gaudí guide","La Sagrada Familia es única. (Sagrada Familia eşsiz.)",[["gaudí|sagrada|katedral","Lleva más de 140 años en obras. (140 yıldır inşaatta.)"],["parque|güell|park","El Park Güell es de mosaico. (Park Güell mozaik.)"]]],["Beach vendor","¡Cerveza fría, agua! (Soğuk bira, su!)",[["playa|beach|plaj","La playa de Barceloneta es famosa. (Barceloneta meşhur.)"],["agua|su","¡Con este sol necesitas agua! (Bu güneşte su lazım!)"]]],["Barça fan","¡Visca el Barça! (Yaşasın Barça!)",[["barça|messi|camp nou|fútbol","Camp Nou es nuestra casa. (Camp Nou evimiz.)"],["madrid|real","Madrid? ¡Nunca! (Madrid mi? Asla!)"]]]],f:["¡Hace mucho sol! (Çok güneşli!)","Vamos a la playa. (Plaja gidelim.)"],sh:[0xa50044,0x004d98,0xf1c40f,0xffffff],hat:[0xffffff]}};
const GENL={en:[[/how are you|nasılsın|how's it going/,["I'm doing great, thanks! How about you? (İyiyim, sen?)","Can't complain! (Fena değil!)"]],[/name|adın|kimsin|who are you/,d=>"I'm "+d.n+", a local "+d.r.toLowerCase()+". (Ben "+d.n+".)"],[/thank|teşekkür|sağ ol/,["You're welcome! (Rica ederim!)","No problem! (Sorun değil!)"]],[/bye|goodbye|görüşürüz|hoşça/,["See you around! (Görüşürüz!)","Take care! (Kendine iyi bak!)"]],[/weather|hava/,["Nice day, isn't it? (Güzel bir gün, değil mi?)"]],[/turk|istanbul|türkiye/,["Turkey? I'd love to visit Istanbul one day! (İstanbul'u görmek isterim!)","Turkish tea is amazing! (Türk çayı harika!)"]],[/food|eat|yemek|hungry|restaurant/,["Try the local food, it's great! (Yerel yemekleri dene!)"]],[/help|yardım|where|nerede/,["Use the map on your phone, it's handy! (Telefondaki harita işe yarar!)"]],[/money|para|price|fiyat/,["Prices are high, but it's worth it. (Fiyatlar yüksek ama değer.)"]]],
es:[[/cómo estás|qué tal|nasılsın|how are you/,["¡Muy bien, gracias! ¿Y tú? (Çok iyiyim, sen?)","Todo bien, hombre. (Her şey yolunda.)"]],[/nombre|adın|kimsin|who are you/,d=>"Soy "+d.n+", "+d.r.toLowerCase()+" local. (Ben "+d.n+".)"],[/gracias|thank|teşekkür|sağ ol/,["¡De nada! (Rica ederim!)","Un placer. (Memnuniyetle.)"]],[/adiós|hasta|bye|görüş/,["¡Hasta luego! (Görüşürüz!)","Cuídate. (Kendine iyi bak.)"]],[/tiempo|calor|hava|weather/,["¡Hace mucho sol! (Çok güneşli!)"]],[/turqu|türk|estambul|istanbul/,["¡Estambul es preciosa! (İstanbul çok güzel!)","Me encanta el café turco. (Türk kahvesini severim.)"]],[/comer|tapas|paella|yemek|food/,["Prueba la paella, ¡deliciosa! (Paella dene, enfes!)"]],[/ayuda|yardım|dónde|nerede|where/,["Usa el mapa del móvil. (Telefondaki haritayı kullan.)"]],[/dinero|para|precio|fiyat/,["Los precios son altos, pero vale la pena. (Fiyatlar yüksek ama değer.)"]]]};
function roofG(x,z,w,d,h,c){const r=(w+.8)/Math.sqrt(3),m=new T.Mesh(new T.CylinderGeometry(r,r,d+1,3),MT(c));m.rotation.x=-PI/2;m.scale.z=.4;m.position.set(x,h+r*.2,z);scene.add(m)}
const TG={o:[[new T.CylinderGeometry(.2,.25,2,6),MT(0x6b4a2a),1],[new T.SphereGeometry(1.8,8,6),MT(0x2f7d32),3.4]],p:[[new T.CylinderGeometry(.15,.3,7,6),MT(0x8a6a3a),3.5],[new T.ConeGeometry(3.2,1.8,8),MT(0x2f9d4f),7.6]],c:[[new T.CylinderGeometry(.2,.25,2,6),MT(0x6b4a2a),1],[new T.SphereGeometry(2.2,8,6),MT(0xf4a6c0),3.6]]};
function inst(list,t){if(!list.length)return;const a=new T.InstancedMesh(t[0][0],t[0][1],list.length),b=new T.InstancedMesh(t[1][0],t[1][1],list.length),o=new T.Object3D();a.frustumCulled=b.frustumCulled=false;list.forEach((p,i)=>{o.position.set(p[0],t[0][2],p[1]);o.updateMatrix();a.setMatrixAt(i,o.matrix);o.position.y=t[1][2];o.updateMatrix();b.setMatrixAt(i,o.matrix)});scene.add(a,b)}
function beachAt(cx,x0,x1){const w=x1-x0;flat(w,32,0xe6d5a8,cx+(x0+x1)/2,.045,218);const n=Math.floor(w/6),um=new T.InstancedMesh(new T.ConeGeometry(1.8,.7,10),new T.MeshLambertMaterial({color:0xffffff}),n),pl_=new T.InstancedMesh(new T.CylinderGeometry(.05,.05,2.4,5),MT(0xeeeeee),n),ob=new T.Object3D(),c3=new T.Color();um.frustumCulled=pl_.frustumCulled=false;
for(let i=0;i<n;i++){const x=cx+rnd(x0+8,x1-8),z=rnd(208,230);ob.position.set(x,2.4,z);ob.updateMatrix();um.setMatrixAt(i,ob.matrix);um.setColorAt(i,c3.setHex(pick([0xe74c3c,0xf1c40f,0x3498db,0xffffff,0x2ecc71])));ob.position.set(x,1.2,z);ob.updateMatrix();pl_.setMatrixAt(i,ob.matrix)}scene.add(um,pl_)}
function colC(p,k){const C=CTY[k],lx=p.x-C.cx;p.x=C.cx+cl(lx,-290,290);p.z=cl(p.z,-290,C.beach&&lx>C.beach[0]&&lx<C.beach[1]?236:230);
for(const b of C.W){const x0=C.cx+b[0],x1=C.cx+b[1];if(p.x>x0&&p.x<x1&&p.z>b[2]&&p.z<b[3]&&!(b[4]||[]).some(q=>Math.abs(p.x-(C.cx+q))<8.2)){const d=[p.z-b[2],b[3]-p.z,p.x-x0,x1-p.x],m=Math.min(...d);if(m==d[0])p.z=b[2];else if(m==d[1])p.z=b[3];else if(m==d[2])p.x=x0;else p.x=x1}}}
function buildCountry(k){const C=CTY[k],cx=C.cx,n0=scene.children.length,TL={o:[],p:[],c:[]};curFlag=C.fm;
const B=(w,h,d,c,x,y,z)=>box(w,h,d,c,x+cx,y,z),Cy=(a,b,h,c,x,y,z,s)=>cyl(a,b,h,c,x+cx,y,z,s),Fl=(w,d,c,x,y,z,m)=>flat(w,d,c,x+cx,y,z,m),Lb=(t,w,h,fs,x,y,z)=>{const q=lab(t,w,h,fs);q.position.set(x+cx,y,z);scene.add(q)},FP=(x,z,h)=>flagPole(x+cx,0,z,h),BD=(x,z,w,d,h,c)=>bld(x+cx,z,w,d,h,c),DM=(x,y,z,r,c)=>{const m=new T.Mesh(new T.SphereGeometry(r,16,10,0,PI*2,0,PI/2),MT(c));m.position.set(x+cx,y,z);scene.add(m);return m},BB=(w,h,d,c,x,y,z)=>{const m=new T.Mesh(BG(w,h,d),new T.MeshBasicMaterial({color:c}));m.position.set(x+cx,y,z);scene.add(m)};
flat(800,640,0,cx,0,-90,C.gm);flat(800,200,0,cx,-.3,330,SEAM);
C.HR.forEach(z=>{flat(500,12,0,cx-45,.05,z,RDH);flat(500,2,0,cx-45,.055,z-7,PVM);flat(500,2,0,cx-45,.055,z+7,PVM)});
C.VR.forEach(x=>{flat(12,520,0,x,.06,-35,RDV);flat(2,520,0,x-7,.065,-35,PVM);flat(2,520,0,x+7,.065,-35,PVM)});
C.W.forEach(b=>{const z0=b[2],z1=b[3];flat(b[1]-b[0],z1-z0,0,cx+(b[0]+b[1])/2,.03,(z0+z1)/2,WTM);(b[4]||[]).forEach(x=>{B(16,.7,z1-z0+6,0x8e8e8e,x,-.3,(z0+z1)/2);[-8,8].forEach(sx=>B(.25,1.1,z1-z0+6,0xbbbbbb,x+sx,.6,(z0+z1)/2))})});
const lpp=[];C.HR.forEach(z=>{for(let x=-280;x<=200;x+=40)lpp.push([cx+x,z+(x%80?7.5:-7.5)])});C.VR.forEach(x=>{for(let z=-280;z<=220;z+=40)lpp.push([x+(z%80?7.5:-7.5),z])});
{const a=new T.InstancedMesh(new T.CylinderGeometry(.1,.12,6,5),MT(0x444444),lpp.length),b=new T.InstancedMesh(BG(.7,.3,.7),lampMat,lpp.length),o=new T.Object3D();a.frustumCulled=b.frustumCulled=false;lpp.forEach((p,i)=>{o.position.set(p[0],3,p[1]);o.updateMatrix();a.setMatrixAt(i,o.matrix);o.position.set(p[0],6.1,p[1]);o.updateMatrix();b.setMatrixAt(i,o.matrix)});scene.add(a,b)}
const rof=(x,z)=>{let R=C.regs[0],bd=1e9;C.regs.forEach(r=>{const d=Math.hypot(x-r.x,z-r.z);if(d<bd){bd=d;R=r}});return R},nrc=(x,z,m)=>C.HR.some(r=>Math.abs(z-r)<6+m)||C.VR.some(r=>Math.abs(x-r)<6+m),inW=(x,z,m)=>C.W.some(b=>x-cx>b[0]-m&&x-cx<b[1]+m&&z>b[2]-m&&z<b[3]+m);
for(let gx=-270;gx<=200;gx+=24)for(let gz=-280;gz<=190;gz+=24){const x=cx+gx+rnd(-3,3),z=gz+rnd(-3,3);if(C.zones.some(q=>Math.hypot(x-cx-q[0],z-q[1])<q[2])||inW(x,z,16)||Math.random()<.1)continue;
const w=rnd(10,15),d=rnd(10,15);if(nrc(x,z,Math.max(w,d)/2+1))continue;const R=rof(x,z),h=rnd(R.h[0],R.h[1]),c=pick(R.pal);bld(x,z,w,d,h,c);
if(R.up&&h>45){const u=h*.12;box(w*.6,u,d*.6,c,x,h+u/2,z);box(w*.3,u*1.2,d*.3,c,x,h+u*1.7,z)}
if(R.roof)roofG(x,z,w,d,h,pick(R.rc));if(Math.random()<R.tp)TL[R.tk].push([x+w/2+3,z+d/2+3])}
C.HR.forEach(z=>{for(let x=-270;x<200;x+=30)TL[C.tk].push([cx+x,z+9.5])});C.VR.forEach(x=>{for(let z=-270;z<220;z+=30)if(!inW(x,z,2))TL[C.tk].push([x+9.5,z])});
if(k==1){
BD(-10,-110,22,22,52,0xb8c4d0);B(15,22,15,0xb8c4d0,-10,63,-110);B(8,14,8,0xb8c4d0,-10,81,-110);Cy(.2,.8,26,0xcccccc,-10,101,-110,6);Lb('EMPIRE STATE',14,2.4,52,-10,118,-110);FP(5,-98,12);
for(let i=0;i<12;i++)BB(5,6,.4,pick(NEON),i%2?30:18,9+(i%3)*7,-160+i*5);Lb('TIMES SQUARE',16,3,56,24,26,-130);
Fl(60,100,0x4f8a3a,100,.04,-108);Fl(16,16,0x2a6f9e,100,.045,-108);for(let i=0;i<40;i++)TL.o.push([cx+rnd(72,128),rnd(-156,-60)]);Lb('CENTRAL PARK',14,2.4,50,100,12,-108);
Fl(24,24,0xb9a36a,200,.05,-238);B(7,6,7,0xaaaaaa,200,3,-238);Cy(2.4,1.6,16,0x6fb59b,200,14,-238,8);Cy(1.2,1.2,2,0x6fb59b,200,23,-238,8);Cy(.9,.9,7,0x6fb59b,201.5,25,-238,6);{const t=new T.Mesh(new T.SphereGeometry(.9,8,6),new T.MeshBasicMaterial({color:0xffd24a}));t.position.set(cx+201.5,29.5,-238);scene.add(t)}Lb('Özgürlük Heykeli',12,2.2,46,200,36,-238);
B(46,10,18,0xf4f1ea,-130,5,75);Cy(6,6,5,0xf4f1ea,-130,12.5,75,16);DM(-130,15,75,7.5,0xf8f8f4);Cy(.5,.5,4,0xffffff,-130,23,75,6);for(let i=0;i<9;i++)Cy(.6,.6,9,0xffffff,-146+i*4,5,84.8,8);cols.push({x:cx-130,z:75,hw:23,hd:9});FP(-105,64,14);Lb('KONGRE BİNASI',12,2,44,-130,27,75);
B(30,9,14,0xf5f5f0,-10,4.5,75);B(30.4,.6,14.4,0xdddddd,-10,9.3,75);for(let i=0;i<6;i++)Cy(.5,.5,8,0xffffff,-20+i*4,4.5,82.8,8);cols.push({x:cx-10,z:75,hw:15,hd:7});FP(8,70,12);Lb('BEYAZ SARAY',12,2,46,-10,14,75);
Cy(.9,2.6,55,0xf2f2f2,102,27.5,75,4);Cy(0,1.4,6,0xf2f2f2,102,58,75,4);cols.push({x:cx+102,z:75,r:2.8});Fl(12,40,0x3a7fb5,125,.07,75);Lb('WASHINGTON ANITI',14,2,44,102,66,75);for(let i=0;i<24;i++)TL.c.push([cx+rnd(108,142),rnd(52,98)]);
[-46,-22,2,26,70,94,118].forEach(x=>{const h=rnd(9,16);BD(x,176,16,11,h,pick([0xf8b6c8,0x9be3d6,0xfff0a8,0xffc9a0,0xb6d4ff]));B(16.4,.6,11.4,0xffffff,x,h+.3,176);BB(7,1.4,.2,pick(NEON),x,h-2.5,182)});Lb('OCEAN DRIVE',14,2.4,52,36,6,170);
Cy(2.6,2.6,40,0xf2f2f2,-130,20,150,12);Cy(0,2.6,8,0xf2f2f2,-130,44,150,12);[-3.6,3.6].forEach(dx=>{Cy(1,1,22,0xdddddd,-130+dx,11,150,8);Cy(0,1,4,0xcc3322,-130+dx,24,150,8)});B(3,46,3,0xbb4433,-120,23,150);cols.push({x:cx-130,z:150,r:4.5});Lb('NASA · Kennedy Uzay Merkezi',20,2.6,36,-130,52,150);
beachAt(cx,-250,120);for(let x=-250;x<120;x+=14)TL.p.push([cx+x,204]);for(let i=0;i<5;i++)FP(-230+i*70,208,9)}
if(k==2){
B(7,48,7,0xc9b48a,50,24,72);[[3.6,0],[-3.6,0],[0,3.6],[0,-3.6]].forEach(q=>BB(q[0]?.2:4,4,q[1]?.2:4,0xfff2c0,50+q[0],40,72+q[1]));Cy(0,5.6,12,0x3a6b5a,50,54,72,4);cols.push({x:cx+50,z:72,r:4.2});Lb('BIG BEN',8,2,56,50,64,72);
B(40,13,12,0xc9b48a,26,6.5,82);B(41,1,13,0x8a7a5a,26,13.5,82);[-14,0,14].forEach(q=>Cy(.9,1.5,8,0xc9b48a,26+q,17,82,6));cols.push({x:cx+26,z:82,hw:20,hd:6});
{const g=new T.Group();g.position.set(cx+105,28,72);g.add(new T.Mesh(new T.TorusGeometry(22,.6,6,36),MT(0xf5f5f5)));for(let i=0;i<8;i++){const sp=new T.Mesh(BG(44,.2,.2),MT(0xdddddd));sp.rotation.z=i*PI/8;g.add(sp)}for(let i=0;i<16;i++){const a=i/16*PI*2,cp=new T.Mesh(BG(2,2,2),MT(0xeaf6ff));cp.position.set(Math.cos(a)*22,Math.sin(a)*22,0);g.add(cp)}scene.add(g);SPIN.push({g,s:.06});B(1.2,30,1.2,0xe0e0e0,98,14,66);B(1.2,30,1.2,0xe0e0e0,112,14,66);Lb('LONDON EYE',10,2,52,105,56,72)}
[96,106].forEach(z=>[-9.5,9.5].forEach(sx=>{B(5,30,5,0xa9a9a9,150+sx,15,z);Cy(0,3.2,6,0x4a6fb5,150+sx,33,z,4);cols.push({x:cx+150+sx,z,r:3.5})}));B(24,1.6,2.4,0x2a5fb5,150,26,96);B(24,1.6,2.4,0x2a5fb5,150,26,106);B(.8,1,28,0x2a5fb5,142,1,101);B(.8,1,28,0x2a5fb5,158,1,101);Lb('TOWER BRIDGE',14,2.4,48,150,38,101);
[92,110].forEach(z=>[-9,9].forEach(sx=>{B(3.4,24,3.4,0xb9a98a,-140+sx,12,z);cols.push({x:cx-140+sx,z,r:2.4})}));[-9,9].forEach(sx=>{const cv=new T.QuadraticBezierCurve3(new T.Vector3(cx-140+sx,24,92),new T.Vector3(cx-140+sx,-4,101),new T.Vector3(cx-140+sx,24,110));scene.add(new T.Mesh(new T.TubeGeometry(cv,20,.22,6),MT(0xdddddd)));for(let i=1;i<10;i++){const pt=cv.getPoint(i/10);box(.1,pt.y,.1,0xcccccc,pt.x,pt.y/2,pt.z)}});Lb('CLIFTON KÖPRÜSÜ',14,2.4,44,-140,30,101);
for(let i=0;i<3;i++){const g=new T.Group(),sp=new T.Mesh(new T.SphereGeometry(5,12,10),MT(pick([0xe74c3c,0xf1c40f,0x3498db])));sp.position.y=8;g.add(sp,new T.Mesh(BG(1.4,1,1.4),MT(0x8a6a3a)));g.position.set(cx-170+i*30,30+i*8,60+i*15);scene.add(g);BAL.push(g)}
B(26,3.5,7,0x1a1a1a,-215,.9,101);[-8,0,8].forEach(dx=>{Cy(.3,.3,16,0x6b4a2a,-215+dx,10,101,6);B(.2,9,5,0xf0f0f0,-215+dx,12,101)});Lb('SS Great Britain',12,2.2,44,-215,19,101);
[[-100,-60],[-80,-52],[-60,-58],[-110,-30]].forEach(q=>Cy(.9,1.5,36,0x8a3a2a,q[0],18,q[1],10));
mkStd({id:2,name:'Old Trafford',home:'Manchester United',cx:cx-70,cz:-160,R:40,off:60,fx:[['Liverpool',40000],['Chelsea',30000],['Arsenal',32000],['Manchester City',45000],['Galatasaray',38000,1],['Real Madrid',55000,1]]});
{const pb=new T.InstancedMesh(BG(1,2.3,1),MT(0xcc1a1a),18),o=new T.Object3D();pb.frustumCulled=false;for(let i=0;i<18;i++){if(i%2)o.position.set(cx+rnd(-250,190),1.15,pick(C.HR)+8.8);else o.position.set(pick(C.VR)+8.8,1.15,rnd(-250,70));o.updateMatrix();pb.setMatrixAt(i,o.matrix)}scene.add(pb)}
for(let i=0;i<4;i++)FP(-230+i*100,-80,10)}
if(k==3){
Fl(62,62,0xc9b79c,-135,.045,-70);BD(-135,-95,62,10,16,0xc97b5a);BD(-163,-45,26,10,16,0xc97b5a);BD(-107,-45,26,10,16,0xc97b5a);BD(-165,-70,10,48,16,0xc97b5a);BD(-105,-70,10,48,16,0xc97b5a);B(2.4,2,2.4,0xcfcfcf,-135,1,-70);{const sp=person(0x6b5a3a,0x6b5a3a,0x8a7a58,0x4a3a2a,false);sp.scale.setScalar(1.3);sp.position.set(cx-135,2,-70);scene.add(sp)}Lb('PLAZA MAYOR',12,2,48,-135,22,-95);
BD(-135,-170,70,22,18,0xf2ead6);B(70.6,1.2,22.6,0xffffff,-135,18.6,-170);for(let i=0;i<14;i++)Cy(.5,.5,12,0xffffff,-167+i*5,6,-158.6,8);FP(-165,-182,14);FP(-105,-182,14);Lb('KRALİYET SARAYI',14,2.4,44,-135,24,-170);
Cy(9,9,.8,0xdddddd,-35,.4,-70,24);Cy(8,8,.85,0x3a7fb5,-35,.45,-70,24);Cy(2.6,3,4,0xeeeeee,-35,2,-70,12);Cy(1.2,1.2,3,0xdddddd,-35,5.5,-70,8);{const cs=new T.Mesh(new T.SphereGeometry(1,8,6),MT(0xdddddd));cs.position.set(cx-35,7.6,-70);scene.add(cs)}cols.push({x:cx-35,z:-70,r:9.3});Lb('Cibeles',8,1.8,48,-35,11,-70);
[-6,6].forEach(sx=>{B(3.2,11,4,0xd8cfc0,-35+sx,5.5,-140);cols.push({x:cx-35+sx,z:-140,r:2})});B(15.4,3,4,0xd8cfc0,-35,12.5,-140);[-9.5,9.5].forEach(sx=>B(3,7,4,0xd8cfc0,-35+sx,3.5,-140));Lb('Puerta de Alcalá',12,2,44,-35,17,-140);
mkStd({id:3,name:'Santiago Bernabéu',home:'Real Madrid',cx:cx+55,cz:-180,R:34,off:30,fx:[['Barcelona',60000],['Atlético Madrid',30000],['Sevilla',12000],['Valencia',10000],['Bayern München',55000,1],['Galatasaray',40000,1]]});
mkStd({id:4,name:'Camp Nou',home:'Barcelona',cx:cx+145,cz:-170,R:34,off:90,fx:[['Real Madrid',60000],['Sevilla',11000],['Valencia',9000],['Atlético Madrid',28000],['Galatasaray',35000,1],['Manchester City',50000,1]]});
BD(55,30,24,56,20,0xd9c9a8);[-9,-3,3,9].forEach(dx=>[-22,22].forEach(dz=>{Cy(1.5,2.2,46,0xd9c9a8,55+dx,23,30+dz,8);Cy(0,1.5,8,0xcdb88a,55+dx,50,30+dz,8)}));Cy(2.6,3.4,70,0xd9c9a8,55,35,30,10);Cy(0,2.6,10,0xcdb88a,55,75,30,10);B(.5,5,.5,0xffd24a,55,82,30);B(2.6,.5,.5,0xffd24a,55,82.5,30);B(.8,60,.8,0xf1c40f,82,30,10);B(36,.8,.8,0xf1c40f,70,60,10);cols.push({x:cx+55,z:30,hw:12,hd:28});Lb('SAGRADA FAMILIA',16,2.4,40,55,90,30);
Cy(7,7,38,0x3a6fb0,145,19,30,20);{const tp=DM(145,38,30,7,0x3a6fb0);tp.scale.y=1.4}cols.push({x:cx+145,z:30,r:7.5});Lb('Torre Glòries',10,2,48,145,52,30);
Fl(50,40,0xc9b79c,145,.05,115);for(let i=0;i<16;i++)B(3,1,1.2,pick([0xe74c3c,0xf1c40f,0x2ecc71,0x3498db,0xffffff,0xe67e22]),121+i*3.2,.6+Math.sin(i*.6)*.4,128);B(8,6,8,0xe8c7a0,165,3,108);Cy(0,5.5,5,0xd9534f,165,8.5,108,8);Lb('Park Güell',10,2,48,145,12,115);
beachAt(cx,-50,170);for(let x=-50;x<170;x+=12)TL.p.push([cx+x,204]);for(let i=0;i<4;i++)FP(-215+i*80,-90,10)}
inst(TL.o,TG.o);inst(TL.p,TG.p);inst(TL.c,TG.c);
atm(cx-20,C.HR[1]+9);atm(cx+60,C.HR[2]-9);
{const ax=C.ap.x+12,az=C.ap.z;B(14,10,50,0xd8dde2,ax,5,az);B(14.2,3,50.2,0x335577,ax,6.5,az);cols.push({x:cx+ax,z:az,hw:7,hd:25});Fl(16,150,0x2f3236,ax+40,.05,az);for(let q=-65;q<70;q+=14)Fl(.6,6,0xeeeeee,ax+40,.06,az+q);Fl(20,100,0x4a4e52,ax+22,.045,az);const a=mkPlane(),b=mkPlane();a.g.position.set(cx+ax+22,2.4,az-25);a.g.rotation.y=PI;b.g.position.set(cx+ax+22,2.4,az+25);Lb(C.apn,22,4.3,36,ax,14,az);FP(ax-9,az-30,12)}
C.regs.forEach(r=>{const pk=RG[r.n];for(let i=0;i<8;i++){let x,z,t=0;do{t++;if(Math.random()<.5){z=pick(C.HR)+pick([-9,9]);x=cx+rnd(-270,200)}else{x=pick(C.VR)+pick([-9,9]);z=rnd(-270,200)}}while(t<40&&(rof(x,z)!==r||inW(x,z,3)));const ro=pick(pk.r);mkNpc({n:pick(C.names),r:ro[0],hi:ro[1],t:ro[2],f:pk.f,fl:C.lang},x,z,pick(pk.sh),0,pk.hat&&pk.hat.length&&Math.random()<.4?pick(pk.hat):0,pk.pn)}});
for(let i=0;i<12;i++){const hz=i%2==0,r=pick(hz?C.HR:C.VR),dir=pick([1,-1]),tx=i<3&&C.taxi,c=spawnCar(hz?cx+rnd(-250,150):r,hz?r:rnd(-250,70),0,tx?C.taxi:pick(C.cc),{ax:hz?'x':'z',r,dir,sp:rnd(7,11),o:cx-45,b:250,ld:C.ld});c.x=hz?c.x:r-dir*3*C.ld;c.z=hz?r+dir*3*C.ld:c.z;c.taxi=!!tx}
for(let j=0;j<2;j++){const hz=j==0,r=pick(hz?C.HR:C.VR),dir=hz?1:-1,c={g:mkBus(C.bus),x:hz?cx+rnd(-200,100):r-dir*3*C.ld,z:hz?r+dir*3*C.ld:rnd(-200,60),a:0,v:0,ai:{ax:hz?'x':'z',r,dir,sp:8,o:cx-45,b:250,ld:C.ld},bus:1};CARS.push(c);sh(c.g)}
for(let i=n0;i<scene.children.length;i++)sh(scene.children[i])}
const RAINP=new T.Points((()=>{const g=new T.BufferGeometry(),a=new Float32Array(4500);for(let i=0;i<1500;i++){a[i*3]=rnd(-40,40);a[i*3+1]=rnd(0,30);a[i*3+2]=rnd(-40,40)}g.setAttribute('position',new T.BufferAttribute(a,3));return g})(),new T.PointsMaterial({color:0xaac4e8,size:1.4,sizeAttenuation:false,transparent:true,opacity:.5,fog:false,depthWrite:false}));RAINP.frustumCulled=false;RAINP.visible=false;scene.add(RAINP);
// uçuş
let FPL=null,CLD=null;const CLM=new T.MeshBasicMaterial({color:0xffffff,transparent:true,opacity:.9,fog:false});
function ensureC(k){const C=CTY[k];if(k&&!C.built){C.built=1;buildCountry(k)}}
function openUcus(){app='ucus';openPhone()}
function startFlight(k1){const pp=car||mp,ap=AIRP.find(a=>Math.hypot(a.x-pp.x,a.z-pp.z)<70);if(!ap)return toast('Havalimanında değilsin');const f=FLT(ap.k,k1);if(!f)return;if(S.money<f[1])return toast('Yetersiz nakit');S.money-=f[1];closePhone();if(car)exitCar();if(inside)exitHouse();if(watch)exitStd();
$('fade').style.opacity=1;setTimeout(()=>{ensureC(k1);if(!FPL){FPL=mkPlane();CLD=new T.Group();for(let i=0;i<70;i++){const m=new T.Mesh(new T.SphereGeometry(1,8,6),CLM);m.scale.set(rnd(18,40),rnd(5,10),rnd(18,40));m.position.set(rnd(-350,350),rnd(-60,-30),rnd(-300,300));CLD.add(m)}scene.add(CLD)}CLD.position.set(20000,900,0);flight={k0:ap.k,k1,t:0,dur:f[0]};me.visible=false;$('fade').style.opacity=0;toast('✈️ İyi uçuşlar!')},650)}
function fcam(dt){const f=flight;f.t+=dt;const px=20000,py=900,a=f.t*.12+.6;FPL.g.position.set(px,py,0);FPL.g.rotation.set(0,0,Math.sin(f.t*.4)*.06);FPL.ls.visible=Math.floor(f.t*1.3)%2==0;
cam.position.set(px+Math.sin(a)*48,py+8+Math.sin(f.t*.3)*4,Math.cos(a)*48);cam.lookAt(px,py,0);CLM.color.setScalar(.25+.75*LF);CLD.children.forEach(m=>{m.position.z-=95*dt;if(m.position.z<-320)m.position.z+=640});if(f.t>=f.dur)endFlight()}
function endFlight(){const k=flight.k1,ap=AIRP[k];flight=null;me.visible=true;$('fade').style.opacity=1;setTimeout(()=>{mp.set(ap.x-4,0,ap.z);col(mp,.4);yaw=PI/2;$('fade').style.opacity=0;toast(CTY[k].fl+' Hoş geldin: '+CTY[k].nm)},500)}
// ---------- ÇOK OYUNCULU ----------
const MP={rm:null,sync:false,name:'Oyuncu'+Math.floor(rnd(100,999)),col:pick([0xe74c3c,0x3498db,0x2ecc71,0xf1c40f,0x9b59b6,0xe67e22]),chat:[],pl:{},lt:0,st:'Tek oyunculu'};
async function mpConnect(code){try{if(!window.claude||!window.claude.use){MP.st='Tek oyunculu (web)';return}const r=await window.claude.use('room');if(!r){MP.st='Tek oyunculu (çevrimiçi oda yok)';return}let rm=r;if(code){try{rm=await r.join(code)}catch(e){toast('Oda açılamadı, lobiye bağlanıldı')}}
MP.rm=rm;MP.sync=true;gt=Date.now()/1000;S.day=Math.floor(gt/300);MP.st=code?'Oda: '+code:'Lobi';
rm.on('chat',m=>{const d=m.data||{},n=String(d.n||'?').slice(0,14),t=String(d.t||'').slice(0,120);MP.chat.push({n,t,me:m.isMe});if(MP.chat.length>30)MP.chat.shift();if(!m.isMe)toast('💬 '+n+': '+t.slice(0,60));if(phoneOpen&&app=='online')render()},()=>{});
rm.onPeers(()=>{if(phoneOpen&&app=='online')render()},()=>{});toast('🌐 Çok oyunculu bağlandı')}catch(e){MP.st='Bağlanamadı'}}
function mpUpdate(now,dt,pp){const rm=MP.rm;if(!rm)return;
if(now-MP.lt>100){MP.lt=now;rm.presence({n:MP.name,x:Math.round(pp.x*10)/10,z:Math.round(pp.z*10)/10,a:Math.round((car?car.a:me.rotation.y)*100)/100,c:car?1:0,w:Math.round(spd*10)/10,h:MP.col,i:inside||watch||flight?1:0}).catch(()=>{})}
const seen={};for(const q of rm.peers()){if(q.isMe||q.kind!='viewer')continue;const p=q.presence;if(p.x==null)continue;seen[q.peer]=1;let r=MP.pl[q.peer];
if(!r){const g=person(p.h||0x3498db,0x23232f,pick(SK),pick(HC),false);scene.add(g);const lb=lab(String(p.n||'Oyuncu').slice(0,14),3.4,.85,54);lb.position.y=2.3;g.add(lb);g.position.set(p.x,0,p.z);r={g,lb,nm:p.n,cr:null,x:p.x,z:p.z,ph:0};MP.pl[q.peer]=r}
if(p.n!==r.nm){r.nm=p.n;r.g.remove(r.lb);r.lb=lab(String(p.n||'Oyuncu').slice(0,14),3.4,.85,54);r.lb.position.y=2.3;r.g.add(r.lb)}
r.x=p.x;r.z=p.z;const g=r.g,dx=p.x-g.position.x,dz=p.z-g.position.z;if(Math.hypot(dx,dz)>30){g.position.x=p.x;g.position.z=p.z}else{g.position.x+=dx*Math.min(1,dt*8);g.position.z+=dz*Math.min(1,dt*8)}
const vis=!p.i&&Math.hypot(g.position.x-pp.x,g.position.z-pp.z)<160;
if(p.c){if(!r.cr){r.cr=mkCar(p.h||0x3498db);sh(r.cr)}r.cr.visible=vis;r.cr.position.set(g.position.x,0,g.position.z);r.cr.rotation.y=p.a||0;g.visible=false}
else{if(r.cr)r.cr.visible=false;g.visible=vis;g.rotation.y+=((((p.a||0)-g.rotation.y+PI)%(2*PI)+2*PI)%(2*PI)-PI)*Math.min(1,dt*8);r.ph+=(p.w||0)*dt*2.6;anim(g,r.ph,Math.min(.9,(p.w||0)*.22))}}
for(const id in MP.pl)if(!seen[id]){scene.remove(MP.pl[id].g);if(MP.pl[id].cr)scene.remove(MP.pl[id].cr);delete MP.pl[id]}}
// ---------- WhatsApp & harita ----------
const PAR={anne:{d:{n:'Anne',r:'Annen',hi:'Canım evladım, nasılsın? Yemek yedin mi?',t:[['yemek|aç','Sana mantı yaptım, gel de ye.'],['ev|taşın','Evinize gelirim, çay demlerim.'],['evlen|eş|torun','Torun sevmek istiyorum, belki bir gün...']],f:['Allah korusun, kendine iyi bak.','Seni çok özledim.','Hep dua ediyorum sana.']}},baba:{d:{n:'Baba',r:'Baban',hi:'Evladım, işler nasıl? Para lazım mı?',t:[['para|borç','Gerekirse söyle, babana güven.'],['maç|galatasaray|fener','Hafta sonu maça gidelim mi?'],['iş|meslek','Çalış, dürüst ol, gerisi gelir.']],f:['Aferin evladım.','Kendine dikkat et, trafikte yavaş git.','Annen seni sordu.']}}};
let wk='';const WMT=["Nasılsın?","Akşam yemeğe gelir misin?","Bugün hava çok güzel!","Maç izledin mi?","Sana bir şey anlatacağım!"];
const waAll=()=>{const a=[{k:'anne',l:'👩 Anne',o:PAR.anne,f:1},{k:'baba',l:'👨 Baba',o:PAR.baba,f:1}];S.friends.forEach(i=>a.push({k:'n'+i,l:'🤝 '+N[i].d.n,o:N[i],f:0}));return a};
function waGen(){if(mode=='play'&&Math.random()<.35){const c=pick(waAll());(S.wa[c.k]=S.wa[c.k]||[]).push({t:pick(WMT)});S.wu++;toast('💬 '+c.l+': yeni mesaj')}}
const PLC=[['Seyrantepe · Rams Park',-150,-78,'p'],['Şükrü Saracoğlu Stadı',-245,138,'p'],['Kadıköy · Boğa & İskele',-95,203,'p'],['Kadıköy Merkez',-100,69,'p'],['Maltepe Merkez',112,84,'p'],['Maltepe Sahil Plajı',166,215,'p'],['Pendik Marina',237,224,'p'],['Sabiha Gökçen Havalimanı',244,152,'p'],['Tüpraş Rafineri',150,-70,'p'],['Banka (Merkez)',40,-38,'p'],['Karakol',-35,-88,'p'],['Boğaz Köprüsü',0,40,'p']];
let MAPP=[...PLC,...SCH.map(q=>[q[0],q[1],q[2]+16,'s']),...MET.map(q=>[q[0]+' Metro',q[1],q[2],'m']),...BST.map(q=>[q[0]+' Durağı',q[1],q[2],'b'])];const MAPI=MAPP;
function drawMap(){const c=$('mc');if(!c)return;const g=c.getContext('2d'),W=320,Hh=300,X=x=>(x-cxm+300)/600*W,Z=z=>(z+300)/560*Hh;
g.fillStyle=ct>0?CTY[ct].gc:'#6f8a55';g.fillRect(0,0,W,Hh);g.fillStyle='#2a6f9e';g.fillRect(0,Z(230),W,Hh);if(!ct)g.fillRect(0,Z(8),W,Z(32)-Z(8));else CTY[ct].W.forEach(b=>g.fillRect(X(b[0]+cxm),Z(b[2]),X(b[1]+cxm)-X(b[0]+cxm),Z(b[3])-Z(b[2])));
g.strokeStyle='#555';g.lineWidth=2;(ct>0?CTY[ct].HR:HR).forEach(z=>{g.beginPath();g.moveTo(0,Z(z));g.lineTo(W,Z(z));g.stroke()});(ct>0?CTY[ct].VR:VR).forEach(x=>{g.beginPath();g.moveTo(X(x),0);g.lineTo(X(x),Z(230));g.stroke()});
g.strokeStyle='#c0392b';g.lineWidth=4;BRX.forEach(x=>{g.beginPath();g.moveTo(X(x),Z(8));g.lineTo(X(x),Z(32));g.stroke()});
g.fillStyle='rgba(255,255,255,.8)';g.font='bold 10px sans-serif';g.textAlign='center';(ct>0?CTY[ct].regs:D).forEach(d=>{if(!d.nl)g.fillText(d.n.toUpperCase(),X(d.x),Z(d.z)-14)});
const cm={p:'#f39c12',s:'#2ecc71',m:'#e74c3c',b:'#3498db'};MAPP.forEach((q,i)=>{g.fillStyle=cm[q[3]];g.beginPath();g.arc(X(q[1]),Z(q[2]),i==S.msel?6:3.5,0,PI*2);g.fill();if(i==S.msel){g.strokeStyle='#fff';g.lineWidth=2;g.stroke()}});
if(MP.rm)for(const id in MP.pl){const r=MP.pl[id];g.fillStyle='#ffd24a';g.beginPath();g.arc(X(r.x),Z(r.z),3.5,0,PI*2);g.fill()}const pp=car||mp;g.fillStyle='#fff';g.strokeStyle='#000';g.lineWidth=1.5;g.beginPath();g.arc(X(pp.x),Z(pp.z),5,0,PI*2);g.fill();g.stroke()}
function goTo(q,cost,msg){if(S.money<cost)return toast('Yetersiz nakit');closePhone();S.money-=cost;if(car)exitCar();if(inside)exitHouse();if(watch)exitStd();$('fade').style.opacity=1;setTimeout(()=>{mp.set(q[1],0,q[2]);col(mp,.4);S.wp=null;$('fade').style.opacity=0;toast(msg+': '+q[0])},550)}
function openMap(){app='map';openPhone()}
$('mapb').onclick=openMap;
// ---------- ses & menü ----------
let AX=null,sfx=1,mus=1,musG,master,amb,crd,jet,engO,engG,burst=0,stepN=0,nextT=0;
const SCL=[0,1,4,5,7,8,10,12,13,16],LEAD=[3,-1,4,5,4,3,2,-1,5,-1,6,7,6,5,4,-1,7,-1,6,5,4,3,2,3,4,-1,2,1,2,-1,0,-1],fr=s=>146.83*Math.pow(2,s/12);
function audioInit(){if(AX)return;try{AX=new(window.AudioContext||window.webkitAudioContext)()}catch(e){return}
master=AX.createGain();master.connect(AX.destination);musG=AX.createGain();musG.connect(master);
const nb=AX.createBuffer(1,AX.sampleRate*2,AX.sampleRate),d=nb.getChannelData(0);for(let i=0;i<d.length;i++)d[i]=Math.random()*2-1;AX.nb=nb;
const nz=(f,q,ty)=>{const s=AX.createBufferSource();s.buffer=nb;s.loop=true;const fl=AX.createBiquadFilter();fl.type=ty;fl.frequency.value=f;fl.Q.value=q;const g=AX.createGain();g.gain.value=0;s.connect(fl);fl.connect(g);g.connect(master);s.start();return g};
amb=nz(500,.4,'lowpass');crd=nz(1100,.7,'bandpass');jet=nz(220,.6,'lowpass');
engO=AX.createOscillator();engO.type='sawtooth';engO.frequency.value=40;engG=AX.createGain();engG.gain.value=0;const lq=AX.createBiquadFilter();lq.frequency.value=400;engO.connect(lq);lq.connect(engG);engG.connect(master);engO.start();setInterval(seq,90)}
function tone(f,t,du,ty,v){const o=AX.createOscillator(),g=AX.createGain();o.type=ty;o.frequency.value=f;g.gain.setValueAtTime(0,t);g.gain.linearRampToValueAtTime(v,t+.03);g.gain.exponentialRampToValueAtTime(.0008,t+du);o.connect(g);g.connect(musG);o.start(t);o.stop(t+du+.05)}
function nbst(t,f,du,v){const s=AX.createBufferSource();s.buffer=AX.nb;const fl=AX.createBiquadFilter();fl.type='highpass';fl.frequency.value=f;const g=AX.createGain();g.gain.setValueAtTime(v,t);g.gain.exponentialRampToValueAtTime(.001,t+du);s.connect(fl);fl.connect(g);g.connect(musG);s.start(t,Math.random());s.stop(t+du+.05)}
function seq(){if(!AX||AX.state!='running')return;const pl=mode=='play',sd=(pl?.2:.34)*(ct==3?.8:1);if(nextT<AX.currentTime)nextT=AX.currentTime+.05;
while(nextT<AX.currentTime+.4){const st=stepN%32,t=nextT,L=LEAD[st];if(L>=0)tone(fr((SCLS[ct]||SCL)[L]+12),t,sd*2.4,pl?'triangle':'sine',pl?.22:.3);
if(st%8==0){tone(fr(-12),t,sd*7,'sine',.5);if(!pl)[0,4,7].forEach(x=>tone(fr(x),t,sd*8,'sawtooth',.045))}
if(pl){if(st%8==0||st%8==3)tone(110,t,.2,'sine',.45);if(st%8==4)nbst(t,3500,.06,.25);if(st%2==1)nbst(t,6000,.03,.08)}
nextT+=sd;stepN++}}
function audUp(dt,pp){if(!AX||!master)return;const t0=AX.currentTime;master.gain.value=sfx?1:0;musG.gain.value=mus?(mode=='play'?.16:.4):0;
amb.gain.setTargetAtTime(inside?.01:(gt%300<180?.05:.025)+rainI*.06,t0,.3);
let cr=0;for(const o of STD){const s=o.s;if(s&&s.ph<2){const d=Math.hypot(o.cx-pp.x,o.cz-pp.z),l=(watch===o?1:cl(1-d/150,0,1)*.5)*(s.ph==1?1:.35);cr=Math.max(cr,l)}}
crd.gain.setTargetAtTime(cr*.28*(.7+.3*Math.sin(gt*9.4))+(cr>.05?burst*.45:0),t0,.08);
engG.gain.setTargetAtTime(car?.03+.05*Math.abs(car.v)/22:0,t0,.1);engO.frequency.setTargetAtTime(38+(car?Math.abs(car.v)*5:0),t0,.1);
let pd=1e9;PLN.forEach(p=>pd=Math.min(pd,Math.hypot(p.x-cam.position.x,p.y-cam.position.y,p.z-cam.position.z)));jet.gain.setTargetAtTime(flight?.14:cl(1-pd/450,0,1)*.1,t0,.3)}
addEventListener('pointerdown',()=>{audioInit();if(AX&&AX.state!='running')AX.resume()},{once:true});
function bars(on){$('lt').style.height=on?'11vh':'0';$('lbb').style.height=on?'11vh':'0'}bars(1);
$('mstart').onclick=()=>{audioInit();if(AX&&AX.state!='running')AX.resume();
if(started){mode='play';document.body.classList.remove('menu');$('menu').style.display='none';return}
$('fade').style.opacity=1;setTimeout(()=>{started=true;gt=0;S.day=0;mode='intro';itime=0;MP.name=($('nick').value.trim()||MP.name).slice(0,14);mpConnect($('roomc').value.trim().toLowerCase().replace(/[^a-z0-9_.-]/g,'-').slice(0,40));document.body.classList.remove('menu');$('menu').style.display='none';$('fade').style.opacity=0;toast('İstanbul · Metropol')},600)};
$('mmus').onclick=()=>{mus=!mus;$('mmus').textContent='🎵 Müzik: '+(mus?'Açık':'Kapalı')};
$('msnd').onclick=()=>{sfx=!sfx;$('msnd').textContent='🔊 Ses: '+(sfx?'Açık':'Kapalı')};
$('mhelp').onclick=()=>{const h=$('help');h.style.display=h.style.display=='block'?'none':'block'};
$('mb').onclick=()=>{mode='menu';document.body.classList.add('menu');$('menu').style.display='flex';$('mstart').textContent='▶ Devam Et'};
function openATM(){app='atm';openPhone()}
// ---------- ana döngü ----------
let last=performance.now(),fp=0,mlast='';const arr="↑↗→↘↓↙←↖";
function loop(now){requestAnimationFrame(loop);const dt=Math.min(.05,(now-last)/1000);last=now;gt=MP.sync?Date.now()/1000:gt+dt;env();
const dayN=Math.floor(gt/300);if(dayN>S.day){S.day=dayN;let r=0;H.forEach(h=>{if(h.own)r+=h.price*.0025});if(r){S.money+=r;toast('🏠 Kira geliri +'+fmt(r))}if(S.bank>0){const ip=S.bank*.005;S.bank+=ip;toast('🏦 Faiz +'+fmt(ip))}S.hap=Math.max(0,S.hap-8);if(S.hap<25)toast('💔 Ailen seni özledi, ara!')}
fp+=dt;if(fp>14){fp=0;tick14()}
const busy=chatOpen||phoneOpen||mode!='play'||!!flight;let fx=busy?0:joy.x,fy=busy?0:joy.y;
if(!busy&&!watch){if(keys.KeyW||keys.ArrowUp)fy+=1;if(keys.KeyS||keys.ArrowDown)fy-=1;if(keys.KeyD||keys.ArrowRight)fx+=1;if(keys.KeyA||keys.ArrowLeft)fx-=1}
if(watch){fx=fy=0}
if(car){drive(car,fx,fy,dt);spd=0}
else if(!watch){const mag=Math.min(1,Math.hypot(fx,fy)),fast=run||keys.ShiftLeft||keys.ShiftRight,tg=mag>.08?(fast?5.2:2.4)*Math.max(.5,mag):0;spd+=(tg-spd)*Math.min(1,dt*6);
let mx=-Math.sin(yaw)*fy+Math.cos(yaw)*fx,mz=-Math.cos(yaw)*fy-Math.sin(yaw)*fx;const ml=Math.hypot(mx,mz);
if(ml>.01&&mag>.08){mx/=ml;mz/=ml;mp.x+=mx*spd*dt;mp.z+=mz*spd*dt;face(me,mx,mz,dt)}
if(inside){mp.x=cl(mp.x,995.7,1004.3);mp.z=cl(mp.z,995.7,1004.3)}else col(mp,.4);
wph+=spd*dt*2.6;anim(me,wph,Math.min(.9,spd*.2));mp.y=Math.abs(Math.sin(wph))*.03*Math.min(1,spd/2.4)}
const pp=car||mp;
// NPC
for(const n of N){if(n.fol){folUpd(n,dt);continue}const p=n.g.position,dd=Math.hypot(p.x-mp.x,p.z-mp.z);n.g.visible=dd<110;if(dd>110&&!(chatNpc===n))continue;n.lb.visible=dd<14;let mv=0;
if(chatOpen&&chatNpc===n)face(n.g,mp.x-p.x,mp.z-p.z,dt);
else{if(n.sc){const t=gt%300,at=t>=20&&t<110;if(n.mode!==at){n.mode=at;const q=at?n.sc:n.sh;n.hx=q.x;n.hz=q.z;n.tx=q.x+rnd(-6,6);n.tz=q.z+rnd(-4,4);n.w=0}}n.t+=dt;if(n.w>0)n.w-=dt;else{const dx=n.tx-p.x,dz=n.tz-p.z,d=Math.hypot(dx,dz);if(d<.5||n.t>10){n.w=rnd(2,6);n.t=0;n.tx=n.hx+rnd(-12,12);n.tz=n.hz+rnd(-12,12)}else{p.x+=dx/d*(n.sp||1.2)*dt;p.z+=dz/d*(n.sp||1.2)*dt;mv=1;face(n.g,dx,dz,dt)}}}
col(p,.4);n.ph+=mv*dt*5;anim(n.g,n.ph,mv?.5:0)}
// arabalar
for(const c of CARS){if(c===car){}else if(c.ai)aiCar(c,dt);else c.v*=Math.max(0,1-dt*3);c.g.position.set(c.x,0,c.z);c.g.rotation.y=c.a;c.g.visible=Math.hypot(c.x-pp.x,c.z-pp.z)<130}
// stadlar
let sbT='',sbD=150,sbU='';
for(const o of STD){const s=mst(o),d=Math.hypot(o.cx-pp.x,o.cz-pp.z),nr_=d<150||watch===o,f=s.f;o.s=s;
if(o.k!==s.k){o.k=s.k;o.last=0;o.p0=0;const hv=KIT[f.home],av=KIT[f.away];o.mA.color.setHex(hv[0]);o.mB.color.setHex(av[0]==hv[0]?av[1]:av[0])}
o.gate.open=!!S.tix[o.id+':'+s.k]&&s.ph<2;o.bar.visible=!o.gate.open;
const live=s.ph==1&&nr_;o.G.visible=live;o.F.visible=live;o.F.position.y=Math.abs(Math.sin(gt*5))*(.06+burst*.25);
if(live){const b=o.bs;b.tm-=dt;if(b.tm<=0){b.tm=rnd(1.5,3);b.t=Math.floor(rnd(0,22))}if(o.gs>0){o.gs-=dt;b.x+=(o.gx-b.x)*Math.min(1,dt*6);b.z+=(o.cz-b.z)*Math.min(1,dt*6)}else{const tp=o.P[b.t].position;b.x+=(tp.x-b.x)*Math.min(1,dt*3);b.z+=(tp.z-b.z)*Math.min(1,dt*3)}o.ball.position.set(b.x,.3,b.z);
for(let i=0;i<22;i++){const j=i%11,sg=i<11?1:-1,bx=o.cx+FXx[j]*sg*o.pw/2,bz=o.cz+FXz[j]*o.pH/2;o.P[i].position.set(j?bx+Math.sin(gt*.7+i*1.7)*4+(b.x-bx)*.15:bx,0,j?bz+Math.cos(gt*.5+i*2.3)*3+(b.z-bz)*.15:o.cz+cl((b.z-o.cz)*.4,-3.2,3.2))}}
if(s.ph==1){const tot=s.sc[0]+s.sc[1];if(tot>o.last){o.gs=3;o.gx=o.cx+(s.sc[0]>(o.p0||0)?1:-1)*o.pw/2;if(nr_){burst=1;toast('⚽ GOL! '+ab(f.home)+' '+s.sc[0]+'-'+s.sc[1]+' '+ab(f.away));if(watch===o)fire(o.cx,o.cz)}}o.last=tot;o.p0=s.sc[0]}
if(watch===o&&s.ph==2){exitStd();toast('Maç bitti: '+ab(f.home)+' '+s.sc[0]+'-'+s.sc[1]+' '+ab(f.away))}
if(nr_&&d<sbD){sbD=d;sbU=f.ucl?'⭐ ŞL · ':'';sbT=s.ph==1?'🔴 '+o.name+' · '+ab(f.home)+' '+s.sc[0]+'-'+s.sc[1]+' '+ab(f.away)+" · "+s.min+"'":s.ph==0?o.name+' · '+f.home+'–'+f.away+' · '+fmt(f.price)+' · '+(110-(s.tt|0))+' sn':o.name+' · MS '+ab(f.home)+' '+s.sc[0]+'-'+s.sc[1]+' '+ab(f.away)}}
const sb=$('sb');sb.style.display=sbT&&!inside?'block':'none';if(sbT&&sb.textContent!==sbU+sbT)sb.textContent=sbU+sbT;
// görev
if(S.ms&&S.job&&!inside&&!watch&&ct===0){const w=S.ms.pts[S.ms.i][1];if(Math.hypot(w.x-pp.x,w.z-pp.z)<5&&(!S.job.car||car)){toast('✓ '+S.ms.pts[S.ms.i][0]);S.ms.i++;if(S.ms.i>=S.ms.pts.length){S.money+=S.ms.pay;toast('💰 Görev tamam +'+fmt(S.ms.pay));startMs()}}}
const wpv=S.ms?{t:S.ms.pts[S.ms.i][0],x:S.ms.pts[S.ms.i][1].x,z:S.ms.pts[S.ms.i][1].z}:S.wp,wpE=$('wp');
if(flight){wpE.style.display='block';const rm=Math.max(0,Math.ceil(flight.dur-flight.t));wpE.textContent='✈️ '+CTY[flight.k0].nm+' → '+CTY[flight.k1].nm+' · '+Math.floor(rm/60)+':'+String(rm%60).padStart(2,'0')}else if(wpv&&!inside){const dx=wpv.x-pp.x,dz=wpv.z-pp.z,dd=Math.hypot(dx,dz);let rel=-(((Math.atan2(dx,dz)-(yaw+PI)+PI)%(2*PI)+2*PI)%(2*PI)-PI);const ar=arr[((Math.round(rel/(PI/4))%8)+8)%8];wpE.style.display='block';wpE.textContent=wpv.t+' · '+Math.round(dd)+' m '+ar;if(S.wp&&!S.ms&&dd<6)S.wp=null}else wpE.style.display='none';
// bölge
ct=flight?-1:cl(Math.round(pp.x/3000),0,3);cxm=ct>0?CTY[ct].cx:0;if(ct!==lastCt&&ct>=0){lastCt=ct;MAPP=ct>0?CTY[ct].mapp:MAPI;S.msel=-1}let bn=ct>0?CTY[ct].nm:'İstanbul',bd=ct>0?230:115;for(const d of (ct>0?CTY[ct].regs:D)){if(d.nl)continue;const k=Math.hypot(d.x-pp.x,d.z-pp.z);if(k<bd){bd=k;bn=d.n}}curReg=bn;const dnm=flight?'✈️ Uçuşta':CTY[Math.max(ct,0)].fl+' '+bn;if($('dist').textContent!==dnm)$('dist').textContent=dnm;
const ms_='💰 '+fmt(S.money)+' · 🏦 '+fmt(S.bank);if(ms_!==mlast){mlast=ms_;$('mny').textContent=ms_}
// bağlam
let best=null,bdd=1e9;const cc=(d,l,f)=>{if(d<bdd){bdd=d;best={l,f}}};
if(!busy){
if(watch)cc(0,'⬅️ Tribünden çık',exitStd);
else if(inside){let d=Math.hypot(mp.x-996.5,mp.z-996.8);if(d<2.6)cc(d,'🛏️ Uyu',sleep);d=Math.hypot(mp.x-1000,mp.z-1004.2);if(d<2.4)cc(d,'🚪 Dışarı çık',exitHouse)}
else if(car)cc(0,'🚗 Arabadan in',exitCar);
else{for(const n of N){if(!n.g.visible)continue;const d=Math.hypot(n.g.position.x-mp.x,n.g.position.z-mp.z);if(d<3.2)cc(d,'💬 '+n.d.n,()=>openChat(n))}
for(const h of H){const d=Math.hypot(mp.x-h.dx,mp.z-h.dz);if(d<2.8)cc(d,h.own?'🏠 Eve gir':'🚪 Kapıyı çal',()=>h.own?enterHouse(h):knock(h))}
for(const c of CARS){if(c.bus)continue;const d=Math.hypot(mp.x-c.x,mp.z-c.z);if(d<4)cc(d+.8,c.taxi?'🚕 Taksiye bin':'🚗 Arabaya bin',c.taxi?openMap:()=>enter(c))}for(const q of TRN){const d=Math.hypot(mp.x-q.x,mp.z-q.z);if(d<5)cc(d,q.t=='m'?'🚇 Metro':'🚌 Durak',openMap)}
for(const a of AIRP){const d=Math.hypot(mp.x-a.x,mp.z-a.z);if(d<14)cc(d,'✈️ Havalimanı · Uçuş',openUcus)}for(const a of ATM){const d=Math.hypot(mp.x-a.x,mp.z-a.z);if(d<3)cc(d,'💳 ATM',openATM)}if(Math.hypot(mp.x-BK.x,mp.z-BK.z)<5)cc(1,'🏦 Banka',openATM);
for(const o of STD){const d=Math.hypot(mp.x-o.cx,mp.z-(o.cz+o.R+3));if(d<9)cc(d,o.gate.open?'🎟️ Maça gir':'🎟️ Giriş (bilet gerekli)',()=>o.gate.open?watchStd(o):toast('Biletin yok. Telefon → Biletler'))}}}
ctxB.style.display=best?'block':'none';if(best&&ctxB.textContent!==best.l)ctxB.textContent=best.l;ctxF=best?best.f:null;
il.intensity=inside?1.2:0;fli.intensity=1+Math.sin(now*.01)*.2;
// kamera
if(flight)fcam(dt);else if(watch){const o=watch,R=o.R-6;cam.position.set(o.cx+Math.sin(yaw)*R,11,o.cz+Math.cos(yaw)*R);cam.lookAt(o.cx,1,o.cz)}
else if(inside){cam.position.set(cl(mp.x+Math.sin(yaw)*Math.cos(pitch)*dist,995.4,1004.6),cl(1.6+Math.sin(pitch)*dist,.6,3.2),cl(mp.z+Math.cos(yaw)*Math.cos(pitch)*dist,995.4,1004.6));cam.lookAt(mp.x,1.3,mp.z)}
else{cam.position.set(pp.x+Math.sin(yaw)*Math.cos(pitch)*dist,1.6+Math.sin(pitch)*dist,pp.z+Math.cos(yaw)*Math.cos(pitch)*dist);cam.lookAt(pp.x,car?1:1.5,pp.z)}
if(mode=='menu'){const a=now*.00004;cam.position.set(-100+Math.sin(a)*150,48,150+Math.cos(a)*150);cam.lookAt(-100,14,150)}
else if(mode=='intro'){itime+=dt;const k=cl(itime/4,0,1),e=k*k*(3-2*k),a=now*.00004,P=cam.position,m0=[-100+Math.sin(a)*150,48,150+Math.cos(a)*150];P.set(m0[0]+(P.x-m0[0])*e,m0[1]+(P.y-m0[1])*e,m0[2]+(P.z-m0[2])*e);cam.lookAt(-100+(pp.x+100)*e,14+(1.5-14)*e,150+(pp.z-150)*e);if(itime>=4)mode='play'}
const foc=mode=='menu'?{x:-100,z:150}:watch?{x:watch.cx,z:watch.cz}:pp;
sun.position.set(foc.x+SD.x*120,SD.y*120,foc.z+SD.z*120);sun.target.position.set(foc.x,0,foc.z);skyD.position.copy(cam.position);
cam.fov=(cam.aspect<1?75:60)+(mode=='play'?(car?Math.abs(car.v)*.5:spd*.9):0);cam.updateProjectionMatrix();
const wb=mode!='play'||!!watch||!!flight;if(wb!==barsOn){barsOn=wb;bars(wb)}
if(watch&&!dr)yaw+=dt*.06;
FL.forEach((f,i)=>{f.rotation.y=Math.sin(gt*2.2+i)*.3});const bl=Math.floor(gt*4)%2==0;PLR.forEach(m=>m.visible=bl);PLB.forEach(m=>m.visible=!bl);
for(const p of PLN){p.x+=Math.sin(p.a)*p.v*dt;p.z+=Math.cos(p.a)*p.v*dt;if(Math.hypot(p.x,p.z)>600)p.a=Math.atan2(-p.x,-p.z)+rnd(-.5,.5);p.g.position.set(p.x,p.y,p.z);p.g.rotation.y=p.a;p.ls.visible=Math.floor(gt*1.3)%2==0}
SEAM.map.offset.x+=dt*.003;burst=Math.max(0,burst-dt*.4);
if(car&&Math.abs(car.v)>17&&gt>fineT&&(CARS.some(c=>c.pol&&c!==car&&Math.hypot(c.x-car.x,c.z-car.z)<50)||N.some(n=>n.pol&&Math.hypot(n.g.position.x-car.x,n.g.position.z-car.z)<40))){fineT=gt+25;S.money-=5000;toast('🚔 Hız cezası! -'+fmt(5000))}
for(let i=FW.length-1;i>=0;i--){const f=FW[i];f.d-=dt;if(f.d>0){f.m.visible=false;continue}f.m.visible=true;f.t+=dt;const pa=f.m.geometry.attributes.position;for(let j=0;j<f.v.length;j++){pa.array[j*3]+=f.v[j][0]*dt;pa.array[j*3+1]+=(f.v[j][1]-f.t*3)*dt;pa.array[j*3+2]+=f.v[j][2]*dt}pa.needsUpdate=true;f.m.material.opacity=Math.max(0,1-f.t/2.2);if(f.t>2.2){scene.remove(f.m);FW.splice(i,1)}}
YT.forEach((y,i)=>{y.position.y=.15+Math.sin(gt*1.3+i)*.12;y.rotation.z=Math.sin(gt+i*2)*.03});
const pbT=S.unread+S.wu?'📱 ('+(S.unread+S.wu)+')':'📱 Telefon';if($('pb').textContent!==pbT)$('pb').textContent=pbT;
SPIN.forEach(q=>{q.g.rotation.z+=q.s*dt});BAL.forEach((g,i)=>{g.position.y+=Math.sin(gt*.6+i)*.02;g.position.x+=Math.cos(gt*.2+i)*.02});
rainI=ct==2?(curReg=='Manchester'?.9:curReg=='London'?.5:.35):0;RAINP.visible=rainI>0&&!flight;if(RAINP.visible){RAINP.position.set(cam.position.x,cam.position.y-12,cam.position.z);RAINP.material.opacity=rainI*.6;const ra=RAINP.geometry.attributes.position.array;for(let i=1;i<ra.length;i+=3){ra[i]-=38*dt;if(ra[i]<0)ra[i]+=30}RAINP.geometry.attributes.position.needsUpdate=true}
mpUpdate(now,dt,pp);
audUp(dt,pp);
ren.render(scene,cam)}
function rs(){cam.aspect=innerWidth/innerHeight;cam.fov=cam.aspect<1?75:60;cam.updateProjectionMatrix();ren.setSize(innerWidth,innerHeight)}
sh(scene);skyD.traverse(m=>{m.castShadow=m.receiveShadow=false});
addEventListener('resize',rs);rs();requestAnimationFrame(loop);
</script></body></html>
