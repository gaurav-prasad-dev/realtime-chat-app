# System Architecture - Real-Time Chat Application

## 1. High-Level Architecture

This project follows a scalable real-time architecture using MERN stack with Socket.IO, Redis, BullMQ, and MongoDB.

            ┌──────────────┐
            │   React App  │
            └──────┬───────┘
                   │ HTTP / WebSocket
                   ↓
        ┌─────────────────────┐
        │  Node.js API Server │
        │  (Express + Auth)   │
        └──────┬──────┬──────┘
               │      │
      REST APIs│      │Socket.IO
               │      ↓
               │  Real-Time Engine
               │
               ↓
    ┌────────────────────────┐
    │      MongoDB           │
    │ (Users, Messages)      │
    └────────────────────────┘

               ↓
    ┌────────────────────────┐
    │        Redis           │
    │  (Pub/Sub + Cache)     │
    └────────────────────────┘

               ↓
    ┌────────────────────────┐
    │        BullMQ         │
    │ (Background Jobs)      │
    └────────────────────────┘

---

## 2. Request Flow (Authentication & APIs)


User → React App
→ HTTP Request
→ Express Server
→ JWT Authentication Middleware
→ Controller
→ MongoDB
→ Response sent back to client


---

## 3. Real-Time Messaging Flow (Socket.IO)


User A → Socket.IO Client
→ send_message event
→ Node.js Socket Server
→ Save message in MongoDB
→ Emit event to receiver

If multiple servers exist:
→ Publish event to Redis Pub/Sub
→ Other servers receive event
→ Forward message to correct user socket

User B receives message instantly


---

## 4. Redis Pub/Sub Flow (Scaling)

Redis is used for scaling Socket.IO across multiple servers.


Server 1 → Publish message to Redis channel
→ Redis broadcasts to all subscribed servers
→ Server 2/3 receives event
→ Delivers message to correct socket


---

## 5. BullMQ Flow (Background Jobs)

BullMQ handles background processing tasks.


API Server
↓
Create Job in Queue
↓
Redis stores queue job
↓
Worker process picks job
↓
Executes task:

Send notifications
Logging
Retry failed tasks

---

## 6. Socket.IO Event System

### Connection Events
- user connects
- user joins room (userId)

### Messaging Events
- send_message
- receive_message

### Presence Events
- user_online
- user_offline

### Typing Events
- typing_start
- typing_stop

---

## 7. End-to-End Message Flow

User sends message
Socket.IO receives event
Message stored in MongoDB
Event emitted to receiver
If user offline → store + queue job
Redis syncs across servers
BullMQ handles background tasks

---

## 8. System Summary


Frontend (React)
↓
Backend (Node + Express)
↓
Socket Layer (Socket.IO)
↓
Redis Layer (Scaling + Cache)
↓
BullMQ (Background Jobs)
↓
MongoDB (Database)


---

## 9. Key Design Highlights

- Stateless backend for scalability
- Redis Pub/Sub for multi-server socket communication
- BullMQ for background processing
- MongoDB for persistent storage
- Socket.IO for real-time communication