# FileReader
html之input tye=file标签 --- 图片的上传、预览 


#### 1.就是通过file标签和js的FileReader接口，把选择的图片文件调用readAsDataURL方法，把图片数据转成base64字符串形式显示在页面上。
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    
    </head>
    <body>
        <div style="border:2px dashed red;">
            <p>
                图片上传前预览：<input type="file" id="xdaTanFileImg" onchange="xmTanUploadImg(this)" accept="image/*"/>
                <input type="button" value="隐藏图片" onclick="document.getElementById('xmTanImg').style.display = 'none';"/>
                <input type="button" value="显示图片" onclick="document.getElementById('xmTanImg').style.display = 'block';"/>
            </p>
            <img id="xmTanImg" width="50%"/>
            <div id="xmTanDiv"></div>
        </div>
        <hr />
        <script type="text/javascript">            
            //判断浏览器是否支持FileReader接口
            if (typeof FileReader == 'undefined') {
                document.getElementById("xmTanDiv").InnerHTML = "<h1>当前浏览器不支持FileReader接口</h1>";
                //使选择控件不可操作
                document.getElementById("xdaTanFileImg").setAttribute("disabled", "disabled");
            }

            //选择图片，马上预览
            function xmTanUploadImg(obj) {
                var file = obj.files[0];
                
                console.log(obj);
                console.log(file);
                console.log("file.size = " + file.size);  //file.size 单位为byte

                var reader = new FileReader();
                
                 reader.readAsDataURL(file)
                //读取文件过程方法
                reader.onloadstart = function (e) {
                    console.log("开始读取....");
                }
                reader.onprogress = function (e) {
                    console.log("正在读取中....");
                }
                reader.onabort = function (e) {
                    console.log("中断读取....");
                }
                reader.onerror = function (e) {
                    console.log("读取异常....");
                }
                reader.onload = function (e) {
                    console.log("成功读取....");

                    var img = document.getElementById("xmTanImg");
                    img.src = e.target.result;
                    //或者 img.src = this.result;  //e.target == this
                }

               
            }
        </script>
    
    </body>
</html>
```
#### 2.另外 FileReader除了有函数readAsDataURL，另外还有另外两个函数readAsBinaryString 和 readAsText，分别可以将选择的文件读取成二进制和文本格式 
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
       
    </head>
    <body>
    <script type="text/javascript">
            //判断浏览器是否支持FileReader接口
            if (typeof FileReader == 'undefined') {
                document.getElementById("xmTanContentDiv").InnerHTML = "<p>当前浏览器不支持FileReader接口！</p>";
                document.getElementById("xmTanFile").setAttribute("disabled", "disabled");
            }

            //选择文件
            function xmTanUploadFile(obj){
                if (obj.files.length < 1) return;

                var file = obj.files[0];
                
                if (file.size > 1024 * 1024) {
                    alert("文件大于1M， 太大了，小点吧！");
                    obj.value = "";
                    return;
                }
            }

            //读取文件为二进制
            function readAsBinaryString() {
                var obj = document.getElementById("xmTanFile");
                if (obj.files.length < 1) return;

                var file = obj.files[0];
                var reader = new FileReader();

                //将文件以二进制形式读入页面
                reader.readAsBinaryString(file);
                reader.onload = function (f) {
                    document.getElementById("xmTanContentDiv").innerHTML = this.result;
                }
            }

            //读取文件为文本
            function readAsText() {
                var obj = document.getElementById("xmTanFile");
                if (obj.files.length < 1) return;

                var file = obj.files[0];
                var reader = new FileReader();

                //将文件以文本形式读入页面
                reader.readAsText(file);
                reader.onload = function (f) {
                    document.getElementById("xmTanContentDiv").innerHTML = this.result;
                }
            }
        </script>
        <div style="border: 2px dashed red; padding: 20px 0px;">
            <label>选择文件：</label>
            <input type="file" id="xmTanFile" accept=".html,.js,.css,.txt,.cs,.xml" onchange="xmTanUploadFile(this)"/>
            <input type="button" value="读取成二进制数据" onclick="readAsBinaryString()" />
            <input type="button" value="读取成文本数据" onclick="readAsText()" />
            <input type="button" value="隐藏读取内容" onclick="document.getElementById('xmTanContentDiv').style.display = 'none';"/>
            <input type="button" value="显示读取内容" onclick="document.getElementById('xmTanContentDiv').style.display = 'block';"/>
            <div id="xmTanContentDiv"></div>
        </div>
    
    </body>
</html>

```
#### 3.----------- a标签之download属性 -------------

　　 设置a标签href为图片链接，再设置download属性，点此链接可以直接下载图片
 ```
 <div style="text-align:center; padding: 5px 20px;width: 70%;">
            <img id="xmTanShowImg" src=""/>
            <h1><a href="javascript:void()" download="girl.jpg" id="xmTanDownload">点此下载</a></h1>
        </div>
        <script type="text/javascript">
            //图片转成base64位字符串数据
            var imgData = "data:image/png;base64,.........";
            //或直接设置图片链接： var imgData = "images/picture.png";

            document.getElementById("xmTanShowImg").setAttribute("src", imgData);  //给图片标签设置src
            document.getElementById("xmTanDownload").setAttribute("href", imgData); //给a标签设置href
        </script>
  ```
