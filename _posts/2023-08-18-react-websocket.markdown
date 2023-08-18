---
layout: post
title: 使用React 18和WebSocket构建实时通信功能
date: 2023-08-18 09:59:24.000000000 +09:00
tags:  React WebSocket 
---


**关键词** `React`   `WebSocket`

# 1. 引言

`WebSocket`是一种在`Web`应用中实现双向通信的协议。它允许服务器主动向客户端推送数据，而不需要客户端发起请求。在现代的实时应用中，`WebSocket`经常用于实时数据传输、聊天功能、实时通知和多人协作等场景。在本篇博客中，我们将探索如何在`React 18`应用中使用`WebSocket`来实现实时通信。

# 2. 准备工作
在开始之前，我们需要安装`React 18`，并确定你已经掌握了`React Hooks`的基本知识。此外，我们还将使用`WebSocket`的npm包来实现`WebSocket`连接。你可以通过以下命令使用`npm`或`yarn`来安装它：

```bash
npm install websocket
# 或
yarn add websocket
```
# 3. 编写自定义钩子

```javascript
import { useEffect, useRef, useState } from 'react';
import WebSocketClient from 'websocket';

export function useWebSocket(accessToken) {
  const clientRef = useRef(null);
  const [isActive, setIsActive] = useState(false);
  const [socketClient, setSockClient] = useState(null);

  useEffect(() => {
    let port = window.location.port;
    let wsUrl = '';

    if (window.location.protocol === 'https:') {
      if (!port) {
        port = '4174';
      }
      wsUrl = 'wss:';
    } else {
      if (!port) {
        port = '8080';
      }
      wsUrl = 'ws:';
    }

    wsUrl +=
      `//${window.location.hostname}:${port}/api/ws/plugins/telemetry?token=` +
      accessToken;

    if (!socketClient) {
      setSockClient(new WebSocketClient(wsUrl, isActive));
    }
  }, [accessToken, isActive, socketClient]);

  useEffect(() => {
    clientRef.current = socketClient;
    if (socketClient?.socket) {
      socketClient.start();
    }

    return () => {
      socketClient?.close();
    };
  }, [socketClient]);

  const connect = () => {
    const client = clientRef.current;
    if (client) {
      client.connect();
    }
  };

  const close = () => {
    const client = clientRef.current;
    if (client) {
      client.close();
    }
  };

  const subscribe = (handler) => {
    const client = clientRef.current;
    setIsActive(true);
    if (client) {
      client.subscribe(handler);
    }
  };

  const unsubscribe = () => {
    const client = clientRef.current;
    if (client && isActive) {
      setIsActive(false);
      client.unsubscribe();
    }
  };

  const send = (message) => {
    const client = clientRef.current;
    if (client) {
      client.send(message);
    }
  };

  return { connect, close, subscribe, unsubscribe, send };
}
```

在上述代码中，我们使用`useRef`来保存`WebSocketClient`实例，使用useState来管理`isActive`和`socketClient`状态。通过创建`WebSocket`连接的URL和`accessToken`，我们可以在useEffect钩子中实例化`WebSocketClient`。然后使用`useEffect`钩子来启动和关闭WebSocket连接，并在组件卸载时关闭连接。
# 4. 创建WebSocketProvider
为了在整个应用中共享`WebSocket`连接对象，我们需要创建一个`WebSocketProvider`组件。这个组件将使用提供者模式将连接对象提供给子组件。

在你的项目中创建一个名为`WebSocketProvider`.js的文件，并添加以下代码：

```javascript
import React, { useContext, useEffect } from 'react';
import { useWebSocket } from './useWebSocket';

const WebSocketContext = React.createContext();

export const useWebSocketContext = () => {
  return useContext(WebSocketContext);
};

export const WebSocketProvider = ({ children, accessToken }) => {
  const webSocket = useWebSocket(accessToken);

  useEffect(() => {
    webSocket.connect();

    return () => {
      webSocket.close();
    };
  }, [webSocket]);

  return (
    <WebSocketContext.Provider value={webSocket}>
      {children}
    </WebSocketContext.Provider>
  );
};
```

在上述代码中，我们使用`useWebSocket`钩子来获取`WebSocket`连接对象，并在`useEffect`钩子中连接`WebSocket`，并在组件卸载时关闭连接。然后，我们将连接对象提供给子组件，通过创建一个`WebSocketContext.Provider`。

# 5. 在组件中使用共享连接
现在，我们可以在应用的任何组件中使用共享的`WebSocket`连接了。

假设我们有一个名为`ChatComponent`的组件，它需要使用`WebSocket`连接来实现实时聊天功能。在`ChatComponent.js`文件中，添加以下代码：

```javascript
import React from 'react';
import { useWebSocketContext } from './WebSocketProvider';

function ChatComponent() {
  const webSocket = useWebSocketContext();

  const sendMessage = () => {
    if (webSocket) {
      webSocket.send('Hello, WebSocket!');
    }
  };

  return (
    <div>
      <button onClick={sendMessage}>Send Message</button>
    </div>
  );
}

export default ChatComponent;
```
在上述代码中，我们使用`useWebSocketContext`来获取`WebSocket`连接对象。然后，我们可以在组件中调用`send`方法来发送消息。

# 6. 示例应用：实时聊天

让我们使用上述代码，创建一个实时聊天应用作为示例。在你的项目中，创建一个名为`RealTimeChatApp`的文件夹，然后在文件夹中创建以下文件：

	`RealTimeChatApp.js:` 主应用组件
	`ChatComponent.js:` 实时聊天组件
	`WebSocketProvider.js`: `WebSocket`连接提供者

在`RealTimeChatApp.js`中，添加以下代码：

```javascript
import React from 'react';
import ChatComponent from './ChatComponent';
import { WebSocketProvider } from './WebSocketProvider';

function RealTimeChatApp() {
  const accessToken = 'your_access_token'; // 替换为你的访问令牌

  return (
    <WebSocketProvider accessToken={accessToken}>
      <div>
        <h1>Real Time Chat App</h1>
        <ChatComponent />
      </div>
    </WebSocketProvider>
  );
}

export default RealTimeChatApp;
```
然后，在`ChatComponent.js`中，添加以下代码：
import React from 'react';
import { useWebSocketContext } from './WebSocketProvider';

```javascript
function ChatComponent() {
  const webSocket = useWebSocketContext();
  const [messages, setMessages] = React.useState([]);
  const [newMessage, setNewMessage] = React.useState('');

  React.useEffect(() => {
    const messageHandler = (message) => {
      setMessages((prevMessages) => [...prevMessages, message]);
    };
    webSocket.subscribe(messageHandler);

    return () => {
      webSocket.unsubscribe();
    };
  }, [webSocket]);

  const sendMessage = () => {
    if (webSocket) {
      webSocket.send(newMessage);
      setNewMessage('');
    }
  };

  return (
    <div>
      <div>
        {messages.map((message, index) => (
          <div key={index}>{message}</div>
        ))}
      </div>
      <div>
        <input
          type="text"
          value={newMessage}
          onChange={(e) => setNewMessage(e.target.value)}
        />
        <button onClick={sendMessage}>Send</button>
      </div>
    </div>
  );
}

export default ChatComponent;
```

最后，启动你的应用，访问`RealTimeChatApp`组件，即可在浏览器中查看实时聊天功能。
# 7. 总结
本文介绍了如何在`React 18`应用中使用`WebSocket`来实现实时通信，并展示了如何通过自定义钩子和提供者模式来共享`WebSocket`连接对象。通过这种方式，我们可以在多个组件中使用同一个连接对象，从而避免了不必要的连接重复实例化和性能开销。`WebSocket`在现代实时应用中发挥着重要作用，帮助我们实现更高效的通信和用户体验。

希望本文能够帮助你理解如何在`React 18`中使用`WebSocket`，并在应用中实现共享连接的目标。如果你想进一步探索`WebSocket`的高级用法，可以深入了解`WebSocket`的各种选项和特性，以满足你的实际需求。

## 致谢

感谢您阅读本文，希望本文对你有所帮助。特别感谢`React 18`和`WebSocket`社区的开发者们，为我们提供了强大的工具和技术，让实时通信变得更加简单和高效。