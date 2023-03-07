# FONGHOKCUNG

#### 2023/3/7
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
    浏览器会自动把data-*的属性名转为小写
