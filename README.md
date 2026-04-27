<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body { margin:0; overflow:hidden; }
canvas { background:#1e7f3f; display:block; }

.controls {
  position: fixed;
  bottom: 20px;
  width: 100%;
}

.btn {
  width: 60px;
  height: 60px;
  background: rgba(255,255,255,0.5);
  border-radius: 50%;
  position: absolute;
}

#up { left: 60px; bottom: 120px; }
#down { left: 60px; bottom: 0px; }
#left { left: 0px; bottom: 60px; }
#right { left: 120px; bottom: 60px; }

#kick { right: 80px; bottom: 60px; }
#dash { right: 0px; bottom: 60px; }
</style>
</head>
<body>

<canvas id="game"></canvas>

<div class="controls">
  <div id="up" class="btn"></div>
  <div id="down" class="btn"></div>
  <div id="left" class="btn"></div>
  <div id="right" class="btn"></div>

  <div id="kick" class="btn"></div>
  <div id="dash" class="btn"></div>
</div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let player = {x:100,y:200,size:15,speed:3};
let ball = {x:200,y:200,size:10,dx:0,dy:0};

let keys = {};

// Touch controls
["up","down","left","right","kick","dash"].forEach(id=>{
  let el = document.getElementById(id);
  el.ontouchstart = ()=> keys[id]=true;
  el.ontouchend = ()=> keys[id]=false;
});

function update(){
  let sp = keys["dash"] ? 6 : 3;

  if(keys["up"]) player.y -= sp;
  if(keys["down"]) player.y += sp;
  if(keys["left"]) player.x -= sp;
  if(keys["right"]) player.x += sp;

  let dx = ball.x - player.x;
  let dy = ball.y - player.y;
  let dist = Math.sqrt(dx*dx+dy*dy);

  if(dist < 25){
    if(keys["kick"]){
      ball.dx = dx * 0.6;
      ball.dy = dy * 0.6;
    } else {
      ball.dx = dx * 0.2;
      ball.dy = dy * 0.2;
    }
  }

  ball.x += ball.dx;
  ball.y += ball.dy;

  ball.dx *= 0.98;
  ball.dy *= 0.98;
}

function draw(){
  ctx.clearRect(0,0,canvas.width,canvas.height);

  ctx.fillStyle="blue";
  ctx.beginPath();
  ctx.arc(player.x,player.y,15,0,Math.PI*2);
  ctx.fill();

  ctx.fillStyle="white";
  ctx.beginPath();
  ctx.arc(ball.x,ball.y,10,0,Math.PI*2);
  ctx.fill();
}

function loop(){
  update();
  draw();
  requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>
