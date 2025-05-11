# 🇨🇳 英文 README.md 
---


# SimpleTodo

This project is a REST API service built on FastAPI, primarily designed for managing software package downloads and authentication tokens.

## ✨ Project Features
1.Package Management Functionality：

Create/List/Retrieve Packages

Asynchronous Package Downloads（Integrates with the Nix package manager to fetch packages asynchronously via subprocess calls）

State Management（created → downloaded → activated）

2.Authentication System：
Token-Based Authentication
Token Generation、Operations、Revocation
Validates tokens globally for all protected routes

## 🚀 Quick Start

### Clone the Project

```bash
git clone //github.com/guiye2/fastapi-example.git
cd fastapi-example
```

# Create & Activate Virtual Environment

```bash
python -m venv venv
venv\Scripts\activate  
```
### Install Dependencies

```bash
pip install fastapi sqlalchemy databases uvicorn pytest pytest-asyncio httpx
```
# Initialize Development Database
```bash
python init.py
```

### Launch Project

```bash
npm run dev
```

The service will run at `http://localhost:8000`
