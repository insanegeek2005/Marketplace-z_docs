Marketplace-Z — Technical Documentation

A multi-vendor local marketplace for home décor, furniture, construction materials, and hardware products.

Project Overview

Marketplace-Z connects customers with local vendors for product discovery, direct purchases, quotation requests, and order fulfillment.

Technology Stack

* Frontend: Next.js, React, TypeScript
* Backend: Python, FastAPI
* Database: PostgreSQL
* ORM & Migrations: SQLAlchemy, Alembic
* Payments: Razorpay
* AI Assistant: LLM with authorized backend tools
* Messaging: Telegram Bot API
* Testing: Pytest, Playwright

Core Features

* Customer, vendor, and admin roles (RBAC)
* Admin-approved vendor onboarding
* Product catalog and inventory management
* Multi-vendor checkout and order tracking
* Direct purchases and quotation workflows
* Razorpay payment integration
* 5% platform commission on product subtotal only, excluding delivery charges
* Vendor-managed delivery or customer pickup
* Support ticket system
* GenAI assistant and Telegram bot integration

Backend Status

The initial FastAPI backend is running locally. The /api/v1/health endpoint has been verified, and Swagger documentation is available at /docs during local development.

Architecture Status

PostgreSQL integration, authentication, marketplace modules, payment processing, AI integration, and Telegram bot functionality remain planned development tasks until implemented and tested.

Security Priorities

* Role-based and resource-level authorization
* Server-side price and commission calculations
* Payment webhook signature verification
* Idempotent payment processing
* Secure secrets management and audit logs

Development Roadmap

1. PostgreSQL setup and database migrations
2. Authentication and RBAC
3. Vendor onboarding and product catalog
4. Cart, checkout, and orders
5. Quotations and payments
6. Fulfillment and support tickets
7. GenAI assistant and Telegram bot
8. Testing and deployment

Useful References

* FastAPI
* PostgreSQL
* SQLAlchemy
* Razorpay
* Telegram Bot API
