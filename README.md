# bd-likeflow-selar-
Bd like flow selar bd smm
<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>LikeflowBD Demo</title>
<style>
  :root{--bg:#0f1113;--card:#14161a;--muted:#9aa0a6;--accent:#2f9e44;}
  *{box-sizing:border-box} body{margin:0;font-family:Inter,system-ui,sans-serif;background:var(--bg);color:#eef2f3;}
  .wrap{max-width:980px;margin:18px auto;padding:18px}
  header{display:flex;justify-content:space-between;align-items:center;margin-bottom:18px;flex-wrap:wrap}
  h1{margin:0} .small{color:var(--muted);font-size:13px}
  .card{background:var(--card);padding:16px;border-radius:10px;margin-bottom:12px;}
  .row{display:flex;gap:12px;flex-wrap:wrap}
  input,button{padding:10px;border-radius:8px;border:0;background:rgba(255,255,255,0.03);color:inherit;font-size:15px}
  button{cursor:pointer}
  .hidden{display:none}
  .label{font-size:13px;color:var(--muted);margin-bottom:6px}
  footer{color:var(--muted);font-size:13px;text-align:center;margin-top:18px}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>LikeflowBD Demo</h1>
    <div id="userArea"></div>
  </header>

  <!-- AUTH -->
  <section id="auth" class="card">
    <div class="row">
      <div style="flex:1;min-width:260px">
        <div class="label">নতুন একাউন্ট</div>
        <input id="regName" placeholder="নাম / ইউজারনেম"/>
        <input id="regEmail" placeholder="ইমেইল বা মোবাইল" style="margin-top:8px"/>
        <input id="regPass" type="password" placeholder="পাসওয়ার্ড" style="margin-top:8px"/>
        <button style="margin-top:8px" onclick="register()">Register</button>
      </div>
      <div style="flex:1;min-width:260px">
        <div class="label">লগইন</div>
        <input id="loginEmail" placeholder="ইমেইল বা মোবাইল"/>
        <input id="loginPass" type="password" placeholder="পাসওয়ার্ড" style="margin-top:8px"/>
        <button style="margin-top:8px" onclick="login()">Login</button>
      </div>
    </div>
  </section>

  <!-- HOME -->
  <section id="home" class="card hidden">
    <div>
      <div class="small">Current Balance: ৳<span id="balance">0</span></div>
      <div class="small">Account Status: <span id="acctStatus" class="muted">Not Active</span></div>
    </div>
    <div id="homeMsg" class="muted">আপনি লগইন করুন বা রেজিস্টার করুন।</div>
  </section>

  <footer class="small">Demo system — LocalStorage ব্যবহার করা হয়েছে।</footer>
</div>

<script>
// LocalStorage simulation
const LS = {
  get(k){ try{return JSON.parse(localStorage.getItem(k)||'null')}catch(e){return null} },
  set(k,v){ localStorage.setItem(k,JSON.stringify(v)) }
};
if(!LS.get('app_users')){
  LS.set('app_users',[{id:1,name:'Admin',email:'admin@site',pass:'admin123',balance:0,active:true,packages:[],linksDoneByDate:{}}]);
}
let currentUser = null;

function show(id){ ['auth','home'].forEach(x=>document.getElementById(x).classList.add('hidden')); document.getElementById(id).classList.remove('hidden'); refreshUI(); }

function refreshUI(){
  const userArea = document.getElementById('userArea');
  if(currentUser){
    userArea.innerHTML = `<div class="small">Hi, ${currentUser.name}</div><button onclick="logout()">Logout</button>`;
  } else userArea.innerHTML = `<button onclick="show('auth')">Login/Register</button>`;
  document.getElementById('balance').innerText = currentUser ? currentUser.balance : '0';
  document.getElementById('acctStatus').innerText = currentUser ? (currentUser.active?'Active':'Not Active'):'Not Active';
  const homeMsg = document.getElementById('homeMsg');
  homeMsg.innerText = currentUser ? 'আপনি লগইন করেছেন।':'আপনি লগইন করুন বা রেজিস্টার করুন।';
}

// Auth functions
function register(){
  const name=document.getElementById('regName').value.trim();
  const email=document.getElementById('regEmail').value.trim();
  const pass=document.getElementById('regPass').value;
  if(!name||!email||!pass){ alert('সবগুলো ফিল্ড পূরণ করুন'); return; }
  const users = LS.get('app_users')||[];
  if(users.find(u=>u.email===email)){ alert('ইমেইল/নাম্বার আগে থেকেই আছে'); return; }
  const id = Date.now();
  users.push({id,name,email,pass,balance:0,active:false,packages:[],linksDoneByDate:{}});
  LS.set('app_users',users);
  alert('Register সফল! এখন লগইন করুন।');
}

function login(){
  const email=document.getElementById('loginEmail').value.trim();
  const pass=document.getElementById('loginPass').value;
  const users = LS.get('app_users')||[];
  const u = users.find(x=>x.email===email && x.pass===pass);
  if(!u){ alert('User not found or wrong password'); return; }
  currentUser=u;
  show('home');
  alert('Login successful');
}

function logout(){ currentUser=null; show('auth'); }
</script>
</body>
</html>
