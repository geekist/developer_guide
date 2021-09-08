
# java WebSocket开发

Oracle 发布的 java 的 WebSocket 的规范是 JSR356规范 ,Tomcat从7.0.27开始支持WebSocket，从7.0.47开始支持JSR-356。

websocket简单实现分为以下几个步骤：

添加websocket库

编写后台代码

编写前端代码。

## 一、添加websocket库

在maven中添加websocket库的代码如下所示：

```java
<dependency>
   <groupId>javax.websocket</groupId>
   <artifactId>javax.websocket-api</artifactId>
   <version>1.1</version>
   <scope>provided</scope>
</dependency>

```

## 二、WebSocket注解

后台实现websocket有两种方式：使用继承类、使用注解；注解方式比较方便，以下代码中使用注解方式来进行演示。

* 1、声明websocket地址

**@ServerEndpoint(value="/websocket/{paraName}") ;** 

类似Spring MVC中的@controller注解，websocket使用@ServerEndpoint来进行声明接口：

其中 “ { } ”用来表示带参数的连接，如果需要获取{}中的参数在参数列表中增加：@PathParam("paraName") Integer userId 。

则连接地址形如：ws://localhost:8080/project-name/websocket/8，

其中每个连接可以设置不同的paraName的值。

* 2、注解、成员数据介绍：

**1.@OnOpen
public void onOpen(Session session) throws IOException{ }** 

有连接时的触发函数。 我们可以在用户连接时记录用户的连接带的参数，只需在参数列表中增加参数：@PathParam("paraName") String paraName。

**2.@OnClose
public void onClose(){ }** 

连接关闭时的调用方法。

**3.@OnMessage
public void onMessage(String message, Session session) { }** 

收到消息时调用的函数，其中Session是每个websocket特有的数据成员，详情见4.

**4.Session** 

每个Session代表了两个web socket端点的会话；当websocket握手成功后，websocket就会提供一个打开的Session，可以通过这个Session来对另一个端点发送数据；如果Session关闭后发送数据将会报错。

**5.Session.getBasicRemote().sendText("message")** 

向该Session连接的用户发送字符串数据。

**6.@OnError
public void onError(Session session, Throwable error) { }** 

发生意外错误时调用的函数。


## 三、后台Java代码

```java

import java.io.IOException;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@ServerEndpoint(value="/websocketTest/{userId}")
public class Test {
    private Logger logger = LoggerFactory.getLogger(Test.class);
    
    private static String userId;
    
    //连接时执行
    @OnOpen
    public void onOpen(@PathParam("userId") String userId,Session session) throws IOException{
        this.userId = userId;
        logger.debug("新连接：{}",userId);
    }
    
    //关闭时执行
    @OnClose
    public void onClose(){
        logger.debug("连接：{} 关闭",this.userId);
    }
    
    //收到消息时执行
    @OnMessage
    public void onMessage(String message, Session session) throws IOException {
        logger.debug("收到用户{}的消息{}",this.userId,message);
        session.getBasicRemote().sendText("收到 "+this.userId+" 的消息 "); //回复用户
    }
    
    //连接错误时执行
    @OnError
    public void onError(Session session, Throwable error){
        logger.debug("用户id为：{}的连接发送错误",this.userId);
        error.printStackTrace();
    }

}

```

## 四、前端代码

```java

<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        websocket Demo---- user000 <br />
        <input id="text" type="text" /> 
        <button onclick="send()"> Send </button>   
        <button   onclick="closeWebSocket()"> Close </button>
        <div id="message">   </div>
        
    <script type="text/javascript">
     //判断当前浏览器是否支持WebSocket
      if('WebSocket' in window){
          websocket = new WebSocket("ws://localhost:8080/Demo/websocketTest/user000");
          console.log("link success")
      }else{
          alert('Not support websocket')
      }
      
      //连接发生错误的回调方法
      websocket.onerror = function(){
          setMessageInnerHTML("error");
      };
       
      //连接成功建立的回调方法
      websocket.onopen = function(event){
          setMessageInnerHTML("open");
      }
       console.log("-----")
      //接收到消息的回调方法
      websocket.onmessage = function(event){
            setMessageInnerHTML(event.data);
      }
       
      //连接关闭的回调方法
      websocket.onclose = function(){
          setMessageInnerHTML("close");
      }
       
      //监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
      window.onbeforeunload = function(){
          websocket.close();
      }
       
      //将消息显示在网页上
      function setMessageInnerHTML(innerHTML){
          document.getElementById('message').innerHTML += innerHTML + '<br/>';
      }
       
      //关闭连接
      function closeWebSocket(){
          websocket.close();
      }
       
      //发送消息
      function send(){
          var message = document.getElementById('text').value;
          websocket.send(message);
      }
    </script>
        
    </body>
</html>


```

