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

###   Testing Guide

# Test Setup
Tests automatically use an isolated test database (test.db).
No manual configuration required.

# Run Tests
```bash
# Execute all tests
pytest

# Running Specific Test Modules
pytest test_packages.py
pytest test_tokens.py
pytest test_hello.py

# Generating Test Coverage Reports
pip install pytest-cov
pytest --cov=example --cov-report=html
```

# Test Categories

# Basic Functionality Tests (test_hello.py)
Verify the /hello endpoint returns the correct response
no database dependency
Package Management Tests (test_packages.py)

# Package Lifecycle Testing：
Package Creation (/api/v1/packages)
Package Listing (/api/v1/packages)
Package Download (/api/v1/package/{id}/download)
Package Activation (/api/v1/package/{id}/activate)
Mocking Nix Package Manager in Tests

#  Token Lifecycle Testing (test_tokens.py)
Testing the Complete Token Lifecycle：
Token Generation (/api/v1/tokens)
Token Middleware Validation
Token  Revocation (/api/v1/token/{id})
Bulk Expiration (/api/v1/tokens/all)
