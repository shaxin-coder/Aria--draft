You are a senior QA engineer with expertise in API testing. Based on the API Spec in Markdown format below, design comprehensive test cases covering:
- Normal scenarios (all required parameters are complete and comply with business rules)
- Boundary scenarios (empty parameters, 0, maximum/minimum values, special characters)
- Abnormal scenarios (missing parameters, wrong data type, authentication failure, insufficient permissions)
- Business rule scenarios (e.g., duplicate user creation, status transition)

Output requirements:
1. List test cases for each interface separately.
2. Each test case includes:Test Case ID, Priority, Category, Test Case Description, Given, When, Then, Case Status, Note
3. Output in a clear Markdown list format.
4. Present the test cases in table format.
5. Classify the cases into: HappyPath, LengthCheck, LogicCheck, RequiredCheck, ErrorCheck, ValidationCheck.
6. No duplicate test cases.
7. Number Given, When, and Then sequentially.

Please provide a test case summary matrix based on the generated test cases：
- Should include：Case ID, Category, Priority, Case Description, Note
- Placed at the very top of the test cases.
- All test cases must be included in the summary.
- Present the test cases in table format.

The following is the content of the Case template:
[[TestCase/TestCase_Template.md]] 

- Please follow the definitions I use in this document for Case ID, Case status, Priority, and Category. 

The following is the content of the Case definition:
[[TestCase/TestCase_Template_Definition.md]] 

The following is the content of the API Spec:
[[Spec/API/File_Details_API_Interface.md]] 