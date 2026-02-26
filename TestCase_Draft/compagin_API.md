| SC002 | 正常认证用户无可用活动 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 0,
  "campaigns": []
}
``` | 边界场景：成功但无数据，需区分于错误场景 |
| SC003 | 活动字段取边界值（point_amount/日期） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 0,
  "campaigns": [
    {
      "id": 3,
      "title": "边界值测试活动",
      "brief_eligibility": [],
      "point_amount": 0,
      "required_documents": [],
      "point_grant_date": "1970-01-01"
    }
  ]
}
``` | 验证字段边界值兼容性 |
| SC004 | 活动字段含特殊字符（title/eligibility） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 0,
  "campaigns": [
    {
      "id": 4,
      "title": "「夏季限定！」￥5000积分＆抽獎活动",
      "brief_eligibility": ["\n新用户\n", "消费满1000元（含税）"],
      "point_amount": 5000,
      "required_documents": ["身份证", "消费凭证"],
      "point_grant_date": "2025-08-01"
    }
  ]
}
``` | 验证字符编码（UTF-8）兼容性 |
| SC005 | 单条活动数据返回（边界：仅1条） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 0,
  "campaigns": [
    {
      "id": 5,
      "title": "单条活动测试",
      "brief_eligibility": ["所有用户"],
      "point_amount": 2000,
      "required_documents": ["无"],
      "point_grant_date": "2025-10-01"
    }
  ]
}
``` | 边界场景：仅返回1条活动数据 |

### 2. HTTP 401（Unauthorized）
| 用例ID | 测试用例描述 | Mock API Request | Mock API Response | 备注（边界/特殊点） |
|--------|--------------|------------------|-------------------|---------------------|
| EC001 | 请求头无Authorization字段 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 1,
  "error_message": "Authentication is required."
}
``` | 基础认证缺失场景 |
| EC002 | Authorization字段无Bearer前缀 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: invalid_token123456
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 1,
  "error_message": "Authentication is required."
}
``` | 格式错误场景 |
| EC003 | Bearer后无token（空值） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer 
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 1,
  "error_message": "Authentication is required."
}
``` | 边界场景：token为空字符串 |
| EC004 | Bearer token为无效值（随机字符串） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer random123456abcdef
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 1,
  "error_message": "Authentication is required."
}
``` | 无效token场景 |
| EC005 | Bearer token已过期 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTc2NzUyMDB9.9fQj5V8lZ0e7R9s8k7j6h5g4f3d2s1a0z9x8w7v6u5t4s3a2q1w0e9r8t7y6u5i4o3p2o1i0u9y8t7g6f5d4s3a2s1d2f3g4h5j6k7l8m9n0b1v2c3x4z5a6s7d8f9g0h1j2k3l4m5n6b7v8c9x0z1a2s3d4f5g6h7j8k9l0m1n2b3v4c5x6z7a8s9d0f1g2h3j4k5l6m7n8b9v0c1x2z3a4s5d6f7g8h9j0k1l2m3n4b5v6c7x8z9a0s1d2f3g4h5j6k7l8m9n0b1v2c3x4z5a6s7d8f9g0h1j2k3l4m5n6b7v8c9x0z1a2s3d4f5g6h7j8k9l0m1n2b3v4c5x6z7a8s9d0f1g2h3j4k5l6m7n8b9v0c1x2z3a4s
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 1,
  "error_message": "Authentication is required."
}
``` | 过期token场景 |
| EC006 | Authorization字段格式正确但token被篡改 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY789.eyJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 1,
  "error_message": "Authentication is required."
}
``` | 篡改合法token的payload部分 |

### 3. HTTP 403（Forbidden）
| 用例ID | 测试用例描述 | Mock API Request | Mock API Response | 备注（边界/特殊点） |
|--------|--------------|------------------|-------------------|---------------------|
| EC007 | 有效token但账户关联未完成 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 2,
  "error_message": "Please complete account linkage first."
}
``` | 核心403场景 |
| EC008 | 有效token但账户关联已失效 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 2,
  "error_message": "Please complete account linkage first."
}
``` | 关联失效边界场景 |
| EC009 | 有效token但用户无访问活动列表权限（权限等级不足） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 2,
  "error_message": "User not allowed to access campaign list."
}
``` | 补充场景：用户权限不足（非关联问题） |
| EC010 | 有效token但用户被封禁（黑名单用户） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 2,
  "error_message": "User not allowed to access campaign list."
}
``` | 补充场景：黑名单用户访问 |
| EC011 | 有效token但活动列表功能被管理员禁用 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 2,
  "error_message": "User not allowed to access campaign list."
}
``` | 补充场景：功能级权限禁用 |

### 4. HTTP 500（Internal Server Error）
| 用例ID | 测试用例描述 | Mock API Request | Mock API Response | 备注（边界/特殊点） |
|--------|--------------|------------------|-------------------|---------------------|
| EC012 | DB连接失败（模拟） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 3,
  "error_message": "A system error has occurred."
}
``` | 核心500场景，验证服务容错 |
| EC013 | SQL查询语法错误（模拟） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 3,
  "error_message": "A system error has occurred."
}
``` | 内部逻辑错误场景 |
| EC014 | 活动数据字段类型异常（如point_amount为字符串） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 3,
  "error_message": "A system error has occurred."
}
``` | 数据异常导致的内部错误 |

### 5. HTTP 503（Service Unavailable）
| 用例ID | 测试用例描述 | Mock API Request | Mock API Response | 备注（边界/特殊点） |
|--------|--------------|------------------|-------------------|---------------------|
| EC015 | 服务过载/维护中（模拟） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 4,
  "error_message": "The service is temporarily unavailable."
}
``` | 503核心场景 |
| EC016 | 依赖服务不可用（如活动数据服务下线） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 4,
  "error_message": "The service is temporarily unavailable."
}
``` | 补充场景：依赖服务异常 |

### 6. HTTP 504（Gateway Timeout）
| 用例ID | 测试用例描述 | Mock API Request | Mock API Response | 备注（边界/特殊点） |
|--------|--------------|------------------|-------------------|---------------------|
| EC017 | 后端服务响应超时（模拟） | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 5,
  "error_message": "Request timed out."
}
``` | 504核心场景，验证超时处理 |
| EC018 | 大数据量查询导致网关超时 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 5,
  "error_message": "Request timed out."
}
``` | 补充场景：数据量过大引发超时 |

### 7. HTTP 400（Bad Request）
| 用例ID | 测试用例描述 | Mock API Request | Mock API Response | 备注（边界/特殊点） |
|--------|--------------|------------------|-------------------|---------------------|
| EC019 | 请求头Accept字段格式错误 | ```http
GET /api/v1/campaigns/list HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json;charset=GBK
Content-Type: application/json
``` | ```json
{
  "result": 6,
  "error_message": "Invalid request header: Accept"
}
``` | 新增场景：请求头格式错误 |
| EC020 | URL路径错误（非/campaigns/list） | ```http
GET /api/v1/campaigns/lists HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjM0NTY3ODkiLCJleHAiOjE3MTk4NjQwMDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
Content-Type: application/json
``` | ```json
{
  "result": 6,
  "error_message": "Invalid request path"
}
``` | 新增场景：路径错误 |

## 总结
1. 核心覆盖场景：补充了403中「用户无访问权限」「黑名单用户」「功能禁用」等缺失场景，新增400错误场景，完善了各状态码的边界用例；
2. 格式优化：移除测试场景分类，直接按HTTP状态码归类，每条用例新增Mock Request/Response，可直接用于API测试执行；
3. 异常覆盖：除基础认证、权限异常外，补充了数据异常、依赖服务异常、路径错误、请求头格式错误等场景，确保全维度覆盖正向/异常/边界场景。