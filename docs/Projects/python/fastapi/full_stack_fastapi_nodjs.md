# Full-Stack Application with Node.js, FastAPI, React, PostgreSQL, Redis, and Machine Learning 🌟

## Table of Contents 📚

1. [Introduction](#introduction)
2. [Project Structure](#project-structure)
3. [Setting Up the Environment](#setting-up-the-environment)
4. [Creating the FastAPI Service](#creating-the-fastapi-service)
5. [Creating the Node.js Service](#creating-the-nodejs-service)
6. [Creating the React Frontend](#creating-the-react-frontend)
7. [Configuring Nginx as a Load Balancer](#configuring-nginx-as-a-load-balancer)
8. [Docker Compose Configuration](#docker-compose-configuration)
9. [Running the Application](#running-the-application)
10. [Testing the Setup](#testing-the-setup)
11. [Conclusion](#conclusion)

---

## 1. Introduction 🎉

In this tutorial, you will create a full-stack application consisting of:

- **Node.js** for managing logs and chat services.
- **FastAPI** for handling machine learning requests, CRUD operations, and caching.
- **PostgreSQL** for data persistence.
- **Redis** for caching and messaging (using Redis Streams).
- **React** as the frontend framework to interact with both backend services.
- **Nginx** to serve as a reverse proxy and load balancer.

## 2. Project Structure 🗂️

Here's the proposed directory structure for the project:

```
/project-root
├── docker-compose.yml
├── fastapi-app
│   ├── src/
│   ├── requirements.txt
│   └── model
│       └── model.pkl
├── node-app
│   ├── src/
│   ├── package.json
│   └── package-lock.json
├── react-app
│   ├── src/
│   └── Dockerfile
└── nginx
    └── default.conf
```

## 3. Setting Up the Environment 🌐

### Prerequisites:

- Install Docker and Docker Compose on your machine.
- Ensure you have Node.js and npm installed for the backend service.
- Ensure you have Python and FastAPI installed for the FastAPI service.

### Create Project Directory:

Create the project root directory and navigate into it:

```bash
mkdir project-root
cd project-root
```

## 4. Creating the FastAPI Service 🚀

### Step 1: Set Up the FastAPI Application

1. Create the `fastapi-app` directory and navigate into it:

   ```bash
   mkdir fastapi-app
   cd fastapi-app
   ```

2. Create a `requirements.txt` file:

   ```plaintext
   fastapi
   uvicorn
   scikit-learn
   numpy
   pandas
   asyncpg
   redis
   ```

3. Create `main.py`:

   ```python
   # fastapi-app/main.py
   from fastapi import FastAPI, HTTPException
   import pickle
   import numpy as np
   import asyncpg
   import redis.asyncio as aioredis

   app = FastAPI()

   # Load the machine learning model
   with open('model/model.pkl', 'rb') as model_file:
       model = pickle.load(model_file)

   # Database connection
   async def get_db_connection():
       return await asyncpg.connect(user='postgres', password='example', database='mydatabase', host='postgres')

   # Redis connection
   redis_client = aioredis.from_url("redis://localhost")

   @app.get("/ml/")
   async def predict(input_data: str):
       data = np.array([input_data])  # Example input transformation
       prediction = model.predict(data)
       return {"prediction": prediction.tolist()}

   @app.get("/crud/{item_id}")
   async def read_item(item_id: int):
       conn = await get_db_connection()
       item = await conn.fetchrow("SELECT * FROM items WHERE id = $1", item_id)
       await conn.close()
       if item:
           return dict(item)
       else:
           raise HTTPException(status_code=404, detail="Item not found.")

   @app.get("/cache/")
   async def cache_example(key: str):
       value = await redis_client.get(key)
       if value:
           return {"cache_value": value}
       else:
           # Simulate fetching from the database
           value = "sample_data"
           await redis_client.set(key, value)
           return {"new_cache_value": value}
   ```

### Step 2: Add Machine Learning Model

- Add your pre-trained model as `model.pkl` in the `model` directory.

## 5. Creating the Node.js Service 🌐

### Step 1: Set Up Node.js Application

1. Create the `node-app` directory:

   ```bash
   mkdir node-app
   cd node-app
   ```

2. Initialize a Node.js application:

   ```bash
   npm init -y
   ```

3. Install required dependencies:

   ```bash
   npm install express body-parser cors redis
   ```

4. Create `index.js`:

   ```javascript
   // node-app/index.js
   const express = require("express");
   const bodyParser = require("body-parser");
   const cors = require("cors");
   const redis = require("redis");

   const app = express();
   const PORT = process.env.PORT || 4000;

   const redisClient = redis.createClient({ url: "redis://localhost" });
   redisClient.connect();

   app.use(cors());
   app.use(bodyParser.json());

   app.get("/log", (req, res) => {
     res.send("Log service is running.");
   });

   app.get("/chat", (req, res) => {
     res.send("Chat service is running.");
   });

   app.post("/chat/send", async (req, res) => {
     const { message } = req.body;
     await redisClient.xAdd("chat_stream", "*", { message });
     res.send("Message sent to chat stream.");
   });

   app.get("/chat/receive", async (req, res) => {
     const messages = await redisClient.xRead("chat_stream", "0");
     res.send(messages);
   });

   app.listen(PORT, () => {
     console.log(`Node.js app listening on port ${PORT}`);
   });
   ```

## 6. Creating the React Frontend 🎨

### Step 1: Set Up React Application

1. Create the `react-app` directory:

   ```bash
   mkdir react-app
   cd react-app
   ```

2. Initialize a React application (you may need to have `create-react-app` installed):

   ```bash
   npx create-react-app .
   ```

3. Modify `src/App.js`:

   ```javascript
   // react-app/src/App.js
   import React, { useEffect, useState } from "react";

   const App = () => {
     const [mlPrediction, setMlPrediction] = useState(null);
     const [crudItem, setCrudItem] = useState(null);
     const [cacheValue, setCacheValue] = useState(null);
     const [message, setMessage] = useState("");
     const [chatMessages, setChatMessages] = useState([]);

     const fetchPrediction = async () => {
       const response = await fetch(
         "http://localhost/ml/?input_data=your_input_here"
       );
       const data = await response.json();
       setMlPrediction(data.prediction);
     };

     const fetchCrud = async () => {
       const response = await fetch("http://localhost/crud/1");
       const data = await response.json();
       setCrudItem(data);
     };

     const fetchCache = async () => {
       const response = await fetch("http://localhost/cache/?key=test_key");
       const data = await response.json();
       setCacheValue(data.cache_value || data.new_cache_value);
     };

     const sendChatMessage = async () => {
       await fetch("http://localhost/chat/send", {
         method: "POST",
         headers: { "Content-Type": "application/json" },
         body: JSON.stringify({ message }),
       });
       setMessage("");
       fetchChatMessages();
     };

     const fetchChatMessages = async () => {
       const response = await fetch("http://localhost/chat/receive");
       const data = await response.json();
       setChatMessages(data);
     };

     useEffect(() => {
       fetchPrediction();
       fetchCrud();
       fetchCache();
       fetchChatMessages();
     }, []);

     return (
       <div>
         <h1>Machine Learning Prediction: {mlPrediction}</h1>
         <h2>
           CRUD Item: {crudItem ? JSON.stringify(crudItem) : "Loading..."}
         </h2>
         <h3>Cache Value: {cacheValue}</h3>
         <h4>Chat Messages:</h4>
         <div>
           {chatMessages.map((msg, index) => (
             <div key={index}>{msg.message}</div>
           ))}
         </div>
         <input
           value={message}
           onChange={(e) => setMessage(e.target.value)}
           placeholder="Type your message"
         />
         <button onClick={sendChatMessage}>Send</button>
       </div>
     );
   };

   export default App;
   ```

## 7. Configuring

Nginx as a Load Balancer 🐳

### Create `nginx/default.conf`:

```nginx
server {
    listen 80;

    # Define upstream servers for load balancing
    upstream backend {
        server node-app:4000;  # Node.js service
        server fastapi:8000;    # FastAPI service
    }

    location /ml/ {
        proxy_pass http://fastapi:8000/ml/;  # Route requests to FastAPI ML endpoint
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
    }

    location /crud/ {
        proxy_pass http://fastapi:8000/crud/;  # Route requests to FastAPI CRUD endpoint
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
    }

    location /log {
        proxy_pass http://node-app:4000/log;  # Route requests to Node.js log endpoint
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
    }

    location /chat {
        proxy_pass http://node-app:4000/chat;  # Route requests to Node.js chat endpoint
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
    }

    location / {
        # Default to serve the React app
        proxy_pass http://react-app:80;  # Serve React app
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
    }
}
```

## 8. Docker Compose Configuration 🐋

### Create `docker-compose.yml`:

```yaml
version: "3.8"

services:
  postgres:
    image: postgres:alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: example
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
    command: redis-server --requirepass example
    volumes:
      - redis-data:/data

  fastapi:
    build:
      context: ./fastapi-app
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    depends_on:
      - postgres
      - redis

  node-app:
    build:
      context: ./node-app
      dockerfile: Dockerfile
    ports:
      - "4000:4000"
    depends_on:
      - redis

  react-app:
    build:
      context: ./react-app
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    depends_on:
      - fastapi
      - node-app

  nginx:
    image: nginx:stable-alpine
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    ports:
      - "8080:80"
    depends_on:
      - fastapi
      - node-app

volumes:
  postgres-data:
  redis-data:
```

## 9. Running the Application 🚀

### Step 1: Build and Start the Application

From the root directory, run the following command to start all services:

```bash
docker-compose up --build
```

### Step 2: Access the Application

- React App: [http://localhost:3000](http://localhost:3000)
- Node.js Services:
  - Logs: [http://localhost:4000/log](http://localhost:4000/log)
  - Chat: [http://localhost:4000/chat](http://localhost:4000/chat)
- FastAPI Services:
  - Machine Learning: [http://localhost:8000/ml](http://localhost:8000/ml)
  - CRUD: [http://localhost:8000/crud/1](http://localhost:8000/crud/1)

## 10. Testing the Setup ✅

### Testing FastAPI Endpoints

```bash
curl http://localhost:8000/ml?input_data=your_input_here
curl http://localhost:8000/crud/1
```

### Testing Node.js Endpoints

```bash
curl http://localhost:4000/log
curl http://localhost:4000/chat
```

### Testing Cache and Message Queue

- **Caching**: Use the FastAPI `/cache/?key=test_key` endpoint to test Redis caching.
- **Message Queue**: Use the Node.js `/chat/send` endpoint to send messages to Redis Streams and `/chat/receive` to retrieve them.

## 11. Conclusion 🎉

Congratulations! You've built a full-stack application using **Node.js**, **FastAPI**, **React**, **PostgreSQL**, **Redis**, and **Machine Learning** with Docker. This architecture provides a solid foundation for further development, including adding more features, enhancing security, and optimizing performance.

---
