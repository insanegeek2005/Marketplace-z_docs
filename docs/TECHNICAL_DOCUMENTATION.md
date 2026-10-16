Marketplace-Z

Technical Documentation

Project: Marketplace-Z
Organization: MindV Technologies
Project Type: Multi-vendor local marketplace
Status: Backend foundation in development

⸻

1. Project Overview

Marketplace-Z is a multi-vendor marketplace platform designed for home décor, furniture, construction materials, and hardware products. It connects local vendors with customers through a unified digital platform.

Customers can browse products, purchase items directly, request quotations, choose eligible delivery or pickup options, and track orders. Vendors can manage their products, inventory, orders, and quotations after admin approval.

The platform also integrates online payments, vendor settlements, a GenAI-powered assistant, a Telegram bot, and a support-ticket system.

2. Project Objectives

* Connect customers with local vendors.
* Provide direct purchasing and quotation-based purchasing.
* Support vendor-managed delivery and customer pickup.
* Enable secure online payments.
* Automate platform commission calculations.
* Provide AI-assisted product discovery and customer support.
* Offer Telegram-based access to marketplace services.
* Maintain role-based access control and auditable transactions.

3. User Roles

Customer

* Register and authenticate.
* Browse products and vendors.
* Manage cart and checkout.
* Place orders and request quotations.
* Pay online and track orders.
* Raise support tickets.
* Interact with the GenAI assistant.

Vendor

* Submit a vendor application.
* Complete required onboarding.
* Create and manage product listings.
* Maintain inventory and pricing.
* Respond to quotation requests.
* Manage accepted orders and fulfillment.
* View eligible payout and settlement information.

Administrator

* Approve or reject vendor applications.
* Manage users, categories, and product listings.
* Monitor orders, payments, refunds, and settlements.
* Configure marketplace policies and commission rules.
* Manage support tickets and disputes.
* Review audit logs and platform activity.

4. Technology Stack

Component	Technology
Frontend	Next.js, React, TypeScript
Backend API	Python, FastAPI
Data validation	Pydantic
Database	PostgreSQL
ORM and migrations	SQLAlchemy, Alembic
Payment gateway	Razorpay
AI assistant	LLM with authorized backend tools
Messaging integration	Telegram Bot API
Testing	Pytest, Playwright
Deployment	To be finalized

The architecture should begin as a modular monolith rather than multiple independent microservices. This keeps the MVP easier to develop, test, and maintain.

5. System Architecture

The frontend communicates with the FastAPI backend through versioned REST APIs. The backend applies business rules, checks permissions, accesses PostgreSQL, and integrates with external services.

Customers / Vendors / Admins
             |
             v
     Next.js Frontend
             |
             v
       FastAPI Backend
          /   |    \
         v    v     v
 PostgreSQL Razorpay AI Assistant
                    |
                    v
              Support Tickets
       Telegram Bot
             |
             v
       FastAPI Backend

The Telegram bot should reuse the same authentication, authorization, business logic, and AI-service boundaries as the web application.

6. Backend API Structure

The backend currently uses FastAPI and a versioned API prefix.

backend/
├── app/
│   ├── main.py
│   ├── core/
│   │   └── config.py
│   ├── api/
│   │   └── v1/
│   │       └── router.py
│   ├── db/
│   └── modules/
└── requirements.txt

The API base path is:

/api/v1

The initial health endpoint is:

GET /api/v1/health

The interactive API documentation is available locally at:

http://127.0.0.1:8000/docs

This local URL works only when the development server is running. It is not a publicly hosted documentation URL.

7. Core Functional Modules

7.1 Authentication and Authorization

Implement secure registration, login, sessions or tokens, password hashing, and role-based permissions.

Every protected operation must also verify that the user owns or is authorized to access the requested resource.

7.2 Vendor Management

Vendor applications require administrative approval before the vendor can publish products. Payment-provider onboarding and marketplace vendor approval must be tracked separately.

7.3 Product Catalog

Manage categories, product descriptions, images, prices, stock quantities, vendor associations, and listing status.

7.4 Orders and Checkout

Support multi-vendor checkout while maintaining separate vendor orders for fulfillment and accounting.

The backend must calculate prices and totals rather than trusting values submitted by the frontend.

7.5 Quotations

Customers can submit quote requests. Vendors respond with quotations containing prices, quantities, validity periods, and applicable delivery information. An accepted quotation can proceed through the appropriate order and payment workflow.

7.6 Payments and Commission

Razorpay handles online payment processing.

The platform commission is 5% of the product subtotal only. Delivery charges are excluded from the commission calculation.

For example:

* Product subtotal: ₹10,000
* Commission rate: 5%
* Platform commission: ₹500
* Vendor amount before other adjustments: ₹9,500

Actual settlements must account for refunds, payment-provider fees where applicable, and other explicitly defined financial adjustments.

Vendor transfers require an appropriate supported Razorpay marketplace or linked-account arrangement. Standard payment-gateway integration alone should not be assumed to implement vendor payouts automatically.

7.7 Fulfillment

Support vendor-managed delivery and customer pickup. Track fulfillment status separately from payment and order status.

7.8 Support Tickets

Customers can create tickets for order issues, payment concerns, product problems, and general assistance. The system should support ticket status, priority, assignment, messages, and audit history.

7.9 GenAI Assistant

The assistant can help with product discovery, marketplace FAQs, order information, and support navigation.

All actions must use authorized backend tools. The assistant must not independently override prices, confirm unverified payments, expose private customer data, or perform unauthorized administrative operations.

7.10 Telegram Bot

The Telegram bot should initially support basic commands, product discovery, FAQs, and support-ticket creation. Account linking and identity verification must be implemented before exposing private order information.

8. Database Design

The following are proposed database entities; their final schema remains to be implemented.

* Identity: users, user_roles, sessions
* Vendors: vendor_profiles, vendor_reviews, vendor_service_areas
* Catalog: categories, products, product_images
* Shopping: carts, cart_items, checkout_groups
* Orders: orders, order_items, inventory_reservations
* Quotations: quote_requests, quote_items, quotations, quotation_items
* Finance: payments, payment_events, refunds, commission_ledger, vendor_settlements
* Fulfillment: fulfillments, fulfillment_events
* Support: support_tickets, ticket_messages, ticket_attachments, ticket_audit_logs
* AI: chat_sessions, chat_messages, chat_feedback
* Platform: notifications, audit_logs, idempotency_keys

Use database transactions and appropriate constraints to protect inventory, order consistency, and financial records.

9. Security Requirements

* Enforce role-based and resource-level authorization.
* Hash passwords securely.
* Store secrets in environment variables or a suitable secret manager.
* Never expose payment-provider secrets in frontend code.
* Verify payment webhook signatures.
* Make webhook processing idempotent.
* Recalculate prices and commissions on the backend.
* Protect against unauthorized access to orders and vendor records.
* Maintain auditable records for financial and administrative actions.
* Avoid storing raw payment-card information.
* Validate uploads and restrict access to private attachments.

10. Development Roadmap

1. Complete the backend project structure.
2. Configure PostgreSQL and SQLAlchemy.
3. Implement authentication and RBAC.
4. Build vendor onboarding and approval.
5. Implement categories, products, and inventory.
6. Implement cart, checkout, and orders.
7. Add quotation workflows.
8. Integrate Razorpay payments and settlement handling.
9. Implement fulfillment and support tickets.
10. Integrate the GenAI assistant and Telegram bot.
11. Add automated tests, security checks, and deployment configuration.

11. Current Development Status

The following has been verified during initial development:

* Python backend environment is available.
* FastAPI application starts successfully.
* Configuration is loaded through Pydantic Settings.
* API v1 router is registered.
* GET /api/v1/health returns a healthy response.
* Swagger documentation is accessible locally.

PostgreSQL configuration, database models, migrations, authentication, marketplace modules, payment integration, AI integration, and production deployment remain future implementation tasks.

12. Official Technical References

* FastAPI: https://fastapi.tiangolo.com/
* PostgreSQL: https://www.postgresql.org/docs/
* SQLAlchemy: https://docs.sqlalchemy.org/
* Alembic: https://alembic.sqlalchemy.org/
* Razorpay: https://razorpay.com/docs/
* Telegram Bot API: https://core.telegram.org/bots/api

⸻

Document note: This document describes the intended architecture and planned functionality of Marketplace-Z. Features identified as planned are not claimed to be implemented or production-ready.
