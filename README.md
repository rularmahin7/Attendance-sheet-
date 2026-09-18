# Attendance-sheet-
Attendance sheet for stuff 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>StaffTrack — Attendance Management</title>
<style>
:root{--bg:#f4f7fb;--card:#fff;--text:#172033;--muted:#718096;--primary:#2563eb;--border:#e5e7eb;--green:#16a34a;--red:#dc2626;--orange:#ea580c;--purple:#7c3aed;--sidebar:#111827}
*{box-sizing:border-box;font-family:Inter,Arial,sans-serif}body{margin:0;background:var(--bg);color:var(--text)}button,input,select{font:inherit}
.app{display:flex;min-height:100vh}.sidebar{width:245px;background:var(--sidebar);color:#fff;padding:25px 15px;position:fixed;height:100vh;left:0;top:0}.logo{font-size:21px;font-weight:800;padding:0 12px 30px}.logo span{color:#60a5fa}.menu{list-style:none;padding:0}.menu li{margin-bottom:6px}.menu button{width:100%;border:0;background:transparent;color:#cbd5e1;text-align:left;padding:13px;border-radius:8px;cursor:pointer}.menu button:hover,.menu button.active{background:#1e293b;color:#fff}
.main{margin-left:245px;width:calc(100% - 245px)}.topbar{height:70px;background:#fff;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;padding:0 28px}.topbar h2{font-size:20px}.date-box{color:var(--muted);font-size:14px}.content{padding:28px}.page{display:none}.page.active{display:block}
.cards{display:grid;grid-template-columns:repeat(4,1fr);gap:18px;margin-bottom:25px}.card{background:#fff;border:1px solid var(--border);border-radius:12px;padding:20px}.card-title{color:var(--muted);font-size:13px;margin-bottom:10px}.card-number{font-size:29px;font-weight:800}.green{color:var(--green)}.red{color:var(--red)}.purple{color:var(--purple)}
.toolbar{display:flex;justify-content:space-between;gap:12px;margin-bottom:18px;flex-wrap:wrap}input,select{padding:11px 13px;border:1px solid var(--border);border-radius:8px;outline:none;background:#fff;font-size:14px}input:focus,select:focus{border-color:var(--primary)}button.primary{background:var(--primary);color:#fff;border:0;padding:11px 17px;border-radius:8px;cursor:pointer;font-weight:600}button.primary:hover{background:#1d4ed8}.table-card{background:#fff;border:1px solid var(--border);border-radius:12px;overflow-x:auto}table{width:100%;border-collapse:collapse;min-width:850px}th{background:#f8fafc;text-align:left;font-size:12px;color:#64748b;text-transform:uppercase;padding:14px 16px;border-bottom:1px solid var(--border)}td{padding:14px 16px;border-bottom:1px solid var(--border);font-size:14px}tr:last-child td{border-bottom:0}
.status{display:inline-block;padding:5px 9px;border-radius:20px;font-size:12px;font-weight:700}.status.present{background:#dcfce7;color:#15803d}.status.absent{background:#fee2e2;color:#b91c1c}.status.leave{background:#ede9fe;color:#6d28d9}.status.late{background:#ffedd5;color:#c2410c}.status.half{background:#fef3c7;color:#a16207}.action-btn{border:0;background:transparent;cursor:pointer;margin-right:8px}.edit{color:var(--primary)}.delete{color:var(--red)}
.modal{position:fixed;inset:0;background:#0008;display:none;align-items:center;justify-content:center;padding:20px;z-index:100}.modal.show{display:flex}.modal-box{background:#fff;width:100%;max-width:500px;border-radius:14px;padding:25px}.modal-header{display:flex;justify-content:space-between;margin-bottom:22px}.close{border:0;background:transparent;font-size:23px;cursor:pointer}.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:15px}.form-group{display:flex;flex-direction:column;gap:7px}.form-group.full{grid-column:1/-1}.form-group label{font-size:13px;font-weight:600}.form-actions{display:flex;justify-content:flex-end;gap:10px;margin-top:22px}.secondary{background:#e5e7eb;border:0;padding:11px 17px;border-radius:8px;cursor:pointer}.empty{text-align:center;padding:40px;color:var(--muted)}.mobile-menu{display:none;border:0;background:transparent;font-size:23px;cursor:pointer}
@media(max-width:900px){.sidebar{transform:translateX(-100%);transition:.25s;z-index:50}.sidebar.open{transform:translateX(0)}.main{margin-left:0;width:100%}.mobile-menu{display:block}.cards{grid-template-columns:repeat(2,1fr)}}
@media(max-width:600px){.content{padding:18px}.topbar{padding:0 18px}.cards{gap:10px}.card{padding:15px}.card-number{font-size:23px}.form-grid{grid-template-columns:1fr}.form-group.full{grid-column:auto}}
</style>
</head>
<body>
<div class="app">
<aside class="sidebar" id="sidebar">
<div class="logo">Staff<span>Track</span></div>
<ul class="menu">
<li><button class="active" onclick="showPage('dashboard',this)">📊 Dashboard</button></li>
<li><button onclick="showPage('attendance',this)">🕐 Attendance</button></li>
<li><button onclick="showPage('staff',this)">👥 Staff</button></li>
<li><button onclick="showPage('history',this)">📋 Attendance History</button></li>
</ul>
</aside>
<main class="main">
<header class="topbar">
<div style="display:flex;align-items:center;gap:12px"><button class="mobile-menu" onclick="toggleSidebar()">☰</button><h2 id="pageTitle">Dashboard</h2></div>
<div class="date-box" id="currentDate"></div>
</header>
<section class="content">

<div class="page active" id="dashboard">
<div class="cards">
<div class="card"><div class="card-title">Total Staff</div><div class="card-number" id="totalStaff">0</div></div>
<div class="card"><div class="card-title">Present Today</div><div class="card-number green" id="presentCount">0</div></div>
<div class="card"><div class="card-title">Absent Today</div><div class="card-number red" id="absentCount">0</div></div>
<div class="card"><div class="card-title">On Leave</div><div class="card-number purple" id="leaveCount">0</div></div>
</div>
<div class="toolbar"><h3>Today's Attendance</h3><button class="primary" onclick="showPageByName('attendance')">+ Manage Attendance</button></div>
<div class="table-card"><table><thead><tr><th>Staff</th><th>Department</th><th>Status</th><th>In Time</th><th>Out Time</th><th>Working Hours</th></tr></thead><tbody id="dashboardTable"></tbody></table></div>
</div>

<div class="page" id="attendance">
<div class="toolbar"><div style="display:flex;gap:10px;flex-wrap:wrap"><input type="date" id="attendanceDate" onchange="renderAttendance()"><input type="text" id="attendanceSearch" placeholder="Search staff..." oninput="renderAttendance()"></div><button class="primary" onclick="saveAttendance()">Save Attendance</button></div>
<div class="table-card"><table><thead><tr><th>Staff</th><th>Department</th><th>Status</th><th>In Time</th><th>Out Time</th><th>Working Hours</th></tr></thead><tbody id="attendanceTable"></tbody></table></div>
</div>

<div class="page" id="staff">
<div class="toolbar"><input type="text" id="staffSearch" placeholder="Search staff..." oninput="renderStaff()"><button class="primary" onclick="openStaffModal()">+ Add Staff</button></div>
<div class="table-card"><table><thead><tr><th>ID</th><th>Name</th><th>Department</th><th>Position</th><th>Phone</th><th>Actions</th></tr></thead><tbody id="staffTable"></tbody></table></div>
</div>

<div class="page" id="history">
<div class="toolbar"><div style="display:flex;gap:10px;flex-wrap:wrap"><input type="date" id="historyDate" onchange="renderHistory()"><input type="text" id="historySearch" placeholder="Search staff..." oninput="renderHistory()"></div><button class="primary" onclick="exportCSV()">↓ Export CSV</button></div>
<div class="table-card"><table><thead><tr><th>Date</th><th>Staff</th><th>Department</th><th>Status</th><th>In Time</th><th>Out Time</th><th>Working Hours</th></tr></thead><tbody id="historyTable"></tbody></table></div>
</div>

</section>
</main>
</div>

<div class="modal" id="staffModal">
<div class="modal-box">
<div class="modal-header"><h3 id="modalTitle">Add Staff</h3><button class="close" onclick="closeStaffModal()">×</button></div>
<form onsubmit="saveStaff(event)">
<input type="hidden" id="editStaffId">
<div class="form-grid">
<div class="form-group"><label>Staff ID</label><input id="staffId" required></div>
<div class="form-group"><label>Full Name</label><input id="staffName" required></div>
<div class="form-group"><label>Department</label><input id="staffDepartment" required></div>
<div class="form-group"><label>Position</label><input id="staffPosition" required></div>
<div class="form-group full"><label>Phone</label><input id="staffPhone"></div>
</div>
<div class="form-actions"><button type="button" class="secondary" onclick="closeStaffModal()">Cancel</button><button class="primary">Save Staff</button></div>
</form>
</div>
</div>

<script>
let staff=JSON.parse(localStorage.getItem("staffData"))||[
{id:"ST-001",name:"Rahim Ahmed",department:"Management",position:"Manager",phone:"01700000000"},
{id:"ST-002",name:"Karim Hasan",department:"Accounts",position:"Accountant",phone:"01800000000"},
{id:"ST-003",name:"Nusrat Jahan",department:"HR",position:"HR Executive",phone:"01900000000"}
];
let attendance=JSON.parse(localStorage.getItem("attendanceData"))||{};
function today(){return new Date().toISOString().split("T")[0]}
document.getElementById("attendanceDate").value=today();
document.getElementById("historyDate").value=today();
document.getElementById("currentDate").textContent=new Date().toLocaleDateString("en-BD",{weekday:"long",year:"numeric",month:"long",day:"numeric"});

function showPage(page,button){
document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
document.getElementById(page).classList.add("active");
document.querySelectorAll(".menu button").forEach(b=>b.classList.remove("active"));
if(button)button.classList.add("active");
const titles={dashboard:"Dashboard",attendance:"Attendance",staff:"Staff Management",history:"Attendance History"};
document.getElementById("pageTitle").textContent=titles[page];
if(page==="dashboard")renderDashboard();if(page==="attendance")renderAttendance();if(page==="staff")renderStaff();if(page==="history")renderHistory();
document.getElementById("sidebar").classList.remove("open")
}
function showPageByName(page){let b=[...document.querySelectorAll(".menu button")].find(x=>x.getAttribute("onclick")?.includes(page));if(b)showPage(page,b)}
function toggleSidebar(){document.getElementById("sidebar").classList.toggle("open")}

function renderStaff(){
let q=document.getElementById("staffSearch").value.toLowerCase(),t=document.getElementById("staffTable");
let f=staff.filter(s=>s.name.toLowerCase().includes(q)||s.id.toLowerCase().includes(q)||s.department.toLowerCase().includes(q));
if(!f.length){t.innerHTML='<tr><td colspan="6" class="empty">No staff found.</td></tr>';return}
t.innerHTML=f.map(s=>`<tr><td>${esc(s.id)}</td><td><strong>${esc(s.name)}</strong></td><td>${esc(s.department)}</td><td>${esc(s.position)}</td><td>${esc(s.phone||"-")}</td><td><button class="action-btn edit" onclick="editStaff('${esc(s.id)}')">Edit</button><button class="action-btn delete" onclick="deleteStaff('${esc(s.id)}')">Delete</button></td></tr>`).join("")
}
function openStaffModal(){document.getElementById("modalTitle").textContent="Add Staff";["editStaffId","staffId","staffName","staffDepartment","staffPosition","staffPhone"].forEach(x=>document.getElementById(x).value="");document.getElementById("staffModal").classList.add("show")}
function closeStaffModal(){document.getElementById("staffModal").classList.remove("show")}
function saveStaff(e){e.preventDefault();let edit=document.getElementById("editStaffId").value,n={id:document.getElementById("staffId").value.trim(),name:document.getElementById("staffName").value.trim(),department:document.getElementById("staffDepartment").value.trim(),position:document.getElementById("staffPosition").value.trim(),phone:document.getElementById("staffPhone").value.trim()};if(edit){let i=staff.findIndex(s=>s.id===edit);if(i>=0)staff[i]=n}else{if(staff.some(s=>s.id===n.id)){alert("Staff ID already exists.");return}staff.push(n)}localStorage.setItem("staffData",JSON.stringify(staff));closeStaffModal();renderStaff();renderDashboard();renderAttendance()}
function editStaff(id){let s=staff.find(x=>x.id===id);if(!s)return;document.getElementById("modalTitle").textContent="Edit Staff";document.getElementById("editStaffId").value=s.id;document.getElementById("staffId").value=s.id;document.getElementById("staffName").value=s.name;document.getElementById("staffDepartment").value=s.department;document.getElementById("staffPosition").value=s.position;document.getElementById("staffPhone").value=s.phone||"";document.getElementById("staffModal").classList.add("show")}
function deleteStaff(id){if(!confirm("Delete this staff member?"))return;staff=staff.filter(s=>s.id!==id);localStorage.setItem("staffData",JSON.stringify(staff));renderStaff();renderDashboard();renderAttendance()}

function getRecord(date,id){if(!attendance[date])attendance[date]={};if(!attendance[date][id])attendance[date][id]={status:"Present",inTime:"",outTime:""};return attendance[date][id]}
function hours(a,b){if(!a||!b)return"-";let s=new Date(`1970-01-01T${a}`),e=new Date(`1970-01-01T${b}`),d=(e-s)/60000;if(d<0)d+=1440;return `${Math.floor(d/60)}h ${d%60}m`}
function statusClass(s){return s==="Present"?"present":s==="Absent"?"absent":s==="Leave"?"leave":s==="Late"?"late":"half"}

function renderAttendance(){
let date=document.getElementById("attendanceDate").value,q=document.getElementById("attendanceSearch").value.toLowerCase(),t=document.getElementById("attendanceTable");
let f=staff.filter(s=>s.name.toLowerCase().includes(q)||s.id.toLowerCase().includes(q));
t.innerHTML=f.map(s=>{let r=getRecord(date,s.id);return `<tr><td><strong>${esc(s.name)}</strong><br><small>${esc(s.id)}</small></td><td>${esc(s.department)}</td><td><select class="status-input" data-id="${esc(s.id)}"><option ${r.status==="Present"?"selected":""}>Present</option><option ${r.status==="Absent"?"selected":""}>Absent</option><option ${r.status==="Leave"?"selected":""}>Leave</option><option ${r.status==="Late"?"selected":""}>Late</option><option ${r.status==="Half Day"?"selected":""}>Half Day</option></select></td><td><input type="time" class="in-input" data-id="${esc(s.id)}" value="${r.inTime||""}"></td><td><input type="time" class="out-input" data-id="${esc(s.id)}" value="${r.outTime||""}"></td><td>${hours(r.inTime,r.outTime)}</td></tr>`}).join("")
}
function saveAttendance(){
let date=document.getElementById("attendanceDate").value;if(!attendance[date])attendance[date]={};
document.querySelectorAll(".status-input").forEach(x=>{let id=x.dataset.id;if(!attendance[date][id])attendance[date][id]={};attendance[date][id].status=x.value});
document.querySelectorAll(".in-input").forEach(x=>{attendance[date][x.dataset.id].inTime=x.value});
document.querySelectorAll(".out-input").forEach(x=>{attendance[date][x.dataset.id].outTime=x.value});
localStorage.setItem("attendanceData",JSON.stringify(attendance));alert("Attendance saved successfully.");renderAttendance();renderDashboard()
}
function renderDashboard(){
let date=today(),p=0,a=0,l=0,t=document.getElementById("dashboardTable");document.getElementById("totalStaff").textContent=staff.length;
t.innerHTML=staff.map(s=>{let r=getRecord(date,s.id);if(r.status==="Present"||r.status==="Late")p++;if(r.status==="Absent")a++;if(r.status==="Leave")l++;return `<tr><td><strong>${esc(s.name)}</strong></td><td>${esc(s.department)}</td><td><span class="status ${statusClass(r.status)}">${r.status}</span></td><td>${r.inTime||"-"}</td><td>${r.outTime||"-"}</td><td>${hours(r.inTime,r.outTime)}</td></tr>`}).join("");
document.getElementById("presentCount").textContent=p;document.getElementById("absentCount").textContent=a;document.getElementById("leaveCount").textContent=l
}
function renderHistory(){
let date=document.getElementById("historyDate").value,q=document.getElementById("historySearch").value.toLowerCase(),t=document.getElementById("historyTable"),records=attendance[date]||{};
let f=staff.filter(s=>s.name.toLowerCase().includes(q)||s.id.toLowerCase().includes(q));
t.innerHTML=f.length?f.map(s=>{let r=records[s.id]||{status:"Present",inTime:"",outTime:""};return `<tr><td>${date}</td><td>${esc(s.name)}</td><td>${esc(s.department)}</td><td><span class="status ${statusClass(r.status)}">${r.status}</span></td><td>${r.inTime||"-"}</td><td>${r.outTime||"-"}</td><td>${hours(r.inTime,r.outTime)}</td></tr>`}).join(""):'<tr><td colspan="7" class="empty">No records found.</td></tr>'
}
function exportCSV(){
let date=document.getElementById("historyDate").value,r=attendance[date]||{},csv="Date,Staff ID,Staff Name,Department,Status,In Time,Out Time,Working Hours\n";
staff.forEach(s=>{let x=r[s.id]||{status:"Present",inTime:"",outTime:""};csv+=[date,s.id,s.name,s.department,x.status,x.inTime||"",x.outTime||"",hours(x.inTime,x.outTime)].map(v=>`"${String(v).replace(/"/g,'""')}"`).join(",")+"\n"});
let u=URL.createObjectURL(new Blob([csv],{type:"text/csv;charset=utf-8;"})),a=document.createElement("a");a.href=u;a.download=`attendance-${date}.csv`;a.click();URL.revokeObjectURL(u)
}
function esc(v){return String(v||"").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;").replace(/'/g,"&#039;")}
renderDashboard();renderStaff();renderAttendance();renderHistory();
</script>
</body>
</html>
