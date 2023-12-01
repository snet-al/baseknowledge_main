**What is a Test Case?**

A test case is a set of actions that verify whether the software application is working per the client's requirements.

**Types of testing**

**Functional Testing:** Functional testing focuses on verifying the software's functionality against the specified requirements. It involves testing individual functions, features, and their interactions within the system. Testers validate inputs, expected outputs, and ensure that the software performs as intended.

**Regression Testing:** Regression testing is performed to ensure that modifications or enhancements to the software do not introduce new defects or break existing functionality. It involves retesting previously tested areas to validate that the changes have not caused any adverse impacts. Regression testing helps maintain software stability and reliability over time.

**Performance Testing:** Performance testing assesses the software's performance characteristics, such as its responsiveness, scalability, and resource usage under various workload conditions. It aims to identify performance bottlenecks, measure response times, and determine if the system meets the desired performance criteria. Performance testing can include load testing, stress testing, and endurance testing.

**Security Testing:** Security testing focuses on identifying vulnerabilities and weaknesses in the software's security mechanisms. It involves testing for potential threats, unauthorized access, data breaches, and confidentiality issues. Security testing aims to ensure that the software has appropriate measures in place to protect sensitive data and maintain the integrity of the system.

**User interface (UI) test cases:** It's important to remember that the user interface is part of the overall system and not just a shell where functionality appears. UI test cases check that each UI element works correctly, displays, and is easy to use.

**User acceptance test cases:** User acceptance test cases verify that an application satisfies its business requirements before users accept it. These determine whether users accept or reject the output produced by a particular system before release to the live environment.

**Test case format**

**Test Case ID** - This field uniquely identifies a test case.

**Test case Description/Summary** - This field describes the test case objective

**Pre-condition** - This field specifies the conditions or steps that must be followed before the test steps executions.

**Test steps** - In this field, the exact steps are mentioned for performing the test case.

**What is a Test Case?**

A test case is a set of actions that verify whether the software application is working per the client's requirements.

**Types of testing**

**Functional Testing:** Functional testing focuses on verifying the software's functionality against the specified requirements. It involves testing individual functions, features, and their interactions within the system. Testers validate inputs, expected outputs, and ensure that the software performs as intended.

**Regression Testing:** Regression testing is performed to ensure that modifications or enhancements to the software do not introduce new defects or break existing functionality. It involves retesting previously tested areas to validate that the changes have not caused any adverse impacts. Regression testing helps maintain software stability and reliability over time.

**Performance Testing:** Performance testing assesses the software's performance characteristics, such as its responsiveness, scalability, and resource usage under various workload conditions. It aims to identify performance bottlenecks, measure response times, and determine if the system meets the desired performance criteria. Performance testing can include load testing, stress testing, and endurance testing.

**Security Testing:** Security testing focuses on identifying vulnerabilities and weaknesses in the software's security mechanisms. It involves testing for potential threats, unauthorized access, data breaches, and confidentiality issues. Security testing aims to ensure that the software has appropriate measures in place to protect sensitive data and maintain the integrity of the system.

**Usability test cases:** Usability test cases help check if users can use the application successfully. These determine whether users can easily use the system without difficulty or confusion. They also verify if users can navigate the system using common procedures and functions.

**User acceptance test cases:** User acceptance test cases verify that an application satisfies its business requirements before users accept it. These determine whether users accept or reject the output produced by a particular system before release to the live environment.

**User interface (UI) test cases:** It's important to remember that the user interface is part of the overall system and not just a shell where functionality appears. UI test cases check that each UI element works correctly, displays, and is easy to use.

**Test case format**

**Test Case ID -** This field uniquely identifies a test case.

**Test case Description/Summary -** This field describes the test case objective.

**Pre-requisites -** This field specifies the conditions or steps that must be followed before the test steps executions

**Test steps -** In this field, the exact steps are mentioned for performing the test case.

**Test data** refers to the input data or values required to execute the test case. For example, username and password are the test data to test the email login.

**Expected result -** This field should define what you expect to see and is how you determine if the test passes or fails.

**Actual result -** This field should define what you is actual results so that you determine if the test passes or fails.


| Test Case ID | Test case Description/Summary                               | Pre-condition                              | Test steps                                                                      | Test Data                                      | Expected result              | Actual result        |
|--------------|-------------------------------------------------------------|--------------------------------------------|---------------------------------------------------------------------------------|------------------------------------------------|------------------------------|----------------------|
| TU01         | Check user  login when  email ID and  password are  entered | https://id.testsigma.com/ui/loginis opened | 1.Enter email ID 2. Enter password 3. Click submit User should be able to login | Email – sample@gmail.com Password – Sample@123 | User should be able to login | Login was successful |
