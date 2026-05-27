<!DOCTYPE html>
<html lang="fr">
<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>DM Tribunal</title>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial, sans-serif;
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

opacity:0.15;
z-index:-1;
}

header{
display:flex;
justify-content:space-between;
align-items:center;
padding:25px 8%;
border-bottom:1px solid rgba(255,255,255,0.08);
backdrop-filter:blur(10px);
position:sticky;
top:0;
background:rgba(7,7,11,0.8);
z-index:999;
}

.logo{
display:flex;
align-items:center;
gap:12px;
font-size:26px;
font-weight:bold;
color:#ff5eb5;
}

.logo span{
font-size:28px;
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
padding:80px 8%;
display:flex;
justify-content:space-between;
gap:60px;
align-items:flex-start;
}

.left{
max-width:560px;
}

.left h1{
font-size:74px;
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
width:470px;
background:#111118;
border:1px solid rgba(255,255,255,0.08);
border-radius:28px;
padding:25px;
box-shadow:0 0 30px rgba(255,79,163,0.12);
}

textarea{
width:100%;
height:190px;
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

.upload p{
margin-bottom:10px;
color:#ccc;
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
transform:translateY(0px);
}

}

.result h2{
margin-bottom:15px;
}

.examples{
padding:70px 8%;
}

.examples h2{
margin-bottom:30px;
font-size:42px;
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

.help-box{
padding:70px 8%;
}

.help-box h2{
margin-bottom:20px;
font-size:42px;
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

.auth-box h2{
margin-bottom:20px;
}

.switch{
margin-top:15px;
color:#aaa;
font-size:14px;
text-align:center;
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

@media(max-width:950px){

.hero{
flex-direction:column;
}

.left h1{
font-size:52px;
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
DM Tribunal
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
Le tribunal de tes
<span>messages</span>
</h1>

<p>
Analyse automatiquement les conversations,
captures d’écran et vibes émotionnelles.
Le tribunal détecte les green flags,
brown flags et red flags.
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
placeholder="Colle ici une conversation..."
></textarea>

<div class="upload">

<p>
Ajouter une capture d’écran ou un fichier
</p>

<input
type="file"
accept="image/*,.pdf,.txt"
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

<div class="result" id="result">

<h2 id="verdict"></h2>

<p id="analysis"></p>

</div>

</div>

</section>

<section class="examples" id="examples">

<h2>
Exemples de verdicts
</h2>

<div class="cards">

<div class="card">

<h3>
🟢 Acquitté
</h3>

<p>
“Bonne nuit ❤️ prends soin de toi”
<br><br>
Forte chaleur émotionnelle détectée.
</p>

</div>

<div class="card">

<h3>
🟠 Coupable
</h3>

<p>
“oe jsp”
<br><br>
Conversation hésitante et peu claire.
</p>

</div>

<div class="card">

<h3>
🔴 Condamné
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
Aide
</h2>

<p>
Tu peux coller une conversation,
ajouter une capture d’écran
ou importer un fichier.
Le tribunal analyse automatiquement
le mood général, l’énergie émotionnelle,
les efforts détectés,
la froideur et les vibes relationnelles.
</p>

</section>

<footer>
© 2026 — DM Tribunal
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

if(username === "" || password === ""){

alert("Remplis tous les champs");

return;

}

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

if(
username === "" ||
password === "" ||
email === ""
){

alert("Remplis tous les champs");

return;

}

const users =
JSON.parse(localStorage.getItem("users")) || [];

const exists =
users.find(
u => u.username === username
);

if(exists){

alert("Ce nom existe déjà");

return;

}

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

if(file.type.startsWith("image/")){

const preview =
document.getElementById("preview");

preview.src =
URL.createObjectURL(file);

preview.style.display = "block";

}

}

/* =========================================================
FREE COUNTER
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
ANALYSE IA
========================================================= */

function analyzeConversation(){

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

const textarea =
document.getElementById("conversation");

const text =
textarea.value.trim();

if(text === ""){

alert("Ajoute une conversation");

return;

}

const lower =
text.toLowerCase();

let warmth = 0;
let coldness = 0;
let effort = 0;
let flirt = 0;
let dryness = 0;

/* LONGUEUR */

if(text.length > 300){

effort += 3;
warmth += 2;

}

if(text.length < 20){

dryness += 3;
coldness += 2;

}

/* EMOJIS */

const emojis =
(text.match(/❤️|🥰|😍|😘|💕|💖|😭|😩|🫶|😻|☺️|😊/g) || []).length;

warmth += emojis;

/* EXCLAMATIONS */

const exclamations =
(text.match(/!/g) || []).length;

warmth += exclamations * 0.5;

/* QUESTIONS */

const questions =
(text.match(/\?/g) || []).length;

if(questions >= 2){

effort += 2;

}

/* DRY REPLIES */

const dryReplies = [

"ok",
"oui",
"non",
"jsp",
"bah",
"vu",
"cool",
"d'accord",
"oe",
"hein"

];

dryReplies.forEach(word=>{

if(lower.includes(word)){

dryness++;
coldness++;

}

});

/* FLIRT */

const flirtSignals = [

"tu me manques",
"jtm",
"je t'aime",
"viens",
"on se voit",
"bb",
"babe",
"mon coeur",
"bonne nuit",
"love",
"je pense à toi",
"hâte de te voir"

];

flirtSignals.forEach(word=>{

if(lower.includes(word)){

flirt += 2;
warmth += 2;

}

});

/* TOXIC */

const toxicSignals = [

"tg",
"ta gueule",
"casse toi",
"bloqué",
"flemme",
"stop",
"forceur"

];

toxicSignals.forEach(word=>{

if(lower.includes(word)){

coldness += 4;

}

});

/* LAUGHTER */

if(
lower.includes("mdrrrr") ||
lower.includes("ahah") ||
lower.includes("HAHA")
){

warmth += 2;

}

/* SCORE */

const finalScore =

(warmth + flirt + effort)
-
(coldness + dryness);

/* RESULT */

const result =
document.getElementById("result");

const verdict =
document.getElementById("verdict");

const analysis =
document.getElementById("analysis");

result.style.display = "block";

/* RANDOM TEXTS */

const acquittedTexts = [

"Très forte alchimie émotionnelle détectée.",

"Conversation naturelle et investie émotionnellement.",

"Le tribunal détecte beaucoup de green flags.",

"Très bonne vibe relationnelle détectée."

];

const guiltyTexts = [

"Conversation mitigée émotionnellement.",

"Vibe hésitante détectée.",

"Intérêt difficile à lire.",

"Communication irrégulière détectée."

];

const condemnedTexts = [

"Très forte froideur émotionnelle détectée.",

"Le tribunal détecte plusieurs red flags.",

"Conversation émotionnellement distante.",

"Ghosting potentiel détecté."

];

/* VERDICT */

if(finalScore >= 6){

verdict.innerHTML =
"🟢 ACQUITTÉ";

analysis.innerHTML =

acquittedTexts[
Math.floor(
Math.random() *
acquittedTexts.length
)
];

}

else if(finalScore <= -1){

verdict.innerHTML =
"🔴 CONDAMNÉ";

analysis.innerHTML =

condemnedTexts[
Math.floor(
Math.random() *
condemnedTexts.length
)
];

}

else{

verdict.innerHTML =
"🟠 COUPABLE";

analysis.innerHTML =

guiltyTexts[
Math.floor(
Math.random() *
guiltyTexts.length
)
];

}

analysis.innerHTML +=

`

<br><br>

🩷 Chaleur émotionnelle :
${Math.max(0,warmth)}

<br>

✨ Efforts détectés :
${Math.max(0,effort)}

<br>

💕 Flirt détecté :
${Math.max(0,flirt)}

<br>

❄️ Froideur :
${Math.max(0,coldness)}

<br>

🪨 Sécheresse :
${Math.max(0,dryness)}

`;

/* RESET */

setTimeout(()=>{

textarea.value = "";

document
.getElementById("preview")
.style.display = "none";

},800);

/* FREE */

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
CLICK OUTSIDE
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