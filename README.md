<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#090914">
<title>AIVORA - My Dashboard</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{background:#090914;color:#fff;font-family:Arial,sans-serif;min-height:100vh}
header{padding:18px 6%;display:flex;justify-content:space-between;align-items:center;background:#101020;border-bottom:1px solid #292943}
.logo{font-size:25px;font-weight:900;color:#a78bfa;letter-spacing:2px}
a,button{cursor:pointer}
.home{color:#c4b5fd;text-decoration:none;font-size:14px}
main{max-width:850px;margin:auto;padding:30px 16px}
h1{font-size:30px;margin-bottom:8px}
.subtitle{color:#999;margin-bottom:25px;font-size:14px}
.card{background:#151528;border:1px solid #30304b;border-radius:17px;padding:22px;margin-bottom:18px}
.profile{text-align:center}
.avatar{width:115px;height:115px;border-radius:50%;object-fit:cover;border:3px solid #a78bfa;background:#25253c;margin-bottom:15px}
.name{font-size:22px;margin-bottom:7px}
.email{color:#aaa;font-size:14px;margin-bottom:18px;overflow-wrap:anywhere}
.actions{display:flex;justify-content:center;gap:10px;flex-wrap:wrap}
.btn{padding:11px 16px;border:0;border-radius:9px;background:#8b5cf6;color:white;font-weight:bold}
.btn.secondary{background:#30304b;color:#ddd}
h2{font-size:18px;margin-bottom:15px;color:#c4b5fd}
.detail{padding:12px 0;border-bottom:1px solid #30304b;font-size:14px;color:#ccc;overflow-wrap:anywhere}
.detail:last-child{border:0}
.empty{color:#999;font-size:14px;line-height:1.6}
.notice{font-size:12px;color:#e9bd70;margin-top:14px;line-height:1.5}
footer{text-align:center;color:#777;padding:25px;font-size:12px}
</style>
</head>
<body>

<header>
 <div class="logo">AIVORA</div>
 <a class="home" href="index.html">← Back to Home</a>
</header>

<main>
 <h1>My Dashboard</h1>
 <p class="subtitle">Manage your AIVORA profile and account.</p>

 <section class="card profile">
  <img id="avatar" class="avatar" alt="Profile photo">
  <h2 class="name" id="profileName">User</h2>
  <p class="email" id="profileEmail"></p>
  <div class="actions">
   <label class="btn" for="photoInput">Upload Photo</label>
   <input id="photoInput" type="file" accept="image/*" hidden>
   <button class="btn secondary" id="removePhoto" type="button">Remove Photo</button>
  </div>
  <p class="notice">Your profile photo is stored in this browser only in this demo.</p>
 </section>

 <section class="card">
  <h2>Account Information</h2>
  <div class="detail">Name: <span id="detailName"></span></div>
  <div class="detail">Email: <span id="detailEmail"></span></div>
  <div class="detail">Account: Demo account</div>
 </section>

 <section class="card">
  <h2>Chat History</h2>
  <p class="empty">Your saved chat history feature will be added in a future update. Chats are not currently stored here.</p>
 </section>

 <section class="card">
  <h2>Account Settings</h2>
  <p class="empty">You can manage your profile photo here. More account settings will be added later.</p>
  <br>
  <button class="btn secondary" id="logoutButton" type="button">Logout</button>
 </section>
</main>

<footer>© 2026 AIVORA · Created by Ohadul Islam</footer>

<script>
const userKey="aivoraUser";
const photoKey="aivoraProfilePhoto";

let user;
try{
 user=JSON.parse(localStorage.getItem(userKey)||"null");
}catch(e){
 user=null;
}

if(!user){
 alert("Please login first.");
 window.location.href="index.html";
}

if(user){
 document.getElementById("profileName").textContent=user.name||"AIVORA User";
 document.getElementById("profileEmail").textContent=user.email||"";
 document.getElementById("detailName").textContent=user.name||"";
 document.getElementById("detailEmail").textContent=user.email||"";
}

const avatar=document.getElementById("avatar");

function showPhoto(){
 const photo=localStorage.getItem(photoKey);
 if(photo){
  avatar.src=photo;
 }else{
  avatar.src="data:image/svg+xml,"+encodeURIComponent(
   '<svg xmlns="http://www.w3.org/2000/svg" width="120" height="120"><rect width="100%" height="100%" fill="#25253c"/><text x="50%" y="56%" font-size="48" text-anchor="middle" fill="#c4b5fd">👤</text></svg>'
  );
 }
}
showPhoto();

document.getElementById("photoInput").addEventListener("change",function(){
 const file=this.files[0];
 if(!file)return;

 if(!file.type.startsWith("image/")){
  alert("Please choose an image file.");
  return;
 }

 const reader=new FileReader();
 reader.onload=function(event){
  const image=new Image();
  image.onload=function(){
   const canvas=document.createElement("canvas");
   const max=400;
   const scale=Math.min(1,max/Math.max(image.width,image.height));
   canvas.width=Math.round(image.width*scale);
   canvas.height=Math.round(image.height*scale);
   const ctx=canvas.getContext("2d");
   ctx.drawImage(image,0,0,canvas.width,canvas.height);
   const compressed=canvas.toDataURL("image/jpeg",0.8);

   try{
    localStorage.setItem(photoKey,compressed);
    showPhoto();
    alert("Profile photo updated!");
   }catch(error){
    alert("Photo could not be saved. Try a smaller image.");
   }
  };
  image.src=event.target.result;
 };
 reader.readAsDataURL(file);
 this.value="";
});

document.getElementById("removePhoto").addEventListener("click",function(){
 localStorage.removeItem(photoKey);
 showPhoto();
});

document.getElementById("logoutButton").addEventListener("click",function(){
 localStorage.removeItem(userKey);
 window.location.href="index.html";
});
</script>
</body>
</html>
