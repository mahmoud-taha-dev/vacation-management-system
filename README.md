# **Vacation Tracking System (VTS)**

**Domain**

In large organizations, managing vacation time is manual, inefficient, and error-prone.
Employees must go through multiple approval layers (manager → HR clerk → system entry), leading to delays, confusion, and wasted HR time.

---

**Vision**

A Vacation Tracking System (VTS) will provide individual employees with the capability to manage their own vacation time, sick leave, and personal time off, without having to be an expert in company policy or the local facility’s leave policies.

---

**Functional Requirements**

■ Implements a flexible rules-based system for validating and verifying leave time requests

■ Enables manager approval (optional)

■ Provides access to requests for the previous calendar year, and allows requests to be made up to a year and a half in the future

■ Uses e-mail notification to request manager approval and notify employees of request status changes

■ Uses existing hardware and middleware

■ Is implemented as an extension to the existing intranet portal system, and uses the portal’s single-sign-on mechanisms for all authentication

■ Keeps activity logs for all transactions

■ Enables the HR and system administration personnel to override all actions restricted by rules, with logging of those overrides

■ Allows managers to directly award personal leave time (with system-set limits)

---

**Non-Functional Requirements**

■ Easy to use, intuitive, and intelligent.

■ Save time and money mostly in the HR department.

---

**Constraints**

■ Use existing hardware and middleware.

■ Must be implemented as part of the existing intranet portal.

■ Must use portal’s single sign-on mechanism.

■ Must interface correctly with HR legacy systems for employee data.

---

**Actors**

■ Employee

■ Manager

■ HR Clerk

■ System Admin

---

**Use Case: Manage Time**

This use case describes how an employee interacts with the Vacation Tracking System (VTS) to:

■ Submit a vacation time request

■ View the status of their vacation requests

**Data Model**

Employee

| Attribute              | Type                                   | Description                        |
| ---------------------- | -------------------------------------- | ---------------------------------- |
| `id`                   | PK                                     | Unique ID of the employee          |
| `first_name`           | String                                 | Employee’s first name              |
| `last_name`            | String                                 | Employee’s last name               |
| `email`                | String                                 | Corporate email (used for login)   |
| `position`             | String                                 | Job title or role                  |
| `manager_id`           | FK → Manager.manager_id                | Link to their manager              |
| `available_leave_days` | Integer                                | Current remaining vacation balance |
| `hire_date`            | Date                                   | When employee joined               |
| `status`               | Enum(`Active`, `On Leave`, `Resigned`) | Employment status                  |

Manager

| Attribute     | Type             | Description               |
| ------------- | ---------------- | ------------------------- |
| `id`          | PK               | Unique ID for the manager |
| `department`  | String           | Department managed        |
| `employee_id` | FK → Employee.id | Department managed        |

VacationRequest

| Attribute        | Type                                                 | Description                       |
| ---------------- | ---------------------------------------------------- | --------------------------------- |
| `request_id`     | PK                                                   | Unique ID of the vacation request |
| `employee_id`    | FK → Employee.id                                     | The requester                     |
| `manager_id`     | FK → Manager.id                                      | Approving manager                 |
| `start_date`     | Date                                                 | Start of vacation                 |
| `end_date`       | Date                                                 | End of vacation                   |
| `days_requested` | Integer                                              | Total leave days requested        |
| `request_date`   | DateTime                                             | When the request was submitted    |
| `status`         | Enum(`Pending`, `Approved`, `Rejected`, `Cancelled`) | Current request state             |
| `comments`       | Text                                                 | Optional manager feedback         |
| `approval_date`  | DateTime                                             | When manager acted on the request |

**Sequence Diagram**

![Sequence Diagram](./vts-sequence.png)

**Pseudocode**

```
BEGIN ManageTimeUseCase

  // Employee logs in through the WebUI
  Employee -> WebUI: Login(credentials)

  // WebUI requests authorization from AuthService
  WebUI -> AuthService: RequestAuthorization(credentials)

  IF AuthService returns "Authorization Success" THEN
      WebUI -> Employee: Display "Manage Time" interface

      // Step 1: Get Vacation Balance
      WebUI -> VTSController: GetVacationBalance(employeeID)
      VTSController -> ValidateVacation: GetVacationBalance(employeeID)
      Database -> ValidateVacation: ReturnVacationBalance(balance)
      VTSController -> WebUI: ReturnVacationBalance(balance)
      WebUI -> Employee: DisplayVacationBalance(balance)

      // Step 2: Submit Vacation Request
      Employee -> WebUI: SubmitVacationDates(startDate, endDate)
      WebUI -> VTSController: SubmitVacationRequest(employeeID, startDate, endDate)
      VTSController -> ValidateVacation: ValidateRequest(employeeID, startDate, endDate)

      IF request is valid THEN
          ValidateVacation -> Database: StoreRequest(employeeID, startDate, endDate)
      ENDIF

      ValidateVacation -> VTSController: ReturnValidationStatus(status)
      VTSController -> WebUI: ReturnValidationStatus(status)
      WebUI -> Employee: DisplayValidationStatus(status)

      // Step 3: Show Vacation Requests
      WebUI -> VTSController: GetVacationRequests(employeeID)
      VTSController -> ValidateVacation: GetVacationRequests(employeeID)
      Database -> ValidateVacation: ReturnVacationRequests(requests)
      VTSController -> WebUI: ReturnVacationRequests(requests)
      WebUI -> Employee: DisplayVacationRequests(requests)

  ELSE
      // Authorization failed
      AuthService -> WebUI: ReturnAuthorizationFailed()
      WebUI -> Employee: Display "Authorization Failed" message
  ENDIF

END ManageTimeUseCase
```
