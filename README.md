## Project Overview 
**Enterprise-Grade Package Management System**  
A production-ready microservice for software package lifecycle management, built with modern Python web technologies.

###  Technical Architecture

**Core Framework**: [FastAPI 0.103+](https://fastapi.tiangolo.com/) with Starlette ASGI server
**Database**: [SQLite 3.40+](https://www.sqlite.org/) with [SQLAlchemy 2.0](https://www.sqlalchemy.org/) ORM
**Authentication**: HMAC-based token system
**Async Processing**: Native [asyncio](https://docs.python.org/3/library/asyncio.html) integration
**Testing**: [pytest 7.4+](https://docs.pytest.org/) with 100% coverage

###  Key Features                                                    

**Auth System** SHA-256 token generation<br>• Token revocation via DELETE endpoint        
**Package Management** Semantic version validation<br>• Status tracking (created/downloaded/activated) 
**Async Worker** Background nix-build integration<br>• Automatic status transitions        
**Security** Middleware-based validation<br>• SQL injection protection                 
<!-- by 周毅鸿 -->
### Development Setup 
### Installation Methods
#### Method 1: Nix Shell 
```bash
nix-shell  # Auto-configures Python 3.11 + dependencies
pytest -v  # Run test suite (10 test cases)

#### Method 2:
# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip3 install -r requirements.txt
pip3 install -e . 

# Verify installation
pytest -v tests/  

example-init  # Creates SQLite database with schema
uvicorn example.server:app --reload  # Auto-reload for development
# Create new package
POST /api/v1/packages {"name": "security-patch", "version": "2.1.0"}
# Activate package
$ pkgctl activate 1 --token $API_KEY
> Success: Activated package v2.1.0
<!-- by 周毅鸿 -->

<!--by 桂椰-->
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

### Server Startup Process - Development Mode

# Environment Setup
```bash
uvicorn example.server:app --reload
Auto-Reload in Development
Access Addresses: http://localhost:8000

API Documentation Interface:
Swagger UI: http://localhost:8000/docs
ReDoc: http://localhost:8000/redoc

#  Production Mode Deployment
uvicorn example.server:app --host 0.0.0.0 --port 8000 --workers 4

# Start Command:
sudo systemctl daemon-reload
sudo systemctl enable example.service
sudo systemctl start example.service
```
<!--by 桂椰-->

<!-- by [罗娟] -->
# A FastAPI Example

## Development

The development environment can be setup in a few ways.

```sh
nix-shell
pytest -v
```
```sh
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
pip3 install -e .
pytest -v
```
Running the server.

```sh
example-init  # initialize database
uvicorn example.server:app --reload
```
## Docker build

```sh
docker build -it example .
docker run --rm -it example
```
```sh
docker build -it example .
docker run --rm -it example
```

## Docker Deployment

### Development Mode
```bash
# Build the image  
docker-compose build

# Start the service (with hot reload)  
docker-compose up
```
### Production Deployment
```bash
# Build the optimized image  
docker build -t fastapi-prod .

# Run the container  
docker run -d \
  -p 8000:8000 \
  -v ./data:/app/data \
  -e APP_ENV=production \
  fastapi-prod
```

### Environment Variables
| Variable Name | Default Value      | Description         |
|------------- -|--------------------|---------------------|
| `APP_ENV`     | `development`      |Runtime environment  |
| `DB_PATH`     | `./data/app.db`    | Database path       |

# Run with hot reload
docker run -it --rm \
-p 8000:8000 \
-v ./data:/app/data \
-e DATABASE_URL=sqlite:////app/data/dev.db \
-e APP_ENV=development \
fastapi-dev


Key Deployment Considerations

Authentication & Security:

All API endpoints require token authentication.

Tokens are generated via POST /api/v1/tokens.

Token validation is implemented in middleware (see server.py).

Production Recommendations:

bash
# Set a strong secret for token generation  
-e SECRET_KEY=your-strong-secret  
Additional Notes:

Store secrets securely using environment variables or secret management tools.

Rotate tokens periodically for enhanced security.

Restrict API access to authorized clients only.
<!-- by [罗娟] -->

<!-- by 班瑞莲 -->
```sh
docker build -it example .
docker run --rm -it example
REST API Documentation (v1.0)
Category	          Endpoint	        Method	    Description	            Parameters	            Returns	
System	            /hello	          GET	        Service health check	   -	            {"message": "Welcome to Package API"}	
                    /api/v1/version	  GET	        Get server version	     -	            {"version": "1.0.0", "build": "20240505"}	
Packages	          /api/v1/packages	GET	        List all packages	       -	            List[Package] (with id,name,version,status,   created_at)	
                                      POST	      Create new package	   {"name":str* (2-50 chars), "version":str* (semver format)}	                                                Created Package object	
        /api/v1/package/{id}	        GET	        Get package details	    id:int* (in path)	Full Package details	
        /api/v1/package/{id}/download	POST	   Schedule background download	id:int* (in path)	{"status":str, "task_id":uuid}	
        /api/v1/package/{id}/activate	POST	   Activate downloaded package	id:int* (in path)	{"status":"activated"}	
Auth	              /api/v1/tokens	  GET	     List active tokens (requires auth)	-	        List[Token] (masked secrets)	
                                      POST	      Generate new token	      -	             {"token":str, "expires_at":iso8601}	
                  /api/v1/tokens/all	DELETE	    Revoke all tokens (except current)	Requires Authorization: Token {value} header	  {}	
                  /api/v1/token/{id}	DELETE	    Revoke specific token	    id:int* (in path) + auth header	    {}	

cURL Examples
1. Create Package:
curl -X POST 'http://api.example.com/v1/packages' \
  -H 'Authorization: Token your_token_here' \
  -H 'Content-Type: application/json' \
  -d '{"name":"nginx","version":"1.25.3"}'

2. Download Package:
curl -X POST 'http://api.example.com/v1/package/123/download' \
  -H 'Authorization: Token your_token_here'

3. List Tokens:
curl -X GET 'http://api.example.com/v1/tokens' \
  -H 'Authorization: Token your_token_here'
 <!-- by 班瑞莲 -->
 
First get a token:
curl -X POST http://localhost:8000/api/v1/tokens

Then use it to create a package:
curl -X POST http://localhost:8000/api/v1/packages \
  -H "Authorization: Token abc123..." \
  -H "Content-Type: application/json" \
  -d '{"name":"demo","version":"1.0.0"}'

Check its status:
curl -X GET http://localhost:8000/api/v1/package/1 \
  -H "Authorization: Token abc123..."
  <-- by 班瑞莲 -->
  
 <!-- by 张佳琦 (ho0oope) -->
### Test Architecture
The project follows a rigorous testing methodology that includes:
- **Unit Tests**: Independent validation of individual modules
- **Integration Tests**: Comprehensive API endpoint validation
- **Mock Testing**: Simulation of external services and asynchronous processes
- **Database Tests**: Isolated database operations with clean state management
- **Security Tests**: Authentication and authorization verification

### Test Configuration
All tests utilize a dedicated SQLite instance configured as follows:
```python
import os
from example.models import initialize

# Configure test database
os.environ['DATABASE_URL'] = 'sqlite:///./test_environment.db'
initialize(drop_all=True)  # Reset database state before each test suite

# Initialize test client
from starlette.testclient import TestClient
from example.server import app
client = TestClient(app)

Test Best Practices
1.Each test file must initialize a fresh database instance
2.External dependencies should be mocked using unittest.mock
3.Include both success and failure test scenarios
4.Validate all possible HTTP status codes
5.Verify complete response payload structures
6.Use descriptive test names following Given-When-Then pattern
7.Maintain atomic tests with single responsibility
8.Implement proper test cleanup procedures
9.Document test dependencies clearly
10.Include performance benchmarks for critical paths
<!-- by 张佳琦 (ho0oope) -->
