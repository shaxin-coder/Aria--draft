## Project defination

### Case ID：

  1. HappyPath--->HP
  2. RequiredCheck--->REQ
  3. LengthCheck--->LEN
  4. ValidationCheck--->VAL
  5. LogicCheck--->LOG
  6. ErrorCheck--->ERR

- **API**：
  TC-API-[APINAME]-[TYPE]-[NUMBER]
  1. TC-API-CAMPAIGN-LIST-HP-001
  2. TC-API-CAMPAIGN-LIST-REQ-001

- **Batch**：
  TC-BATCH-[JOBNAME]-[TYPE]-[NUMBER]
  1. TC-BATCH-POINT-GRANT-LEN-001
  2. TC-BATCH-POINT-GRANT-VAL-001

- **UI**:
  TC-UI-[ModuleName]-[TYPE]-[NUMBER]
  1. TC-UI-IdLinkage-LOG-001
  2. TC-UI-IdLinkage-ERR-001

### Case status：

- Not Run 
- Pass
- Fail 
- Blocked 
- OutOfScope

### Priority:

| Level | Description                  |
| ----- | ---------------------------- |
| L1    | Critical/Blocker/Smoke       |
| L2    | High/Must-Have/Scenario      |
| L3    | Medium/Important/Should-Have |
| L4    | Low/Minor/Nice-Have          |

### Category：

| Category        | Core Focus                                                 | Test Essence                              |
| --------------- | ---------------------------------------------------------- | ----------------------------------------- |
| Happy Path      | Valid inputs + normal process                              | Verify "functionality availability"       |
| RequiredCheck   | Mandatory parameters/headers are missing or empty          | Verify "mandatory field validation rules" |
| LengthCheck     | Parameter length boundaries (minimum / maximum/boundaries) | Verify "length limit rules"               |
| ValidationCheck | Parameter formats, data types, and special characters      | Verify "format validation rules"          |
| LogicCheck      | Business rule relevance                                    | Verify "logical correctness"              |
| ErrorCheck      | Abnormal inputs / environments / states                    | Verify "error handling capability"        |


