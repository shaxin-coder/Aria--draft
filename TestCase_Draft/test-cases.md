你是资深QA工程师，精通API测试。请基于以下Markdown格式的API Spec，设计全面的测试用例，覆盖：
- 正常场景（所有必填参数齐全，符合业务规则）
- 边界场景（参数为空、0、最大/最小值、特殊字符）
- 异常场景（参数缺失、类型错误、鉴权失败、权限不足）
- 业务规则场景（如重复创建用户、状态流转）

输出要求：
1. 每个接口单独列出测试用例
2. 每个测试用例包含：用例ID、用例名称、前置条件、操作步骤、预期结果
3. 用清晰的Markdown列表格式输出

---
以下是API Spec内容：
[此处粘贴你复制的api-spec.md内容]
# Retrieve Campaign List API 测试用例

## 1. 正常场景

- **用例ID:** TC01
    - **用例名称:** 正常获取可用活动列表
    - **前置条件:** 用户已完成账户关联，数据库中有可用活动，用户未全部申请
    - **操作步骤:**
        1. 使用有效 Bearer Token，发送 GET /campaign/list 请求，带正确的请求头
    - **预期结果:**
        - 返回 200 OK，result=0，campaigns 为活动数组，字段完整且符合 Spec

- **用例ID:** TC02
    - **用例名称:** 正常获取空活动列表
    - **前置条件:** 用户已完成账户关联，所有活动都已申请或无可用活动
    - **操作步骤:**
        1. 使用有效 Bearer Token，发送 GET /campaign/list 请求
    - **预期结果:**
        - 返回 200 OK，result=0，campaigns=[]

## 2. 边界场景

- **用例ID:** TC03
    - **用例名称:** Bearer Token 为空
    - **前置条件:** 无
    - **操作步骤:**
        1. Authorization 头为 "Bearer "（无 token），发送请求
    - **预期结果:**
        - 返回 401，result=1，error_message="Authentication is required."

- **用例ID:** TC04
    - **用例名称:** Bearer Token 为特殊字符
    - **前置条件:** 无
    - **操作步骤:**
        1. Authorization 头为 "Bearer !@#$%^&*()",发送请求
    - **预期结果:**
        - 返回 401，result=1，error_message="Authentication is required."

- **用例ID:** TC05
    - **用例名称:** Accept 头缺失
    - **前置条件:** 用户已完成账户关联
    - **操作步骤:**
        1. 不带 Accept 头，发送请求
    - **预期结果:**
        - 返回 200 OK 或 400（视实现），如为 200，返回正常数据

- **用例ID:** TC06
    - **用例名称:** Content-Type 头缺失
    - **前置条件:** 用户已完成账户关联
    - **操作步骤:**
        1. 不带 Content-Type 头，发送请求
    - **预期结果:**
        - 返回 200 OK 或 400（视实现），如为 200，返回正常数据

- **用例ID:** TC07
    - **用例名称:** 活动字段边界值（point_amount=0, 最大值）
    - **前置条件:** master_campaign 配置 point_amount=0 和极大值的活动
    - **操作步骤:**
        1. 用户请求活动列表
    - **预期结果:**
        - 返回活动列表，point_amount 字段为 0 和最大值，类型正确

- **用例ID:** TC08
    - **用例名称:** 活动标题/资格条件/文档含特殊字符
    - **前置条件:** master_campaign 配置含特殊字符的活动
    - **操作步骤:**
        1. 用户请求活动列表
    - **预期结果:**
        - 返回活动列表，title、brief_eligibility、required_documents 字段正确显示特殊字符

- **用例ID:** TC09
    - **用例名称:** point_grant_date 边界值
    - **前置条件:** master_campaign 配置 point_grant_date 为 "1970-01-01" 和 "9999-12-31"
    - **操作步骤:**
        1. 用户请求活动列表
    - **预期结果:**
        - 返回活动列表，point_grant_date 字段为上述日期，格式正确

## 3. 异常场景

- **用例ID:** TC10
    - **用例名称:** 未携带 Authorization 头
    - **前置条件:** 无
    - **操作步骤:**
        1. 不带 Authorization 头，发送请求
    - **预期结果:**
        - 返回 401，result=1，error_message="Authentication is required."

- **用例ID:** TC11
    - **用例名称:** Bearer Token 非法/过期
    - **前置条件:** 无
    - **操作步骤:**
        1. 使用无效或过期 token，发送请求
    - **预期结果:**
        - 返回 401，result=1，error_message="Authentication is required."

- **用例ID:** TC12
    - **用例名称:** 用户未完成账户关联
    - **前置条件:** account_linkage 无该用户记录
    - **操作步骤:**
        1. 使用有效 token，发送请求
    - **预期结果:**
        - 返回 403，result=2，error_message="Please complete account linkage first."

- **用例ID:** TC13
    - **用例名称:** 服务端数据库异常
    - **前置条件:** 数据库故障
    - **操作步骤:**
        1. 发送正常请求
    - **预期结果:**
        - 返回 500，result=3，error_message="A system error has occurred."

- **用例ID:** TC14
    - **用例名称:** 服务暂时不可用
    - **前置条件:** 服务维护中
    - **操作步骤:**
        1. 发送正常请求
    - **预期结果:**
        - 返回 503，result=4，error_message="The service is temporarily unavailable."

- **用例ID:** TC15
    - **用例名称:** 网关超时
    - **前置条件:** 后端超时
    - **操作步骤:**
        1. 发送正常请求
    - **预期结果:**
        - 返回 504，result=5，error_message="Request timed out."

## 4. 业务规则场景

- **用例ID:** TC16
    - **用例名称:** 已申请所有活动
    - **前置条件:** applications 表中该用户所有活动均为已申请状态
    - **操作步骤:**
        1. 发送正常请求
    - **预期结果:**
        - 返回 200 OK，result=0，campaigns=[]

- **用例ID:** TC17
    - **用例名称:** 新增活动后可见
    - **前置条件:** master_campaign 新增新活动，用户未申请
    - **操作步骤:**
        1. 发送正常请求
    - **预期结果:**
        - 返回 200 OK，campaigns 包含新活动

- **用例ID:** TC18
    - **用例名称:** 活动状态流转后不可见
    - **前置条件:** 用户对某活动已申请且状态为 "Under review"、"Final approved"、"Grant Done"
    - **操作步骤:**
        1. 发送正常请求
    - **预期结果:**
        - 返回 200 OK，campaigns 不包含该活动

- **用例ID:** TC19
    - **用例名称:** 多账户/客户号下活动隔离
    - **前置条件:** 用户有多个 customer_number，部分已申请，部分未申请
    - **操作步骤:**
        1. 以不同 customer_number 登录，分别请求
    - **预期结果:**
        - 各自返回各自未申请的活动列表

---

# Campaign List API - Test Cases (Table Format)

## Normal Scenarios

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| TC01 | Retrieve available campaigns successfully | User completed account linkage; DB has available campaigns; user hasn't applied to all | 1. Send GET /campaign/list with valid Bearer token and correct headers | 200 OK, result=0, campaigns array with complete fields matching spec |
| TC02 | Retrieve empty campaign list | User completed account linkage; all campaigns already applied or none available | 1. Send GET /campaign/list with valid Bearer token | 200 OK, result=0, campaigns=[] |

## Boundary Scenarios

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| TC03 | Bearer token empty | N/A | 1. Set Authorization header to "Bearer " (no token)<br>2. Send request | 401 Unauthorized, result=1, error_message="Authentication is required." |
| TC04 | Bearer token with special characters | N/A | 1. Set Authorization header to "Bearer !@#$%^&*()"<br>2. Send request | 401 Unauthorized, result=1, error_message="Authentication is required." |
| TC05 | Missing Accept header | User completed account linkage | 1. Send request without Accept header | 200 OK with normal data or 400 depending on implementation |
| TC06 | Missing Content-Type header | User completed account linkage | 1. Send request without Content-Type header | 200 OK with normal data or 400 depending on implementation |
| TC07 | Point amount boundary values | master_campaign configured with point_amount=0 and maximum values | 1. Request campaign list | 200 OK, campaigns with point_amount 0 and max value, correct type |
| TC08 | Special characters in campaign fields | master_campaign configured with special characters in title/eligibility/documents | 1. Request campaign list | 200 OK, special characters properly displayed in title, brief_eligibility, required_documents |
| TC09 | Point grant date boundary values | master_campaign configured with point_grant_date "1970-01-01" and "9999-12-31" | 1. Request campaign list | 200 OK, point_grant_date in ISO 8601 format (YYYY-MM-DD) |

## Error Scenarios

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| TC10 | Missing Authorization header | N/A | 1. Send GET /campaign/list without Authorization header | 401 Unauthorized, result=1, error_message="Authentication is required." |
| TC11 | Invalid or expired Bearer token | N/A | 1. Send request with invalid/expired token | 401 Unauthorized, result=1, error_message="Authentication is required." |
| TC12 | Account linkage not completed | No record in account_linkage table for user | 1. Send request with valid token | 403 Forbidden, result=2, error_message="Please complete account linkage first." |
| TC13 | Database connection error | Database unavailable | 1. Send normal request | 500 Internal Server Error, result=3, error_message="A system error has occurred." |
| TC14 | Service temporarily unavailable | Service under maintenance | 1. Send normal request | 503 Service Unavailable, result=4, error_message="The service is temporarily unavailable." |
| TC15 | Gateway timeout | Backend timeout | 1. Send normal request | 504 Gateway Timeout, result=5, error_message="Request timed out." |

## Business Rule Scenarios

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| TC16 | All campaigns already applied | User applied to all campaigns with status "Under review", "Final approved", or "Grant Done" | 1. Send GET /campaign/list with valid token | 200 OK, result=0, campaigns=[] |
| TC17 | New campaign visibility after creation | New campaign added to master_campaign; user hasn't applied | 1. Send GET /campaign/list | 200 OK, campaigns includes new campaign |
| TC18 | Campaign hidden after application approved | User has application with status "Under review", "Final approved", or "Grant Done" | 1. Send GET /campaign/list | 200 OK, campaign not in returned list |
| TC19 | Multi-account campaign isolation | User has multiple customer_number; some with applications, some without | 1. Request with each customer_number<br>2. Verify results differ | Each request returns only unapplied campaigns for that customer_number |
