# Gaming Community Platform - Backend

Node.js/Express backend for the Gaming Community Platform with MongoDB database and real-time chat functionality.

## 🚀 Quick Start

### Prerequisites
- Node.js (v14 or higher)
- MongoDB database
- npm or yarn

### Installation
```bash
cd server
npm install
```

### Environment Setup
Create a `.env` file in the server directory:
```env
MONGO_URI=mongodb://localhost:27017/gaming-community
JWT_SECRET=your_super_secret_jwt_key_here
PORT=5000
NODE_ENV=development
```

### Running the Server
```bash
# Development mode (with auto-restart)
npm run dev

# Production mode
npm start
```

## 📊 Database Models

### User Model
```javascript
{
  name: String (required),
  email: String (required, unique),
  password: String (required, hashed),
  gamertag: String,
  description: String,
  avatar: String,
  favoriteGames: [String],
  platforms: [String],
  isActive: Boolean,
  activeGame: String,
  activeGames: [String],
  role: String (enum: ['user', 'admin']),
  createdAt: Date
}
```

### Chat Model
```javascript
{
  participants: [ObjectId (ref: User)],
  messages: [{
    sender: ObjectId (ref: User),
    text: String,
    timestamp: Date
  }],
  updatedAt: Date
}
```

### Game Model
```javascript
{
  name: String (required),
  imageUrl: String,
  createdAt: Date
}
```

## 🔐 Authentication & Authorization

### JWT Token Structure
```javascript
{
  userId: ObjectId,
  email: String,
  role: String,
  iat: Number,
  exp: Number
}
```

### Middleware
- **authMiddleware**: Verifies JWT token and attaches user to request
- **adminMiddleware**: Ensures user has admin role

### Protected Routes
All routes except `/api/auth/*` require authentication via JWT token in Authorization header:
```
Authorization: Bearer <jwt_token>
```

## 📡 API Endpoints

### Authentication Routes (`/api/auth`)
- `POST /register` - User registration
- `POST /login` - User authentication

### User Routes (`/api/users`)
- `GET /me` - Get current user profile
- `PUT /me` - Update current user profile
- `POST /me/active` - Set user as active for games
- `POST /me/inactive` - Set user as inactive
- `GET /members` - Find active members for a game
- `GET /me/chats` - Get user's chats
- `GET /me/chats/:userId` - Get/create chat with user
- `POST /me/chats/:userId` - Send message to user
- `DELETE /me/chats/:chatId` - Delete chat
- `GET /:id` - Get user by ID

### Game Routes (`/api/games`)
- `GET /` - Get all games
- `POST /` - Add new game (admin only)

## 🔄 Real-time Features

### Socket.IO Integration
The server includes Socket.IO for real-time chat functionality:

```javascript
// Socket events
io.on('connection', (socket) => {
  console.log('User connected:', socket.id);
  
  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });
  
  socket.on('disconnect', () => {
    console.log('User disconnected:', socket.id);
  });
});
```

## 🛡️ Security Features

### Password Security
- Passwords are hashed using bcryptjs with salt rounds of 10
- Password field is excluded from queries by default

### Input Validation
- Email validation using regex pattern
- Required field validation
- Data sanitization

### Error Handling
- Centralized error handling
- Environment-based error responses (detailed in development, generic in production)

## 📚 Dependencies

### Production Dependencies
- `express`: Web framework
- `mongoose`: MongoDB ODM
- `bcryptjs`: Password hashing
- `jsonwebtoken`: JWT authentication
- `cors`: Cross-origin resource sharing
- `dotenv`: Environment variables
- `socket.io`: Real-time communication

### Development Dependencies
- `nodemon`: Auto-restart development server

## 🤝 Contributing

1. Follow the existing code style
2. Add proper error handling
3. Include input validation
4. Write clear commit messages
5. Test your changes thoroughly

---

**Backend API for Gaming Community Platform** 