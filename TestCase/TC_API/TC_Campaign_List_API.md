# Retrieve Campaign List API – Test Cases

| Spec           | `Campaign_List_API_Interface.md` |
|----------------|----------------------------------|
| Endpoint       | GET `/campaign/list`            |
| Authentication | Bearer token                    |
| Note           | No DB update.                   |

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

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |----------------------------- | ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ssfully with valid token and ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |list when all campaigns ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |multiple required ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ader ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |der ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ero ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |aximum ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |mum ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |any ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |many ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |y: minimum ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |y: maximum ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |l ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ial ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |pecial ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ial ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |0 on ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ved application ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      | review ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      | done ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ed ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |awn ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |er ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      | ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |r ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |leted ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |r ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ailable ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |out ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |ns field on ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |
## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      | ## Test Case Summary Matrix

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                                                 | Note |
|------|------------------------------|----------|-----------------|---------------------------------------------------------------------------------------|------|
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with account linkage                                   |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when no campaign applied                                  |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with required documents                                              |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                                                |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                                                       |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                                                 |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                                               |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without "Bearer" prefix                                                   |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                                                      |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                                                   |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                                                     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with maximum conditions                                              |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with maximum items                                                  |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                                                |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                                                |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                                                   |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                                                 |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                                              |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                                                 |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                                                     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                                         |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer when query succeeds                                            |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists                                          |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                                            |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if granted application                                                |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                                                 |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                                                |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                                                   |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number data isolation                                                |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID in ascending order                                              |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                                                        |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                                                    |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                                                       |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                                                  |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                                                      |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                                                         |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                                            |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                                          |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaign data when query succeeds                                     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                                                     |      |

**Total Test Cases: 40**

---

## Test Cases Steps

| No.  | Test Case ID                 | Priority | Category        | Test Case Description                                        | Given                                                                                                                            | When                                                                                                          | Then                                                                                          | Case Status | Note |
| ---- | ---------------------------- | -------- | --------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ----------- | ---- |
| 1    | TC-API-CAMPAIGN-LIST-HP-001  | L1       | HappyPath       | Retrieve campaigns successfully with valid token and linkage | 1. User has completed account linkage<br>2. User has not applied to all campaigns<br>3. DB has 2+ campaigns                      | 1. Send GET /campaign/list<br>2. Include valid Bearer token<br>3. Include Accept and Content-Type headers     | 1. HTTP 200<br>2. result=0<br>3. campaigns array with all required fields                     | Not Run     |      |
| 2    | TC-API-CAMPAIGN-LIST-HP-002  | L2       | HappyPath       | Retrieve empty campaign list when all campaigns applied      | 1. User has completed account linkage<br>2. User has applied to all campaigns (status: Under review, Final approved, Grant Done) | 1. Send GET /campaign/list with valid token and headers                                                       | 1. HTTP 200<br>2. result=0<br>3. campaigns=[]                                                 | Not Run     |      |
| 3    | TC-API-CAMPAIGN-LIST-HP-003  | L2       | HappyPath       | Retrieve campaigns with multiple required documents          | 1. User has completed account linkage<br>2. Campaigns have multiple required_documents                                           | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. Each campaign's required_documents contains multiple objects | Not Run     |      |
| 4    | TC-API-CAMPAIGN-LIST-REQ-001 | L1       | RequiredCheck   | Missing Authorization header validation                      | 1. None                                                                                                                          | 1. Send GET /campaign/list<br>2. Omit Authorization header<br>3. Include other required headers               | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 5    | TC-API-CAMPAIGN-LIST-REQ-002 | L2       | RequiredCheck   | Missing Accept header validation                             | 1. User has completed account linkage                                                                                            | 1. Send GET /campaign/list<br>2. Omit Accept header<br>3. Include Authorization and Content-Type              | 1. HTTP 200 or 400<br>2. If 200: valid campaign data                                          | Not Run     |      |
| 6    | TC-API-CAMPAIGN-LIST-REQ-003 | L2       | RequiredCheck   | Missing Content-Type header validation                       | 1. User has completed account linkage                                                                                            | 1. Send GET /campaign/list<br>2. Omit Content-Type header<br>3. Include Authorization and Accept              | 1. HTTP 200 or 400<br>2. If 200: valid campaign data                                          | Not Run     |      |
| 7    | TC-API-CAMPAIGN-LIST-REQ-004 | L1       | RequiredCheck   | Empty Bearer token value                                     | 1. None                                                                                                                          | 1. Set Authorization: "Bearer "<br>2. Send GET /campaign/list                                                 | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 8    | TC-API-CAMPAIGN-LIST-REQ-005 | L1       | RequiredCheck   | Bearer token without prefix                                  | 1. None                                                                                                                          | 1. Set Authorization: "token_value_only"<br>2. Send GET /campaign/list                                        | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 9    | TC-API-CAMPAIGN-LIST-LEN-001 | L2       | LengthCheck     | Point amount boundary: zero value                            | 1. master_campaign has campaign with point_amount=0                                                                              | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. Campaign with point_amount=0 displayed as Number             | Not Run     |      |
| 10   | TC-API-CAMPAIGN-LIST-LEN-002 | L2       | LengthCheck     | Point amount boundary: maximum value                         | 1. master_campaign has campaign with point_amount=999999999                                                                      | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. Campaign with maximum point_amount displayed                 | Not Run     |      |
| 11   | TC-API-CAMPAIGN-LIST-LEN-003 | L2       | LengthCheck     | Campaign title with maximum length                           | 1. master_campaign has campaign with title length 255+ chars                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. Full title returned                                          | Not Run     |      |
| 12   | TC-API-CAMPAIGN-LIST-LEN-004 | L3       | LengthCheck     | Brief eligibility with many conditions                       | 1. master_campaign has campaign with 10+ eligibility conditions                                                                  | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. All brief_eligibility items returned                         | Not Run     |      |
| 13   | TC-API-CAMPAIGN-LIST-LEN-005 | L3       | LengthCheck     | Required documents with many items                           | 1. master_campaign has campaign with 5+ required_documents                                                                       | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. All required_documents returned                              | Not Run     |      |
| 14   | TC-API-CAMPAIGN-LIST-LEN-006 | L3       | LengthCheck     | Point grant date boundary: minimum date                      | 1. master_campaign has campaign with point_grant_date="1970-01-01"                                                               | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="1970-01-01"                                | Not Run     |      |
| 15   | TC-API-CAMPAIGN-LIST-LEN-007 | L4       | LengthCheck     | Point grant date boundary: maximum date                      | 1. master_campaign has campaign with point_grant_date="9999-12-31"                                                               | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. point_grant_date="9999-12-31"                                | Not Run     |      |
| 16   | TC-API-CAMPAIGN-LIST-VAL-001 | L2       | ValidationCheck | Bearer token with special characters                         | 1. None                                                                                                                          | 1. Set Authorization: "Bearer !@#$%^&*()"<br>2. Send GET /campaign/list                                       | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 17   | TC-API-CAMPAIGN-LIST-VAL-002 | L3       | ValidationCheck | Campaign title with special characters                       | 1. master_campaign has title with emoji/symbols                                                                                  | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. Title displays special characters                            | Not Run     |      |
| 18   | TC-API-CAMPAIGN-LIST-VAL-003 | L3       | ValidationCheck | Brief eligibility with special characters                    | 1. master_campaign has eligibility with \n and special chars                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. brief_eligibility preserves formatting                       | Not Run     |      |
| 19   | TC-API-CAMPAIGN-LIST-VAL-004 | L3       | ValidationCheck | Document title with special characters                       | 1. master_campaign has document title with special chars                                                                         | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. Document title displays special characters                   | Not Run     |      |
| 20   | TC-API-CAMPAIGN-LIST-VAL-005 | L1       | ValidationCheck | Point grant date format validation                           | 1. None (server-side validation)                                                                                                 | 1. Verify response point_grant_date                                                                           | 1. HTTP 200<br>2. All point_grant_date values match YYYY-MM-DD                                | Not Run     |      |
| 21   | TC-API-CAMPAIGN-LIST-VAL-006 | L3       | ValidationCheck | Campaign ID format validation                                | 1. master_campaign has campaigns with id=1,999,1000000                                                                           | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. All campaign ids are Integer                                 | Not Run     |      |
| 22   | TC-API-CAMPAIGN-LIST-VAL-007 | L1       | ValidationCheck | Result field is integer 0 on success                         | 1. User has completed account linkage                                                                                            | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0 (Integer)                                                          | Not Run     |      |
| 23   | TC-API-CAMPAIGN-LIST-LOG-001 | L1       | LogicCheck      | Campaign hidden if approved application exists               | 1. applications: user has campaign_id=1, status="Final approved"                                                                 | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200<br>2. result=0<br>3. campaign id=1 NOT in campaigns                               | Not Run     |      |
| 24   | TC-API-CAMPAIGN-LIST-LOG-002 | L1       | LogicCheck      | Campaign hidden if under review application                  | 1. applications: user has campaign_id=2, status="Under review"                                                                   | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=2 NOT in campaigns                               | Not Run     |      |
| 25   | TC-API-CAMPAIGN-LIST-LOG-003 | L1       | LogicCheck      | Campaign hidden if grant done application                    | 1. applications: user has campaign_id=3, status="Grant Done"                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=3 NOT in campaigns                               | Not Run     |      |
| 26   | TC-API-CAMPAIGN-LIST-LOG-004 | L2       | LogicCheck      | Campaign shown if rejected application                       | 1. applications: user has campaign_id=4, status="Rejected"                                                                       | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=4 IS in campaigns                                | Not Run     |      |
| 27   | TC-API-CAMPAIGN-LIST-LOG-005 | L2       | LogicCheck      | Campaign shown if withdrawn application                      | 1. applications: user has campaign_id=5, status="Withdrawn"                                                                      | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. result=0<br>3. campaign id=5 IS in campaigns                                | Not Run     |      |
| 28   | TC-API-CAMPAIGN-LIST-LOG-006 | L2       | LogicCheck      | New campaign appears after insertion                         | 1. master_campaign: insert new campaign id=100                                                                                   | 1. Send GET /campaign/list before insert<br>2. Insert new campaign<br>3. Send GET /campaign/list after insert | 1. First: campaign id=100 not present<br>2. Second: campaign id=100 present                   | Not Run     |      |
| 29   | TC-API-CAMPAIGN-LIST-LOG-007 | L1       | LogicCheck      | Multiple customer number isolation                           | 1. User has 2 customer_numbers<br>2. customer_A applied to campaign 1<br>3. customer_B hasn't applied                            | 1. Request with customer_A token<br>2. Request with customer_B token                                          | 1. customer_A: campaign 1 NOT in list<br>2. customer_B: campaign 1 IS in list                 | Not Run     |      |
| 30   | TC-API-CAMPAIGN-LIST-LOG-008 | L3       | LogicCheck      | Campaigns ordered by ID ascending                            | 1. master_campaign has campaigns with ids: 5,2,10,1                                                                              | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200<br>2. campaigns ordered by id: 1,2,5,10                                           | Not Run     |      |
| 31   | TC-API-CAMPAIGN-LIST-ERR-001 | L1       | ErrorCheck      | Invalid or expired Bearer token                              | 1. None                                                                                                                          | 1. Use expired/invalid token<br>2. Send GET /campaign/list                                                    | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 32   | TC-API-CAMPAIGN-LIST-ERR-002 | L1       | ErrorCheck      | Account linkage not completed (403)                          | 1. account_linkage: no record for user                                                                                           | 1. Use valid token, no linkage<br>2. Send GET /campaign/list                                                  | 1. HTTP 403<br>2. result=2<br>3. error_message="Please complete account linkage first."       | Not Run     |      |
| 33   | TC-API-CAMPAIGN-LIST-ERR-003 | L1       | ErrorCheck      | Database connection error (500)                              | 1. Database unavailable                                                                                                          | 1. Send GET /campaign/list<br>2. DB connection fails                                                          | 1. HTTP 500<br>2. result=3<br>3. error_message="A system error has occurred."                 | Not Run     |      |
| 34   | TC-API-CAMPAIGN-LIST-ERR-004 | L2       | ErrorCheck      | Service temporarily unavailable (503)                        | 1. Service under maintenance                                                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 503<br>2. result=4<br>3. error_message="The service is temporarily unavailable."      | Not Run     |      |
| 35   | TC-API-CAMPAIGN-LIST-ERR-005 | L2       | ErrorCheck      | Gateway or upstream timeout (504)                            | 1. Backend slow/unresponsive                                                                                                     | 1. Send GET /campaign/list                                                                                    | 1. HTTP 504<br>2. result=5<br>3. error_message="Request timed out."                           | Not Run     |      |
| 36   | TC-API-CAMPAIGN-LIST-ERR-006 | L1       | ErrorCheck      | Malformed Authorization header                               | 1. Authorization="Bearertoken123"                                                                                                | 1. Set Authorization: "Bearertoken123"<br>2. Send GET /campaign/list                                          | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 37   | TC-API-CAMPAIGN-LIST-ERR-007 | L1       | ErrorCheck      | Wrong authentication scheme                                  | 1. Authorization="Basic base64string"                                                                                            | 1. Set Authorization: "Basic base64string"<br>2. Send GET /campaign/list                                      | 1. HTTP 401<br>2. result=1<br>3. error_message="Authentication is required."                  | Not Run     |      |
| 38   | TC-API-CAMPAIGN-LIST-ERR-008 | L1       | ErrorCheck      | Response missing result field                                | 1. Backend omits result field                                                                                                    | 1. Send GET /campaign/list                                                                                    | 1. HTTP 200 (invalid structure)<br>2. Response must contain result field as Integer           | Not Run     |      |
| 39   | TC-API-CAMPAIGN-LIST-ERR-009 | L1       | ErrorCheck      | Response missing campaigns field on success                  | 1. Backend omits campaigns field                                                                                                 | 1. Send GET /campaign/list with valid token                                                                   | 1. HTTP 200 (invalid)<br>2. Response must contain campaigns array                             | Not Run     |      |
| 40   | TC-API-CAMPAIGN-LIST-ERR-010 | L2       | ErrorCheck      | Error response structure validation                          | 1. Backend 401 includes both result and campaigns                                                                                | 1. Send request with invalid token<br>2. Receive 401                                                          | 1. HTTP 401<br>2. Response should NOT contain campaigns field                                 | Not Run     |      |
