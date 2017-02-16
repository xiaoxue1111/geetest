# geetest  web端

验证码插件geetest

http://www.geetest.com/     官网地址

step1：引用
```
<script src="http://code.jquery.com/jquery-1.12.3.min.js"></script>
<script src="http://static.geetest.com/static/tools/gt.js"></script>
```
注意，如果您的网站使用https，则只需要将引入极验库的地方换成https协议即可，同时在初始化的参数中添加https为true。例如SDK中的demo要使用https，则将以下代码：
```
<script src="https://code.jquery.com/jquery-1.12.3.min.js"></script>
<script src="http://static.geetest.com/static/tools/gt.js"></script>
```

step2：调用
```
<script>
   $.ajax({
          // 获取id，challenge，success（是否启用failback）
          url: "/index/startCaptchaServlet?type=pc&t=" + (new Date()).getTime(), // 加随机数防止缓存
          type: "get",
          dataType: "json",
          success: function (data) {
              // 使用initGeetest接口
              // 参数1：配置参数
              // 参数2：回调，回调的第一个参数验证码对象，之后可以使用它做appendTo之类的事件
              initGeetest({
                  gt: data.gt,
                  challenge: data.challenge,
                  product: "float", // 产品形式，包括：float，embed，popup。注意只对PC版验证码有效
                  offline: !data.success // 表示用户后台检测极验服务器是否宕机，一般不需要关注
                  // 更多配置参数请参见：http://www.geetest.com/install/sections/idx-client-sdk.html#config
              }, handlerEmbed);

          }
      });
   var handlerEmbed = function（captchaObj）{
         //captchaObj 为验证码对象
         captchaObj.appendTo(position)  //将验证码添加到某个位置
         captchaObj.onReady(function(){
             //验证码dom元素生成完毕时的回调函数
         })
         captchaObj.onSuccess(function(){      //验证码成功调用时的回调函数
              var validate = captchaObj.getValidate(); //如果验证成功返回三个参数，错误返回false;
              //  对象格式如下：
                {
                    geetest_challenge: 'xxx',
                    geetest_validate: 'xxx',
                    geetest_seccode: 'xxx'
                }
              注：有时候获取不到以上对象，可以选择动态获取这三个值
              例： var geetest_challenge = $('position'）.find('').find('')其他两个同理
            
         })
         
         
         
   }   
        
</script>
```
config的配置参数见  http://www.geetest.com/install/sections/idx-client-sdk.html#web
