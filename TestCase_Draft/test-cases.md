
# Campaign List API - Additional Test Cases

## HappyPath (正常场景)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| HP04 | Retrieve campaigns with single item | User completed account linkage; DB has 1 available campaign | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept: application/json<br>4. Include Content-Type: application/json | HTTP 200<br>result=0<br>campaigns array contains exactly 1 campaign object with all required fields |
| HP05 | Verify campaign id is Integer type | User completed account linkage; campaigns exist | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>Each campaign.id is Integer type (not string) |
| HP06 | Verify point_amount is Number type | User completed account linkage; campaigns exist | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>Each campaign.point_amount is Number type with decimal support |

## RequiredCheck (必需字段检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| RC06 | Invalid token format (too short) | N/A | 1. Set Authorization: "Bearer x"<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| RC07 | Null Authorization header value | N/A | 1. Set Authorization header to null/empty string<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |

## LengthCheck (长度和边界值检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| LC08 | Campaign id = 1 (minimum positive integer) | master_campaign contains campaign with id=1 | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Campaign with id=1 displayed as Integer |
| LC09 | Campaign id = maximum integer value (2147483647) | master_campaign contains campaign with id=2147483647 | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Campaign with maximum id displayed correctly |
| LC10 | brief_eligibility empty array | master_campaign contains campaign with brief_eligibility=[] | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Campaign displayed with empty brief_eligibility array |
| LC11 | required_documents empty array | master_campaign contains campaign with required_documents=[] | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Campaign displayed with empty required_documents array |
| LC12 | point_amount with decimal places (e.g., 1234.56) | master_campaign contains campaign with point_amount=1234.56 | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>point_amount preserved with decimal precision |

## ValidationCheck (格式和特殊字符检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| VC08 | Campaign title with CJK characters | master_campaign contains title: "新規ご契約特典キャンペーン" | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>Title correctly displays CJK characters without encoding issues |
| VC09 | brief_eligibility with mixed line breaks and special chars | master_campaign contains eligibility: ["Line1\n特別条件", "Line2\\特殊文字"] | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Array items preserve escape sequences and special characters |
| VC10 | required_documents id with special characters | master_campaign contains document with id="doc_001-特別" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Document id correctly displays with hyphen and CJK characters |
| VC11 | response result is not string type "0" | User completed account linkage | 1. Send GET /campaign/list<br>2. Parse JSON response | HTTP 200<br>result value is Integer 0, not string "0"<br>Type verification: typeof result === number |
| VC12 | campaigns field is Array not Object | User completed account linkage | 1. Send GET /campaign/list<br>2. Verify campaigns type | HTTP 200<br>campaigns field is Array type []<br>Not an Object {} |

## LogicCheck (业务逻辑检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| LGC09 | Only user's own applications hide campaigns (data isolation) | User1 applied to campaign 1; User2 hasn't applied | 1. Request with User1 token<br>2. Request with User2 token | User1: campaign 1 NOT in list<br>User2: campaign 1 IS in list |
| LGC10 | Application status case sensitivity check | applications table: campaign with status="under review" (lowercase) | 1. Send GET /campaign/list | HTTP 200<br>Verify behavior: campaign shown or hidden based on case handling<br>Spec requires exact match: "Under review" |
| LGC11 | Campaign shown if application has null/undefined status | applications table: record with campaign_id=6, status=NULL | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>campaign id=6 IS in returned campaigns array |
| LGC12 | Campaign display consistency across multiple requests | User completed linkage; no applications | 1. Send GET /campaign/list (Request 1)<br>2. Wait 1 second<br>3. Send GET /campaign/list (Request 2) | Both requests return identical campaigns array<br>Same order, same data |

## ErrorCheck (错误场景检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| EC11 | Bearer token with leading/trailing whitespace | N/A | 1. Set Authorization: "Bearer token123  "<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| EC12 | Case sensitivity of Bearer prefix | N/A | 1. Set Authorization: "bearer lowercase_token"<br>2. Send GET /campaign/list | HTTP 401 (if case-sensitive)<br>result=1<br>error_message="Authentication is required." |
| EC13 | Multiple Authorization headers | N/A | 1. Include 2 Authorization headers with different tokens<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| EC14 | Response error_message field missing on error | Backend error: 401 response omits error_message | 1. Send request with invalid token<br>2. Receive 401 response | HTTP 401<br>Verify response must contain error_message field as String |
| EC15 | Account linkage check uses correct customer_number | User has multiple customer_numbers; one without linkage | 1. Use token with unmapped customer_number<br>2. Send GET /campaign/list | HTTP 403<br>result=2<br>error_message="Please complete account linkage first." |
