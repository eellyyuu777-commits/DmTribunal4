<script>

/* =========================================================
DM TRIBUNAL — SCRIPT COMPLET
========================================================= */

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
ANALYSE MOOD IA
========================================================= */

function analyzeConversation(){

const isLoggedIn =
localStorage.getItem("loggedIn");

/* =========================================================
LIMITES GRATUITES
========================================================= */

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

/* =========================================================
RÉCUP TEXTE
========================================================= */

const textarea =
document.getElementById("conversation");

const text =
textarea.value.trim();

if(text === ""){

alert("Ajoute une conversation");

return;

}

const result =
document.getElementById("result");

const verdict =
document.getElementById("verdict");

const analysis =
document.getElementById("analysis");

result.style.display = "block";

/* =========================================================
ANALYSE ÉMOTIONNELLE
========================================================= */

let energy = 0;
let affection = 0;
let investment = 0;
let coldness = 0;
let tension = 0;

/* =========================================================
LONGUEUR
========================================================= */

const length = text.length;

if(length > 30){

investment += 2;

}

if(length > 100){

investment += 3;

}

if(length > 250){

investment += 4;

}

/* =========================================================
QUESTIONS
========================================================= */

const questions =
(text.match(/\?/g) || []).length;

investment += questions;

/* =========================================================
EXCLAMATIONS
========================================================= */

const exclamations =
(text.match(/!/g) || []).length;

energy += exclamations * 1.5;

/* =========================================================
EMOJIS
========================================================= */

const emojis =
(text.match(
/❤️|💕|💖|🥰|😍|😘|🫶|😭|😩|😻|☺️|😊|💞|😌/g
) || []).length;

affection += emojis * 2;

/* =========================================================
MAJUSCULES
========================================================= */

const caps =
(text.match(/[A-Z]/g) || []).length;

if(caps >= 6){

energy += 2;

}

/* =========================================================
RIRE / EXPRESSIVITÉ
========================================================= */

if(
text.includes("ahah") ||
text.includes("HAHA") ||
text.includes("mdrrrr") ||
text.includes("PTDRRR")
){

energy += 3;
affection += 1;

}

/* =========================================================
MESSAGES ULTRA COURTS
========================================================= */

const wordCount =
text.split(" ").length;

if(wordCount <= 2){

coldness += 2;

}

/* =========================================================
POINTS DE SUSPENSION
========================================================= */

if(
text.includes("...")
){

tension += 1;

}

/* =========================================================
MULTIPLES QUESTIONS
========================================================= */

if(
text.includes("???")
){

tension += 3;

}

/* =========================================================
DOUCEUR GLOBALE
========================================================= */

const softPattern =
/(bonne nuit|prends soin|coeur|cœur|bb|babe|princesse|mon ange|love|jtm|je t'aime|tu me manques|trop beau|trop belle)/gi;

const softness =
(text.match(softPattern) || []).length;

affection += softness * 4;

/* =========================================================
AGRESSIVITÉ
========================================================= */

const aggressivePattern =
/(tg|ta gueule|ferme la|casse toi|bloque|bloqué|flemme|forceur|gênant|arrête|stop)/gi;

const aggression =
(text.match(aggressivePattern) || []).length;

coldness += aggression * 5;
tension += aggression * 3;

/* =========================================================
SCORE FINAL
========================================================= */

const finalScore =

(
energy +
affection +
investment
)

-

(
coldness +
tension
);

/* =========================================================
TEXTES
========================================================= */

const acquittedTexts = [

"Très forte énergie émotionnelle détectée. La conversation paraît sincère et investie.",

"La vibe générale paraît naturelle, affectueuse et chaleureuse.",

"Le tribunal détecte beaucoup de douceur et d’intérêt émotionnel.",

"Très bonne connexion émotionnelle détectée."

];

const guiltyTexts = [

"La conversation paraît émotionnellement mitigée.",

"Le mood général semble hésitant ou difficile à lire.",

"Le tribunal détecte une énergie émotionnelle instable.",

"La dynamique relationnelle paraît floue."

];

const condemnedTexts = [

"Très forte froideur émotionnelle détectée.",

"La conversation paraît distante ou déséquilibrée.",

"Le tribunal détecte une énergie tendue ou sèche.",

"Ghosting ou désintérêt émotionnel potentiel détecté."

];

/* =========================================================
VERDICT FINAL
========================================================= */

if(finalScore >= 8){

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

else if(finalScore <= 0){

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

/* =========================================================
DÉTAILS
========================================================= */

analysis.innerHTML +=

`

<br><br>

⚡ Énergie :
${Math.max(0,Math.round(energy))}

<br>

💕 Affection :
${Math.max(0,Math.round(affection))}

<br>

✨ Investissement :
${Math.max(0,Math.round(investment))}

<br>

❄️ Froideur :
${Math.max(0,Math.round(coldness))}

<br>

🌧️ Tension :
${Math.max(0,Math.round(tension))}

`;

/* =========================================================
RESET AUTO
========================================================= */

setTimeout(()=>{

textarea.value = "";

document
.getElementById("preview")
.style.display = "none";

},700);

/* =========================================================
ANALYSES GRATUITES
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