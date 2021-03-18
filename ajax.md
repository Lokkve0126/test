##### jquery-ajax封装使用
>html部分
```
<div class="outer">
        <span class="qx">点击你要选择的球星</span>
        <img src="./img/logo.jpeg" alt="">
        <ul class="tab">
            <li name="james">詹姆斯</li>
            <li name="hadeng">哈登</li>
            <li name="kuli">库里</li>
            <li name="ouwen">欧文</li>
            <li name="dulante">杜兰特</li>
        </ul>
    </div>
```
>js部分
```
<script>
    $(".outer>.tab>li").on("click", function () {
        //获取点击按钮的name
        var name = $(this).attr("name");
        //获取img对象
        var img = $("img");
        //获取顶部span对象
        var span = $(".qx");
        
        var obj = {
            name : $(this).attr("name"),
            age: "18",
            height: "172",
            fit: "62kg"
        };
                $.ajax({
                    url: "index.php",
                    type: "GET",
                    dataType: "json",
                    data: obj,
                    contentType: "application/json;charset=UTF-8",
                    timeout: 20000,
                    success : function(r,s,x) {
                        // console.log(r);
                        // for(let i in r){
                        //     console.log(r[i]);
                        // }
                        // var str = JSON.parse(x.responseText);
                        var obj = r[name];
                        span.text(obj.qx);
                        img.attr("src",obj.img);
                    },
                    error : function(x) {
                        console.log("错误码" + x.status +"\n请检查数据");
                    }
                });
            });
</script>
```
>json部分
```
{
    "james":{
        "qx": "勒布朗詹姆斯",
        "img":"img/james.jpeg"
    },
    "hadeng":{
        "qx": "詹姆斯哈登",
        "img":"img/hadeng.jpeg"
    },
    "kuli":{
        "qx": "史蒂芬库里",
        "img":"img/kuli.jpeg"
    },
    "ouwen":{
        "qx": "凯里欧文",
        "img":"img/ouwen.jpeg"
    },
    "dulante":{
        "qx": "凯文杜兰特",
        "img":"img/dulante.jpeg"
    }
}
```
>php后端部分
```
<?php
// $array = array();
// $array['name'] = $_GET['name'];
// $array['age'] = $_GET['age'];
// $array['height'] = $_GET['height'];
// $array['fit'] = $_GET['fit'];
// $array = array($_GET["name"]=>"勒布朗詹姆斯");
// echo json_encode($array);

echo file_get_contents("nba.json");

?>
```
>注意事项:指定了数据类型为json 提交时可以不调用转换JSON的方法
后端接收到数据后处理 将处理的数据存到数组里转换成JSON传到前台接收
前台可不解析直接调用对象