# Generate Temp Application ID API 测试用例
## 一、功能测试用例
| 用例ID | 用例名称 | 测试步骤 | 预期结果 | 优先级 |
|--------|----------|----------|----------|--------|
| F001 | 正常生成临时应用ID | 1. 构造符合要求的POST请求，携带有效的Bearer token、合法且已关联用户的X-Customer-Number头，Accept和Content-Type为指定值；<br>2. 发送请求至`/member/api/v1/applications/init`；<br>3. 解析响应。 | 1. HTTP状态码为201；<br>2. 响应体包含`result: 0`和合法UUID格式的`tmp_app_id`；<br>3. 响应头格式正确。 | 高 |
| F002 | X-Customer-Number为空 | 1. 构造POST请求，Bearer token有效，X-Customer-Number头为空，其他头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 高 |
| F003 | X-Customer-Number格式非法（含特殊字符/超长） | 1. 构造POST请求，Bearer token有效，X-Customer-Number为包含特殊字符（如!@#$%）或长度超出系统限制的字符串，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |
| F004 | Bearer token缺失 | 1. 构造POST请求，不携带Authorization头，X-Customer-Number合法且已关联，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为401；<br>2. 响应体`result: 3`，`error_message: Authentication is required.`； | 高 |
| F005 | Bearer token无效（过期/伪造） | 1. 构造POST请求，携带过期/伪造的Bearer token，X-Customer-Number合法且已关联，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为401；<br>2. 响应体`result: 3`，`error_message: Authentication is required.`； | 高 |
| F006 | 用户未完成账户关联 | 1. 构造POST请求，Bearer token有效，X-Customer-Number合法但未关联至该用户，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为403；<br>2. 响应体`result: 4`，`error_message: Please complete account linkage first.`； | 高 |
| F007 | X-Customer-Number未关联至当前用户 | 1. 构造POST请求，Bearer token有效，X-Customer-Number属于其他用户，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为403；<br>2. 响应体`result: 4`，`error_message: Please complete account linkage first.`； | 高 |
| F008 | 临时应用ID已被用于已提交的应用 | 1. 构造请求触发系统判定`tmp_app_id`已被用于提交的应用（模拟该场景）；<br>2. 发送合法请求；<br>3. 解析响应。 | 1. HTTP状态码为409；<br>2. 响应体`result: 5`，`error_message: This application ID has already been used.`； | 中 |
| F009 | Accept头缺失 | 1. 构造POST请求，Bearer token和X-Customer-Number合法，不携带Accept头，Content-Type合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |
| F010 | Accept头值非法（非application/json） | 1. 构造POST请求，Accept头为`text/plain`，其他头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |
| F011 | Content-Type头缺失 | 1. 构造POST请求，不携带Content-Type头，其他头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |
| F012 | Content-Type头值非法（非application/json） | 1. 构造POST请求，Content-Type头为`application/xml`，其他头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |
| F013 | 请求体非空（携带多余数据） | 1. 构造POST请求，携带非空请求体（如`{"key":"value"}`），其他头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为201；<br>2. 正常返回`result:0`和`tmp_app_id`（忽略请求体）； | 低 |

## 二、异常场景测试用例
| 用例ID | 用例名称 | 测试步骤 | 预期结果 | 优先级 |
|--------|----------|----------|----------|--------|
| E001 | 服务端数据库连接失败 | 1. 模拟后端数据库连接异常；<br>2. 发送合法请求；<br>3. 解析响应。 | 1. HTTP状态码为500；<br>2. 响应体`result: 2`，`error_message: A system error has occurred.`； | 中 |
| E002 | 服务临时不可用 | 1. 模拟服务端503状态（如服务熔断/限流）；<br>2. 发送合法请求；<br>3. 解析响应。 | 1. HTTP状态码为503；<br>2. 响应体`result: 6`，`error_message: The service is temporarily unavailable.`； | 中 |
| E003 | 网关/上游超时 | 1. 模拟服务端网关超时场景；<br>2. 发送合法请求；<br>3. 解析响应。 | 1. HTTP状态码为504；<br>2. 响应体`result: 7`，`error_message: Request timed out.`； | 中 |
| E004 | 高并发请求生成tmp_app_id | 1. 模拟大量并发请求（如1000+）调用该API；<br>2. 验证每个合法请求的响应。 | 1. 所有合法请求均返回201和唯一的`tmp_app_id`；<br>2. 无重复`tmp_app_id`；<br>3. 无服务异常。 | 低 |
| E005 | X-Customer-Number为全空格 | 1. 构造POST请求，X-Customer-Number为全空格字符串，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |
| E006 | Authorization头格式错误（无Bearer前缀） | 1. 构造POST请求，Authorization头为`xxx_token`（无Bearer），其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为401；<br>2. 响应体`result: 3`，`error_message: Authentication is required.`； | 高 |
| E007 | Authorization头Bearer后无token | 1. 构造POST请求，Authorization头为`Bearer `（空格后无内容），其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为401；<br>2. 响应体`result: 3`，`error_message: Authentication is required.`； | 高 |

## 三、边界值测试用例
| 用例ID | 用例名称 | 测试步骤 | 预期结果 | 优先级 |
|--------|----------|----------|----------|--------|
| B001 | X-Customer-Number为最小长度（如1位） | 1. 构造POST请求，X-Customer-Number为1位合法字符，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. 若该值已关联用户：HTTP 201，返回`result:0`和`tmp_app_id`；<br>2. 若未关联：HTTP 403，`result:4`； | 中 |
| B002 | X-Customer-Number为最大长度（系统限制值） | 1. 构造POST请求，X-Customer-Number为系统允许的最大长度合法字符，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. 若该值已关联用户：HTTP 201，返回`result:0`和`tmp_app_id`；<br>2. 若未关联：HTTP 403，`result:4`； | 中 |
| B003 | X-Customer-Number为最大长度+1（超长） | 1. 构造POST请求，X-Customer-Number长度超出系统限制，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400；<br>2. 响应体`result: 1`，`error_message: Request parameter is invalid.`； | 中 |

## 四、兼容性测试用例
| 用例ID | 用例名称 | 测试步骤 | 预期结果 | 优先级 |
|--------|----------|----------|----------|--------|
| C001 | 不同字符编码的Content-Type（UTF-8显式声明） | 1. 构造POST请求，Content-Type为`application/json; charset=UTF-8`，其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为201；<br>2. 响应体包含`result: 0`和`tmp_app_id`； | 低 |
| C002 | HTTP 1.0协议请求 | 1. 构造HTTP 1.0协议的POST请求，头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为201；<br>2. 响应体包含`result: 0`和`tmp_app_id`； | 低 |
| C003 | 携带额外非必要请求头 | 1. 构造POST请求，携带自定义非必要头（如`X-Test: 123`），其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为201；<br>2. 响应体包含`result: 0`和`tmp_app_id`； | 低 |

## 五、性能测试用例
| 用例ID | 用例名称 | 测试步骤 | 预期结果 | 优先级 |
|--------|----------|----------|----------|--------|
| P001 | 单用户高频次调用（100次/秒） | 1. 单用户身份下，以100次/秒频率调用API；<br>2. 持续1分钟；<br>3. 统计响应状态和耗时。 | 1. 响应成功率≥99%；<br>2. 平均响应耗时≤200ms；<br>3. 无5xx错误（除预期的503/504）； | 低 |
| P002 | 多用户并发调用（100用户×10次/秒） | 1. 100个不同合法用户，各以10次/秒频率调用API；<br>2. 持续1分钟；<br>3. 统计响应状态和耗时。 | 1. 响应成功率≥99%；<br>2. 平均响应耗时≤500ms；<br>3. 无重复`tmp_app_id`生成； | 低 |

## 六、安全测试用例
| 用例ID | 用例名称 | 测试步骤 | 预期结果 | 优先级 |
|--------|----------|----------|----------|--------|
| S001 | X-Customer-Number注入攻击（SQL注入字符） | 1. 构造POST请求，X-Customer-Number为SQL注入字符（如`' OR 1=1 --`），其他头合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. HTTP状态码为400/403（根据是否匹配合法格式）；<br>2. 无数据库注入风险，响应无敏感信息泄露； | 中 |
| S002 | 跨站请求伪造（CSRF）模拟 | 1. 构造含CSRF攻击特征的请求，头信息合法；<br>2. 发送请求；<br>3. 解析响应。 | 1. 正常鉴权流程生效，无未授权操作；<br>2. 响应状态符合鉴权规则； | 中 |
| S003 | 敏感信息泄露检查 | 1. 发送各类合法/非法请求；<br>2. 检查响应头和响应体是否包含敏感信息（如数据库地址、token密钥、用户明文信息）。 | 1. 响应中无任何敏感信息泄露；<br>2. 错误信息仅返回标准化提示，无内部细节； | 高 |