
### 方法1：使用Nix环境  !--by 杨玉华 --
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
1.包管理功能：
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

### 开始项目

```bash
npm run dev
```

The service will run at `http://localhost:8000`


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

# FastAPI 示例

## 开发环境

```sh
nix-shell
pytest -v
```
或
```sh
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
pip3 install -e .
pytest -v
```

启动服务：
```sh
example-init  # 初始化数据库
uvicorn example.server:app --reload
```

## Docker 构建
```sh
docker build -it example .
docker run --rm -it example
```

## Docker 部署

### 开发模式
```bash
docker-compose build
docker-compose up
```

### 生产环境
```bash
docker build -t fastapi-prod .
docker run -d \
  -p 8000:8000 \
  -v ./data:/app/data \
  -e APP_ENV=production \
  fastapi-prod
```

### 环境变量
| 变量名 | 默认值 | 说明 |
|-------|-------|------|
| `APP_ENV` | `development` | 运行环境 |
| `DB_PATH` | `./data/app.db` | 数据库路径 |

热重载运行：
```bash
docker run -it --rm \
  -p 8000:8000 \
  -v ./data:/app/data \
  -e DATABASE_URL=sqlite:////app/data/dev.db \
  -e APP_ENV=development \
  fastapi-dev
```

## 关键事项

### 认证安全
- 所有API需要令牌认证
- 生成令牌：POST /api/v1/tokens
- 验证逻辑见server.py中间件

生产建议：
```bash
-e SECRET_KEY=your-strong-secret
```

注意事项：
- 使用环境变量或密钥管理工具存储密钥
- 定期更换令牌
- 限制API访问权限





### 开发环境配置

#### 方式一：使用Nix Shell（推荐）
```bash
nix-shell  # 自动配置Python 3.11开发环境及所有依赖
pytest -v  # 执行测试套件（共10个测试用例）
```

#### 方式二：传统虚拟环境
```bash
# 创建并激活虚拟环境
python3 -m venv venv
source venv/bin/activate

# 安装项目依赖
pip3 install -r requirements.txt
pip3 install -e .  # 以开发模式安装

# 验证安装结果
pytest -v tests/  # 运行测试用例
```

### Docker容器化部署
```bash
# 构建Docker镜像
docker build -it example .

# 运行容器实例
docker run --rm -it example
```

## REST API 接口文档（v1.0版）

### 系统接口
| 端点               | 方法   | 说明               | 请求参数 | 返回示例 |
|--------------------|--------|--------------------|----------|----------|
| `/hello`           | GET    | 服务健康检查       | 无       | `{"message": "欢迎使用软件包管理API"}` |
| `/api/v1/version`  | GET    | 获取服务版本信息   | 无       | `{"version": "1.0.0", "build": "20240505"}` |

### 软件包管理
| 端点                           | 方法   | 说明                   | 请求参数                     | 返回内容 |
|--------------------------------|--------|------------------------|------------------------------|----------|
| `/api/v1/packages`             | GET    | 获取所有软件包列表     | 无                           | 软件包对象数组（含ID、名称、版本、状态、创建时间） |
|                                | POST   | 创建新软件包           | `{"name":"字符串(2-50字符)", "version":"语义化版本号"}` | 新建的软件包对象 |
| `/api/v1/package/{id}`         | GET    | 获取指定软件包详情     | `id`: 路径参数（整数）       | 完整软件包详情 |
| `/api/v1/package/{id}/download`| POST   | 调度后台下载任务       | `id`: 路径参数（整数）       | `{"status":"状态", "task_id":"任务ID"}` |
| `/api/v1/package/{id}/activate`| POST   | 激活已下载的软件包     | `id`: 路径参数（整数）       | `{"status":"activated"}` |

### 认证鉴权
| 端点                   | 方法   | 说明                     | 认证要求                     | 返回内容 |
|------------------------|--------|--------------------------|------------------------------|----------|
| `/api/v1/tokens`       | GET    | 获取当前有效令牌列表     | 需要认证                     | 令牌对象数组（密钥脱敏） |
|                        | POST   | 生成新访问令牌           | 无                           | `{"token":"令牌字符串", "expires_at":"过期时间"}` |
| `/api/v1/tokens/all`   | DELETE | 撤销所有令牌（当前除外） | 需`Authorization: Token`请求头 | 空对象`{}` |
| `/api/v1/token/{id}`   | DELETE | 撤销指定令牌             | 需路径参数`id`+认证头        | 空对象`{}` |

## 接口调用示例

1. 创建软件包：
```bash
curl -X POST 'http://api.example.com/v1/packages' \
  -H 'Authorization: Token 您的访问令牌' \
  -H 'Content-Type: application/json' \
  -d '{"name":"nginx","version":"1.25.3"}'
```

2. 触发软件包下载：
```bash
curl -X POST 'http://api.example.com/v1/package/123/download' \
  -H 'Authorization: Token 您的访问令牌'
```

3. 查询有效令牌：
```bash
curl -X GET 'http://api.example.com/v1/tokens' \
  -H 'Authorization: Token 您的访问令牌'
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



