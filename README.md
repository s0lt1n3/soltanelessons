# soltanelessons
A simple lesson tracking app for managing study materials and notes
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>soltanelessons</title>
<style>
:root{--bg:#f2f2f2;--card:#ffffff;--text:#2b2b2b;--muted:#6f6f6f;--line:#d9d9d9;--accent:#d62828;--accent2:#f77f00;--accent-t:#ffffff;--warn:#f77f00;--ok:#6f6f6f;--bad:#d62828;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font:15px/1.5 system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
.wrap{max-width:900px;margin:0 auto;padding:20px 16px 60px}
header{border-bottom:3px solid var(--accent);padding-bottom:12px;display:flex;flex-wrap:wrap;gap:10px;align-items:center;justify-content:space-between;margin-bottom:16px}
h1{margin:0;font-size:1.5rem;letter-spacing:-.02em}
h1 span{color:var(--accent)}
button,input,select,textarea{font:inherit;color:inherit}
button{cursor:pointer}
.btn{background:var(--card);border:1px solid var(--line);border-radius:10px;padding:8px 14px}
.btn:hover{border-color:var(--accent)}
.btn.primary{background:var(--accent);color:var(--accent-t);border-color:var(--accent);font-weight:600}
.btn.primary:hover{background:var(--accent2);border-color:var(--accent2)}
.btn.danger{color:var(--bad)}
.row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin-bottom:12px}
input,select,textarea{width:100%;background:var(--card);border:1px solid var(--line);border-radius:10px;padding:9px 12px}
input:focus,select:focus,textarea:focus{outline:2px solid var(--accent);outline-offset:-1px}
#q{flex:1;min-width:180px}
.chip{background:var(--card);border:1px solid var(--line);border-radius:999px;padding:5px 12px;font-size:.88rem}
.chip.on{background:var(--accent);color:var(--accent-t);border-color:var(--accent)}
.chip:hover{border-color:var(--accent2)}
.chip small{opacity:.7;margin-left:4px}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:14px}
.card{background:var(--card);border:1px solid var(--line);border-top:3px solid var(--accent2);border-radius:14px;padding:16px;display:flex;flex-direction:column;gap:8px}
.card h3{margin:0;font-size:1.05rem}
.top{display:flex;justify-content:space-between;gap:8px;align-items:center}
.pill{font-size:.75rem;color:var(--muted);border:1px solid var(--line);border-radius:999px;padding:1px 9px}
.st{font-size:.75rem;font-weight:700;border:0;border-radius:6px;padding:3px 9px;background:var(--line)}
.st.s1{color:var(--warn)}.st.s2{color:var(--accent)}.st.s3{color:var(--ok)}
.notes{white-space:pre-wrap;word-break:break-word;color:var(--muted);font-size:.9rem;margin:0}
.tags{display:flex;flex-wrap:wrap;gap:5px}
.tags span{font-size:.75rem;color:var(--muted)}
.acts{display:flex;gap:6px;margin-top:auto;padding-top:8px;border-top:1px solid var(--line);align-items:center}
.acts .btn{padding:4px 10px;font-size:.85rem;text-decoration:none;color:inherit}
.acts .sp{flex:1}
.star.on{color:var(--accent2);border-color:var(--accent2)}
.empty{grid-column:1/-1;text-align:center;color:var(--muted);padding:40px 10px}
dialog{background:var(--card);color:var(--text);border:1px solid var(--line);border-radius:14px;padding:20px;width:min(520px,92vw)}
dialog::backdrop{background:rgba(0,0,0,.55)}
dialog h2{margin:0 0 12px;font-size:1.15rem}
label{display:block;font-size:.82rem;color:var(--muted);margin:10px 0 4px}
.two{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.end{display:flex;justify-content:flex-end;gap:8px;margin-top:16px}
textarea{min-height:110px;resize:vertical}
#json{min-height:200px;font-family:ui-monospace,Menlo,monospace;font-size:.8rem}
.msg{font-size:.85rem;margin-top:8px;min-height:1.2em}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>soltane<span>lessons</span></h1>
    <div class="row" style="margin:0">
      <button class="btn" id="bBackup">Backup</button>
      <button class="btn primary" id="bNew">+ New lesson</button>
    </div>
  </header>

  <div class="row"><input id="q" type="search" placeholder="Search lessons, notes, tags..."></div>
  <div class="row" id="cats"></div>
  <div class="row" id="stats"></div>
  <div class="grid" id="list"></div>
</div>

<dialog id="dLesson">
  <h2 id="lhead">New lesson</h2>
  <label for="fTitle">Title</label>
  <input id="fTitle" maxlength="120">
  <div class="two">
    <div><label for="fCat">Category</label><select id="fCat"></select></div>
    <div><label for="fStatus">Status</label>
      <select id="fStatus"><option value="1">To study</option><option value="2">Studying</option><option value="3">Done</option></select></div>
  </div>
  <label for="fTags">Tags (comma separated)</label>
  <input id="fTags" placeholder="exam, formulas">
  <label for="fUrl">Link (optional, http or https)</label>
  <input id="fUrl" placeholder="https://...">
  <label for="fNotes">Notes</label>
  <textarea id="fNotes" placeholder="Key points, formulas, things to remember..."></textarea>
  <div class="msg" id="lmsg" style="color:var(--bad)"></div>
  <div class="end"><button class="btn" id="lCancel">Cancel</button><button class="btn primary" id="lSave">Save</button></div>
</dialog>

<dialog id="dCat">
  <h2>New category</h2>
  <input id="cName" maxlength="40" placeholder="e.g. Physics">
  <div class="msg" id="cmsg" style="color:var(--bad)"></div>
  <div class="end"><button class="btn" id="cCancel">Cancel</button><button class="btn primary" id="cSave">Add</button></div>
</dialog>

<dialog id="dAsk">
  <h2 id="askText">Are you sure?</h2>
  <div class="end"><button class="btn" id="askNo">Cancel</button><button class="btn danger" id="askYes">Yes, delete</button></div>
</dialog>

<dialog id="dBackup">
  <h2>Backup</h2>
  <p class="msg" style="color:var(--muted)">Copy this text and keep it somewhere safe. To restore, paste a backup here and press Import.</p>
  <textarea id="json" spellcheck="false"></textarea>
  <div class="msg" id="bmsg"></div>
  <div class="end"><button class="btn" id="bClose">Close</button><button class="btn" id="bCopy">Select all</button><button class="btn primary" id="bImport">Import</button></div>
</dialog>

<script>
const $ = s => document.querySelector(s);
const STATUS = {1:'To study', 2:'Studying', 3:'Done'};

const store = {
  get(k, d){ try{ const v = localStorage.getItem(k); return v ? JSON.parse(v) : d; }catch(e){ return d; } },
  set(k, v){ try{ localStorage.setItem(k, JSON.stringify(v)); }catch(e){} }
};

let cats = store.get('sl_cats', null);
let lessons = store.get('sl_lessons', null);
if(!Array.isArray(cats) || !cats.every(c => typeof c === 'string')) cats = ['General','Math','Science','Languages'];
if(!Array.isArray(lessons)) lessons = [];
lessons = lessons.filter(l => l && typeof l.title === 'string');
if(!cats.includes('General')) cats.unshift('General');

const state = { cat:'all', status:'all', q:'' };
let editId = null;

function save(){ store.set('sl_cats', cats); store.set('sl_lessons', lessons); }

function h(tag, attrs, ...kids){
  const e = document.createElement(tag);
  for(const [k,v] of Object.entries(attrs || {})){
    if(k === 'class') e.className = v;
    else if(k.startsWith('on')) e.addEventListener(k.slice(2), v);
    else e.setAttribute(k, v);
  }
  kids.flat().forEach(c => e.append(c));
  return e;
}

function safeUrl(u){
  if(!u) return '';
  try{ const x = new URL(u); return (x.protocol === 'http:' || x.protocol === 'https:') ? x.href : ''; }
  catch(e){ return ''; }
}

function ask(text, yes, label){
  $('#askText').textContent = text;
  $('#askYes').textContent = label || 'Yes, delete';
  $('#askYes').onclick = () => { $('#dAsk').close(); yes(); };
  $('#dAsk').showModal();
}

function render(){
  // category chips
  const box = $('#cats'); box.replaceChildren();
  const chip = (val, label, count) => h('button', { class:'chip' + (state.cat === val ? ' on' : ''), onclick:() => { state.cat = val; render(); } },
    label, h('small', {}, String(count)));
  box.append(chip('all', 'All', lessons.length));
  box.append(chip('fav', '★ Favorites', lessons.filter(l => l.fav).length));
  cats.forEach(c => box.append(chip(c, c, lessons.filter(l => l.cat === c).length)));
  box.append(h('button', { class:'chip', onclick:openCat }, '+ Category'));
  if(cats.includes(state.cat) && state.cat !== 'General'){
    box.append(h('button', { class:'chip', style:'color:var(--bad)', onclick:() => delCat(state.cat) }, 'Delete "' + state.cat + '"'));
  }

  // status tabs
  const sb = $('#stats'); sb.replaceChildren();
  [['all','Any status'], ['1','To study'], ['2','Studying'], ['3','Done']].forEach(([v, t]) =>
    sb.append(h('button', { class:'chip' + (String(state.status) === v ? ' on' : ''), onclick:() => { state.status = v; render(); } }, t)));

  // lessons
  const q = state.q.toLowerCase();
  const items = lessons.filter(l => {
    const okCat = state.cat === 'all' || (state.cat === 'fav' ? l.fav : l.cat === state.cat);
    const okSt = state.status === 'all' || String(l.status) === state.status;
    const okQ = !q || l.title.toLowerCase().includes(q) || (l.notes || '').toLowerCase().includes(q) || (l.tags || []).some(t => t.toLowerCase().includes(q));
    return okCat && okSt && okQ;
  });

  const list = $('#list'); list.replaceChildren();
  if(!items.length){
    list.append(h('div', { class:'empty' }, lessons.length ? 'No lessons match these filters.' : 'No lessons yet. Press "+ New lesson" to add your first one.'));
    return;
  }
  items.forEach(l => {
    const url = safeUrl(l.url);
    const s = Number(l.status) || 1;
    list.append(h('div', { class:'card' },
      h('div', { class:'top' },
        h('span', { class:'pill' }, l.cat),
        h('button', { class:'st s' + s, title:'Click to change status', onclick:() => { l.status = s % 3 + 1; save(); render(); } }, STATUS[s])),
      h('h3', {}, l.title),
      l.notes ? h('p', { class:'notes' }, l.notes) : '',
      (l.tags && l.tags.length) ? h('div', { class:'tags' }, l.tags.map(t => h('span', {}, '#' + t))) : '',
      h('div', { class:'acts' },
        h('button', { class:'btn star' + (l.fav ? ' on' : ''), title:'Favorite', onclick:() => { l.fav = !l.fav; save(); render(); } }, l.fav ? '★' : '☆'),
        url ? h('a', { class:'btn', href:url, target:'_blank', rel:'noopener noreferrer' }, 'Open link') : '',
        h('span', { class:'sp' }),
        h('button', { class:'btn', onclick:() => openLesson(l.id) }, 'Edit'),
        h('button', { class:'btn danger', onclick:() => ask('Delete "' + l.title + '"?', () => { lessons = lessons.filter(x => x.id !== l.id); save(); render(); }) }, 'Delete'))
    ));
  });
}

function fillCats(sel){
  const s = $('#fCat'); s.replaceChildren();
  cats.forEach(c => s.append(h('option', { value:c }, c)));
  s.value = sel && cats.includes(sel) ? sel : (cats.includes(state.cat) ? state.cat : cats[0]);
}

function openLesson(id){
  editId = id || null;
  const l = id ? lessons.find(x => x.id === id) : null;
  $('#lhead').textContent = l ? 'Edit lesson' : 'New lesson';
  $('#fTitle').value = l ? l.title : '';
  fillCats(l && l.cat);
  $('#fStatus').value = l ? String(l.status || 1) : '1';
  $('#fTags').value = l ? (l.tags || []).join(', ') : '';
  $('#fUrl').value = l ? (l.url || '') : '';
  $('#fNotes').value = l ? (l.notes || '') : '';
  $('#lmsg').textContent = '';
  $('#dLesson').showModal();
  $('#fTitle').focus();
}

function saveLesson(){
  const title = $('#fTitle').value.trim();
  const rawUrl = $('#fUrl').value.trim();
  if(!title){ $('#lmsg').textContent = 'Please enter a title.'; return; }
  if(rawUrl && !safeUrl(rawUrl)){ $('#lmsg').textContent = 'The link must start with http:// or https://'; return; }
  const data = {
    title, cat: $('#fCat').value, status: Number($('#fStatus').value),
    tags: $('#fTags').value.split(',').map(t => t.trim()).filter(Boolean),
    url: rawUrl, notes: $('#fNotes').value.trim()
  };
  if(editId){
    const l = lessons.find(x => x.id === editId);
    if(l) Object.assign(l, data);
  }else{
    lessons.unshift({ id: Date.now().toString(36) + Math.random().toString(36).slice(2, 6), fav:false, ...data });
  }
  save(); render(); $('#dLesson').close();
}

function openCat(){ $('#cName').value = ''; $('#cmsg').textContent = ''; $('#dCat').showModal(); $('#cName').focus(); }

function saveCat(){
  const name = $('#cName').value.trim();
  if(!name){ $('#cmsg').textContent = 'Please enter a name.'; return; }
  if(cats.some(c => c.toLowerCase() === name.toLowerCase())){ $('#cmsg').textContent = 'That category already exists.'; return; }
  cats.push(name); state.cat = name; save(); render(); $('#dCat').close();
}

function delCat(name){
  const n = lessons.filter(l => l.cat === name).length;
  ask('Delete category "' + name + '"?' + (n ? ' Its ' + n + ' lesson(s) will move to General.' : ''), () => {
    lessons.forEach(l => { if(l.cat === name) l.cat = 'General'; });
    cats = cats.filter(c => c !== name);
    state.cat = 'all'; save(); render();
  });
}

function openBackup(){
  $('#json').value = JSON.stringify({ cats, lessons }, null, 2);
  $('#bmsg').textContent = ''; $('#dBackup').showModal();
}

function importBackup(){
  const msg = $('#bmsg');
  try{
    const d = JSON.parse($('#json').value);
    const nc = (d.cats || d.subjects || []).map(c => typeof c === 'string' ? c : c && c.name).filter(Boolean);
    const nl = (d.lessons || []).filter(l => l && typeof l.title === 'string').map(l => ({
      id: String(l.id || Date.now().toString(36) + Math.random().toString(36).slice(2, 6)),
      title: l.title, cat: l.cat || l.subject || 'General',
      status: Number(l.status) || ({'To Study':1,'In Progress':2,'Completed':3}[l.status] || 1),
      tags: Array.isArray(l.tags) ? l.tags.map(String) : [],
      url: safeUrl(l.url), notes: String(l.notes || l.content || ''), fav: !!(l.fav || l.favorite)
    }));
    if(!nl.length && !nc.length) throw new Error('empty');
    cats = [...new Set(['General', ...nc, ...nl.map(l => l.cat)])];
    lessons = nl; state.cat = 'all'; state.status = 'all';
    save(); render();
    msg.style.color = 'var(--ok)'; msg.textContent = 'Imported ' + nl.length + ' lesson(s).';
  }catch(e){
    msg.style.color = 'var(--bad)'; msg.textContent = 'That is not a valid backup.';
  }
}

$('#bNew').onclick = () => openLesson();
$('#bBackup').onclick = openBackup;
$('#lCancel').onclick = () => $('#dLesson').close();
$('#lSave').onclick = saveLesson;
$('#cCancel').onclick = () => $('#dCat').close();
$('#cSave').onclick = saveCat;
$('#cName').addEventListener('keydown', e => { if(e.key === 'Enter') saveCat(); });
$('#askNo').onclick = () => $('#dAsk').close();
$('#bClose').onclick = () => $('#dBackup').close();
$('#bCopy').onclick = () => { $('#json').focus(); $('#json').select(); };
$('#bImport').onclick = importBackup;
$('#q').addEventListener('input', e => { state.q = e.target.value; render(); });

render();
</script>
</body>
</html>
