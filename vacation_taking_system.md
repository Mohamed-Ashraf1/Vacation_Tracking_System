# Vacation Taking System (VTS)
**Project Specification & Requirements Document**

---

## 1. Vision

Provide individual employees with the capability to seamlessly manage their own vacation time, sick leave, and personal time off (PTO)—without needing to be an expert in complex company policies or local facility leave regulations.

---

## 2. Problem Statement

### Background & Context
In modern organizational environments, employee autonomy and flexible working models have increased significantly. Workers frequently split their time across multiple concurrent projects and report to multiple project leads. As a result:
* **Diminished Informal Communication:** Managers have fewer informal, face-to-face interactions with team members, making informal leave tracking ineffective.
* **Administrative Inefficiencies:** Manual, disconnected processes cause delays in request processing, lack clear audit trails, and increase overhead for HR personnel.

---

## 3. System Actors

| Actor | Type | Description |
| :--- | :--- | :--- |
| **Employee** | Primary | Submits, tracks, and manages personal vacation, sick leave, and PTO requests; reviews current and previous year balances. |
| **Manager** | Primary | Reviews and approves/rejects team leave requests; awards personal leave time within prescribed boundaries. |
| **HR Clerk :** | Administrative | Oversees leave compliance, reviews employee personal and leave records, and executes policy overrides when justified. |
| **System Administrator** | Administrative | Manages system configurations, oversees rule engine settings, performs emergency rule overrides, and monitors integrations. |

---

## 4. Functional Requirements

| ID | Functional Requirement |
| :--- | :--- |
| **FR-01** | Employees can manage their vacations and leaves |
| **FR-02** | Validating and verifying leave time requests |
| **FR-03** | Provides access to requests for the previous calendar year |
| **FR-04** | Uses e-mail notification to request manager approval and notify employees of request status changes |
| **FR-05** | Enables the HR and system administration personnel to override all actions restricted by rules, with logging of those overrides |
| **FR-06** | Allows managers to directly award personal leave time |
| **FR-07** | Keeps activity logs for all transactions |
| **FR-08** | Provides a Web service interface for other internal systems to query any given employee’s vacation request summary |
| **FR-09** | Interfaces with the HR department legacy systems to retrieve required employee information and changes |
| **FR-10** | HR department has sufficient rights to view employees’ personal data |

---

## 5. Non-Functional Requirements

* **NFR-1:** The user interface must be clean, responsive, and intuitive, enabling employees to request time off with minimal clicks and zero prior policy training.

---

## 6. Constraints

* **C-1 (Managerial Award Boundary):** The capability for managers to directly award personal leave time must be strictly bounded by defined policy limits (e.g., maximum cap on days per employee/period).
* **C-2 (Portal Integration & SSO):** The application must be implemented as an extension of the existing corporate intranet portal and must leverage the portal’s Single Sign-On (SSO) mechanism for all authentication.
* **C-3 (Infrastructure & Middleware):** The system must utilize existing corporate hardware and middleware infrastructure, avoiding new dedicated server or platform dependencies.

---

## 7. Workflow Pseudocode

### 7.1 Employee Workflow

```text
Employee:

    Employee selects VTS link from intranet portal

    Request employee credentials
    GetEmployeeVacationData(employeeId)
    Display employee vacation history

    IF Employee selects "Create New Request" THEN

        GetVacationCategoriesWithPositiveBalance(employeeId)

        Display availableCategories
        Employee selects a vacation category
        Display visual calendar
        Enter vacation dates

        IF Employee selects "Submit" THEN

            validationErrors = ValidateRequest(requestDTO)

            IF validationErrors is not empty THEN
                Display validationErrors
                Allow employee to modify or cancel the request
            END IF

        END IF

        vacationRequest = CreateVacationRequest(requestDTO)

        IF RequestRequiresManagerApproval(vacationRequest) THEN

            Set vacationRequest status to "Pending"
            Save vacationRequest
            Send approval email to manager

        ELSE
            Set vacationRequest status to "Approved"
            Save vacationRequest
        END IF

        RETURN employee to VTS home page

    END IF
```

### 7.2 Manager Workflow

```text
Manager:

    Request manager credentials
    GetManagerVacationData(managerId)
    Display manager's vacation history
    GetPendingRequestsForSubordinates(managerId)
    Display pendingRequests in a separate section

    selectedRequest = Manager selects one pending request

    Display selectedRequest details
    Prompt manager to approve or reject the request

    decision = Manager selects "Approve" or "Reject"

    IF decision = "Reject" THEN

        Prompt manager to enter rejection explanation

        WHILE rejection explanation is empty DO
            Display error message: "Rejection explanation cannot be empty."
            Prompt manager to enter rejection explanation
        END WHILE
        Set selectedRequest status to "Rejected"
        Save selectedRequest

    ELSE
        Set selectedRequest status to "Approved"
        Save selectedRequest
    END IF

    Manager submits decision
```

---

## 8. System Diagrams

### 8.1 Sequence Diagram
*Source File: [sequence Diagram.drawio](<Diagrams/Manage Time/sequence Diagram.drawio>)*

![Sequence Diagram](Diagrams/Manage%20Time/sequence%20Diagram.png)

### 8.2 Process Flowchart
*Source File: [FlowChart.drawio](<Diagrams/Manage Time/FlowChart.drawio>)*

![Process Flowchart](Diagrams/Manage%20Time/FlowChart.png)

### 8.3 Entity Relationship Diagram (ERD)
*Source File: [ERD.drawio](<Diagrams/Manage Time/ERD.drawio>)*

![Entity Relationship Diagram](Diagrams/Manage%20Time/ERD.png)
