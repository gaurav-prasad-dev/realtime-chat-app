# API Design - Real-Time Chat Application

## 1. Overview

This document defines all REST APIs and Socket events used in the system.

We divide APIs into:
- Auth APIs
- User APIs
- Conversation APIs
- Message APIs
- Socket Events

---

# 2. Authentication APIs

## Register User

POST /api/auth/register

### Request
{
  "username": "john",
  "email": "john@gmail.com",
  "password": "123456"
}

### Response
{
  "success": true,
  "message": "User created successfully",
  "token": "JWT_TOKEN"
}

---

## Login User

POST /api/auth/login

### Request
{
  "email": "john@gmail.com",
  "password": "123456"
}

### Response
{
  "success": true,
  "token": "JWT_TOKEN",
  "user": {
    "id": "123",
    "username": "john"
  }
}

---

# 3. User APIs

## Get All Users

GET /api/users

### Headers
Authorization: Bearer TOKEN

### Response
[
  {
    "id": "1",
    "username": "john",
    "status": "online"
  }
]

---

# 4. Conversation APIs

## Create or Get Conversation

POST /api/conversations

### Request
{
  "receiverId": "userId2"
}

### Response
{
  "conversationId": "abc123"
}

---

## Get User Conversations

GET /api/conversations

### Response
[
  {
    "conversationId": "abc123",
    "lastMessage": "Hello",
    "lastMessageTime": "2026-01-01"
  }
]

---

# 5. Message APIs

## Get Messages

GET /api/messages/:conversationId

### Response
[
  {
    "senderId": "user1",
    "message": "Hello",
    "createdAt": "time"
  }
]

---

## Send Message (REST fallback)

POST /api/messages

### Request
{
  "conversationId": "abc123",
  "receiverId": "user2",
  "message": "Hello"
}

---

# 6. Socket.IO Events (REAL-TIME CORE)

## Connection Event

socket.connect()

User joins room:
- room = userId

---

## Send Message

EVENT: send_message

### Payload
{
  "conversationId": "abc123",
  "senderId": "user1",
  "receiverId": "user2",
  "message": "Hello"
}

---

## Receive Message

EVENT: receive_message

### Payload
{
  "conversationId": "abc123",
  "message": "Hello",
  "senderId": "user1"
}

---

## Typing Indicator

EVENT: typing

{
  "conversationId": "abc123",
  "from": "user1",
  "to": "user2",
  "isTyping": true
}

---

## User Online

EVENT: user_online

{
  "userId": "user1"
}

---

## User Offline

EVENT: user_offline

{
  "userId": "user1"
}

---

# 7. System Flow (IMPORTANT)

## Sending Message Flow

1. User sends message via Socket
2. Server receives event
3. Save message in MongoDB
4. Emit to receiver room
5. If receiver offline → store + queue job

---

# 8. Security Rules

- All APIs require JWT authentication
- Socket connection must verify JWT
- Rate limiting applied on message APIs
- Input validation required on all requests

---

# 9. Summary

This API design ensures:
- REST APIs for normal operations
- Socket.IO for real-time communication
- Scalable backend architecture