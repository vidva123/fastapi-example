<!-- by 周毅鸿 -->
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


<!-- by [罗娟] -->
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

<!-- by [罗娟] -->

<!-- by 班瑞莲 -->
master
```sh
docker build -it example .
docker run --rm -it example
```
<!--
1. Package Management APIs
Basic Operations
Endpoint	                Method	Description	                         Parameters	                Returns
/api/v1/packages	        GET	    Retrieve list of all packages	    -	                        List[Package]
/api/v1/package/{record_id}	GET	    Get details of a specific package	record_id: int	            Package
/api/v1/packages	        POST	Create a new package	        name: str, version: str	        Package

Status Operations
Endpoint	                            Method	Description	                        Parameters	    Returns
/api/v1/package/{record_id}/download	POST	Schedule background download task	record_id: int	{status: str}
/api/v1/package/{record_id}/activate	POST	Activate a downloaded package	    record_id: int	{status: str}

2. Authentication Token APIs
Endpoint	                Method	Description	                        Parameters	            Returns
/api/v1/tokens	            GET	    List all active tokens	            -	                List[Token]
/api/v1/tokens	            POST	Generate new authentication token	-	                Token
/api/v1/tokens/all	        DELETE	Revoke all tokens (except current)	Requires auth	    {}
/api/v1/token/{record_id}	DELETE	Delete specific token	            record_id: int	    {}

3. System APIs
Endpoint	    Method	Description	        Returns
/hello	        GET	    Test endpoint	    {message: str}
/api/v1/version	GET	    Get server version	{version: str}

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
  
  ## Testing <!-- by 张佳琦 (ho0oope) -->

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

