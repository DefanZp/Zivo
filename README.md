# Zivo — AI-Powered E-Commerce Platform

Zivo is a full-stack e-commerce platform with an integrated AI Shopping Agent. It combines a customer-facing storefront, an administration interface, a Laravel REST API, an independent AI service, and event-driven automation.

The project explores practical full-stack and AI engineering: authentication and authorization, product discovery, transactional checkout, payment integration, retrieval-augmented generation (RAG), tool integration through the Model Context Protocol (MCP), streaming responses, and automated notifications.

> **Repository purpose:** This repository is the central project overview and documentation hub. The application source code lives in the separate repositories listed below.

---

## Live Application

| Service | Link |
|---|---|
| Customer application | [Zivo Web App](https://zivo-front-end-next-5uvw.vercel.app/) |
| Backend API | [Zivo API](https://api.zivoapp.cloud) |

## Application Preview

The screenshots below show the customer-facing pages and administrator interface.

### Customer Application

#### Home Page

![Zivo home page](docs/images/preview/customer/home.png)

#### Product Catalog

![Product catalog](docs/images/preview/customer/product-catalog.png)

#### Product Details

![Product details](docs/images/preview/customer/product-detail.png)

#### Shopping Cart

![Shopping cart](docs/images/preview/customer/cart.png)

#### Checkout

![Checkout](docs/images/preview/customer/checkout.png)

#### AI Shopping Assistant

![AI Shopping Assistant](docs/images/preview/customer/ai-shopping-assistant.png)

#### Order History

![Order history](docs/images/preview/customer/order-history.png)

#### Order Details

![Order details](docs/images/preview/customer/order-detail.png)

#### Account Profile

![Account profile](docs/images/preview/customer/profile.png)

#### Address Management

![Address management](docs/images/preview/customer/addresses.png)

#### Login

![Login page](docs/images/preview/customer/login.png)

#### Registration

![Registration page](docs/images/preview/customer/register.png)

### Administrator Dashboard

#### Dashboard Overview

![Administrator dashboard](docs/images/preview/admin/dashboard.png)

#### Product Management

![Product management](docs/images/preview/admin/products.png)

#### Create Product

![Create product](docs/images/preview/admin/create-product.png)

#### Edit Product

![Edit product](docs/images/preview/admin/edit-product.png)

#### Order Management

![Order management](docs/images/preview/admin/orders.png)

#### Order Details

![Order details](docs/images/preview/admin/order-detail.png)

---

## Project Overview

Zivo is built as four cooperating parts:

1. **E-commerce frontend** — provides the storefront, customer account pages, checkout experience, and administrator interface.
2. **E-commerce backend** — owns the REST API, application data, authentication, authorization, and transactional business rules.
3. **AI Shopping Agent** — helps customers discover products and store-policy information through natural-language conversation, and exposes supported shopping actions through tools.
4. **Automation layer** — uses n8n to coordinate event-driven notifications and scheduled operational workflows.

The services are maintained in separate repositories so that each part can be developed, configured, and deployed independently.

## Key Features

### Customer Experience

- Customer registration, login, and email verification.
- Product catalog with search, category filtering, sorting, and pagination.
- Product detail pages.
- Shopping cart management.
- Saved delivery addresses and default address selection.
- Checkout and order creation.
- Midtrans Snap payment flow and payment-result pages.
- Order history and order details.
- AI-assisted product discovery through a chat interface.

### Administrator Experience

- Sales and revenue overview.
- Low-stock product monitoring.
- Product creation, editing, deletion, and image upload.
- Order listing, order details, and business-rule-based order status management.

### AI Shopping Assistant

- Natural-language product discovery.
- Semantic retrieval of product candidates using a product knowledge collection.
- Store-policy retrieval from a separate knowledge collection.
- Verification of current product information through the Laravel API.
- Streaming chat responses using Server-Sent Events (SSE).
- Tool-based cart operations for authenticated users, subject to backend authorization.

RAG retrieval is used to find relevant candidates and reference information. Laravel remains the source of truth for current product details, prices, stock, cart contents, orders, and other transactional data.

### Automation

The project documents three n8n workflow use cases:

- **Order created:** notify the administrator after an order is created.
- **Payment settled:** notify the customer after payment settlement.
- **Daily low-stock summary:** periodically retrieve low-stock products and prepare a summary for the administrator.

The workflow screenshots currently stored in this repository are visual references. The n8n repository snapshot contains deployment configuration but does not include workflow JSON exports, so node-level implementation and active workflow status should be verified in the n8n instance.

#### Workflow Snapshots

**Order Created → Admin Notification**

![Order created automation](docs/images/automation/order-created.png)

**Payment Settled → Customer Notification**

![Payment settled automation](docs/images/automation/payment-settled.png)

**Daily Low Stock AI Summary**

![Daily low-stock automation](docs/images/automation/low-stock.png)

These images document workflow canvases; they do not independently confirm that each workflow is currently active or successfully executing in production.

---

## System Architecture

The diagrams below use plain-text flowcharts. They are written directly in this README, so the architecture does not depend on screenshots or external diagram files.

### 1. Main E-Commerce Flow

```text
                 CUSTOMER / ADMIN
                        |
                        v
                 NEXT.JS FRONTEND
               Storefront / Admin UI
                        |
                 REST API requests
                        |
                        v
                  LARAVEL BACKEND
          Authentication, Catalog, Cart,
               Orders, and Payments
                        |
                 Read / write data
                        |
                        v
                    POSTGRESQL


LARAVEL BACKEND
      |
      +---- Authenticates users and checks permissions
      |
      +---- Applies e-commerce business rules
      |
      +---- Reads and updates application data
```

Laravel is the source of truth for identity, permissions, product data, prices, stock, cart contents, orders, and payment state. Frontend route guards control navigation but do not replace backend authorization.

### 2. AI Shopping Assistant Flow

```text
CUSTOMER
   |
   v
NEXT.JS CHAT WIDGET
   |
   v
NEXT.JS ROUTE HANDLER
   /api/chat/stream
   |
   | HTTP streaming request
   v
FASTAPI AI SERVICE
   |
   v
LANGGRAPH AGENT <--------------------> POSTGRESQL CHECKPOINTER
   |
   +-------------------+---------------------+
   |                   |                     |
   v                   v                     v
GEMINI MODEL       RAG RETRIEVAL          MCP TOOLS
                   |                     |
                   v                     +---- Product operations
             CHROMA COLLECTIONS          +---- Cart operations
             |                           |
             +-- Product catalog         | Live API requests
             +-- Store policies          v
                                    LARAVEL BACKEND
                                           |
                                           v
                                      POSTGRESQL


STREAMING RESPONSE (SSE)
FASTAPI  --->  NEXT.JS ROUTE HANDLER  --->  CHAT WIDGET
```

The agent uses Gemini for language reasoning, RAG to retrieve relevant product candidates or store-policy information, and MCP tools for supported live operations. Product and cart actions that need current application data go through Laravel. The response is streamed back to the browser using SSE events.

### 3. Payment and Automation Flow

```text
CUSTOMER CHECKOUT
       |
       v
LARAVEL BACKEND
       |
       | Creates payment request
       v
MIDTRANS SNAP
       |
       | Payment notification
       v
LARAVEL PAYMENT CALLBACK
       |
       +---- Verify notification signature
       |
       +---- Update payment / order state
       |
       +---- Commit database transaction
                    |
                    | After-commit webhook
                    v
                   N8N
                    |
                    v
          EMAIL / ADMIN / CUSTOMER
               NOTIFICATIONS


SCHEDULED N8N WORKFLOW
       |
       | Scheduled API request
       v
LARAVEL BACKEND
       |
       v
LOW-STOCK DATA / SUMMARY
       |
       v
ADMIN NOTIFICATION
```

This flow represents the integration use cases described by the project documentation. The n8n repository snapshot contains deployment configuration but no workflow JSON exports, so the exact nodes and current activation status still need to be checked in the n8n instance.

### Service Responsibilities

| Service | Main responsibility | Main technologies |
|---|---|---|
| Frontend | Storefront, account pages, admin UI, and chat interface | Next.js, React, TypeScript, Tailwind CSS, Zustand |
| Backend | REST API, identity, authorization, catalog, cart, checkout, orders, and payments | Laravel, PHP, PostgreSQL, Sanctum |
| AI service | Agent orchestration, retrieval, tool execution, and chat streaming | Python, FastAPI, LangChain, LangGraph, Gemini, MCP, Chroma |
| Automation | Event-driven and scheduled workflows | n8n, Docker, GitHub Actions |

**Database note:** the Laravel application database and LangGraph checkpointer use separate configuration variables. The supplied source snapshots do not establish whether production uses the same PostgreSQL instance or separate instances.

## Technical Highlights

### Transactional Checkout

The backend processes order creation inside a database transaction. It verifies address ownership, locks selected product rows, checks stock, calculates totals using current product data, creates the order and order items, decreases stock, and creates the related payment record.

### Payment Integration

Midtrans Snap is used to initiate payments. Payment notifications are verified by the backend using a SHA-512 signature check, and sensitive payment updates are handled through database transactions and row locking.

### AI Retrieval and Tool Integration

The AI service uses Chroma to retrieve product candidates and store-policy information. MCP tools connect the agent to supported backend operations. Current transactional information is retrieved from Laravel rather than trusted from generated text or potentially stale vector data.

### Streaming Chat

The browser sends chat messages through a Next.js Route Handler, which proxies the request to FastAPI. The AI service streams `token`, `done`, and `error` events using SSE.

### Authentication and Authorization

The frontend uses Zustand to manage its client-side authentication state. Laravel Sanctum authenticates protected API requests, while backend middleware enforces customer and administrator access. Frontend route guards are navigation controls and do not replace backend authorization.

---

## Technology Stack

| Area | Technology |
|---|---|
| Frontend framework | Next.js 16.4.0 |
| UI library | React 19.3.0 |
| Frontend language | TypeScript 5.9.3 |
| Styling | Tailwind CSS 4.3.3 |
| Client state | Zustand 5.0.15 |
| Backend framework | Laravel 12.62.0 |
| Backend language | PHP `^8.2` (Docker image uses PHP 8.3) |
| Database | PostgreSQL 16 in the supplied backend Docker configuration |
| API authentication | Laravel Sanctum 4.3.2 |
| Payment | Midtrans PHP SDK 2.6.2 |
| Image processing | Intervention Image 3.11.8 |
| Shipping / region integration | RajaOngkir (region lookup in the reviewed backend service) |
| AI API | Python, FastAPI |
| Agent orchestration | LangChain `create_agent`, LangGraph |
| LLM | Google Gemini (`gemini-3.1-flash-lite` default in the supplied AI snapshot) |
| Tool integration | Model Context Protocol (MCP) |
| Vector store | Chroma |
| Embeddings | Ollama Embeddings |
| Automation | n8n 2.35.7 in the supplied Compose configuration |
| Containerization | Docker / Docker Compose |
| Reverse proxy | Nginx |
| Frontend deployment | Vercel (as described by the project configuration) |
| CI/CD | GitHub Actions |

Versions above come from the supplied repository snapshots and lock/configuration files. They should be checked against the current deployed repositories before being treated as the latest production versions.

## Repository Guide

Zivo is maintained across four application repositories:

| Repository | Description |
|---|---|
| [Zivo Frontend](https://github.com/DefanZp/Zivo-frontEnd-next) | Next.js storefront, customer pages, administrator interface, and chat widget. |
| [Zivo Backend](https://github.com/DefanZp/Zivo-backEnd) | Laravel REST API and core e-commerce business logic. |
| [Zivo AI Service](https://github.com/DefanZp/Zivo-ai) | FastAPI service, LangGraph agent, RAG, MCP integration, and streaming. |
| [Zivo n8n](https://github.com/DefanZp/Zivo-n8n) | n8n Docker deployment and GitHub Actions deployment workflow. |

## Getting Started

This repository is an overview and documentation hub; it does not contain the application source code.

To run Zivo locally:

1. Clone the repositories you want to work with using the links in the [Repository Guide](#repository-guide).
2. Follow the installation and environment configuration instructions in that repository's README.
3. Configure service URLs and credentials through local environment files. Do not commit `.env` files, API keys, access tokens, or private credentials.
4. Start the required services. The frontend requires the Laravel API; AI chat additionally requires the FastAPI service and its configured dependencies. Automation workflows require a running n8n instance and their external integrations.

## Known Limitations and Verification Notes

The following items were identified during reviews of the supplied repository snapshots. They are source-level findings, not necessarily current production incidents; verify them against the latest code before making changes or publishing claims about runtime behavior.

- The frontend admin order list offers `shipped`, while the backend order state validation currently documents `pending`, `processing`, `completed`, and `cancelled`.
- The backend payment `cancel` / `deny` path marks payment as failed but does not currently follow the same order cancellation and stock-restoration path as an expired payment.
- The AI `increase_quantity` and `decrease_quantity` wrappers were found without `await` on their MCP calls in the supplied snapshot; their schemas also need to be aligned with the selected-product state flow.
- The AI API snapshot logs full tool results in one chat path; cart-related output should be redacted from logs.
- Guest LangGraph checkpoints are not covered by the existing authenticated-thread activity cleanup mechanism in the supplied AI snapshot.
- The frontend `next/image` allowlist in the supplied snapshot only includes the local Laravel storage host; production image host configuration needs confirmation.
- The n8n repository snapshot does not contain exported workflow JSON files. The existence and active state of the workflows should be confirmed in the n8n instance.
- Full frontend build, backend domain/integration tests, and AI end-to-end tests were not established by the supplied source audits.

## Security Notes

- Do not publish administrator passwords, API keys, bearer tokens, webhook secrets, or production environment values in this repository.
- Use dedicated demo accounts with limited permissions if public demo access is provided.
- Laravel must remain responsible for authorization and validation of protected business operations.
- Treat retrieved documents and model/tool output as untrusted data.
- Avoid logging credentials, full cart contents, or other unnecessary personal data.

## Future Improvements

Potential areas for continued development include:

- Expand automated tests across frontend, backend, and AI service boundaries.
- Reconcile order and payment lifecycle behavior across services.
- Improve AI tool reliability, logging redaction, and guest checkpoint retention.
- Version and maintain n8n workflow exports alongside deployment configuration.
- Improve AI observability and evaluation.
- Continue refining responsive customer and administrator experiences.

## Author

**Defan Zain Pratama**  
Full-Stack Developer interested in backend systems and AI-powered applications.

- GitHub: [@DefanZp](https://github.com/DefanZp)
