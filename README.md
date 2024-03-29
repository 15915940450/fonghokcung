# FONGHOKCUNG

#### 2023/4/20

###### Dynamic

- 斐波那契数列
- LIS, LCS
- 零钱兑换

#### 2023/3/7

###### dataset

    ```
    <div id="test" data-mydata="test">自定义属性data-</div>

    let t = document.querySelector("#test");
    t.setAttribute("data-test", "js-insert");


    let t = document.querySelector("#test");
    t.setAttribute("data-test", "js-insert");
    console.log(t.getAttribute("data-mydata"));

    let t = document.querySelector("#test");
    t.setAttribute("data-test", "js-insert");
    console.log(t.dataset["mydata"]); // test
    console.log(t.dataset["test"]); // js-insert
    ```
    > 浏览器会自动把data-*的属性名转为小写

###### 矩形包围盒碰撞检测

https://heptaluan.github.io/2020/11/28/Essay/31/
https://heptaluan.github.io/demos/example/blog/OBB.html

针对于这种情况，两个矩形的 OBB 检测我们可以使用分离轴定理（Separating Axis Theorem）来进行解决，所谓分离轴定理，即通过判断任意两个矩形在『任意角度下的投影是否均存在重叠』来判断是否发生碰撞

> 如果两个多边形在所有轴上的投影都发生重叠，则判定为碰撞，否则没有发生碰撞

```

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>OBB</title>
  <style>
    .a, .b {
      width: 350px;
      height: 150px;
      position: absolute;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .a {
      background: lightslategray;
      top: 355px;
      left: 171px;
    }

    .b {
      background: lightseagreen;
      transform: rotate(40deg);
      top: 220px;
      left: 400px;
    }
  </style>
</head>
<body>

  <div class="box">
    <div>元素 A 与 B 是否相交，<span class="text">true</span></div>
    <br>
    <button class="aBtn">调整 A 与 B 相交</button>
    <button class="bBtn">调整 A 与 B 分离</button>
  </div>

  <div class="a">A</div>
  <div class="b">B</div>


  <script>

    // OBB 算法
    class OBB {
      constructor(centerPoint, width, height, rotation) {
        this.centerPoint = centerPoint
        this.extents = [width / 2, height / 2]
        this.axes = [new Vector2(Math.cos(rotation), Math.sin(rotation)), new Vector2(-1 * Math.sin(rotation), Math.cos(rotation))]

        this._width = width
        this._height = height
        this._rotation = rotation
      }
      getProjectionRadius(axis) {
        return this.extents[0] * Math.abs(axis.dot(this.axes[0])) + this.extents[1] * Math.abs(axis.dot(this.axes[1]))
      }
    }

    class Vector2 {
      constructor(x, y) {
        this.x = x || 0
        this.y = y || 0
      }
      sub(v) {
        return new Vector2(this.x - v.x, this.y - v.y)
      }
      dot(v) {
        return this.x * v.x + this.y * v.y
      }
    }

    const detectorOBBvsOBB = (OBB1, OBB2) => {
      var nv = OBB1.centerPoint.sub(OBB2.centerPoint)
      var axisA1 = OBB1.axes[0]
      if (OBB1.getProjectionRadius(axisA1) + OBB2.getProjectionRadius(axisA1) <= Math.abs(nv.dot(axisA1))) return false
      var axisA2 = OBB1.axes[1]
      if (OBB1.getProjectionRadius(axisA2) + OBB2.getProjectionRadius(axisA2) <= Math.abs(nv.dot(axisA2))) return false
      var axisB1 = OBB2.axes[0]
      if (OBB1.getProjectionRadius(axisB1) + OBB2.getProjectionRadius(axisB1) <= Math.abs(nv.dot(axisB1))) return false
      var axisB2 = OBB2.axes[1]
      if (OBB1.getProjectionRadius(axisB2) + OBB2.getProjectionRadius(axisB2) <= Math.abs(nv.dot(axisB2))) return false
      return true
    }

    // 测试使用
    function getElement() {

      const rectOneClientRect = document.querySelector('.a').getBoundingClientRect()
      const rectTwoClientRect = document.querySelector('.b').getBoundingClientRect()

      const OBB1Options = {
        x: rectOneClientRect.left + rectOneClientRect.width / 2,
        y: rectOneClientRect.top + rectOneClientRect.height / 2,
        w: 350,
        h: 150,
        r: 0
      }

      const OBB2Options = {
        x: rectTwoClientRect.left + rectTwoClientRect.width / 2,
        y: rectTwoClientRect.top + rectTwoClientRect.height / 2,
        w: 350,
        h: 150,
        r: 220
      }

      return {
        OBB1: new OBB(new Vector2(OBB1Options.x, OBB1Options.y), OBB1Options.w, OBB1Options.h, OBB1Options.r * Math.PI / 180),
        OBB2: new OBB(new Vector2(OBB2Options.x, OBB2Options.y), OBB2Options.w, OBB2Options.h, OBB2Options.r * Math.PI / 180)
      }
    }

    // ========================================================
    // ========================================================
    // ========================================================

    // 测试使用
    aBtn = document.querySelector('.aBtn')
    bBtn = document.querySelector('.bBtn')

    const { OBB1, OBB2 } = getElement()
    document.querySelector('.text').innerHTML = detectorOBBvsOBB(OBB1, OBB2)

    aBtn.addEventListener('click', function () {
      document.querySelector('.a').style.top = 340 + 'px'
      const { OBB1, OBB2 } = getElement()
      document.querySelector('.text').innerHTML = detectorOBBvsOBB(OBB1, OBB2)
    }, false)

    bBtn.addEventListener('click', function () {
      document.querySelector('.a').style.top = 355 + 'px'
      const { OBB1, OBB2 } = getElement()
      document.querySelector('.text').innerHTML = detectorOBBvsOBB(OBB1, OBB2)
    }, false)

  </script>

</body>
</html>
```

### Louis
