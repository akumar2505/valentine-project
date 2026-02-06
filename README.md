<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Be My Valentine</title>

<style>
*{box-sizing:border-box}

body{
 margin:0;
 height:100vh;
 display:flex;
 justify-content:center;
 align-items:center;
 background:#ffe6ec;
 font-family:Arial;
 overflow:hidden;
}

.card{
 position:relative;
 background:white;
 padding:30px 35px 40px;
 border-radius:20px;
 text-align:center;
 box-shadow:0 0 20px rgba(0,0,0,.2);
 width:320px;
 z-index:2;
}

img{width:120px}
h2{margin:15px 0}

.btns{
 position:relative;
 width:220px;
 margin:10px auto 0;
 height:50px;
}

button{
 border:none;
 border-radius:10px;
 cursor:pointer;
 font-size:18px;

 width:90px;
 height:44px;
 line-height:44px;

 padding:0;
 white-space:nowrap;
}

/* YES */
#yes{
 position:absolute;
 left:0;
 background:#22c55e;
 color:white;
}

/* NO */
#no{
 position:absolute;
 right:0;
 background:#ef4444;
 color:white;
 z-index:3;
 transition:left .25s ease, top .25s ease;
}

/* Popup */
#popup{
 position:fixed;
 inset:0;
 background:rgba(0,0,0,.5);
 display:none;
 justify-content:center;
 align-items:center;
 z-index:5;
}

.popbox{
 background:white;
 padding:30px;
 border-radius:18px;
 text-align:center;
 min-width:260px;
}

.popbox button{
 width:auto;
 height:auto;
 line-height:normal;
 padding:10px 20px;
 background:#22c55e;
 color:white;
}

/* Confetti */
#confetti{
 position:fixed;
 inset:0;
 pointer-events:none;
}
</style>
</head>

<body>

<canvas id="confetti"></canvas>

<div class="card">

<img src="https://media.tenor.com/0AVbKGY_MxMAAAAi/tkthao219-bubududu.gif">

<h2>Will you be my Valentine? ❤️</h2>

<div class="btns">
  <button id="yes" onclick="yes()">Yes</button>
  <button id="no">No</button>
</div>

</div>

<div id="popup">
  <div class="popbox">
    <h1>😏 I knew it!</h1>
    <p>You love me! Ab chocolate treat pakki 🍫❤️</p>
    <button onclick="closePop()">Okay 😄</button>
  </div>
</div>

<script>
const noBtn=document.getElementById("no");

let freed=false;
let w,h;
let startX,startY;

// Confetti control
let running=false;
let animId=null;

// Hover on NO
noBtn.addEventListener("mouseenter", ()=>{
 if(!freed){
   const r=noBtn.getBoundingClientRect();
   w=r.width;
   h=r.height;
   startX=r.left;
   startY=r.top;

   noBtn.style.width=w+"px";
   noBtn.style.height=h+"px";
   noBtn.style.lineHeight=h+"px";

   noBtn.style.position="fixed";
   noBtn.style.left=r.left+"px";
   noBtn.style.top=r.top+"px";
   freed=true;
 }
 moveNo();
});

// Move NO anywhere
function moveNo(){
 const maxX = window.innerWidth - w;
 const maxY = window.innerHeight - h;

 noBtn.style.left = Math.random()*maxX+"px";
 noBtn.style.top  = Math.random()*maxY+"px";
}

// YES click
function yes(){
 if(freed){
   noBtn.style.left=startX+"px";
   noBtn.style.top=startY+"px";
 }
 document.getElementById("popup").style.display="flex";
 startConfetti();
}

// Close popup + stop celebration
function closePop(){
 document.getElementById("popup").style.display="none";
 stopConfetti();
}

/* CONFETTI SYSTEM */
const canvas=document.getElementById("confetti");
const ctx=canvas.getContext("2d");
resize();
window.onresize=resize;

let pieces=[];

function resize(){
 canvas.width=innerWidth;
 canvas.height=innerHeight;
}

function startConfetti(){
 running=true;
 pieces=[];
 for(let i=0;i<150;i++){
   pieces.push({
     x:Math.random()*canvas.width,
     y:Math.random()*canvas.height,
     r:Math.random()*6+4,
     c:`hsl(${Math.random()*360},100%,60%)`,
     s:Math.random()*3+2
   });
 }
 draw();
}

function stopConfetti(){
 running=false;
 cancelAnimationFrame(animId);
 ctx.clearRect(0,0,canvas.width,canvas.height);
}

function draw(){
 if(!running) return;

 ctx.clearRect(0,0,canvas.width,canvas.height);
 pieces.forEach(p=>{
   ctx.beginPath();
   ctx.fillStyle=p.c;
   ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
   ctx.fill();
   p.y+=p.s;
   if(p.y>canvas.height) p.y=0;
 });

 animId=requestAnimationFrame(draw);
}
</script>

</body>
</html>
