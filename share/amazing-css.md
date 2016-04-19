# 神奇的 CSS
## 正方形
```
#square {
	width: 150px;
	height: 150px;
	background-color: #1fa561;
}
```
## 长方形
```
#rectangle {
	width: 300px;
	height: 150px;
	background-color: #1fa561;
}
```
## 圆形
```
#circle {
	width: 100px;
	height: 100px;
	background: #1fa561;
	-moz-border-radius: 50px;
	-webkit-border-radius: 50px;
	border-radius: 50px;
	background-color: #1fa561;
}
```
## 椭圆形
```
#circle {
	width: 200px;
	height: 100px;
	background: #1fa561;
	-moz-border-radius: 50px;
	-webkit-border-radius: 50px;
	border-radius: 50px;
	background-color: #1fa561;
}
```
## 三角形
```
#triangle {
	width: 0;
	height: 0;
	border-left: 50px solid transparent;
	border-right: 50px solid transparent;
	border-bottom: 100px solid #1fa561;
}
```
## 梯形
```
#trapezoid {
	border-bottom: 100px solid #1fa561;
	border-left: 50px solid transparent;
	border-right: 50px solid transparent;
	height: 0;
	width: 100px;
}
```
## 平行四边形
```
#parallelogram {
	width: 150px;
	height: 100px;
	-webkit-transform: skew(20deg);
	-moz-transform: skew(20deg);
	-o-transform: skew(20deg);
	background: #1fa561;
}
```
## 六角星
```
#star-six {
	width: 0;
	height: 0;
	border-left: 50px solid transparent;
	border-right: 50px solid transparent;
	border-bottom: 100px solid #1fa561;
	position: relative;
}
#star-six:after {
	width: 0;
	height: 0;
	border-left: 50px solid transparent;
	border-right: 50px solid transparent;
	border-top: 100px solid #1fa561;
	position: absolute;
	content: "";
	top: 30px;
	left: -50px;
}
```
## 五角星
```
#star-five {
   margin: 50px 0;
   position: relative;
   display: block;
   color: #1fa561;
   width: 0px;
   height: 0px;
   border-right:  100px solid transparent;
   border-bottom: 70px  solid #1fa561;
   border-left:   100px solid transparent;
   -moz-transform:    rotate(35deg);
   -webkit-transform: rotate(35deg);
   -ms-transform:     rotate(35deg);
   -o-transform:      rotate(35deg);
}
#star-five:before {
   content: '';
   border-bottom: 80px solid #1fa561;
   border-left: 30px solid transparent;
   border-right: 30px solid transparent;
   position: absolute;
   height: 0;
   width: 0;
   top: -45px;
   left: -65px;
   display: block;
   -webkit-transform: rotate(-35deg);
   -moz-transform:    rotate(-35deg);
   -ms-transform:     rotate(-35deg);
   -o-transform:      rotate(-35deg);
}
#star-five:after {
   content: '';
   position: absolute;
   display: block;
   color: #1fa561;
   top: 3px;
   left: -105px;
   width: 0px;
   height: 0px;
   border-right: 100px solid transparent;
   border-bottom: 70px solid #1fa561;
   border-left: 100px solid transparent;
   -webkit-transform: rotate(-70deg);
   -moz-transform:    rotate(-70deg);
   -ms-transform:     rotate(-70deg);
   -o-transform:      rotate(-70deg);
}
```
## 五边形
```
#pentagon {
    position: relative;
    width: 54px;
    border-width: 50px 18px 0;
    border-style: solid;
    border-color: #1fa561 transparent;
}
#pentagon:before {
    content: "";
    position: absolute;
    height: 0;
    width: 0;
    top: -85px;
    left: -18px;
    border-width: 0 45px 35px;
    border-style: solid;
    border-color: transparent transparent #1fa561;
}
```
## 六边形
```
#hexagon {
	width: 100px;
	height: 55px;
	background: #1fa561;
	position: relative;
}
#hexagon:before {
	content: "";
	position: absolute;
	top: -25px;
	left: 0;
	width: 0;
	height: 0;
	border-left: 50px solid transparent;
	border-right: 50px solid transparent;
	border-bottom: 25px solid #1fa561;
}
#hexagon:after {
	content: "";
	position: absolute;
	bottom: -25px;
	left: 0;
	width: 0;
	height: 0;
	border-left: 50px solid transparent;
	border-right: 50px solid transparent;
	border-top: 25px solid #1fa561;
}
```
## 心形
```
#heart {
    position: relative;
    width: 100px;
    height: 90px;
}
#heart:before,
#heart:after {
    position: absolute;
    content: "";
    left: 50px;
    top: 0;
    width: 50px;
    height: 80px;
    background: #1fa561;
    -moz-border-radius: 50px 50px 0 0;
    border-radius: 50px 50px 0 0;
    -webkit-transform: rotate(-45deg);
       -moz-transform: rotate(-45deg);
        -ms-transform: rotate(-45deg);
         -o-transform: rotate(-45deg);
            transform: rotate(-45deg);
    -webkit-transform-origin: 0 100%;
       -moz-transform-origin: 0 100%;
        -ms-transform-origin: 0 100%;
         -o-transform-origin: 0 100%;
            transform-origin: 0 100%;
}
#heart:after {
    left: 0;
    -webkit-transform: rotate(45deg);
       -moz-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
         -o-transform: rotate(45deg);
            transform: rotate(45deg);
    -webkit-transform-origin: 100% 100%;
       -moz-transform-origin: 100% 100%;
        -ms-transform-origin: 100% 100%;
         -o-transform-origin: 100% 100%;
            transform-origin :100% 100%;
}
```
## 鸡蛋
```
#egg {
   display:block;
   width: 126px;
   height: 180px;
   background-color: #1fa561;
   -webkit-border-radius: 63px 63px 63px 63px / 108px 108px 72px 72px;
   border-radius:         50%  50%  50%  50%  / 60%   60%   40%  40%;
}
```
## 阴阳
```
#yin-yang {
	width: 96px;
	height: 48px;
	background: #eee;
	border-color: #1fa561;
	border-style: solid;
	border-width: 2px 2px 50px 2px;
	border-radius: 100%;
	position: relative;
}

#yin-yang:before {
	content: "";
	position: absolute;
	top: 50%;
	left: 0;
	background: #eee;
	border: 18px solid #1fa561;
	border-radius: 100%;
	width: 12px;
	height: 12px;
}

#yin-yang:after {
	content: "";
	position: absolute;
	top: 50%;
	left: 50%;
	background: #1fa561;
	border: 18px solid #eee;
	border-radius:100%;
	width: 12px;
	height: 12px;
}
```
## 月亮
```
#moon {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  box-shadow: 15px 15px 0 0 #1fa561;
}
```



