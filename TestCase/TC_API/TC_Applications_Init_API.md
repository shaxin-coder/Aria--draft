# Generate Temp Application ID API – Test Cases

| Spec           | `API_APPLICATIONS_INIT.md` |
| -------------- | -------------------------- |
| Endpoint       | POST `/member/api/v1/applications/init` |
| Authentication | Bearer token               |
| Note           | No DB write. Reads account_linkage only. |

---

## API overview

**API name:** Generate Temp Application ID API  
**Description:** Issue a temporary application ID (tmp_app_id) for the authenticated user to create a new draft application. The ID is used for temp-applications upload/list/delete and for final submit.

---

## Sample success response

```json
{
  "result": 0,
  "tmp_app_id": "018e1234-5678-7890-abcd-ef1234567890"
}
```

---

## Test Case Summary Matrix

| No.  | Test Case ID                         | Priority | Category        | Test Case Description                                              | Note |
| ---- | ------------------------------------ | -------- | --------------- | ------------------------------------------------------------------ | ---- |
| 1    | TC-API-APPLICATIONS-INIT-HP-001      | L1       | HappyPath       | Successfully generate tmp_app_id with valid token and linked X-Customer-Number |      |
| 2    | TC-API-APPLICATIONS-INIT-HP-002      | L1       | HappyPath       | Response returns 201 Created with result=0 and tmp_app_id           |      |
| 3    | TC-API-APPLICATIONS-INIT-HP-003      | L2       | HappyPath       | tmp_app_id is valid UUID format                                    |      |
| 4    | TC-API-APPLICATIONS-INIT-REQ-001     | L1       | RequiredCheck   | Missing Authorization header validation                           |      |
| 5    | TC-API-APPLICATIONS-INIT-REQ-002     | L1       | RequiredCheck   | Missing X-Customer-Number header validation                        |      |
| 6    | TC-API-APPLICATIONS-INIT-REQ-003     | L2       | RequiredCheck   | Missing Accept header validation                                   |      |
| 7    | TC-API-APPLICATIONS-INIT-REQ-004     | L2       | RequiredCheck   | Missing Content-Type header validation                             |      |
| 8    | TC-API-APPLICATIONS-INIT-REQ-005     | L1       | RequiredCheck   | Empty Bearer token value                                           |      |
| 9    | TC-API-APPLICATIONS-INIT-REQ-006     | L1       | RequiredCheck   | Bearer token without prefix                                        |      |
| 10   | TC-API-APPLICATIONS-INIT-REQ-007     | L1       | RequiredCheck   | Empty X-Customer-Number header value                               |      |
| 11   | TC-API-APPLICATIONS-INIT-LEN-001      | L2       | LengthCheck     | X-Customer-Number with minimum valid length                        |      |
| 12   | TC-API-APPLICATIONS-INIT-LEN-002      | L2       | LengthCheck     | X-Customer-Number with maximum valid length                        |      |
| 13   | TC-API-APPLICATIONS-INIT-LEN-003      | L3       | LengthCheck     | X-Customer-Number exceeding maximum length                         |      |
| 14   | TC-API-APPLICATIONS-INIT-VAL-001     | L1       | ValidationCheck | X-Customer-Number with invalid format                               |      |
| 15   | TC-API-APPLICATIONS-INIT-VAL-002     | L2       | ValidationCheck | X-Customer-Number with special characters                          |      |
| 16   | TC-API-APPLICATIONS-INIT-VAL-003     | L1       | ValidationCheck | tmp_app_id in response is UUID format                              |      |
| 17   | TC-API-APPLICATIONS-INIT-VAL-004     | L1       | ValidationCheck | result field is integer 0 on success                                |      |
| 18   | TC-API-APPLICATIONS-INIT-LOG-001     | L1       | LogicCheck      | X-Customer-Number linked to user returns success                   |      |
| 19   | TC-API-APPLICATIONS-INIT-LOG-002     | L1       | LogicCheck      | X-Customer-Number not linked to user returns 403                    |      |
| 20   | TC-API-APPLICATIONS-INIT-LOG-003     | L1       | LogicCheck      | Account linkage not completed returns 403                          |      |
| 21   | TC-API-APPLICATIONS-INIT-LOG-004     | L2       | LogicCheck      | Each call generates unique tmp_app_id                               |      |
| 22   | TC-API-APPLICATIONS-INIT-LOG-005     | L2       | LogicCheck      | Multiple customer numbers: user can init with each linked customer |      |
| 23   | TC-API-APPLICATIONS-INIT-ERR-001     | L1       | ErrorCheck      | Invalid or expired Bearer token returns 401                         |      |
| 24   | TC-API-APPLICATIONS-INIT-ERR-002     | L1       | ErrorCheck      | Invalid/missing X-Customer-Number returns 400 (result 1)           |      |
| 25   | TC-API-APPLICATIONS-INIT-ERR-003     | L1       | ErrorCheck      | Account linkage not completed returns 403 (result 4)                |      |
| 26   | TC-API-APPLICATIONS-INIT-ERR-004     | L1       | ErrorCheck      | Database connection error returns 500 (result 2)                   |      |
| 27   | TC-API-APPLICATIONS-INIT-ERR-005     | L2       | ErrorCheck      | Service temporarily unavailable returns 503 (result 6)              |      |
| 28   | TC-API-APPLICATIONS-INIT-ERR-006     | L2       | ErrorCheck      | Gateway or upstream timeout returns 504 (result 7)                  |      |
| 29   | TC-API-APPLICATIONS-INIT-ERR-007     | L1       | ErrorCheck      | Malformed Authorization header returns 401                         |      |
| 30   | TC-API-APPLICATIONS-INIT-ERR-008     | L1       | ErrorCheck      | Wrong authentication scheme (Basic) returns 401                      |      |
| 31   | TC-API-APPLICATIONS-INIT-ERR-009     | L1       | ErrorCheck      | Response missing result field on success                           |      |
| 32   | TC-API-APPLICATIONS-INIT-ERR-010     | L1       | ErrorCheck      | Response missing tmp_app_id field on success                       |      |

**Total Test Cases: 32**

---

## Test Cases Steps

| No.  | Test Case ID                         | Priority | Category        | Test Case Description                                              | Given                                                        | When                                                         | Then                                                         | Case Status | Note |
| ---- | ------------------------------------ | -------- | --------------- | ------------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------- | ---- |
| 1    | TC-API-APPLICATIONS-INIT-HP-001      | L1       | HappyPath       | Successfully generate tmp_app_id with valid token and linked X-Customer-Number | 1. User has completed account linkage<br>2. X-Customer-Number is linked to user in account_linkage | 1. Send POST /member/api/v1/applications/init<br>2. Include valid Bearer token<br>3. Include X-Customer-Number, Accept, Content-Type headers | 1. HTTP 201 Created<br>2. result=0<br>3. tmp_app_id present (UUID string) | Not Run     |      |
| 2    | TC-API-APPLICATIONS-INIT-HP-002      | L1       | HappyPath       | Response returns 201 Created with result=0 and tmp_app_id           | 1. User has completed account linkage<br>2. X-Customer-Number linked to user | 1. Send POST /member/api/v1/applications/init with all required headers | 1. HTTP 201 Created<br>2. result=0 (Integer)<br>3. tmp_app_id (String) | Not Run     |      |
| 3    | TC-API-APPLICATIONS-INIT-HP-003      | L2       | HappyPath       | tmp_app_id is valid UUID format                                     | 1. User has completed account linkage                        | 1. Send POST /member/api/v1/applications/init with valid token | 1. HTTP 201<br>2. tmp_app_id matches UUID pattern (e.g. xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx) | Not Run     |      |
| 4    | TC-API-APPLICATIONS-INIT-REQ-001     | L1       | RequiredCheck   | Missing Authorization header validation                             | 1. None                                                      | 1. Send POST /member/api/v1/applications/init<br>2. Omit Authorization header<br>3. Include X-Customer-Number, Accept, Content-Type | 1. HTTP 401<br>2. result=3<br>3. error_message="Authentication is required." | Not Run     |      |
| 5    | TC-API-APPLICATIONS-INIT-REQ-002     | L1       | RequiredCheck   | Missing X-Customer-Number header validation                         | 1. User has completed account linkage                        | 1. Send POST /member/api/v1/applications/init<br>2. Omit X-Customer-Number header<br>3. Include valid Bearer, Accept, Content-Type | 1. HTTP 400<br>2. result=1<br>3. error_message="Request parameter is invalid." | Not Run     |      |
| 6    | TC-API-APPLICATIONS-INIT-REQ-003     | L2       | RequiredCheck   | Missing Accept header validation                                    | 1. User has completed account linkage<br>2. X-Customer-Number linked | 1. Send POST /member/api/v1/applications/init<br>2. Omit Accept header<br>3. Include Authorization, X-Customer-Number, Content-Type | 1. HTTP 400 or 201<br>2. If 201: valid response with tmp_app_id | Not Run     |      |
| 7    | TC-API-APPLICATIONS-INIT-REQ-004     | L2       | RequiredCheck   | Missing Content-Type header validation                              | 1. User has completed account linkage<br>2. X-Customer-Number linked | 1. Send POST /member/api/v1/applications/init<br>2. Omit Content-Type header<br>3. Include Authorization, X-Customer-Number, Accept | 1. HTTP 400 or 201<br>2. If 201: valid response with tmp_app_id | Not Run     |      |
| 8    | TC-API-APPLICATIONS-INIT-REQ-005     | L1       | RequiredCheck   | Empty Bearer token value                                            | 1. None                                                      | 1. Set Authorization: "Bearer "<br>2. Send POST /member/api/v1/applications/init with X-Customer-Number | 1. HTTP 401<br>2. result=3<br>3. error_message="Authentication is required." | Not Run     |      |
| 9    | TC-API-APPLICATIONS-INIT-REQ-006     | L1       | RequiredCheck   | Bearer token without prefix                                         | 1. None                                                      | 1. Set Authorization: "token_value_only"<br>2. Send POST /member/api/v1/applications/init | 1. HTTP 401<br>2. result=3<br>3. error_message="Authentication is required." | Not Run     |      |
| 10   | TC-API-APPLICATIONS-INIT-REQ-007     | L1       | RequiredCheck   | Empty X-Customer-Number header value                                | 1. User has completed account linkage                        | 1. Set X-Customer-Number: ""<br>2. Send POST /member/api/v1/applications/init with valid token | 1. HTTP 400<br>2. result=1<br>3. error_message="Request parameter is invalid." | Not Run     |      |
| 11   | TC-API-APPLICATIONS-INIT-LEN-001      | L2       | LengthCheck     | X-Customer-Number with minimum valid length                         | 1. account_linkage has customer_number with min valid length | 1. Send POST with X-Customer-Number at min length (e.g. 1 char if allowed) | 1. HTTP 201<br>2. result=0<br>3. tmp_app_id returned | Not Run     |      |
| 12   | TC-API-APPLICATIONS-INIT-LEN-002      | L2       | LengthCheck     | X-Customer-Number with maximum valid length                         | 1. account_linkage has customer_number at max length         | 1. Send POST with X-Customer-Number at max allowed length | 1. HTTP 201<br>2. result=0<br>3. tmp_app_id returned | Not Run     |      |
| 13   | TC-API-APPLICATIONS-INIT-LEN-003      | L3       | LengthCheck     | X-Customer-Number exceeding maximum length                          | 1. None                                                      | 1. Send POST with X-Customer-Number exceeding max length (e.g. 1000+ chars) | 1. HTTP 400<br>2. result=1<br>3. error_message="Request parameter is invalid." | Not Run     |      |
| 14   | TC-API-APPLICATIONS-INIT-VAL-001     | L1       | ValidationCheck | X-Customer-Number with invalid format                               | 1. None                                                      | 1. Set X-Customer-Number: "invalid_format_123!@#"<br>2. Send POST with valid token | 1. HTTP 400 or 403<br>2. result=1 or 4<br>3. Appropriate error_message | Not Run     |      |
| 15   | TC-API-APPLICATIONS-INIT-VAL-002     | L2       | ValidationCheck | X-Customer-Number with special characters                           | 1. None                                                      | 1. Set X-Customer-Number: "cust<>\"&"<br>2. Send POST with valid token | 1. HTTP 400 or 403<br>2. result=1 or 4<br>3. No injection/error handling | Not Run     |      |
| 16   | TC-API-APPLICATIONS-INIT-VAL-003     | L1       | ValidationCheck | tmp_app_id in response is UUID format                               | 1. User has completed account linkage                        | 1. Send POST /member/api/v1/applications/init successfully | 1. HTTP 201<br>2. tmp_app_id matches UUID v4 or v7 format | Not Run     |      |
| 17   | TC-API-APPLICATIONS-INIT-VAL-004     | L1       | ValidationCheck | result field is integer 0 on success                                | 1. User has completed account linkage                        | 1. Send POST /member/api/v1/applications/init successfully | 1. HTTP 201<br>2. result=0 (Integer type) | Not Run     |      |
| 18   | TC-API-APPLICATIONS-INIT-LOG-001     | L1       | LogicCheck      | X-Customer-Number linked to user returns success                   | 1. account_linkage: (easy_id, customer_number) exists       | 1. Send POST with that X-Customer-Number and valid token | 1. HTTP 201<br>2. result=0<br>3. tmp_app_id returned | Not Run     |      |
| 19   | TC-API-APPLICATIONS-INIT-LOG-002     | L1       | LogicCheck      | X-Customer-Number not linked to user returns 403                    | 1. account_linkage: customer_number exists but not for this user | 1. Send POST with that X-Customer-Number and valid token | 1. HTTP 403<br>2. result=4<br>3. error_message="Please complete account linkage first." | Not Run     |      |
| 20   | TC-API-APPLICATIONS-INIT-LOG-003     | L1       | LogicCheck      | Account linkage not completed returns 403                           | 1. account_linkage: no record for user                       | 1. Send POST with valid token and any X-Customer-Number | 1. HTTP 403<br>2. result=4<br>3. error_message="Please complete account linkage first." | Not Run     |      |
| 21   | TC-API-APPLICATIONS-INIT-LOG-004     | L2       | LogicCheck      | Each call generates unique tmp_app_id                               | 1. User has completed account linkage                        | 1. Send POST twice with same headers                         | 1. Both return HTTP 201<br>2. Two different tmp_app_id values | Not Run     |      |
| 22   | TC-API-APPLICATIONS-INIT-LOG-005     | L2       | LogicCheck      | Multiple customer numbers: user can init with each linked customer  | 1. User has 2 customer_numbers in account_linkage            | 1. Send POST with customer_A<br>2. Send POST with customer_B | 1. Both return HTTP 201<br>2. Each returns unique tmp_app_id | Not Run     |      |
| 23   | TC-API-APPLICATIONS-INIT-ERR-001     | L1       | ErrorCheck      | Invalid or expired Bearer token returns 401                         | 1. None                                                      | 1. Use expired/invalid token<br>2. Send POST with X-Customer-Number | 1. HTTP 401<br>2. result=3<br>3. error_message="Authentication is required." | Not Run     |      |
| 24   | TC-API-APPLICATIONS-INIT-ERR-002     | L1       | ErrorCheck      | Invalid/missing X-Customer-Number returns 400 (result 1)            | 1. None                                                      | 1. Send POST with invalid or missing X-Customer-Number | 1. HTTP 400<br>2. result=1<br>3. error_message="Request parameter is invalid." | Not Run     |      |
| 25   | TC-API-APPLICATIONS-INIT-ERR-003     | L1       | ErrorCheck      | Account linkage not completed returns 403 (result 4)                | 1. account_linkage: no record for user                       | 1. Send POST with valid token and valid format X-Customer-Number | 1. HTTP 403<br>2. result=4<br>3. error_message="Please complete account linkage first." | Not Run     |      |
| 26   | TC-API-APPLICATIONS-INIT-ERR-004     | L1       | ErrorCheck      | Database connection error returns 500 (result 2)                    | 1. Database unavailable                                      | 1. Send POST /member/api/v1/applications/init | 1. HTTP 500<br>2. result=2<br>3. error_message="A system error has occurred." | Not Run     |      |
| 27   | TC-API-APPLICATIONS-INIT-ERR-005     | L2       | ErrorCheck      | Service temporarily unavailable returns 503 (result 6)              | 1. Service under maintenance                                 | 1. Send POST /member/api/v1/applications/init | 1. HTTP 503<br>2. result=6<br>3. error_message="The service is temporarily unavailable." | Not Run     |      |
| 28   | TC-API-APPLICATIONS-INIT-ERR-006     | L2       | ErrorCheck      | Gateway or upstream timeout returns 504 (result 7)                  | 1. Backend slow/unresponsive                                 | 1. Send POST /member/api/v1/applications/init | 1. HTTP 504<br>2. result=7<br>3. error_message="Request timed out." | Not Run     |      |
| 29   | TC-API-APPLICATIONS-INIT-ERR-007     | L1       | ErrorCheck      | Malformed Authorization header returns 401                          | 1. None                                                      | 1. Set Authorization: "Bearertoken123"<br>2. Send POST | 1. HTTP 401<br>2. result=3<br>3. error_message="Authentication is required." | Not Run     |      |
| 30   | TC-API-APPLICATIONS-INIT-ERR-008     | L1       | ErrorCheck      | Wrong authentication scheme (Basic) returns 401                     | 1. None                                                      | 1. Set Authorization: "Basic base64string"<br>2. Send POST | 1. HTTP 401<br>2. result=3<br>3. error_message="Authentication is required." | Not Run     |      |
| 31   | TC-API-APPLICATIONS-INIT-ERR-009     | L1       | ErrorCheck      | Response missing result field on success                            | 1. Backend omits result field                                | 1. Send POST with valid request                             | 1. HTTP 201 (invalid structure)<br>2. Response must contain result field as Integer | Not Run     |      |
| 32   | TC-API-APPLICATIONS-INIT-ERR-010     | L1       | ErrorCheck      | Response missing tmp_app_id field on success                       | 1. Backend omits tmp_app_id field                            | 1. Send POST with valid request                             | 1. HTTP 201 (invalid)<br>2. Response must contain tmp_app_id as String | Not Run     |      |
