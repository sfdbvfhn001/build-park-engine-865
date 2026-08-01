# 拖拽元素到画布



## 说明

将左侧的形状拖到右侧画布中，在画布上自由放置。实现了一个基本的"拖放式设计器"原型。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head>

<meta charset="UTF-8">

<title>拖拽到画布</title>

<style>

  body { font-family: Arial; display: flex; gap: 20px; padding: 20px; background: #f0f0f0; }

  .sidebar { background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }

  .sidebar h3 { margin-top: 0; }

  .shape { width: 60px; height: 60px; margin: 10px 0; cursor: grab; display: flex; align-items: center; justify-content: center; font-size: 30px; border-radius: 8px; user-select: none; }

  .canvas { flex: 1; min-height: 500px; background: white; border: 3px dashed #ccc; border-radius: 12px; position: relative; overflow: hidden; }

  .canvas-item { position: absolute; width: 60px; height: 60px; display: flex; align-items: center; justify-content: center; font-size: 28px; cursor: move; border-radius: 8px; border: 2px solid transparent; }

  .canvas-item:hover { border-color: #2196F3; }

  .toolbar { margin: 10px 0; }

  .toolbar button { padding: 8px 16px; border: none; border-radius: 6px; cursor: pointer; background: #2196F3; color: white; }

</style>

</head>

<body>

<div class="sidebar">

  <h3>📦 元素库</h3>

  <div class="shape" draggable="true" data-type="🔴" style="background:#ffebee">🔴</div>

  <div class="shape" draggable="true" data-type="🔵" style="background:#e3f2fd">🔵</div>

  <div class="shape" draggable="true" data-type="🟢" style="background:#e8f5e9">🟢</div>

  <div class="shape" draggable="true" data-type="⭐" style="background:#fff8e1">⭐</div>

  <div class="shape" draggable="true" data-type="❤️" style="background:#fce4ec">❤️</div>

  <div class="toolbar" style="margin-top:20px">

    <button onclick="clearCanvas()">清空画布</button>

    <button onclick="saveCanvas()">保存截图</button>

  </div>

</div>

<div class="canvas" id="canvas"></div>



<script>

let itemId = 0;

const canvas = document.getElementById("canvas");



// 从侧边栏拖拽到画布

document.querySelectorAll(".shape").forEach(shape => {

  shape.addEventListener("dragstart", e => {

    e.dataTransfer.setData("text/plain", shape.dataset.type);

  });

});



// 画布上的项目: 允许拖动位置

canvas.addEventListener("dragover", e => e.preventDefault());



canvas.addEventListener("drop", e => {

  e.preventDefault();

  let type = e.dataTransfer.getData("text/plain");

  if (!type) return;



  let rect = canvas.getBoundingClientRect();

  let x = e.clientX - rect.left - 30; // 居中偏移

  let y = e.clientY - rect.top - 30;



  addItem(type, x, y);

});



function addItem(type, x, y) {

  let div = document.createElement("div");

  div.className = "canvas-item";

  div.textContent = type;

  div.id = "item-" + (++itemId);

  div.style.left = Math.max(0, Math.min(x, 700)) + "px";

  div.style.top  = Math.max(0, Math.min(y, 440)) + "px";

  div.draggable = true;



  // 画布内项目也能拖拽

  let offsetX = 0, offsetY = 0;

  div.addEventListener("mousedown", e => {

    offsetX = e.clientX - div.getBoundingClientRect().left;

    offsetY = e.clientY - div.getBoundingClientRect().top;

    div.style.cursor = "grabbing";



    function onMove(ev) {

      let crect = canvas.getBoundingClientRect();

      div.style.left = Math.max(0, Math.min(ev.clientX - crect.left - 30, 700)) + "px";

      div.style.top  = Math.max(0, Math.min(ev.clientY - crect.top - 30, 440)) + "px";

    }

    function onUp() {

      div.style.cursor = "move";

      document.removeEventListener("mousemove", onMove);

      document.removeEventListener("mouseup", onUp);

    }

    document.addEventListener("mousemove", onMove);

    document.addEventListener("mouseup", onUp);

  });



  // 右键删除

  div.addEventListener("contextmenu", e => {

    e.preventDefault();

    div.remove();

  });



  canvas.appendChild(div);

}



function clearCanvas() {

  canvas.innerHTML = "";

  itemId = 0;

}



function saveCanvas() {

  alert("提示：可用浏览器截图工具保存，或集成 html2canvas 库实现导出");

}

</script>

</body>

</html>

```



## 教学重点

- 侧边栏 → 画布的拖放数据流：`dragstart` 设数据 → `drop` 接收并创建新元素

- 画布内元素用 `mousedown/mousemove/mouseup` 实现自由拖动

- `getBoundingClientRect()` 获取画布位置，偏移量实现相对定位

- `contextmenu` 右键事件自定义删除

- 位置限制 `Math.max(0, Math.min(x, max))` 防止拖出边界

