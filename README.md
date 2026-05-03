

# 🧠 1 Million Checkboxes — Real-Time Distributed System

A high-performance real-time system where multiple authenticated users can toggle and view the state of **1,000,000 checkboxes** simultaneously.

This project demonstrates:

* Efficient state storage using **bit manipulation**
* Real-time sync using **WebSockets**
* Distributed messaging using **Redis Pub/Sub**
* Secure authentication using **OIDC / OAuth 2.0**
* Rate limiting and scalability design

---



# 📌 Project Overview

Instead of storing checkbox states as individual records, this system:

👉 Uses a **bitset stored in Redis**
👉 Each checkbox = **1 bit (0 or 1)**
👉 Total memory for 1M checkboxes ≈ **125 KB**

---

# ⚙️ Tech Stack

### Backend

* Node.js
* Express
* Socket.IO
* Redis
* OIDC / OAuth 2.0
* JWT

### Frontend

* HTML / JS (or React if used)

---

# 🏗️ System Architecture

```id="arch1"
/client (browser)
        ↓ (WebSocket)
Socket.IO Server (Node.js)
        ↓
Redis (Bit Storage)
        ↓
Redis Pub/Sub
        ↓
All Connected Clients (Real-time sync)
```

---

# 🔐 Authentication Flow (OIDC)

1. User clicks **Login**
2. Redirected to Identity Provider
3. After login → `/callback`
4. Server exchanges **authorization code → token**
5. User session stored

### Protected Actions

* Only logged-in users can toggle checkboxes

---

# 🔌 Socket Event Flow

### 1. Fetch checkbox range

```id="flow1"
client → server: "client:request:range"
server → Redis: GETBIT (pipeline)
server → client: "server:response:range"
```

---

### 2. Toggle checkbox

```id="flow2"
client → server: "client:checkbox:change"
server → Redis: SETBIT
server → Redis Pub/Sub: publish event
subscribers → broadcast to all clients
```

---

# 🧵 Redis Usage

## 1. Bit Storage

```id="redis1"
SETBIT checkbox_state index value
GETBIT checkbox_state index
```

👉 Efficient storage:

* 1,000,000 checkboxes = ~125 KB

---

## 2. Pub/Sub

Channel:

```id="redis2"
checkbox:update
```

Used to:

* Broadcast updates across all instances
* Enable horizontal scaling

---

## ⚡ Why Bitset Instead of DB?

| Approach       | Problem           |
| -------------- | ----------------- |
| SQL rows       | 1M rows = slow    |
| JSON array     | heavy memory      |
| Bitset (Redis) | ✅ ultra efficient |

---

# 🚦 Rate Limiting

Implemented using Redis:

```id="ratelimit1"
INCR key
EXPIRE key
```

### Rules:

* Max **10 actions per second per user**

👉 Prevents spam + protects system

---

# 🧠 Key Design Decisions

## 1. Why Redis?

* In-memory → ultra fast
* Bit operations → perfect for boolean state
* Pub/Sub → real-time sync

---

## 2. Why NOT direct broadcasting only?

Without Pub/Sub:

* Multi-server scaling breaks
* Users connected to different instances won't sync

---

## 3. Why range-based fetching?

Loading 1M checkboxes at once ❌
Instead:

```id="range1"
Load only visible range (virtualization)
```

---

# 📁 Project Structure

```id="struct1"
/public         → frontend
/server.js      → main backend
.env            → environment config
```

---

# ⚙️ Setup Instructions

## 1. Clone repo

```bash
git clone <your-repo-link>
cd project
```

---

## 2. Install dependencies

```bash
npm install
```

---

## 3. Setup Redis

```bash
docker run -p 6379:6379 redis
```

---

## 4. Configure environment

Create `.env`:

```env
PORT=8000
REDIS_URL=redis://localhost:6379

SESSION_SECRET=your_secret

IDP_URL=your_oidc_provider
CLIENT_ID=your_client_id
CLIENT_SECRET=your_secret
REDIRECT_URI=http://localhost:8000/callback
POST_LOGOUT_REDIRECT_URI=http://localhost:8000
```

---

## 5. Run server

```bash
node server.js
```

---

# 🌐 API Endpoints

| Endpoint    | Description      |
| ----------- | ---------------- |
| `/login`    | Start OIDC login |
| `/callback` | Handle auth      |
| `/logout`   | Logout           |
| `/me`       | Get current user |

---

# 🔄 Real-Time Features

✅ Multiple users see updates instantly
✅ Toggle sync across all clients
✅ Efficient partial loading
✅ Auth-protected actions

---

# ⚠️ Error Handling

* Invalid index ignored
* Unauthorized actions blocked
* Rate limit enforced
* Safe socket disconnect handling

---

# 📉 Limitations

* No persistent DB (Redis only)
* No checkbox grouping/filtering
* UI can be improved for large-scale rendering

---

# 🔮 Future Improvements

* Add Kafka for event streaming
* Add persistent DB storage
* Improve frontend virtualization
* Add user activity tracking

---

# 🧪 Demo Requirements Covered

✔ Login flow
✔ Real-time checkbox updates
✔ Multi-user sync
✔ Rate limiting
✔ Efficient storage

---

# 🧠 System Understanding

This project demonstrates:

* Real-time system design
* Memory-efficient data modeling
* Distributed event propagation
* Auth + WebSocket integration

---

# 📜 License

MIT License

---

If you want next, I can:

* turn this into a **perfect GitHub repo structure**
* help you **prepare demo explanation (very important for marks)**
* or simulate **interview questions from this project**

Just tell me 👍
