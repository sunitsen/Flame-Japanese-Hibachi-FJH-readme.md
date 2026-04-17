
# Flame Japanese Hibachi 🔥🍱

A production-ready multi-location restaurant ordering and management platform built with Next.js, PostgreSQL, and Prisma.

**Status:** 🚀 In Development | **Last Updated:** January 2024

---

## 📋 Quick Links

- **[Data Model Documentation](./DATA_MODEL.md)** - Complete schema design, relationships, and domain architecture
- **[API Endpoints Reference](./docs/API_ENDPOINTS_REFERENCE.md)** - Full REST API specification
- **[Implementation Guide](./docs/IMPLEMENTATION_GUIDE.md)** - Phase-by-phase development roadmap

---

## ✨ Overview

Flame Japanese Hibachi is a comprehensive ordering and management system for multi-location hibachi restaurants. The platform enables:

- 🛒 **Customer Ordering** - Browse, select location, customize items, apply promotions
- 📦 **Shop Management** - Manage locations, menus, inventory, orders, and staff
- 👥 **Role-Based Access** - Super Admin → Shop Admin → Employee → Customer → Guest
- 🚚 **DoorDash Integration** - Seamless order handoff with tracking and retry logic
- 📊 **Analytics & Reporting** - Shop-level and system-wide performance metrics
- 🔐 **Security & Compliance** - Audit logging, soft deletes, RBAC, encrypted sessions
- 🌐 **Multi-Provider Auth** - Local, Google, Facebook, Apple OAuth support

---

## 🏗️ Architecture

**Pattern:** Modular Monolith with Domain-Driven Design  
**Database:** PostgreSQL + Prisma ORM  
**Frontend:** Next.js 14+ (App Router)  
**Authentication:** NextAuth.js / Auth.js with JWT  
**Hosting:** (TBD - AWS/Vercel recommended)  

### Core Domains

| Domain | Models | Purpose |
|--------|--------|---------|
| **Authentication** | User, AuthAccount, Session, UserProfile | Identity & auth provider management |
| **Shop & Location** | Shop, Address, StoreOperatingHours, StoreStatus | Multi-location operations |
| **Menu & Inventory** | MenuItem, MenuCategory, ShopMenuItem, ModifierGroup, ModifierOption | Global menu + store overrides |
| **Cart & Checkout** | ShoppingCart, CartItem, CustomerAddress, SavedPaymentMethod | Stateful shopping experience |
| **Order & Fulfillment** | Order, OrderItem, OrderItemModifier, GuestOrderProfile, DoorDashDispatch | Order lifecycle & external integration |
| **Promotions & Discounts** | Promotion, Coupon | Discount eligibility & application |
| **Access Control & Audit** | EmployeeAssignment, AuditLog | RBAC & compliance logging |
| **Analytics** | AnalyticsSnapshot | Metrics & reporting |

**Total:** 27 models across 8 domains

See [Data Model Documentation](./DATA_MODEL.md) for complete design details.

---

## 🎯 Features

### For Customers
- ✅ Guest checkout (no registration required)
- ✅ Social authentication (Google, Facebook, Apple)
- ✅ Save delivery addresses and payment methods
- ✅ Browse shops by location
- ✅ View menu with item availability
- ✅ Add items to cart with modifiers/customizations
- ✅ Apply coupons and promotions
- ✅ Track order status in real-time
- ✅ View order history
- ✅ Loyalty points (future feature)

### For Shop Admins
- ✅ Manage shop information and operating hours
- ✅ Override menu item availability per location
- ✅ Adjust pricing for items at specific shops
- ✅ Manage inventory/stock quantities
- ✅ View and process incoming orders
- ✅ Update order status (confirm, preparing, ready)
- ✅ Handoff orders to DoorDash
- ✅ View shop-specific analytics and metrics
- ✅ Manage shop employees and permissions
- ✅ Create store-specific promotions

### For Super Admin
- ✅ Create and delete shops
- ✅ Manage global menu items (all shops)
- ✅ Create and manage global promotions
- ✅ Create and manage coupons
- ✅ View system-wide analytics
- ✅ Compare performance across stores
- ✅ Access audit logs for all actions
- ✅ Manage user roles and permissions

### For Employees
- ✅ View shop orders in real-time
- ✅ Update order status
- ✅ Manage menu item availability
- ✅ Update inventory/stock
- ✅ View assigned shop's operations

### System Features
- ✅ DoorDash integration with idempotent handoff
- ✅ Automatic retry with exponential backoff
- ✅ Soft deletes for compliance and history
- ✅ Complete audit trail
- ✅ Role-based access control (RBAC)
- ✅ Multi-location support
- ✅ Guest order tracking
- ✅ Pre-computed analytics snapshots

---

## 🛠️ Tech Stack

### Backend
- **Framework:** Next.js 14+ (API Routes or App Router)
- **Database:** PostgreSQL 14+
- **ORM:** Prisma 5+
- **Authentication:** NextAuth.js v5 / Auth.js
- **Validation:** Zod or Yup
- **Error Handling:** Custom error classes + global middleware
- **Logging:** Winston or Pino
- **Task Queue:** Bull (for background jobs)
- **Email:** SendGrid / AWS SES

### Frontend
- **Framework:** Next.js 14+ (React 18+)
- **Styling:** Tailwind CSS
- **State Management:** React Context / Zustand
- **API Client:** SWR or React Query
- **UI Components:** Shadcn/ui or custom

### DevOps
- **Version Control:** Git + GitHub
- **CI/CD:** GitHub Actions
- **Database Migration:** Prisma Migrate
- **Testing:** Jest + Supertest
- **Code Quality:** ESLint + Prettier

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- npm/yarn
- PostgreSQL 14+

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/flame-japanese-hibachi.git
   cd flame-japanese-hibachi
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Setup environment variables**
   ```bash
   cp .env.example .env.local
   ```

   Update `.env.local` with:
   ```env
   # Database
   DATABASE_URL="postgresql://user:password@localhost:5432/flame_hibachi"

   # NextAuth
   NEXTAUTH_URL="http://localhost:3000"
   NEXTAUTH_SECRET="your-secret-key-here"

   # OAuth Providers
   GOOGLE_CLIENT_ID="..."
   GOOGLE_CLIENT_SECRET="..."
   FACEBOOK_APP_ID="..."
   FACEBOOK_APP_SECRET="..."
   APPLE_CLIENT_ID="..."
   APPLE_TEAM_ID="..."
   APPLE_KEY_ID="..."
   APPLE_PRIVATE_KEY="..."

   # DoorDash Integration
   DOORDASH_API_KEY="..."
   DOORDASH_MERCHANT_ID="..."

   # External Services
   SENDGRID_API_KEY="..."
   STRIPE_PUBLIC_KEY="..."
   STRIPE_SECRET_KEY="..."
   ```

4. **Initialize the database**
   ```bash
   npx prisma migrate dev --name init
   npx prisma generate
   ```

5. **Seed the database (optional)**
   ```bash
   npx prisma db seed
   ```

6. **Start development server**
   ```bash
   npm run dev
   ```

7. **Open in browser**
   ```
   http://localhost:3000
   ```

### Database Explorer (Optional)
```bash
npx prisma studio
```
Opens interactive database UI at `http://localhost:5555`

---

## 📁 Project Structure

```
flame-japanese-hibachi/
├── prisma/
│   ├── schema.prisma          # Prisma data model
│   ├── migrations/            # Database migrations
│   └── seed.ts                # Seed script
│
├── src/
│   ├── api/                   # API routes/endpoints
│   │   ├── auth/
│   │   ├── shops/
│   │   ├── menu/
│   │   ├── orders/
│   │   ├── cart/
│   │   └── [other domains]
│   │
│   ├── middleware/            # Auth, RBAC, error handling
│   ├── services/              # Business logic
│   ├── lib/                   # Utilities (Prisma client, auth, etc.)
│   ├── types/                 # TypeScript types
│   ├── components/            # React components (frontend)
│   ├── pages/                 # Next.js pages
│   └── styles/                # Global styles
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── fixtures/
│
├── docs/
│   ├── DATA_MODEL.md          # Schema design (this file)
│   ├── API_ENDPOINTS_REFERENCE.md
│   └── IMPLEMENTATION_GUIDE.md
│
├── .env.example               # Example environment variables
├── package.json
├── tsconfig.json
├── README.md                  # This file
└── DATA_MODEL.md              # Data model documentation
```

---

## 📊 Development Phases

| Phase | Duration | Focus | Status |
|-------|----------|-------|--------|
| 1 | 2 weeks | Authentication (OAuth, sessions) | ⏳ Planned |
| 2 | 1 week | Shop Management (CRUD, admin) | ⏳ Planned |
| 3 | 1 week | Menu Management (items, modifiers) | ⏳ Planned |
| 4 | 1 week | Shopping Cart & Checkout | ⏳ Planned |
| 5 | 2 weeks | Order Management & Lifecycle | ⏳ Planned |
| 6 | 1 week | DoorDash Integration | ⏳ Planned |
| 7 | 1 week | Promotions & Coupons | ⏳ Planned |
| 8 | 1 week | Analytics & Reporting | ⏳ Planned |
| 9 | 1 week | Access Control & Audit | ⏳ Planned |
| 10 | 2 weeks | Testing & Security Audit | ⏳ Planned |
| 11 | 1 week | Deployment Preparation | ⏳ Planned |
| 12 | 1 week | Beta & Launch | ⏳ Planned |

**Total Estimated Timeline:** 15-16 weeks to production-ready platform

See [Implementation Guide](./docs/IMPLEMENTATION_GUIDE.md) for detailed phase breakdown.

---

## 🔐 Security Considerations

### Implementation Checklist
- [ ] All passwords hashed with bcrypt
- [ ] JWT tokens with short expiration (15 min)
- [ ] Refresh tokens with longer expiration (7 days)
- [ ] HTTPS only in production
- [ ] Secure cookies (httpOnly, sameSite, secure flags)
- [ ] Input validation on all endpoints (Zod/Yup)
- [ ] SQL injection prevention (Prisma parameterized queries)
- [ ] CSRF protection on state-changing endpoints
- [ ] Rate limiting on auth endpoints (5 req/min per IP)
- [ ] Rate limiting on API endpoints (100 req/min per user)
- [ ] Encrypted sensitive fields (SSN, full credit cards - use payment processor)
- [ ] CORS configured for frontend domains only
- [ ] API request signing for DoorDash webhooks
- [ ] Audit logging for all admin actions
- [ ] Regular secret rotation

### Compliance
- ✅ PCI DSS (credit card handling)
- ✅ GDPR (user data, soft deletes)
- ✅ Data retention policies
- ✅ Audit trail for regulatory review

---

## 🧪 Testing

### Running Tests
```bash
# Unit tests
npm run test:unit

# Integration tests
npm run test:integration

# All tests
npm test

# With coverage
npm run test:coverage
```

### Load Testing
```bash
# Install k6 (Grafana's load testing tool)
npm install -g k6

# Run load test
k6 run tests/load/order-creation.js
```

---

## 📡 API Documentation

Complete API endpoint specification available in [API_ENDPOINTS_REFERENCE.md](./docs/API_ENDPOINTS_REFERENCE.md)

**Key Endpoints:**
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login with email/password
- `POST /api/auth/oauth/callback` - OAuth provider callback
- `GET /api/shops` - List all shops
- `POST /api/orders` - Create order
- `PUT /api/orders/:id/status` - Update order status
- `GET /api/profile` - Get user profile
- `GET /api/analytics/dashboard` - View analytics

See full documentation [here](./docs/API_ENDPOINTS_REFERENCE.md).

---

## 🤝 Contributing

### Development Workflow
1. Create feature branch: `git checkout -b feature/your-feature`
2. Make changes and commit: `git commit -am 'Add new feature'`
3. Push to branch: `git push origin feature/your-feature`
4. Submit pull request

### Code Standards
- Follow ESLint/Prettier configuration
- Write TypeScript (strict mode)
- Add tests for new features (minimum 80% coverage)
- Document complex functions with JSDoc
- Reference relevant issue numbers in commits

### Before Submitting PR
- [ ] Tests pass locally (`npm test`)
- [ ] No linting errors (`npm run lint`)
- [ ] Code formatted (`npm run format`)
- [ ] Database migrations created if needed
- [ ] Documentation updated if needed
- [ ] Commit messages follow convention

---

## 🐛 Known Issues & Limitations

### Current Phase
- Guest order tracking is email-based (no persistent order history)
- DoorDash integration is stubbed (ready for API key)
- Analytics snapshots not yet automated
- Email notifications not yet implemented
- Payment processing relies on external processor

### Future Enhancements
- [ ] Loyalty points system
- [ ] Push notifications
- [ ] Mobile app (React Native)
- [ ] Inventory forecasting
- [ ] Dynamic pricing
- [ ] Subscription/meal plans
- [ ] Store ratings and reviews
- [ ] Referral program

---

## 📞 Support & Contact

### Getting Help
- 📖 Read [Data Model Documentation](./DATA_MODEL.md) for architecture questions
- 🔌 Check [API Endpoints Reference](./docs/API_ENDPOINTS_REFERENCE.md) for endpoint details
- 📋 Review [Implementation Guide](./docs/IMPLEMENTATION_GUIDE.md) for development phases
- 🐛 Open GitHub issues for bugs
- 💬 Start discussions for feature requests

### Team
- **Project Lead:** [Your Name]
- **Architecture:** [Team Member]
- **Frontend:** [Team Member]
- **Backend:** [Team Member]

---

## 📄 License

This project is licensed under the MIT License - see [LICENSE](./LICENSE) file for details.

---

## 🗺️ Roadmap

### Q1 2024
- [x] Data model design & documentation
- [ ] Phase 1-3: Auth, Shop, Menu management
- [ ] Frontend scaffolding

### Q2 2024
- [ ] Phase 4-6: Cart, Orders, DoorDash integration
- [ ] Beta testing with pilot shops
- [ ] Performance optimization

### Q3 2024
- [ ] Phase 7-9: Promotions, Analytics, RBAC
- [ ] Security audit & hardening
- [ ] Production deployment

### Q4 2024
- [ ] Launch to production
- [ ] Monitor metrics & stability
- [ ] Begin Phase 2 features (loyalty, reviews, etc.)

---

## 📚 Documentation Index

| Document | Purpose | Audience |
|----------|---------|----------|
| [README.md](./README.md) | Project overview | Everyone |
| [DATA_MODEL.md](./DATA_MODEL.md) | Schema design & architecture | Architects, Developers |
| [API_ENDPOINTS_REFERENCE.md](./docs/API_ENDPOINTS_REFERENCE.md) | REST API specification | Developers, QA |
| [IMPLEMENTATION_GUIDE.md](./docs/IMPLEMENTATION_GUIDE.md) | Phase-by-phase roadmap | Project Managers, Developers |
| [ARCHITECTURE.md](./docs/ARCHITECTURE.md) (future) | System design decisions | Architects |
| [DEPLOYMENT.md](./docs/DEPLOYMENT.md) (future) | Production setup & ops | DevOps, Developers |

---

## 🙏 Acknowledgments

Built with ❤️ using:
- [Next.js](https://nextjs.org/)
- [Prisma](https://www.prisma.io/)
- [NextAuth.js](https://next-auth.js.org/)
- [PostgreSQL](https://www.postgresql.org/)
- [TypeScript](https://www.typescriptlang.org/)

---

**Last Updated:** January 2024 | **Version:** 0.1.0 (Pre-Alpha)

For the latest updates, check the [CHANGELOG.md](./CHANGELOG.md)
