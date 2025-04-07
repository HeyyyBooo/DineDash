# 🍽️ DineDash – Microservices Based Food Ordering System

DineDash is a full-stack microservices-based food ordering and delivery system. This project includes:

- A **Node.js gateway** that handles routing and static frontend.
- Backend services built with **FastAPI**, each handling a different domain:
  - **auth** – Admin/Customer authentication
  - **restaurant** – Restaurant and menu management
  - **order** – Order placement and management
  - **delivery** – Delivery tracking
  - **sqlt** – Databse

All services are containerized with Docker and orchestrated using Docker Compose.

---

## 🗂️ Directory Structure
Below is an example directory structure for the project:

```
DINEDASH/
├── docker-compose.yml           # Docker Compose orchestration file
├── README.md                    # This file
├── gateway/                     # Node.js gateway project
│   ├── Dockerfile               # Dockerfile for gateway
│   ├── server.js                # Gateway server code
│   └── static/                  # static files
│       ├── index.html           # Home / Login page
│       ├── login.html           # Alternative login page (if needed)
│       ├── admin.html           # Admin dashboard page
│       ├── customer.html        # Customer dashboard page
│       ├── js/                  # Backend JavaScript folder
│       │   └── main.js          # Contains gatewaylogic of all services
│       └── css/
│           └── style.css        # CSS styles
├── sqlt/
│   ├── Dockerfile               # Dockerfile for sqlite container
│   ├── main.py                  #fastAPI file for sql
└── services/                    # Backend microservices
    ├── auth/                    # Authentication service
    │   ├── Dockerfile           # Dockerfile for auth service
    │   ├── app.py               # FastAPI application code for auth
    │   └── ...                  # Additional auth service files
    ├── restaurant/              # Restaurant service
    │   ├── Dockerfile           # Dockerfile for restaurant service
    │   ├── app.py               # FastAPI application code for restaurants
    │   └── ...                  # Additional restaurant service files
    ├── order/                   # Order service
    │   ├── Dockerfile           # Dockerfile for order service
    │   ├── app.py               # FastAPI application code for orders
    │   └── ...                  # Additional order service files
    └── delivery/                # Delivery service
        ├── Dockerfile           # Dockerfile for delivery service
        ├── app.py               # FastAPI application code for delivery
        └── ...                  # Additional delivery service files
```

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

Clone the repository and change to the project directory:

```bash
git clone https://github.com/HeyyyBooo/dinedash.git
cd dinedash
```

### 2️⃣ Build & Start All Containers

Use Docker Compose to build and run all services:

```bash
docker network create foodnet
docker-compose up --build
```

This command will:

- Build the Docker images for each microservice and the gateway.
- Create a shared Docker network (`foodnet`) for inter-container communication.
- Start all the services:
  - **Gateway** on port `8080`
  - **sqlite databse** on port `8000`
  - **Restaurant Service** on port `8001`
  - **Order Service** on port `8002`
  - **Delivery Service** on port `8003`
  - **Auth Service** on port `8004`

After the containers are up, visit [http://localhost:8080](http://localhost:8080) to access the gateway.

---

## 🔐 Authentication & Routing Methods

### API Endpoints

| Endpoint           | Method | Description                       |
|--------------------|--------|-----------------------------------|
| `/auth/admin`      | POST   | Authenticate admin credentials    |
| `/auth/customer`   | POST   | Authenticate customer credentials |
| `restaurants/restaurant`   | POST   | Add Restaurant |
| `restaurants/menu`   | POST   | Add Menu Item |
| `restaurants/menu/{item_id}`   | DELETE | Delete Menu Item |
| `restaurants/restaurants`   | GET   | Get All Restaurants |
| `restaurants/menu/{restaurant_id}`   | GET   | Get Menu Items |
| `orders/order`   | POST | Place Order |
| `orders/orders`   | GET | Get All Orders |
| `orders/order/{order_id}`   | GET | Get Order Details |
| `orders/order/{order_id}`   | DELETE | Cancel Order |
| `delivery/delivery`   | POST | Assign Delivery |
| `delivery/delivery/{order_id}`   | GET | Get Delivery Info|
| `delivery/delivery/{order_id}`   | PUT | Update Delivery Status|
| `delivery/delivery/{order_id}`   | DELETE | Cancel Delivery|


### Gateway Proxy Configuration

The Node.js gateway (`gateway/server.js`) routes requests to the appropriate backend containers based on the URL:

- `/api/restaurants` → `http://restaurant-container:8001`
- `/api/orders` → `http://order-container:8002`
- `/api/delivery` → `http://delivery-container:8003`
- `/auth` → `http://auth-container:8004`

---

## 🚧 Managing the Cluster

### Starting the Cluster

From the project root, run:

```bash
docker-compose up --build
```

### Stopping the Cluster

To stop all containers and remove the network:

```bash
docker-compose down
```

To also remove volumes (if any):

```bash
docker-compose down -v
```

---

## 📌 Additional Information

- **Gateway:**  
  The Node.js gateway serves static files (HTML, CSS, JS) from the `gateway/static` folder and proxies API calls to the backend services using container names.

- **Session Management:**  
  Simple session handling is implemented on the frontend using `sessionStorage`. Users must be authenticated to access protected pages (e.g., `admin.html`, `customer.html`).

- **Container Communication:**  
  All services are connected on the `foodnet` network. The gateway uses the container names (e.g., `auth-container`) to route requests.

- **Service Ports:**

| Service             | External Port | Internal Port |
|---------------------|---------------|---------------|
| Gateway             | 8080          | 8080          |
| sqlt database       | 8000          | 8000 (internal)|
| Restaurant Service  | 8001          | 8001 (internal)|
| Order Service       | 8002          | 8002 (internal)|
| Delivery Service    | 8003          | 8003 (internal)|
| Auth Service        | 8004          | 8004 (internal)|


---

## 👨‍💻 Author

Developed with ❤️ by HeyyyBooo (Cloud Computing Project by Nishant Narjinary).


---
