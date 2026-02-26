一、HTTP 200（成功场景）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
SC001	正常认证用户查询到多个有效活动	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中存在未申请 / 未审核通过的活动数据	1. 请求方法为 GET
2. 请求头包含有效 Authorization（Bearer + 合法 token）
3. 请求头 Accept=application/json、Content-Type=application/json	1. HTTP 状态码 200
2. Response Body 中 result=0
3. campaigns 数组包含多条活动数据
4. 每个活动字段（id/title/brief_eligibility/point_amount/required_documents/point_grant_date）均非空且格式合法	基础正向用例，验证核心返回结构
SC002	正常认证用户无可用活动	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中无符合条件的活动（所有活动已申请 / 审核通过）	1. 请求方法为 GET
2. 请求头包含有效 Authorization
3. 请求头 Accept/Content-Type 格式正确	1. HTTP 状态码 200
2. Response Body 中 result=0
3. campaigns 数组为空数组（[]）
4. 无 error_message 字段	边界场景：成功但无数据，区分于错误场景
SC003	活动字段取边界值（point_amount / 日期）	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中活动数据：
- point_amount=0（最小值）
- point_grant_date=1970-01-01（ISO 8601 最小日期）
- brief_eligibility/required_documents 为空数组	1. 请求参数 / 格式均合法	1. HTTP 状态码 200
2. result=0
3. 各边界字段返回值与 DB 一致：
- point_amount=0（Number 类型）
- point_grant_date="1970-01-01"（字符串）
- 空数组字段返回 []	验证字段边界值兼容性
SC004	活动字段含特殊字符（title/eligibility）	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中活动 title 含日文 / 特殊符号、brief_eligibility 含换行符	1. 请求格式合法	1. HTTP 状态码 200
2. result=0
3. 特殊字符无乱码、无截断
4. 换行符等控制字符正常返回	验证 UTF-8 编码兼容性
SC005	单条活动数据返回（边界：仅 1 条）	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中仅存在 1 条符合条件的活动数据	1. 请求格式合法	1. HTTP 状态码 200
2. result=0
3. campaigns 数组长度 = 1
4. 单条数据字段完整且格式合法	边界场景：仅返回 1 条数据
二、HTTP 401（Unauthorized）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
EC001	请求头无 Authorization 字段	1. 请求头仅含 Accept、Content-Type，无 Authorization
2. 账户关联状态任意	1. 请求方法为 GET
2. 请求头缺失 Authorization 字段	1. HTTP 状态码 401
2. result=1
3. error_message="Authentication is required."	基础认证缺失场景
EC002	Authorization 字段无 Bearer 前缀	1. 请求头 Authorization="invalid_token"（无 Bearer）
2. 账户关联状态任意	1. Authorization 字段无 "Bearer" 前缀	1. HTTP 状态码 401
2. result=1
3. error_message="Authentication is required."	格式错误场景
EC003	Bearer 后无 token（空值）	1. 请求头 Authorization="Bearer"
2. 账户关联状态任意	1. Authorization 字段为 "Bearer + 空字符串"	1. HTTP 状态码 401
2. result=1
3. error_message="Authentication is required."	边界场景：token 为空字符串
EC004	Bearer token 为无效值（随机字符串）	1. 请求头 Authorization="Bearer random123456"
2. 账户关联状态任意	1. Authorization 字段格式正确但 token 无效	1. HTTP 状态码 401
2. result=1
3. error_message="Authentication is required."	无效 token 场景
EC005	Bearer token 已过期	1. 请求头 Authorization="Bearer expired_token"（已失效的合法 token）
2. 账户关联状态任意	1. Authorization 字段格式正确但 token 已过期	1. HTTP 状态码 401
2. result=1
3. error_message="Authentication is required."	过期 token 场景
EC006	Authorization 字段格式正确但 token 被篡改	1. 请求头 Authorization 为篡改后的合法 token 格式
2. 账户关联状态任意	1. Authorization 格式正确但 token 内容非法	1. HTTP 状态码 401
2. result=1
3. error_message="Authentication is required."	篡改合法 token 的 payload / 签名场景
三、HTTP 403（Forbidden）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
EC007	有效 token 但账户关联未完成	1. 有效 Bearer Token
2. account_linkage 表中无该用户记录	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 403
2. result=2
3. error_message="Please complete account linkage first."	核心 403 场景（文档定义）
EC008	有效 token 但账户关联已失效	1. 有效 Bearer Token
2. account_linkage 表中该用户记录标记为失效	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 403
2. result=2
3. error_message="Please complete account linkage first."	账户关联失效边界场景
EC009	有效 token 但用户无访问活动列表权限	1. 有效 Bearer Token
2. 账户关联完成
3. 用户权限等级不足，无活动列表访问权限	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 403
2. result=2
3. error_message="User not allowed to access campaign list."	补充场景（文档定义的权限限制）
EC010	有效 token 但用户被封禁（黑名单）	1. 有效 Bearer Token
2. 账户关联完成
3. 用户在系统黑名单中	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 403
2. result=2
3. error_message="User not allowed to access campaign list."	补充权限限制场景
EC011	有效 token 但活动列表功能被禁用	1. 有效 Bearer Token
2. 账户关联完成
3. 活动列表功能被管理员全局 / 用户级禁用	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 403
2. result=2
3. error_message="User not allowed to access campaign list."	补充功能级权限限制场景
四、HTTP 500（Internal Server Error）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
EC012	DB 连接失败（模拟）	1. 有效 Bearer Token
2. 账户关联已完成
3. 人为断开 API 与 DB 的连接	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 500
2. result=3
3. error_message="A system error has occurred."	核心 500 场景（文档定义的 DB 异常）
EC013	SQL 查询语法错误（模拟）	1. 有效 Bearer Token
2. 账户关联已完成
3. 篡改查询 master_campaign 的 SQL 语句（语法错误）	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 500
2. result=3
3. error_message="A system error has occurred."	内部逻辑错误场景
EC014	活动数据字段类型异常	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中 point_amount 为字符串类型、date 字段为非 ISO 格式	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 500
2. result=3
3. error_message="A system error has occurred."	数据异常导致的内部服务错误
五、HTTP 503（Service Unavailable）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
EC015	服务过载 / 维护中（模拟）	1. 有效 Bearer Token
2. 账户关联已完成
3. 模拟服务实例全部下线 / 过载	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 503
2. result=4
3. error_message="The service is temporarily unavailable."	503 核心场景（文档定义）
EC016	依赖服务不可用	1. 有效 Bearer Token
2. 账户关联已完成
3. 活动数据依赖的下游服务下线	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 503
2. result=4
3. error_message="The service is temporarily unavailable."	补充依赖服务异常场景
六、HTTP 504（Gateway Timeout）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
EC017	后端服务响应超时（模拟）	1. 有效 Bearer Token
2. 账户关联已完成
3. 设置 API 调用后端服务的超时时间为 1ms	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 504
2. result=5
3. error_message="Request timed out."	504 核心场景（文档定义）
EC018	大数据量查询导致网关超时	1. 有效 Bearer Token
2. 账户关联已完成
3. DB 中存在超大量（如 10000 条）活动数据	1. 请求头 Authorization 有效
2. 请求格式合法	1. HTTP 状态码 504
2. result=5
3. error_message="Request timed out."	补充数据量过大引发的超时场景
七、HTTP 400（Bad Request）
表格
用例 ID	测试用例描述	请求参数 / 前置条件	预期请求检查点	预期响应检查点	备注（边界 / 特殊点）
EC019	请求头 Accept 字段格式错误	1. 有效 Bearer Token
2. 账户关联已完成
3. Accept 字段为 application/json;charset=GBK（非 UTF-8）	1. Authorization 有效
2. Accept 字段格式非法	1. HTTP 状态码 400
2. result=6
3. error_message="Invalid request header: Accept"	请求头格式错误场景
EC020	URL 路径错误（非 /campaign/list）	1. 有效 Bearer Token
2. 账户关联已完成
3. 请求路径为 /campaign/lists（多 s）或 /xxx/campaign/list	1. Authorization 有效
2. 请求路径非法	1. HTTP 状态码 400
2. result=6
3. error_message="Invalid request path"	路径错误场景，补充基础请求校验
EC021	请求头 Content-Type 字段错误	1. 有效 Bearer Token
2. 账户关联已完成
3. Content-Type=application/xml（非 json）	1. Authorization 有效
2. Content-Type 字段非法	1. HTTP 状态码 400
2. result=6
3. error_message="Invalid request header: Content-Type"