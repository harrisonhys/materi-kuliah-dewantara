# Pertemuan 15: Final Project — Full-Stack E-Commerce Application

## 1. Project Overview

**Objective:** Build comprehensive full-stack e-commerce application integrating:
- Vue.js 3 frontend with Composition API
- Express.js REST API backend
- PostgreSQL/MySQL database
- User authentication (JWT)
- Shopping cart & checkout
- Payment integration
- Real-time notifications
- Admin dashboard

**Time:** 3 weeks
**Format:** Individual atau pair
**Deliverables:** Working application, documentation, presentation

## 2. Project Requirements

### 2.1 Frontend Features

```
Homepage:
- Product listing with filters (category, price, rating)
- Search functionality
- Product detail page
- Shopping cart (add, remove, update quantity)

Authentication:
- Register/Login/Logout
- JWT token management
- Protected routes

User Features:
- Profile management (update info, change password)
- Order history with status tracking
- Wishlist management
- Reviews & ratings

Cart & Checkout:
- Cart summary (subtotal, tax, shipping)
- Shipping address form
- Payment method selection
- Order confirmation

Admin Dashboard:
- Product management (CRUD)
- User management
- Order management
- Sales analytics
```

### 2.2 Backend Requirements

```
Authentication:
- User registration with validation
- Login with password hashing (bcrypt)
- JWT token generation & refresh
- Role-based access control (RBAC)

Products API:
- CRUD operations
- Filtering, sorting, pagination
- Search with full-text search
- Image upload to S3

Orders API:
- Create order from cart
- Order status tracking
- Payment verification
- Invoice generation

Users API:
- Profile management
- Address management
- Order history

Admin API:
- User management
- Product management
- Sales reports
- Analytics

Security:
- Input validation (Joi)
- Rate limiting
- CORS configuration
- SQL injection prevention
- XSS protection
```

### 2.3 Database Schema

```sql
Users:
- id, email, password_hash, name, phone, role, created_at, updated_at

Products:
- id, sku, name, description, price, stock, category, image_url, created_at

Orders:
- id, user_id, status, total_price, shipping_address, created_at

OrderItems:
- id, order_id, product_id, quantity, unit_price

Payments:
- id, order_id, amount, status, payment_method, external_id

Reviews:
- id, product_id, user_id, rating, comment, created_at
```

## 3. Technical Stack

```
Frontend:
- Vue.js 3 (Composition API)
- Vite (build tool)
- Vue Router (navigation)
- Pinia (state management)
- Axios (HTTP client)
- Tailwind CSS (styling)

Backend:
- Node.js + Express.js
- Sequelize or Prisma (ORM)
- PostgreSQL or MySQL
- JWT (authentication)
- Multer (file upload)
- Socket.io (real-time)

Deployment:
- Frontend: Vercel or Netlify
- Backend: Railway or Heroku
- Database: Cloud hosted
- Storage: AWS S3
```

## 4. Project Structure

```
my-ecommerce/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── ProductCard.vue
│   │   │   ├── CartSummary.vue
│   │   │   └── ...
│   │   ├── pages/
│   │   │   ├── Home.vue
│   │   │   ├── Products.vue
│   │   │   ├── ProductDetail.vue
│   │   │   ├── Cart.vue
│   │   │   ├── Checkout.vue
│   │   │   └── ...
│   │   ├── stores/
│   │   │   ├── cartStore.js
│   │   │   ├── authStore.js
│   │   │   └── ...
│   │   ├── router/
│   │   │   └── index.js
│   │   ├── api/
│   │   │   ├── client.js
│   │   │   ├── services/
│   │   │   │   ├── productService.js
│   │   │   │   └── orderService.js
│   │   │   └── ...
│   │   └── main.js
│   ├── package.json
│   └── vite.config.js
│
├── backend/
│   ├── src/
│   │   ├── models/
│   │   │   ├── User.js
│   │   │   ├── Product.js
│   │   │   └── Order.js
│   │   ├── routes/
│   │   │   ├── auth.js
│   │   │   ├── products.js
│   │   │   ├── orders.js
│   │   │   └── admin.js
│   │   ├── middleware/
│   │   │   ├── auth.js
│   │   │   ├── validation.js
│   │   │   └── errorHandler.js
│   │   ├── services/
│   │   │   ├── authService.js
│   │   │   ├── orderService.js
│   │   │   └── paymentService.js
│   │   ├── app.js
│   │   └── server.js
│   ├── migrations/
│   ├── package.json
│   └── .env
│
└── README.md
```

## 5. Implementation Guide

### Phase 1: Database & Backend (Week 1)

```
Day 1-2: Setup & Database
- Initialize Node.js project
- Setup database (PostgreSQL/MySQL)
- Create models (User, Product, Order, etc)
- Setup migrations

Day 3-4: Authentication
- User registration endpoint
- Login endpoint (JWT)
- Token refresh endpoint
- Authentication middleware

Day 5: Products API
- Product CRUD endpoints
- Listing with pagination
- Search and filters
- Image upload to S3

Day 6-7: Orders API
- Create order endpoint
- Order status tracking
- Order history endpoint
- Payment integration
```

### Phase 2: Frontend (Week 1-2)

```
Day 1-2: Setup & Pages
- Vue.js project setup (Vite)
- Router configuration
- Store setup (Pinia)
- Basic pages (Home, Products, etc)

Day 3-4: Components & Features
- Product components
- Shopping cart implementation
- User profile page
- Order history

Day 5-6: Forms & Validation
- Login/Register forms
- Checkout form
- Form validation (Vee-Validate)
- Error handling

Day 7: Integration & Testing
- API integration
- End-to-end testing
- Performance optimization
```

### Phase 3: Polish & Deployment (Week 2-3)

```
Day 1-2: Admin Dashboard
- Product management interface
- User management interface
- Order management interface
- Sales analytics

Day 3-4: Optimization
- Code splitting
- Image optimization
- Caching strategies
- Performance profiling

Day 5: Testing
- Unit tests (Jest)
- Component tests (Vue Test Utils)
- API tests
- Coverage > 80%

Day 6-7: Deployment
- Backend deployment (Railway/Heroku)
- Frontend deployment (Vercel/Netlify)
- Database setup
- Environment configuration
```

## 6. Evaluation Criteria

| Aspect | Weight | Points |
|--------|--------|--------|
| **Functionality** | 30% | Working features end-to-end |
| **Code Quality** | 20% | Clean, readable, maintainable |
| **Database Design** | 15% | Normalized, indexed, optimized |
| **User Experience** | 15% | Responsive, intuitive, smooth |
| **Documentation** | 10% | README, API docs, deployment guide |
| **Deployment** | 10% | Working in production, monitoring |

## 7. Grading Rubric

**Functionality (30 pts):**
- Core features work: 15 pts
- Edge cases handled: 8 pts
- Performance acceptable: 7 pts

**Code Quality (20 pts):**
- SOLID principles: 8 pts
- Proper error handling: 7 pts
- Modular structure: 5 pts

**Database (15 pts):**
- Correct schema: 8 pts
- Indexes optimized: 5 pts
- No N+1 queries: 2 pts

**UX (15 pts):**
- Responsive design: 8 pts
- Smooth interactions: 5 pts
- Good error messages: 2 pts

**Documentation (10 pts):**
- README complete: 5 pts
- API documented: 3 pts
- Deployment guide: 2 pts

**Deployment (10 pts):**
- Frontend deployed: 3 pts
- Backend deployed: 3 pts
- Database accessible: 2 pts
- Monitoring setup: 2 pts

## 8. Submission Checklist

```
[ ] Frontend
  [ ] All components built
  [ ] Forms validated
  [ ] API integrated
  [ ] Tests passing (>80% coverage)
  [ ] Deployed to Vercel/Netlify
  [ ] Responsive on mobile
  
[ ] Backend
  [ ] All endpoints implemented
  [ ] Input validation working
  [ ] Authentication secure
  [ ] Tests passing (>80% coverage)
  [ ] Deployed to Railway/Heroku
  [ ] Environment variables secured
  
[ ] Database
  [ ] Schema created
  [ ] Migrations working
  [ ] Indexes added
  [ ] Backups configured
  
[ ] Documentation
  [ ] README with setup instructions
  [ ] API documentation (Swagger)
  [ ] Architecture diagram
  [ ] Deployment guide
  [ ] Known issues/limitations
  
[ ] Deployment
  [ ] Frontend live and accessible
  [ ] Backend live and accessible
  [ ] Database connected
  [ ] SSL/HTTPS enabled
  [ ] Monitoring configured
```

## 9. Common Pitfalls to Avoid

```
❌ Hardcoded credentials
❌ No input validation
❌ Missing error handling
❌ No tests
❌ Not using indexes
❌ No pagination
❌ Sensitive data in logs
❌ No CORS handling
❌ Weak passwords
❌ No rate limiting
❌ Not handling async errors
❌ Storing files locally
❌ No environment variables
```

## 10. Nice-to-Have Features

```
🌟 Email notifications (order status)
🌟 Real-time notifications (Socket.io)
🌟 Wishlist feature
🌟 Product recommendations
🌟 User reviews & ratings
🌟 Order tracking with map
🌟 Multiple payment methods
🌟 Coupon/discount system
🌟 Product comparison
🌟 Email verification
```

## 11. Resources

- [Vue.js Documentation](https://vuejs.org)
- [Express.js Guide](https://expressjs.com)
- [Sequelize ORM](https://sequelize.org)
- [JWT Auth](https://jwt.io)
- [Stripe API](https://stripe.com) (for payments)
- [Vercel Deployment](https://vercel.com)
- [Railway Deployment](https://railway.app)

## 12. Presentation Guide

**15 minutes total:**
1. Problem statement (1 min) — what problem does this solve?
2. Architecture diagram (2 min) — frontend, backend, database
3. Live demo (7 min) — show working features
4. Technical highlights (3 min) — challenges, solutions
5. Q&A (2 min)

**Slides include:**
- Title slide
- Problem & target users
- Architecture diagram
- Tech stack
- Key features
- Live demo screenshots
- Challenges & lessons
- Future improvements

**Demo script:**
1. Homepage with products
2. Filter/search
3. Add to cart
4. Checkout flow
5. Order confirmation
6. Admin dashboard

Good luck! 🚀
