# **Difference between Authentication and Authorization with FastAPI** 🔑🔓

**Welcome!** 🎉  
In this tutorial, we'll explore the difference between **Authentication** and **Authorization** and how to implement them using **FastAPI**. By the end of this guide, you'll be able to secure your applications by applying proper authentication and authorization mechanisms.

---

### **📜 Table of Contents**

1. [Introduction](#1-introduction)
2. [What is Authentication?](#2-what-is-authentication)
3. [What is Authorization?](#3-what-is-authorization)
4. [Authentication vs Authorization](#4-authentication-vs-authorization)
5. [Setting Up FastAPI Project](#5-setting-up-fastapi-project)
6. [Implementing Authentication in FastAPI](#6-implementing-authentication-in-fastapi)
7. [Implementing Authorization in FastAPI](#7-implementing-authorization-in-fastapi)
8. [Practical Example](#8-practical-example)
9. [Conclusion](#9-conclusion)
10. [References](#10-references)

---

### **1. Introduction** ⚙️🛠️

In software development, **Authentication** and **Authorization** are two essential security concepts. Understanding and properly applying these concepts helps ensure that your applications are protected from unauthorized access and that users have appropriate permissions to perform specific actions.

---

### **2. What is Authentication?** 🕵️‍♂️

**Authentication** is the process of verifying the identity of a user or system. It ensures that the person or entity trying to access a system is who they claim to be.

**Common Authentication Methods:**
- **Password-Based:** The most common method where the user provides a password to verify their identity.
- **Two-Factor Authentication (2FA):** Combines something you know (password) with something you have (like a phone that receives a verification code).
- **Biometrics:** Uses physical traits like fingerprints or facial recognition to verify identity.

---

### **3. What is Authorization?** 🔐

**Authorization** refers to the process of granting or denying specific permissions to an authenticated user. Once a user is authenticated, authorization ensures they only access resources or perform actions they are permitted to do.

**Example:**
- **Role-Based Access Control (RBAC):** A common authorization method where users are assigned roles (e.g., admin, editor, viewer) with specific permissions.

---

### **4. Authentication vs Authorization** 🧐

- **Authentication:** Confirms the identity of a user (Who are you?).
- **Authorization:** Determines what the authenticated user is allowed to do (What can you do?).

| **Feature**           | **Authentication**                           | **Authorization**                              |
|-----------------------|----------------------------------------------|------------------------------------------------|
| **Purpose**           | Verify identity                              | Control access to resources                    |
| **When It Occurs**    | First, before authorization                  | After authentication                           |
| **Examples**          | Login with username and password             | Checking if a user is allowed to access an admin panel |

---

### **5. Setting Up FastAPI Project**

Let's create a simple FastAPI application. First, install FastAPI and `uvicorn` (the ASGI server):

```bash
pip install fastapi uvicorn
```

Create a file called `main.py`:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Welcome to FastAPI Authentication and Authorization tutorial!"}
```

Run the application:

```bash
uvicorn main:app --reload
```

Navigate to `http://127.0.0.1:8000/` to see the welcome message.

---

### **6. Implementing Authentication in FastAPI**

To implement authentication, we will use a simple **OAuth2** flow with a password.

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

def fake_decode_token(token):
    # Here we decode the token and return the username (a mock example)
    return token

@app.post("/token")
async def login():
    return {"access_token": "fake-token", "token_type": "bearer"}

@app.get("/users/me")
async def read_users_me(token: str = Depends(oauth2_scheme)):
    user = fake_decode_token(token)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid authentication credentials",
            headers={"WWW-Authenticate": "Bearer"},
        )
    return {"user": user}
```

Here, we have a simple endpoint to simulate a token-based authentication system. The `/token` endpoint provides a token (in reality, you would verify the user with a database), and `/users/me` checks the token for authentication.

---

### **7. Implementing Authorization in FastAPI**

Authorization is about controlling access. Let's implement role-based authorization by defining an admin role and ensuring only admin users can access specific routes.

```python
from fastapi import Security

roles = {"alice": "admin", "bob": "user"}

def get_current_user(token: str = Depends(oauth2_scheme)):
    user = fake_decode_token(token)
    if user not in roles:
        raise HTTPException(status_code=400, detail="Invalid user")
    return user

def get_admin_user(current_user: str = Security(get_current_user)):
    if roles.get(current_user) != "admin":
        raise HTTPException(status_code=403, detail="Not enough permissions")
    return current_user

@app.get("/admin")
async def read_admin_data(admin_user: str = Depends(get_admin_user)):
    return {"admin_data": "You have admin access!"}
```

In this example, users with the role "admin" can access the `/admin` route, while others will be denied.

---

### **8. Practical Example**

Let's run through the process of authenticating a user and checking their authorization. Here’s a scenario:

1. **User Logs In**: They provide credentials, and a token is generated (`/token`).
2. **User Accesses Resources**: The token is used to access protected resources.
3. **Authorization Check**: If the user tries to access admin routes, the role is checked.

---

### **9. Conclusion** 🎉

In this tutorial, we covered the difference between **Authentication** and **Authorization** using **FastAPI**. We demonstrated how to implement authentication using OAuth2 and how to restrict access to certain routes based on user roles.

- **Authentication**: Verifies identity.
- **Authorization**: Determines permissions.

By using these techniques, you can ensure that your applications are secure and that users only have access to the resources they are allowed to see.

---

### **10. References**

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [OAuth2 Security in FastAPI](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/)

---

