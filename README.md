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
  by 班瑞莲 -->
=======
example-init  # Creates SQLite database with schema
uvicorn example.server:app --reload  # Auto-reload for development
# Create new package
POST /api/v1/packages {"name": "security-patch", "version": "2.1.0"}
# Activate package
$ pkgctl activate 1 --token $API_KEY
> Success: Activated package v2.1.0
<!-- by 周毅鸿 -->
 master
