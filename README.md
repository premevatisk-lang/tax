<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>เรียงเอกสารตามวันที่</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<style>
@import url('https://fonts.googleapis.com/css2?family=Special+Elite&family=IBM+Plex+Sans:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500;600&display=swap');

:root{
  --paper: #EAE6DB;
  --paper-card: #F6F3EA;
  --ink: #26314A;
  --ink-soft: #5B6478;
  --stamp: #A2402C;
  --stamp-dark: #7C2F20;
  --line: #C7BFA9;
  --teal: #4F6E67;
  --warn-bg: #F1DFCF;
  --good-bg: #E1E7DC;
}

*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{
  background:
    radial-gradient(ellipse at top left, rgba(255,255,255,0.35), transparent 55%),
    var(--paper);
  color:var(--ink);
  font-family:'IBM Plex Sans', sans-serif;
  min-height:100vh;
  padding:32px 20px 60px;
}

.app{max-width:1080px;margin:0 auto;}

header{
  border-bottom:2px solid var(--ink);
  padding-bottom:18px;
  margin-bottom:28px;
}
header h1{
  font-family:'Special Elite', cursive;
  font-weight:400;
  font-size:30px;
  letter-spacing:0.5px;
  margin:0 0 6px;
  color:var(--ink);
}
header p{margin:0;color:var(--ink-soft);font-size:14px;max-width:600px;line-height:1.5;}
.eyebrow{
  font-family:'IBM Plex Mono', monospace;
  font-size:11px;
  letter-spacing:2px;
  color:var(--stamp);
  text-transform:uppercase;
  margin:0 0 4px;
}

main{display:grid;grid-template-columns:1fr;gap:28px;}

.dropzone{
  border:2px dashed var(--line);
  border-radius:4px;
  background:var(--paper-card);
  padding:26px;
  text-align:center;
  cursor:pointer;
  transition:border-color .15s ease, background .15s ease;
}
.dropzone.drag{border-color:var(--stamp);background:var(--warn-bg);}
.dropzone svg{opacity:.55;margin-bottom:8px;}
.dropzone .title{font-weight:600;font-size:15px;margin-bottom:4px;}
.dropzone .sub{font-size:12.5px;color:var(--ink-soft);font-family:'IBM Plex Mono',monospace;}
#fileInput{display:none;}

.progress-wrap{display:flex;align-items:center;gap:10px;margin-top:14px;}
.progress-bar{flex:1;height:8px;background:var(--line);border-radius:4px;overflow:hidden;}
.progress-fill{height:100%;width:0%;background:var(--stamp);transition:width .25s ease;border-radius:4px;}
.progress-label{font-family:'IBM Plex Mono',monospace;font-size:12px;color:var(--ink-soft);min-width:40px;text-align:right;}

.queue{margin-top:14px;display:flex;flex-direction:column;gap:8px;}
.queue-item{
  display:flex;align-items:center;gap:12px;
  background:var(--paper-card);
  border:1px solid var(--line);
  border-radius:3px;
  padding:10px 12px;
  font-size:13px;
}
.queue-item .name{flex:1;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;font-weight:500;}
.queue-item .status{font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--ink-soft);white-space:nowrap;}
.spinner{
  width:13px;height:13px;border-radius:50%;
  border:2px solid var(--line);border-top-color:var(--stamp);
  animation:spin .7s linear infinite;flex-shrink:0;
}
@keyframes spin{to{transform:rotate(360deg);}}

.section-title{
  font-family:'Special Elite', cursive;
  font-size:18px;font-weight:400;
  margin:0 0 4px;
}
.section-sub{font-size:13px;color:var(--ink-soft);margin:0 0 16px;}

.timeline-wrap{
  background:var(--paper-card);
  border:1px solid var(--line);
  border-radius:4px;
  padding:28px 24px 20px;
  overflow-x:auto;
}
.timeline-track{
  position:relative;
  height:2px;
  background:var(--line);
  margin:38px 10px 46px;
  min-width:600px;
}
.timeline-track .end-label{
  position:absolute;top:14px;font-family:'IBM Plex Mono',monospace;font-size:11px;color:var(--ink-soft);
}
.timeline-track .end-label.start{left:0;}
.timeline-track .end-label.end{right:0;text-align:right;}

.marker{
  position:absolute;top:50%;transform:translate(-50%,-50%);
  display:flex;flex-direction:column;align-items:center;
  cursor:default;
}
.stamp{
  font-family:'Special Elite', cursive;
  font-size:10.5px;
  color:var(--stamp);
  border:1.5px solid var(--stamp);
  border-radius:3px;
  padding:2px 6px;
  transform:rotate(-5deg);
  white-space:nowrap;
  background:rgba(246,243,234,0.9);
  animation:stampIn .35s ease-out;
}
@keyframes stampIn{
  0%{opacity:0;transform:rotate(-5deg) scale(2.4);}
  60%{opacity:1;}
  100%{opacity:1;transform:rotate(-5deg) scale(1);}
}
.marker .dot{width:9px;height:9px;border-radius:50%;background:var(--ink);margin-top:6px;border:2px solid var(--paper-card);}
.marker .fname{
  margin-top:5px;font-size:10px;color:var(--ink-soft);max-width:100px;
  overflow:hidden;text-overflow:ellipsis;white-space:nowrap;text-align:center;
}

.empty-note{font-size:13px;color:var(--ink-soft);font-style:italic;text-align:center;padding:20px 0;}

table{width:100%;border-collapse:collapse;background:var(--paper-card);border:1px solid var(--line);border-radius:4px;overflow:hidden;}
thead th{
  text-align:left;font-family:'IBM Plex Mono',monospace;font-size:11px;letter-spacing:1px;
  text-transform:uppercase;color:var(--ink-soft);
  border-bottom:1px solid var(--line);padding:10px 12px;
}
tbody td{padding:9px 12px;border-bottom:1px solid var(--line);font-size:13.5px;vertical-align:middle;}
tbody tr:last-child td{border-bottom:none;}
tbody tr:hover{background:rgba(0,0,0,0.02);}
.order-badge{
  font-family:'IBM Plex Mono',monospace;font-weight:600;font-size:12px;
  background:var(--ink);color:var(--paper-card);
  border-radius:3px;width:22px;height:22px;display:inline-flex;align-items:center;justify-content:center;
}
.badge{
  font-family:'IBM Plex Mono',monospace;font-size:10.5px;padding:2px 7px;border-radius:10px;
}
.badge.auto{background:var(--good-bg);color:var(--teal);}
.badge.missing{background:var(--warn-bg);color:var(--stamp-dark);}
.datecell{font-family:'IBM Plex Mono',monospace;font-size:13px;}
.srccell{font-size:12px;color:var(--ink-soft);}

.actions{display:flex;gap:12px;margin-top:20px;flex-wrap:wrap;}
button.primary, button.secondary{
  font-family:'IBM Plex Sans',sans-serif;font-weight:600;font-size:14px;
  border-radius:3px;padding:11px 20px;cursor:pointer;border:1.5px solid var(--stamp);
  transition:transform .1s ease;
}
button.primary{background:var(--stamp);color:#fff;}
button.primary:hover{background:var(--stamp-dark);}
button.secondary{background:transparent;color:var(--stamp);}
button.secondary:hover{background:var(--warn-bg);}
button:disabled{opacity:.4;cursor:not-allowed;}
button:active:not(:disabled){transform:scale(0.98);}
button.ready{animation:readyPulse 1.4s ease-in-out 2;}
@keyframes readyPulse{
  0%,100%{box-shadow:0 0 0 0 rgba(162,64,44,0.35);}
  50%{box-shadow:0 0 0 8px rgba(162,64,44,0);}
}

.note{font-size:12.5px;color:var(--ink-soft);margin-top:10px;}

footer{margin-top:34px;font-size:11.5px;color:var(--ink-soft);text-align:center;font-family:'IBM Plex Mono',monospace;}
</style>
</head>
<body>
<div class="app">

  <header>
    <p class="eyebrow">Document Timeline Utility</p>
    <h1>เรียงเอกสารตามวันที่</h1>
    <p>อัปโหลด PDF ได้ทั้งแบบหลายไฟล์ หรือไฟล์เดียวที่รวมเอกสารหลายฉบับไว้ข้างใน (เช่นสแกนใบเสร็จหลายใบต่อกันเป็นไฟล์เดียว) ระบบจะอ่านทีละหน้า แยกว่าหน้าไหนเป็นเอกสารฉบับไหน หาวันที่ของแต่ละฉบับ แล้วเรียงหน้าใหม่ให้ทั้งไฟล์โดยอัตโนมัติ</p>
  </header>

  <main>
    <section>
      <div class="dropzone" id="dropzone">
        <svg width="30" height="30" viewBox="0 0 24 24" fill="none" stroke="var(--ink)" stroke-width="1.6"><path d="M12 3v12m0 0l-4-4m4 4l4-4M4 17v2a2 2 0 002 2h12a2 2 0 002-2v-2"/></svg>
        <div class="title">ลากไฟล์ PDF มาวางที่นี่ หรือคลิกเพื่อเลือกไฟล์</div>
        <div class="sub">รองรับทั้งหลายไฟล์ และไฟล์เดียวที่มีหลายเอกสารข้างใน · .pdf เท่านั้น</div>
        <input type="file" id="fileInput" accept="application/pdf,.pdf" multiple />
      </div>
      <div class="progress-wrap" id="progressWrap" style="display:none;">
        <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
        <div class="progress-label" id="progressLabel">0%</div>
      </div>
      <div class="queue" id="queue"></div>
    </section>

    <section>
      <p class="section-title">เส้นเวลาเอกสาร</p>
      <p class="section-sub">แต่ละจุดคือเอกสาร 1 ฉบับที่ตรวจพบ เรียงจากซ้าย (เก่าสุด) ไปขวา (ใหม่สุด)</p>
      <div class="timeline-wrap">
        <div class="timeline-track" id="timelineTrack"></div>
        <div id="timelineEmpty" class="empty-note">ยังไม่มีเอกสารที่อ่านวันที่ได้</div>
      </div>
    </section>

    <section>
      <p class="section-title">รายการเรียงลำดับ</p>
      <p class="section-sub">เรียงอัตโนมัติจากวันที่ที่พบในแต่ละเอกสาร เอกสารที่หาวันที่ไม่เจอจะถูกวางไว้ท้ายรายการ</p>
      <table>
        <thead>
          <tr><th>#</th><th>เอกสาร</th><th>ที่มา</th><th>วันที่ที่พบ</th><th>สถานะ</th></tr>
        </thead>
        <tbody id="resultsBody">
          <tr><td colspan="5" class="empty-note">ยังไม่มีไฟล์ที่ประมวลผลแล้ว</td></tr>
        </tbody>
      </table>
      <div class="actions">
        <button class="primary" id="downloadMerged" disabled>ดาวน์โหลด PDF รวม (เรียงแล้ว)</button>
        <button class="secondary" id="downloadZip" disabled>ดาวน์โหลด ZIP (แยกเอกสาร เรียงเลขนำหน้า)</button>
      </div>
      <p class="note" id="ocrNote" style="display:none;"></p>
      <p class="note" id="missingNote" style="display:none;"></p>
    </section>
  </main>

  <footer>ประมวลผลทั้งหมดในเบราว์เซอร์ของคุณ ไม่มีการอัปโหลดไฟล์ขึ้นเซิร์ฟเวอร์ใด ๆ</footer>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf-lib/1.17.1/pdf-lib.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/tesseract.js/5.0.4/tesseract.min.js"></script>
<script>
pdfjsLib.GlobalWorkerOptions.workerSrc = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

/* ---------------- Date extraction ---------------- */
const THAI_MONTHS_FULL = ['มกราคม','กุมภาพันธ์','มีนาคม','เมษายน','พฤษภาคม','มิถุนายน','กรกฎาคม','สิงหาคม','กันยายน','ตุลาคม','พฤศจิกายน','ธันวาคม'];
const THAI_MONTHS_ABBR = ['ม.ค.','ก.พ.','มี.ค.','เม.ย.','พ.ค.','มิ.ย.','ก.ค.','ส.ค.','ก.ย.','ต.ค.','พ.ย.','ธ.ค.'];
const EN_MONTHS_FULL = ['January','February','March','April','May','June','July','August','September','October','November','December'];
const EN_MONTHS_ABBR = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
const KEYWORDS = ['วันที่ออก','วันที่','ลงวันที่','ว/ด/ป','Date','Dated','date'];

function escapeRegex(s){return s.replace(/[.*+?^${}()|[\]\\]/g,'\\$&');}
function pad2(n){return String(n).padStart(2,'0');}

function normalizeYear(y){
  if(y > 2400) return y - 543; // Buddhist -> Gregorian
  return y;
}

function makeCandidate(index, year, month, day, keywordBonus, isMonthName){
  if(month<1||month>12) return null;
  if(day<1||day>31) return null;
  if(year<1990||year>2100) return null;
  return {
    index, year, month, day,
    sortKey: year*10000 + month*100 + day,
    display: `${pad2(day)}/${pad2(month)}/${year}`,
    score: (keywordBonus?1000:0) + (isMonthName?10:0) - index*0.0001
  };
}

function thaiMonthIndex(str){
  let i = THAI_MONTHS_FULL.indexOf(str);
  if(i>=0) return i;
  i = THAI_MONTHS_ABBR.indexOf(str);
  return i;
}
function enMonthIndex(str){
  const clean = str.replace(/\.$/,'');
  let i = EN_MONTHS_FULL.findIndex(m=>m.toLowerCase()===clean.toLowerCase());
  if(i>=0) return i;
  i = EN_MONTHS_ABBR.findIndex(m=>m.toLowerCase()===clean.toLowerCase());
  return i;
}

function hasKeywordNearby(text, idx){
  const context = text.slice(Math.max(0, idx-25), idx);
  return KEYWORDS.some(k => context.includes(k));
}

function findDateCandidates(text){
  const candidates = [];
  const thAlt = [...THAI_MONTHS_FULL, ...THAI_MONTHS_ABBR].map(escapeRegex).join('|');
  const enAlt = [...EN_MONTHS_FULL, ...EN_MONTHS_ABBR].map(escapeRegex).join('|');

  {
    const re = new RegExp(`(\\d{1,2})\\s*(?:เดือน\\s*)?(${thAlt})\\s*(?:พ\\.ศ\\.?\\s*)?(\\d{4})`, 'g');
    let m;
    while((m = re.exec(text))){
      const mi = thaiMonthIndex(m[2]);
      if(mi<0) continue;
      const c = makeCandidate(m.index, normalizeYear(+m[3]), mi+1, +m[1], hasKeywordNearby(text,m.index), true);
      if(c) candidates.push(c);
    }
  }
  {
    // "Month Day, Year" e.g. "July 30, 2026" — also allow hyphens: "Jul-30-2026"
    const re = new RegExp(`(${enAlt})\\.?[\\s\\-]+(\\d{1,2})(?:st|nd|rd|th)?,?[\\s\\-]+(\\d{4})`, 'gi');
    let m;
    while((m = re.exec(text))){
      const mi = enMonthIndex(m[1]);
      if(mi<0) continue;
      const c = makeCandidate(m.index, normalizeYear(+m[3]), mi+1, +m[2], hasKeywordNearby(text,m.index), true);
      if(c) candidates.push(c);
    }
  }
  {
    // "Day Month Year" e.g. "30 July 2026" — also allow hyphens: "10-Jul-2026"
    const re = new RegExp(`(\\d{1,2})(?:st|nd|rd|th)?[\\s\\-]+(${enAlt})\\.?,?[\\s\\-]+(\\d{4})`, 'gi');
    let m;
    while((m = re.exec(text))){
      const mi = enMonthIndex(m[2]);
      if(mi<0) continue;
      const c = makeCandidate(m.index, normalizeYear(+m[3]), mi+1, +m[1], hasKeywordNearby(text,m.index), true);
      if(c) candidates.push(c);
    }
  }
  {
    const re = /(\d{4})-(\d{1,2})-(\d{1,2})/g;
    let m;
    while((m = re.exec(text))){
      const c = makeCandidate(m.index, normalizeYear(+m[1]), +m[2], +m[3], hasKeywordNearby(text,m.index), false);
      if(c) candidates.push(c);
    }
  }
  {
    const re = /(\d{1,2})[\/.\-](\d{1,2})[\/.\-](\d{4})/g;
    let m;
    while((m = re.exec(text))){
      const c = makeCandidate(m.index, normalizeYear(+m[3]), +m[2], +m[1], hasKeywordNearby(text,m.index), false);
      if(c) candidates.push(c);
    }
  }

  return candidates;
}

function bestDate(text){
  const candidates = findDateCandidates(text);
  if(candidates.length===0) return null;
  candidates.sort((a,b)=>b.score-a.score);
  return candidates[0];
}

/* ---------------- Page-group (document) detection ---------------- */
// Detects "หน้าที่ 1 จาก 2" / "หน้า 1/2" / "Page 1 of 2" style markers so
// multi-page documents stay together instead of being split page-by-page.
function parsePageMarker(text){
  let m = text.match(/หน้าที่\s*[:\-]?\s*(\d{1,3})\s*(?:\/|จาก)\s*(\d{1,3})/);
  if(m) return {cur:+m[1], total:+m[2]};
  m = text.match(/หน้า\s*[:\-]?\s*(\d{1,3})\s*\/\s*(\d{1,3})/);
  if(m) return {cur:+m[1], total:+m[2]};
  m = text.match(/Page\s*[:\-]?\s*(\d{1,3})\s*(?:of|\/)\s*(\d{1,3})/i);
  if(m) return {cur:+m[1], total:+m[2]};
  return null;
}

function groupPagesIntoDocuments(pageTexts){
  const groups = [];
  let i = 0;
  while(i < pageTexts.length){
    const marker = parsePageMarker(pageTexts[i]);
    let end = i;
    if(marker && marker.total > 1 && marker.cur === 1){
      let expected = 2;
      let j = i+1;
      while(j < pageTexts.length){
        const m2 = parsePageMarker(pageTexts[j]);
        if(m2 && m2.total === marker.total && m2.cur === expected){
          end = j; expected++; j++;
        } else break;
      }
    }
    groups.push({pageStart:i, pageEnd:end});
    i = end + 1;
  }
  return groups;
}

/* ---------------- App state ---------------- */
let files = []; // {id, file, name, status, pageTexts, pageCount, error}
let documents = []; // {id, fileId, fileName, pageStart, pageEnd, dateInfo}
let counter = 0;
let docCounter = 0;
const pdfLibCache = new Map();
const pdfJsDocCache = new Map(); // fileId -> loaded pdf.js document (for OCR rendering)
const ocrResults = new Map(); // "fileId:pageStart" -> dateInfo or null (null = attempted, not found)
let ocrWorkerPromise = null;
let ocrRunning = false;
let mergedBlob = null; // pre-built merged PDF, ready once all processing finishes
let mergedBlobVersion = -1; // documentsVersion the cached mergedBlob was built from
let documentsVersion = 0; // bumped every time rebuildDocuments() runs

// Progress tracking across text extraction + OCR fallback
let progress = { textTotal: 0, textDone: 0, ocrTotal: 0, ocrDone: 0 };

async function getOcrWorker(){
  if(!ocrWorkerPromise){
    ocrWorkerPromise = Tesseract.createWorker('tha+eng');
  }
  return ocrWorkerPromise;
}

const dropzone = document.getElementById('dropzone');
const fileInput = document.getElementById('fileInput');
const queueEl = document.getElementById('queue');
const progressWrap = document.getElementById('progressWrap');
const progressFill = document.getElementById('progressFill');
const progressLabel = document.getElementById('progressLabel');
const timelineTrack = document.getElementById('timelineTrack');
const timelineEmpty = document.getElementById('timelineEmpty');
const resultsBody = document.getElementById('resultsBody');
const downloadMergedBtn = document.getElementById('downloadMerged');
const downloadZipBtn = document.getElementById('downloadZip');
const missingNote = document.getElementById('missingNote');

dropzone.addEventListener('click', ()=>fileInput.click());
dropzone.addEventListener('dragover', e=>{e.preventDefault(); dropzone.classList.add('drag');});
dropzone.addEventListener('dragleave', ()=>dropzone.classList.remove('drag'));
dropzone.addEventListener('drop', e=>{
  e.preventDefault(); dropzone.classList.remove('drag');
  handleFiles(e.dataTransfer.files);
});
fileInput.addEventListener('change', e=>{
  handleFiles(e.target.files);
  fileInput.value = '';
});

function handleFiles(fileList){
  const pdfFiles = Array.from(fileList).filter(f=>f.type==='application/pdf' || f.name.toLowerCase().endsWith('.pdf'));
  if(pdfFiles.length===0) return;
  mergedBlob = null;
  mergedBlobVersion = -1;
  downloadMergedBtn.textContent = 'ดาวน์โหลด PDF รวม (เรียงแล้ว)';
  pdfFiles.forEach(file=>{
    const item = {id: counter++, file, name: file.name, status:'pending'};
    files.push(item);
    processFile(item);
  });
  renderQueue();
}

function renderProgress(){
  const total = progress.textTotal + progress.ocrTotal;
  const done = progress.textDone + progress.ocrDone;
  if(total===0){ progressWrap.style.display = 'none'; return; }
  const pct = Math.min(100, Math.round((done/total)*100));
  progressWrap.style.display = 'flex';
  progressFill.style.width = pct + '%';
  progressLabel.textContent = pct + '%';
  if(pct>=100 && !ocrRunning && files.every(f=>f.status==='done'||f.status==='error')){
    progressLabel.textContent = 'เสร็จสิ้น';
    setTimeout(()=>{ if(progress.textDone+progress.ocrDone>=progress.textTotal+progress.ocrTotal) progressWrap.style.display='none'; }, 1800);
  }
}

async function processFile(item){
  item.status = 'processing';
  renderQueue();
  try{
    const buf = await item.file.arrayBuffer();
    const doc = await pdfjsLib.getDocument({data: buf}).promise;
    const total = doc.numPages;
    progress.textTotal += total;
    renderProgress();
    const pageTexts = [];
    for(let p=1; p<=total; p++){
      const page = await doc.getPage(p);
      const content = await page.getTextContent();
      pageTexts.push(content.items.map(it=>it.str).join(' '));
      progress.textDone += 1;
      renderProgress();
    }
    item.pageTexts = pageTexts;
    item.pageCount = total;
    item.status = 'done';
    pdfJsDocCache.set(item.id, doc); // keep for OCR rendering fallback
  }catch(err){
    item.status = 'error';
    item.error = err && err.message ? err.message : 'อ่านไฟล์ไม่สำเร็จ';
  }
  renderQueue();
  rebuildDocuments();
  renderResults();
  scheduleOcrFallback();
}

function rebuildDocuments(){
  documents = [];
  documentsVersion++;
  files.forEach(item=>{
    if(item.status !== 'done') return;
    const groups = groupPagesIntoDocuments(item.pageTexts);
    groups.forEach(g=>{
      const text = item.pageTexts.slice(g.pageStart, g.pageEnd+1).join('\n');
      let dateInfo = bestDate(text);
      let ocr = false;
      if(!dateInfo){
        const cached = ocrResults.get(`${item.id}:${g.pageStart}`);
        if(cached){ dateInfo = cached; ocr = true; }
      }
      documents.push({
        id: docCounter++,
        fileId: item.id,
        fileName: item.name,
        pageStart: g.pageStart,
        pageEnd: g.pageEnd,
        dateInfo,
        ocr
      });
    });
  });
}

// Renders a page to a canvas, optionally cropped to just the top portion.
// heightFraction 1 = full page; 0.55 = top 55% only.
async function renderPageRegion(fileId, pageNum, heightFraction){
  const pdfDoc = pdfJsDocCache.get(fileId);
  if(!pdfDoc) return null;
  const page = await pdfDoc.getPage(pageNum);
  const viewport = page.getViewport({scale: 3});
  const canvas = document.createElement('canvas');
  canvas.width = viewport.width;
  canvas.height = viewport.height;
  await page.render({canvasContext: canvas.getContext('2d'), viewport}).promise;
  if(heightFraction >= 1) return canvas;

  const cropHeight = Math.max(1, Math.floor(canvas.height * heightFraction));
  const cropped = document.createElement('canvas');
  cropped.width = canvas.width;
  cropped.height = cropHeight;
  cropped.getContext('2d').drawImage(canvas, 0, 0, canvas.width, cropHeight, 0, 0, canvas.width, cropHeight);
  return cropped;
}

// Runs after each file finishes text extraction: any document that still has
// no date (typically a scanned page with no text layer) gets OCR'd. First pass
// checks just the top ~55% of the page (fast, avoids stray numbers further
// down); if that finds nothing, a second pass OCRs the full page in case this
// particular document's date sits lower than usual. Results are cached so
// re-running rebuildDocuments() doesn't repeat work.
async function scheduleOcrFallback(){
  if(ocrRunning) return;
  ocrRunning = true;
  try{
    const ocrNote = document.getElementById('ocrNote');
    while(true){
      const remainingTargets = documents.filter(d => !d.dateInfo && !ocrResults.has(`${d.fileId}:${d.pageStart}`));
      if(remainingTargets.length===0) break;
      // Recompute total each pass so the bar reflects newly-discovered targets too
      progress.ocrTotal = progress.ocrDone + remainingTargets.length;
      renderProgress();
      const target = remainingTargets[0];
      ocrNote.style.display = 'block';
      ocrNote.textContent = `กำลังอ่านหน้าที่เป็นภาพสแกนด้วย OCR... (เหลืออีกประมาณ ${remainingTargets.length} หน้า)`;
      try{
        const worker = await getOcrWorker();
        let dateInfo = null;

        const crop = await renderPageRegion(target.fileId, target.pageStart+1, 0.55);
        if(crop){
          const { data } = await worker.recognize(crop);
          dateInfo = bestDate(data.text || '');
        }

        if(!dateInfo){
          // top-of-page crop found nothing — retry with the whole page
          ocrNote.textContent = `หาวันที่ไม่เจอในส่วนบนของหน้า ${target.pageStart+1} กำลังอ่านทั้งหน้าเพิ่มเติม...`;
          const full = await renderPageRegion(target.fileId, target.pageStart+1, 1);
          if(full){
            const { data } = await worker.recognize(full);
            dateInfo = bestDate(data.text || '');
          }
        }

        ocrResults.set(`${target.fileId}:${target.pageStart}`, dateInfo); // cache even null = "attempted"
      }catch(err){
        ocrResults.set(`${target.fileId}:${target.pageStart}`, null);
      }
      progress.ocrDone += 1;
      rebuildDocuments();
      renderResults();
      renderProgress();
    }
    ocrNote.style.display = 'none';
  }finally{
    ocrRunning = false;
    renderProgress();
    maybeAutoBuildMerged();
    updateReconcileButtonState();
  }
}

// Once every file has finished text extraction and every document without a
// text-based date has had its OCR attempt (found or not), pre-build the
// merged, sorted PDF in the background so the download is instant.
async function maybeAutoBuildMerged(){
  const allFilesSettled = files.length>0 && files.every(f=>f.status==='done'||f.status==='error');
  const allOcrSettled = documents.every(d=> d.dateInfo || ocrResults.has(`${d.fileId}:${d.pageStart}`));
  if(!allFilesSettled || !allOcrSettled || ocrRunning) return;
  if(documents.length===0) return;
  const versionAtStart = documentsVersion;
  try{
    const blob = await buildMergedPdfBlob();
    if(documentsVersion !== versionAtStart) return; // data changed mid-build; discard, don't cache
    mergedBlob = blob;
    mergedBlobVersion = versionAtStart;
    downloadMergedBtn.textContent = 'ดาวน์โหลด PDF รวม (จัดเรียงหน้าเสร็จแล้ว)';
    downloadMergedBtn.disabled = false;
    downloadMergedBtn.classList.add('ready');
    setTimeout(()=>downloadMergedBtn.classList.remove('ready'), 3000);
  }catch(err){
    // leave the button in its normal state; the click handler will retry
  }
}

async function buildMergedPdfBlob(){
  const {dated, undated} = sortedDocuments();
  const ordered = [...dated, ...undated];
  const merged = await PDFLib.PDFDocument.create();
  for(const d of ordered){
    const src = await getPdfLibDoc(d.fileId);
    const pages = await merged.copyPages(src, pageRangeArray(d.pageStart, d.pageEnd));
    pages.forEach(p=>merged.addPage(p));
  }
  const bytes = await merged.save();
  return new Blob([bytes], {type:'application/pdf'});
}

function renderQueue(){
  queueEl.innerHTML = '';
  files.forEach(item=>{
    const row = document.createElement('div');
    row.className = 'queue-item';
    let statusHtml = '';
    if(item.status==='pending' || item.status==='processing'){
      statusHtml = `<div class="spinner"></div><span class="status">กำลังอ่าน...</span>`;
    }else if(item.status==='done'){
      const docCount = groupPagesIntoDocuments(item.pageTexts).length;
      statusHtml = `<span class="status">${item.pageCount} หน้า · พบ ${docCount} เอกสาร</span>`;
    }else if(item.status==='error'){
      statusHtml = `<span class="status">✕ ${item.error}</span>`;
    }
    row.innerHTML = `<span class="name">${item.name}</span>${statusHtml}`;
    queueEl.appendChild(row);
  });
}

function docLabel(d){
  const pages = d.pageStart===d.pageEnd ? `หน้า ${d.pageStart+1}` : `หน้า ${d.pageStart+1}-${d.pageEnd+1}`;
  return pages;
}

function sortedDocuments(){
  const dated = documents.filter(d=>d.dateInfo).sort((a,b)=>a.dateInfo.sortKey-b.dateInfo.sortKey);
  const undated = documents.filter(d=>!d.dateInfo);
  return {dated, undated};
}

function renderResults(){
  const {dated, undated} = sortedDocuments();

  // Timeline
  timelineTrack.querySelectorAll('.marker, .end-label').forEach(m=>m.remove());
  if(dated.length===0){
    timelineEmpty.style.display = 'block';
  }else{
    timelineEmpty.style.display = 'none';
    const min = dated[0].dateInfo.sortKey;
    const max = dated[dated.length-1].dateInfo.sortKey;
    const span = max - min;
    dated.forEach((d,i)=>{
      const pct = span===0 ? (dated.length===1?50: (i/(dated.length-1))*100) : ((d.dateInfo.sortKey-min)/span)*100;
      const marker = document.createElement('div');
      marker.className = 'marker';
      marker.style.left = pct+'%';
      marker.innerHTML = `<div class="stamp">${d.dateInfo.display}</div><div class="dot"></div><div class="fname" title="${d.fileName} ${docLabel(d)}">${docLabel(d)}</div>`;
      timelineTrack.appendChild(marker);
    });
    const startLabel = document.createElement('div');
    startLabel.className = 'end-label start';
    startLabel.textContent = 'เก่าสุด';
    const endLabel = document.createElement('div');
    endLabel.className = 'end-label end';
    endLabel.textContent = 'ใหม่สุด';
    timelineTrack.appendChild(startLabel);
    timelineTrack.appendChild(endLabel);
  }

  // Table
  const combined = [...dated, ...undated];
  if(combined.length===0){
    resultsBody.innerHTML = `<tr><td colspan="5" class="empty-note">ยังไม่มีไฟล์ที่ประมวลผลแล้ว</td></tr>`;
  }else{
    const multiFile = files.length > 1;
    resultsBody.innerHTML = combined.map((d,i)=>{
      const isMissing = !d.dateInfo;
      const badge = isMissing
        ? `<span class="badge missing">ไม่พบวันที่</span>`
        : (d.ocr ? `<span class="badge auto">OCR</span>` : `<span class="badge auto">อัตโนมัติ</span>`);
      const dateText = d.dateInfo ? d.dateInfo.display : '—';
      const srcText = multiFile ? `${d.fileName} · ${docLabel(d)}` : docLabel(d);
      return `<tr>
        <td><span class="order-badge">${i+1}</span></td>
        <td>${docLabel(d)}</td>
        <td class="srccell">${multiFile ? d.fileName : '—'}</td>
        <td class="datecell">${dateText}</td>
        <td>${badge}</td>
      </tr>`;
    }).join('');
  }

  if(undated.length>0){
    missingNote.style.display = 'block';
    missingNote.textContent = `เอกสาร ${undated.length} รายการหาวันที่ไม่เจอ ระบบวางไว้ท้ายรายการตามลำดับเดิม (ยังรวมไฟล์/ดาวน์โหลดได้ตามปกติ)`;
  }else{
    missingNote.style.display = 'none';
  }

  downloadMergedBtn.disabled = combined.length===0;
  downloadZipBtn.disabled = combined.length===0;
}

async function getPdfLibDoc(fileId){
  if(!pdfLibCache.has(fileId)){
    const item = files.find(f=>f.id===fileId);
    const buf = await item.file.arrayBuffer();
    const doc = await PDFLib.PDFDocument.load(buf);
    pdfLibCache.set(fileId, doc);
  }
  return pdfLibCache.get(fileId);
}

function pageRangeArray(start,end){
  const arr = [];
  for(let p=start;p<=end;p++) arr.push(p);
  return arr;
}

/* ---------------- Downloads ---------------- */
downloadMergedBtn.addEventListener('click', async ()=>{
  if(mergedBlob && mergedBlobVersion === documentsVersion){
    triggerDownload(mergedBlob, 'sorted-merged.pdf');
    return;
  }
  downloadMergedBtn.disabled = true;
  downloadMergedBtn.textContent = 'กำลังรวมไฟล์...';
  try{
    const versionAtStart = documentsVersion;
    const blob = await buildMergedPdfBlob();
    if(documentsVersion === versionAtStart){
      mergedBlob = blob;
      mergedBlobVersion = versionAtStart;
    }
    triggerDownload(blob, 'sorted-merged.pdf');
  }catch(err){
    alert('รวมไฟล์ไม่สำเร็จ: ' + err.message);
  }
  downloadMergedBtn.textContent = 'ดาวน์โหลด PDF รวม (เรียงแล้ว)';
  renderResults();
});

downloadZipBtn.addEventListener('click', async ()=>{
  downloadZipBtn.disabled = true;
  downloadZipBtn.textContent = 'กำลังสร้าง ZIP...';
  try{
    const {dated, undated} = sortedDocuments();
    const ordered = [...dated, ...undated];
    const zip = new JSZip();
    for(let i=0;i<ordered.length;i++){
      const d = ordered[i];
      const src = await getPdfLibDoc(d.fileId);
      const out = await PDFLib.PDFDocument.create();
      const pages = await out.copyPages(src, pageRangeArray(d.pageStart, d.pageEnd));
      pages.forEach(p=>out.addPage(p));
      const bytes = await out.save();
      const dateTag = d.dateInfo ? d.dateInfo.display.replace(/\//g,'-') : 'no-date';
      zip.file(`${String(i+1).padStart(2,'0')}_${dateTag}.pdf`, bytes);
    }
    const blob = await zip.generateAsync({type:'blob'});
    triggerDownload(blob, 'sorted-documents.zip');
  }catch(err){
    alert('สร้าง ZIP ไม่สำเร็จ: ' + err.message);
  }
  downloadZipBtn.textContent = 'ดาวน์โหลด ZIP (แยกเอกสาร เรียงเลขนำหน้า)';
  renderResults();
});

function triggerDownload(blob, filename){
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = filename;
  document.body.appendChild(a); a.click(); document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

</script>
</body>
</html>
