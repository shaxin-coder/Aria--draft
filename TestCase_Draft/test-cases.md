
# Campaign List API - Comprehensive Test Cases

## HappyPath (正常场景)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| HP01 | Retrieve available campaigns successfully | User completed account linkage; DB has 2+ available campaigns; user hasn't applied to all | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept: application/json<br>4. Include Content-Type: application/json | HTTP 200<br>result=0<br>campaigns array with id, title, brief_eligibility (array), point_amount, required_documents (array), point_grant_date in YYYY-MM-DD format |
| HP02 | Retrieve empty campaign list (no eligible campaigns) | User completed account linkage; all campaigns already applied with status "Under review", "Final approved", or "Grant Done" | 1. Send GET /campaign/list with valid Bearer token and headers | HTTP 200<br>result=0<br>campaigns=[] (empty array) |
| HP03 | Retrieve campaigns with multiple documents | User completed account linkage; campaigns have multiple required_documents | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>Each campaign's required_documents contains multiple objects with id and title |

## RequiredCheck (必需字段检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| RC01 | Missing Authorization header | N/A | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include other required headers | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| RC02 | Missing Accept header | User completed account linkage | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type | HTTP 200 or 400 (implementation-dependent)<br>If 200: return valid campaign data |
| RC03 | Missing Content-Type header | User completed account linkage | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept | HTTP 200 or 400 (implementation-dependent)<br>If 200: return valid campaign data |
| RC04 | Empty Bearer token value | N/A | 1. Set Authorization: "Bearer "<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| RC05 | Bearer token without "Bearer " prefix | N/A | 1. Set Authorization: "token_value_only"<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |

## LengthCheck (长度和边界值检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| LC01 | point_amount = 0 | master_campaign contains campaign with point_amount=0 | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>Campaign with point_amount=0 displayed correctly as Number type |
| LC02 | point_amount = maximum value (999999999) | master_campaign contains campaign with point_amount=999999999 | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Campaign with maximum point_amount displayed correctly |
| LC03 | Campaign title with maximum length | master_campaign contains campaign with very long title (255+ chars) | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Full title returned without truncation |
| LC04 | brief_eligibility with many conditions (10+ items) | master_campaign contains campaign with 10+ eligibility conditions | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>All brief_eligibility items returned in array |
| LC05 | required_documents with many items (5+ docs) | master_campaign contains campaign requiring 5+ documents | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>All required_documents items returned with id and title |
| LC06 | point_grant_date = "1970-01-01" (minimum date) | master_campaign contains campaign with point_grant_date="1970-01-01" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>point_grant_date="1970-01-01" in YYYY-MM-DD format |
| LC07 | point_grant_date = "9999-12-31" (maximum date) | master_campaign contains campaign with point_grant_date="9999-12-31" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>point_grant_date="9999-12-31" in YYYY-MM-DD format |

## ValidationCheck (格式和特殊字符检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| VC01 | Bearer token with special characters | N/A | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| VC02 | Campaign title with special characters (emoji, symbols) | master_campaign contains title with emoji and symbols: "特典🎉!@#$%" | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>Title correctly displays: "特典🎉!@#$%" |
| VC03 | brief_eligibility with special characters and line breaks | master_campaign contains eligibility with \n and special chars | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>brief_eligibility array items preserve special characters and formatting |
| VC04 | required_documents title with special characters | master_campaign contains document title: "融資実行確認書類（#001）" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>Document title correctly displays with parentheses and numbers |
| VC05 | point_grant_date invalid format (non-ISO 8601) | N/A (server-side validation) | 1. Verify response point_grant_date | HTTP 200<br>All point_grant_date values strictly match YYYY-MM-DD format<br>No timestamps or other formats |
| VC06 | campaign id format validation | master_campaign contains campaigns with id=1, id=999, id=1000000 | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>All campaign ids are returned as Integer type |
| VC07 | Response result field = 0 for success | User completed account linkage; valid request | 1. Send GET /campaign/list | HTTP 200<br>result field value is exactly 0 (Integer, not string "0") |

## LogicCheck (业务逻辑检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| LGC01 | Campaign hidden if user has approved application | applications table: user has campaign_id=1 with status="Final approved" | 1. Send GET /campaign/list with valid token | HTTP 200<br>result=0<br>campaign id=1 NOT in returned campaigns array |
| LGC02 | Campaign hidden if user has "Under review" application | applications table: user has campaign_id=2 with status="Under review" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>campaign id=2 NOT in returned campaigns array |
| LGC03 | Campaign hidden if user has "Grant Done" application | applications table: user has campaign_id=3 with status="Grant Done" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>campaign id=3 NOT in returned campaigns array |
| LGC04 | Campaign shown if user has "Rejected" application | applications table: user has campaign_id=4 with status="Rejected" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>campaign id=4 IS in returned campaigns array |
| LGC05 | Campaign shown if user has "Withdrawn" application | applications table: user has campaign_id=5 with status="Withdrawn" | 1. Send GET /campaign/list | HTTP 200<br>result=0<br>campaign id=5 IS in returned campaigns array |
| LGC06 | New campaign appears after insertion | master_campaign: insert new campaign with id=100, title="New Campaign" | 1. Send GET /campaign/list before insert<br>2. Insert new campaign<br>3. Send GET /campaign/list after insert | First response: campaign id=100 not present<br>Second response: campaign id=100 present with all required fields |
| LGC07 | Multiple customer_number isolation | User has 2 customer_numbers; customer_A applied to campaign 1; customer_B hasn't applied | 1. Request with customer_A token<br>2. Request with customer_B token | customer_A response: campaign 1 NOT in list<br>customer_B response: campaign 1 IS in list |
| LGC08 | Campaigns ordered by id ascending | master_campaign contains campaigns with ids: 5, 2, 10, 1 | 1. Send GET /campaign/list | HTTP 200<br>campaigns array returned in ascending order by id: 1, 2, 5, 10 |

## ErrorCheck (错误场景检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| EC01 | Invalid or expired Bearer token | N/A | 1. Use expired/invalid token<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| EC02 | Account linkage not completed | account_linkage table: no record for this user's easy_id and customer_number | 1. Use valid token but no linkage<br>2. Send GET /campaign/list | HTTP 403<br>result=2<br>error_message="Please complete account linkage first." |
| EC03 | Database connection error | Database is unavailable/offline | 1. Send GET /campaign/list<br>2. Database connection fails | HTTP 500<br>result=3<br>error_message="A system error has occurred." |
| EC04 | Service temporarily unavailable | Service is under maintenance/deployment | 1. Send GET /campaign/list | HTTP 503<br>result=4<br>error_message="The service is temporarily unavailable." |
| EC05 | Gateway or upstream timeout | Backend service is slow/unresponsive (>timeout threshold) | 1. Send GET /campaign/list | HTTP 504<br>result=5<br>error_message="Request timed out." |
| EC06 | Malformed Authorization header (missing space) | Authorization header = "Bearertoken123" (no space) | 1. Set Authorization: "Bearertoken123"<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| EC07 | Wrong authentication scheme | Authorization header = "Basic base64encodedtoken" | 1. Set Authorization: "Basic base64string"<br>2. Send GET /campaign/list | HTTP 401<br>result=1<br>error_message="Authentication is required." |
| EC08 | Response structure validation - missing result field | Backend error: response omits result field | 1. Send GET /campaign/list | HTTP 200 (but invalid structure)<br>Verify response must contain result field as Integer |
| EC09 | Response structure validation - missing campaigns field on success | Backend error: 200 response omits campaigns field | 1. Send GET /campaign/list with valid token | HTTP 200 (but invalid)<br>Verify response must contain campaigns array field |
| EC10 | Response structure validation - error response has both result and campaigns | Backend error: 401 response includes both result and campaigns | 1. Send request with invalid token<br>2. Receive 401 response | HTTP 401<br>Response contains result and error_message<br>Response should NOT contain campaigns field |





# Campaign List API - Comprehensive Test Cases

## HappyPath (正常场景)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| HP01 | Retrieve available campaigns successfully | 1. User completed account linkage<br>2. DB has 2+ available campaigns<br>3. User hasn't applied to all | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept: application/json<br>4. Include Content-Type: application/json | 1. HTTP 200<br>2. result=0<br>3. campaigns array with id, title, brief_eligibility (array), point_amount, required_documents (array), point_grant_date in YYYY-MM-DD format |
| HP02 | Retrieve empty campaign list (no eligible campaigns) | 1. User completed account linkage<br>2. All campaigns already applied with status "Under review", "Final approved", or "Grant Done" | 1. Send GET /campaign/list with valid Bearer token and headers | 1. HTTP 200<br>2. result=0<br>3. campaigns=[] (empty array) |
| HP03 | Retrieve campaigns with multiple documents | 1. User completed account linkage<br>2. Campaigns have multiple required_documents | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Each campaign's required_documents contains multiple objects with id and title |

## RequiredCheck (必需字段检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| RC01 | Missing Authorization header | 1. N/A | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include other required headers | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |
| RC02 | Missing Accept header | 1. User completed account linkage | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type | 1. HTTP 200 or 400<br>2. If 200: return valid campaign data |
| RC03 | Missing Content-Type header | 1. User completed account linkage | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept | 1. HTTP 200 or 400<br>2. If 200: return valid campaign data |
| RC04 | Empty Bearer token value | 1. N/A | 1. Set Authorization: "Bearer "<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |
| RC05 | Bearer token without "Bearer " prefix | 1. N/A | 1. Set Authorization: "token_value_only"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |

## LengthCheck (长度和边界值检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| LC01 | point_amount = 0 | 1. master_campaign contains campaign with point_amount=0 | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Campaign with point_amount=0 displayed correctly as Number type |
| LC02 | point_amount = maximum value (999999999) | 1. master_campaign contains campaign with point_amount=999999999 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Campaign with maximum point_amount displayed correctly |
| LC03 | Campaign title with maximum length | 1. master_campaign contains campaign with very long title (255+ chars) | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Full title returned without truncation |
| LC04 | brief_eligibility with many conditions (10+ items) | 1. master_campaign contains campaign with 10+ eligibility conditions | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All brief_eligibility items returned in array |
| LC05 | required_documents with many items (5+ docs) | 1. master_campaign contains campaign requiring 5+ documents | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All required_documents items returned with id and title |
| LC06 | point_grant_date = "1970-01-01" (minimum date) | 1. master_campaign contains campaign with point_grant_date="1970-01-01" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="1970-01-01" in YYYY-MM-DD format |
| LC07 | point_grant_date = "9999-12-31" (maximum date) | 1. master_campaign contains campaign with point_grant_date="9999-12-31" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="9999-12-31" in YYYY-MM-DD format |

## ValidationCheck (格式和特殊字符检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| VC01 | Bearer token with special characters | 1. N/A | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |
| VC02 | Campaign title with special characters (emoji, symbols) | 1. master_campaign contains title with emoji and symbols: "特典🎉!@#$%" | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Title correctly displays: "特典🎉!@#$%" |
| VC03 | brief_eligibility with special characters and line breaks | 1. master_campaign contains eligibility with \n and special chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. brief_eligibility array items preserve special characters and formatting |
| VC04 | required_documents title with special characters | 1. master_campaign contains document title: "融資実行確認書類（#001）" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Document title correctly displays with parentheses and numbers |
| VC05 | point_grant_date invalid format (non-ISO 8601) | 1. N/A (server-side validation) | 1. Verify response point_grant_date | 1. HTTP 200<br>2. All point_grant_date values strictly match YYYY-MM-DD format<br>3. No timestamps or other formats |
| VC06 | campaign id format validation | 1. master_campaign contains campaigns with id=1, id=999, id=1000000 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All campaign ids are returned as Integer type |
| VC07 | Response result field = 0 for success | 1. User completed account linkage<br>2. Valid request | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result field value is exactly 0 (Integer, not string "0") |

## LogicCheck (业务逻辑检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| LGC01 | Campaign hidden if user has approved application | 1. applications table: user has campaign_id=1 with status="Final approved" | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. campaign id=1 NOT in returned campaigns array |
| LGC02 | Campaign hidden if user has "Under review" application | 1. applications table: user has campaign_id=2 with status="Under review" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=2 NOT in returned campaigns array |
| LGC03 | Campaign hidden if user has "Grant Done" application | 1. applications table: user has campaign_id=3 with status="Grant Done" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=3 NOT in returned campaigns array |
| LGC04 | Campaign shown if user has "Rejected" application | 1. applications table: user has campaign_id=4 with status="Rejected" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=4 IS in returned campaigns array |
| LGC05 | Campaign shown if user has "Withdrawn" application | 1. applications table: user has campaign_id=5 with status="Withdrawn" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=5 IS in returned campaigns array |
| LGC06 | New campaign appears after insertion | 1. master_campaign: insert new campaign with id=100, title="New Campaign" | 1. Send GET /campaign/list before insert<br>2. Insert new campaign<br>3. Send GET /campaign/list after insert | 1. First response: campaign id=100 not present<br>2. Second response: campaign id=100 present with all required fields |
| LGC07 | Multiple customer_number isolation | 1. User has 2 customer_numbers<br>2. customer_A applied to campaign 1<br>3. customer_B hasn't applied | 1. Request with customer_A token<br>2. Request with customer_B token | 1. customer_A response: campaign 1 NOT in list<br>2. customer_B response: campaign 1 IS in list |
| LGC08 | Campaigns ordered by id ascending | 1. master_campaign contains campaigns with ids: 5, 2, 10, 1 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. campaigns array returned in ascending order by id: 1, 2, 5, 10 |

## ErrorCheck (错误场景检查)

| Test Case ID | Test Name | Preconditions | Steps | Expected Result |
|---|---|---|---|---|
| EC01 | Invalid or expired Bearer token | 1. N/A | 1. Use expired/invalid token<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |
| EC02 | Account linkage not completed | 1. account_linkage table: no record for this user's easy_id and customer_number | 1. Use valid token but no linkage<br>2. Send GET /campaign/list | 1. HTTP 403<br>2. result=2<br>3. error_message="Please complete account linkage first." |
| EC03 | Database connection error | 1. Database is unavailable/offline | 1. Send GET /campaign/list<br>2. Database connection fails | 1. HTTP 500<br>2. result=3<br>3. error_message="A system error has occurred." |
| EC04 | Service temporarily unavailable | 1. Service is under maintenance/deployment | 1. Send GET /campaign/list | 1. HTTP 503<br>2. result=4<br>3. error_message="The service is temporarily unavailable." |
| EC05 | Gateway or upstream timeout | 1. Backend service is slow/unresponsive (>timeout threshold) | 1. Send GET /campaign/list | 1. HTTP 504<br>2. result=5<br>3. error_message="Request timed out." |
| EC06 | Malformed Authorization header (missing space) | 1. Authorization header = "Bearertoken123" (no space) | 1. Set Authorization: "Bearertoken123"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |
| EC07 | Wrong authentication scheme | 1. Authorization header = "Basic base64encodedtoken" | 1. Set Authorization: "Basic base64string"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." |
| EC08 | Response structure validation - missing result field | 1. Backend error: response omits result field | 1. Send GET /campaign/list | 1. HTTP 200 (but invalid structure)<br>2. Verify response must contain result field as Integer |
| EC09 | Response structure validation - missing campaigns field on success | 1. Backend error: 200 response omits campaigns field | 1. Send GET /campaign/list with valid token | 1. HTTP 200 (but invalid)<br>2. Verify response must contain campaigns array field |
| EC10 | Response structure validation - error response has both result and campaigns | 1. Backend error: 401 response includes both result and campaigns | 1. Send request with invalid token<br>2. Receive 401 response | 1. HTTP 401<br>2. Response contains result and error_message<br>3. Response should NOT contain campaigns field |
