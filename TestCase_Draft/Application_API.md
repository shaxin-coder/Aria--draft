# Generate Temp Application ID API 测试用例（按Result Code分类）
## 一、Result_code = 0（Success - 201 Created）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| SUCC-001 | 正常生成临时应用ID（核心正向场景） | 1. 请求头包含合法的Accept（application/json）、Content-Type（application/json; charset=UTF-8）、Authorization（有效Bearer token）、X-Customer-Number（格式合法且绑定当前用户）；<br>2. 用户已完成账户关联；<br>3. 请求方法为POST，无请求体 | 1. HTTP方法为POST；<br>2. 请求头参数完整且格式符合要求；<br>3. 无请求体 | 1. HTTP状态码为201；<br>2. 响应体result为0；<br>3. tmp_app_id为合法UUID格式字符串 | 覆盖所有合法前置条件 |
| SUCC-002 | X-Customer-Number为最大长度合法值 | 1. X-Customer-Number为系统允许的最长合法字符串；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. X-Customer-Number长度达上限但格式合法；<br>3. 无请求体 | 1. HTTP状态码为201；<br>2. 响应体result为0；<br>3. tmp_app_id为合法UUID | 边界点：参数长度达上限 |
| SUCC-003 | X-Customer-Number为最小长度合法值（1位） | 1. X-Customer-Number为1位合法字符；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. X-Customer-Number长度为最小合法值；<br>3. 无请求体 | 1. HTTP状态码为201；<br>2. 响应体result为0；<br>3. tmp_app_id为合法UUID | 边界点：参数长度达下限 |
| SUCC-004 | 并发请求生成tmp_app_id | 1. 多线程同时发送相同合法请求（同一用户+同一X-Customer-Number）；<br>2. 所有请求头均合法；<br>3. 用户已完成账户关联；<br>4. 多个POST请求同时发起，无请求体 | 1. 所有请求HTTP方法为POST；<br>2. 请求头参数完整且格式合法；<br>3. 无请求体 | 1. 所有请求HTTP状态码为201；<br>2. 每个响应result为0；<br>3. 每个响应tmp_app_id为唯一UUID | 边界点：并发场景，验证tmp_app_id唯一性 |

## 二、Result_code = 1（Client Error - 400 Bad Request）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR1-001 | 缺失X-Customer-Number请求头 | 1. 请求头缺少X-Customer-Number；<br>2. 其他请求头（Accept/Content-Type/Authorization）均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 请求头缺失X-Customer-Number；<br>3. 无请求体 | 1. HTTP状态码为400；<br>2. 响应体result为1；<br>3. error_message为"Request parameter is invalid." | 边界点：必填请求头缺失 |
| ERR1-002 | X-Customer-Number为空字符串 | 1. X-Customer-Number值为空字符串；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. X-Customer-Number为空字符串；<br>3. 无请求体 | 1. HTTP状态码为400；<br>2. 响应体result为1；<br>3. error_message为"Request parameter is invalid." | 边界点：参数格式非法（空值） |
| ERR1-003 | X-Customer-Number含非法特殊字符 | 1. X-Customer-Number值为"JHF@123#"；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. X-Customer-Number含非法字符；<br>3. 无请求体 | 1. HTTP状态码为400；<br>2. 响应体result为1；<br>3. error_message为"Request parameter is invalid." | 边界点：参数格式非法（特殊字符） |
| ERR1-004 | Accept请求头值错误（非application/json） | 1. Accept值为"text/plain"；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. Accept值非法；<br>3. 无请求体 | 1. HTTP状态码为400；<br>2. 响应体result为1；<br>3. error_message为"Request parameter is invalid." | 边界点：Accept头值不符合要求 |
| ERR1-005 | Content-Type请求头值错误（非application/json） | 1. Content-Type值为"application/xml"；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. Content-Type值非法；<br>3. 无请求体 | 1. HTTP状态码为400；<br>2. 响应体result为1；<br>3. error_message为"Request parameter is invalid." | 边界点：Content-Type头值不符合要求 |
| ERR1-006 | Content-Type缺失charset=UTF-8 | 1. Content-Type值为"application/json"（无charset）；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. Content-Type缺少charset；<br>3. 无请求体 | 1. HTTP状态码为400；<br>2. 响应体result为1；<br>3. error_message为"Request parameter is invalid." | 边界点：Content-Type格式不完整 |

## 三、Result_code = 2（Internal Server Error - 500 Internal Server Error）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR2-001 | 服务端数据库连接失败 | 1. 所有请求头均合法；<br>2. 用户已完成账户关联；<br>3. 模拟后端数据库连接失败；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 请求头参数完整且格式合法；<br>3. 无请求体 | 1. HTTP状态码为500；<br>2. 响应体result为2；<br>3. error_message为"A system error has occurred." | 特殊点：服务端内部异常 |

## 四、Result_code = 3（Unauthorized - 401 Unauthorized）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR3-001 | 缺失Authorization请求头 | 1. 请求头缺少Authorization；<br>2. 其他请求头（Accept/Content-Type/X-Customer-Number）均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 缺失Authorization请求头；<br>3. 无请求体 | 1. HTTP状态码为401；<br>2. 响应体result为3；<br>3. error_message为"Authentication is required." | 边界点：认证头缺失 |
| ERR3-002 | Authorization格式错误（无Bearer前缀） | 1. Authorization值为"123456"；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. Authorization格式非法；<br>3. 无请求体 | 1. HTTP状态码为401；<br>2. 响应体result为3；<br>3. error_message为"Authentication is required." | 边界点：认证头格式错误 |
| ERR3-003 | Authorization中token已过期 | 1. Authorization值为"Bearer <过期的access_token>"；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. Authorization格式合法但token过期；<br>3. 无请求体 | 1. HTTP状态码为401；<br>2. 响应体result为3；<br>3. error_message为"Authentication is required." | 特殊点：token过期 |
| ERR3-004 | Authorization中token伪造 | 1. Authorization值为"Bearer <伪造的access_token>"；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. Authorization格式合法但token伪造；<br>3. 无请求体 | 1. HTTP状态码为401；<br>2. 响应体result为3；<br>3. error_message为"Authentication is required." | 特殊点：token伪造 |

## 五、Result_code = 4（Forbidden - 403 Forbidden）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR4-001 | 用户未完成账户关联 | 1. 所有请求头均合法；<br>2. 用户未完成账户关联；<br>3. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 请求头参数完整且格式合法；<br>3. 无请求体 | 1. HTTP状态码为403；<br>2. 响应体result为4；<br>3. error_message为"Please complete account linkage first." | 核心异常场景：前置条件不满足 |
| ERR4-002 | X-Customer-Number未绑定当前用户 | 1. X-Customer-Number格式合法但未关联到认证用户；<br>2. 其他请求头均合法；<br>3. 用户已完成账户关联（但该X-Customer-Number不属于用户）；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. X-Customer-Number格式合法；<br>3. 无请求体 | 1. HTTP状态码为403；<br>2. 响应体result为4；<br>3. error_message为"Please complete account linkage first." | 特殊点：参数格式合法但归属错误 |

## 六、Result_code = 5（Conflict - 409 Conflict）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR5-001 | tmp_app_id已被用于已提交的应用 | 1. 所有请求头均合法；<br>2. 用户已完成账户关联；<br>3. 模拟生成的tmp_app_id已被用于提交过的应用；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 请求头参数完整且格式合法；<br>3. 无请求体 | 1. HTTP状态码为409；<br>2. 响应体result为5；<br>3. error_message为"This application ID has already been used." | 特殊点：资源冲突 |

## 七、Result_code = 6（Service Unavailable - 503 Service Unavailable）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR6-001 | 服务暂时不可用（降级/维护） | 1. 所有请求头均合法；<br>2. 用户已完成账户关联；<br>3. 模拟服务降级/不可用；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 请求头参数完整且格式合法；<br>3. 无请求体 | 1. HTTP状态码为503；<br>2. 响应体result为6；<br>3. error_message为"The service is temporarily unavailable." | 特殊点：服务可用性异常 |

## 八、Result_code = 7（Gateway Timeout - 504 Gateway Timeout）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|------------------|----------------|----------------|---------------------|
| ERR7-001 | 网关/上游服务响应超时 | 1. 所有请求头均合法；<br>2. 用户已完成账户关联；<br>3. 模拟网关/上游服务响应超时；<br>4. POST请求，无请求体 | 1. HTTP方法为POST；<br>2. 请求头参数完整且格式合法；<br>3. 无请求体 | 1. HTTP状态码为504；<br>2. 响应体result为7；<br>3. error_message为"Request timed out." | 特殊点：请求超时异常 |