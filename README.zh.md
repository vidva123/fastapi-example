### 方法1：
nix-shell  # 自动配置Python 3.11及依赖环境  
pytest -v  # 运行测试套件（共10个测试用例）  

### 方法2：
# 创建并激活虚拟环境  
python3 -m venv venv  
source venv/bin/activate  

# 安装依赖项  
pip3 install -r requirements.txt  
pip3 install -e .  

# 验证安装  
pytest -v tests/  

# 初始化示例  
example-init  # 创建带结构的SQLite数据库  
uvicorn example.server:app --reload  # 开发时自动重载  

# 创建新软件包  
POST /api/v1/packages {"name": "security-patch", "version": "2.1.0"}  

# 激活软件包  
$ pkgctl activate 1 --token $API_KEY  
> 成功：已激活软件包 v2.1.0  

# 🇨🇳 中文版 README.md 
---


# SimpleTodo

这个项目是一个基于FastAPI的REST API服务，主要用于管理软件包下载和认证令牌。

## ✨ 项目特点
1.理功能：
创建、列出、检索软件包
异步下载软件包（使用Nix包管理器）
包状态管理（created → downloaded → activated）
2.认证系统：
基于令牌的认证
令牌生成、管理和撤销
中间件验证所有请求的令牌
## 🚀 快速开始

### 克隆项目

```bash
git clone //github.com/guiye2/fastapi-example.git
cd fastapi-example
```

# 创建并激活虚拟环境

```bash
python -m venv venv
venv\Scripts\activate  
```
### 安装依赖

```bash
pip install fastapi sqlalchemy databases uvicorn pytest pytest-asyncio httpx
```
# 初始化开发数据库
```bash
python init.py
```

###    测试说明

# 测试准备
测试使用独立的测试数据库(test.db)，无需手动配置。

# 运行测试
```bash
# 运行全部测试
pytest

# 运行特定测试模块
pytest test_packages.py
pytest test_tokens.py
pytest test_hello.py

# 生成测试覆盖率报告
pip install pytest-cov
pytest --cov=example --cov-report=html
```

# 测试分类说明

# 基础功能测试 (test_hello.py)
测试/hello端点返回正确响应
不依赖数据库状态
包管理测试 (test_packages.py)

# 测试包的生命周期管理：
创建包 (/api/v1/packages)
列出包 (/api/v1/packages)
下载包 (/api/v1/package/{id}/download)
激活包 (/api/v1/package/{id}/activate)
使用mock避免实际调用Nix

# 令牌管理测试 (test_tokens.py)
测试Token的完整生命周期：
创建Token (/api/v1/tokens)
Token验证中间件
Token删除 (/api/v1/token/{id})
批量删除 (/api/v1/tokens/all)

### 服务器启动流程 - 开发模式

# 环境配置
```bash
uvicorn example.server:app --reload
Auto-Reload in Development
Access Addresses: http://localhost:8000

API Documentation Interface:
Swagger UI: http://localhost:8000/docs
ReDoc: http://localhost:8000/redoc

#  生产模式部署
uvicorn example.server:app --host 0.0.0.0 --port 8000 --workers 4

# 服务启动命令
sudo systemctl daemon-reload
sudo systemctl enable example.service
sudo systemctl start example.service
```

# FastAPI 示例项目

## 开发环境配置

可通过以下方式搭建开发环境：

```sh
nix-shell  # 进入Nix开发环境
pytest -v  # 运行测试套件
```
```sh
python3 -m venv venv  # 创建虚拟环境
source venv/bin/activate  # 激活虚拟环境
pip3 install -r requirements.txt  # 安装依赖
pip3 install -e .  # 可编辑模式安装
pytest -v  # 运行测试
```

启动开发服务器：

```sh
example-init  # 初始化数据库
uvicorn example.server:app --reload  # 带热重载启动
```

## Docker 构建

```sh
docker build -it example .  # 构建镜像
docker run --rm -it example  # 运行容器
```

## Docker 部署方案

### 开发模式
```bash
# 构建镜像
docker-compose build

# 启动服务（支持代码热更新）
docker-compose up
```

### 生产环境
```bash
# 构建生产级镜像
docker build -t fastapi-prod .

# 运行容器
docker run -d \
  -p 8000:8000 \
  -v ./data:/app/data \
  -e APP_ENV=production \
  fastapi-prod
```

### 环境变量配置
| 变量名       | 默认值             | 说明                 |
|--------------|--------------------|----------------------|
| `APP_ENV`    | `development`      | 应用运行环境         |
| `DB_PATH`    | `./data/app.db`    | SQLite数据库文件路径 |
