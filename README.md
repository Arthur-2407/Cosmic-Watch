# Cosmic Watch - NEO Monitoring Application

A comprehensive Near-Earth Object (NEO) monitoring application built with React, Node.js, and Docker. Track potentially hazardous asteroids, monitor their trajectories, and receive real-time alerts.

## 🚀 Features

- **Real-time NEO Tracking**: Monitor near-Earth objects using NASA's API
- **Risk Assessment**: Automated risk level calculations for each asteroid
- **Interactive Dashboard**: Beautiful, responsive UI with real-time data
- **Watchlist Management**: Track specific asteroids of interest
- **Alert System**: Get notified about high-risk objects
- **3D Visualization**: Interactive 3D models of asteroid trajectories
- **Authentication**: Secure user authentication and authorization

## 🛠️ Tech Stack

### Frontend
- **React 18** with TypeScript
- **Vite** for fast development
- **Tailwind CSS** for styling
- **Three.js** for 3D visualizations
- **Redux Toolkit** for state management
- **React Router** for navigation

### Backend
- **Node.js** with Express
- **JWT** for authentication
- **bcryptjs** for password hashing
- **CORS** for cross-origin requests
- **JSON file-based database**

### DevOps & Deployment
- **Docker** & Docker Compose
- **Nginx** as reverse proxy

## 📋 Prerequisites

- Node.js 18+
- Docker & Docker Compose
- Git

## 🚀 Quick Start

### Option 1: Local Development

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd build-cosmic-watch-app
   ```

2. **Install dependencies**
   ```bash
   # Frontend
   cd build-cosmic-watch-app
   npm install
   
   # Backend
   cd ../server
   npm install
   ```

3. **Start the servers**
   ```bash
   # Start backend (in server directory)
   npm run dev
   
   # Start frontend (in build-cosmic-watch-app directory)
   npm run dev
   ```

4. **Access the application**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:3001

### Option 2: Docker Deployment

1. **Using Docker Compose (Recommended)**
   ```bash
   docker-compose up -d
   ```

2. **Access the application**
   - Frontend: http://localhost:80
   - Backend API: http://localhost:3001

### Option 3: Manual Docker Build

1. **Build backend image**
   ```bash
   cd server
   docker build -t cosmic-watch-backend .
   docker run -p 3001:3001 cosmic-watch-backend
   ```

2. **Build frontend image**
   ```bash
   cd build-cosmic-watch-app
   docker build -t cosmic-watch-frontend .
   docker run -p 80:80 cosmic-watch-frontend
   ```

## 📚 API Documentation

### Authentication Endpoints

- `POST /api/auth/signup` - Create new user
- `POST /api/auth/login` - User login
- `GET /api/auth/profile` - Get user profile

### Watchlist Endpoints

- `GET /api/watchlist` - Get user's watchlist
- `POST /api/watchlist/:id` - Add asteroid to watchlist
- `DELETE /api/watchlist/:id` - Remove from watchlist

### Alerts Endpoints

- `GET /api/alerts` - Get user's alerts
- `POST /api/alerts` - Create new alert
- `PUT /api/alerts/:id` - Update alert

### System Endpoints

- `GET /api/health` - Health check

## 🧪 Testing with Postman

1. **Import the collection**
   - Open Postman
   - Import `postman/Cosmic_Watch_API.postman_collection.json`
   - Set environment variable `baseUrl` to `http://localhost:3001`

2. **Test the API**
   - Use the "Sign Up" endpoint to create an account
   - Use the "Login" endpoint to get authentication token
   - Test other endpoints with the token

## 🚀 Deployment

### Docker Production Deployment

1. **Set up production server**
   ```bash
   # On production server
   git clone <your-repo-url>
   cd <repository>
   docker-compose up -d
   ```

2. **Environment Variables**
   Create `.env` file:
   ```
   NODE_ENV=production
   JWT_SECRET=your-secret-key
   PORT=3001
   ```

## 🔧 Configuration

### Environment Variables

Create `.env` files in both frontend and backend directories:

**Backend (.env)**
```
NODE_ENV=development
JWT_SECRET=your-jwt-secret
PORT=3001
```

**Frontend (.env)**
```
VITE_API_URL=http://localhost:3001
VITE_NASA_API_KEY=your-nasa-api-key
```

### NASA API Key

1. Get API key from [NASA API Portal](https://api.nasa.gov/)
2. Add it to your environment variables
3. Restart the application

## 📁 Project Structure

```
├── build-cosmic-watch-app/     # Frontend React app
│   ├── src/
│   │   ├── components/        # Reusable components
│   │   ├── pages/            # Page components
│   │   ├── api/              # API calls
│   │   └── store/            # Redux store
│   ├── public/
│   └── Dockerfile
├── server/                    # Backend Node.js app
│   ├── index.js              # Main server file
│   ├── db.js                 # Database helper
│   ├── data.json             # JSON database
│   └── Dockerfile
├── postman/                   # API documentation
├── docker-compose.yml         # Docker orchestration
└── README.md
```

## 🤝 Contributing

1. Create a feature branch (`git checkout -b feature/amazing-feature`)
2. Commit your changes (`git commit -m 'Add amazing feature'`)
3. Push the branch to your chosen remote
4. Share your changes for review

## 👥 Contributors

1. akgupta2025-en
2. Arthur-2407
3. ayushk-01-ak

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- NASA for providing the NEO API
- Three.js community for 3D visualization tools
- React and TypeScript communities

## 📞 Support

For support and questions:
- Check the [Postman documentation](postman/README.md) for API usage
- Review the [deployment guide](#-deployment) for setup issues
