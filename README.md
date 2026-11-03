# 🛒 Zivo — AI-Powered E-Commerce Platform

A full-stack e-commerce platform built with **Next.js, Laravel 12, PostgreSQL, and Python-based AI services**.

Zivo combines a modern e-commerce experience with an AI Shopping Agent that helps customers discover products through natural language, explore product recommendations, compare options, and interact with their shopping cart.

The project focuses on practical full-stack engineering, including authentication, authorization, transactional order processing, payment integration, shipping calculations, workflow automation, and AI agent orchestration.

---

## 🚀 Live Demo

| Service | Link |
|---|---|
| 🌐 Frontend | [Zivo Live](https://zivo-front-end-next-5uvw.vercel.app/) |
| ⚙️ Backend API | [Zivo API](https://api.zivoapp.cloud) |
| 💻 Frontend Repository | [Zivo Frontend](https://github.com/DefanZp/Zivo-frontEnd-next) |
| 🔧 Backend Repository | [Zivo Backend](https://github.com/DefanZp/Zivo-backEnd) |
| ⚡ Automation Repository | [Zivo n8n](https://github.com/DefanZp/Zivo-n8n) |
| 🤖 AI Service Repository | [Zivo Ai](https://github.com/DefanZp/Zivo-ai) |


## 🔐 Demo Accounts

Use the following accounts to explore the application.

### Customer

| Field | Credential |
|---|---|
| Email | `demo.user@zivo.com` |
| Password | `ZivoUser123!` |

### Administrator

| Field | Credential |
|---|---|
| Email | `demo.admin@zivo.com` |
| Password | `ZivoAdmin123!` |

---

# 📖 About the Project

Zivo started as a full-stack development project and gradually evolved into a more complete e-commerce application.

The platform consists of three main parts:

1. **E-Commerce Platform** — product discovery, shopping cart, checkout, payment, shipping, customer accounts, and administration.
2. **AI Shopping Agent** — natural language product discovery, RAG-based retrieval, product recommendations, and tool-based shopping cart interactions.
3. **Automation System** — event-driven workflows for order notifications, payment notifications, and low-stock monitoring.

The frontend is built with Next.js and React, while Laravel 12 handles the core e-commerce business logic and REST APIs.

The AI capabilities are implemented as a separate Python service using FastAPI, LangChain, LangGraph, MCP, and Google Gemini.

The application uses PostgreSQL for persistent data and follows a Service Layer architecture to keep business logic separated from HTTP controllers.

---

# ⭐ Project Highlights

- Full-stack e-commerce architecture using Next.js and Laravel 12.
- AI Shopping Assistant powered by LangGraph and Google Gemini.
- RAG-based product discovery using Chroma and Ollama Embeddings.
- Model Context Protocol (MCP) integration for external tools.
- Streaming AI responses using Server-Sent Events (SSE).
- Zustand-based authentication state management.
- Laravel Sanctum authentication and authorization.
- PostgreSQL with transactional order processing.
- Midtrans payment integration with webhook verification.
- RajaOngkir shipping integration.
- Product image optimization using WebP and automatic resizing.
- n8n-based event-driven automation.
- AI-assisted email notifications and reporting.
- Dockerized backend and AI service infrastructure.
- VPS deployment with Nginx and HTTPS.
- GitHub Actions-based deployment workflow.

---

# ✨ Features

## 👤 Customer Features

### Authentication & Account

- Register and login using Laravel Sanctum.
- Email verification flow.
- Protected routes for authenticated users.
- Verified-user route protection.
- Persistent authentication state using Zustand.
- User profile management.
- Manage multiple delivery addresses.
- Set a default delivery address.

### Product Discovery

- Browse products.
- Product pagination.
- Search products by name.
- Filter products by category.
- Sort products by newest, price, or name.
- View product details.

### Shopping Cart & Checkout

- Add products to the shopping cart.
- Increase and decrease item quantities.
- Remove individual cart items.
- Clear the shopping cart.
- Select a delivery address.
- Create orders from cart items.
- Validate product stock and pricing on the backend.

### Orders & Payments

- View order history with pagination.
- View order details.
- View shipping information stored with an order.
- Track order status through an order timeline.
- Complete payments using Midtrans Snap.
- View payment results.
- Reopen unpaid payments through the Pay Now flow.

## 🛠 Administrator Features

### Dashboard

- Sales statistics.
- Revenue summary from completed orders.
- Low-stock product monitoring.

### Product Management

- View products.
- Create products.
- Edit product information.
- Delete products.
- Upload product images.
- Automatic image resizing and WebP conversion.

### Order Management

- View customer orders.
- View order details.
- View payment information.
- Update order status according to business rules.
- Update payment status through the appropriate backend workflow.
- Monitor order processing.

### Access Control

- Protected administrator routes.
- Admin-specific route guard.
- Backend authorization for administrative operations.

---

# 🤖 AI Shopping Assistant

Zivo includes an AI Shopping Assistant that allows customers to interact with the product catalog using natural language.

Instead of relying only on conventional keyword searches, customers can describe what they need and receive product suggestions based on their requirements.

## AI Features

### 1. Natural Language Product Discovery

Customers can describe product requirements in ordinary language.

Example:

```text
Find a breathable shirt for warm weather.
```

The agent can identify relevant requirements such as material, intended use, and product characteristics.

### 2. RAG-Based Product Retrieval

Zivo uses Retrieval-Augmented Generation (RAG) to retrieve relevant product candidates from its product knowledge base.

The retrieval workflow:

1. Product information is retrieved from the Laravel API.
2. Each product is converted into a document.
3. Ollama Embeddings generates vector representations.
4. Product vectors are stored in Chroma.
5. The agent performs similarity search based on the user's request.
6. Retrieved candidates are evaluated before further product lookups.

The current product retrieval implementation uses a similarity search with a maximum of three candidates.

### 3. Product Details Verification

RAG is used to discover relevant candidates, not as the source of truth for live transactional information.

The agent retrieves current product details through Laravel API tools before presenting information such as:

- Product price.
- Product stock.
- Product details.

This separation helps prevent outdated vector-store information from being treated as current transactional data.

### 4. Product Comparison & Recommendations

The agent can evaluate product candidates based on customer requirements, including:

- Material.
- Breathability.
- Intended use.
- Care instructions.
- Product characteristics.
- Price and availability.

Recommendations are based on the retrieved product information and verified product details.

### 5. AI Shopping Cart Operations

The agent can interact with shopping cart functionality through MCP tools.

Supported operations include:

- Retrieve the current cart.
- Add a product to the cart.
- Increase item quantity.
- Decrease item quantity.
- Remove cart items.
- Clear the cart.

Cart operations require authentication and are subject to Laravel backend authorization.

Guest users can still use public product discovery features without logging in.

### 6. Streaming Responses

The chatbot supports streamed responses using Server-Sent Events (SSE).

Instead of waiting for the complete response before displaying it, the frontend receives and renders text chunks as they arrive.

This provides a more interactive chatbot experience.

---

# 🧠 AI Architecture

The AI Shopping Assistant runs as a separate service from the main e-commerce backend.

## Technology Stack

| Component | Technology |
|---|---|
| AI Service | Python, FastAPI |
| Agent Framework | LangChain, LangGraph |
| LLM Provider | Google Gemini |
| Tool Integration | Model Context Protocol (MCP) |
| Vector Database | Chroma |
| Embedding Model | Ollama Embeddings |
| E-Commerce Integration | Laravel REST API |
| Agent Checkpointing | LangGraph + PostgreSQL |

## AI Request Flow

```text
Next.js Chat Widget
        |
        v
Next.js Route Handler
        |
        v
FastAPI AI Service
        |
        v
LangGraph Agent
        |
        v
MCP Client
        |
        v
MCP Server
     /      \
    v        v
 Chroma   Laravel API
    |        |
    +----+---+
         |
         v
    Tool Results
         |
         v
    LangGraph Agent
         |
         v
  Streaming Response
         |
         v
  Next.js Chat Widget
```

## Agent Architecture

The AI agent uses a combination of:

- **RAG Agent:** retrieves relevant product and store-policy information.
- **ReAct-style tool execution:** allows the agent to reason about actions, invoke tools, and evaluate tool results.
- **Agent State:** maintains information required during a conversation.
- **Agent Context:** provides runtime information needed by tools.
- **Checkpointing:** persists conversation state using PostgreSQL.

## MCP Integration

The MCP Server exposes tools for product discovery, product information, knowledge retrieval, and shopping cart operations.

### MCP Tools

| Tool | Purpose |
|---|---|
| `search_products` | Search products through Laravel API |
| `get_product` | Retrieve current product details |
| `search_product_knowledge` | Search product knowledge in Chroma |
| `search_policy_knowledge` | Retrieve store policy information |
| `get_cart` | Retrieve the customer's cart |
| `add_to_cart` | Add an item to the cart |
| `increase_quantity` | Increase cart item quantity |
| `decrease_quantity` | Decrease cart item quantity |
| `remove_from_cart` | Remove an item from the cart |
| `clear_cart` | Remove all cart items |

The agent also maintains local state-management tools for selecting and clearing the current product.

### MCP Communication

The current local development architecture uses:

- MCP Host: FastAPI and LangGraph AI service.
- MCP Client: `MCPAdapter`.
- MCP Server: `mcp_server.py`.
- Transport: STDIO.

At startup, the application connects to the MCP Server, discovers the available tools, and provides them to the agent.

## AI Authentication Flow

For authenticated shopping cart operations:

1. Next.js obtains the current authentication token from the auth store.
2. The token is forwarded through the chat request.
3. FastAPI verifies the user through the Laravel API.
4. The authenticated token is provided to the agent runtime context.
5. Tool wrappers pass the required authentication context to MCP tools.
6. Laravel validates authentication and ownership before performing cart operations.

The AI service does not treat model-generated information as proof of authorization. Laravel remains responsible for validating access to protected business operations.

## RAG Knowledge Collections

The AI service uses Chroma collections for different knowledge domains:

| Collection | Purpose |
|---|---|
| `zivo_product_catalog` | Product attribute retrieval |
| `zivo_returns_policy` | Store return-policy retrieval |

Product knowledge retrieval is used for candidate discovery, while Laravel provides the current product details.

The product catalog vector store currently requires re-ingestion when catalog information changes.

---

# 🔄 Automation Workflows

Zivo uses n8n as a separate automation layer to connect Laravel events with AI-assisted notifications.

## 1. Order Created → Admin Notification

When a customer creates an order, Laravel sends an event to an n8n webhook.

```text
Laravel
   |
   v
n8n Webhook
   |
   v
Ollama
   |
   v
Email to Admin
```

The AI model generates the notification wording, while order information comes from Laravel.

## 2. Payment Settled → Customer Notification

When a payment is successfully settled, Laravel processes the Midtrans notification and triggers an n8n workflow.

```text
Midtrans
   |
   v
Laravel
   |
   v
n8n Webhook
   |
   v
Ollama
   |
   v
Email to Customer
```

The AI model is responsible for generating message wording. Critical transaction information remains controlled by the backend.

## 3. Daily Low Stock AI Summary

A scheduled n8n workflow checks the Laravel API every morning for products below the configured low-stock threshold.

```text
Schedule Trigger
       |
       v
Laravel Low Stock API
       |
       v
Condition Check
       |
       v
Ollama
       |
       v
Email Summary
```

The workflow generates an AI-assisted summary of products that require attention.

## Automation AI Model

The existing n8n notification workflows use Ollama with the `qwen2.5:1.5b` model.

---

# 📦 Order & Checkout

Checkout is handled by Laravel to ensure that important business rules are enforced on the backend rather than trusted to the frontend.

When an order is created, the backend:

1. Validates that the selected delivery address belongs to the authenticated user.
2. Locks the related products before checking stock.
3. Checks product availability.
4. Calculates the order total using the current product price.
5. Creates the order and order items.
6. Reduces product stock.
7. Creates the related payment record.
8. Commits the operation inside a database transaction.

This helps maintain data consistency when an error occurs during order creation.

## Address Snapshot

Zivo uses an address snapshot when an order is created.

```text
Customer Address
       |
       v
    Checkout
       |
       v
Order Address Snapshot
```

The shipping information is copied into the order at checkout.

Changing a customer's address later does not change the shipping information stored in an existing order.

## Stock Management

Stock is handled as part of the order business logic.

```text
Current Stock
     -
Order Quantity
     =
New Stock
```

Products are locked using `lockForUpdate()` while the order is being processed.

This helps prevent inconsistent stock when multiple requests attempt to purchase the same product concurrently.

When an order is cancelled, stock is restored through the cancellation flow.

Stock restoration is handled in one place to avoid restoring the same stock multiple times.

---

# 💳 Payment Integration

Zivo uses Midtrans as its payment gateway and integrates payment status with the order lifecycle.

## Payment Flow

```text
Checkout
   |
   v
Create Order
   |
   v
Create Payment
   |
   v
Clear Cart
   |
   v
Midtrans Snap
   |
   v
Customer Payment
   |
   v
Midtrans Webhook
   |
   v
Update Payment
   |
   v
Update Order
```

Customers can open the Midtrans Snap payment page directly from checkout or later from the order detail page using Pay Now.

## Supported Payment States

Zivo maps Midtrans transaction statuses to its own payment states:

| Midtrans Status | Zivo Payment State |
|---|---|
| `pending` | `unpaid` |
| `settlement` | `paid` |
| `capture` | `paid` |
| `expire` | `expired` |
| `cancel` | `failed` |
| `deny` | `failed` |

## State Transition Rules

When a payment becomes paid:

```text
Payment (paid) → Order (processing)
```

When an unpaid payment expires:

```text
Payment (expired)
       |
       v
Order (cancelled)
       |
       v
Stock Restored
```

## Payment Security & Consistency

- Payment notifications are verified using Midtrans signatures.
- Payment notifications are processed through database transactions.
- Payment and order records are locked when handling sensitive status changes.
- Customers can only access their own payments.
- Completed or expired payments cannot be processed again.
- Existing Midtrans transactions are checked before creating a new Snap transaction to prevent duplicate payment transactions.
- Customers can reopen an unpaid payment from the Order Detail page using Pay Now.
- Payment status is controlled by the payment gateway and webhook flow, while order status follows Zivo's business rules.

---

# 📍 Shipping Integration

Zivo integrates RajaOngkir for shipping cost calculations.

Customers can:

- Manage delivery addresses.
- Select a delivery address during checkout.
- Calculate shipping costs.
- View shipping information associated with their orders.

Shipping information is stored as part of the order snapshot to preserve the original delivery details.

---

# 🔄 Order Status Rules

Order status is controlled by business rules instead of allowing arbitrary status changes.

Current transitions:

```text
pending
  |
  +----> processing
  |
  +----> cancelled

processing
  |
  +----> completed
  |
  +----> cancelled

completed
  |
  +----> cannot be changed

cancelled
  |
  +----> cannot be changed
```

`pending → processing` is only allowed when the related payment has been successfully paid.

This prevents invalid transitions such as:

```text
pending → completed
processing → pending
completed → cancelled
cancelled → processing
```

---

# 🖼️ Product Image Optimization

Zivo uses an optimized image pipeline for product images.

## Admin Upload

```text
Image Upload
     |
     v
Validation
     |
     v
Resize to maximum 1200px
     |
     v
Convert to WebP
     |
     v
Store in Product Storage
     |
     v
Save image_path
```

The database stores only the image path, while the `image_url` attribute is generated by the Product model accessor.

Example:

```text
image_path
    |
    v
products/product_xxxxx.webp

image_url
    |
    v
https://api.zivoapp.cloud/storage/products/product_xxxxx.webp
```

Product images on the frontend use lazy loading to reduce unnecessary network requests.

---

# 🧱 Architecture

## Frontend Architecture

The Next.js application uses the App Router and separates route definitions, feature-specific components, shared UI, services, and application state.

```text
Next.js App Router
        |
        v
Feature Components
        |
        v
Service Layer
        |
        v
API Client / Fetch
        |
        v
Laravel REST API
```

### Main Frontend Layers

| Layer | Responsibility |
|---|---|
| `app/` | App Router pages, layouts, and API route handlers |
| `components/` | Shared components and reusable UI |
| `features/` | Feature-specific UI and business interactions |
| `lib/services/` | API communication and service logic |
| `lib/stores/` | Application state management |
| `types/` | TypeScript data types and interfaces |

### State Management

Zivo uses Zustand for authentication state management.

The auth store uses Zustand's `persist` middleware to persist selected authentication information.

The frontend also includes:

- AuthGuard.
- AdminGuard.
- VerifiedGuard.

These guards control frontend navigation and visibility. They complement, but do not replace, backend authentication and authorization.

## Backend Architecture

The Laravel backend follows a Service Layer structure:

```text
Route
  |
  v
Controller
  |
  v
Service
  |
  v
Model
  |
  v
Database
```

Controllers handle HTTP requests and responses, while business logic is placed inside services.

Examples:

- `OrderService`
- `PaymentService`
- `ProductService`

This separation helps keep controllers focused and makes business logic easier to maintain and test.

## AI Service Architecture

The AI service uses FastAPI for HTTP endpoints and LangGraph for agent execution.

```text
FastAPI
   |
   v
ZivoAgentService
   |
   v
LangGraph Agent
   |
   v
MCP Client
   |
   v
MCP Server
   |
   +------ Laravel API
   |
   +------ Chroma
```

The AI service also provides streaming and non-streaming chat endpoints.

The Next.js application exposes corresponding route handlers that proxy requests to the AI service.

## Authentication Architecture

```text
Next.js Application
        |
        v
Zustand Auth Store
        |
        v
Laravel Sanctum
        |
        v
Backend Authentication
        |
        v
User / Admin Authorization
```

Laravel remains the source of truth for user identity and protected operations.

## Automation & Integration

```text
Laravel
   |
   +---- Midtrans
   |        |
   |        v
   |    Payment Webhook
   |
   +---- n8n
            |
            v
          Ollama
            |
            v
           Email
```

## Production Infrastructure

The existing production infrastructure uses Docker and a VPS-based deployment for backend services.

```text
Cloudflare
    |
    v
VPS
    |
    +---- Nginx
    |
    +---- Laravel
    |
    +---- Queue
    |
    +---- PostgreSQL
    |
    +---- n8n
```

The frontend is deployed separately through Vercel.


---

# 🗂️ Main Frontend Routes

The frontend uses Next.js App Router with route groups for customer-facing and administrator pages.

## Public & Customer Routes

| Route | Description |
|---|---|
| `/` | Homepage |
| `/products` | Product listing |
| `/product/[id]` | Product detail |
| `/cart` | Shopping cart |
| `/checkout` | Checkout |
| `/payment/result` | Payment result |
| `/user/settings` | User profile settings |
| `/user/settings/addresses` | Address management |
| `/user/orders` | Order history |
| `/user/orders/[id]` | Order detail |

## Authentication Routes

| Route | Description |
|---|---|
| `/auth/login` | User login |
| `/auth/register` | User registration |
| `/auth/verify-email` | Email verification |
| `/auth/email-verified` | Email verification result |

## Administrator Routes

| Route | Description |
|---|---|
| `/admin/dashboard` | Administrator dashboard |
| `/admin/products` | Product management |
| `/admin/products/create` | Create product |
| `/admin/products/[id]/edit` | Edit product |
| `/admin/orders` | Order management |
| `/admin/orders/[id]` | Order details |

## Chat API Routes

| Endpoint | Method | Description |
|---|---|---|
| `/api/chat` | POST | Non-streaming AI chat |
| `/api/chat/stream` | POST | Streaming AI chat using SSE |

The chat endpoints are Next.js Route Handlers that forward requests to the FastAPI AI service. They are not separate frontend pages.

---

# 🧩 Frontend Project Structure

The following is a simplified view of the Next.js frontend structure.

```text
Zivo-frontEnd-next/
├── app/
│   ├── (admin)/
│   │   └── admin/
│   │       ├── dashboard/
│   │       ├── orders/
│   │       └── products/
│   ├── (shop)/
│   │   ├── cart/
│   │   ├── checkout/
│   │   ├── payment/
│   │   ├── product/
│   │   ├── products/
│   │   └── user/
│   ├── api/
│   │   └── chat/
│   │       ├── route.ts
│   │       └── stream/
│   │           └── route.ts
│   └── auth/
│       ├── email-verified/
│       ├── login/
│       ├── register/
│       └── verify-email/
├── components/
│   ├── admin/
│   ├── auth/
│   ├── chatbot/
│   ├── shared/
│   └── ui/
├── features/
│   ├── admin/
│   ├── auth/
│   ├── cart/
│   ├── checkout/
│   ├── home/
│   ├── orders/
│   ├── payment/
│   ├── products/
│   └── user-settings/
├── lib/
│   ├── api/
│   ├── services/
│   └── stores/
└── types/
```

This structure separates application routing, feature components, reusable UI, API services, and shared types.

---

# 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend Framework | Next.js |
| UI Library | React |
| Frontend Language | TypeScript |
| Styling | Tailwind CSS |
| State Management | Zustand |
| Routing | Next.js App Router |
| Backend Framework | Laravel 12 |
| Backend Language | PHP 8.2+ |
| Database | PostgreSQL 17 |
| ORM | Eloquent ORM |
| Authentication | Laravel Sanctum |
| API Architecture | REST API |
| AI Service | Python, FastAPI |
| Agent Framework | LangChain, LangGraph |
| AI Protocol | Model Context Protocol (MCP) |
| LLM | Google Gemini |
| Vector Database | Chroma |
| Embeddings | Ollama |
| Automation | n8n |
| Notification AI | Ollama |
| Payment Gateway | Midtrans |
| Shipping Integration | RajaOngkir |
| Image Processing | Intervention Image |
| Containerization | Docker / Docker Compose |
| Reverse Proxy | Nginx |
| Frontend Deployment | Vercel |
| Backend Infrastructure | VPS |
| CI/CD | GitHub Actions |


---

# 🧠 Technical Decisions

## Service Layer

Business logic is separated from controllers using the Service Layer pattern.

This is especially useful for:

- Checkout.
- Order creation.
- Stock management.
- Payment processing.
- Order cancellation.
- Product management.

## PostgreSQL

PostgreSQL is used as the main relational database.

It stores structured application data such as:

- Users.
- Products.
- Categories.
- Carts.
- Orders.
- Order items.
- Payments.
- Addresses.

## Database Transactions

Transactions are used when multiple database changes need to succeed or fail together.

For example:

```text
Create Order
     +
Create Order Items
     +
Reduce Stock
     +
Create Payment
```

Using a database transaction helps keep related operations consistent.

## Concurrency Handling

`lockForUpdate()` is used when processing products and updating sensitive order and payment data.

This helps prevent inconsistent results when multiple requests happen concurrently.

## AI Source of Truth

The AI Shopping Agent uses RAG to retrieve relevant product candidates.

However, live business information such as product prices, stock, and cart state comes from the Laravel backend.

This keeps transactional information separate from generated model responses.

## MCP Integration

MCP provides a standard communication protocol between the AI application and external tools.

The agent runtime manages its state and context, while the MCP Server exposes specific capabilities.

This separation helps keep tool implementations independent from the agent's internal orchestration logic.

## Automation with n8n

n8n is used as a separate automation layer instead of putting notification logic directly inside frontend components.

This keeps event-driven workflows separate from the main application logic.

## Authentication State Management

Zustand manages frontend authentication state and persistence.

Laravel Sanctum remains responsible for validating authenticated API requests.

Frontend route guards are used for navigation and user experience, not as the only security mechanism.

---

# 🔒 Security Considerations

Zivo implements several security-related mechanisms across its application layers.

## Authentication & Authorization

- Laravel Sanctum protects authenticated API endpoints.
- Administrator operations require appropriate backend authorization.
- Customers can access only their own protected resources.
- Frontend guards control access to protected pages.
- Verified-user routes are protected separately.

## Payment Security

- Midtrans webhook signatures are verified.
- Payment state changes are handled through backend business rules.
- Database transactions and row locking are used for sensitive operations.
- Duplicate payment processing is prevented through transaction checks.

## AI Tool Security

- Cart operations require authenticated access.
- Laravel validates user access and cart ownership.
- Tool arguments must be validated before execution.
- Authentication credentials should remain outside model-generated content.
- Retrieved documents are treated as external data, not trusted developer instructions.

## Data Privacy

- API keys and credentials must be stored in environment variables.
- Authorization headers and bearer tokens should not be written to application logs.
- User-specific information must be protected by appropriate access controls.
- AI context should include only the information required for the task.

These mechanisms should be complemented with regular integration tests, security testing, and monitoring.

---

# ⚙️ Installation & Local Development

Zivo consists of separate services. The frontend requires the Laravel backend to be available, while AI chat functionality additionally requires the FastAPI AI service.

## Prerequisites

- Node.js and npm.
- PHP 8.2 or later.
- Composer.
- PostgreSQL.
- Python.
- Docker / Docker Compose (for the services that use it).
- Ollama (for local embedding and automation workflows, where required).

## 1. Clone the Frontend Repository

```bash
git clone https://github.com/DefanZp/Zivo-frontEnd-next
cd Zivo-frontEnd-next
```

Install dependencies:

```bash
npm install
```

## 2. Configure Frontend Environment Variables

Create `.env.local` in the frontend root directory.

```env
# Laravel API base URL
NEXT_PUBLIC_API_URL=<YOUR_LARAVEL_API_BASE_URL>

# FastAPI AI service URL
AI_SERVICE_URL=<YOUR_FASTAPI_AI_SERVICE_URL>
```

`AI_SERVICE_URL` is used by the Next.js chat Route Handlers.

Make sure the Laravel API environment variable matches the actual variable name used in `lib/api/client.ts`.

Do not commit `.env.local` or real API credentials to GitHub.

## 3. Run the Frontend

Start the Next.js development server:

```bash
npm run dev
```

Open:

```text
http://localhost:3000
```

For production builds:

```bash
npm run build
npm run start
```

For available development, build, and other npm scripts, see the [`package.json`](https://github.com/DefanZp/Zivo-frontEnd-next/blob/master/package.json) in the frontend repository.

## 4. Set Up the Laravel Backend

Clone the backend repository:

```bash
git clone https://github.com/DefanZp/Zivo-backEnd
cd Zivo-backEnd
```

Install PHP dependencies:

```bash
composer install
```

Prepare the environment file:

```bash
cp .env.example .env
```

Generate the application key:

```bash
php artisan key:generate
```

Configure PostgreSQL, Sanctum, Midtrans, RajaOngkir, storage, and other required environment variables according to the backend README.

Run migrations:

```bash
php artisan migrate
```

Create the storage symlink when required:

```bash
php artisan storage:link
```

Run the backend development server:

```bash
php artisan serve
```

For detailed instructions on database seeding and additional service configuration, please refer to the [Backend Repository README](https://github.com/DefanZp/Zivo-backEnd#readme).

## 5. Set Up the AI Service

The AI Shopping Assistant runs as a separate Python service.

Follow the AI service repository README for its Python environment, dependency installation, and configuration.

The current architecture requires configuration for:

- `GEMINI_API_KEY`
- `GEMINI_MODEL`
- `BACKEND_URL`
- `LANGGRAPH_DB_URI`
- Ollama configuration for local embeddings.

The MCP Server is currently started by the FastAPI AI service through the local STDIO transport.

For AI chat functionality, ensure the FastAPI service and its dependencies are running and that `AI_SERVICE_URL` points to the correct service address.

## 6. External Services

Depending on the functionality being tested, configure the required external services:

| Service | Purpose |
|---|---|
| Midtrans | Payment processing |
| RajaOngkir | Shipping cost calculation |
| n8n | Automated workflows |
| Ollama | Local embeddings and notification generation |
| Google Gemini | AI Shopping Agent |
| Chroma | Product and policy retrieval |

Some functionality will remain unavailable until its external services are configured.

---

# 📂 Source Code Repositories

| Repository | Responsibility |
|---|---|
| Next.js Frontend (https://github.com/DefanZp/Zivo-frontEnd-next) | Customer interface, administrator interface, and chat widget |
| [Laravel Backend](https://github.com/DefanZp/Zivo-backEnd) | E-commerce API and business logic |
| [n8n Automation](https://github.com/DefanZp/Zivo-n8n) | Event-driven automation workflows |
| AI Service (https://github.com/DefanZp/Zivo-ai) | FastAPI, LangGraph agent, MCP integration, and RAG |


---

# 🚧 Future Improvements

Possible improvements for future development include:

- Wishlist functionality.
- Expanded automated testing coverage.
- More comprehensive API and AI agent evaluation.
- Improved AI observability and tracing.
- Structured LLM cost tracking.
- Long-term user preference memory.
- Improved product catalog synchronization for RAG.
- More comprehensive security and red-team testing.
- Further improvements to responsive and mobile UX.
- Expanded deployment automation.
- Remote MCP Server deployment.

---

# 👨‍💻 Author

**Defan Zain Pratama**

Full-Stack Developer interested in building web applications, backend systems, and AI-powered applications.

GitHub: [@DefanZp](https://github.com/DefanZp)

---
