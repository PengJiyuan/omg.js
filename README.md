<!-- <img src=".github/logo.png" /> -->

# omg.js
[![](https://travis-ci.org/PengJiyuan/omg.js.svg?branch=master)](https://travis-ci.org/PengJiyuan/omg.js)
[![][npm-image]][npm-url]
[![][downloads-image]][downloads-url]

Oh my goodness! Awesome Canvas Render Library. [Demo](https://pengjiyuan.github.io/omg.js)

## Install

**NPM**    

`npm install omg.js --save`

```javascript
const OMG = require('omg.js');
console.log(OMG); // ...
```

**DOWNLOAD**    

[Source Code](dist/omg.min.js)

```html
<script src="path/omg.min.js"></script>
<script>
  console.log(OMG);// ...
</script>
```

## Example

```javascript
import omg from 'omg.js';

const stage = omg({
  element: document.getElementById('canvas'),
  width: 500,
  height: 500,
  enableGlobalTranslate: true,
  enableGlobalScale: true
});

const rect = stage.rectangle({
  x: 120,
  y: 120,
  width: 200,
  height: 200,
  color: '#'+(~~(Math.random()*(1<<24))).toString(16)
}).on('mousedown', function() {
  console.log('click rect2');
}).on('mouseenter', function() {
  rect.color = '#'+(~~(Math.random()*(1<<24))).toString(16);
  stage.redraw();
}).on('mouseleave', function() {
  rect.color = '#'+(~~(Math.random()*(1<<24))).toString(16);
  stage.redraw();
}).on('dragin', function() {
  console.log('drag in rect2');
  rect.color = '#ffffff';
  stage.redraw();
}).on('dragout', function() {
  console.log('drag out rect2');
  rect.color = '#'+(~~(Math.random()*(1<<24))).toString(16);
  stage.redraw();
}).on('drop', function() {
  console.log('you drop on the rect2!');
  rect.color = '#000';
  stage.redraw();
}).config({
  drag: true,
  changeIndex: true
});

stage.addChild(rect);

stage.show();
```

## Usage
```javascript
import omg from 'omg.js';

// Init
const stage = omg({
  element: document.getElementById('canvas'),
  width: 500,
  height: 500,
  enableGlobalTranslate: true, // enable global drag events.
  enableGlobalScale: true, // enable global scale events.
});

stage.getVersion(); // v x.x.x
```

### Shapes

**Rectangle**

```javascript
const rect = stage.rectangle({
  x: 0,
  y: 0,
  width: 110,
  height: 110,
  color: '#514022'
});
```

**Line**

```javascript
const line = stage.line({
  x: 200,
  y: 100,
  endX: 400,
  endY: 420
});
```

**Polyline**
```javascript
const polyline = stage.line({
  matrix: [
    [10, 180],
    [40, 50],
    [80, 180],
    [90, 80],
    [110, 100],
    [140, 50],
    [260, 180]
  ]
});
```

**Polygon**
```javascript
const polygon = stage.polygon({
  matrix: [
    [310, 120],
    [360, 120],
    [348, 230],
    [250, 340],
    [146, 200]
  ],
  color: 'black',
  style: 'stroke',
  lineWidth: 4
});
```

**Image**

```javascript
const image = stage.image({
  x: 0,
  y: 0,
  width: 800,
  height: 500,
  src: './img/timg.jpg'
});

// For more detail, check canvas api.
const image3 = stage.image({
  x: 200,
  y: 200,
  width: 97,
  height: 110,
  sliceX: 5,
  sliceY: 0,
  sliceWidth: 97,
  sliceHeight: 110,
  src: './img/action.png'
});
```

**Text**

```javascript
const text = stage.text({
  x: 300,
  y: 40,
  width: 150,
  height: 40, // x, y, width, height指定了一个矩形， 文字就在其中
  paddingTop: 8, // 用于调整文字在矩形中的位置
  center: true,  // 文字居中
  backgroundColor: 'blue', // 给文字添加背景颜色(矩形)
  font: 'italic bold 20px arial,sans-serif', // 文字样式
  text: 'Hello stage', // 文字内容
  color: '#fff', // 文字颜色
  style: 'fill' // fill -- 填充， stroke -- 描边
});
```

**Circle**

```javascript
const arc = stage.arc({
  x: 400,
  y: 400,
  radius: 30,
  color: 'rgba(255, 255, 255, 0.5)',
  style: 'fill' // fill -- 填充， stroke -- 描边
});
```

**Sector**

```javascript
const arcb = stage.arc({
  x: 130,
  y: 380,
  radius: 100,
  startAngle: 45,
  endAngle: 165,
  color: '#512854',
  style: 'fill'
});
```

### Add Event

#### global events

Global event (mousedown, mousemove) for whole omg.

```javascript
stage.mousedown(function(e) {
  console.log(stage.utils.getPos(e));
});

stage.mousemove(function(e) {
  console.log(stage.utils.getPos(e));
});
```

#### shape events

All types supported:

* mousedown
* mouseup
* mouseenter
* mouseleave
* mousemove'
* drag
* dragend
* dragin
* dragout
* drop

Support chain call.

```javascript
/*!
 * @cur: current shape.
 */
shape.on('mousedown', function( cur ) {
  console.log('you click rect');
}).on('mousemove', function( cur ) {
  console.log('you move!');
}).on('mouseleave', function( cur ) {
  console.log('you leave!');
}).drag(true).config(){...};
```

### config

```javascript
rect.config({
  drag: true, // 允许形状(对象)被拖拽  
  changeIndex: true， // 在形状被选中的时候允许改变个形状展示的顺序
  fixed: true, // 免受globalTranslate的影响
  bg: true // 作为bg的话无法绑定事件，可触发globalTranslate
});
```

### Add shapes to stage.

```javascript
stage.addChild(rect);
stage.addChild(line);

// or

stage.addChild([rect, line, arc1, text1]);
```

### Remove shapes from stage.

```javascript
stage.removeChild(rect);

stage.removeChild([rect, arc, line]);

stage.removeFirstChild();

stage.removeLastChild();

stage.removeAllChilds();
```


### Show
Draw and trigger events.

```javascript
stage.show();
```

If you call function addChild or removeChild and bind some new events or unbind some events, you shold reset events trigger by:
```javascript
stage.show()
```
or
```javascript
stage.draw();
stage._events.triggerEvents();
```

### Draw and Redraw
```javascript
stage.draw();
stage.redraw();
```

### Animation

#### Global Animation

```javascript
function go() {
  rect.x++;
  line.y = line.y + 2;
  arc.radius++;
}

const a = stage.animate(go);
```

#### Shap Animation

[Demo](https://pengjiyuan.github.io/omg.js/animation.html)

```javascript
/**
 * @param: {keys | Object}   -- the end value of your tween
 * @param: {config | Object} -- the tween settings
 */
shape.animateTo({
  x: 100,
  y: 100,
  width: 200,
  height: 200
}, {
  duration: 1000, //default = 500
  delay: 500, // defalut = 0
  easing: 'bounceOut', // defalut = 'linear'
  onStart: function(keys) {
    /**
     * @param: keys
     * the values during you tween
     * same below
     */
    console.log(keys.x, keys.y, keys.width, keys.height);
  }, // defalut = undefined
  onUpdate: function(keys) {
    console.log(keys.x, keys.y, keys.width, keys.height);
  }, // defalut = undefined
  onFinish: function(keys) {
    console.log(keys.x, keys.y, keys.width, keys.height);
  }, // defalut = undefined
});
```

#### Easing Types
* linear
* quadIn
* quadOut
* quadInOut
* cubicIn
* cubicOut
* cubicInOut
* quartIn
* quartOut
* quartInOut
* quintIn
* quintOut
* quintInOut
* sineIn
* sineOut
* sineInOut
* bounceOut
* bounceIn
* bounceInOut

#### Clear Animation
```javascript
stage.clearAnimation();
```

### FPS

#### fpsOn

If you use some animations and want to know the fps, just call function `fpsOn` to get fps.

```javascript
stage.fpsOn(function(fps) {
  console.log(fps);
});
```

#### Fps off
```javascript
stage.fpsOff();
```

## [CHANGELOG](./.github/CHANGELOG.md)

## [MIT](./LICENSE)

[npm-image]: https://img.shields.io/npm/v/omg.js.svg
[npm-url]: https://npmjs.org/package/omg.js
[downloads-image]: https://img.shields.io/npm/dm/omg.js.svg
[downloads-url]: https://npmjs.org/package/omg.js
