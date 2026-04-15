# Pertemuan 13: Real-time Features dengan WebSocket & Socket.io

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Memahami WebSocket vs HTTP polling
- Mengimplementasikan Socket.io server dan client
- Membuat room-based communication
- Mengimplementasikan event-driven architecture
- Menangani reconnection strategy
- Membuat real-time notifications
- Mengoptimalkan WebSocket untuk scalability

## 2. Pengantar: Hook

HTTP adalah request-response model. Server tidak bisa proactively send data. Untuk real-time, gunakan WebSocket.

DANA live notifications: transaction completed → notify instantly. Tidak bisa polling server every 100ms (waste resources).

Socket.io:
- Real-time communication
- Automatic reconnection
- Room support (chat dengan specific users)
- Broadcasting (send to all users)
- Binary data support

## 3. Konsep Utama

### 3.1 WebSocket vs HTTP

```
HTTP (Polling):
1. Client: "Any data?"
2. Server: "No"
3. Wait 1 second
4. Repeat 10x per second = waste!

WebSocket (Push):
1. Connection established
2. Server: "New transaction!"
3. Client receives immediately
4. Server: "Payment complete!"
5. Client receives immediately
```

### 3.2 Socket.io Architecture

```
Connection → Authentication → Join Room → Listen Events
    ↓
Event emitted on server
    ↓
Broadcast to room
    ↓
All clients in room receive
    ↓
Handle in client
```

## 4. Ilustrasi & Analogi

**Analogi: Broadcast vs WhatsApp**

- **HTTP Polling:** Calling friend every 1 second "any news?"
- **WebSocket:** Friend calls you when news happens
- **Rooms:** Group chat (broadcast to group)

## 5. Contoh Teknis

### 5.1 Socket.io Server Setup

```javascript
// npm install socket.io

// src/server.js
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');
const cors = require('cors');

const app = express();
const server = http.createServer(app);
const io = new Server(server, {
    cors: { origin: process.env.FRONTEND_URL },
    transports: ['websocket', 'polling'] // fallback to polling if needed
});

app.use(cors());
app.use(express.json());

// Middleware to authenticate socket connections
io.use((socket, next) => {
    const token = socket.handshake.auth.token;
    
    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        socket.userId = decoded.sub;
        next();
    } catch (err) {
        next(new Error('Authentication failed'));
    }
});

// Connection handler
io.on('connection', (socket) => {
    console.log(`User ${socket.userId} connected: ${socket.id}`);
    
    // User joined
    socket.on('join-wallet', (walletId) => {
        socket.join(`wallet:${walletId}`);
        console.log(`User ${socket.userId} joined wallet ${walletId}`);
    });
    
    // Real-time message
    socket.on('send-message', (data) => {
        io.to(`chat:${data.chatId}`).emit('new-message', {
            from: socket.userId,
            text: data.text,
            timestamp: new Date()
        });
    });
    
    // Balance update (server→client)
    // Called from payment processing
    
    // Disconnect
    socket.on('disconnect', () => {
        console.log(`User ${socket.userId} disconnected`);
    });
    
    // Error handling
    socket.on('error', (err) => {
        console.error(`Socket error for ${socket.userId}:`, err);
    });
});

// Emit balance update when transaction processed
app.post('/api/transactions', authenticateToken, async (req, res) => {
    const { toUserId, amount } = req.body;
    
    // Process transaction
    const tx = await Transaction.create({ toUserId, amount });
    
    // Notify sender (real-time balance update)
    io.to(`wallet:${req.user.id}`).emit('balance-updated', {
        newBalance: updatedBalance.sender,
        transaction: tx
    });
    
    // Notify recipient
    io.to(`wallet:${toUserId}`).emit('balance-updated', {
        newBalance: updatedBalance.recipient,
        transaction: tx
    });
    
    res.json({ success: true, transactionId: tx.id });
});

server.listen(3000, () => {
    console.log('Server running on port 3000');
});

module.exports = { io, server };
```

### 5.2 Socket.io Client Setup (Vue)

```javascript
// src/plugins/socket.js
import io from 'socket.io-client';

let socket = null;

export const initSocket = (token) => {
    socket = io(process.env.VUE_APP_API_URL, {
        auth: { token },
        reconnection: true,
        reconnectionDelay: 1000,
        reconnectionDelayMax: 5000,
        reconnectionAttempts: 5
    });
    
    // Connection events
    socket.on('connect', () => {
        console.log('Connected to server');
    });
    
    socket.on('disconnect', (reason) => {
        console.log('Disconnected:', reason);
        // Automatic reconnect is handled by socket.io
    });
    
    socket.on('error', (err) => {
        console.error('Socket error:', err);
    });
    
    return socket;
};

export const getSocket = () => socket;

// src/main.js
import { initSocket } from '@/plugins/socket';

const app = createApp(App);
const token = localStorage.getItem('accessToken');
if (token) {
    initSocket(token);
}

app.mount('#app');

// src/composables/useSocket.js
import { getSocket } from '@/plugins/socket';
import { ref, onMounted, onUnmounted } from 'vue';

export const useSocket = (eventName, callback) => {
    const socket = getSocket();
    
    onMounted(() => {
        if (socket) {
            socket.on(eventName, callback);
        }
    });
    
    onUnmounted(() => {
        if (socket) {
            socket.off(eventName, callback);
        }
    });
};
```

### 5.3 Real-time Notifications Component

```javascript
// src/components/TransactionNotifications.vue
<template>
    <div class="notifications">
        <transition-group name="slide">
            <div 
                v-for="notification in notifications"
                :key="notification.id"
                class="notification"
                :class="notification.type"
            >
                <p>{{ notification.message }}</p>
                <button @click="closeNotification(notification.id)">×</button>
            </div>
        </transition-group>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useSocket } from '@/composables/useSocket';

export default {
    setup() {
        const notifications = ref([]);
        
        const addNotification = (message, type = 'info', duration = 5000) => {
            const id = Date.now();
            notifications.value.push({ id, message, type });
            
            setTimeout(() => closeNotification(id), duration);
        };
        
        const closeNotification = (id) => {
            notifications.value = notifications.value.filter(n => n.id !== id);
        };
        
        // Listen to transaction events
        useSocket('balance-updated', (data) => {
            addNotification(
                `Transaction: ${data.transaction.type} Rp${data.transaction.amount}`,
                'success'
            );
        });
        
        useSocket('transaction-failed', (data) => {
            addNotification(
                `Transaction failed: ${data.error}`,
                'error',
                10000
            );
        });
        
        return { notifications, closeNotification };
    }
}
</script>

<style scoped>
.notifications { position: fixed; top: 20px; right: 20px; z-index: 1000; }

.notification {
    background: white;
    border-radius: 4px;
    padding: 15px;
    margin-bottom: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    display: flex;
    justify-content: space-between;
    align-items: center;
    min-width: 300px;
}

.notification.success { border-left: 4px solid #28a745; }
.notification.error { border-left: 4px solid #dc3545; }
.notification.warning { border-left: 4px solid #ffc107; }

.notification button {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    padding: 0;
    margin-left: 15px;
}

.slide-enter-active, .slide-leave-active { transition: all 0.3s; }
.slide-enter-from { transform: translateX(100%); }
.slide-leave-to { transform: translateX(100%); }
</style>
```

### 5.4 Chat Application Example

```javascript
// Server
io.on('connection', (socket) => {
    // Join chat room
    socket.on('join-chat', (chatId) => {
        socket.join(`chat:${chatId}`);
        io.to(`chat:${chatId}`).emit('user-joined', {
            userId: socket.userId,
            timestamp: new Date()
        });
    });
    
    // Send message to room
    socket.on('send-message', (data) => {
        io.to(`chat:${data.chatId}`).emit('new-message', {
            from: socket.userId,
            text: data.text,
            timestamp: new Date()
        });
    });
    
    // Typing indicator
    socket.on('user-typing', (data) => {
        socket.to(`chat:${data.chatId}`).emit('user-typing', {
            userId: socket.userId
        });
    });
});

// Client Component
<template>
    <div class="chat">
        <div class="messages" ref="messagesContainer">
            <div v-for="msg in messages" :key="msg.id" class="message">
                <strong>{{ msg.fromName }}:</strong> {{ msg.text }}
            </div>
        </div>
        
        <div v-if="typingUsers.length > 0" class="typing">
            {{ typingUsers.join(', ') }} typing...
        </div>
        
        <input 
            v-model="messageText"
            @keydown.enter="sendMessage"
            @input="notifyTyping"
            placeholder="Type a message..."
        />
    </div>
</template>

<script>
import { ref, onMounted, watch } from 'vue';
import { useSocket } from '@/composables/useSocket';

export default {
    props: { chatId: String },
    setup(props) {
        const messages = ref([]);
        const messageText = ref('');
        const typingUsers = ref([]);
        
        useSocket('new-message', (data) => {
            messages.value.push({
                id: Date.now(),
                from: data.from,
                text: data.text,
                timestamp: data.timestamp
            });
        });
        
        useSocket('user-typing', (data) => {
            if (!typingUsers.value.includes(data.userId)) {
                typingUsers.value.push(data.userId);
                setTimeout(() => {
                    typingUsers.value = typingUsers.value.filter(
                        u => u !== data.userId
                    );
                }, 3000);
            }
        });
        
        const socket = getSocket();
        
        const sendMessage = () => {
            socket.emit('send-message', {
                chatId: props.chatId,
                text: messageText.value
            });
            messageText.value = '';
        };
        
        const notifyTyping = () => {
            socket.emit('user-typing', { chatId: props.chatId });
        };
        
        onMounted(() => {
            socket.emit('join-chat', props.chatId);
        });
        
        return { messages, messageText, typingUsers, sendMessage };
    }
}
</script>
```

## 6. Studi Kasus Nyata: DANA Live Transaction Notifications

```javascript
// When user A transfers to user B:
// 1. Payment processing starts
// 2. Server connects to user B's socket
// 3. Emit 'balance-updated' event
// 4. User B receives immediately (not polling)
// 5. Balance updates in real-time
```

## 7. Visualisasi: Real-time Flow

```
User A initiates transfer
    ↓
Server processes payment
    ↓
    ├─ Get User B's socket
    ├─ io.to(userB).emit('balance-updated')
    └─ User B connected? → receives instantly
        Not connected? → stored on next login
    ↓
User B sees balance update instantly
```

## 8. Kesalahan Umum

### ❌ Not handling reconnection

```javascript
// ❌ WRONG - state lost on reconnect
socket.on('connect', () => { /* setup */ });

// ✅ CORRECT - rejoin rooms, resync state
socket.on('reconnect', () => {
    socket.emit('rejoin-rooms');
    loadBalance(); // fetch latest state
});
```

### ❌ Broadcasting to all users

```javascript
// ❌ WRONG - wastes bandwidth
io.emit('event', data); // sends to everyone

// ✅ CORRECT - send only to relevant users
io.to(`wallet:${userId}`).emit('event', data);
```

## 9. Latihan & Studi Kasus

### Latihan 1: Simple Chat
```javascript
// Implement basic chat with Socket.io
// - Join room
// - Send/receive messages
```

### Latihan 2: Notifications
```javascript
// Implement notification system
// - Emit on server
// - Display on client
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami WebSocket vs HTTP
- [ ] Bisa setup Socket.io server
- [ ] Bisa setup Socket.io client
- [ ] Bisa membuat rooms
- [ ] Bisa emit dan listen events
- [ ] Mengerti authentication
- [ ] Bisa handle reconnection
- [ ] Bisa broadcast selectively
- [ ] Mengerti scalability (Redis adapter)
- [ ] Bisa implement real-time features

## 11. Referensi

- [Socket.io Documentation](https://socket.io/)
- [WebSocket Protocol](https://tools.ietf.org/html/rfc6455)
- [Socket.io with Redis](https://socket.io/docs/v4/redis-adapter/)

**Status:** ✅ Pertemuan 13 selesai
