<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Electrobrain Freelancer Desk</title>
<style>
:root{--nav:#14213d;--blue:#2457e6;--bg:#f3f5fa;--text:#172033;--muted:#718096;--border:#e4e9f2;--green:#12865e;--red:#d94350}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font-family:Arial,sans-serif}
header{background:var(--nav);color:white;padding:16px;display:flex;justify-content:space-between;gap:10px}
header h2{margin:0;font-size:20px}
header small{color:#c3d0e8}
.layout{display:flex;min-height:90vh}
nav{width:200px;background:var(--nav);padding:10px;flex-shrink:0}
nav button{display:block;width:100%;text-align:left;background:transparent;color:white;border:0;padding:12px;border-radius:7px;margin:3px 0}
nav button.active,nav button:hover{background:#304b7d}
main{flex:1;min-width:0;padding:18px}
.page{display:none}.page.active{display:block}
h1{font-size:24px;margin:0 0 7px}
.sub,.muted{color:var(--muted)}
.panel,.card{background:white;padding:16px;border-radius:11px;margin-bottom:15px;box-shadow:0 3px 14px #15264a0b}
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px;margin:15px 0}
.card small{color:var(--muted)}.card h2{font-size:20px;overflow-wrap:anywhere}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:10px}
label{font-size:13px;font-weight:bold}
input,select,textarea{width:100%;padding:11px;border:1px solid var(--border);border-radius:7px;margin:6px 0 12px;font:inherit;background:white}
textarea{min-height:65px}
.btn{background:var(--blue);color:white;border:0;border-radius:7px;padding:10px 13px;margin:3px;cursor:pointer}
.green{background:var(--green)}.red{background:var(--red)}.gray{background:#e9eef8;color:#253c70}
.tablewrap{overflow-x:auto}
table{border-collapse:collapse;width:100%;min-width:600px}
th,td{text-align:left;padding:10px;border-bottom:1px solid var(--border);font-size:13px}
th{color:var(--muted);background:#f8faff}
.badge{padding:5px 8px;border-radius:15px;background:#edf1f8;font-size:11px}
.Paid{background:#d9f5e9;color:#08774e}.Unpaid{background:#fff0d9;color:#946000}.Overdue{background:#ffe0e1;color:#a51e2b}
.hidden{display:none!important}
#toast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%);background:#172033;color:white;padding:12px;border-radius:8px;display:none}
footer{text-align:center;padding:20px;color:var(--muted);font-size:12px}
@media(max-width:700px){.layout{display:block}nav{width:100%;display:flex;overflow-x:auto;position:sticky;top:0;z-index:5}nav button{width:auto;white-space:nowrap}main{padding:12px}}
@media print{body *{visibility:hidden!important}#printArea,#printArea *{visibility:visible!important}#printArea{display:block!important;position:absolute;top:0;left:0;width:100%;padding:20px}}
</style>
</head>
<body>

<header>
<div><h2>⚡ Electrobrain</h2><small>Freelancer Desk · Local Edition</small></div>
<strong id="brandName">My Business</strong>
</header>

<div class="layout">
<nav>
<button class="active" onclick="page('dashboard',this)">▦ Dashboard</button>
<button onclick="page('contacts',this)">♙ Contacts</button>
<button onclick="page('catalog',this)">▣ Products & Services</button>
<button onclick="page('quotes',this)">▤ Quotes</button>
<button onclick="page('invoices',this)">▤ Invoices</button>
<button onclick="page('expenses',this)">↗ Expenses</button>
<button onclick="page('reports',this)">◷ Reports</button>
<button onclick="page('settings',this)">⚙ Settings & Backup</button>
<button onclick="page('help',this)">? Help</button>
</nav>

<main>

<section id="dashboard" class="page active">
<h1>Dashboard</h1><p class="sub">Your business overview.</p>
<div class="cards">
<div class="card"><small>Total invoiced</small><h2 id="dInvoiced">0</h2></div>
<div class="card"><small>Payments received</small><h2 id="dReceived">0</h2></div>
<div class="card"><small>Outstanding</small><h2 id="dOutstanding">0</h2></div>
<div class="card"><small>Expenses</small><h2 id="dExpenses">0</h2></div>
<div class="card"><small>Cash balance</small><h2 id="dProfit">0</h2></div>
<div class="card"><small>Contacts</small><h2 id="dContacts">0</h2></div>
</div>
<div class="panel"><h3>Payment reminders</h3><div id="reminderList"></div></div>
<div class="panel"><h3>Recent invoices</h3><div class="tablewrap"><table><thead><tr><th>Number</th><th>Client</th><th>Total</th><th>Status</th></tr></thead><tbody id="recent"></tbody></table></div></div>
<button class="btn" onclick="page('contacts')">Add contact</button>
<button class="btn green" onclick="newInvoice()">Create invoice</button>
<button class="btn gray" onclick="page('quotes')">Create quote</button>
<button class="btn gray" onclick="page('expenses')">Record expense</button>
</section>

<section id="contacts" class="page">
<h1>Contacts</h1><p class="sub">Customers and suppliers.</p>
<div class="panel">
<h3 id="contactHeading">Add contact</h3>
<form id="contactForm">
<input type="hidden" id="contactId">
<div class="grid">
<div><label>Name *</label><input id="contactName" required></div>
<div><label>Type</label><select id="contactType"><option>Customer</option><option>Supplier</option><option>Both</option></select></div>
<div><label>Company</label><input id="contactCompany"></div>
<div><label>Email</label><input id="contactEmail" type="email"></div>
<div><label>Phone</label><input id="contactPhone"></div>
</div>
<label>Address</label><textarea id="contactAddress"></textarea>
<button class="btn">Save contact</button><button type="button" class="btn gray" onclick="resetContact()">Clear</button>
</form>
</div>
<div class="panel"><h3>Contact list</h3><input id="contactSearch" placeholder="Search contacts..." oninput="renderContacts()"><div class="tablewrap"><table><thead><tr><th>Name</th><th>Type</th><th>Company</th><th>Phone</th><th>Email</th><th>Actions</th></tr></thead><tbody id="contactRows"></tbody></table></div></div>
</section>

<section id="catalog" class="page">
<h1>Products & Services</h1><p class="sub">Save frequently used items and prices.</p>
<div class="panel">
<h3 id="itemHeading">Add product or service</h3>
<form id="itemForm">
<input type="hidden" id="itemId">
<div class="grid">
<div><label>Item name *</label><input id="itemName" required></div>
<div><label>Type</label><select id="itemType"><option>Service</option><option>Product</option></select></div>
<div><label>Unit price *</label><input id="itemPrice" type="number" min="0" step=".01" required></div>
</div>
<button class="btn">Save item</button><button type="button" class="btn gray" onclick="resetItem()">Clear</button>
</form>
</div>
<div class="panel"><h3>Saved items</h3><div class="tablewrap"><table><thead><tr><th>Name</th><th>Type</th><th>Price</th><th>Actions</th></tr></thead><tbody id="itemRows"></tbody></table></div></div>
</section>

<section id="quotes" class="page">
<h1>Quotes & Estimates</h1><p class="sub">Prepare estimates before starting work.</p>
<div class="panel">
<h3 id="quoteHeading">Create quote</h3>
<form id="quoteForm">
<input type="hidden" id="quoteId">
<div class="grid">
<div><label>Quote number</label><input id="quoteNumber" required></div>
<div><label>Customer *</label><input id="quoteClientName" list="customerList" required placeholder="Type customer name"><datalist id="customerList"></datalist></div>
<div><label>Date</label><input type="date" id="quoteDate" required></div>
<div><label>Valid until</label><input type="date" id="quoteValid"></div>
<div><label>Description *</label><input id="quoteDescription" required></div>
<div><label>Quantity</label><input type="number" id="quoteQty" min=".01" step="any" value="1"></div>
<div><label>Unit price</label><input type="number" id="quotePrice" min="0" step=".01" required></div>
<div><label>Discount amount</label><input type="number" id="quoteDiscount" min="0" step=".01" value="0"></div>
<div><label>Tax percentage</label><input type="number" id="quoteTax" min="0" max="100" step=".01" value="0"></div>
</div>
<p><b>Quote total: <span id="quoteTotal">0</span></b></p>
<button class="btn">Save quote</button><button type="button" class="btn gray" onclick="resetQuote()">Clear</button>
</form>
</div>
<div class="panel"><h3>Quote records</h3><div class="tablewrap"><table><thead><tr><th>Quote</th><th>Client</th><th>Date</th><th>Total</th><th>Status</th><th>Actions</th></tr></thead><tbody id="quoteRows"></tbody></table></div></div>
</section>

<section id="invoices" class="page">
<h1>Invoices</h1><p class="sub">Create invoices and record payments.</p>
<div class="panel">
<h3 id="invoiceHeading">Create invoice</h3>
<form id="invoiceForm">
<input type="hidden" id="invoiceId">
<div class="grid">
<div><label>Invoice number</label><input id="invoiceNumber" required></div>
<div><label>Customer *</label><input id="invoiceClientName" list="customerList" required placeholder="Type or select customer"></div>
<div><label>Invoice date</label><input type="date" id="invoiceDate" required></div>
<div><label>Due date</label><input type="date" id="invoiceDue"></div>
<div><label>Description *</label><input id="invoiceDescription" required></div>
<div><label>Quantity</label><input type="number" id="invoiceQty" min=".01" step="any" value="1" required></div>
<div><label>Unit price</label><input type="number" id="invoicePrice" min="0" step=".01" required></div>
<div><label>Discount amount</label><input type="number" id="invoiceDiscount" min="0" step=".01" value="0"></div>
<div><label>Tax percentage</label><input type="number" id="invoiceTax" min="0" max="100" step=".01" value="0"></div>
</div>
<p><b>Invoice total: <span id="invoiceTotal">0</span></b></p>
<button class="btn">Save invoice</button><button type="button" class="btn gray" onclick="resetInvoice()">Clear</button>
</form>
</div>
<div class="panel"><h3>Invoice records</h3><input id="invoiceSearch" placeholder="Search invoice or customer..." oninput="renderInvoices()"><div class="tablewrap"><table><thead><tr><th>Invoice</th><th>Client</th><th>Date</th><th>Total</th><th>Paid</th><th>Balance</th><th>Status</th><th>Actions</th></tr></thead><tbody id="invoiceRows"></tbody></table></div></div>
</section>

<section id="expenses" class="page">
<h1>Expenses</h1><p class="sub">Record business spending.</p>
<div class="panel">
<h3 id="expenseHeading">Add expense</h3>
<form id="expenseForm">
<input type="hidden" id="expenseId">
<div class="grid">
<div><label>Description *</label><input id="expenseDescription" required></div>
<div><label>Category</label><select id="expenseCategory"><option>Equipment</option><option>Transport</option><option>Internet</option><option>Software</option><option>Office</option><option>Marketing</option><option>Utilities</option><option>Other</option></select></div>
<div><label>Amount *</label><input type="number" id="expenseAmount" min=".01" step=".01" required></div>
<div><label>Date</label><input type="date" id="expenseDate" required></div>
</div>
<button class="btn">Save expense</button><button type="button" class="btn gray" onclick="resetExpense()">Clear</button>
</form>
</div>
<div class="panel"><h3>Expense records</h3><div class="tablewrap"><table><thead><tr><th>Description</th><th>Category</th><th>Date</th><th>Amount</th><th>Actions</th></tr></thead><tbody id="expenseRows"></tbody></table></div></div>
</section>

<section id="reports" class="page">
<h1>Reports</h1><p class="sub">Your financial overview.</p>
<div class="cards">
<div class="card"><small>Payments received</small><h2 id="rReceived">0</h2></div>
<div class="card"><small>Expenses</small><h2 id="rExpenses">0</h2></div>
<div class="card"><small>Cash balance</small><h2 id="rBalance">0</h2></div>
<div class="card"><small>Outstanding</small><h2 id="rOutstanding">0</h2></div>
</div>
<div class="panel">
<p>Invoices: <b id="rInvoices">0</b></p><p>Quotes: <b id="rQuotes">0</b></p><p>Contacts: <b id="rContacts">0</b></p><p>Expenses recorded: <b id="rExpenseCount">0</b></p><p>Unpaid invoices: <b id="rUnpaid">0</b></p>
<button class="btn" onclick="csv('invoices')">Export invoices CSV</button>
<button class="btn" onclick="csv('expenses')">Export expenses CSV</button>
<button class="btn" onclick="csv('contacts')">Export contacts CSV</button>
</div>
</section>

<section id="settings" class="page">
<h1>Settings & Backup</h1>
<div class="panel">
<h3>Business profile</h3>
<label>Business name</label><input id="sName">
<label>Business email</label><input id="sEmail" type="email">
<label>Business phone</label><input id="sPhone">
<label>Business address</label><textarea id="sAddress"></textarea>
<label>Currency</label>
<select id="sCurrency"><option>GHS</option><option>USD</option><option>GBP</option><option>EUR</option><option>NGN</option><option>KES</option><option>ZAR</option></select>
<button class="btn" onclick="saveSettings()">Save profile</button>
</div>
<div class="panel">
<h3>Data backup</h3><button class="btn green" onclick="backup()">Download backup</button>
<label>Restore backup file</label><input type="file" id="restoreFile" accept=".json">
<button class="btn gray" onclick="restore()">Restore backup</button>
</div>
<div class="panel"><h3>Demo records</h3><button class="btn gray" onclick="demo()">Load demo data</button></div>
<div class="panel"><h3>Danger zone</h3><button class="btn red" onclick="wipe()">Delete all app data</button></div>
</section>

<section id="help" class="page">
<h1>Help & Information</h1>
<div class="panel"><h3>Getting started</h3><ol><li>Enter your business details.</li><li>Add customers in Contacts.</li><li>Create quotes or invoices.</li><li>Record payments.</li><li>Record expenses.</li><li>Download backups regularly.</li></ol></div>
<div class="panel"><h3>Important information</h3><p>This version stores records in this browser on this device. It does not provide cloud synchronization or automatic messaging. Use the backup feature regularly. Check records carefully before relying on financial totals.</p></div>
<div class="panel"><h3>About</h3><p>Electrobrain Freelancer Desk · Version 3 Local Edition</p></div>
</section>

</main>
</div>

<footer>Electrobrain Freelancer Desk · Local Edition</footer>
<div id="toast"></div>
<div id="printArea" class="hidden"></div>

<script>
const KEY="electrobrain_desk_v3";
let db={contacts:[],items:[],quotes:[],invoices:[],expenses:[],settings:{name:"My Business",email:"",phone:"",address:"",currency:"GHS"}};
const $=id=>document.getElementById(id);
const uid=()=>Date.now().toString(36)+Math.random().toString(36).slice(2,7);
const today=()=>new Date().toISOString().slice(0,10);
const num=x=>Number(x)||0;

function esc(v){return String(v??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function msg(t){$("toast").textContent=t;$("toast").style.display="block";setTimeout(()=>$("toast").style.display="none",2300)}
function money(n){try{return new Intl.NumberFormat("en",{style:"currency",currency:db.settings.currency||"GHS"}).format(num(n))}catch(e){return (db.settings.currency||"GHS")+" "+num(n).toFixed(2)}}
function load(){try{let x=JSON.parse(localStorage.getItem(KEY)||"null");if(x)db={...db,...x,settings:{...db.settings,...(x.settings||{})}}}catch(e){msg("Could not read saved data.")}}
function save(){try{localStorage.setItem(KEY,JSON.stringify(db));renderAll()}catch(e){msg("Storage full. Download a backup.")}}
function page(name,button){document.querySelectorAll(".page").forEach(x=>x.classList.remove("active"));$(name).classList.add("active");document.querySelectorAll("nav button").forEach(x=>x.classList.remove("active"));if(button)button.classList.add("active");renderAll()}
function person(cid){return db.contacts.find(c=>c.id===cid)}
function personName(cid){return person(cid)?.name||"Contact unavailable"}
function total(q,p,d,t){let b=Math.max(0,num(q)*num(p)-num(d));return b+b*num(t)/100}
function balance(i){return Math.max(0,num(i.total)-num(i.paid))}
function status(i){return balance(i)<=0?"Paid":i.due&&i.due<today()?"Overdue":"Unpaid"}
function badge(s){return `<span class="badge ${s}">${s}</span>`}

function findOrCreateCustomer(name){
 name=name.trim();
 let found=db.contacts.find(c=>c.name.toLowerCase()===name.toLowerCase()&&c.type!=="Supplier");
 if(found)return found;
 let c={id:uid(),name,type:"Customer",company:"",email:"",phone:"",address:""};
 db.contacts.push(c);return c;
}
function options(){
 $("customerList").innerHTML=db.contacts.filter(c=>c.type!=="Supplier").map(c=>`<option value="${esc(c.name)}"></option>`).join("");
}
function renderAll(){options();renderDashboard();renderContacts();renderItems();renderQuotes();renderInvoices();renderExpenses();renderReports();loadSettings()}
function renderDashboard(){
 let invoiced=db.invoices.reduce((s,i)=>s+num(i.total),0);
 let received=db.invoices.reduce((s,i)=>s+num(i.paid),0);
 let outstanding=db.invoices.reduce((s,i)=>s+balance(i),0);
 let expenses=db.expenses.reduce((s,e)=>s+num(e.amount),0);
 $("dInvoiced").textContent=money(invoiced);$("dReceived").textContent=money(received);
 $("dOutstanding").textContent=money(outstanding);$("dExpenses").textContent=money(expenses);
 $("dProfit").textContent=money(received-expenses);$("dContacts").textContent=db.contacts.length;
 $("recent").innerHTML=db.invoices.length?[...db.invoices].reverse().slice(0,5).map(i=>`<tr><td>${esc(i.number)}</td><td>${esc(personName(i.client))}</td><td>${money(i.total)}</td><td>${badge(status(i))}</td></tr>`).join(""):'<tr><td colspan="4">No invoices yet.</td></tr>';
 let soon=new Date(Date.now()+3*86400000).toISOString().slice(0,10);
 let reminders=db.invoices.filter(i=>balance(i)>0&&i.due&&i.due<=soon);
 $("reminderList").innerHTML=reminders.length?reminders.map(i=>`<p><b>${esc(i.number)}</b> — ${esc(personName(i.client))} · Due ${esc(i.due)} · ${money(balance(i))} <button class="btn sm" onclick="remind('${i.id}')">Remind</button></p>`).join(""):"<p class='muted'>No upcoming payment reminders.</p>";
}
function renderContacts(){
 let q=($("contactSearch").value||"").toLowerCase();
 $("contactRows").innerHTML=db.contacts.filter(c=>(c.name+c.company+c.phone+c.email).toLowerCase().includes(q)).map(c=>`<tr><td>${esc(c.name)}</td><td>${esc(c.type)}</td><td>${esc(c.company)}</td><td>${esc(c.phone)}</td><td>${esc(c.email)}</td><td><button class="btn sm" onclick="editContact('${c.id}')">Edit</button><button class="btn red sm" onclick="del('contacts','${c.id}')">Delete</button></td></tr>`).join("")||'<tr><td colspan="6">No contacts found.</td></tr>';
}
function resetContact(){$("contactForm").reset();$("contactId").value="";$("contactHeading").textContent="Add contact"}
$("contactForm").onsubmit=e=>{
 e.preventDefault();let cid=$("contactId").value;
 let c={id:cid||uid(),name:$("contactName").value.trim(),type:$("contactType").value,company:$("contactCompany").value,email:$("contactEmail").value,phone:$("contactPhone").value,address:$("contactAddress").value};
 if(cid)db.contacts[db.contacts.findIndex(x=>x.id===cid)]=c;else db.contacts.push(c);
 resetContact();save();msg("Contact saved");
};
function editContact(cid){let c=person(cid);if(!c)return;$("contactId").value=c.id;$("contactName").value=c.name;$("contactType").value=c.type;$("contactCompany").value=c.company;$("contactEmail").value=c.email;$("contactPhone").value=c.phone;$("contactAddress").value=c.address;$("contactHeading").textContent="Edit contact";page("contacts")}
function renderItems(){$("itemRows").innerHTML=db.items.map(i=>`<tr><td>${esc(i.name)}</td><td>${esc(i.type)}</td><td>${money(i.price)}</td><td><button class="btn sm" onclick="editItem('${i.id}')">Edit</button><button class="btn red sm" onclick="del('items','${i.id}')">Delete</button></td></tr>`).join("")||'<tr><td colspan="4">No saved items.</td></tr>'}
function resetItem(){$("itemForm").reset();$("itemId").value="";$("itemHeading").textContent="Add product or service"}
$("itemForm").onsubmit=e=>{e.preventDefault();let iid=$("itemId").value;let x={id:iid||uid(),name:$(
