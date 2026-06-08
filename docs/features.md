## User System

### Responsibilities
- Register new users
- Login users securely
- Authenticate requests using JWT
- Maintain user identity across sockets

### Core Features
- Signup / Login
- Password hashing (bcrypt)
- JWT token generation
- Protected APIs
- Socket authentication middleware

### Database Model
- User
  - _id
  - username
  - email
  - password
  - createdAt


  ## Chat System

### Responsibilities
- Handle one-to-one messaging
- Store messages
- Retrieve chat history

### Core Features
- Send message
- Receive message in real-time
- Fetch previous messages
- Message persistence

### Database Model
- Message
  - _id
  - senderId
  - receiverId
  - message
  - timestamp
  - status (sent/delivered/read)

  ## Socket System (Socket.IO)

### Responsibilities
- Handle real-time communication
- Maintain user connections
- Emit and receive events

### Events

#### Connection
- user connects → join room (userId)

#### Messaging
- send_message
- receive_message

#### Presence
- user_online
- user_offline

#### Typing
- typing_start
- typing_stop

### Room Strategy
- Each user = one room (userId)


## Redis System (Pub/Sub + Cache)

### Responsibilities
- Scale Socket.IO across multiple servers
- Cache frequently used data
- Enable real-time message sync

### Features

#### Pub/Sub
- Publish messages from one server
- Subscribe on other servers
- Sync socket events across instances

#### Caching
- Store recent chats
- Reduce MongoDB queries


## Queue System (BullMQ)

### Responsibilities
- Handle background tasks
- Improve performance

### Jobs
- Send notifications
- Log messages
- Retry failed tasks
- Future email system

### Flow
User sends message
→ API saves message
→ Job added to queue
→ Worker processes job


## Rate Limiting System

### Responsibilities
- Prevent spam
- Protect APIs from abuse

### Features
- Limit requests per IP/user
- Block excessive messaging
- Use Redis for tracking request counts

## Notification System

### Responsibilities
- Notify users about new messages

### Features
- Real-time socket notification
- Background job notifications (BullMQ)
- Future email/push support