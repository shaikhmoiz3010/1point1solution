# 1Point 1Solution - Government Documentation Service Platform

A full-stack MERN application for managing government documentation services with admin panel, user dashboard, and booking system.

## 🚀 Live Demo
- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Admin Panel**: http://localhost:3000/admin

## 📋 Prerequisites

Before you begin, ensure you have installed:
- [Node.js](https://nodejs.org/) (v14 or higher)
- [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)
- [MongoDB](https://www.mongodb.com/) (Local or Atlas)
- [Git](https://git-scm.com/)

## 🛠️ Installation & Setup

### Step 1: Clone the Repository
```bash
# Clone the project
git clone <your-repository-url>
cd 1Point-1Solution
```

### Step 2: Install All Dependencies
```bash
# Install dependencies for root, client, and server
npm run install-all
```
This command will install:
- Root dependencies (concurrently)
- Frontend (React) dependencies
- Backend (Node.js/Express) dependencies

### Step 3: Configure Environment Variables

#### Backend Configuration (`server/.env`):
```env
# Create a new file at server/.env and add:
PORT=5000
NODE_ENV=development

# MongoDB Connection
# For Local MongoDB:
MONGODB_URI=mongodb://localhost:27017/1point1solution

# OR for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/1point1solution

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRE=30d

# Optional: For production
# FRONTEND_URL=http://localhost:3000
```

#### Frontend Configuration (`client/.env`):
```env
# Create a new file at client/.env and add:
# API Base URL
VITE_API_URL=http://localhost:5000/api
```

### Step 4: Set Up MongoDB

#### Option A: Local MongoDB (Recommended for Development)
1. **Install MongoDB Community Edition** from [MongoDB Website](https://www.mongodb.com/try/download/community)
2. **Start MongoDB service**:
   ```bash
   # On macOS/Linux:
   sudo systemctl start mongod
   
   # On Windows (Run as Administrator):
   net start MongoDB
   
   # Or using MongoDB Compass GUI
   ```
3. **Verify MongoDB is running**:
   ```bash
   mongosh --eval "db.version()"
   ```

#### Option B: MongoDB Atlas (Cloud)
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Create database user
4. Get connection string and update `MONGODB_URI` in `.env`

### Step 5: Seed the Database
```bash
# This will:
# 1. Connect to MongoDB
# 2. Create 25+ services (Driving License, Passport, RTO, etc.)
# 3. Create test users (admin and regular user)
npm run seed
```

**Seeded Credentials:**
- **Admin User**: `admin@1point1solution.com` / `admin123`
- **Regular User**: `user@1point1solution.com` / `user123`

### Step 6: Start the Development Server
```bash
# This will start both frontend and backend concurrently
npm run dev
```

This starts:
- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Hot Reload**: Enabled for both

## 📁 Project Structure

```
1Point-1Solution/
├── client/                 # React Frontend
│   ├── public/
│   ├── src/
│   │   ├── components/    # Reusable components
│   │   │   ├── admin/    # Admin panel components
│   │   │   ├── Navbar.jsx
│   │   │   └── Footer.jsx
│   │   ├── pages/        # Page components
│   │   ├── contexts/     # React contexts (Auth)
│   │   ├── utils/        # API utilities
│   │   └── App.jsx       # Main app component
│   └── package.json
│
├── server/                # Node.js Backend
│   ├── controllers/       # Route controllers
│   ├── models/           # MongoDB models
│   ├── routes/           # API routes
│   ├── middleware/       # Auth & validation
│   ├── index.js          # Server entry point
│   └── package.json
│
├── package.json          # Root package.json
└── README.md             # This file
```

## 🚪 Available Scripts

### Root Directory:
```bash
npm run dev               # Start both frontend & backend
npm run client            # Start only frontend
npm run server            # Start only backend
npm run install-all       # Install all dependencies
npm run seed              # Seed database with initial data
```

### Client Directory (`/client`):
```bash
cd client
npm run dev               # Start React dev server (port 3000)
npm run build             # Build for production
npm run preview           # Preview production build
npm run lint              # Run ESLint
```

### Server Directory (`/server`):
```bash
cd server
npm run dev               # Start Express server with nodemon
npm start                 # Start Express server
npm run seed              # Seed database
```

## 🔧 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user
- `PUT /api/auth/update-profile` - Update profile

### Services
- `GET /api/services` - Get all services
- `GET /api/services/categories` - Get service categories
- `GET /api/services/category/:category` - Get services by category
- `GET /api/services/id/:id` - Get service by ID

### Bookings
- `POST /api/bookings` - Create booking
- `GET /api/bookings/my-bookings` - Get user bookings
- `GET /api/bookings/:id` - Get booking details
- `PUT /api/bookings/:id/cancel` - Cancel booking

### Admin Endpoints (Requires admin role)
- `GET /api/admin/stats` - Get dashboard stats
- `GET /api/admin/bookings` - Get all bookings
- `GET /api/admin/users` - Get all users
- `PUT /api/admin/bookings/:id/status` - Update booking status
- `GET /api/admin/analytics/services` - Get service analytics

## 👥 User Roles & Access

### 1. **Regular User**
- **Access**: Home, Services, Dashboard, Bookings
- **Features**:
  - Browse services
  - Book services
  - Track bookings
  - Update profile
  - Make payments

### 2. **Admin User**
- **Access**: All regular features + Admin Panel
- **Features**:
  - Manage all bookings
  - Manage users
  - View analytics
  - Update booking status
  - Send notifications

## 📊 Database Schema

### Users Collection
```javascript
{
  fullName: String,
  email: String (unique),
  phone: String,
  password: String (hashed),
  role: ['user', 'admin'],
  address: Object,
  bookings: [ObjectId]
}
```

### Services Collection
```javascript
{
  category: String, // 'driving-licence', 'passport', etc.
  serviceId: String, // 'A', 'B', 'C'
  name: String,
  description: String,
  fee: Number,
  processingTime: String,
  isActive: Boolean
}
```

### Bookings Collection
```javascript
{
  bookingId: String (unique),
  user: ObjectId,
  service: ObjectId,
  serviceName: String,
  serviceFee: Number,
  status: ['pending', 'processing', 'completed', 'cancelled'],
  paymentStatus: ['pending', 'paid', 'failed', 'refunded'],
  tracking: Array, // Status history
  createdAt: Date
}
```

## 🔐 Authentication Flow

1. **Registration**: User registers with email, password, phone
2. **Login**: JWT token generated and stored
3. **Protected Routes**: Token verified on each request
4. **Role-based Access**: Admin routes check user role

## 💳 Payment Integration

Currently supports:
- ✅ Cash payment (at office)
- ✅ UPI payment (Coming soon)
- ✅ Bank transfer (Coming soon)
- ✅ Online payment (Coming soon)

**Payment Flow**:
1. User creates booking
2. Chooses payment method
3. Makes payment
4. Admin marks as paid
5. Booking status updates

## 🚀 Deployment

### Frontend (Vercel/Netlify)
```bash
cd client
npm run build
# Upload build/ folder to hosting
```

### Backend (Railway/Render/Heroku)
```bash
cd server
# Update .env with production values
npm start
```

### Environment Variables for Production:
```env
NODE_ENV=production
MONGODB_URI=your_production_mongodb_uri
JWT_SECRET=strong_production_secret
FRONTEND_URL=https://your-frontend-domain.com
CORS_ORIGIN=https://your-frontend-domain.com
```

## 🐛 Troubleshooting

### Common Issues:

1. **MongoDB Connection Failed**
   ```bash
   # Check if MongoDB is running
   mongosh --eval "db.version()"
   
   # Restart MongoDB service
   sudo systemctl restart mongod
   ```


3. **Module Not Found Errors**
   ```bash
   # Reinstall dependencies
   rm -rf node_modules package-lock.json
   npm run install-all
   ```

4. **JWT Authentication Issues**
   ```bash
   # Clear browser localStorage
   localStorage.clear()
   
   # Or use incognito mode
   ```

## 📝 Development Notes

### Adding New Services
1. Add service to `server/models/Service.js` schema
2. Add to seed data in `server/seed.js`
3. Run `npm run seed` to add to database

### Creating New Pages
1. Create component in `client/src/pages/`
2. Add route in `client/src/App.jsx`
3. Update navigation if needed

### API Development
1. Create controller in `server/controllers/`
2. Add route in `server/routes/`
3. Test with Postman/Thunder Client

## 🧪 Testing

```bash
# Test API endpoints
curl http://localhost:5000/api/health
curl http://localhost:5000/api/services

# Test with seeded users
# Login as admin and test admin endpoints
```


## 📄 License

This project is licensed under the MIT License.

## 📧 Support

For support, email: `support@1point1solution.com` or create an issue in the repository.

---

**Quick Start Cheat Sheet:**

Copy this into your project as `README.md` and give it to any coder. They just need to follow these 4 commands:

```bash
# 1. Clone
git clone https://github.com/shaikhmoiz3010/1point1solution
cd 1Point-1Solution

# 2. Install everything
npm run install-all

# 3. Setup environment (copy .env.example to .env)
cp server/.env.example server/.env
cp client/.env.example client/.env

# 4. Seed database
npm run seed

# 5. Start development
npm run dev
```

**Happy Coding!** 🚀