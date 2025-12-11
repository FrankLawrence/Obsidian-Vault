---
Tags: 
Created: 2024-06-24 17:21:40
---
(Links:: [[Software Engineering Processes]])
Types of testing in terms of purpose
- Functional testing
- User testing
- Performance and load testing
- Security testing

# Functional Testing
![[Functional testing.canvas]]
## Black-box testing
- **Equivalence partitioning**: This involves the identification of sets of inputs that will be treated in the same way by the program.
- **Boundary value analysis**: Based on testing the boundary values of valid and invalid partitions. The behavior at the edge of the equivalence partition is more likely to be incorrect than the behavior within the partition
## White-box testing
- **Statement coverage testing**: Statement coverage is a technique in which all the executable statements in the source code are executed at least once.
- **Branch coverage testing**: A branch is the outcome of a decision (e.g. an if statement and a loop control statement), so branch coverage simply measures which decision outcomes have been tested.

# Test Automation
Automated testing is based on the idea that tests should be executable. An executable test includes the input data to the unit that is being tested, the expected result, and a check that the unit returns the expected result. You run the test and the test passes if the unit returns the expected result.

It is a good practice to structure automated tests into three parts:
1. **Arrange**: You set up the system to run the test. This involves defining the test parameters.
2. **Action**: You call the unit that is being tested with the test parameters.
3. **Assert**: You make an assertion about what should hold if the unit being tested has executed successfully.
# Test-driven Development (TDD)
Test-driven development is an approach to program development that is based around the general idea that you should write an executable test or tests for code that you are writing before you write the code.

Test-driven development works best for the development of individual program units.
Given a functionality, the system should have:
1. **Identify partial implementation**: Break down the implementation of the functionality required into smaller mini-units. Choose one of these mini-units for implementation.
2. **Write mini-unit tests**: Write one or more automated tests for the mini-unit that you have chosen for implementation.
3. **Write an incomplete code that will fail test**: Write incomplete code that will be called to implement the mini-unit. You know this will fail at first.
4. **Run all existing automated tests**: All previous tests should pass. The test for the incomplete code should fail.
5. **Implement code that should cause the failing test to pass**: Write code to implement the mini-unit, which should cause it to operate correctly
6. **Rerun all automated tests**: If any tests fail, your code is probably incorrect. Keep working on it until all tests pass.

---
References: