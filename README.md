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