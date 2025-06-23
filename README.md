# 🐳 DevOps Reverse Proxy Assignment

This project demonstrates containerized deployment of two backend services using **Docker Compose**, with **NGINX** acting as a reverse proxy inside a Docker container.

---

## 📁 Project Structure

```
.
├── docker-compose.yml
├── nginx/
│   ├── nginx.conf
│   └── Dockerfile
├── service_1/
│   ├── Dockerfile
│   └── main.go
├── service_2/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
└── README.md
```

---

## 🚀 Setup Instructions

### 1. 🔧 Prerequisites

- Docker
- Docker Compose

### 2. 🛠️ Run the Application

From the root of the project:

```bash
docker-compose up --build
```

This will:
- Build all three services (`service_1`, `service_2`, `nginx`)
- Start them in isolated containers using bridge networking

### 3. 🌐 Access the Services

Once the system is up, test these URLs in your browser or terminal:

| URL                            | Description            |
|--------------------------------|------------------------|
| http://localhost:8080/service1 | Go backend service     |
| http://localhost:8080/service2 | Python Flask service   |

Example using `curl`:

```bash
curl http://localhost:8080/service1/ping
curl http://localhost:8080/service2
```

---

## 🔀 How Routing Works (NGINX Reverse Proxy)

We use a **custom `nginx.conf`** with reverse proxy rules inside a Docker container.

### 📦 `docker-compose.yml` (Relevant Part)

```yaml
services:
  nginx:
    build: ./nginx
    ports:
      - "8080:80"
    depends_on:
      - service_1
      - service_2
```

### 📁 `nginx/nginx.conf` (Simplified)

```nginx
location /service1/ {
    proxy_pass http://service_1:8001/;
}
location /service2/ {
    proxy_pass http://service_2:8002/;
}
```

So:
- Requests to `localhost:8080/service1` → Go service on port 8001
- Requests to `localhost:8080/service2` → Python Flask service on port 8002

📝 Also logs each incoming request with timestamp and path.

---

## ✅ Features Implemented

### ✅ NGINX Reverse Proxy in Docker

- NGINX runs in its **own Docker container**
- Uses path-based routing to backend services

### ✅ Health Checks (Bonus ✔️)

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:8001/ping"]
```

- Both services have health checks
- Ensures NGINX only starts routing once services are healthy

### ✅ Logging (Bonus ✔️)

NGINX logs all incoming requests:

```
127.0.0.1 - [23/Jun/2025:21:00:01 +0000] "GET /service1 HTTP/1.1" 200
```

### ✅ Single Port Access

- Both apps accessible on `http://localhost:8080` using different path prefixes.

---

## 🧹 Cleanup

To stop and clean all containers:

```bash
docker-compose down
```

To free disk space (optional):

```bash
docker system prune -f
```

---

## 📌 .gitignore (Recommended)

```gitignore
# Python
__pycache__/
*.pyc

# Go
main
*.exe

# Docker
*.log

# Misc
.env
.DS_Store
```

---

## 📚 Conclusion

This project shows how to:
- Use **Docker Compose** to orchestrate multi-language services (Go + Python)
- Use **NGINX reverse proxy** (in a container) for unified routing
- Add **health checks** and **structured logging**
- Serve multiple apps via a single exposed port

---

## 👨‍💻 Author

Varalakshmi P  
DevOps Intern Assignment — 2025
