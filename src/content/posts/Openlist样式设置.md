---
published: 2026-11-20
title: Openlist样式设置
pinned: false
description: 个人 Openlist 样式设置分享&备份
tags: [Openlist]
category: 教程
image: ./images/openlist.png
---
成品如上图，其中音乐播放器可收缩

## 参考样式
- Jetbrain maple mono medium 字体
- 随机图API，横、竖屏使用地址不同，显示效果不同，API需自行配置，下方为空白
- Aplayer + Meting 音乐播放器，Meting API请自行配置，下方为空白，注意参数是否对应
- 一定程度的动画、样式效果

## 代码
将以下内容分别复制至对应位置，保存即可
### 自定义头部：

```
<link rel="preconnect" href="https://fontsapi.zeoseven.com" crossorigin>
<link rel="stylesheet" href="https://fontsapi.zeoseven.com/521/main/result.css">

<!-- APlayer -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js"></script>

<!-- MetingJS 自定义 API：必须先于 Meting.min.js 加载 -->
<script>
window.meting_api =
  "https://api.example.com/api?server=:server&type=:type&id=:id&auth=:auth&r=:r";
</script>
<script src="https://cdn.jsdelivr.net/npm/meting@2/dist/Meting.min.js"></script>

<style>
:root{
 --ol-wallpaper-opacity:.76;
 --ol-wallpaper-fade:1.2s;
 --ol-glass-bg:rgba(255,255,255,.24);
 --ol-glass-blur:16px;
 --ol-glass-saturate:1.35
}
.hope-ui-dark{--ol-glass-bg:rgba(16,20,32,.28)}

html,body,#root,.App,.body,.footer{
 background:transparent!important
}
body,*{
 font-family:"JetBrains Maple Mono",monospace!important;
 font-weight:500
}

/* Logo 行及导航透明 */
#root .header,#root .body>.nav{
 background:transparent!important;
 background-image:none!important;
 box-shadow:none!important;
 backdrop-filter:none!important;
 -webkit-backdrop-filter:none!important
}
#root .body>.nav::after{display:none!important}
#root{position:relative;z-index:1}

/* 壁纸层 */
#openlist-wallpaper-stage{
 position:fixed;
 inset:0;
 z-index:0;
 overflow:hidden;
 pointer-events:none
}
.openlist-wallpaper-layer{
 position:absolute;
 inset:0;
 opacity:0;
 background:center/cover no-repeat;
 transition:opacity var(--ol-wallpaper-fade) ease
}
.openlist-wallpaper-layer.is-active{
 opacity:var(--ol-wallpaper-opacity)
}

/* 内容毛玻璃 */
.obj-box.hope-stack.hope-c-dhzjXW.hope-c-PJLV.hope-c-PJLV-igScBhH-css{
 background:var(--ol-glass-bg)!important;
 backdrop-filter:blur(var(--ol-glass-blur)) saturate(var(--ol-glass-saturate))!important;
 -webkit-backdrop-filter:blur(var(--ol-glass-blur)) saturate(var(--ol-glass-saturate))!important
}

/* 指定容器毛玻璃数值归零 */
.hope-c-PJLV.hope-c-PJLV-ikUPwEx-css,
.hope-center.hope-c-jKOUQW.hope-c-PJLV.hope-c-PJLV-ibZqGFV-css{
 --ol-glass-bg:transparent!important;
 --ol-glass-blur:0px!important;
 --ol-glass-saturate:1!important;
 background:transparent!important;
 backdrop-filter:none!important;
 -webkit-backdrop-filter:none!important
}

/* APlayer 固定位置及收缩 */
.aplayer .aplayer-body,
.aplayer.aplayer-withlist{
 bottom:0!important
}
.aplayer.aplayer-fixed.aplayer-narrow .aplayer-body{
 left:-66px!important
}
.aplayer.aplayer-fixed.aplayer-narrow .aplayer-body:hover{
 left:0!important
}

/* 减少动态效果 */
@media(prefers-reduced-motion:reduce){
 .openlist-wallpaper-layer{transition:none}
}
</style>

<script>
(()=>{
 const urls=[
  "https://example.com/横屏",
  "https://example.com/竖屏"
 ];

 const init=()=>{
  if(!document.body||
     document.getElementById("openlist-wallpaper-stage"))return;

  const stage=document.createElement("div");
  stage.id="openlist-wallpaper-stage";
  stage.setAttribute("aria-hidden","true");
  stage.innerHTML="<i></i><i></i>";
  [...stage.children].forEach(
   x=>x.className="openlist-wallpaper-layer"
  );
  document.body.prepend(stage);

  const layers=[...stage.children];
  const media=matchMedia("(orientation:portrait)");
  let active=1,seq=0;

  const change=()=>{
   const next=1-active;
   const token=++seq;
   const url=urls[+media.matches]+"?t="+Date.now();
   const img=new Image();

   img.onload=()=>{
    if(token!==seq)return;
    layers[next].style.backgroundImage=`url("${url}")`;
    requestAnimationFrame(()=>{
     if(token!==seq)return;
     layers[next].classList.add("is-active");
     layers[active].classList.remove("is-active");
     active=next;
    });
   };

   img.src=url;
  };

  change();

  if(media.addEventListener)
   media.addEventListener("change",change);
  else
   media.addListener(change);

  setInterval(change,600000);
 };

 if(document.readyState==="loading")
  document.addEventListener("DOMContentLoaded",init,{once:true});
 else init();
})();
</script>
```

### 自定义内容：
下方ID配置自己的，记得修改！

```
<meting-js fixed="true" autoplay="true" theme="#FFB7C5" list-folded="true" auto="https://music.163.com/#/playlist?id=0000000000"></meting-js>
```
