<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<title>我想你了 ☁️</title>
<style>
    *{margin:0;padding:0;box-sizing:border-box;}
    html,body{width:100%;height:100%;overflow:hidden;background:#fff;font-family:"微软雅黑",sans-serif;}
    .tip-box{
        position:fixed;
        width:200px;
        height:60px;
        display:flex;
        align-items:center;
        justify-content:center;
        font-size:15px;
        color:#333;
        border-radius:6px;
        box-shadow:0 2px 8px rgba(0,0,0,.15);
        user-select:none;
        animation:fadeIn .3s ease-out;
    }
    @keyframes fadeIn{from{opacity:0;transform:scale(.9);}to{opacity:1;transform:scale(1);}}
    #ctrl-info{
        position:fixed;
        top:10px;left:50%;
        transform:translateX(-50%);
        background:rgba(0,0,0,.65);
        color:#fff;
        padding:6px 14px;
        border-radius:4px;
        font-size:13px;
        z-index:9999;
    }
</style>
</head>
<body>
<div id="ctrl-info">按 Ctrl + C 清屏并停止创建</div>
<script>
const tips = ['早安呀','午安哦','晚安啦','多喝水','笑一笑','加油呀','休息下','深呼吸','放轻松','慢慢来','你很棒','别太累','要开心','会好的','向前看','别放弃','真不错','真厉害','真可爱','真勇敢','真聪明','真努力','真坚强','真善良','真漂亮','真帅气','真有趣','真幸福','真幸运','真温暖','今天好','明天更好','天气不错','风景很美','心情要好','照顾自己','爱你哟','想你啦','加油哦','没问题','没关系','别担心','放轻松','慢慢来','会成功','会实现','会幸福','要自信','要乐观','要积极','要坚持','要努力','要珍惜','要感恩','要善良','要微笑','要开心','要快乐','要健康','要平安','要顺利','要好运','要幸福','要甜蜜','要美好','阳光好','月光美','星星亮','风儿轻','云儿飘','花儿香','草儿绿','鸟儿唱','虫儿鸣','鱼儿游','世界美','生活好','每一天','都精彩','每一刻','都珍贵','每一分','都美好','每一秒','都幸福','别熬夜','早睡觉','吃早餐','多运动','多读书','多学习','多思考','多微笑','多感恩','多付出','多收获','多快乐','多幸福','多美好','多幸运','多平安','多健康','多顺利','多开心','多欢笑','多温暖','多甜蜜','多可爱','多帅气','多聪明','多勇敢','多坚强','多努力','多成功','多梦想','多希望','多未来','多憧憬','多期待','多美好','多精彩','多绚丽','多灿烂','多辉煌','多闪耀','多明亮','多温暖','多舒适','多惬意','多自在','多逍遥','多快乐','多幸福','多美满','多团圆','多和睦','多友爱','多真诚','多善良','多美好','多可爱','多迷人','多动人','多精彩','多难忘','多珍贵','多稀有','多特别','多独特'];
const bgColors = ['lightpink','skyblue','lightgreen','lavender','lightyellow','plum','coral','bisque','aquamarine','mistyrose','honeydew','peachpuff','paleturquoise','lavenderblush','oldlace','lemonchiffon','lightcyan','lightgray','lightpink','lightsalmon','lightseagreen','lightskyblue','lightslategray','lightsteelblue','lightyellow'];
function randRange(min,max){return Math.floor(Math.random()*(max-min+1))+min;}
function randItem(arr){return arr[randRange(0,arr.length-1)];}
function createTipBox(){
    const box = document.createElement('div');
    box.className = 'tip-box';
    box.style.left = randRange(0,window.innerWidth - 200)+'px';
    box.style.top  = randRange(0,window.innerHeight - 60)+'px';
    box.style.background = randItem(bgColors);
    box.textContent = randItem(tips);
    document.body.appendChild(box);
    setTimeout(()=>box.remove(),3000);
}
const workerCode = `
let timer=null;
self.onmessage=function(e){
    if(e.data==='start'){
        timer=setInterval(()=>self.postMessage('create'),10);
    }else if(e.data==='stop'){
        clearInterval(timer);timer=null;
    }
};`;
const blob = new Blob([workerCode],{type:'application/javascript'});
const worker = new Worker(URL.createObjectURL(blob));
worker.onmessage = ()=>createTipBox();
let running=true;worker.postMessage('start');
document.addEventListener('keydown',e=>{
    if(e.ctrlKey&&e.key.toLowerCase()==='c'){
        if(!running)return;
        running=false;worker.postMessage('stop');
        document.body.innerHTML='';
        const done=document.createElement('div');
        done.style.cssText='position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);font-size:28px;color:#666;';
        done.textContent='已清屏，刷新页面可重新播放';
        document.body.appendChild(done);
    }
});
</script>
</body>
</html>
