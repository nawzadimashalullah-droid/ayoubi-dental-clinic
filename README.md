
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ayoubi Dental Clinic</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f1f5f9;color:#1e293b}
header{background:#075985;color:white;padding:25px 15px;text-align:center}
header h1{margin:0 0 8px}
header p{margin:0}
.container{max-width:950px;margin:25px auto;padding:15px}
.card{background:white;padding:20px;border-radius:12px;margin-bottom:20px;box-shadow:0 2px 10px #0001}
.stats{display:flex;gap:15px;flex-wrap:wrap}
.stat{background:#e0f2fe;padding:18px;border-radius:10px;flex:1;min-width:140px}
.stat strong{display:block;font-size:27px;color:#0369a1}
input,select,textarea{width:100%;padding:12px;margin:7px 0 14px;border:1px solid #cbd5e1;border-radius:7px;font-size:15px}
label{font-weight:bold}
button{background:#0284c7;color:white;border:0;padding:12px 18px;border-radius:7px;cursor:pointer}
button:hover{background:#0369a1}
.delete{background:#dc2626;padding:7px 10px}
table{width:100%;border-collapse:collapse;min-width:650px}
th,td{padding:12px;border-bottom:1px solid #e2e8f0;text-align:left}
th{background:#e0f2fe}
.table-wrap{overflow-x:auto}
footer{text-align:center;padding:20px;color:#64748b}
</style>
</head>
<body>

<header>
<h1>🦷 Ayoubi Dental Clinic</h1>
<p>Patient Management System</p>
</header>

<div class="container">

<div class="stats">
<div class="stat">
Total Patients
<strong id="total">0</strong>
</div>
<div class="stat">
Clinic
<strong style="font-size:20px">Ayoubi</strong>
</div>
</div>

<div class="card">
<h2>➕ Register New Patient</h2>

<form id="patientForm">
<label>Patient Full Name</label>
<input id="name" type="text" required placeholder="Enter full name">

<label>Phone Number</label>
<input id="phone" type="tel" placeholder="Enter phone number">

<label>Age</label>
<input id="age" type="number" min="0" placeholder="Enter age">

<label>Gender</label>
<select id="gender">
<option value="">Select gender</option>
<option>Male</option>
<option>Female</option>
</select>

<label>Chief Complaint</label>
<textarea id="complaint" placeholder="Patient complaint"></textarea>

<label>Medical History</label>
<textarea id="history" placeholder="Medical history"></textarea>

<label>Address</label>
<input id="address" type="text" placeholder="Patient address">

<button type="submit">Save Patient</button>
</form>
</div>

<div class="card">
<h2>📋 Patient Records</h2>
<input id="search" type="search" placeholder="Search patient by name or phone">

<div class="table-wrap">
<table>
<thead>
<tr>
<th>Name</th>
<th>Phone</th>
<th>Age</th>
<th>Gender</th>
<th>Complaint</th>
<th>Action</th>
</tr>
</thead>
<tbody id="patientList"></tbody>
</table>
</div>
</div>
</div>

<footer>
Ayoubi Dental Clinic © 2026
</footer>

<script>
let patients = [];

try {
patients = JSON.parse(localStorage.getItem("ayoubiPatients")) || [];
} catch(e) {
patients = [];
}

function saveData(){
localStorage.setItem("ayoubiPatients",JSON.stringify(patients));
}

function displayPatients(){
const list=document.getElementById("patientList");
const search=document.getElementById("search").value.toLowerCase();
list.innerHTML="";

patients.forEach((p,index)=>{
if(
p.name.toLowerCase().includes(search) ||
p.phone.toLowerCase().includes(search)
){
const tr=document.createElement("tr");

[p.name,p.phone,p.age,p.gender,p.complaint].forEach(value=>{
const td=document.createElement("td");
td.textContent=value || "-";
tr.appendChild(td);
});

const action=document.createElement("td");
const btn=document.createElement("button");
btn.textContent="Delete";
btn.className="delete";
btn.onclick=()=>{
if(confirm("Delete this patient?")){
patients.splice(index,1);
saveData();
displayPatients();
}
};
action.appendChild(btn);
tr.appendChild(action);
list.appendChild(tr);
}
});

document.getElementById("total").textContent=patients.length;
}

document.getElementById("patientForm").addEventListener("submit",function(e){
e.preventDefault();

const patient={
name:document.getElementById("name").value.trim(),
phone:document.getElementById("phone").value.trim(),
age:document.getElementById("age").value,
gender:document.getElementById("gender").value,
complaint:document.getElementById("complaint").value.trim(),
history:document.getElementById("history").value.trim(),
address:document.getElementById("address").value.trim()
};

patients.push(patient);
saveData();
displayPatients();
this.reset();
alert("Patient saved successfully!");
});

document.getElementById("search").addEventListener("input",displayPatients);

displayPatients();
</script>

</body>
</html>
