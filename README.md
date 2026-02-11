<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Valentine 💗</title>

<style>

body{
 margin:0;
 font-family:Arial, sans-serif;
 background:linear-gradient(#ffffff,#ffe4ec);
 display:flex;
 justify-content:center;
 align-items:center;
 height:100vh;
}

.wrapper{
 text-align:center;
 width:360px;
}

/* GRID */

.grid{
 display:grid;
 grid-template-columns:repeat(5,64px);
 gap:10px;
 justify-content:center;
 margin-bottom:25px;
}

.tile{
 width:64px;
 height:64px;
 border-radius:14px;
 background:#ffdce6;
 display:flex;
 justify-content:center;
 align-items:center;
 font-size:28px;
 font-weight:bold;
 color:#333;
}

.green{ background:#2ecc71; color:white;}
.yellow{ background:#f6c344; color:white;}
.gray{ background:#cfcfcf; color:white;}

/* KEYBOARD */

.kb{
 display:flex;
 flex-direction:column;
 gap:8px;
}

.row{
 display:flex;
 justify-content:center;
 gap:6px;
}

.key{
 background:#ffdce6;
 padding:12px;
 border-radius:10px;
 min-width:28px;
 text-align:center;
 cursor:pointer;
 font-weight:bold;
}

.key.green{background:#2ecc71;color:white}
.key.yellow{background:#f6c344;color:white}
.key.gray{background:#bdbdbd;color:white}

.wide{padding:12px 16px}

/* BUTTONS */

.controls{
 margin-top:15px;
}

.btn{
 background:#ff6fa5;
 border:none;
 padding:12px 18px;
 border-radius:12px;
 color:white;
 font-size:16px;
 margin:5px;
 cursor:pointer;
 box-shadow:0 4px #cc4f7d;
}

.btn:active{
 transform:translateY(2px);
 box-shadow:none;
}

</style>
</head>

<body>

<div class="wrapper">

<div id="grid" class="grid"></div>

<div id="keyboard" class="kb"></div>

<div class="controls">
<button class="btn" onclick="resetGame()">Play Again</button>
<button class="btn" onclick="giveUp()">Give Up?</button>
</div>

</div>

<script>

const answer="ilyaj";
const phrase="willyoubemyvalentine";

let row=0;
let current="";
const grid=document.getElementById("grid");
const kb=document.getElementById("keyboard");

const layout=[
"qwertyuiop",
"asdfghjkl",
"zxcvbnm"
];

let keyMap={};

/* BUILD GRID */

for(let i=0;i<30;i++){
 let d=document.createElement("div");
 d.className="tile";
 grid.appendChild(d);
}

/* BUILD KEYBOARD */

layout.forEach(line=>{
 let r=document.createElement("div");
 r.className="row";

 line.split("").forEach(letter=>{
  let k=document.createElement("div");
  k.className="key";
  k.textContent=letter;
  k.onclick=()=>press(letter);
  r.appendChild(k);
  keyMap[letter]=k;
 });

 kb.appendChild(r);
});

/* ENTER + DELETE ROW */

let r=document.createElement("div");
r.className="row";

let enter=document.createElement("div");
enter.textContent="ENTER";
enter.className="key wide";
enter.onclick=submit;
r.appendChild(enter);

let del=document.createElement("div");
del.textContent="⌫";
del.className="key wide";
del.onclick=()=>{
 current=current.slice(0,-1);
 draw();
};
r.appendChild(del);

kb.appendChild(r);

/* INPUT */

function press(l){
 if(current.length<5){
  current+=l;
  draw();
 }
}

function draw(){
 for(let i=0;i<5;i++){
  grid.children[row*5+i].textContent=current[i]||"";
 }
}

/* KEY COLOR PRIORITY */

function colorKey(letter,color){
 let k=keyMap[letter];
 if(!k) return;

 if(k.classList.contains("green")) return;
 if(k.classList.contains("yellow") && color==="gray") return;

 k.classList.remove("green","yellow","gray");
 k.classList.add(color);
}

/* SUBMIT */

function submit(){
 if(current.length!==5) return;

 for(let i=0;i<5;i++){

  let tile=grid.children[row*5+i];
  let l=current[i];
  let color;

  if(l===answer[i]) color="green";
  else if(answer.includes(l) || phrase.includes(l)) color="yellow";
  else color="gray";

  tile.classList.add(color);
  colorKey(l,color);
 }

 if(current===answer){
  setTimeout(()=>alert("You got it 💗"),300);
 }

 row++;
 current="";
}

/* RESET */

function resetGame(){
 location.reload();
}

/* GIVE UP */

function giveUp(){
 alert("Answer: "+answer.toUpperCase());
 location.reload();
}

</script>

</body>
</html>
