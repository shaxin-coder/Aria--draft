

## Test Case Summary Matrix

Case status：
- Not Run 
- Pass
- Fail 
- Blocked 
- OutOfScope

Priority:
- Critical
- High
- Medium
- Low

Option1:

| Module / API Name  | Scenario        | Test Description                                             | Case ID | Priority | Case Count |
| ------------------ | --------------- | ------------------------------------------------------------ | ------- | -------- | ---------- |
| GET /campaign/list | HappyPath       | Retrieve campaigns successfully with valid token and linkage | HP01    | High     | 3          |
| GET /campaign/list | HappyPath       | Retrieve empty campaign list when all campaigns applied      | HP02    | Medium   |            |
| GET /campaign/list | HappyPath       | Retrieve campaigns with multiple required documents          | HP03    | Medium   |            |
| GET /campaign/list | RequiredCheck   | Missing Authorization header validation                      | RC01    | High     | 5          |
| GET /campaign/list | RequiredCheck   | Missing Accept header validation                             | RC02    | Medium   |            |
| GET /campaign/list | RequiredCheck   | Missing Content-Type header validation                       | RC03    | Medium   |            |
| GET /campaign/list | RequiredCheck   | Empty Bearer token value                                     | RC04    | High     |            |
| GET /campaign/list | RequiredCheck   | Bearer token without prefix                                  | RC05    | High     |            |
| GET /campaign/list | LengthCheck     | Point amount boundary: zero value                            | LC01    | Medium   | 7          |
| GET /campaign/list | LengthCheck     | Point amount boundary: maximum value                         | LC02    | Medium   |            |
| GET /campaign/list | LengthCheck     | Campaign title with maximum length                           | LC03    | Medium   |            |
| GET /campaign/list | LengthCheck     | Brief eligibility with many conditions                       | LC04    | Low      |            |
| GET /campaign/list | LengthCheck     | Required documents with many items                           | LC05    | Low      |            |
| GET /campaign/list | LengthCheck     | Point grant date boundary: minimum date                      | LC06    | Low      |            |
| GET /campaign/list | LengthCheck     | Point grant date boundary: maximum date                      | LC07    | Low      |            |
| GET /campaign/list | ValidationCheck | Bearer token with special characters                         | VC01    | Medium   | 7          |
| GET /campaign/list | ValidationCheck | Campaign title with special characters                       | VC02    | Low      |            |
| GET /campaign/list | ValidationCheck | Brief eligibility with special characters                    | VC03    | Low      |            |
| GET /campaign/list | ValidationCheck | Document title with special characters                       | VC04    | Low      |            |
| GET /campaign/list | ValidationCheck | Point grant date format validation                           | VC05    | High     |            |
| GET /campaign/list | ValidationCheck | Campaign ID format validation                                | VC06    | Medium   |            |
| GET /campaign/list | ValidationCheck | Result field is integer 0 on success                         | VC07    | High     |            |
| GET /campaign/list | LogicCheck      | Campaign hidden if approved application exists               | LGC01   | High     | 8          |
| GET /campaign/list | LogicCheck      | Campaign hidden if under review application                  | LGC02   | High     |            |
| GET /campaign/list | LogicCheck      | Campaign hidden if grant done application                    | LGC03   | High     |            |
| GET /campaign/list | LogicCheck      | Campaign shown if rejected application                       | LGC04   | Medium   |            |
| GET /campaign/list | LogicCheck      | Campaign shown if withdrawn application                      | LGC05   | Medium   |            |
| GET /campaign/list | LogicCheck      | New campaign appears after insertion                         | LGC06   | Medium   |            |
| GET /campaign/list | LogicCheck      | Multiple customer number isolation                           | LGC07   | High     |            |
| GET /campaign/list | LogicCheck      | Campaigns ordered by ID ascending                            | LGC08   | Low      |            |
| GET /campaign/list | ErrorCheck      | Invalid or expired Bearer token                              | EC01    | High     | 10         |
| GET /campaign/list | ErrorCheck      | Account linkage not completed (403)                          | EC02    | High     |            |
| GET /campaign/list | ErrorCheck      | Database connection error (500)                              | EC03    | High     |            |
| GET /campaign/list | ErrorCheck      | Service temporarily unavailable (503)                        | EC04    | Medium   |            |
| GET /campaign/list | ErrorCheck      | Gateway or upstream timeout (504)                            | EC05    | Medium   |            |
| GET /campaign/list | ErrorCheck      | Malformed Authorization header                               | EC06    | High     |            |
| GET /campaign/list | ErrorCheck      | Wrong authentication scheme                                  | EC07    | High     |            |
| GET /campaign/list | ErrorCheck      | Response missing result field                                | EC08    | High     |            |
| GET /campaign/list | ErrorCheck      | Response missing campaigns field on success                  | EC09    | High     |            |
| GET /campaign/list | ErrorCheck      | Error response structure validation                          | EC10    | High     |            |



Option2:



| Module / API Name  | Scenario        | Verification Points          | Test Description                                             | Case ID        | Priority    | Case Count |
| ------------------ | --------------- | ---------------------------- | ------------------------------------------------------------ | -------------- | ----------- | ---------- |
| GET /campaign/list | HappyPath       | Success Response Structure   | Retrieve campaigns with valid token and completed linkage    | HP01           | High        | 3          |
| GET /campaign/list | HappyPath       | Empty Result Handling        | Return empty campaigns array when all eligible campaigns applied | HP02           | Medium      |            |
| GET /campaign/list | HappyPath       | Response Data Integrity      | Verify all required fields present in campaign objects       | HP03           | Medium      |            |
| GET /campaign/list | RequiredCheck   | Authorization Header         | Missing, empty, or malformed Authorization header validation | RC01-RC05      | High        | 5          |
| GET /campaign/list | RequiredCheck   | Content Headers              | Missing Accept and Content-Type headers handling             | RC02-RC03      | Medium      |            |
| GET /campaign/list | LengthCheck     | Numeric Boundaries           | Zero and maximum point amount values                         | LC01-LC02      | Medium      | 7          |
| GET /campaign/list | LengthCheck     | String Boundaries            | Maximum length titles and long eligibility/document lists    | LC03-LC05      | Low         |            |
| GET /campaign/list | LengthCheck     | Date Boundaries              | Minimum and maximum date values (YYYY-MM-DD format)          | LC06-LC07      | Low         |            |
| GET /campaign/list | ValidationCheck | Token Validation             | Special characters and format validation in Bearer token     | VC01           | Medium      | 7          |
| GET /campaign/list | ValidationCheck | Data Format                  | Special characters in titles, eligibility, documents         | VC02-VC04      | Low         |            |
| GET /campaign/list | ValidationCheck | Response Format              | Date format compliance and integer type validation           | VC05-VC07      | High        |            |
| GET /campaign/list | LogicCheck      | Application Status Filtering | Hide campaigns with approved/under review/grant done status  | LGC01-LGC03    | High        | 8          |
| GET /campaign/list | LogicCheck      | Rejected/Withdrawn Handling  | Show campaigns for rejected or withdrawn applications        | LGC04-LGC05    | Medium      |            |
| GET /campaign/list | LogicCheck      | Dynamic Data                 | New campaigns appear after insertion                         | LGC06          | Medium      |            |
| GET /campaign/list | LogicCheck      | Multi-Customer Isolation     | Separate campaign visibility per customer number             | LGC07          | High        |            |
| GET /campaign/list | LogicCheck      | Result Ordering              | Campaigns ordered by ID ascending                            | LGC08          | Low         |            |
| GET /campaign/list | ErrorCheck      | Authentication Errors        | Invalid/expired tokens and missing authentication            | EC01,EC06-EC07 | High        | 10         |
| GET /campaign/list | ErrorCheck      | Authorization Errors         | Account linkage not completed (403 Forbidden)                | EC02           | High        |            |
| GET /campaign/list | ErrorCheck      | Server Errors                | Database/system errors (500, 503, 504)                       | EC03-EC05      | High-Medium |            |
| GET /campaign/list | ErrorCheck      | Response Validation          | Missing required fields in success/error responses           | EC08-EC10      | High        |            |

**Total Test Cases: 40**

---

## Test Cases by Classification

### HappyPath (3 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| HP01 | Retrieve campaigns successfully | High | 1. User has completed account linkage<br>2. User has not applied to all campaigns<br>3. DB has 2+ campaigns | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept and Content-Type headers | 1. HTTP 200<br>2. result=0<br>3. campaigns array with all required fields | Not Run | - |
| HP02 | Retrieve empty campaign list | Medium | 1. User has completed account linkage<br>2. User has applied to all campaigns (status: Under review, Final approved, Grant Done) | 1. Send GET /campaign/list with valid token and headers | 1. HTTP 200<br>2. result=0<br>3. campaigns=[] | Not Run | - |
| HP03 | Retrieve campaigns with multiple required_documents | Medium | 1. User has completed account linkage<br>2. Campaigns have multiple required_documents | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Each campaign's required_documents contains multiple objects | Not Run | - |

### RequiredCheck (5 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| RC01 | Missing Authorization header | High | 1. None | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include other required headers | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| RC02 | Missing Accept header | Medium | 1. User has completed account linkage | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type | 1. HTTP 200 or 400<br>2. If 200: valid campaign data | Not Run | - |
| RC03 | Missing Content-Type header | Medium | 1. User has completed account linkage | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept | 1. HTTP 200 or 400<br>2. If 200: valid campaign data | Not Run | - |
| RC04 | Empty Bearer token value | High | 1. None | 1. Set Authorization: "Bearer "<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| RC05 | Bearer token without "Bearer " prefix | High | 1. None | 1. Set Authorization: "token_value_only"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |

### LengthCheck (7 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| LC01 | point_amount = 0 | Medium | 1. master_campaign has campaign with point_amount=0 | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Campaign with point_amount=0 displayed as Number | Not Run | - |
| LC02 | point_amount = maximum value | Medium | 1. master_campaign has campaign with point_amount=999999999 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Campaign with maximum point_amount displayed | Not Run | - |
| LC03 | Campaign title with maximum length | Medium | 1. master_campaign has campaign with title length 255+ chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Full title returned | Not Run | - |
| LC04 | brief_eligibility with many conditions | Low | 1. master_campaign has campaign with 10+ eligibility conditions | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All brief_eligibility items returned | Not Run | - |
| LC05 | required_documents with many items | Low | 1. master_campaign has campaign with 5+ required_documents | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All required_documents returned | Not Run | - |
| LC06 | point_grant_date = "1970-01-01" | Low | 1. master_campaign has campaign with point_grant_date="1970-01-01" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="1970-01-01" | Not Run | - |
| LC07 | point_grant_date = "9999-12-31" | Low | 1. master_campaign has campaign with point_grant_date="9999-12-31" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="9999-12-31" | Not Run | - |

### ValidationCheck (7 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| VC01 | Bearer token with special characters | Medium | 1. None | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| VC02 | Campaign title with special characters | Low | 1. master_campaign has title with emoji/symbols | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Title displays special characters | Not Run | - |
| VC03 | brief_eligibility with special characters/line breaks | Low | 1. master_campaign has eligibility with \n and special chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. brief_eligibility preserves formatting | Not Run | - |
| VC04 | required_documents title with special characters | Low | 1. master_campaign has document title with special chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Document title displays special characters | Not Run | - |
| VC05 | point_grant_date invalid format | High | 1. None (server-side validation) | 1. Verify response point_grant_date | 1. HTTP 200<br>2. All point_grant_date values match YYYY-MM-DD | Not Run | - |
| VC06 | campaign id format validation | Medium | 1. master_campaign has campaigns with id=1,999,1000000 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All campaign ids are Integer | Not Run | - |
| VC07 | result field is integer 0 on success | High | 1. User has completed account linkage | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0 (Integer) | Not Run | - |

### LogicCheck (8 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| LGC01 | Campaign hidden if user has approved application | High | 1. applications: user has campaign_id=1, status="Final approved" | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. campaign id=1 NOT in campaigns | Not Run | - |
| LGC02 | Campaign hidden if user has "Under review" application | High | 1. applications: user has campaign_id=2, status="Under review" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=2 NOT in campaigns | Not Run | - |
| LGC03 | Campaign hidden if user has "Grant Done" application | High | 1. applications: user has campaign_id=3, status="Grant Done" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=3 NOT in campaigns | Not Run | - |
| LGC04 | Campaign shown if user has "Rejected" application | Medium | 1. applications: user has campaign_id=4, status="Rejected" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=4 IS in campaigns | Not Run | - |
| LGC05 | Campaign shown if user has "Withdrawn" application | Medium | 1. applications: user has campaign_id=5, status="Withdrawn" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. campaign id=5 IS in campaigns | Not Run | - |
| LGC06 | New campaign appears after insertion | Medium | 1. master_campaign: insert new campaign id=100 | 1. Send GET /campaign/list before insert<br>2. Insert new campaign<br>3. Send GET /campaign/list after insert | 1. First: campaign id=100 not present<br>2. Second: campaign id=100 present | Not Run | - |
| LGC07 | Multiple customer_number isolation | High | 1. User has 2 customer_numbers<br>2. customer_A applied to campaign 1<br>3. customer_B hasn't applied | 1. Request with customer_A token<br>2. Request with customer_B token | 1. customer_A: campaign 1 NOT in list<br>2. customer_B: campaign 1 IS in list | Not Run | - |
| LGC08 | Campaigns ordered by id ascending | Low | 1. master_campaign has campaigns with ids: 5,2,10,1 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. campaigns ordered by id: 1,2,5,10 | Not Run | - |

### ErrorCheck (10 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| EC01 | Invalid or expired Bearer token | High | 1. None | 1. Use expired/invalid token<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| EC02 | Account linkage not completed | High | 1. account_linkage: no record for user | 1. Use valid token, no linkage<br>2. Send GET /campaign/list | 1. HTTP 403<br>2. result=2<br>3. error_message="Please complete account linkage first." | Not Run | - |
| EC03 | Database connection error | High | 1. Database unavailable | 1. Send GET /campaign/list<br>2. DB connection fails | 1. HTTP 500<br>2. result=3<br>3. error_message="A system error has occurred." | Not Run | - |
| EC04 | Service temporarily unavailable | Medium | 1. Service under maintenance | 1. Send GET /campaign/list | 1. HTTP 503<br>2. result=4<br>3. error_message="The service is temporarily unavailable." | Not Run | - |
| EC05 | Gateway or upstream timeout | Medium | 1. Backend slow/unresponsive | 1. Send GET /campaign/list | 1. HTTP 504<br>2. result=5<br>3. error_message="Request timed out." | Not Run | - |
| EC06 | Malformed Authorization header (missing space) | High | 1. Authorization="Bearertoken123" | 1. Set Authorization: "Bearertoken123"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| EC07 | Wrong authentication scheme | High | 1. Authorization="Basic base64string" | 1. Set Authorization: "Basic base64string"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| EC08 | Response missing result field | High | 1. Backend omits result field | 1. Send GET /campaign/list | 1. HTTP 200 (invalid structure)<br>2. Response must contain result field as Integer | Not Run | - |
| EC09 | Response missing campaigns field on success | High | 1. Backend omits campaigns field | 1. Send GET /campaign/list with valid token | 1. HTTP 200 (invalid)<br>2. Response must contain campaigns array | Not Run | - |
| EC10 | Error response has both result and campaigns | High | 1. Backend 401 includes both result and campaigns | 1. Send request with invalid token<br>2. Receive 401 | 1. HTTP 401<br>2. Response should NOT contain campaigns field | Not Run | - |






# Campaign List API - Comprehensive Test Cases

## Test Case Summary Matrix

| Module / API Name | Scenario | Verification Points | Test Description | Case ID | Priority | Case Count |
|-------------------|----------|-------------------|------------------|---------|----------|-----------|
| GET /campaign/list | HappyPath | Success Response Structure | Retrieve campaigns with valid token and completed linkage | HP01 | High | 3 |
| GET /campaign/list | HappyPath | Empty Result Handling | Return empty campaigns array when all eligible campaigns applied | HP02 | Medium | |
| GET /campaign/list | HappyPath | Response Data Integrity | Verify all required fields present in campaign objects | HP03 | Medium | |
| GET /campaign/list | RequiredCheck | Authorization Header | Missing, empty, or malformed Authorization header validation | RC01-RC05 | High | 5 |
| GET /campaign/list | RequiredCheck | Content Headers | Missing Accept and Content-Type headers handling | RC02-RC03 | Medium | |
| GET /campaign/list | LengthCheck | Numeric Boundaries | Zero and maximum point amount values | LC01-LC02 | Medium | 7 |
| GET /campaign/list | LengthCheck | String Boundaries | Maximum length titles and long eligibility/document lists | LC03-LC05 | Low | |
| GET /campaign/list | LengthCheck | Date Boundaries | Minimum and maximum date values (YYYY-MM-DD format) | LC06-LC07 | Low | |
| GET /campaign/list | ValidationCheck | Token Validation | Special characters and format validation in Bearer token | VC01 | Medium | 7 |
| GET /campaign/list | ValidationCheck | Data Format | Special characters in titles, eligibility, documents | VC02-VC04 | Low | |
| GET /campaign/list | ValidationCheck | Response Format | Date format compliance and integer type validation | VC05-VC07 | High | |
| GET /campaign/list | LogicCheck | Application Status Filtering | Hide campaigns with approved/under review/grant done status | LGC01-LGC03 | High | 8 |
| GET /campaign/list | LogicCheck | Rejected/Withdrawn Handling | Show campaigns for rejected or withdrawn applications | LGC04-LGC05 | Medium | |
| GET /campaign/list | LogicCheck | Dynamic Data | New campaigns appear after insertion | LGC06 | Medium | |
| GET /campaign/list | LogicCheck | Multi-Customer Isolation | Separate campaign visibility per customer number | LGC07 | High | |
| GET /campaign/list | LogicCheck | Result Ordering | Campaigns ordered by ID ascending | LGC08 | Low | |
| GET /campaign/list | ErrorCheck | Authentication Errors | Invalid/expired tokens and missing authentication | EC01,EC06-EC07 | High | 10 |
| GET /campaign/list | ErrorCheck | Authorization Errors | Account linkage not completed (403 Forbidden) | EC02 | High | |
| GET /campaign/list | ErrorCheck | Server Errors | Database/system errors (500, 503, 504) | EC03-EC05 | High-Medium | |
| GET /campaign/list | ErrorCheck | Response Validation | Missing required fields in success/error responses | EC08-EC10 | High | |

**Total Test Cases: 40**

---

## Test Cases by Classification

### HappyPath (3 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note | Issue |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|-------|
| HP01 | Retrieve campaigns successfully with valid authentication | High | 1. User has Bearer token<br>2. Account linkage completed<br>3. User not applied to all campaigns | 1. Send GET /campaign/list<br>2. Include Authorization: Bearer {valid_token}<br>3. Include Accept: application/json<br>4. Include Content-Type: application/json | 1. HTTP Status 200<br>2. result=0 (Integer)<br>3. campaigns array with 1+ items<br>4. Each campaign has: id, title, brief_eligibility, point_amount, required_documents, point_grant_date | Not Run | Valid happy path scenario | - |
| HP02 | Retrieve empty campaign list when all campaigns applied | Medium | 1. User has Bearer token<br>2. Account linkage completed<br>3. User applied to all campaigns (statuses: Under review/Final approved/Grant Done) | 1. Send GET /campaign/list with valid token<br>2. Include all required headers | 1. HTTP Status 200<br>2. result=0<br>3. campaigns=[] (empty array)<br>4. No error_message field | Not Run | Verify empty result handling | - |
| HP03 | Retrieve campaigns with multiple required_documents | Medium | 1. User has Bearer token<br>2. Account linkage completed<br>3. Campaigns have 2+ required_documents items | 1. Send GET /campaign/list<br>2. Include valid Bearer token | 1. HTTP Status 200<br>2. result=0<br>3. Each campaign.required_documents is array of objects<br>4. Each document object has: id (string) and title (string) | Not Run | Verify complex data structure | - |

### RequiredCheck (5 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note | Issue |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|-------|
| RC01 | Missing Authorization header returns 401 | High | 1. None | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include Accept: application/json, Content-Type: application/json | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required."<br>4. No campaigns field | Not Run | Mandatory header validation | - |
| RC02 | Missing Accept header handling | Medium | 1. User has valid Bearer token<br>2. Account linkage completed | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type | 1. HTTP Status 200 or 400<br>2. If 200: result=0, valid campaigns<br>3. If 400: error response | Not Run | Accept is marked required | - |
| RC03 | Missing Content-Type header handling | Medium | 1. User has valid Bearer token<br>2. Account linkage completed | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept | 1. HTTP Status 200 or 400<br>2. If 200: result=0, valid campaigns<br>3. If 400: error response | Not Run | Content-Type is marked required | - |
| RC04 | Empty Bearer token value returns 401 | High | 1. None | 1. Set Authorization: "Bearer " (empty value)<br>2. Send GET /campaign/list | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | Invalid token format | - |
| RC05 | Bearer token without "Bearer " prefix returns 401 | High | 1. None | 1. Set Authorization: "token_value_only" (no Bearer prefix)<br>2. Send GET /campaign/list | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | Authentication scheme validation | - |

### LengthCheck (7 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note | Issue |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|-------|
| LC01 | point_amount boundary: zero value | Medium | 1. master_campaign has campaign with point_amount=0<br>2. User eligible for campaign | 1. Send GET /campaign/list with valid token | 1. HTTP Status 200<br>2. result=0<br>3. Campaign with point_amount=0 returned as Number type | Not Run | Zero is valid boundary | - |
| LC02 | point_amount boundary: maximum value | Medium | 1. master_campaign has campaign with point_amount=999999999<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. Campaign with large point_amount displayed correctly | Not Run | Large number handling | - |
| LC03 | Campaign title maximum length | Medium | 1. master_campaign has campaign with title 255+ characters<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. Full title returned without truncation | Not Run | Long string handling | - |
| LC04 | brief_eligibility with many conditions | Low | 1. master_campaign has campaign with 10+ eligibility items<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. All brief_eligibility items returned | Not Run | Large array handling | - |
| LC05 | required_documents with many items | Low | 1. master_campaign has campaign with 5+ required_documents<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. All required_documents returned | Not Run | Complex nested structure | - |
| LC06 | point_grant_date minimum boundary | Low | 1. master_campaign has campaign with point_grant_date="1970-01-01"<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. point_grant_date="1970-01-01" returned | Not Run | Historical date handling | - |
| LC07 | point_grant_date maximum boundary | Low | 1. master_campaign has campaign with point_grant_date="9999-12-31"<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. point_grant_date="9999-12-31" returned | Not Run | Future date handling | - |

### ValidationCheck (7 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note | Issue |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|-------|
| VC01 | Bearer token with special characters | Medium | 1. None | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | Invalid token detection | - |
| VC02 | Campaign title with special characters/emoji | Low | 1. master_campaign has title with emoji, symbols (e.g., "🎁 特典 & 期間限定")<br>2. User eligible | 1. Send GET /campaign/list with valid token | 1. HTTP Status 200<br>2. result=0<br>3. Title with special characters preserved | Not Run | UTF-8 character encoding | - |
| VC03 | brief_eligibility with line breaks and special chars | Low | 1. master_campaign has eligibility with \n and special characters<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. brief_eligibility preserves formatting | Not Run | Multiline string handling | - |
| VC04 | required_documents title with special characters | Low | 1. master_campaign has document title with symbols (e.g., "融資実行確認書類 (PDF)")<br>2. User eligible | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. Document title with special characters preserved | Not Run | Complex character sets | - |
| VC05 | point_grant_date format validation (YYYY-MM-DD) | High | 1. None (server-side validation) | 1. Send GET /campaign/list with valid token<br>2. Verify all returned dates | 1. HTTP Status 200<br>2. All point_grant_date values match pattern YYYY-MM-DD<br>3. Example: 2025-12-31 | Not Run | ISO 8601 compliance | - |
| VC06 | campaign id must be Integer type | Medium | 1. master_campaign has campaigns with ids: 1, 999, 1000000<br>2. User eligible for all | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. All campaign.id values are Integer type (not string/float) | Not Run | Type validation | - |
| VC07 | result field must be integer 0 on success | High | 1. User has completed account linkage | 1. Send GET /campaign/list<br>2. Verify response result field | 1. HTTP Status 200<br>2. result field exists and equals 0 (Integer, not string "0") | Not Run | Response field type | - |

### LogicCheck (8 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note | Issue |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|-------|
| LGC01 | Campaign hidden if user has approved application | High | 1. applications: user_id=A, campaign_id=1, status="Final approved"<br>2. master_campaign: campaign id=1 exists<br>3. User has valid token | 1. Send GET /campaign/list with user_id=A token | 1. HTTP Status 200<br>2. result=0<br>3. campaign id=1 NOT in campaigns array | Not Run | Status filtering logic | - |
| LGC02 | Campaign hidden if user has "Under review" application | High | 1. applications: user_id=B, campaign_id=2, status="Under review"<br>2. master_campaign: campaign id=2 exists<br>3. User has valid token | 1. Send GET /campaign/list with user_id=B token | 1. HTTP Status 200<br>2. result=0<br>3. campaign id=2 NOT in campaigns array | Not Run | Pending application handling | - |
| LGC03 | Campaign hidden if user has "Grant Done" application | High | 1. applications: user_id=C, campaign_id=3, status="Grant Done"<br>2. master_campaign: campaign id=3 exists<br>3. User has valid token | 1. Send GET /campaign/list with user_id=C token | 1. HTTP Status 200<br>2. result=0<br>3. campaign id=3 NOT in campaigns array | Not Run | Completed application handling | - |
| LGC04 | Campaign shown if user has "Rejected" application | Medium | 1. applications: user_id=D, campaign_id=4, status="Rejected"<br>2. master_campaign: campaign id=4 exists<br>3. User has valid token | 1. Send GET /campaign/list with user_id=D token | 1. HTTP Status 200<br>2. result=0<br>3. campaign id=4 IS in campaigns array | Not Run | Rejected status allows reapply | - |
| LGC05 | Campaign shown if user has "Withdrawn" application | Medium | 1. applications: user_id=E, campaign_id=5, status="Withdrawn"<br>2. master_campaign: campaign id=5 exists<br>3. User has valid token | 1. Send GET /campaign/list with user_id=E token | 1. HTTP Status 200<br>2. result=0<br>3. campaign id=5 IS in campaigns array | Not Run | Withdrawn status allows reapply | - |
| LGC06 | New campaign appears after insertion into master_campaign | Medium | 1. None | 1. Send GET /campaign/list (before insert)<br>2. Verify campaign id=100 not present<br>3. INSERT into master_campaign: campaign id=100<br>4. Send GET /campaign/list (after insert) | 1. First response: campaign id=100 absent<br>2. Second response: campaign id=100 present with result=0 | Not Run | Dynamic data refresh | - |
| LGC07 | Multiple customer_number isolation | High | 1. account_linkage: easy_id=user1 has customer_A and customer_B<br>2. applications: customer_A applied to campaign_1 (status: Final approved)<br>3. applications: customer_B has no application for campaign_1 | 1. Request with customer_A token<br>2. Request with customer_B token | 1. customer_A response: campaign_1 NOT in list<br>2. customer_B response: campaign_1 IS in list | Not Run | Customer isolation | - |
| LGC08 | Campaigns ordered by id ascending | Low | 1. master_campaign has campaigns with ids: 5, 2, 10, 1<br>2. User eligible for all | 1. Send GET /campaign/list | 1. HTTP Status 200<br>2. result=0<br>3. campaigns array ordered by id: [1, 2, 5, 10] | Not Run | Sort order validation | - |

### ErrorCheck (10 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Case Status | Note | Issue |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|-------|
| EC01 | Invalid or expired Bearer token | High | 1. Token is expired or invalid (e.g., tampered) | 1. Use token: "Bearer expired_token_xyz"<br>2. Send GET /campaign/list | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required."<br>4. No campaigns field | Not Run | Auth failure handling | - |
| EC02 | Account linkage not completed (403 Forbidden) | High | 1. Token is valid but user has no entry in account_linkage table | 1. Use valid token from unlinkled user<br>2. Send GET /campaign/list | 1. HTTP Status 403<br>2. result=2<br>3. error_message="Please complete account linkage first."<br>4. No campaigns field | Not Run | Authorization check | - |
| EC03 | Database connection error (500 Internal Server Error) | High | 1. Database is temporarily unavailable/unreachable | 1. Send GET /campaign/list with valid token<br>2. Backend database connection fails | 1. HTTP Status 500<br>2. result=3<br>3. error_message="A system error has occurred."<br>4. No campaigns field | Not Run | Server error handling | - |
| EC04 | Service temporarily unavailable (503) | Medium | 1. Service under maintenance or overloaded | 1. Send GET /campaign/list | 1. HTTP Status 503<br>2. result=4<br>3. error_message="The service is temporarily unavailable." | Not Run | Service unavailability | - |
| EC05 | Gateway or upstream timeout (504) | Medium | 1. Backend service is slow or unresponsive | 1. Send GET /campaign/list with valid token<br>2. Request timeout occurs | 1. HTTP Status 504<br>2. result=5<br>3. error_message="Request timed out." | Not Run | Timeout handling | - |
| EC06 | Malformed Authorization header (missing space) | High | 1. None | 1. Set Authorization: "Bearertoken123" (no space)<br>2. Send GET /campaign/list | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | Header format validation | - |
| EC07 | Wrong authentication scheme (Basic instead of Bearer) | High | 1. None | 1. Set Authorization: "Basic dXNlcjpwYXNz"<br>2. Send GET /campaign/list | 1. HTTP Status 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | Auth scheme validation | - |
| EC08 | Success response missing result field | High | 1. Backend bug: omits result field | 1. Send GET /campaign/list with valid token<br>2. Receive 200 response | 1. HTTP Status 200 (invalid structure)<br>2. Response SHOULD contain result field as Integer<br>3. If missing: mark as malformed response | Not Run | Response structure | - |
| EC09 | Success response missing campaigns field | High | 1. Backend bug: omits campaigns field | 1. Send GET /campaign/list with valid token<br>2. User has eligible campaigns | 1. HTTP Status 200 (invalid)<br>2. Response SHOULD contain campaigns array<br>3. If missing: mark as malformed response | Not Run | Response completeness | - |
| EC10 | Error response includes campaigns field | High | 1. Backend inconsistency: 401 includes campaigns | 1. Send GET /campaign/list with invalid token<br>2. Receive 401 response | 1. HTTP Status 401<br>2. Response has result and error_message<br>3. Response SHOULD NOT have campaigns field | Not Run | Error response purity | - |