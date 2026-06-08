# Real-Time Chat Application

## Summary
Build a scalable real-time chat app using MERN + Socket.IO + Redis + BullMQ + Docker.

## Goal
The goal of this project is to build a production-like real-time messaging system that demonstrates how modern chat applications (like WhatsApp or Slack) work under the hood.

## Why This Project
This project is designed to understand and implement:

- Real-time communication using WebSockets (Socket.IO)
- Scalable backend architecture
- Message queuing using BullMQ
- Caching and pub/sub using Redis
- Rate limiting for API protection
- Containerization using Docker

## Core Features

### Messaging
- One-to-one messaging
- Message delivery status (sent, delivered, seen)
- Typing indicator
- Message timestamps
- Persistent chat history

### User Experience
- Online/offline status
- Last seen status
- Chat list with recent conversations
- User search functionality

### Data Handling
- Message persistence in MongoDB
- Chat history retrieval on reconnect
- Pagination for messages

### Security
- JWT authentication
- Protected Socket.IO connections
- API rate limiting
- Input validation & sanitization

### Scalability
- Redis Pub/Sub for horizontal scaling
- Stateless backend architecture
- Background job processing using BullMQ

### Background Jobs
- Notification system using BullMQ
- Retry mechanism for failed jobs


## System Overview

This system will simulate a simplified version of real-world chat systems:

Client (React)
   ↓
Backend API (Node + Express)
   ↓
Socket Server (Socket.IO)
   ↓
Redis Pub/Sub (scaling real-time messages)
   ↓
MongoDB (data storage)
   ↓
BullMQ (background jobs like notifications)

## Tech Stack

Frontend:
- React.js

Backend:
- Node.js
- Express.js

Database:
- MongoDB

Real-time:
- Socket.IO

Caching / PubSub:
- Redis

Queue System:
- BullMQ

DevOps:
- Docker


## Learning Outcome

After completing this project, I will understand:

- How real-time messaging systems work
- How scaling WebSockets works using Redis
- How background jobs are handled in production systems
- How to design system architecture like production applications