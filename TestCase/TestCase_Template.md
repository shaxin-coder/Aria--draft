# Retrieve Campaign List API – Test Cases

| Spec           | `Campaign_List_API_Interface.md` |
|----------------|----------------------------------|
| Endpoint       | GET `/campaign/list`            |
| Authentication | Bearer token                    |
| Note           | No DB update.                  |

---

## API overview

**API name:** Retrieve Campaign List API  
**Description:** Return the list of campaigns available to the authenticated user.

---

## Sample success response

```json
{
  "result": 0,
  "campaigns": [
    {
      "id": 1,
      "title": "新規ご契約特典",
      "brief_eligibility": [
        "2026年10月1日以降に住宅ローンの融資が実行されていること",
        "同一債権に対するポイント申請1回限りとします",
        "融資実行日から〇ヶ月以内に申請すること"
      ],
      "point_amount": 5000,
      "required_documents": [
        { "id": "doc_1", "title": "融資実行確認書類" }
      ],
      "point_grant_date": "2025-12-31"
    },
    {
      "id": 2,
      "title": "ご子息誕生お祝い特典",
      "brief_eligibility": [
        "2026年10月1日以降に住宅ローンの融資が実行されていること",
        "同一債権に対するポイント申請1回限りとします",
        "融資実行日から〇ヶ月以内に申請すること"
      ],
      "point_amount": 30000,
      "required_documents": [
        { "id": "doc_1", "title": "融資実行確認書類" }
      ],
      "point_grant_date": "2025-01-31"
    }
  ]
}
```

---


## Test Case Summary Matrix

| Test Case ID                      | Priority | Category        | Test Case Description                                             | Note |
| ---------------------------- | -------- | --------------- | ------------------------------------------------------------ | ---- |
| TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with valid token and linkage |      |
| TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve empty campaign list when all campaigns applied      |      |
| TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with multiple required documents          |      |
| TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                      |      |
| TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                             |      |
| TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                       |      |
| TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                     |      |
| TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without prefix                                  |      |
| TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                            |      |
| TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                         |      |
| TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                           |      |
| TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with many conditions                       |      |
| TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with many items                           |      |
| TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                      |      |
| TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                      |      |
| TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                         |      |
| TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                       |      |
| TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                    |      |
| TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                       |      |
| TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                           |      |
| TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                |      |
| TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer 0 on success                         |      |
| TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists               |      |
| TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                  |      |
| TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if grant done application                    |      |
| TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                       |      |
| TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                      |      |
| TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                         |      |
| TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number isolation                           |      |
| TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID ascending                            |      |
| TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                              |      |
| TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                          |      |
| TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                              |      |
| TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                        |      |
| TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                            |      |
| TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                               |      |
| TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                  |      |
| TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                |      |
| TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaigns field on success                  |      |
| TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                          |      |

**Total Test Cases: 40**

---

## Test Cases Steps

| Test Case ID                       | Priority | Category        | Test Case Description                                             | Given                                                                                                                            | When                                                                                                          | Then                                                                                          | Case Status | Note |
| ----------------------------- | -------- | --------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ----------- | ---- |
| TC-API-CAMPAIGN-LIST-HP-001   | L1       | HappyPath       | Retrieve campaigns successfully with valid token and linkage | 1. User has completed account linkage<br>2. User has not applied to all campaigns<br>3. DB has 2+ campaigns                      | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept and Content-Type headers     | 1. HTTP 200<br>2. result=0<br>3. campaigns array with all required fields                     | Not Run     |      |
| TC-API-CAMPAIGN-LIST-HP-002   | L2       | HappyPath       | Retrieve empty campaign list when all campaigns applied      | 1. User has completed account linkage<br>2. User has applied to all campaigns (status: Under review, Final approved, Grant Done) | 1. Send GET /campaign/list with valid token and headers                                                       | 1. HTTP 200<br>2. result=0<br>3. campaigns=[]                                                 | Not Run     |      |
| TC-API-CAMPAIGN-LIST-HP-003   | L2       | HappyPath       | Retrieve campaigns with multiple required documents          | 1. User has completed account linkage<br>2. Campaigns have multiple required_documents                                           | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. Each campaign's required_documents contains multiple objects | Not Run     |      |
| TC-API-CAMPAIGN-LIST-REQ-001  | L1       | RequiredCheck   | Missing Authorization header validation                      | 1. None                                                                                                                          | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include other required headers               | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-REQ-002  | L2       | RequiredCheck   | Missing Accept header validation                             | 1. User has completed account linkage                                                                                            | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type              | 1. HTTP 200 or 400<br>2. If 200: valid campaign data                                          | Not Run     |      |
| TC-API-CAMPAIGN-LIST-REQ-003  | L2       | RequiredCheck   | Missing Content-Type header validation                       | 1. User has completed account linkage                                                                                            | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept              | 1. HTTP 200 or 400<br>2. If 200: valid campaign data                                          | Not Run     |      |
| TC-API-CAMPAIGN-LIST-REQ-004  | L1       | RequiredCheck   | Empty Bearer token value                                     | 1. None                                                                                                                          | 1. Set Authorization: "Bearer "<br>2. Send GET /campaign/list                                                 | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-REQ-005  | L1       | RequiredCheck   | Bearer token without prefix                                  | 1. None                                                                                                                          | 1. Set Authorization: "token_value_only"<br>2. Send GET /campaign/list                                        | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-001  | L2       | LengthCheck     | Point amount boundary: zero value                            | 1. master_campaign has campaign with point_amount=0                                                                              | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. Campaign with point_amount=0 displayed as Number             | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-002  | L2       | LengthCheck     | Point amount boundary: maximum value                         | 1. master_campaign has campaign with point_amount=999999999                                                                      | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. Campaign with maximum point_amount displayed                 | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-003  | L2       | LengthCheck     | Campaign title with maximum length                           | 1. master_campaign has campaign with title length 255+ chars                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. Full title returned                                          | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-004  | L3       | LengthCheck     | Brief eligibility with many conditions                       | 1. master_campaign has campaign with 10+ eligibility conditions                                                                  | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. All brief_eligibility items returned                         | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-005  | L3       | LengthCheck     | Required documents with many items                           | 1. master_campaign has campaign with 5+ required_documents                                                                       | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. All required_documents returned                              | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-006  | L3       | LengthCheck     | Point grant date boundary: minimum date                      | 1. master_campaign has campaign with point_grant_date="1970-01-01"                                                               | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="1970-01-01"                                | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LEN-007  | L4       | LengthCheck     | Point grant date boundary: maximum date                      | 1. master_campaign has campaign with point_grant_date="9999-12-31"                                                               | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="9999-12-31"                                | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-001  | L2       | ValidationCheck | Bearer token with special characters                         | 1. None                                                                                                                          | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list                                       | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-002  | L3       | ValidationCheck | Campaign title with special characters                       | 1. master_campaign has title with emoji/symbols                                                                                  | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. Title displays special characters                            | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-003  | L3       | ValidationCheck | Brief eligibility with special characters                    | 1. master_campaign has eligibility with \n and special chars                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. brief_eligibility preserves formatting                       | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-004  | L3       | ValidationCheck | Document title with special characters                       | 1. master_campaign has document title with special chars                                                                         | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. Document title displays special characters                   | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-005  | L1       | ValidationCheck | Point grant date format validation                           | 1. None (server-side validation)                                                                                                 | 1. Verify response point_grant_date                                                                           | 1. HTTP 200<br>2. All point_grant_date values match YYYY-MM-DD                                | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-006  | L3       | ValidationCheck | Campaign ID format validation                                | 1. master_campaign has campaigns with id=1,999,1000000                                                                           | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. All campaign ids are Integer                                 | Not Run     |      |
| TC-API-CAMPAIGN-LIST-VAL-007  | L1       | ValidationCheck | Result field is integer 0 on success                         | 1. User has completed account linkage                                                                                            | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0 (Integer)                                                          | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-001  | L1       | LogicCheck      | Campaign hidden if approved application exists               | 1. applications: user has campaign_id=1, status="Final approved"                                                                 | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. campaign id=1 NOT in campaigns                               | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-002  | L1       | LogicCheck      | Campaign hidden if under review application                  | 1. applications: user has campaign_id=2, status="Under review"                                                                   | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=2 NOT in campaigns                               | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-0C03 | L1       | LogicCheck      | Campaign hidden if grant done application                    | 1. applications: user has campaign_id=3, status="Grant Done"                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=3 NOT in campaigns                               | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-004  | L2       | LogicCheck      | Campaign shown if rejected application                       | 1. applications: user has campaign_id=4, status="Rejected"                                                                       | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=4 IS in campaigns                                | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-005  | L2       | LogicCheck      | Campaign shown if withdrawn application                      | 1. applications: user has campaign_id=5, status="Withdrawn"                                                                      | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=5 IS in campaigns                                | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-006  | L2       | LogicCheck      | New campaign appears after insertion                         | 1. master_campaign: insert new campaign id=100                                                                                   | 1. Send GET /campaign/list before insert<br>2. Insert new campaign<br>3. Send GET /campaign/list after insert | 1. First: campaign id=100 not present<br>2. Second: campaign id=100 present                   | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-007  | L1       | LogicCheck      | Multiple customer number isolation                           | 1. User has 2 customer_numbers<br>2. customer_A applied to campaign 1<br>3. customer_B hasn't applied                            | 1. Request with customer_A token<br>2. Request with customer_B token                                          | 1. customer_A: campaign 1 NOT in list<br>2. customer_B: campaign 1 IS in list                 | Not Run     |      |
| TC-API-CAMPAIGN-LIST-LOG-008  | L3       | LogicCheck      | Campaigns ordered by ID ascending                            | 1. master_campaign has campaigns with ids: 5,2,10,1                                                                              | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. campaigns ordered by id: 1,2,5,10                                           | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-001  | L1       | ErrorCheck      | Invalid or expired Bearer token                              | 1. None                                                                                                                          | 1. Use expired/invalid token<br>2. Send GET /campaign/list                                                    | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-002  | L1       | ErrorCheck      | Account linkage not completed (403)                          | 1. account_linkage: no record for user                                                                                           | 1. Use valid token, no linkage<br>2. Send GET /campaign/list                                                  | 1. HTTP 403<br>2. result=2<br>3. error_message="Please complete account linkage first."       | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-003  | L1       | ErrorCheck      | Database connection error (500)                              | 1. Database unavailable                                                                                                          | 1. Send GET /campaign/list<br>2. DB connection fails                                                          | 1. HTTP 500<br>2. result=3<br>3. error_message="A system error has occurred."                 | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-004  | L2       | ErrorCheck      | Service temporarily unavailable (503)                        | 1. Service under maintenance                                                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 503<br>2. result=4<br>3. error_message="The service is temporarily unavailable."      | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-005  | L2       | ErrorCheck      | Gateway or upstream timeout (504)                            | 1. Backend slow/unresponsive                                                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 504<br>2. result=5<br>3. error_message="Request timed out."                           | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-006  | L1       | ErrorCheck      | Malformed Authorization header                               | 1. Authorization="Bearertoken123"                                                                                                | 1. Set Authorization: "Bearertoken123"<br>2. Send GET /campaign/list                                          | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-007  | L1       | ErrorCheck      | Wrong authentication scheme                                  | 1. Authorization="Basic base64string"                                                                                            | 1. Set Authorization: "Basic base64string"<br>2. Send GET /campaign/list                                      | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-008  | L1       | ErrorCheck      | Response missing result field                                | 1. Backend omits result field                                                                                                    | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200 (invalid structure)<br>2. Response must contain result field as Integer           | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-009  | L1       | ErrorCheck      | Response missing campaigns field on success                  | 1. Backend omits campaigns field                                                                                                 | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200 (invalid)<br>2. Response must contain campaigns array                             | Not Run     |      |
| TC-API-CAMPAIGN-LIST-ERR-010  | L2       | ErrorCheck      | Error response structure validation                          | 1. Backend 401 includes both result and campaigns                                                                                | 1. Send request with invalid token<br>2. Receive 401                                                          | 1. HTTP 401<br>2. Response should NOT contain campaigns field                                 | Not Run     |      |