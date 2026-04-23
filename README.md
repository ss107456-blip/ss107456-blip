<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Abhishek Grover</title>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=IBM+Plex+Mono:wght@300;400;500&family=Geist:wght@200;300;400;500&display=swap" rel="stylesheet"/>
<style>
*,*::before,*::after{margin:0;padding:0;box-sizing:border-box}
:root{
  --ink:#e4dfd8;--ink-2:#9a9088;--ink-3:#4a4540;--ink-4:#2a2722;
  --bg:#080705;--bg-2:#0f0e0b;--bg-3:#161410;--bg-4:#1d1b16;
  --rule:#1e1c17;--rule-2:#272420;
  --serif:'Instrument Serif',Georgia,serif;
  --mono:'IBM Plex Mono','Courier New',monospace;
  --sans:'Geist',system-ui,sans-serif;
  --glow:rgba(200,180,140,0.06);
}
html,body{height:100%;background:var(--bg)}
body{font-family:var(--sans);font-weight:300;-webkit-font-smoothing:antialiased;overflow-x:hidden;cursor:none}

/* custom cursor */
#cursor{position:fixed;width:6px;height:6px;background:var(--ink);border-radius:50%;pointer-events:none;z-index:9999;transform:translate(-50%,-50%);transition:transform .1s;mix-blend-mode:difference}
#cursor-ring{position:fixed;width:28px;height:28px;border:1px solid rgba(228,223,216,.2);border-radius:50%;pointer-events:none;z-index:9998;transform:translate(-50%,-50%);transition:all .18s ease}

/* canvas bg */
#three-canvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:0;opacity:.55}

/* noise overlay */
.noise{position:fixed;inset:0;z-index:1;opacity:.025;pointer-events:none;
  background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  background-size:180px;
}

/* vignette */
.vignette{position:fixed;inset:0;z-index:2;pointer-events:none;
  background:radial-gradient(ellipse 80% 80% at 50% 50%,transparent 40%,rgba(4,3,2,.7) 100%)}

/* shell */
.shell{position:relative;z-index:10;max-width:820px;margin:0 auto;padding:0 36px 120px}

/* topbar */
.topbar{border-bottom:1px solid var(--rule);padding:18px 0;display:flex;justify-content:space-between;align-items:center;
  font-family:var(--mono);font-size:10px;letter-spacing:.1em;color:var(--ink-3);
  animation:fadeUp .8s ease both}
.topbar a{color:var(--ink-3);text-decoration:none;transition:color .2s}
.topbar a:hover{color:var(--ink-2)}

/* hero */
.hero{padding:80px 0 60px;border-bottom:1px solid var(--rule);
  animation:fadeUp .8s .1s ease both;opacity:0}
.hero-name{
  font-family:var(--serif);font-weight:400;
  font-size:clamp(3.2rem,8vw,5.5rem);
  line-height:.95;letter-spacing:-.03em;
  color:var(--ink);
  text-shadow:
    0 1px 0 #3a3530,
    0 2px 0 #2e2a26,
    0 3px 0 #22201c,
    0 4px 0 #191714,
    0 5px 0 #100f0c,
    0 6px 12px rgba(0,0,0,.6),
    0 12px 30px rgba(0,0,0,.4);
  transform-style:preserve-3d;
  display:inline-block;
  margin-bottom:20px;
  transition:text-shadow .3s,transform .3s;
}
.hero-name:hover{
  text-shadow:
    0 1px 0 #3a3530,0 2px 0 #2e2a26,0 3px 0 #22201c,
    0 4px 0 #191714,0 5px 0 #100f0c,0 6px 0 #080705,
    0 7px 0 #040302,0 8px 20px rgba(0,0,0,.7),
    0 20px 50px rgba(0,0,0,.5);
  transform:translateY(-2px);
}
.hero-sub{font-size:.8rem;font-family:var(--mono);color:var(--ink-3);letter-spacing:.1em;text-transform:uppercase;margin-bottom:28px}
.hero-chips{display:flex;flex-wrap:wrap;gap:7px}
.hchip{
  font-family:var(--mono);font-size:.68rem;letter-spacing:.05em;
  border:1px solid var(--rule-2);color:var(--ink-3);
  padding:5px 12px;border-radius:2px;
  background:rgba(255,255,255,.015);
  backdrop-filter:blur(4px);
  transition:all .25s;
  transform:perspective(400px) rotateX(0deg);
}
.hchip:hover{color:var(--ink-2);border-color:var(--ink-3);transform:perspective(400px) rotateX(-8deg) translateY(-2px);background:rgba(255,255,255,.04)}
.hchip.green{border-color:#2a3d2a;color:#5a8060}

/* section grid */
.grid{display:grid;grid-template-columns:164px 1fr;animation:fadeUp .8s .2s ease both;opacity:0}
.sl{padding:36px 20px 36px 0;border-bottom:1px solid var(--rule);
  font-family:var(--mono);font-size:.6rem;letter-spacing:.14em;text-transform:uppercase;
  color:var(--ink-4);align-self:stretch;display:flex;align-items:flex-start;padding-top:42px}
.sc{padding:36px 0 36px 32px;border-bottom:1px solid var(--rule);border-left:1px solid var(--rule)}

/* 3d code card */
.code-card{
  perspective:1000px;
  transform-style:preserve-3d;
}
.code-inner{
  background:var(--bg-2);
  border:1px solid var(--rule-2);
  border-radius:5px;
  overflow:hidden;
  transition:transform .4s ease, box-shadow .4s ease;
  transform:perspective(1000px) rotateX(2deg) rotateY(-1deg);
  box-shadow:0 8px 32px rgba(0,0,0,.5),0 2px 8px rgba(0,0,0,.4),inset 0 1px 0 rgba(255,255,255,.03);
}
.code-card:hover .code-inner{
  transform:perspective(1000px) rotateX(-1deg) rotateY(1deg) translateZ(8px);
  box-shadow:0 20px 60px rgba(0,0,0,.6),0 8px 20px rgba(0,0,0,.5),inset 0 1px 0 rgba(255,255,255,.05);
}
.cbar{background:var(--bg-3);border-bottom:1px solid var(--rule);padding:9px 14px;
  display:flex;align-items:center;gap:6px}
.dot{width:8px;height:8px;border-radius:50%;background:var(--rule-2)}
.cfile{font-family:var(--mono);font-size:.62rem;color:var(--ink-4);margin-left:8px;letter-spacing:.04em}
.cbody{padding:20px 22px;font-family:var(--mono);font-size:.74rem;line-height:2;overflow-x:auto}
.ln{color:var(--ink-4);user-select:none;display:inline-block;width:20px;text-align:right;margin-right:16px;font-size:.65rem}
.kw{color:#6a6458}.fn{color:#c8bc9c}.st{color:#8a9878}.cm{color:#333028}.nb{color:#9a9080}.nx{color:#d8ccb4}

/* stats */
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:1px;
  background:var(--rule);border:1px solid var(--rule);border-radius:4px;overflow:hidden}
.stat{
  background:var(--bg-2);padding:22px 18px;
  transition:all .3s;transform:perspective(600px) rotateX(0);
}
.stat:hover{background:var(--bg-3);transform:perspective(600px) rotateX(-4deg) translateZ(4px)}
.sn{font-family:var(--serif);font-size:2.2rem;color:var(--ink);line-height:1;margin-bottom:5px;
  text-shadow:0 2px 0 #2a2520,0 4px 8px rgba(0,0,0,.4)}
.sl2{font-family:var(--mono);font-size:.6rem;color:var(--ink-3);letter-spacing:.1em;text-transform:uppercase}

/* cert table */
.ct{width:100%;border-collapse:collapse}
.ct thead tr{border-bottom:1px solid var(--rule-2)}
.ct th{font-family:var(--mono);font-size:.6rem;letter-spacing:.12em;text-transform:uppercase;
  color:var(--ink-4);font-weight:400;padding:0 0 10px;text-align:left}
.ct th:last-child{text-align:right}
.ct td{padding:11px 0;font-size:.77rem;font-weight:300;color:#7a7468;
  border-bottom:1px solid var(--rule);vertical-align:middle;transition:color .2s}
.ct td:first-child{color:#b0a898;font-weight:400;padding-right:16px}
.ct td:last-child{text-align:right}
.ct tr:last-child td{border-bottom:none}
.ct tr:hover td{color:var(--ink-2)}
.pill{font-family:var(--mono);font-size:.61rem;padding:2px 8px;
  border:1px solid var(--rule-2);border-radius:2px;color:var(--ink-3);white-space:nowrap}

/* skills */
.sb{margin-bottom:20px}
.sb:last-child{margin-bottom:0}
.sh{font-family:var(--mono);font-size:.6rem;letter-spacing:.12em;text-transform:uppercase;color:var(--ink-4);margin-bottom:9px}
.chips{display:flex;flex-wrap:wrap;gap:5px}
.chip{
  font-family:var(--mono);font-size:.71rem;padding:4px 11px;
  border:1px solid var(--rule-2);border-radius:2px;color:#6a6458;
  transition:all .25s;transform:perspective(300px) rotateX(0);
}
.chip:hover{color:var(--ink-2);border-color:var(--ink-4);
  transform:perspective(300px) rotateX(-6deg) translateZ(3px);
  background:rgba(255,255,255,.025)}

/* projects */
.plist{display:flex;flex-direction:column}
.pi{padding:20px 0;border-bottom:1px solid var(--rule);display:grid;grid-template-columns:1fr auto;gap:16px;align-items:start}
.pi:first-child{padding-top:0}
.pi:last-child{border-bottom:none;padding-bottom:0}
.pt{font-size:.87rem;font-weight:500;color:#c0b8a8;margin-bottom:5px;letter-spacing:-.01em}
.pd{font-size:.76rem;color:var(--ink-3);line-height:1.75;font-weight:300}
.pty{font-family:var(--mono);font-size:.6rem;color:var(--ink-4);letter-spacing:.08em;text-transform:uppercase;white-space:nowrap;padding-top:3px}

/* edu */
.er{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:5px}
.ed{font-size:.87rem;font-weight:500;color:#c0b8a8}
.edt{font-family:var(--mono);font-size:.67rem;color:var(--ink-3)}
.esc{font-size:.77rem;color:var(--ink-3);margin-bottom:10px}
.en{font-family:var(--mono);font-size:.7rem;color:var(--ink-4);line-height:1.9}

/* footer */
.foot{padding:52px 0 0;display:flex;justify-content:space-between;align-items:center;
  font-family:var(--mono);font-size:.63rem;color:var(--ink-4);letter-spacing:.06em;
  border-top:1px solid var(--rule);animation:fadeUp .8s .3s ease both;opacity:0}
.fq{font-family:var(--serif);font-style:italic;font-size:.85rem;color:var(--ink-3)}

/* animations */
@keyframes fadeUp{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:translateY(0)}}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
</style>
</head>
<body>

<div id="cursor"></div>
<div id="cursor-ring"></div>
<canvas id="three-canvas"></canvas>
<div class="noise"></div>
<div class="vignette"></div>

<div class="shell">

  <div class="topbar">
    <span>github.com / AbhishekGrover</span>
    <div style="display:flex;gap:20px">
      <a href="mailto:ss107456@gmail.com">ss107456@gmail.com</a>
      <span>New Delhi, India</span>
    </div>
  </div>

  <div class="hero">
    <div class="hero-name" id="heroName">Abhishek Grover</div>
    <div class="hero-sub">Data Scientist &nbsp;/&nbsp; Python Developer &nbsp;/&nbsp; AI & Analytics</div>
    <div class="hero-chips">
      <span class="hchip green">Open to opportunities</span>
      <span class="hchip">BCA · Amity University · Sem 4</span>
      <span class="hchip">10 Industry Certifications</span>
      <span class="hchip">80% workflow efficiency gain</span>
    </div>
  </div>

  <div class="grid">

    <div class="sl">About</div>
    <div class="sc">
      <div class="code-card" id="codeCard">
        <div class="code-inner">
          <div class="cbar"><div class="dot"></div><div class="dot"></div><div class="dot"></div><span class="cfile">profile.py</span></div>
          <div class="cbody">
<span class="ln">1</span><span class="kw">class</span> <span class="fn">AbhishekGrover</span>:<br>
<span class="ln">2</span>&nbsp;&nbsp;&nbsp;&nbsp;<span class="kw">def</span> <span class="fn">__init__</span>(<span class="nb">self</span>):<br>
<span class="ln">3</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nb">self</span>.<span class="nx">role</span>      <span class="kw">=</span> <span class="st">"Data Scientist | Python Developer | AI Specialist"</span><br>
<span class="ln">4</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nb">self</span>.<span class="nx">education</span> <span class="kw">=</span> <span class="st">"BCA, Amity University Online — Sem 4 (2027)"</span><br>
<span class="ln">5</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nb">self</span>.<span class="nx">certs</span>     <span class="kw">=</span> <span class="nb">10</span>  <span class="cm"># JPMorgan · BCG · Deloitte · TATA · BA</span><br>
<span class="ln">6</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cm">#</span><br>
<span class="ln">7</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nb">self</span>.<span class="nx">stack</span> <span class="kw">=</span> {<br>
<span class="ln">8</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="st">"data"</span>     : [<span class="st">"Pandas"</span>, <span class="st">"NumPy"</span>, <span class="st">"Matplotlib"</span>, <span class="st">"Scikit-learn"</span>],<br>
<span class="ln">9</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="st">"ai"</span>       : [<span class="st">"Prompt Engineering"</span>, <span class="st">"ChatGPT API"</span>, <span class="st">"Midjourney"</span>],<br>
<span class="ln">10</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="st">"creative"</span> : [<span class="st">"Photoshop"</span>, <span class="st">"Premiere Pro"</span>, <span class="st">"Canva"</span>],<br>
<span class="ln">11</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
<span class="ln">12</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cm">#</span><br>
<span class="ln">13</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nb">self</span>.<span class="nx">impact</span>  <span class="kw">=</span> <span class="st">"80% reduction in workflow time via AI pipelines"</span><br>
<span class="ln">14</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="nb">self</span>.<span class="nx">status</span>  <span class="kw">=</span> <span class="st">"Kaggle ML · Coursera DL · actively learning"</span>
          </div>
        </div>
      </div>
    </div>

    <div class="sl">Numbers</div>
    <div class="sc">
      <div class="stats">
        <div class="stat"><div class="sn" id="cn">0</div><div class="sl2">Certifications</div></div>
        <div class="stat"><div class="sn" id="wn">0%</div><div class="sl2">Workflow saved</div></div>
        <div class="stat"><div class="sn">3+</div><div class="sl2">Projects</div></div>
      </div>
    </div>

    <div class="sl">Certifications</div>
    <div class="sc">
      <table class="ct">
        <thead><tr><th>#</th><th>Firm</th><th>Programme</th><th>Domain</th></tr></thead>
        <tbody id="ctbody"></tbody>
      </table>
    </div>

    <div class="sl">Stack</div>
    <div class="sc">
      <div class="sb"><div class="sh">Python &amp; Data</div>
        <div class="chips"><span class="chip">Python</span><span class="chip">Pandas</span><span class="chip">NumPy</span><span class="chip">Matplotlib</span><span class="chip">Scikit-learn</span><span class="chip">EDA</span><span class="chip">Statistics</span></div>
      </div>
      <div class="sb"><div class="sh">AI &amp; Automation</div>
        <div class="chips"><span class="chip">Prompt Engineering</span><span class="chip">ChatGPT API</span><span class="chip">Midjourney</span><span class="chip">Generative AI</span><span class="chip">Workflow Automation</span></div>
      </div>
      <div class="sb"><div class="sh">Creative Suite</div>
        <div class="chips"><span class="chip">Adobe Photoshop</span><span class="chip">Premiere Pro</span><span class="chip">Canva</span><span class="chip">CapCut</span></div>
      </div>
    </div>

    <div class="sl">Projects</div>
    <div class="sc">
      <div class="plist">
        <div class="pi">
          <div><div class="pt">Python Data Analysis &amp; Visualization</div>
          <div class="pd">End-to-end ML pipeline — data ingestion, cleaning, regression and classification via Scikit-learn, stakeholder-ready Matplotlib reports. Full EDA → Feature Engineering → Model Evaluation.</div></div>
          <div class="pty">Academic</div>
        </div>
        <div class="pi">
          <div><div class="pt">AI-Powered Workflow Automation Suite</div>
          <div class="pd">Rebuilt repetitive workflows as GPT-based pipelines. 80% reduction in task completion time. Reusable prompt-template library with documented best practices.</div></div>
          <div class="pty">Personal</div>
        </div>
        <div class="pi">
          <div><div class="pt">Creative Content Portfolio</div>
          <div class="pd">Brand assets, short-form video and social content for small businesses. Integrated Midjourney AI into design workflow for 10× creative throughput.</div></div>
          <div class="pty">Freelance</div>
        </div>
      </div>
    </div>

    <div class="sl">Education</div>
    <div class="sc">
      <div class="er"><span class="ed">Bachelor of Computer Applications</span><span class="edt">Expected 2027</span></div>
      <div class="esc">Amity University Online, Noida &nbsp;·&nbsp; Semester 4</div>
      <div class="en">Data Structures &nbsp;·&nbsp; DBMS &nbsp;·&nbsp; Statistics &nbsp;·&nbsp; Python Programming<br>Supplemented with 10 industry virtual programmes and self-directed projects</div>
    </div>

  </div>

  <div class="foot">
    <span class="fq">"Data tells the story. AI writes the future. I build the pipeline."</span>
    <span>Abhishek Grover &nbsp;·&nbsp; 2025</span>
  </div>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
/* ── Custom cursor ── */
const cur = document.getElementById('cursor');
const ring = document.getElementById('cursor-ring');
let mx=0,my=0,rx=0,ry=0;
document.addEventListener('mousemove',e=>{mx=e.clientX;my=e.clientY;cur.style.left=mx+'px';cur.style.top=my+'px'});
setInterval(()=>{rx+=(mx-rx)*.12;ry+=(my-ry)*.12;ring.style.left=Math.round(rx)+'px';ring.style.top=Math.round(ry)+'px'},16);
document.querySelectorAll('a,button,.hchip,.chip,.stat,.pi').forEach(el=>{
  el.addEventListener('mouseenter',()=>{ring.style.width='44px';ring.style.height='44px';ring.style.borderColor='rgba(228,223,216,.5)'});
  el.addEventListener('mouseleave',()=>{ring.style.width='28px';ring.style.height='28px';ring.style.borderColor='rgba(228,223,216,.2)'});
});

/* ── Three.js background ── */
const canvas = document.getElementById('three-canvas');
const renderer = new THREE.WebGLRenderer({canvas,alpha:true,antialias:true});
renderer.setPixelRatio(Math.min(window.devicePixelRatio,2));
renderer.setSize(window.innerWidth,window.innerHeight);

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(45,window.innerWidth/window.innerHeight,.1,100);
camera.position.set(0,0,5);

/* Create a wireframe icosahedron */
const geo = new THREE.IcosahedronGeometry(1.6, 1);
const edges = new THREE.EdgesGeometry(geo);
const mat = new THREE.LineBasicMaterial({color:0x3a3530,transparent:true,opacity:.7});
const wireframe = new THREE.LineSegments(edges, mat);
scene.add(wireframe);

/* Inner sphere */
const sphereGeo = new THREE.IcosahedronGeometry(1.1, 1);
const sphereEdges = new THREE.EdgesGeometry(sphereGeo);
const sphereMat = new THREE.LineBasicMaterial({color:0x28241e,transparent:true,opacity:.5});
const innerSphere = new THREE.LineSegments(sphereEdges, sphereMat);
scene.add(innerSphere);

/* Orbit ring */
const ringGeo = new THREE.TorusGeometry(2.4, 0.004, 2, 120);
const ringMat = new THREE.MeshBasicMaterial({color:0x302c26,transparent:true,opacity:.4});
const orbitRing = new THREE.Mesh(ringGeo, ringMat);
orbitRing.rotation.x = Math.PI/2.4;
scene.add(orbitRing);

/* Dot particles */
const dotGeo = new THREE.BufferGeometry();
const dotCount = 140;
const positions = new Float32Array(dotCount*3);
for(let i=0;i<dotCount;i++){
  const r=2.8+Math.random()*.8;
  const theta=Math.random()*Math.PI*2;
  const phi=Math.acos(2*Math.random()-1);
  positions[i*3]  =r*Math.sin(phi)*Math.cos(theta);
  positions[i*3+1]=r*Math.sin(phi)*Math.sin(theta);
  positions[i*3+2]=r*Math.cos(phi);
}
dotGeo.setAttribute('position',new THREE.BufferAttribute(positions,3));
const dotMat = new THREE.PointsMaterial({color:0x4a4438,size:.018,transparent:true,opacity:.8});
const dots = new THREE.Points(dotGeo, dotMat);
scene.add(dots);

/* Mouse parallax */
let targetX=0,targetY=0;
document.addEventListener('mousemove',e=>{
  targetX=(e.clientX/window.innerWidth-.5)*.5;
  targetY=(e.clientY/window.innerHeight-.5)*.5;
});

let time=0;
function animate(){
  requestAnimationFrame(animate);
  time+=.005;
  wireframe.rotation.y += .003;
  wireframe.rotation.x += .001;
  innerSphere.rotation.y -= .004;
  innerSphere.rotation.z += .002;
  orbitRing.rotation.z += .001;
  dots.rotation.y += .0008;

  /* Breathing scale */
  const s = 1 + Math.sin(time)*.03;
  wireframe.scale.set(s,s,s);

  /* Parallax */
  scene.rotation.y += (targetX - scene.rotation.y)*.04;
  scene.rotation.x += (targetY - scene.rotation.x)*.04;

  renderer.render(scene,camera);
}
animate();

window.addEventListener('resize',()=>{
  camera.aspect=window.innerWidth/window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth,window.innerHeight);
});

/* ── Cert table ── */
const certs=[
  ['01','JPMorgan Chase &amp; Co.','Quantitative Research','Quant Modeling'],
  ['02','BCG X','Generative AI Experience','AI Strategy'],
  ['03','BCG X','Data Science Experience','ML Pipelines'],
  ['04','Quantium','Data Analytics','Segmentation'],
  ['05','British Airways','Data Science','Predictive Analytics'],
  ['06','Deloitte','Data Analytics','Forensic Analysis'],
  ['07','TATA Group','Gen AI-Powered Analytics','AI Reporting'],
  ['08','Lloyds Banking Group','Data Science','Financial Modelling'],
  ['09','Commonwealth Bank','Intro to Data Science','EDA &amp; Data Literacy'],
  ['10','Electronic Arts','Product Management','Data-Driven Product'],
];
const tb=document.getElementById('ctbody');
certs.forEach(([n,firm,prog,domain],i)=>{
  const tr=document.createElement('tr');
  tr.style.opacity='0';
  tr.style.transition=`opacity .4s ${i*.05}s`;
  tr.innerHTML=`<td style="font-family:var(--mono);font-size:.65rem;color:var(--ink-4)">${n}</td><td>${firm}</td><td style="color:#9a9080">${prog}</td><td><span class="pill">${domain}</span></td>`;
  tb.appendChild(tr);
  setTimeout(()=>tr.style.opacity='1', 600+i*60);
});

/* ── Counter animation ── */
function count(el,to,suffix,dur=1600){
  let start=null;
  function step(ts){
    if(!start)start=ts;
    const p=Math.min((ts-start)/dur,1);
    const v=Math.round(p*to);
    el.textContent=v+suffix;
    if(p<1)requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}
setTimeout(()=>{
  count(document.getElementById('cn'),10,'');
  count(document.getElementById('wn'),80,'%');
},500);

/* ── 3D card tilt on mouse ── */
const card=document.getElementById('codeCard');
card.addEventListener('mousemove',e=>{
  const r=card.getBoundingClientRect();
  const x=(e.clientX-r.left)/r.width-.5;
  const y=(e.clientY-r.top)/r.height-.5;
  card.querySelector('.code-inner').style.transform=
    `perspective(1000px) rotateX(${-y*10}deg) rotateY(${x*10}deg) translateZ(8px)`;
});
card.addEventListener('mouseleave',()=>{
  card.querySelector('.code-inner').style.transform='perspective(1000px) rotateX(2deg) rotateY(-1deg)';
});
</script>
</body>
</html>
