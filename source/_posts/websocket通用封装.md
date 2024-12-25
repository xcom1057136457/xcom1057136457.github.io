---
title: websocket通用封装
date: 2023-01-31 16:55:51
---
```` javascript
import store from '@/store';

export function WebSocketFunc(ws, wsUrl, callback, callback1) {
  var lockReconnect = false; // 避免重复连接
  function createWebSocket(url) {
    try {
      console.log('wsUrl', wsUrl);
      if ('WebSocket' in window) {
        ws = new WebSocket(url);
      } else if ('MozWebSocket' in window) {
        ws = new MozWebSocket(url);
      } else {
        ws = new SockJS(url);
      }
      callback1 && callback1(ws);
      initEventHandle();
    } catch (e) {
      reconnect(wsUrl);
    }
  }

  function initEventHandle() {
    ws.onclose = function (evnt) {
      // console.log('websocket服务关闭了');
      console.log(
        'websocket 断开: ' +
          'code:' +
          evnt.code +
          ' ' +
          'reason:' +
          evnt.reason +
          ' ' +
          'wasClean:' +
          evnt.wasClean
      );
      // reconnect(evnt.target.url);
      // 1.前端主动关闭连接 2.运营商关闭连接
      // if (evnt.url === wsUrl) {
      //   console.log('前端主动连接中断');
      // } else {
      //   // 可能是运营商关闭连接
      //   reconnect(evnt.target.url);
      //   console.log('心跳重连...');
      // }
      if (store.state.websocket.isClose) {
        console.log('前端主动连接中断');
        store.dispatch('websocket/getWsClose', false);
      } else {
        reconnect(evnt.target.url);
        console.log('心跳重连...');
      }
    };
    ws.onerror = function () {
      console.log('websocket服务出错了');
      reconnect(wsUrl);
    };
    ws.onopen = function () {
      console.log('send device sn: ', store.getters['user/curDevice'].sn);
      ws.send(store.getters['user/curDevice'].sn);
      // 心跳检测重置
      heartCheck.reset().start();
    };
    ws.onmessage = function (evnt) {
      // 如果获取到消息，心跳检测重置
      // 拿到任何消息都说明当前连接是正常的
      // console.log('websocket服务获得数据了')
      // 接受消息后的UI变化
      doWithMsg(evnt.data);
      heartCheck.reset().start();
    };

    // 收到消息推送
    function doWithMsg(msg) {
      console.log(
        "websocket's msg",
        msg.indexOf('成功建立websocket连接') == 0 ||
          !msg.indexOf('Pong') ||
          msg.indexOf('设备') != -1 ||
          msg.indexOf('连接成功') != -1
          ? msg
          : JSON.parse(msg)
      );
      callback && callback(msg);
    }
  }

  function reconnect(url) {
    console.log('正在重连...');
    if (lockReconnect) return;
    lockReconnect = true;
    // 没连接上会一直重连，设置延迟避免请求过多
    setTimeout(function () {
      createWebSocket(url);
      lockReconnect = false;
    }, 2000);
  }

  // 心跳检测
  var heartCheck = {
    timeout: 10000, // 5秒
    timeoutObj: null,
    serverTimeoutObj: null,
    reset: function () {
      clearTimeout(this.timeoutObj);
      clearTimeout(this.serverTimeoutObj);
      return this;
    },
    start: function () {
      var self = this;
      this.timeoutObj = setTimeout(function () {
        // 这里发送一个心跳，后端收到后，返回一个心跳消息
        // onmessage拿到返回的心跳就说明连接正常
        ws.send('Ping');
        console.log('Ping');
        self.serverTimeoutObj = setTimeout(function () {
          // 如果超过一定时间还没重置，说明后端主动断开了
          ws.close(); // 如果onclose会执行reconnect，我们执行ws.close()就行了.如果直接执行reconnect 会触发onclose导致重连两次
        }, self.timeout);
      }, this.timeout);
    }
  };

  // 初始化websocket
  createWebSocket(wsUrl);
}
````

