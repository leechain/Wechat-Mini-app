# Wechat-Mini-app
微信小程序的学习与开发

这是一个微信小程序的小案例--贪吃蛇

功能：
它与传统的贪吃蛇不大一样，在屏幕上随机分配了20个食物，蛇头和食物碰撞的时候就吃到了食物，
蛇身长度不受限制，也不会因为碰到屏幕四周或者碰到自身而终结游戏，它只会不断地一直吃食物。

当然，食物也不会因为蛇吃了就变少了，当一个食物被吃掉地时候，屏幕的其他地方又会随机多出来一个食物，
屏幕上始终保持有20个食物供蛇来吃。

所以，准确来说，它是一条永远不会吃饱地“吃货蛇”。

遇到的问题：
在微信开发者工具中开发完之后，编译运行成功后，能用鼠标控制蛇头的上下左右移动，没有bug。
但是当用手机端扫码的时候却出现了错误：
requestAnimationFrame is not defined

查阅了微信小程序的API文档以及网上的相关资料才知道其中的原因：
微信小程序不支持requestAnimationFrame，所以部分性能的优化做不了，原因未知。

然后就把requestAnimationFrame相关代码：

     wx.drawCanvas({
        canvasId: "snakCanvas",
        actions: context.getActions()
      });
      
      requestAnimationFrame(animate);
    
    
替换成了如下代码：

    wx.drawCanvas({
        canvasId: "snakCanvas",
        actions: context.getActions()
      });
      ani(animate, 500);
    
    function ani(callback) {
      setTimeout(callback, 1000 / 60);
    }
    
    
通过构造一个函数ani来实现requestAnimationFrame的功能，再调用自己构造的函数，解决了这个bug。
