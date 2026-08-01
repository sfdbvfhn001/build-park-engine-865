# 石头剪刀布



## 说明

与电脑对战石头剪刀布，统计胜率。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>石头剪刀布</title>

<style>

body{text-align:center;font-family:Arial;margin:30px;background:#e8eaf6}

.choices{display:flex;justify-content:center;gap:20px;margin:20px}

.choice{font-size:60px;padding:15px;border-radius:16px;cursor:pointer;background:white;box-shadow:0 4px 12px rgba(0,0,0,0.1);border:none;transition:transform 0.2s}

.choice:hover{transform:scale(1.15);box-shadow:0 6px 20px rgba(0,0,0,0.2)}

.result{font-size:24px;margin:20px;min-height:40px}

.score{font-size:18px;color:#555}

.com{font-size:48px}

</style></head>

<body>

<h2>✊✋✌ 石头剪刀布</h2>

<div class="choices">

  <button class="choice" onclick="play('rock')">✊</button>

  <button class="choice" onclick="play('paper')">✋</button>

  <button class="choice" onclick="play('scissors')">✌</button>

</div>

<div class="com">电脑: <span id="computer">❓</span></div>

<div class="result" id="result"></div>

<div class="score">胜: <span id="win">0</span> | 平: <span id="draw">0</span> | 负: <span id="lose">0</span></div>



<script>

let w=0,d=0,l=0;

const icons={rock:"✊",paper:"✋",scissors:"✌"};



function play(user){

  let comp=Object.keys(icons)[Math.floor(Math.random()*3)];

  document.getElementById("computer").textContent=icons[comp];

  let res="";

  if(user===comp){res="平局!";d++;}

  else if((user==="rock"&&comp==="scissors")||(user==="paper"&&comp==="rock")||(user==="scissors"&&comp==="paper")){res="你赢了! 🎉";w++;}

  else{res="电脑赢了! 😢";l++;}

  document.getElementById("result").textContent=res;

  document.getElementById("win").textContent=w;

  document.getElementById("draw").textContent=d;

  document.getElementById("lose").textContent=l;

}

</script></body></html>

```

