(<!--by 杨玉华 -->)
### 方法1：使用Nix环境
nix-shell  # 自动配置Python 3.11开发环境及所有依赖项  
pytest -v  # 执行完整测试套件（包含10个测试用例）  

### 方法2：传统虚拟环境方式
# 创建Python虚拟环境  
python3 -m venv venv  
# 激活虚拟环境  
source venv/bin/activate  

# 安装项目依赖
pip3 install -r requirements.txt  # 安装基础依赖
pip3 install -e .  # 以开发模式安装当前项目

# 运行测试验证
pytest -v tests/  # 执行测试套件验证安装

# 初始化项目数据库
example-init  # 初始化SQLite数据库并创建数据表结构
# 启动开发服务器
uvicorn example.server:app --reload  # 启动服务并启用代码热重载

# 软件包管理操作示例
# 创建新软件包（API请求示例）
POST /api/v1/packages {"name": "security-patch", "version": "2.1.0"}  

# 使用命令行工具激活软件包
$ pkgctl activate 1 --token $API_KEY  
> 操作成功：软件包v2.1.0已激活

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

## 测试配置说明

测试环境使用独立的测试数据库(test.db)，系统会自动配置，无需人工干预。

## 测试执行命令

```bash
# 执行完整测试套件
pytest

# 按模块执行测试
pytest test_packages.py  # 软件包管理测试
pytest test_tokens.py    # 令牌管理测试 
pytest test_hello.py     # 基础功能测试

# 生成测试覆盖率报告
pip install pytest-cov
pytest --cov=example --cov-report=html
```

## 测试分类详解

### 基础功能测试 (test_hello.py)
- 验证/hello端点响应正确性
- 独立于数据库环境的纯功能测试

### 软件包管理测试 (test_packages.py)
- 完整测试软件包生命周期：
  - 创建软件包接口(/api/v1/packages)
  - 软件包列表查询(/api/v1/packages)
  - 软件包下载(/api/v1/package/{id}/download) 
  - 软件包激活(/api/v1/package/{id}/activate)
- 使用mock技术模拟Nix调用，避免实际依赖

### 令牌管理测试 (test_tokens.py)
- 全面测试令牌管理功能：
  - 令牌创建(/api/v1/tokens)
  - 令牌验证中间件
  - 单个令牌删除(/api/v1/token/{id})
  - 批量令牌删除(/api/v1/tokens/all)

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
pip3 install -r requirements.txt  # 安装依赖项
pip3 install -e .  # 以可编辑模式安装
pytest -v  # 运行测试
```

启动开发服务器：

```sh
example-init  # 初始化数据库
uvicorn example.server:app --reload  # 带热重载启动
```

## Docker 构建

```sh
docker build -it example .  # 构建Docker镜像
docker run --rm -it example  # 运行容器
```

---

## API 接口文档

### 1. 软件包管理API

#### 基础操作
| 端点路径                      | 方法  | 描述                 | 参数                     | 返回类型          |
|-------------------------------|-------|----------------------|--------------------------|-------------------|
| `/api/v1/packages`            | GET   | 获取所有软件包列表   | -                        | List[Package]     |
| `/api/v1/package/{record_id}` | GET   | 获取特定软件包详情   | record_id: int           | Package           |
| `/api/v1/packages`            | POST  | 创建新软件包         | name: str, version: str  | Package           |

#### 状态操作
| 端点路径                                  | 方法  | 描述                     | 参数           | 返回类型          |
|-------------------------------------------|-------|--------------------------|----------------|-------------------|
| `/api/v1/package/{record_id}/download`    | POST  | 调度后台下载任务         | record_id: int | {status: str}     |
| `/api/v1/package/{record_id}/activate`    | POST  | 激活已下载的软件包       | record_id: int | {status: str}     |

### 2. 认证令牌API
| 端点路径                      | 方法    | 描述                         | 参数           | 返回类型      |
|-------------------------------|---------|------------------------------|----------------|---------------|
| `/api/v1/tokens`              | GET     | 列出所有活跃令牌             | -              | List[Token]   |
| `/api/v1/tokens`              | POST    | 生成新认证令牌               | -              | Token         |
| `/api/v1/tokens/all`          | DELETE  | 撤销所有令牌（当前除外）     | 需要认证       | {}            |
| `/api/v1/token/{record_id}`   | DELETE  | 删除特定令牌                 | record_id: int | {}            |

### 3. 系统API
| 端点路径              | 方法  | 描述             | 返回类型          |
|-----------------------|-------|------------------|-------------------|
| `/hello`              | GET   | 测试端点         | {message: str}    |
| `/api/v1/version`     | GET   | 获取服务器版本   | {version: str}    |

---

## 使用示例

1. 首先获取令牌：
```bash
curl -X POST http://localhost:8000/api/v1/tokens
```

2. 使用令牌创建软件包：
```bash
curl -X POST http://localhost:8000/api/v1/packages \
  -H "Authorization: Token abc123..." \
  -H "Content-Type: application/json" \
  -d '{"name":"demo","version":"1.0.0"}'
```

3. 检查软件包状态：
```bash
curl -X GET http://localhost:8000/api/v1/package/1 \
  -H "Authorization: Token abc123..."
```

## 测试规范

### 测试体系架构
本项目采用严格的测试方法论，包含以下测试类型：
- **单元测试**：独立验证各个功能模块
- **集成测试**：全面验证API端点功能
- **模拟测试**：外部服务和异步流程的模拟测试
- **数据库测试**：带清洁状态管理的独立数据库操作测试
- **安全测试**：认证与授权机制验证

### 测试环境配置
所有测试使用专用SQLite实例，配置如下：
```python
import os
from example.models import initialize

# 测试数据库配置
os.environ['DATABASE_URL'] = 'sqlite:///./test_environment.db'
initialize(drop_all=True)  # 每个测试套件执行前重置数据库状态

# 初始化测试客户端
from starlette.testclient import TestClient
from example.server import app
client = TestClient(app)
```

### 测试最佳实践
1. 每个测试文件必须初始化全新的数据库实例
2. 使用unittest.mock模拟外部依赖
3. 需包含成功和失败两种测试场景
4. 验证所有可能的HTTP状态码
5. 检查完整的响应数据结构
6. 测试命名遵循Given-When-Then模式
7. 保持测试原子性（单一职责原则）
8. 实现完善的测试清理机制
9. 明确标注测试依赖项
10. 对关键路径包含性能基准测试

(<!--by 杨玉华 -->)


