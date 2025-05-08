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
## 术语表截图
![术语表文件内容](ai_usage_screenshots/2205308010326_1.ccccccccc)  
<!-- by 黄小丽 -->

## API 文档界面
![FastAPI Swagger UI](ai_usage_screenshots/2205308010326_2.png)  
<!-- by 黄小丽 -->

## API 文档界面
![FastAPI Swagger UI](ai_usage_screenshots/2205308010326_3.png)  
<!-- by 黄小丽 -->