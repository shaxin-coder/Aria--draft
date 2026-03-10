## Project defination

### Case ID：
- API：
TC-API-[APINAME]-[TYPE]-[NUMBER]
    1. TC-API-CAMPAIGN-LIST-HP-001
    2. TC-API-CAMPAIGN-LIST-REQ-002
    3. TC-API-CAMPAIGN-LIST-ERR-003
    4. TC-API-CAMPAIGN-LIST-LOG-004

- Batch：
TC-BATCH-[JOBNAME]-[TYPE]-[NUMBER]
    1. TC-BATCH-POINT-GRANT-HP-001
    2. TC-BATCH-POINT-GRANT-VAL-002
    3. TC-BATCH-POINT-GRANT-ERR-003

- UI:
TC-UI-[ModuleName]-[TYPE]-[NUMBER]
    1. TC-UI-IdLinkage-HP-001
    2. TC-UI-IdLinkage-VAL-002
    3. TC-UI-IdLinkage-ERR-003

### Case status：
- Not Run 
- Pass
- Fail 
- Blocked 
- OutOfScope

### Priority:
- Critical
- High
- Medium
- Low

## Test Case Summary Matrix

### Option1:

| Case ID | Priority | Category        | Verification Points                                    | Test Description                                             |
| ------- | -------- | --------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| HP01    | High     | HappyPath       | Valid token and account linkage                        | Retrieve campaigns successfully with valid token and linkage |
| HP02    | Medium   | HappyPath       | Campaign list empty when all campaigns already applied | Retrieve empty campaign list when all campaigns applied      |
| HP03    | Medium   | HappyPath       | Campaign includes multiple required documents          | Retrieve campaigns with multiple required documents          |
| RC01    | High     | RequiredCheck   | Authorization header missing                           | Missing Authorization header validation                      |
| RC02    | Medium   | RequiredCheck   | Accept header missing                                  | Missing Accept header validation                             |
| RC03    | Medium   | RequiredCheck   | Content-Type header missing                            | Missing Content-Type header validation                       |
| RC04    | High     | RequiredCheck   | Bearer token value empty                               | Empty Bearer token value                                     |
| RC05    | High     | RequiredCheck   | Bearer token without prefix                            | Bearer token without prefix                                  |
| LC01    | Medium   | LengthCheck     | Point amount boundary equals 0                         | Point amount boundary: zero value                            |
| LC02    | Medium   | LengthCheck     | Point amount maximum boundary                          | Point amount boundary: maximum value                         |
| LC03    | Medium   | LengthCheck     | Campaign title maximum length                          | Campaign title with maximum length                           |
| LC04    | Low      | LengthCheck     | Large number of eligibility conditions                 | Brief eligibility with many conditions                       |
| LC05    | Low      | LengthCheck     | Large number of required documents                     | Required documents with many items                           |
| LC06    | Low      | LengthCheck     | Minimum point grant date boundary                      | Point grant date boundary: minimum date                      |
| LC07    | Low      | LengthCheck     | Maximum point grant date boundary                      | Point grant date boundary: maximum date                      |
| VC01    | Medium   | ValidationCheck | Bearer token contains special characters               | Bearer token with special characters                         |
| VC02    | Low      | ValidationCheck | Campaign title contains special characters             | Campaign title with special characters                       |
| VC03    | Low      | ValidationCheck | Eligibility text contains special characters           | Brief eligibility with special characters                    |
| VC04    | Low      | ValidationCheck | Document title contains special characters             | Document title with special characters                       |
| VC05    | High     | ValidationCheck | Date format must follow YYYY-MM-DD                     | Point grant date format validation                           |
| VC06    | Medium   | ValidationCheck | Campaign ID must be integer format                     | Campaign ID format validation                                |
| VC07    | High     | ValidationCheck | Result field equals integer 0 on success               | Result field is integer 0 on success                         |
| LGC01   | High     | LogicCheck      | Campaign hidden when approved application exists       | Campaign hidden if approved application exists               |
| LGC02   | High     | LogicCheck      | Campaign hidden when application under review          | Campaign hidden if under review application                  |
| LGC03   | High     | LogicCheck      | Campaign hidden when grant already completed           | Campaign hidden if grant done application                    |
| LGC04   | Medium   | LogicCheck      | Campaign visible when application rejected             | Campaign shown if rejected application                       |
| LGC05   | Medium   | LogicCheck      | Campaign visible when application withdrawn            | Campaign shown if withdrawn application                      |
| LGC06   | Medium   | LogicCheck      | New campaign inserted appears in response              | New campaign appears after insertion                         |
| LGC07   | High     | LogicCheck      | Campaign data isolated by customer number              | Multiple customer number isolation                           |
| LGC08   | Low      | LogicCheck      | Campaign list ordered by campaign ID ascending         | Campaigns ordered by ID ascending                            |
| EC01    | High     | ErrorCheck      | Invalid or expired Bearer token                        | Invalid or expired Bearer token                              |
| EC02    | High     | ErrorCheck      | Account linkage not completed                          | Account linkage not completed (403)                          |
| EC03    | High     | ErrorCheck      | Database connection failure                            | Database connection error (500)                              |
| EC04    | Medium   | ErrorCheck      | Service temporarily unavailable                        | Service temporarily unavailable (503)                        |
| EC05    | Medium   | ErrorCheck      | Gateway or upstream timeout                            | Gateway or upstream timeout (504)                            |
| EC06    | High     | ErrorCheck      | Authorization header malformed                         | Malformed Authorization header                               |
| EC07    | High     | ErrorCheck      | Wrong authentication scheme used                       | Wrong authentication scheme                                  |
| EC08    | High     | ErrorCheck      | Response missing required field result                 | Response missing result field                                |
| EC09    | High     | ErrorCheck      | Response missing campaigns field                       | Response missing campaigns field on success                  |
| EC10    | High     | ErrorCheck      | Error response structure validation                    | Error response structure validation                          |


### Option2:



| Module / API Name / Batch Name  | Scenario        | Verification Points          | Test Description                                             | Case ID        | Priority    | Case Count |
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

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Log Check(For batch) ｜ Case Status | Note | 
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| HP01 | Retrieve campaigns successfully | High | 1. User has completed account linkage<br>2. User has not applied to all campaigns<br>3. DB has 2+ campaigns | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept and Content-Type headers | 1. HTTP 200<br>2. result=0<br>3. campaigns array with all required fields | Not Run | - |
| HP02 | Retrieve empty campaign list | Medium | 1. User has completed account linkage<br>2. User has applied to all campaigns (status: Under review, Final approved, Grant Done) | 1. Send GET /campaign/list with valid token and headers | 1. HTTP 200<br>2. result=0<br>3. campaigns=[] | Not Run | - |
| HP03 | Retrieve campaigns with multiple required_documents | Medium | 1. User has completed account linkage<br>2. Campaigns have multiple required_documents | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Each campaign's required_documents contains multiple objects | Not Run | - |

### RequiredCheck (5 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Log Check(For batch) ｜ Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| RC01 | Missing Authorization header | High | 1. None | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include other required headers | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| RC02 | Missing Accept header | Medium | 1. User has completed account linkage | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type | 1. HTTP 200 or 400<br>2. If 200: valid campaign data | Not Run | - |
| RC03 | Missing Content-Type header | Medium | 1. User has completed account linkage | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept | 1. HTTP 200 or 400<br>2. If 200: valid campaign data | Not Run | - |
| RC04 | Empty Bearer token value | High | 1. None | 1. Set Authorization: "Bearer "<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| RC05 | Bearer token without "Bearer " prefix | High | 1. None | 1. Set Authorization: "token_value_only"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |

### LengthCheck (7 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Log Check(For batch) ｜ Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| LC01 | point_amount = 0 | Medium | 1. master_campaign has campaign with point_amount=0 | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Campaign with point_amount=0 displayed as Number | Not Run | - |
| LC02 | point_amount = maximum value | Medium | 1. master_campaign has campaign with point_amount=999999999 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Campaign with maximum point_amount displayed | Not Run | - |
| LC03 | Campaign title with maximum length | Medium | 1. master_campaign has campaign with title length 255+ chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Full title returned | Not Run | - |
| LC04 | brief_eligibility with many conditions | Low | 1. master_campaign has campaign with 10+ eligibility conditions | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All brief_eligibility items returned | Not Run | - |
| LC05 | required_documents with many items | Low | 1. master_campaign has campaign with 5+ required_documents | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All required_documents returned | Not Run | - |
| LC06 | point_grant_date = "1970-01-01" | Low | 1. master_campaign has campaign with point_grant_date="1970-01-01" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="1970-01-01" | Not Run | - |
| LC07 | point_grant_date = "9999-12-31" | Low | 1. master_campaign has campaign with point_grant_date="9999-12-31" | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="9999-12-31" | Not Run | - |

### ValidationCheck (7 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Log Check(For batch) ｜ Case Status | Note |
|---------|-----------|----------|--------------|-------|-----------------|-------------|------|
| VC01 | Bearer token with special characters | Medium | 1. None | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required." | Not Run | - |
| VC02 | Campaign title with special characters | Low | 1. master_campaign has title with emoji/symbols | 1. Send GET /campaign/list with valid token | 1. HTTP 200<br>2. result=0<br>3. Title displays special characters | Not Run | - |
| VC03 | brief_eligibility with special characters/line breaks | Low | 1. master_campaign has eligibility with \n and special chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. brief_eligibility preserves formatting | Not Run | - |
| VC04 | required_documents title with special characters | Low | 1. master_campaign has document title with special chars | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. Document title displays special characters | Not Run | - |
| VC05 | point_grant_date invalid format | High | 1. None (server-side validation) | 1. Verify response point_grant_date | 1. HTTP 200<br>2. All point_grant_date values match YYYY-MM-DD | Not Run | - |
| VC06 | campaign id format validation | Medium | 1. master_campaign has campaigns with id=1,999,1000000 | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0<br>3. All campaign ids are Integer | Not Run | - |
| VC07 | result field is integer 0 on success | High | 1. User has completed account linkage | 1. Send GET /campaign/list | 1. HTTP 200<br>2. result=0 (Integer) | Not Run | - |

### LogicCheck (8 cases)

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Log Check(For batch) ｜ Case Status | Note |
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

| Case ID | Case Name | Priority | Precondition | Steps | Expected Result | Log Check(For batch) ｜ Case Status | Note |
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

