# 🚀 Flame Japanese Hibachi (FJH)

A production-ready, full-stack restaurant management platform built with a modular monolith architecture. FJH enables seamless multi-location restaurant operations with public ordering, store management, and administrative dashboards.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Features](#features)
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Environment Setup](#environment-setup)
- [Development](#development)
- [API Documentation](#api-documentation)
- [Database](#database)
- [Contributing](#contributing)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## 🎯 Overview

**Flame Japanese Hibachi (FJH)** is a comprehensive restaurant management system designed to handle the complexity of multi-location dining operations. The platform provides:

- **Customer-Facing:** Intuitive public ordering interface with real-time status tracking
- **Store Operations:** Order management, kitchen workflows, and inventory tracking
- **Admin Suite:** Comprehensive dashboards for analytics, reporting, and multi-location management
- **Scalable Backend:** Robust API supporting menus, orders, users, locations, and more

**Key Philosophy:** Performance, maintainability, and clean architecture through modular design.

---

## 🧱 Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------| 
| **Frontend** | Next.js 14+ (App Router) | Server-side rendering & optimized performance |
| **Language** | TypeScript | Type-safe development & better DX |
| **Styling** | Tailwind CSS | Utility-first CSS framework |
| **Backend** | Next.js API Routes | Serverless API endpoints |
| **Database** | PostgreSQL 15+ | Relational data with ACID compliance |
| **ORM** | Prisma 5+ | Type-safe database client & migrations |
| **Auth** | NextAuth.js / JWT | Session management & security |
| **Validation** | Zod / Yup | Runtime type validation |
| **Testing** | Jest / Vitest | Unit & integration tests |

---

## 🏗️ Architecture

### Architecture Pattern
**Modular Monolith** — Single repository with feature-based module organization

**Benefits:**
- ✅ Simplified deployment (one application)
- ✅ Easier future migration to microservices
- ✅ Reduced operational complexity
- ✅ Shared type system across layers

### System Layers

```
┌─────────────────────────────────────────────────────┐
│   Presentation Layer (Next.js UI)                   │  Pages, Components, Forms
├─────────────────────────────────────────────────────┤
│   API Layer (Next.js Routes)                        │  REST endpoints, Auth
├─────────────────────────────────────────────────────┤
│   Business Logic Layer (Services)                   │  Order processing, Analytics
├─────────────────────────────────────────────────────┤
│   Data Access Layer (Prisma ORM)                    │  Database queries
├─────────────────────────────────────────────────────┤
│   Database Layer (PostgreSQL)                       │  Persistent data storage
└─────────────────────────────────────────────────────┘
```

### Key Domains

1. **Ordering System** — Menu management, cart operations, checkout
2. **Location Management** — Multi-location support, store configurations
3. **User Management** — Authentication, roles (customer, staff, admin)
4. **Order Fulfillment** — Status tracking, kitchen workflows
5. **Analytics & Reporting** — Sales reports, performance metrics

---

## 📁 Project Structure

```
Flame-Japanese-Hibachi-FJH/
├── app/                                   # Next.js app directory
│   ├── (auth)/                            # Auth-related pages
│   │   ├── login/page.tsx
│   │   ├── register/page.tsx
│   │   └── forgot-password/page.tsx
│   ├── (public)/                          # Public-facing pages
│   │   ├── menu/page.tsx
│   │   ├── cart/page.tsx
│   │   └── checkout/page.tsx
│   ├── (dashboard)/                       # Protected dashboard pages
│   │   ├── admin/
│   │   ├── store/
│   │   └── customer/
│   ├── api/                               # API routes
│   │   ├── auth/
│   │   ├── orders/
│   │   ├── menu/
│   │   ├── locations/
│   │   └── users/
│   ├── layout.tsx                         # Root layout
│   └── page.tsx                           # Home page
├── components/                             # Reusable React components
│   ├── ui/                                # UI components
│   ├── forms/                             # Form components
│   ├── cards/                             # Card components
│   └── navigation/                        # Navigation components
├── lib/                                    # Utility functions
│   ├── auth.ts                            # Auth utilities
│   ├── db.ts                              # Prisma client
│   ├── validation.ts                      # Schema validation
│   └── api-client.ts                      # API utilities
├── styles/                                 # Global styles
│   └── globals.css
├── prisma/                                 # Database configuration
│   ├── schema.prisma                      # Database schema
│   └── migrations/                        # Database migrations
├── types/                                  # TypeScript type definitions
│   ├── index.ts
│   ├── api.ts
│   └── domain.ts
├── hooks/                                  # Custom React hooks
│   ├── useAuth.ts
│   └── useOrders.ts
├── public/                                 # Static assets
│   ├── images/
│   └── icons/
├── .env.example                            # Environment variables template
├── .env.local                              # Local environment variables
├── tsconfig.json                           # TypeScript configuration
├── tailwind.config.js                      # Tailwind CSS configuration
├── next.config.js                          # Next.js configuration
├── package.json                            # Project dependencies
├── README.md                               # This file
└── .gitignore
```

---

## ✨ Features

### 🛒 Customer Features
- [ ] Browse restaurant locations & menus
- [ ] Real-time menu availability
- [ ] Shopping cart with persistent storage
- [ ] Multiple payment methods (Stripe, PayPal)
- [ ] Order tracking in real-time
- [ ] Order history & reorder functionality
- [ ] Dietary preferences & special requests

### 🏪 Store Management
- [ ] Order queue & priority management
- [ ] Kitchen display system (KDS)
- [ ] Inventory tracking & alerts
- [ ] Staff shift management
- [ ] Store-specific analytics dashboard

### 👨‍💼 Admin Features
- [ ] Multi-location overview & comparison
- [ ] Revenue & sales analytics
- [ ] User & customer management
- [ ] Menu & pricing management across locations
- [ ] Staff & permissions management
- [ ] System-wide reporting & exports

---

## 🚀 Getting Started

### Prerequisites

- **Node.js:** 18.17+ or 19+
- **npm/yarn:** Latest stable version
- **PostgreSQL:** 14+ (local or remote)
- **Git:** For version control

### Quick Start (5 minutes)

1. **Clone the repository**
   ```bash
   git clone https://github.com/sunitsen/Flame-Japanese-Hibachi-FJH.git
   cd Flame-Japanese-Hibachi-FJH
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Setup environment variables**
   ```bash
   cp .env.example .env.local
   # Edit .env.local with your database credentials
   ```

4. **Setup database**
   ```bash
   npx prisma migrate dev --name init
   ```

5. **Run development server**
   ```bash
   npm run dev
   ```

   Open [http://localhost:3000](http://localhost:3000) in your browser. 🎉

---

## ⚙️ Installation

### Detailed Setup Instructions

#### 1. Clone Repository
```bash
git clone https://github.com/sunitsen/Flame-Japanese-Hibachi-FJH.git
cd Flame-Japanese-Hibachi-FJH
```

#### 2. Install Dependencies
```bash
npm install
# or with yarn
yarn install
```

#### 3. PostgreSQL Setup

**macOS (Homebrew)**
```bash
brew install postgresql
brew services start postgresql
```

**Linux (Ubuntu/Debian)**
```bash
sudo apt-get install postgresql postgresql-contrib
sudo service postgresql start
```

**Windows**
Download from [postgresql.org](https://www.postgresql.org/download/windows/)

Create a database:
```bash
createdb fjh_development
```

#### 4. Configure Environment Variables
See [Environment Setup](#environment-setup) section below.

#### 5. Run Database Migrations
```bash
npx prisma migrate dev --name init
```

#### 6. Seed Database (Optional)
```bash
npx prisma db seed
```

#### 7. Start Development Server
```bash
npm run dev
```

Access the application at `http://localhost:3000`

---

## 🔑 Environment Setup

Create `.env.local` in the root directory with the following variables:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/fjh_development"

# NextAuth Configuration
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-secret-key-min-32-chars-for-production"
NEXTAUTH_ADMIN_EMAIL="admin@fjh.local"

# API Configuration
NEXT_PUBLIC_API_BASE_URL="http://localhost:3000/api"

# Payment Processing (Optional)
STRIPE_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."

# Email Service (Optional)
EMAIL_FROM="noreply@fjh.local"
SMTP_HOST="smtp.mailtrap.io"
SMTP_PORT=465
SMTP_USER="your-smtp-user"
SMTP_PASSWORD="your-smtp-password"

# Application
NODE_ENV="development"
```

**⚠️ Important Security Notes:**
- Never commit `.env.local` to version control
- Generate a secure `NEXTAUTH_SECRET`: `openssl rand -base64 32`
- Use environment-specific values for production
- Rotate secrets regularly

---

## 💻 Development

### Available Scripts

```bash
# Development server with hot reload
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Run TypeScript compiler check
npm run type-check

# Run linting (ESLint)
npm run lint

# Format code (Prettier)
npm run format

# Run tests
npm test

# Database management
npx prisma studio          # Visual database browser
npx prisma migrate dev     # Create new migration
npx prisma migrate deploy  # Apply migrations to production
```

### Development Workflow

1. **Create Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes & Test**
   ```bash
   npm run dev              # Start dev server
   npm run lint             # Check code quality
   npm test                 # Run tests
   ```

3. **Commit & Push**
   ```bash
   git add .
   git commit -m "feat: add new feature"
   git push origin feature/your-feature-name
   ```

4. **Create Pull Request** on GitHub

---

## 📚 API Documentation

### Authentication Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|----|
| POST | `/api/auth/register` | User registration | No |
| POST | `/api/auth/login` | User login | No |
| POST | `/api/auth/logout` | User logout | Yes |
| POST | `/api/auth/refresh` | Refresh JWT token | Yes |
| GET | `/api/auth/me` | Get current user | Yes |

### Orders Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|----|
| POST | `/api/orders` | Create new order | Yes |
| GET | `/api/orders` | List user orders | Yes |
| GET | `/api/orders/:id` | Get order details | Yes |
| PATCH | `/api/orders/:id` | Update order status | Yes (Admin) |

### Menu Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|----|
| GET | `/api/menu` | List menu items | No |
| GET | `/api/menu/:id` | Get menu item details | No |
| POST | `/api/menu` | Create menu item | Yes (Admin) |
| PATCH | `/api/menu/:id` | Update menu item | Yes (Admin) |
| DELETE | `/api/menu/:id` | Delete menu item | Yes (Admin) |

### Locations Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|----|
| GET | `/api/locations` | List all locations | No |
| GET | `/api/locations/:id` | Get location details | No |
| POST | `/api/locations` | Create location | Yes (Admin) |

### Example Requests

**Create an Order**
```bash
curl -X POST http://localhost:3000/api/orders \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "locationId": "loc_123",
    "items": [
      { "menuItemId": "item_1", "quantity": 2 },
      { "menuItemId": "item_2", "quantity": 1 }
    ],
    "specialRequests": "No spicy"
  }'
```

**Get User Orders**
```bash
curl -X GET http://localhost:3000/api/orders \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

---

## 📝 Database

### Schema Overview

```
Users
├── id (UUID)
├── email (String, unique)
├── role (CUSTOMER | STAFF | ADMIN)
└── createdAt (DateTime)

Locations
├── id (UUID)
├── name (String)
├── address (String)
└── manager (FK: Users)

MenuItems
├── id (UUID)
├── name (String)
├── locationId (FK: Locations)
├── price (Decimal)
└── isAvailable (Boolean)

Orders
├── id (UUID)
├── userId (FK: Users)
├── locationId (FK: Locations)
├── status (PENDING | PREPARING | READY | COMPLETED)
├── totalPrice (Decimal)
└── createdAt (DateTime)

OrderItems
├── id (UUID)
├── orderId (FK: Orders)
├── menuItemId (FK: MenuItems)
└── quantity (Int)
```

### Database Tools

**Visual Database Browser**
```bash
# Open Prisma Studio
npx prisma studio
```

**Running Migrations**
```bash
# Create new migration after schema changes
npx prisma migrate dev --name description_of_change

# Apply pending migrations (production)
npx prisma migrate deploy

# Reset database (development only!)
npx prisma migrate reset
```

---

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### Code of Conduct
- Be respectful and inclusive
- Focus on the code, not the person
- Help others learn and grow

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
   - Follow existing code style
   - Write clear, descriptive commits
   - Add tests for new features
4. **Run quality checks**
   ```bash
   npm run lint
   npm run type-check
   npm test
   ```
5. **Push to your fork**
6. **Create a Pull Request**
   - Reference any related issues (#123)
   - Describe your changes clearly

### Coding Standards

- **TypeScript:** Use strict mode
- **Naming:** camelCase for variables/functions, PascalCase for components/types
- **Comments:** Meaningful comments for complex logic
- **Imports:** Absolute imports using tsconfig `paths`
- **Error Handling:** Always use try-catch in async functions

---

## 🚢 Deployment

### Deployment Platforms

#### Vercel (Recommended for Next.js)

1. **Push to GitHub**
   ```bash
   git push origin main
   ```

2. **Connect to Vercel**
   - Go to [vercel.com](https://vercel.com)
   - Click "New Project" → "Import Git Repository"
   - Select your FJH repository

3. **Configure Environment Variables**
   - In Vercel dashboard → Settings → Environment Variables
   - Add all variables from `.env.example`

4. **Deploy**
   ```bash
   vercel
   ```

#### AWS

```bash
# Using AWS Amplify
npm install -g @aws-amplify/cli
amplify init
amplify add hosting
amplify publish
```

#### Docker

Create `Dockerfile`:
```dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package*.json ./
RUN npm ci --only=production
EXPOSE 3000
CMD ["npm", "start"]
```

Build and run:
```bash
docker build -t fjh .
docker run -p 3000:3000 fjh
```

---

## 🔧 Troubleshooting

### Common Issues & Solutions

#### "DATABASE_URL is not set"
```bash
# Ensure .env.local exists and contains DATABASE_URL
cp .env.example .env.local
# Edit .env.local with your database credentials
cat .env.local | grep DATABASE_URL
```

#### PostgreSQL Connection Error
```bash
# Check if PostgreSQL is running
sudo service postgresql status  # Linux
brew services list             # macOS

# Test connection
psql -U postgres -d fjh_development

# Reset connection
npx prisma db push
```

#### Port 3000 Already in Use
```bash
# Find process using port 3000
lsof -i :3000

# Run on different port
npm run dev -- -p 3001
```

#### Type Errors After Schema Changes
```bash
# Regenerate Prisma types
npx prisma generate

# Check for type issues
npm run type-check
```

#### Migration Fails
```bash
# Reset database (development only!)
npx prisma migrate reset

# Or manually resolve conflict
npx prisma migrate resolve --rolled-back migration_name
```

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 📞 Support & Contact

- **Issues:** [GitHub Issues](https://github.com/sunitsen/Flame-Japanese-Hibachi-FJH/issues)
- **Discussions:** [GitHub Discussions](https://github.com/sunitsen/Flame-Japanese-Hibachi-FJH/discussions)
- **Email:** support@fjh.local

---

## 🙏 Acknowledgments

Built with passion by the FJH team using modern web technologies.

---

**Last Updated:** 2026-04-16 | **Status:** Active Development 🚀