# Campaign List API 测试用例设计
## 一、HTTP 200（成功场景）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| SC001 | 正常认证用户查询到多个有效活动 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中存在未申请/未审核通过的活动数据 | 1. 请求方法为GET<br>2. 请求头包含有效Authorization（Bearer + 合法token）<br>3. 请求头Accept=application/json、Content-Type=application/json | 1. HTTP状态码200<br>2. Response Body中result=0<br>3. campaigns数组包含多条活动数据<br>4. 每个活动字段（id/title/brief_eligibility/point_amount/required_documents/point_grant_date）均非空且格式合法 | 基础正向用例，验证核心返回结构 |
| SC002 | 正常认证用户无可用活动 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中无符合条件的活动（所有活动已申请/审核通过） | 1. 请求方法为GET<br>2. 请求头包含有效Authorization<br>3. 请求头Accept/Content-Type格式正确 | 1. HTTP状态码200<br>2. Response Body中result=0<br>3. campaigns数组为空数组（[]）<br>4. 无error_message字段 | 边界场景：成功但无数据，区分于错误场景 |
| SC003 | 活动字段取边界值（point_amount/日期） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中活动数据：<br>   - point_amount=0（最小值）<br>   - point_grant_date=1970-01-01（ISO 8601最小日期）<br>   - brief_eligibility/required_documents为空数组 | 1. 请求参数/格式均合法 | 1. HTTP状态码200<br>2. result=0<br>3. 各边界字段返回值与DB一致：<br>   - point_amount=0（Number类型）<br>   - point_grant_date="1970-01-01"（字符串）<br>   - 空数组字段返回[] | 验证字段边界值兼容性 |
| SC004 | 活动字段含特殊字符（title/eligibility） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中活动title含日文/特殊符号、brief_eligibility含换行符 | 1. 请求格式合法 | 1. HTTP状态码200<br>2. result=0<br>3. 特殊字符无乱码、无截断<br>4. 换行符等控制字符正常返回 | 验证UTF-8编码兼容性 |
| SC005 | 单条活动数据返回（边界：仅1条） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中仅存在1条符合条件的活动数据 | 1. 请求格式合法 | 1. HTTP状态码200<br>2. result=0<br>3. campaigns数组长度=1<br>4. 单条数据字段完整且格式合法 | 边界场景：仅返回1条数据 |

## 二、HTTP 401（Unauthorized）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| EC001 | 请求头无Authorization字段 | 1. 请求头仅含Accept、Content-Type，无Authorization<br>2. 账户关联状态任意 | 1. 请求方法为GET<br>2. 请求头缺失Authorization字段 | 1. HTTP状态码401<br>2. result=1<br>3. error_message="Authentication is required." | 基础认证缺失场景 |
| EC002 | Authorization字段无Bearer前缀 | 1. 请求头Authorization="invalid_token"（无Bearer）<br>2. 账户关联状态任意 | 1. Authorization字段无"Bearer "前缀 | 1. HTTP状态码401<br>2. result=1<br>3. error_message="Authentication is required." | 格式错误场景 |
| EC003 | Bearer后无token（空值） | 1. 请求头Authorization="Bearer "<br>2. 账户关联状态任意 | 1. Authorization字段为"Bearer + 空字符串" | 1. HTTP状态码401<br>2. result=1<br>3. error_message="Authentication is required." | 边界场景：token为空字符串 |
| EC004 | Bearer token为无效值（随机字符串） | 1. 请求头Authorization="Bearer random123456"<br>2. 账户关联状态任意 | 1. Authorization字段格式正确但token无效 | 1. HTTP状态码401<br>2. result=1<br>3. error_message="Authentication is required." | 无效token场景 |
| EC005 | Bearer token已过期 | 1. 请求头Authorization="Bearer expired_token"（已失效的合法token）<br>2. 账户关联状态任意 | 1. Authorization字段格式正确但token已过期 | 1. HTTP状态码401<br>2. result=1<br>3. error_message="Authentication is required." | 过期token场景 |
| EC006 | Authorization字段格式正确但token被篡改 | 1. 请求头Authorization为篡改后的合法token格式<br>2. 账户关联状态任意 | 1. Authorization格式正确但token内容非法 | 1. HTTP状态码401<br>2. result=1<br>3. error_message="Authentication is required." | 篡改合法token的payload/签名场景 |

## 三、HTTP 403（Forbidden）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| EC007 | 有效token但账户关联未完成 | 1. 有效Bearer Token<br>2. account_linkage表中无该用户记录 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码403<br>2. result=2<br>3. error_message="Please complete account linkage first." | 核心403场景（文档定义） |
| EC008 | 有效token但账户关联已失效 | 1. 有效Bearer Token<br>2. account_linkage表中该用户记录标记为失效 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码403<br>2. result=2<br>3. error_message="Please complete account linkage first." | 账户关联失效边界场景 |
| EC009 | 有效token但用户无访问活动列表权限 | 1. 有效Bearer Token<br>2. 账户关联完成<br>3. 用户权限等级不足，无活动列表访问权限 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码403<br>2. result=2<br>3. error_message="User not allowed to access campaign list." | 补充场景（文档定义的权限限制） |
| EC010 | 有效token但用户被封禁（黑名单） | 1. 有效Bearer Token<br>2. 账户关联完成<br>3. 用户在系统黑名单中 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码403<br>2. result=2<br>3. error_message="User not allowed to access campaign list." | 补充权限限制场景 |
| EC011 | 有效token但活动列表功能被禁用 | 1. 有效Bearer Token<br>2. 账户关联完成<br>3. 活动列表功能被管理员全局/用户级禁用 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码403<br>2. result=2<br>3. error_message="User not allowed to access campaign list." | 补充功能级权限限制场景 |

## 四、HTTP 500（Internal Server Error）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| EC012 | DB连接失败（模拟） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. 人为断开API与DB的连接 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码500<br>2. result=3<br>3. error_message="A system error has occurred." | 核心500场景（文档定义的DB异常） |
| EC013 | SQL查询语法错误（模拟） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. 篡改查询master_campaign的SQL语句（语法错误） | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码500<br>2. result=3<br>3. error_message="A system error has occurred." | 内部逻辑错误场景 |
| EC014 | 活动数据字段类型异常 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中point_amount为字符串类型、date字段为非ISO格式 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码500<br>2. result=3<br>3. error_message="A system error has occurred." | 数据异常导致的内部服务错误 |

## 五、HTTP 503（Service Unavailable）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| EC015 | 服务过载/维护中（模拟） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. 模拟服务实例全部下线/过载 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码503<br>2. result=4<br>3. error_message="The service is temporarily unavailable." | 503核心场景（文档定义） |
| EC016 | 依赖服务不可用 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. 活动数据依赖的下游服务下线 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码503<br>2. result=4<br>3. error_message="The service is temporarily unavailable." | 补充依赖服务异常场景 |

## 六、HTTP 504（Gateway Timeout）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| EC017 | 后端服务响应超时（模拟） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. 设置API调用后端服务的超时时间为1ms | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码504<br>2. result=5<br>3. error_message="Request timed out." | 504核心场景（文档定义） |
| EC018 | 大数据量查询导致网关超时 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. DB中存在超大量（如10000条）活动数据 | 1. 请求头Authorization有效<br>2. 请求格式合法 | 1. HTTP状态码504<br>2. result=5<br>3. error_message="Request timed out." | 补充数据量过大引发的超时场景 |

## 七、HTTP 400（Bad Request）
| 用例ID | 测试用例描述 | 请求参数/前置条件 | 预期请求检查点 | 预期响应检查点 | 备注（边界/特殊点） |
|--------|--------------|-------------------|----------------|----------------|---------------------|
| EC019 | 请求头Accept字段格式错误 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. Accept字段为application/json;charset=GBK（非UTF-8） | 1. Authorization有效<br>2. Accept字段格式非法 | 1. HTTP状态码400<br>2. result=6<br>3. error_message="Invalid request header: Accept" | 请求头格式错误场景 |
| EC020 | URL路径错误（非/campaign/list） | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. 请求路径为/campaign/lists（多s）或/xxx/campaign/list | 1. Authorization有效<br>2. 请求路径非法 | 1. HTTP状态码400<br>2. result=6<br>3. error_message="Invalid request path" | 路径错误场景，补充基础请求校验 |
| EC021 | 请求头Content-Type字段错误 | 1. 有效Bearer Token<br>2. 账户关联已完成<br>3. Content-Type=application/xml（非json） | 1. Authorization有效<br>2. Content-Type字段非法 | 1. HTTP状态码400<br>2. result=6<br>3. error_message="Invalid request header: Content-Type" | 补充请求头核心字段校验场景 |

