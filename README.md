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

**Sequence Diagram**

![Sequence Diagram](./vts-sequence.png)
