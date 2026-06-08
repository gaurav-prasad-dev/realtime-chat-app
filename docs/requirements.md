## Scope

We are building a real-time chat application similar to WhatsApp (simplified version), focusing on scalability, real-time messaging, and backend architecture.

## MVP Features

### Authentication
- User registration
- User login (JWT)
- Protected routes

### Chat System
- One-to-one messaging
- Real-time message sending (Socket.IO)
- Receive messages instantly

### Data Storage
- Store users in MongoDB
- Store messages in MongoDB
- Fetch chat history

### Basic UI Support (optional frontend)
- Simple chat UI
- User list
- Chat window

### Core Backend
- REST APIs for auth & users
- Socket server for real-time communication

## Phase 2 (Scalability & Production Features)

### Redis
- Redis Pub/Sub for scaling Socket.IO across multiple servers
- Cache recent messages

### Rate Limiting
- Prevent spam requests using rate limiter

### Background Jobs
- BullMQ for background tasks (notifications, logging)

### Performance
- Pagination for chat messages
- Optimize DB queries

## Phase 3 (Advanced Features)

- Typing indicator
- Online/offline status
- Last seen
- Message delivery status (sent/delivered/read)
- File/image sharing
- Group chat system

## Out of Scope (For Now)

- Video/voice calling
- AI chatbot
- Push notifications (mobile)
- End-to-end encryption


## System Behavior

1. User logs in → receives JWT
2. User connects to Socket.IO server
3. User joins personal room (userId)
4. When message is sent:
   - Save message in MongoDB
   - Emit via Socket.IO
   - If multiple servers → Redis Pub/Sub handles sync
5. Receiver gets message instantly