<!DOCTYPE html>
<html lang="fr">

<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>DM Tribunal AI</title>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
background:#07070b;
color:white;
overflow-x:hidden;
}

body::before{
content:"";
position:fixed;
inset:0;
background:
radial-gradient(circle at top left,#ff2f92 0%,transparent 30%),
radial-gradient(circle at bottom right,#7b2fff 0%,transparent 30%);
opacity:0.14;
z-index:-1;
}

header{
display:flex;
justify-content:space-between;
align-items:center;
padding:25px 8%;
border-bottom:1px solid rgba(255,255,255,0.08);
background:rgba(7,7,11,0.85);
backdrop-filter:blur(10px);
position:sticky;
top:0;
z-index:999;
}

.logo{
display:flex;
align-items:center;
gap:12px;
font-size:28px;
font-weight:bold;
color:#ff5eb5;
}

.logo span{
font-size:30px;
color:#d9d9d9;
}

nav{
display:flex;
gap:18px;
align-items:center;
}

nav button{
background:none;
border:none;
color:#ccc;
cursor:pointer;
font-size:15px;
transition:0.3s;
}

nav button:hover{
color:#ff5eb5;
}

.main-btn{
background:linear-gradient(90deg,#ff4fa3,#ff73c7);
border:none;
padding:14px 24px;
border-radius:30px;
color:white;
cursor:pointer;
font-weight:bold;
transition:0.3s;
}

.main-btn:hover{
transform:translateY(-2px);
}

.hero{
padding:90px 8%;
display:flex;
justify-content:space-between;
gap:60px;
align-items:flex-start;
}

.left{
max-width:580px;
}

.left h1{
font-size:76px;
line-height:1;
margin-bottom:25px;
}

.left h1 span{
color:#ff5eb5;
}

.left p{
color:#aaa;
line-height:1.8;
margin-bottom:35px;
}

.counter{
margin-top:20px;
color:#ff73c7;
font-weight:bold;
}

.panel{
width:500px;
background:#111118;
border:1px solid rgba(255,255,255,0.08);
border-radius:28px;
padding:25px;
box-shadow:0 0 30px rgba(255,79,163,0.1);
}

textarea{
width:100%;
height:220px;
background:#1a1a22;
border:none;
border-radius:18px;
padding:18px;
color:white;
resize:none;
margin-bottom:18px;
font-size:15px;
}

.upload{
background:#1a1a22;
padding:20px;
border-radius:18px;
margin-bottom:18px;
}

input{
width:100%;
padding:15px;
background:#1a1a22;
border:none;
border-radius:15px;
color:white;
margin-bottom:15px;
}

.preview{
width:100%;
border-radius:15px;
margin-top:15px;
display:none;
max-height:300px;
object-fit:cover;
}

.result{
margin-top:20px;
background:rgba(255,255,255,0.04);
padding:22px;
border-radius:20px;
display:none;
animation:fade 0.4s ease;
}

@keyframes fade{

from{
opacity:0;
transform:translateY(10px);
}

to{
opacity:1;
transform:translateY(0);
}

}

.examples,
.help-box{
padding:80px 8%;
}

.examples h2,
.help-box h2{
font-size:42px;
margin-bottom:30px;
}

.cards{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
gap:20px;
}

.card{
background:rgba(255,255,255,0.04);
border:1px solid rgba(255,255,255,0.08);
border-radius:22px;
padding:25px;
}

.card h3{
margin-bottom:15px;
color:#ff73c7;
}

.help-box p{
color:#aaa;
line-height:1.8;
max-width:800px;
}

.auth{
position:fixed;
inset:0;
background:rgba(0,0,0,0.85);
display:none;
justify-content:center;
align-items:center;
z-index:9999;
}

.auth-box{
width:420px;
background:#111118;
padding:30px;
border-radius:25px;
}

.switch{
margin-top:15px;
font-size:14px;
text-align:center;
color:#aaa;
}

.switch span{
color:#ff73c7;
cursor:pointer;
}

footer{
padding:50px;
text-align:center;
color:#777;
}

.loading{
display:none;
margin-top:15px;
color:#ff73c7;
font-weight:bold;
}

@media(max-width:950px){

.hero{
flex-direction:column;
}

.left h1{
font-size:54px;
}

.panel{
width:100%;
}

nav{
display:none;
}

}

</style>

</head>

<body>

<header>

<div class="logo">
<span>⚖️</span>
DM Tribunal AI
</div>

<nav>

<button onclick="scrollToSection('home')">
Accueil
</button>

<button onclick="scrollToSection('examples')">
Exemples
</button>

<button onclick="scrollToSection('help')">
Aide
</button>

<button class="main-btn" onclick="openAuth()">
Connexion
</button>

</nav>

</header>

<section class="hero" id="home">

<div class="left">

<h1>
Le tribunal
des <span>DM</span>
</h1>

<p>
Analyse automatiquement les conversations,
captures d’écran et vibes relationnelles
avec une intelligence artificielle émotionnelle.
</p>

<button class="main-btn" onclick="scrollToAnalyzer()">
Analyser maintenant
</button>

<div class="counter" id="freeCounter">
5 analyses gratuites restantes
</div>

</div>

<div class="panel" id="analyzer">

<h2 style="margin-bottom:20px;">
Nouvelle analyse
</h2>

<textarea
id="conversation"
placeholder="Colle une conversation..."
></textarea>

<div class="upload">

<p style="margin-bottom:10px;color:#ccc;">
Ajouter une capture d’écran
</p>

<input
type="file"
accept="image/*"
onchange="previewImage(event)"
>

<img id="preview" class="preview">

</div>

<button
class="main-btn"
style="width:100%;"
onclick="analyzeConversation()"
>
Analyser
</button>

<div class="loading" id="loading">
Analyse IA en cours...
</div>

<div class="result" id="result">

<h2 id="verdict"></h2>

<p id="analysis"></p>

</div>

</div>

</section>

<section class="examples" id="examples">

<h2>
Exemples
</h2>

<div class="cards">

<div class="card">

<h3>
🟢 ACQUITTÉ
</h3>

<p>
“Bonne nuit princesse ❤️”
<br><br>
Énergie chaleureuse,
flirt réciproque,
effort émotionnel détecté.
</p>

</div>

<div class="card">

<h3>
🟠 COUPABLE
</h3>

<p>
“oe jsp”
<br><br>
Conversation émotionnellement floue.
</p>

</div>

<div class="card">

<h3>
🔴 CONDAMNÉ
</h3>

<p>
“vu”
<br><br>
Très forte froideur émotionnelle détectée.
</p>

</div>

</div>

</section>

<section class="help-box" id="help">

<h2>
Comment ça marche ?
</h2>

<p>
DM Tribunal AI analyse automatiquement
les conversations afin de détecter
la vibe émotionnelle,
les green flags,
les red flags,
le niveau d’investissement,
la chaleur émotionnelle,
la froideur,
les comportements toxiques
et les dynamiques relationnelles.
</p>

</section>

<footer>
© 2026 — DM Tribunal AI
</footer>

<div class="auth" id="auth">

<div class="auth-box">

<h2 id="authTitle">
Connexion
</h2>

<input
type="text"
id="username"
placeholder="Nom d'utilisateur"
>

<input
type="email"
id="email"
placeholder="Adresse mail"
style="display:none;"
>

<input
type="password"
id="password"
placeholder="Mot de passe"
>

<button
class="main-btn"
style="width:100%;"
onclick="submitAuth()"
id="authButton"
>
Se connecter
</button>

<div class="switch">

<span onclick="toggleAuthMode()">
Pas encore de compte ? Créer un compte
</span>

</div>

</div>

</div>

<script>

/* =========================================================
CONFIG
========================================================= */

let loginMode = true;
let freeAnalyses = 5;

const savedAnalyses =
localStorage.getItem("freeAnalyses");

if(savedAnalyses !== null){

freeAnalyses =
parseInt(savedAnalyses);

}

/* =========================================================
AUTH
========================================================= */

function openAuth(){

document.getElementById("auth").style.display =
"flex";

}

function closeAuth(){

document.getElementById("auth").style.display =
"none";

}

function toggleAuthMode(){

loginMode = !loginMode;

const title =
document.getElementById("authTitle");

const button =
document.getElementById("authButton");

const switchText =
document.querySelector(".switch span");

const emailField =
document.getElementById("email");

if(loginMode){

title.innerHTML = "Connexion";

button.innerHTML = "Se connecter";

switchText.innerHTML =
"Pas encore de compte ? Créer un compte";

emailField.style.display = "none";

}

else{

title.innerHTML = "Créer un compte";

button.innerHTML = "Créer le compte";

switchText.innerHTML =
"Déjà un compte ? Se connecter";

emailField.style.display = "block";

}

}

function submitAuth(){

const username =
document.getElementById("username").value;

const password =
document.getElementById("password").value;

const email =
document.getElementById("email").value;

if(loginMode){

const users =
JSON.parse(localStorage.getItem("users")) || [];

const user =
users.find(
u =>
u.username === username &&
u.password === password
);

if(!user){

alert("Identifiants incorrects");

return;

}

localStorage.setItem(
"loggedIn",
"true"
);

localStorage.setItem(
"loggedUser",
username
);

alert("Connexion réussie ✅");

closeAuth();

}

else{

const users =
JSON.parse(localStorage.getItem("users")) || [];

users.push({
username,
password,
email
});

localStorage.setItem(
"users",
JSON.stringify(users)
);

localStorage.setItem(
"loggedIn",
"true"
);

localStorage.setItem(
"loggedUser",
username
);

alert("Compte créé ✅");

closeAuth();

}

updateFreeCounter();

}

/* =========================================================
PREVIEW IMAGE
========================================================= */

function previewImage(event){

const file = event.target.files[0];

if(!file) return;

const preview =
document.getElementById("preview");

preview.src =
URL.createObjectURL(file);

preview.style.display = "block";

}

/* =========================================================
COUNTER
========================================================= */

function updateFreeCounter(){

const isLoggedIn =
localStorage.getItem("loggedIn");

const counter =
document.getElementById("freeCounter");

if(isLoggedIn){

counter.innerHTML =
"Connecté en tant que : " +
localStorage.getItem("loggedUser");

}

else{

counter.innerHTML =
freeAnalyses +
" analyses gratuites restantes";

}

}

/* =========================================================
IA ANALYSIS
========================================================= */

async function analyzeConversation(){

const isLoggedIn =
localStorage.getItem("loggedIn");

if(
freeAnalyses <= 0 &&
!isLoggedIn
){

alert(
"Tu as utilisé tes 5 analyses gratuites."
);

openAuth();

return;

}

const text =
document
.getElementById("conversation")
.value
.trim();

if(text === ""){

alert("Ajoute une conversation");

return;

}

document
.getElementById("loading")
.style.display = "block";

document
.getElementById("result")
.style.display = "none";

/* =========================================================
SIMULATION IA
========================================================= */

await new Promise(resolve =>
setTimeout(resolve,1800)
);

let affection = 0;
let tension = 0;
let investment = 0;
let dryness = 0;

/* ÉNERGIE */

if(text.length > 50){

investment += 2;

}

if(text.length > 150){

investment += 3;

}

/* QUESTIONS */

const questions =
(text.match(/\?/g) || []).length;

investment += questions;

/* EMOJIS */

const emojis =
(text.match(
/❤️|💕|🥰|😍|😘|🫶|😭|😊|☺️/g
) || []).length;

affection += emojis * 2;

/* EXPRESSIVITÉ */

if(
text.includes("!!!") ||
text.includes("ahah") ||
text.includes("mdrrrr")
){

affection += 3;

}

/* FROIDEUR */

if(
text.length < 8
){

dryness += 3;

}

if(
text.includes("vu") ||
text.includes("ok.") ||
text.includes("cool.")
){

dryness += 4;

}

/* TENSION */

if(
text.includes("???")
){

tension += 3;

}

if(
text.includes("tg") ||
text.includes("ta gueule")
){

tension += 5;

}

/* SCORE */

const finalScore =

(
affection +
investment
)

-

(
dryness +
tension
);

/* =========================================================
RESULT
========================================================= */

const result =
document.getElementById("result");

const verdict =
document.getElementById("verdict");

const analysis =
document.getElementById("analysis");

result.style.display = "block";

document
.getElementById("loading")
.style.display = "none";

if(finalScore >= 5){

verdict.innerHTML =
"🟢 ACQUITTÉ";

analysis.innerHTML =

`
La conversation paraît émotionnellement investie
et chaleureuse.

Le tribunal détecte :
<br><br>

💕 Affection visible
<br>
✨ Efforts émotionnels
<br>
🫶 Bonne vibe générale
`;

}

else if(finalScore <= 0){

verdict.innerHTML =
"🔴 CONDAMNÉ";

analysis.innerHTML =

`
Le tribunal détecte une conversation froide,
sèche ou émotionnellement distante.

<br><br>

❄️ Froideur détectée
<br>
🌧️ Déséquilibre émotionnel
<br>
🚩 Red flags potentiels
`;

}

else{

verdict.innerHTML =
"🟠 COUPABLE";

analysis.innerHTML =

`
La conversation paraît mitigée émotionnellement.

<br><br>

⚖️ Énergie hésitante
<br>
🫥 Vibe difficile à lire
<br>
✨ Quelques efforts détectés
`;

}

/* =========================================================
RESET
========================================================= */

setTimeout(()=>{

document
.getElementById("conversation")
.value = "";

document
.getElementById("preview")
.style.display = "none";

},1000);

/* =========================================================
FREE ANALYSES
========================================================= */

if(!isLoggedIn){

freeAnalyses--;

localStorage.setItem(
"freeAnalyses",
freeAnalyses
);

}

updateFreeCounter();

}

/* =========================================================
SCROLL
========================================================= */

function scrollToAnalyzer(){

document
.getElementById("analyzer")
.scrollIntoView({
behavior:"smooth"
});

}

function scrollToSection(id){

document
.getElementById(id)
.scrollIntoView({
behavior:"smooth"
});

}

/* =========================================================
CLICK OUTSIDE AUTH
========================================================= */

window.onclick = function(event){

const auth =
document.getElementById("auth");

if(event.target === auth){

closeAuth();

}

};

/* =========================================================
LOAD
========================================================= */

window.onload = () => {

updateFreeCounter();

};

</script>

</body>
</html>