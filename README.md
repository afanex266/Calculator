# Calculator
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Calculator</title>
<style>
*{box-sizing:border-box}
body{margin:0;min-height:100vh;display:grid;place-items:center;background:#12171d;font-family:system-ui,sans-serif}
.calc{width:min(92vw,340px);background:#1b222a;border-radius:20px;padding:16px;box-shadow:0 10px 30px rgba(0,0,0,.3)}
#screen{background:#0e1318;color:#f5f7fa;border-radius:12px;padding:20px 14px;text-align:right;font-size:2.2rem;min-height:80px;word-break:break-all}
.keys{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-top:14px}
button{border:0;border-radius:14px;padding:18px 0;font-size:1.3rem;background:#27313b;color:#f5f7fa}
button:active{opacity:.6}
.op{background:#34414d}
.eq{background:#5b8cff;color:#0e1318;grid-row:span 2}
.zero{grid-column:span 2}
</style>
</head>
<body>
<div class="calc">
  <div id="screen">0</div>
  <div class="keys">
    <button class="op">C</button><button class="op">⌫</button><button class="op">÷</button><button class="op">×</button>
    <button>7</button><button>8</button><button>9</button><button class="op">-</button>
    <button>4</button><button>5</button><button>6</button><button class="op">+</button>
    <button>1</button><button>2</button><button>3</button><button class="eq">=</button>
    <button class="zero">0</button><button>.</button>
  </div>
</div>
<script>
let cur="0",prev=null,op=null,fresh=false;
const scr=document.getElementById("screen");

function calc(a,b,o){
  a=parseFloat(a);b=parseFloat(b);
  let r=o=="+"?a+b:o=="-"?a-b:o=="×"?a*b:(b==0?NaN:a/b);
  return isNaN(r)?"Error":String(parseFloat(r.toPrecision(12)));
}

function press(k){
  if(cur=="Error")cur="0";
  if(k>="0"&&k<="9"&&k.length==1){
    cur=(cur=="0"||fresh)?k:cur+k;fresh=false;
  }else if(k=="."){
    if(fresh){cur="0.";fresh=false;}
    else if(!cur.includes("."))cur+=".";
  }else if(k=="C"){
    cur="0";prev=null;op=null;fresh=false;
  }else if(k=="⌫"){
    cur=(cur.length>1&&!fresh)?cur.slice(0,-1):"0";
    if(cur=="-")cur="0";
  }else if(k=="="){
    if(op!==null&&prev!==null){
      cur=calc(prev,cur,op);prev=null;op=null;fresh=true;
    }
  }else{
    if(op!==null&&prev!==null&&!fresh)cur=calc(prev,cur,op);
    prev=cur;op=k;fresh=true;
  }
  scr.textContent=cur;
}

document.querySelector(".keys").addEventListener("click",e=>{
  if(e.target.tagName=="BUTTON")press(e.target.textContent);
});
</script>
</body>
</html>