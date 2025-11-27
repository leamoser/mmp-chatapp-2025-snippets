# Websockets 2025 Snippets

![Static Badge](https://img.shields.io/badge/Angepasst_am-27.11.2025-ffca3a)
![Static Badge](https://img.shields.io/badge/Fach-Major_Media_Applications-489fb5)

> Bausteine zum kopieren für das Modul "Realtime Datenbanken & Websockets"

### 1
```json
{
  "name": "socketio-chat",
  "version": "0.0.1",
  "description": "mmp major chat app",
  "type": "module",
  "dependencies": {}
}
```

### 2
```js
import express from 'express';
import { createServer } from 'node:http';

const app = express();
const server = createServer(app);

app.get('/', (req, res) => {
  res.send('<h1>Hello world</h1>');
});

server.listen(3000, () => {
  console.log('💻 server running at http://localhost:3000');
});
```

### 3
```js
res.sendFile(new URL('./index.html', import.meta.url).pathname);
```

### 4
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>MMP Major Chat App</title>
    <link rel="stylesheet" href="style.css" type="text/css">
</head>
<body>
<header>Eingeloggt als: <span id="name"></span></header>
<ul id="messages"></ul>
<form id="form" action="">
    <input aria-label="chat message" id="input" autocomplete="off" placeholder="Your message" />
    <button>Send</button>
</form>
<script src="client.js" type="module"></script>
</body>
</html>
```

### 5
```css
* {
    margin: 0 0;
    padding: 0 0;
    box-sizing: border-box;
}

body {
    margin: 0;
    padding-bottom: 3rem;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

:root {
    --co-bg: #ffffff;
    --co-grey-light: #eaeaea;
    --co-grey: #cccccc;
    --co-dark: #333333;
    --pa-around: 24px;
    --pa-small: 8px;
    --he-controls: 32px;
}

header {
    background: cornflowerblue;
    padding: 12px 24px;
    position: sticky;
    top: 0;
    color: var(--co-bg);
    span {
        font-weight: bold;
    }
}

#form {
    background: var(--co-grey);
    padding: var(--pa-small);
    position: fixed;
    inset: auto 0 0 0;
    display: flex;
    gap: var(--pa-small);
    backdrop-filter: blur(10px);
    #input {
        border: none;
        padding: 0 1rem;
        flex-grow: 1;
        border-radius: 2rem;
        height: var(--he-controls);
        transition: background 0.2s ease;
        &:focus {
            outline: none;
            background: var(--co-grey-light);
        }
    }
    button {
        background: #333;
        border: none;
        padding: 0 1rem;
        border-radius: 3px;
        outline: none;
        color: #fff;
        height: var(--he-controls);
    }
}

#messages {
    list-style-type: none;
    li {
        padding: 8px var(--pa-around);
        display: grid;
        grid-template-columns: 75px minmax(auto, 1fr);
        gap: 10px;
        &:nth-child(odd) {
            background: var(--co-grey-light);
        }
        span.name {
            font-style: italic;
            font-size: 50%;
            display: inline-flex;
            align-items: center;
        }
    }
}
```

### 6

```js
io.on('connection', (socket) => {
  console.log('🟢 a user connected');
	socket.on('disconnect', () => {
    console.log('🔴 user disconnected');
  });
});
```

### 7
```html
<script src="/socket.io/socket.io.js"></script>
```

### 8
```js
const form = document.querySelector('#form');
const input = document.querySelector('#input');

form.addEventListener('submit', (e) => {
    e.preventDefault();
    if (input.value) {
      console.log(input.value);
      input.value = '';
    }
});
```

### 9
```js
const messages = document.getElementById('messages');
socket.on('broadcast_chat', (msg) => {
  const item = document.createElement('li');
  item.textContent = msg;
  messages.appendChild(item);
  window.scrollTo(0, document.body.scrollHeight);
})
```

### 10
```js
const io = new Server(server, {
  connectionStateRecovery: {}
});
```

### 11
```js
db.exec(`
  CREATE TABLE IF NOT EXISTS messages (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      msg TEXT,
      username TEXT
  );
`);
```

### 12
```js
let result;
try {
    const stmt = db.prepare('INSERT INTO messages (msg, username) VALUES (?, ?)');
    result = stmt.run(msg, username);
} catch (e) {
    console.error('🧑🏽‍💻 error on inserting message into database', e);
    return;
}
io.emit('broadcast_chat', msg, username, result.lastInsertRowid);
```

### 13
```js
const socket = io({
    auth: {
        serverOffset: 0
    }
});
```

### 14
```js
    if (!socket.recovered) {
        try {
            const stmt = db.prepare('SELECT id, msg, username FROM messages WHERE id > ?');
            const rows = stmt.all(socket.handshake.auth.serverOffset || 0);
            rows.forEach((row) => {
                socket.emit('broadcast_chat', row.msg, row.username, row.id);
            });
        } catch (e) {
            console.error('🧑🏽‍💻 error on reading old messages from database', e);
        }
    }
```
