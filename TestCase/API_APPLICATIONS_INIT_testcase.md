# Generate Temp Application ID – Test Cases

| Spec           | `API_APPLICATIONS_INIT.md` |
|----------------|-----------------------------|
| Endpoint       | POST `/member/api/v1/applications/init` |
| Authentication | Bearer token |
| Note           | No DB update. |

---

## API overview

**API name:** Generate Temp Application ID  
Issue a temporary application ID (tmp_app_id) for the authenticated user to create a new draft application. The ID is used for temp-applications upload/list/delete and for final submit.

---

## Sample success response

```json
{
  "result": 0,
  "tmp_app_id": "018e1234-5678-7890-abcd-ef1234567890"
}
```

---

## Test case levels

| Level | Description                     |
|-------|---------------------------------|
| L1    | Critical/Blocker/Smoke          |
| L2    | High/Must-Have/Scenario         |
| L3    | Medium/Important/Should-Have    |

---

## Test case summary

| CaseId | Level | Test Description                               | Verification Points                                                 | Note |
| ------ | ----- | ---------------------------------------------- | ------------------------------------------------------------------- | ---- |
| TC-001 | L1    | Generate temporary application ID successfully | Verify API returns `201 Created`, `result=0`, and `tmp_app_id` UUID |      |
| TC-002 | L1    | Missing Authorization header                   | Verify API returns `401 Unauthorized` with `result=3`               |      |
| TC-003 | L1    | Invalid Bearer token                           | Verify API returns `401 Unauthorized` with correct error message    |      |
| TC-004 | L1    | Expired Bearer token                           | Verify API returns `401 Unauthorized`                               |      |
| TC-005 | L1    | Missing X-Customer-Number header               | Verify API returns `400 Bad Request` with `result=1`                |      |
| TC-006 | L1    | Empty X-Customer-Number header                 | Verify API returns `400 Bad Request`                                |      |
| TC-007 | L1    | Customer not linked to user                    | Verify API returns `403 Forbidden` with `result=4`                  |      |
| TC-008 | L3    | Wrong HTTP method (GET request)                | Verify API returns `405 Method Not Allowed`                         |      |
| TC-009 | L3    | Wrong HTTP method (PUT request)                | Verify API returns `405 Method Not Allowed`                         |      |
| TC-010 | L3    | Wrong HTTP method (DELETE request)             | Verify API returns `405 Method Not Allowed`                         |      |
| TC-011 | L1    | Internal server error occurs                   | Verify API returns `500 Internal Server Error` with `result=2`      |      |
| TC-012 | L1    | Service temporarily unavailable                | Verify API returns `503 Service Unavailable` with `result=6`        |      |
| TC-013 | L1    | Gateway timeout occurs                         | Verify API returns `504 Gateway Timeout` with `result=7`            |      |
| TC-014 | L2    | End-to-end success scenario                    | Verify API returns `201 Created` with valid `tmp_app_id`            |      |
| TC-015 | L1    | Invalid customer number format                 | Verify API returns `400 Bad Request` with `result=1`                |      |
| TC-016 | L1    | Invalid authentication token                   | Verify API returns `401 Unauthorized` with `result=3`               |      |
| TC-017 | L1    | Customer linkage verification failed           | Verify API returns `403 Forbidden` with `result=4`                  |      |
| TC-018 | L1    | Successful process flow validation             | Verify API returns `201 Created` with `result=0` and `tmp_app_id`   |      |
| TC-019 | L2    | Response schema validation                     | Verify response contains `result` and valid UUID `tmp_app_id`       |      |

---

## Test cases

### Positive case (success path, 200/201, result=0)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-001  | L1    | Valid Bearer token and X-Customer-Number linked to the user | POST request to `/member/api/v1/applications/init` | 201 Created with `result=0` and `tmp_app_id` in response. No DB update. |

---

### Negative case – Required / Auth (header)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-002  | L1    | Missing Authorization header | POST request to `/member/api/v1/applications/init` | 401 Unauthorized with `result=3` and `error_message="Authentication is required."` |
| TC-003  | L1    | Invalid Bearer token | POST request to `/member/api/v1/applications/init` | 401 Unauthorized with `result=3` and `error_message="Authentication is required."` |
| TC-004  | L1    | Expired Bearer token | POST request to `/member/api/v1/applications/init` | 401 Unauthorized with `result=3` and `error_message="Authentication is required."` |

---

### Negative case – Required check (parameter)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-005  | L1    | Missing X-Customer-Number header | POST request to `/member/api/v1/applications/init` | 400 Bad Request with `result=1` and `error_message="Request parameter is invalid."` |
| TC-006  | L1    | Empty X-Customer-Number header | POST request to `/member/api/v1/applications/init` | 400 Bad Request with `result=1` and `error_message="Request parameter is invalid."` |

---

### Negative case – Forbidden (403, account linkage, access denied)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-007  | L1    | Valid Bearer token but X-Customer-Number not linked to the user | POST request to `/member/api/v1/applications/init` | 403 Forbidden with `result=4` and `error_message="Please complete account linkage first."` |

---

### Negative case – Wrong HTTP method (405/400 for POST/PUT/DELETE on wrong path)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-008  | L3    | Valid Bearer token and X-Customer-Number | GET request to `/member/api/v1/applications/init` | 405 Method Not Allowed with appropriate error message. |
| TC-009  | L3    | Valid Bearer token and X-Customer-Number | PUT request to `/member/api/v1/applications/init` | 405 Method Not Allowed with appropriate error message. |
| TC-010  | L3    | Valid Bearer token and X-Customer-Number | DELETE request to `/member/api/v1/applications/init` | 405 Method Not Allowed with appropriate error message. |

---

### Error case – Server / gateway

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-011  | L1    | Valid Bearer token and X-Customer-Number | Internal server error occurs | 500 Internal Server Error with `result=2` and `error_message="A system error has occurred."` |
| TC-012  | L1    | Valid Bearer token and X-Customer-Number | Service temporarily unavailable | 503 Service Unavailable with `result=6` and `error_message="The service is temporarily unavailable."` |
| TC-013  | L1    | Valid Bearer token and X-Customer-Number | Gateway timeout occurs | 504 Gateway Timeout with `result=7` and `error_message="Request timed out."` |

---

### Scenario case (end-to-end success/alternate path)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-014  | L2    | Valid Bearer token and X-Customer-Number linked to the user | POST request to `/member/api/v1/applications/init` | 201 Created with `result=0` and `tmp_app_id` in response. No DB update. |

---

### Flow chart – Process Flow node confirmation

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-015  | L1    | Valid Bearer token and X-Customer-Number | X-Customer-Number is invalid | 400 Bad Request with `result=1` and `error_message="Request parameter is invalid."` |
| TC-016  | L1    | Valid Bearer token and X-Customer-Number | Bearer token is invalid | 401 Unauthorized with `result=3` and `error_message="Authentication is required."` |
| TC-017  | L1    | Valid Bearer token and X-Customer-Number | X-Customer-Number is not linked to the user | 403 Forbidden with `result=4` and `error_message="Please complete account linkage first."` |
| TC-018  | L1    | Valid Bearer token and X-Customer-Number | All checks pass | 201 Created with `result=0` and `tmp_app_id` in response. No DB update. |

---

### Response shape (success)

| Case ID | Level | GIVEN | WHEN | THEN |
|---------|-------|-------|------|------|
| TC-019  | L2    | Valid Bearer token and X-Customer-Number linked to the user | POST request to `/member/api/v1/applications/init` | Response includes `result=0` and `tmp_app_id` (UUID). No DB update. |
