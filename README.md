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
