<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
    outline: none;
}

body,
div,
dl,
dt,
dd,
ul,
ol,
li,
h1,
h2,
h3,
h4,
h5,
h6,
pre,
code,
form,
fieldset,
legend,
input,
textarea,
p,
blockquote,
th,
td,
hr,
button,
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section,
i,
span,
a {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
}

article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
nav,
section,
summary {
    display: block
}

[hidden] {
    display: none
}

html {
    font-family: "microsoft yahei", "Arial";
    -webkit-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
    font-size: 14px;
}

body {
    min-width: 320px;
    line-height: 20px;
    background: #efeff7;
    color: #666;
    overflow-x: hidden;
}

a:active,
a:hover,
a:link,
a:visited {
    outline: 0;
    text-decoration: none;
    color: inherit;
}

h1,
h2,
h3,
h4,
h5 {
    font-weight: normal;
    font-size: 14px;
    color: #333;
}

i,
em {
    font-style: normal;
}

mark {
    background: #ff0;
    color: #000
}

img {
    border: 0;
    max-width: 100%;
    width: 100%;
    height: auto;
}

input,
textarea,
button,
select {
    font-family: inherit;
    color: #666;
    font-size: inherit;
}

button,
html input[type="button"],
input[type="reset"],
input[type="submit"] {
    -webkit-appearance: button;
}

input[type="checkbox"],
input[type="radio"] {
    box-sizing: border-box;
    padding: 0
}

input[type="search"] {
    -webkit-appearance: textfield;
}

input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
    -webkit-appearance: none
}

button::-moz-focus-inner,
input::-moz-focus-inner {
    border: 0;
    padding: 0
}

::-webkit-input-placeholder {
    color: #a9a9a9;
}

:-moz-placeholder {
    color: #a9a9a9;
}

::-moz-placeholder {
    color: #a9a9a9;
}

:-ms-input-placeholder {
    color: #a9a9a9;
}

textarea {
    overflow: auto;
    vertical-align: top
}

table {
    border-collapse: collapse;
    border-spacing: 0
}

ol,
ul {
    list-style: none
}

.guangg {
    position: relative;
}

.guangg:after {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    width: 23px;
    height: 10px;
    background: url(//pics.lvjs.com.cn/mobile/img/icons/guangg.png) center;
    background-size: 100%;
    display: block;
}

@keyframes loading {
    from {
        transform: rotate(0deg);
        -webkit-transform: rotate(0deg)
    }
    to {
        transform: rotate(360deg);
        -webkit-transform: rotate(360deg)
    }
}

@-webkit-keyframes loading {
    from {
        transform: rotate(0deg);
        -webkit-transform: rotate(0deg)
    }
    to {
        transform: rotate(360deg);
        -webkit-transform: rotate(360deg)
    }
}

.lv-ld {
    position: fixed;
    top: 50%;
    left: 50%;
    width: 75px;
    height: 75px;
    border-radius: 4px;
    background: rgba(0, 0, 0, .6);
    color: #fff;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    white-space: nowrap;
    z-index: 9999;
}

.lv-ld>i.ld-cir,
.lv-ld>i.ld-bac {
    position: absolute;
    width: 30px;
    height: 30px;
    left: 23px;
    margin-top: 10px;
}

.lv-ld>i.ld-cir {
    background: url(//pics.lvjs.com.cn/mobile/img/loading/loading-circle.png) center center no-repeat;
    background-size: 29px;
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
    -webkit-animation: loading 1s linear 0s infinite;
    animation: loading 1s linear 0s infinite
}

.lv-ld>i.ld-bac {
    background: url(//pics.lvjs.com.cn/mobile/img/loading/loading-lv.png) center center no-repeat;
    background-size: 18px;
}

.lv-ld>span {
    position: absolute;
    bottom: 13px;
    font-size: 14px;
    left: 11px
}

.lv-ld.lv-ld-hd {
    display: none;
}

.lv-dot {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
    -webkit-transform: translate3d(-50%, -50%, 0);
}

.lv-dot>div {
    background-color: #d30775;
    width: 10px;
    height: 10px;
    border-radius: 100%;
    margin: 2px;
    display: inline-block;
    -webkit-animation: ball-beat .7s 0s infinite linear;
    animation: ball-beat .7s 0s infinite linear;
}

.lv-dot>div:nth-child(2n-1) {
    -webkit-animation-delay: .35s!important;
    animation-delay: .35s!important;
}

.lv-dot.lv-ld-hd {
    display: none;
}

@-webkit-keyframes ball-beat {
    from {
        opacity: .2;
        -webkit-transform: scale(.75);
        transform: scale(.75)
    }
    to {
        opacity: 1;
        -webkit-transform: scale(1);
        transform: scale(1)
    }
}

@keyframes ball-beat {
    from {
        opacity: .2;
        -webkit-transform: scale(.75);
        transform: scale(.75)
    }
    to {
        opacity: 1;
        -webkit-transform: scale(1);
        transform: scale(1)
    }
}


/*用scale实现上下左右直线1像素,线条颜色可层叠*/

.lv-bd,
.lv-bd-left,
.lv-bd-right,
.lv-bd-top,
.lv-bd-bottom {
    position: relative;
}

.lv-bd:after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    border: 1px solid #DDDDDD;
    width: 200%;
    height: 200%;
    transform: scale(0.5);
    transform-origin: 0 0;
    -webkit-transform: scale(0.5);
    -webkit-transform-origin: 0 0;
}

.lv-bd-top:after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    border-top: 1px solid #DDDDDD;
    width: 100%;
    transform: scaleY(0.5);
    transform-origin: 0 0;
    -webkit-transform: scaleY(0.5);
    -webkit-transform-origin: 0 0;
}

.lv-bd-bottom:after {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    border-bottom: 1px solid #DDDDDD;
    width: 100%;
    transform: scaleY(0.5);
    transform-origin: 0 0;
    -webkit-transform: scaleY(0.5);
    -webkit-transform-origin: 0 0;
}

.lv-bd-left:after {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    border-left: 1px solid #DDDDDD;
    height: 100%;
    transform: scaleX(0.5);
    transform-origin: 0 0;
    -webkit-transform: scaleX(0.5);
    -webkit-transform-origin: 0 0;
}

.lv-bd-right:after {
    content: "";
    position: absolute;
    right: 0;
    bottom: 0;
    border-right: 1px solid #DDDDDD;
    height: 100%;
    transform: scaleX(0.5);
    transform-origin: 0 0;
    -webkit-transform: scaleX(0.5);
    -webkit-transform-origin: 0 0;
}


/*如果线条是灰色的，可以用这个*/

.lv-bd2 {
    -webkit-border-image: url(data:image/gif;base64,R0lGODlhBQAFAPABANra2v///yH5BAUHAAEALAAAAAAFAAUAAAIHhB9pGatnCgA7) 2 stretch;
    border-style: solid;
    border-width: 0;
}

.lv-bd-all2 {
    border-width: 1px;
}

.lv-bd-left2 {
    border-left-width: 1px;
}

.lv-bd-right2 {
    border-right-width: 1px;
}

.lv-bd-top2 {
    border-top-width: 1px;
}

.lv-bd-bottom2 {
    border-bottom-width: 1px;
}


/*圆角小标签，颜色可层叠*/

.lv-label-wrap span {
    position: relative;
    padding: 0 4px;
    line-height: 18px;
    font-size: 12px;
    color: #FFCF7C;
    margin-right: 5px;
    display: inline-block;
}

.lv-label-wrap span:after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    border: 1px solid #FFCF7C;
    box-sizing: border-box;
    width: 200%;
    height: 200%;
    -webkit-transform: scale(.5);
    transform: scale(.5);
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
    border-radius: 6px;
}

.lv-label {
    position: relative;
    padding: 0 4px;
    line-height: 18px;
    font-size: 12px;
    color: #FFCF7C;
    margin-right: 5px;
    display: inline-block;
}

.lv-label:after {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    border: 1px solid #FFCF7C;
    box-sizing: border-box;
    width: 200%;
    height: 200%;
    -webkit-transform: scale(.5);
    transform: scale(.5);
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
    border-radius: 6px;
}


/* 点击列表页产品模块添加背景色的样式 */

.lvAddBgcolor:active {
    background: #e6f5ff!important;
}
</style>
