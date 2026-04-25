# 🛍️ Zambian E-Commerce Store

A full-stack e-commerce web application built with the MERN stack (MongoDB, Express, React, Node.js). Features a dark editorial design, admin product management, image uploads via Cloudinary, and full order flow from browsing to checkout.

---

## 👀Live Demo
https://ecommerce-frontend-ten-plum.vercel.app/

note: For sample of the admin account features use email: test@gmail.com with password: User123

## 📸 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, React Router v6, Axios, Vite |
| Backend | Node.js, Express.js |
| Database | MongoDB Atlas (Mongoose ODM) |
| Auth | JWT (JSON Web Tokens) |
| Image Uploads | Cloudinary + Multer |
| Deployment | Vercel (Frontend) · Render (Backend) · MongoDB Atlas (DB) |

---

## 📁 Repository Structure

This repository uses **Git submodules**. The frontend and backend are maintained as separate repositories and linked here as submodules.

```
ecommerce-store/
├── Frontend/          ← Git submodule (React / Vite)
├── Backend/           ← Git submodule (Node / Express)
└── README.md
```

### Cloning with submodules

```bash
# Clone the repo and initialise all submodules in one command
git clone --recurse-submodules https://github.com/your-username/ecommerce-store.git

# If you already cloned without submodules, run:
git submodule update --init --recursive
```

### Updating submodules

```bash
# Pull latest changes for all submodules
git submodule update --remote --merge
```

---

## ✨ Features

### Storefront
- Browse all products on a responsive home page
- Live search filtering by name, category, and description
- Product detail page with image, stock status, qty selector, and add-to-cart
- Persistent shopping cart stored in `localStorage`
- Floating cart button with live item count badge

### Auth
- Register and login on a single toggling page
- JWT-based authentication stored in `localStorage`
- Protected routes — guests are redirected to `/auth`
- User menu (top-right) shows name, avatar initials, and role badge

### Checkout & Orders
- Checkout form with shipping address validation
- Orders saved to MongoDB with full item breakdown
- Order history page with expandable accordion cards
- Mark-as-paid flow

### Admin Dashboard
- Create, edit, and delete products
- Real image uploads via Cloudinary (drag-and-drop style file picker)
- Live image preview before saving
- Product grid with inline edit/delete actions

---

## 🚀 Getting Started

### Prerequisites

- Node.js v18+
- npm v9+
- A [MongoDB Atlas](https://cloud.mongodb.com) account
- A [Cloudinary](https://cloudinary.com) account

---

### 1. Clone the repository

```bash
git clone --recurse-submodules https://github.com/your-username/ecommerce-store.git
cd ecommerce-store
```

---

### 2. Backend setup

```bash
cd Backend
npm install
```

Create a `.env` file in the `Backend/` directory:

```env
PORT=5000
MONGO_URI=mongodb+srv://<user>:<password>@cluster0.xxxxx.mongodb.net/myshop?retryWrites=true&w=majority
JWT_SECRET=your_super_secret_key
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

Start the backend:

```bash
npm run dev      # with nodemon (recommended)
# or
npm start        # without nodemon
```

Backend runs on `http://localhost:5000`

---

### 3. Frontend setup

```bash
cd Frontend
npm install
```

Create a `.env` file in the `Frontend/` directory:

```env
VITE_API_URL=http://localhost:5000/api
```

Start the frontend:

```bash
npm run dev
```

Frontend runs on `http://localhost:5173`

---

## 🗄️ Database Models

### User
| Field | Type | Notes |
|---|---|---|
| `name` | String | Required |
| `email` | String | Required, unique |
| `password` | String | Hashed with bcrypt |
| `role` | String | `"user"` or `"admin"`, default `"user"` |

### Product
| Field | Type | Notes |
|---|---|---|
| `name` | String | Required |
| `price` | Number | Required |
| `description` | String | |
| `image` | String | Cloudinary URL |
| `category` | String | |
| `countInStock` | Number | Default `0` |

### Order
| Field | Type | Notes |
|---|---|---|
| `user` | ObjectId | Ref → User |
| `orderItems` | Array | name, qty, image, price, product ref |
| `shippingAddress` | Object | address, city, country |
| `totalPrice` | Number | |
| `isPaid` | Boolean | Default `false` |
| `isDelivered` | Boolean | Default `false` |

---

## 🔌 API Endpoints

### Auth — `/api/auth`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/register` | Public | Register a new user |
| POST | `/login` | Public | Login and receive JWT |

### Products — `/api/products`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| GET | `/` | Public | Get all products |
| GET | `/:id` | Public | Get single product |
| POST | `/` | Admin | Create product |
| PUT | `/:id` | Admin | Update product |
| DELETE | `/:id` | Admin | Delete product |

### Orders — `/api/orders`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/` | Private | Place an order |
| GET | `/myorders` | Private | Get logged-in user's orders |
| PUT | `/:id/pay` | Private | Mark order as paid |

### Upload — `/api/upload`
| Method | Endpoint | Access | Description |
|---|---|---|---|
| POST | `/` | Admin | Upload image to Cloudinary |

---

## 🌐 Deployment

### MongoDB Atlas
1. Create a free M0 cluster at [cloud.mongodb.com](https://cloud.mongodb.com)
2. Create a database user and whitelist `0.0.0.0/0` under Network Access
3. Copy the connection string and set it as `MONGO_URI` in your backend env vars

### Backend → Render
1. Push `Backend/` to its own GitHub repo
2. New Web Service on [render.com](https://render.com)
3. Build command: `npm install` · Start command: `node server.js`
4. Add all environment variables from `.env` in the Render dashboard
5. Note your service URL: `https://your-app.onrender.com`

> ⚠️ Free tier instances spin down after 15 min of inactivity. The first request after sleep takes ~30 seconds.

### Frontend → Vercel
1. Push `Frontend/` to its own GitHub repo
2. New Project on [vercel.com](https://vercel.com), import the repo
3. Framework: **Vite** · Output dir: `dist`
4. Add environment variable: `VITE_API_URL=https://your-app.onrender.com/api`
5. Add a `vercel.json` in the frontend root for React Router support:

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/" }
  ]
}
```

---

## 🔐 Environment Variables Reference

### Backend (`Backend/.env`)

| Variable | Description |
|---|---|
| `PORT` | Server port (default 5000) |
| `MONGO_URI` | MongoDB Atlas connection string |
| `JWT_SECRET` | Secret key for signing JWTs |
| `CLOUDINARY_CLOUD_NAME` | Cloudinary account cloud name |
| `CLOUDINARY_API_KEY` | Cloudinary API key |
| `CLOUDINARY_API_SECRET` | Cloudinary API secret |
| `NODE_ENV` | `development` or `production` |
| `FRONTEND_URL` | Vercel frontend URL (for CORS) |

### Frontend (`Frontend/.env`)

| Variable | Description |
|---|---|
| `VITE_API_URL` | Full base URL of your backend API |

---

## 📂 Frontend Pages

| Route | Component | Access |
|---|---|---|
| `/` | `Home` | Public |
| `/auth` | `AuthPage` | Public (redirects if logged in) |
| `/product/:id` | `Product` | Public |
| `/cart` | `Cart` | Protected |
| `/checkout` | `Checkout` | Protected |
| `/orders` | `Orders` | Protected |
| `/admin` | `Admin` | Protected |

---

## 🎨 Design System

All styles live in `Frontend/src/App.css`. The design uses a **dark editorial theme** with CSS custom properties:

| Variable | Value | Usage |
|---|---|---|
| `--bg` | `#0f0f0f` | Page background |
| `--surface` | `#181818` | Cards, panels |
| `--accent` | `#d4a843` | Amber — buttons, badges, highlights |
| `--text` | `#e8e4dc` | Primary text |
| `--text-muted` | `#7a7570` | Secondary text |
| `--font-display` | Playfair Display | Headings, prices |
| `--font-body` | DM Sans | All other text |

---

## 🛠️ Scripts

### Backend
```bash
npm start          # Start with node
npm run dev        # Start with nodemon (auto-restart)
```

### Frontend
```bash
npm run dev        # Vite dev server
npm run build      # Production build → dist/
npm run preview    # Preview production build locally
```

---

## 📄 License

MIT — free to use and modify for personal or commercial projects.
