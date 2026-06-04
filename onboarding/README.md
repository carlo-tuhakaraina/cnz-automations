# Worker Onboarding Automation
### CNZ Automations - Royal New Zealand Coastguard

## The Problem

When a new staff member or volunteer joined the organisation, someone on the HSW team had to manually create their profile in Vault GRC. It was a repetitive admin task that depended on HSW being notified at the right time and entering the correct details. This was a repetative administrative task that would often be overlooked due to competing projects and team bandwidth.

## What It Does

When HR submits the onboarding form, the automation creates the worker profile in Vault GRC automatically with the correct employee type and shift classification. A confirmation is logged and emailed to the HSW team and the HR submitter.

## How It Works

1. HR submits the Vault Onboarding Form (Microsoft Forms) with the worker's CNZ Employee Number, first name, last name, employee type, CNZ department or unit, and hire date
2. Power Automate triggers and sends the request to the Vercel proxy
3. The Vercel proxy creates a new worker profile in Vault GRC with the correct details
4. A record is created in the Vault Onboarding Log in SharePoint
5. A confirmation email is sent to the HSW team and the HR submitter

## Technical Challenges Solved

**Power Automate was truncating the JSON request body**
When building the request body using a Power Automate expression, the output was being cut off. This was solved by switching to a native JSON body with dynamic tokens instead of an fx expression.

**Vault GRC does not accept "Permanent" or "Volunteer" as an employee type**
The assumption was that employee type would map directly to CNZ terminology. Through API debugging it was discovered that all workers are created with employeeType set to "Employee" and the Permanent or Volunteer distinction is stored separately in a shift field, with capital letter values of "Permanent" and "Volunteer".

**Finding the correct shift field values**
The correct values were confirmed by querying existing known workers via the API and checking what Vault had stored against their profiles.

## Tech Stack

- **Microsoft Forms** - onboarding request capture
- **Microsoft Power Automate** - workflow orchestration
- **Vercel** - proxy layer handling HMAC SHA256 authentication
- **Vault GRC** - HSW management platform
- **SharePoint** - onboarding audit log
- **Email** - confirmation to HSW team and HR submitter

## Relationship to the Offboarding Automation

This automation is the reverse of the [Worker Offboarding Automation](../offboarding/). Together they form a complete worker lifecycle system inside Vault GRC, triggered entirely by HR form submissions with no manual HSW involvement required.

## Built By

Carlo Tuhakaraina identified the problem, designed the solution, and did all the testing. Claude (Anthropic) wrote the code.

---

*Part of the [CNZ Automations](../) portfolio.*
