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
