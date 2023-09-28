# Functional Testing Process 
## Purpose

This document details the validation process for delivery of components to Prometheus X.
To be accepted, components must demonstrate that they perform as expected at initial delivery.
This process aims to be automated to prevent regressions in future releases.
Components are expected to integrate with other Prometheus X components and should provide consistent APIs.
Scenarii defined for functional testing should used foundations for [interoperability tests](interoperability-test.md). 

## Definition of Functional Test Scenarii and Success Indicators:

* **Test Scenarii:** Functional test scenarii are detailed, step-by-step procedures and operations that are executed to validate that a software service component performs as intended. These tests will focus on black box testing, meaning that the inner workings of the application aren't considered; only the output of the action is tested.
  
* **Success Indicators:** These are the predetermined and expected outcomes for each scenario. If the outcome matches the success indicator, the test is considered successful.


## Writing Test Scenarii

* **Automated Scenarii:**
    * Defined in GitHub repositories as code.
    * GitHub Actions will be used to instantiate test environments using docker-compose.yml in the repository, and load data used in the test environment
    * k6 will be employed as the client tool to simulate user behavior with various levels of traffic and generate reports.
    * These tests should include setup, execution, and teardown phases.

* **Manual Testing:**
    * To be used only when automation is infeasible or when the test scenario requires human judgment.
    * Document each manual test scenario with detailed steps and expected outcomes for consistency.


## Test Execution and Report Production

* **Automated Test Execution:**
    * GitHub Actions will trigger tests upon commits or scheduled intervals.
    * k6 will execute the defined load tests.
    * Any failures or anomalies are logged automatically.

* **Manual Test Execution:**
    * The tester follows the defined steps and records the outcomes.
    * Any deviations from the expected outcomes are documented.

* **Report Production:**
    * For automated tests: GitHub Actions will produce a report containing details about each executed test, including success or failure status, logs, and any anomalies.
    * For manual tests: The tester will produce a report detailing the outcomes, deviations, and any additional observations.


## Validation by Technical Committee

* All reports are submitted by email to the technical committee for review.
* The technical committee will evaluate the results against the defined success indicators within 2 weeks after notification.
* If all success indicators are met, the component is validated for delivery.
* Any deviations or anomalies will require corrective action and retesting.

## Actors

- **CL (Component Lead):** : From the component team, owns the component, product manager/owner.
- **CTE (Component Testing Engineer):** : From the component teams, may be a developer or QA ; write and validate tests.
- **PA (Prometheus X Architect):** : Support for testing, provides capacity for running tests
- **TC (Technical Committee):** : Prometheus X technical committee and interested parties. They validate the results within 15 days after notification.

## RACI Matrix

| Task/Role                     | CL | CTE | PA | TC |
|-------------------------------|----|-----|----|----|
| Define Test Scenarii          | A  | R   | C  | I  |
| Define Success Indicators     | A  | R   | C  | I  |
| Write Automated Tests         | A  | R   | C  | I  |
| Write Manual Tests            | A  | R   | C  | I  |
| Execute Automated Tests       | I  | R/A | C  | I  |
| Execute Manual Tests          | I  | R/A | C  | I  |
| Produce Test Reports          | A  | R   | I  | I  |
| Review and Validate Reports   | C  | I   | C  | R/A|

**RACI Legend:**


- **R (Responsible):** Person(s) who actually perform the task.
- **A (Accountable):** Person who is the "owner" of the task and accountable for the completion of the task. There should be only one accountable person per task.
- **C (Consulted):** Person(s) who provide input based on their expertise and can influence the outcome. Two-way communication.
- **I (Informed):** Person(s) who need to be informed of the task's outcome. One-way communication.
