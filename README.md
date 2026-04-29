# 🚕 Urban Routes QA Case Study  
## Manual Testing, Bug Management & Python Selenium Automation

## 📌 Overview
Urban Routes is a route-building web application that calculates travel time and cost across different transportation options.

This case study demonstrates my QA process across three phases:

1. **Strategic Analysis & Test Design**
2. **Manual Testing & Bug Management**
3. **Automation Framework with Python & Selenium**

![Urban Routes app](images/UrbanRoutes.jpeg)

---

# Phase 1: Strategic Analysis & Test Design

## 🧠 Requirements Visualization

Before testing, I analyzed the Urban Routes application flow using a mind map. This helped me understand the relationship between route inputs, transportation modes, booking options, and expected system behavior.

![Mind Map](/images/mindMap.jpeg)

*Figure: Mind map used to visualize app flow, logic, and test coverage.*

## 🧪 Test Design Techniques

I applied structured QA techniques to design stronger test coverage:

- **Equivalence Class Partitioning (ECP)**
- **Boundary Value Analysis (BVA)**
- **Positive and negative testing**

Focus areas included:

- “From” and “To” address fields
- Travel modes
- Carsharing booking flow
- Payment method validation
- Driver’s license form behavior

---

# Phase 2: Manual Testing & Bug Management

## 🔍 Manual Testing Scope

Manual testing focused on validating the user experience and core business logic of the Urban Routes app.

Test coverage included:

- Route calculation
- Travel time and cost accuracy
- Carsharing and Aerotaxi features
- Booking flow
- Payment method
- UI layout against Figma design
- Cross-browser behavior in Chrome and Firefox

## 🎨 UI Validation

I compared the application layout against Figma designs to verify spacing, alignment, text accuracy, and overall consistency.

![Figma Design](/images/figma_carsharing.jpeg)

*Figure: Figma reference used for UI validation.*

---

## 🐞 Defect Analysis

During testing, I identified and documented **42 bugs** in Jira.

Each bug report included:

- Clear title
- Steps to reproduce
- Expected result
- Actual result
- Severity
- Supporting screenshots or evidence

---
Here’s your **priority distribution report** based on the WDP6 bugs you shared:

---

## 📊 Bug Priority Distribution

| Priority   | Count | Description                                                                 |
| ---------- | ----: | --------------------------------------------------------------------------- |
| 🔴 Highest |     8 | Critical failures that crash the app or completely block core functionality |
| 🟠 High    |     9 | Major issues affecting pricing accuracy or key user decisions               |
| 🟡 Medium  |     16 | Functional inconsistencies, missing details, or unclear user flows          |
| 🟢 Low     |     6 | Minor UI or readability issues                                              |
| ⚪ Lowest   |     3 | Cosmetic or non-impactful issues                                            |


**Total Bugs Identified: 42**

---

## 🎯 Top 5 Critical Bugs Identified

#### 1. Application Freezes After Cancel Confirmation (WDP3-31)

When the user clicks “Yes” to confirm canceling an order, the application becomes unresponsive.

**Impact:** Completely breaks the app experience and prevents any further user action.
**Priority:** Highest

---

### 2. Cancel Button Is Unresponsive (WDP3-30)

Clicking the “Cancel” button on the “Order sent” window does not trigger any action.

**Impact:** Users cannot cancel their order, removing critical control over the booking process.
**Priority:** Highest

---

### 3. Cannot Cancel Order During Free Waiting Time (WDP3-3)

Users are unable to cancel an order during the free waiting time period.

**Impact:** Blocks a key user action and can lead to frustration or unintended charges.
**Priority:** Highest

---

### 4. Application Freezes When Free Waiting Time Ends (WDP3-4)

When the free waiting time expires, the application stops responding.

**Impact:** Causes system instability and disrupts the entire user experience.
**Priority:** Highest

---

### 5. Car Icons on Map Are Not Selectable (WDP3-21)

Users cannot select car icons displayed on the map.

**Impact:** Prevents users from choosing a car, blocking a core step in the booking flow.
**Priority:** Highest

---

## 🧾 Example Bug Report (Jira)

**Title:** During the free waiting time period, click the cancel “X” icon on “order sent” window doesn’t cancel the order.

**Description**
The order can not be canceled during the free waiting time period by click on the cancel "X" icon on "Order sent" window.
**Preconditions**
1. Launch the Urban Routes application
2. Enter “1917 Bay St.“ in the “From” field
3. Enter “615 S Broadway“ in the “To“ field
4. Select “Custom”
5. Select “Drive” car icon
6. Click “Book”
   
**Steps to Reproduce:**

1. Add driver license
2. Add a bank card
3. Click on "Book" button
4. Click on "X" cancel button

**Expected Result:**
The order is canceled. 

**Actual Result:**
The order is not canceled. the cancel button isn’t responding.

**Environment:**

* Browser:
  - Chrome 118.0.5993.88 (Official Build) (x86_64) Screen solution 800x600
  - FireFox 119.0 (64-bit) Screen solution 1920x1080
    
* OS: macOS Ventura 13.5.2


**Severity:** 🔴 High

**Priority:** High

---

👉 Full bug reports available upon request (tracked in Jira)

---

# Phase 3: Automation Framework  
## Python & Selenium

## 🤖 Automation Goal

After completing manual testing, I built a Python Selenium automation framework to validate important user flows and prepare the project for scalable regression testing.

The framework focuses on:

- Maintainability
- Reusability
- Clear test structure
- Separation of test data, helper logic, page elements, and test cases

---

## 🧱 Framework Architecture

```text
Urban-Routes-Automation/
│
├── data.py       # Test constants and input data
├── helpers.py    # Utility functions and server health check
├── pages.py      # Page Object Model: selectors and page actions
└── main.py       # Pytest test suite
````

---

## 📦 `data.py` — Centralized Test Data

The `data.py` file stores reusable constants such as URLs, addresses, and phone numbers.

```python
URBAN_ROUTES_URL = ""
ADDRESS_FROM = "East 2nd Street, 601"
ADDRESS_TO = "1300 1st St"
PHONE_NUMBER = "+1234567890"
```

This keeps test data separate from test logic and makes updates easier.

---

## 🔌 `helpers.py` — Server Health Check

The framework includes a server health check to verify that the testing environment is available before running tests.

```python
@classmethod
def setup_class(cls):
    if helpers.is_url_reachable(data.URBAN_ROUTES_URL):
        print("Connected to the Urban Routes server")
    else:
        print("Cannot connect to Urban Routes. Check the server")
        sys.exit()
```

This helps prevent false failures caused by an unavailable server.

---

## 🧩 `pages.py` — Page Object Model

The `pages.py` file contains element selectors and interaction methods.

Example page methods include:

```python
routes_page.set_route(data.ADDRESS_FROM, data.ADDRESS_TO)
routes_page.fill_phone_number(data.PHONE_NUMBER)
routes_page.verified_phone_number()
```

Using the Page Object Model helps keep the test suite clean, readable, and easier to maintain.

---

## 🧪 `main.py` — Pytest Test Suite

The test suite is structured using Pytest classes and descriptive test methods.

### Example Automated Test

```python
def test_fill_phone_number(self):
    """Verify that the entered phone number is correctly saved and displayed."""
    self.driver.get(data.URBAN_ROUTES_URL)
    routes_page = UrbanRoutesPage(self.driver)
    routes_page.set_route(data.ADDRESS_FROM, data.ADDRESS_TO)
    routes_page.fill_phone_number(data.PHONE_NUMBER)

    entered_phone = routes_page.verified_phone_number()
    assert entered_phone == data.PHONE_NUMBER, f"Expected phone: {data.PHONE_NUMBER}, but got: {entered_phone}"
```

## ✅ What This Test Demonstrates

* Selenium-based browser interaction
* Page Object Model structure
* Data-driven test design
* Clear assertion and validation
* Real user flow automation

---

## 🧪 Automated Test Scenarios

The framework was designed to cover key Urban Routes flows:

* Set route
* Select plan
* Fill phone number
* Add payment method
* Add driver message
* Add extras
* Order ice cream
* Verify car search modal

---
## 🧼 Code Quality Standards

* snake_case naming
* UPPERCASE constants
* Modular structure
* Reusable components
* Clean, readable tests

---

## 🧰 Tools & Technologies

* Python
* Selenium WebDriver
* Pytest
* Jira
* Figma
* Charles Proxy
* Chrome
* Firefox
* Chrome DevTools

---

## 📊 Results & Impact

* Identified and documented **42 bugs**
* Improved coverage for critical booking and route-building flows
* Validated UI behavior against Figma design
* Built an automation-ready Selenium framework
* Demonstrated both manual QA and automation engineering skills

---

## 💡 Key Takeaways

This project helped me strengthen my QA skills by combining strategic test design, hands-on defect reporting, and automation framework development.

I learned how to:

* Break down requirements into testable flows
* Use ECP and BVA to improve coverage
* Report bugs clearly and professionally
* Build maintainable automation using Python, Selenium, Pytest, and POM

---

## 👩‍💻 Author

**Warunee Dinunzio**

QA Automation Engineer

