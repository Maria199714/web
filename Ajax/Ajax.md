![](F:\娃哈哈\Ajax\images\请求报文格式.png)

![](F:\娃哈哈\Ajax\images\响应报文格式.png)**跨域问题的解决方案**

```javascript
//创建路由规则
app.get('/server',(request,response)=>{
//    设置响应头
    response.setHeader('Access-Controll-Allow-Origin','*');//设置允许跨域
//    设置响应
    response.send('Hello Ajax')
});
```

1. 在相应的头部设置：**('Access-Controll-Allow-Origin','*')**

**原生Ajax请求的基本操作**

```javascript
<script>
    var btn=document.querySelector('button');
    var div=document.querySelector('div');
    btn.addEventListener('click',function () {
    //    发送Ajax请求
    //    1、创建对象
        var xhr=new XMLHttpRequest();
    //    2、初始化 设置请求方法和URL
        xhr.open('GET','http://127.0.0.1:8000/server');
    //    3、发送
        xhr.send();
    //    4、事件绑定 处理服务端返回的结果
        xhr.onreadystatechange=function () {
        //    判断 （服务端返回了所有的结果）
            if (xhr.readyState===4){
            //    判断响应状态码 200 404 401 500
            //    2XX 成功
                if (xhr.status>=200&&xhr.status<300){
                //    处理结果
                    console.log(xhr.response);
                    div.innerHTML=xhr.response;
                }else{
                    console.log(xhr.status)
                }
            }
        }

    })
</script>
```

**在Ajax请求中如何来设置URL的参数？**

1. get请求可以直接在URL字符串后面进行参数的拼接：（用问号**？**进行分割）

   ```javascript
   xhr.open('GET','http://127.0.0.1:8000/server?a=100&b=200&c=300');
   ```

2. post请求可以在send（）方法中采样URL写参数的形式进行参数的设置：

   ```javascript
   xhr.send('a=100&c=300');
   ```

   注意：括号里面参数的形式需要根据服务端需要获取的参数的形式来进行协商而定。

3. 

**Ajax发送POST请求**

```javascript
<script>
    var box=document.querySelector('div');
    box.onmousemove=function () {
    //    通过Ajax发送POST请求
    //    1、创建对象
        var xhr=new XMLHttpRequest();
    //    2、初始化  设置请求方式和URL
        xhr.open('POST','http://127.0.0.1:8000/server');
    //    3、发送  
        xhr.send();
    //    4、对服务端返回的数据进行处理
        xhr.onreadystatechange=function () {
            if (xhr.readyState===4){
                if (xhr.status>=200&&xhr.status<300){
                    box.innerHTML=xhr.response;
                }else{

                }
            }
        }j
    }
</script>
```

**同源策略**

**跨域**

违背同源策略就是跨域。

Ajax在发送请求的时候默认是需要遵循同源策略的。

**JSONP**

**CORS**

一般为了解决跨域问题，我们需要在服务器的返回的时候，设置下面的头信息：

```javascript
//    设置响应头
    response.setHeader('Access-Control-Allow-Origin','*');//设置允许跨域
    response.setHeader('Access-Control-Allow-Headers','*');
    response.setHeader('Access-Control-Allow-Method','*');
```

