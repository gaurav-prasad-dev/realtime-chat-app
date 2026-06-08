# Database Design - Real-Time Chat Application

## 1. Overview

We are using MongoDB because chat systems need:
- Fast writes (messages)
- Flexible structure
- Easy horizontal scaling

We design 3 main collections:
- Users
- Conversations (Chats)
- Messages

---

# 2. User Schema (User Account)

User
---
_id: ObjectId
username: string
email: string
password: string (hashed)
avatar: string
status: online | offline
lastSeen: Date
createdAt: Date
updatedAt: Date

---

# 3. Conversation Schema (Chat Room)

Conversation
---
_id: ObjectId
participants: [userId1, userId2]
lastMessage: string
lastMessageTime: Date
createdAt: Date
updatedAt: Date

---

# 4. Message Schema

Message
---
_id: ObjectId
conversationId: ObjectId
senderId: ObjectId
receiverId: ObjectId
message: string
messageType: text | image | file
status: sent | delivered | seen
createdAt: Date

---

# 5. Relationships

User (1) ──── (N) Conversations  
Conversation (1) ──── (N) Messages  

---

# 6. Why This Design?

- Users store authentication and profile data
- Conversations represent chat between two users
- Messages store actual chat data

This structure is used in real-world chat apps like WhatsApp (simplified version)

---

# 7. Message Flow in Database

1. User sends message  
2. Check if conversation exists  
3. If not → create conversation  
4. Save message in Message collection  
5. Update conversation lastMessage and lastMessageTime  

---

# 8. Indexing (Performance)

db.messages.createIndex({ conversationId: 1, createdAt: -1 })

db.conversations.createIndex({ participants: 1 })

---

# 9. Summary

This schema ensures:
- Fast message retrieval
- Scalable chat system
- Clean separation of concerns