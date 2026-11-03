const GROUPS={
 build:{name:"Building & home repairs",color:"#B8321F"},
 power:{name:"Power & electronics",color:"#2D6FB7"},
 auto:{name:"Auto & transport",color:"#1C1F1A"},
 style:{name:"Fashion & beauty",color:"#C2408A"},
 home:{name:"Home & cleaning",color:"#156B50"},
 events:{name:"Events & food",color:"#D69A00"}
};
// [name, group, min price ₦, unit]
const T=[
 ["Electrician","build",8000,"per visit"],["Plumber","build",7000,"per visit"],["Carpenter","build",15000,"per job"],
 ["Bricklayer","build",12000,"per day"],["Tiler","build",1500,"per m²"],["Painter","build",900,"per m²"],
 ["POP Ceiling Installer","build",3500,"per m²"],["Welder","build",15000,"per job"],["Aluminium Fabricator","build",45000,"per window"],
 ["Roofer","build",25000,"per job"],["Borehole Driller","build",450000,"per borehole"],["Interlock Paver","build",4500,"per m²"],
 ["Furniture Maker","build",60000,"per piece"],["Upholsterer","build",20000,"per seat"],["Screeding & Wall Finisher","build",1200,"per m²"],
 ["Solar Installer","power",250000,"per setup"],["Inverter Technician","power",10000,"per visit"],["AC Technician","power",8000,"per unit"],
 ["Generator Repairer","power",7000,"per visit"],["CCTV Installer","power",40000,"per setup"],["Satellite TV Installer","power",6000,"per visit"],
 ["Phone Repairer","power",5000,"per repair"],["Laptop Technician","power",8000,"per repair"],["Fridge & Freezer Technician","power",9000,"per visit"],
 ["Washing Machine Repairer","power",9000,"per visit"],
 ["Auto Mechanic","auto",10000,"per visit"],["Vulcanizer","auto",1500,"per tyre"],["Panel Beater","auto",20000,"per job"],
 ["Auto Electrician","auto",10000,"per visit"],["Car Wash & Detailer","auto",4000,"per car"],["Keke & Okada Mechanic","auto",4000,"per visit"],
 ["Tailor","style",12000,"per outfit"],["Fashion Designer","style",35000,"per outfit"],["Shoemaker","style",18000,"per pair"],
 ["Hairstylist","style",8000,"per style"],["Barber","style",2500,"per cut"],["Makeup Artist","style",20000,"per face"],
 ["Nail Technician","style",6000,"per set"],["Gele Tier","style",5000,"per gele"],["Bead Maker","style",15000,"per set"],
 ["Cleaner","home",15000,"per day"],["Fumigator","home",25000,"per home"],["Laundry & Dry Cleaner","home",1500,"per item"],
 ["Gardener","home",10000,"per visit"],["Water Tank Cleaner","home",12000,"per tank"],["Mover & Haulage","home",40000,"per move"],
 ["Caterer","events",5000,"per head"],["Cake Baker","events",25000,"per cake"],["Event Decorator","events",80000,"per event"],
 ["Photographer","events",50000,"per event"],["DJ","events",60000,"per event"],["MC & Compere","events",70000,"per event"]
];
const slug=s=>s.toLowerCase().replace(/&/g,"").replace(/[^a-z0-9]+/g,"-").replace(/^-|-$/g,"");
const TRADES=T.map(([name,group,price,unit])=>({name,group,price,unit,slug:slug(name)}));
const CITIES=["Lagos","Abuja","Port Harcourt","Ibadan","Kano","Enugu","Benin City","Kaduna","Owerri","Uyo","Abeokuta","Jos","Ilorin","Calabar","Warri","Onitsha","Asaba","Akure"];
const AREAS={"Lagos":["Ikeja","Yaba","Lekki","Surulere","Ikorodu","Ajah","Festac","Ogba"],"Abuja":["Wuse","Garki","Kubwa","Gwarinpa","Lugbe","Maitama"],"Port Harcourt":["GRA","Rumuokoro","Trans-Amadi","D-Line","Eliozu"],"Ibadan":["Bodija","Ring Road","Challenge","Dugbe","Akobo"],"Kano":["Nassarawa","Sabon Gari","Tarauni","Gwale"],"Enugu":["Independence Layout","New Haven","Trans-Ekulu","Abakpa"]};
const FIRST=["Chinedu","Adebayo","Musa","Ngozi","Funmilayo","Ibrahim","Emeka","Aisha","Tunde","Blessing","Yusuf","Chiamaka","Segun","Halima","Obinna","Kemi","Efosa","Amaka","Bello","Ifeoma","Sola","Zainab","Uche","Bisi"];
const LAST=["Okafor","Adeyemi","Abubakar","Eze","Balogun","Lawal","Nwosu","Okonkwo","Bello","Ogunleye","Danjuma","Osagie","Ibe","Afolabi","Yakubu","Akpan"];
const COLORS=["#0F4D3A","#B8321F","#2D6FB7","#C2408A","#D69A00","#1C1F1A"];
const naira=n=>"₦"+Number(n).toLocaleString("en-NG");
const $=s=>document.querySelector(s);
function setHash(h){try{history.replaceState(null,"",h)}catch(e){try{location.hash=h}catch(_){}}}
const esc=s=>String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]));

// storage (this device only)
const store={get(k,d){try{return JSON.parse(localStorage.getItem(k))??d}catch(e){return d}},set(k,v){try{localStorage.setItem(k,JSON.stringify(v))}catch(e){}}};
let requests=store.get("skilarr-requests",[]);
let myPros=store.get("skilarr-pros",[]);

function rng(seed){let h=2166136261;for(const c of seed)h=Math.imul(h^c.charCodeAt(0),16777619);return()=>{h=Math.imul(h^(h>>>15),2246822507);h=Math.imul(h^(h>>>13),3266489909);return((h^=h>>>16)>>>0)/4294967296}}
function samplePros(trade,city){
  const r=rng(trade.slug+city),out=[];
  for(let i=0;i<6;i++){
    const f=FIRST[Math.floor(r()*FIRST.length)],l=LAST[Math.floor(r()*LAST.length)];
    const areas=AREAS[city]||["Central","GRA","New Layout","Old Town"];
    out.push({id:trade.slug+"-"+city+"-"+i,name:f+" "+l,trade:trade.name,city,area:areas[Math.floor(r()*areas.length)],
      years:2+Math.floor(r()*18),price:Math.round(trade.price*(0.85+r()*0.7)/500)*500||trade.price,
      verified:r()>.3,langs:["English",["Pidgin","Yoruba","Igbo","Hausa"][Math.floor(r()*4)]],sample:true,
      color:COLORS[Math.floor(r()*COLORS.length)]});
  }
  return out;
}

// populate selects
const cityOpts=CITIES.map(c=>`<option>${c}</option>`).join("");
$("#qCity").innerHTML=cityOpts;$("#joinCity").innerHTML='<option value="">Choose city</option>'+cityOpts;
$("#joinTrade").innerHTML='<option value="">Choose trade</option>'+TRADES.map(t=>`<option value="${t.slug}">${t.name}</option>`).join("");
$("#tradeList").innerHTML=TRADES.map(t=>`<option value="${t.name}">`).join("");
$("#quick").innerHTML=["AC Technician","Hairstylist","Auto Mechanic","Fumigator","Cake Baker"].map(n=>`<button type="button" data-t="${slug(n)}">${n}</button>`).join("");

// trade grid
let group="all";
function renderChips(){
  $("#groupChips").innerHTML=[["all","All trades"],...Object.entries(GROUPS).map(([k,v])=>[k,v.name])].map(([k,n])=>`<button class="chip" aria-pressed="${k===group}" data-g="${k}">${n}</button>`).join("");
}
function renderTrades(){
  const q=$("#tradeFilter").value.trim().toLowerCase();
  const list=TRADES.filter(t=>(group==="all"||t.group===group)&&(!q||t.name.toLowerCase().includes(q)));
  $("#tradeGrid").innerHTML=list.length?list.map(t=>`<button class="trade" data-t="${t.slug}"><span class="bar" style="background:${GROUPS[t.group].color}"></span><span class="tn">${t.name}</span><span class="tg">${GROUPS[t.group].name}</span><span class="tp">From ${naira(t.price)} ${t.unit}</span></button>`).join("")
    :`<div class="empty" style="grid-column:1/-1">No trade matches "${esc(q)}". Try a shorter word, like "repair" or "fashion".</div>`;
}
$("#groupChips").addEventListener("click",e=>{const b=e.target.closest("[data-g]");if(!b)return;group=b.dataset.g;renderChips();renderTrades()});
$("#tradeFilter").addEventListener("input",renderTrades);
renderChips();renderTrades();

// trade view
let current=null,currentCity="Lagos";
function openTrade(s,city){
  const t=TRADES.find(x=>x.slug===s);if(!t)return;
  current=t;if(city)currentCity=city;
  const mine=myPros.filter(p=>p.tradeSlug===t.slug&&p.city===currentCity);
  const pros=[...mine,...samplePros(t,currentCity)];
  $("#tradeViewInner").innerHTML=`
    <button class="back" id="backBtn">Back to all trades</button>
    <div class="tv-head">
      <div><h2>${t.name}s in ${currentCity}</h2><p style="margin:8px 0 0;color:var(--muted)">${GROUPS[t.group].name}. Typical starting price ${naira(t.price)} ${t.unit}.</p></div>
      <label style="font-weight:700;font-size:15px">City <select class="city" id="tvCity">${CITIES.map(c=>`<option ${c===currentCity?"selected":""}>${c}</option>`).join("")}</select></label>
    </div>
    <p class="note">Profiles marked "Sample" are placeholders that show how listings will look. They are not real artisans.</p>
    <div class="pros">${pros.map(p=>`
      <article class="pro">
        <div class="top"><div class="av" style="background:${p.color}">${esc(p.name.split(" ").map(w=>w[0]).join("").slice(0,2))}</div>
          <div><h3>${esc(p.name)}</h3><div class="meta">${esc(p.area?p.area+", ":"")}${esc(p.city)}</div></div></div>
        <div class="tags">${p.verified?'<span class="tag v">Verified</span>':""}${p.sample?'<span class="tag">Sample</span>':'<span class="tag me">Added on this device</span>'}<span class="tag">${p.years} yrs experience</span>${(p.langs||[]).map(l=>`<span class="tag">${l}</span>`).join("")}</div>
        <div class="price">From ${naira(p.price)} <span style="font-weight:400;color:var(--muted)">${t.unit}</span></div>
        <button class="btn" data-hire="${esc(p.id)}">Request this artisan</button>
      </article>`).join("")}</div>`;
  $("#tradeView").classList.add("open");
  window._pros=pros;
  if(location.hash!=="#"+t.slug)setHash("#"+t.slug);
  $("#tradeView").scrollIntoView({behavior:matchMedia("(prefers-reduced-motion: reduce)").matches?"auto":"smooth"});
}
$("#tradeViewInner").addEventListener("change",e=>{if(e.target.id==="tvCity")openTrade(current.slug,e.target.value)});
$("#tradeViewInner").addEventListener("click",e=>{
  if(e.target.id==="backBtn"){$("#tradeView").classList.remove("open");setHash("#trades");$("#trades").scrollIntoView();return}
  const h=e.target.closest("[data-hire]");if(h)openHire(window._pros.find(p=>p.id===h.dataset.hire));
});
document.addEventListener("click",e=>{const b=e.target.closest("[data-t]");if(b)openTrade(b.dataset.t)});
$("#heroSearch").addEventListener("submit",e=>{
  e.preventDefault();
  const q=$("#qTrade").value.trim().toLowerCase();
  const t=TRADES.find(x=>x.name.toLowerCase()===q)||TRADES.find(x=>x.name.toLowerCase().includes(q)&&q);
  if(t)openTrade(t.slug,$("#qCity").value);
  else{$("#tradeFilter").value=$("#qTrade").value;group="all";renderChips();renderTrades();$("#trades").scrollIntoView()}
});

// dialog
const scrim=$("#scrim"),dlg=$("#dialog");let lastFocus=null;
function openDialog(html){lastFocus=document.activeElement;dlg.innerHTML=`<button class="x" aria-label="Close" id="xBtn">×</button>`+html;scrim.classList.add("open");setTimeout(()=>dlg.querySelector("input,textarea,button:not(.x)")?.focus(),30)}
function closeDialog(){scrim.classList.remove("open");lastFocus?.focus()}
scrim.addEventListener("click",e=>{if(e.target===scrim||e.target.id==="xBtn")closeDialog()});
document.addEventListener("keydown",e=>{if(e.key==="Escape"&&scrim.classList.contains("open"))closeDialog()});

function openHire(p){
  openDialog(`<h3>Request ${esc(p.name.split(" ")[0])}</h3><p style="margin:0;color:var(--muted)">${esc(p.trade)} in ${esc(p.area||"")}${p.area?", ":""}${esc(p.city)}. Starting from ${naira(p.price)}.</p>
  <form class="card" id="hireForm" novalidate>
    <label>Your name<input name="name" required autocomplete="name"></label>
    <label>Phone number<input name="phone" required inputmode="tel" autocomplete="tel"></label>
    <label class="full">What do you need done?<textarea name="job" required placeholder="e.g. Kitchen tap is leaking and the sink drains slowly"></textarea></label>
    <label>Address or area<input name="addr" required></label>
    <label>Preferred date<input name="date" type="date" required></label>
    <label class="full">Your budget (₦, optional)<input name="budget" type="number" min="0" step="500"></label>
    <p class="err" id="hireErr">Fill in your name, phone, job description, address and date to send the request.</p>
    <button class="btn" type="submit">Send request</button>
  </form>`);
  $("#hireForm").addEventListener("submit",e=>{
    e.preventDefault();const f=e.target;
    if(!f.checkValidity()){$("#hireErr").style.display="block";return}
    const d=Object.fromEntries(new FormData(f));
    requests.unshift({...d,pro:p.name,trade:p.trade,city:p.city,at:new Date().toISOString()});
    store.set("skilarr-requests",requests);updateCount();
    dlg.innerHTML=`<button class="x" aria-label="Close" id="xBtn">×</button><h3>Request saved</h3><p>Your request to ${esc(p.name)} for "${esc(d.job.slice(0,80))}" is in My requests.</p><p style="color:var(--muted);font-size:15px">In this preview it is stored on this device only, so the artisan is not contacted yet. In the live version they would get it by SMS and reply with a quote.</p><button class="btn" id="seeReqs">See my requests</button>`;
    $("#seeReqs").onclick=showRequests;
  });
}
function updateCount(){$("#reqCount").textContent=requests.length}
function showRequests(){
  openDialog(`<h3>My requests</h3>${requests.length?`<div class="reqs">${requests.map(r=>`<div class="req"><b>${esc(r.trade)}: ${esc(r.pro)}</b><div class="meta">${esc(r.addr)}, ${esc(r.city)} · for ${esc(r.date)}${r.budget?" · budget "+naira(r.budget):""}</div><div>${esc(r.job)}</div><span class="status">Waiting for quote</span></div>`).join("")}</div>`
   :`<p>You have not requested anyone yet. Pick a trade, choose an artisan and tap "Request this artisan".</p><button class="btn" id="goTrades">Browse trades</button>`}`);
  $("#goTrades")?.addEventListener("click",()=>{closeDialog();$("#trades").scrollIntoView()});
}
$("#openReqs").addEventListener("click",e=>{e.preventDefault();showRequests()});
updateCount();

// join
$("#joinForm").addEventListener("submit",e=>{
  e.preventDefault();const f=e.target;
  if(!f.checkValidity()){$("#joinErr").style.display="block";$("#joinOk").style.display="none";return}
  $("#joinErr").style.display="none";
  const d=Object.fromEntries(new FormData(f));const t=TRADES.find(x=>x.slug===d.trade);
  const p={id:"me-"+Date.now(),name:d.name,trade:t.name,tradeSlug:t.slug,city:d.city,area:d.area,years:+d.years,price:+d.price,verified:false,langs:[],sample:false,color:"#D69A00"};
  myPros.unshift(p);store.set("skilarr-pros",myPros);
  $("#joinOk").innerHTML=`Profile created. You now appear first under ${esc(t.name)}s in ${esc(d.city)}. <button type="button" class="btn alt" data-t="${t.slug}" style="margin-top:10px">View my listing</button>`;
  currentCity=d.city;$("#joinOk").style.display="block";f.reset();
});

// hash routing
function route(){const h=location.hash.slice(1);if(TRADES.some(t=>t.slug===h))openTrade(h)}
window.addEventListener("hashchange",route);route();
