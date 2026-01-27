[[Micro frontend]] = splitting a big frontend ([^1]SPA) into smaller, independently deployable apps.  
Each microfrontend runs on its own **port** (or even separate servers), but the **host (container/shell)** integrates them at runtime.

👉 Two main approaches:

1. **Iframe / Web Components** – simple, but less flexible.
2. **Module Federation (Webpack 5)** – modern, runtime integration, used widely in React + Angular.
## 🔹 What is Module Federation?

Module Federation (introduced in Webpack 5) allows **multiple independently built and deployed apps (microfrontends)** to share code and load each other’s modules at runtime.  
It’s the most popular way to implement **Microfrontends** because it:

- Eliminates duplication (shared libraries like React, Angular, etc. are loaded once).
- Enables **independent deployment** (each microfrontend can be updated without redeploying the whole app).
- Supports **runtime integration** instead of build-time coupling.

---

## 🔹 Core Concepts

- **Host (Shell/App)** → The container that loads remote microfrontends.
- **Remote** → A microfrontend app exposing its modules/components.
- **Shared** → Dependencies shared across apps (e.g., [[React]], [[ReactDOM]], [[Angular]]).

## 🔹 Example: [[React]] + Module Federation

### 1. Remote App (`products`)

**webpack.config.js**

```
const { ModuleFederationPlugin } = require("webpack").container;

module.exports = {
  mode: "development",
  devServer: {
    port: 3001,
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "products", // app name
      filename: "remoteEntry.js", // entry point
      exposes: {
        "./ProductsList": "./src/ProductsList", // expose component
      },
      shared: ["react", "react-dom"],
    }),
  ],
};

```

**src/ProductsList.js**

```
import React from "react";

const ProductsList = () => (
  <div>
    <h2>Products Microfrontend</h2>
    <ul>
      <li>iPhone 16</li>
      <li>MacBook Pro</li>
      <li>AirPods</li>
    </ul>
  </div>
);

export default ProductsList;

```

---

### 2. Host App (`container`)

**webpack.config.js**

```
const { ModuleFederationPlugin } = require("webpack").container;

module.exports = {
  mode: "development",
  devServer: {
    port: 3000,
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "container",
      remotes: {
        products: "products@http://localhost:3001/remoteEntry.js",
      },
      shared: ["react", "react-dom"],
    }),
  ],
};

```
**src/App.js**

```
import React, { Suspense } from "react";

const ProductsList = React.lazy(() => import("products/ProductsList"));

function App() {
  return (
    <div>
      <h1>Host App (Container)</h1>
      <Suspense fallback={<div>Loading Products...</div>}>
        <ProductsList />
      </Suspense>
    </div>
  );
}

export default App;

```

---

## 🔹 [[Angular]] Example (Quick Glimpse)

For [[Angular]], we use `@angular-architects/module-federation` plugin.

- `ng add @angular-architects/module-federation --project shell --port 4200`
- It auto-generates `webpack.config.js` with **remotes/exposes** just like React.

---

## 🔹 Hosting

- Each microfrontend can run on its own **port/server**.
- A **reverse proxy ([[NGINX]], [[API Gateway]],[[Spring Boot]] BFF)** can route requests:
    
    - `/products/** → products microfrontend`
    - `/cart/** → cart microfrontend`
    - `/auth/** → auth microfrontend`
---

## 🔹 Pros

- Independent deployments.
- Shared libraries = reduced bundle size.
- Works well with both React and Angular.
## 🔹 Cons

- Version mismatches (React 17 vs React 18).
- Runtime errors if remote is unavailable.
- Slightly complex CI/CD setup.


Each **microfrontend app** (remote) is built & deployed **independently**.
The **container (shell)** loads remotes dynamically at runtime via `remoteEntry.js`.
Hosting ensures:
1. Users hit **one entrypoint (domain)** instead of juggling multiple ports.
2. Assets (`remoteEntry.js`, JS bundles, CSS) are served correctly.
3. Requests are **routed** to the right microfrontend.
---

## 🔹 Hosting Patterns

### 1. **Direct Hosting (Dev Mode)**

- Each microfrontend runs on its own dev server:
    
    - `container: http://localhost:3000`
    - `products: http://localhost:3001`
    - `cart: http://localhost:3002`

✅ Easy for development  
❌ Not production-ready (multiple ports, CORS issues, scattered entrypoints).

---

### 2. **[[NGINX]] Reverse Proxy**

In production, you usually **serve all microfrontends under one domain** using **NGINX**.

**Example [[NGINX]] config:**

```
server {
    listen 80;

    server_name myshop.com;

    location / {
        root /usr/share/nginx/html/container;
        index index.html;
    }

    location /products/ {
        proxy_pass http://products-service/;
    }

    location /cart/ {
        proxy_pass http://cart-service/;
    }

    location /auth/ {
        proxy_pass http://auth-service/;
    }
}
```

➡️ Users always visit `myshop.com`.

- `/` → Container (shell app)
- `/products/**` → Products MFE
- `/cart/**` → Cart MFE

📌 Benefit: Hides microfrontend ports/servers, gives **single unified domain**.

---

### 3. **API Gateway ([[Spring Cloud Gateway]], Kong, Apigee, AWS API Gateway, etc.)**

- Works like NGINX but more **enterprise-grade**.
    
- Can apply **auth, rate limiting, load balancing, canary releases** at gateway level.
    

**[[Spring Cloud Gateway]] Example (`application.yml`):**

```
spring:
  cloud:
    gateway:
      routes:
        - id: container
          uri: http://container-service:3000
          predicates:
            - Path=/**
        - id: products
          uri: http://products-service:3001
          predicates:
            - Path=/products/**
        - id: cart
          uri: http://cart-service:3002
          predicates:
            - Path=/cart/**

```

📌 Benefit:

- Centralized **routing, monitoring, and security**.
- Can integrate with **[[OAuth 2.0]] / [^2][[JWT]] auth**.
- Better for **multi-tenant SaaS** where rules vary per customer.

---

## 🔹 Typical Deployment Flow

1. Each microfrontend (React, Angular) → built as **static files** (`remoteEntry.js`, `*.js`, `*.css`).
2. Deploy static files to a **CDN or server** (S3 + CloudFront, NGINX container, etc.).
3. Configure **[[NGINX]]/[[API Gateway]]** to serve them from a **single domain**.
4. Container (host) fetches remote [[Micro frontend]] dynamically.

[^1]: Single Page Application

[^2]: Json Web Token
