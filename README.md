# NexShop — B204 E-Commerce App

A full-stack e-commerce web application built with **Node.js**, **Express**, **MongoDB**, and **PayPal Sandbox** payments.

---

## Project Structure

```
nexshop-ecommerce/
├── backend/
│   ├── config/
│   │   └── db.js                  # MongoDB connection
│   ├── controllers/
│   │   ├── orderController.js     # Order CRUD logic
│   │   ├── paymentController.js   # PayPal sandbox integration
│   │   └── productController.js   # Product CRUD logic
│   ├── middleware/
│   │   └── errorHandler.js        # Global error handler
│   ├── models/
│   │   ├── Order.js               # Mongoose Order schema
│   │   └── Product.js             # Mongoose Product schema
│   ├── routes/
│   │   ├── orderRoutes.js
│   │   ├── paymentRoutes.js
│   │   └── productRoutes.js
│   ├── seed.js                    # Sample data seeder
│   └── server.js                  # Express entry point
├── frontend/
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   ├── api.js                 # Fetch wrapper for all API calls
│   │   ├── add-product.js
│   │   ├── main.js                # Shared utilities
│   │   ├── orders.js
│   │   ├── product-detail.js      # PayPal checkout logic
│   │   └── products.js
│   ├── add-product.html
│   ├── index.html                 # Landing page
│   ├── orders.html
│   ├── product-detail.html
│   └── products.html
├── .env.example
├── .gitignore
├── docker-compose.yml
├── Dockerfile
└── package.json
```

---

## API Endpoints

| Method | Endpoint                        | Description                        |
|--------|---------------------------------|------------------------------------|
| GET    | `/api/products`                 | List all products                  |
| GET    | `/api/products/search?name=q`   | Search products by name            |
| GET    | `/api/products/:id`             | Get single product                 |
| POST   | `/api/products`                 | Create a new product               |
| PUT    | `/api/products/:id`             | Update a product                   |
| DELETE | `/api/products/:id`             | Delete a product                   |
| GET    | `/api/orders`                   | List all orders                    |
| GET    | `/api/orders/:id`               | Get single order                   |
| GET    | `/api/payments/config`          | Get PayPal client ID               |
| POST   | `/api/payments/create-order`    | Create PayPal checkout order       |
| POST   | `/api/payments/capture-order`   | Capture approved PayPal payment    |

---

## Setup Instructions

### Prerequisites

Make sure the following are installed on your machine:

- [Node.js 20+](https://nodejs.org/) (for local dev)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Git](https://git-scm.com/)

---

### Option A — Run with Docker Desktop (Recommended)

This runs the full stack (Node.js app + MongoDB) in isolated containers with a single command.

#### Step 1 — Clone / open the project in VSCode

Open VSCode, then open the `nexshop-ecommerce` folder:
```
File → Open Folder → select nexshop-ecommerce
```

#### Step 2 — Create your `.env` file

In the VSCode terminal (`Ctrl + `` ` ``):
```bash
cp .env.example .env
```

Then open `.env` and fill in your PayPal sandbox credentials:
```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://mongo:27017/nexshop
PAYPAL_CLIENT_ID=your_paypal_sandbox_client_id_here
PAYPAL_CLIENT_SECRET=your_paypal_sandbox_client_secret_here
```

> **How to get PayPal sandbox credentials:**
> 1. Go to https://developer.paypal.com/dashboard/
> 2. Sign in (or create a free account)
> 3. Go to **Apps & Credentials** → **Sandbox** tab
> 4. Click **Create App**, give it a name (e.g. "NexShop")
> 5. Copy the **Client ID** and **Secret** into your `.env`

#### Step 3 — Start Docker Desktop

Launch Docker Desktop and wait until it shows **"Engine running"** in the bottom-left corner.

#### Step 4 — Build and start the containers

In the VSCode terminal:
```bash
docker-compose up --build
```

You will see logs from both services. Wait for:
```
✅  NexShop server running on http://localhost:5000
✅  MongoDB connected: mongo
```

#### Step 5 — Seed the database with sample products

Open a **new terminal tab** in VSCode (`+` icon in terminal panel):
```bash
docker exec nexshop_app node backend/seed.js
```

Expected output:
```
✅  Connected to MongoDB
🗑️   Cleared existing products
✅  Seeded 10 products
🔌  Disconnected from MongoDB
```

#### Step 6 — Open the app

Visit: **http://localhost:5000**

---

### Option B — Run Locally (without Docker)

Use this if you have MongoDB installed locally or prefer running without containers.

#### Step 1 — Install dependencies
```bash
npm install
```

#### Step 2 — Create `.env` file
```bash
cp .env.example .env
```

Edit `.env` and set:
```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/nexshop
PAYPAL_CLIENT_ID=your_paypal_sandbox_client_id_here
PAYPAL_CLIENT_SECRET=your_paypal_sandbox_client_secret_here
```

#### Step 3 — Start MongoDB locally

If you have MongoDB installed:
```bash
mongod
```

Or use [MongoDB Atlas](https://www.mongodb.com/atlas) free tier and paste the connection string into `MONGO_URI`.

#### Step 4 — Start the server (development mode with auto-reload)
```bash
npm run dev
```

#### Step 5 — Seed sample data
```bash
node backend/seed.js
```

#### Step 6 — Open the app

Visit: **http://localhost:5000**

---

### Stopping the app

**Docker:**
```bash
docker-compose down
```

To also delete the MongoDB data volume:
```bash
docker-compose down -v
```

**Local:**
Press `Ctrl + C` in the terminal.

---

## Recommended VSCode Extensions

Install these for the best development experience:

| Extension                  | Publisher       | Purpose                    |
|----------------------------|-----------------|----------------------------|
| ESLint                     | Microsoft       | JavaScript linting         |
| Prettier                   | Prettier        | Code formatting            |
| MongoDB for VS Code        | MongoDB         | Browse your DB in VSCode   |
| Docker                     | Microsoft       | Manage containers in UI    |
| REST Client                | Huachao Mao     | Test API endpoints         |
| Thunder Client             | Thunder Client  | API testing (Postman-like) |

---

## Testing the PayPal Sandbox Payment

1. Open **http://localhost:5000/products.html**
2. Click any product card
3. Set the quantity and scroll to the PayPal buttons
4. Click **Pay with PayPal**
5. Log in with your **PayPal sandbox buyer account**
   - Find sandbox accounts at: https://developer.paypal.com/dashboard/accounts/sandbox
6. Approve the payment
7. A success modal will appear with the Order ID
8. Visit **http://localhost:5000/orders.html** to see the recorded transaction

---

## Troubleshooting

| Problem | Solution |
|---|---|
| `Cannot connect to MongoDB` | Wait 10 seconds — the app retries automatically every 5s |
| `PayPal buttons not loading` | Check `PAYPAL_CLIENT_ID` in `.env` is correct and not the live key |
| `Port 5000 already in use` | Change `PORT` in `.env` and update `docker-compose.yml` port mapping |
| `docker-compose: command not found` | Update Docker Desktop — compose is built in on v2+ |
| Products page is empty | Run the seed script: `docker exec nexshop_app node backend/seed.js` |

