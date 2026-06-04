
# Worker Offboarding Automation
### CNZ Automations - Royal New Zealand Coastguard

## The Problem

When a staff member left the organisation, someone on the HSW team had to manually locate them in Vault GRC and archive their profile. It was a small task but it relied on HSW being notified, remembering to do it, and knowing the correct process in Vault. It was the kind of thing that fell through the cracks.

## What It Does

When HR submits the offboarding form, the automation handles everything. The worker is located in Vault GRC, archived with the correct status, and a confirmation is logged and emailed to the HSW team. No manual steps required.

## How It Works

1. HR submits the Vault Offboarding Request form (Microsoft Forms) with the worker's full name, CNZ Employee Number, termination date, and reason
2. Power Automate triggers and sends the request to the Vercel proxy
3. The Vercel proxy searches Vault GRC for the worker by Employee Number using paginated search (500 records per page, exits as soon as a match is found)
4. Once the worker is located, the proxy archives them in Vault GRC with the correct status and termination date
5. A record is created in the Vault Offboarding Log in SharePoint
6. A confirmation email is sent to the HSW team

## Technical Challenges Solved

**Vault GRC uses HMAC SHA256 authentication**
Power Automate cannot generate HMAC SHA256 signatures natively. This was solved by routing all Vault API calls through a Vercel proxy (Node.js) that handles the authentication before passing the request to Vault.

**Vault requires an internal ID, not an Employee Number**
HR knows workers by their CNZ Employee Number. Vault GRC uses its own internal ID. The proxy searches Vault by Employee Number across paginated results to find the correct internal ID before archiving.

**Archiving requires two fields sent together**
Discovering that Vault's archive process requires both `empStatus: ARCHIVED` and `terminationCheck: true` to be sent in the same request was not documented. It was found by examining the two-step manual process in the Vault UI and replicating it via the API.

## Tech Stack

- **Microsoft Forms** - offboarding request capture
- **Microsoft Power Automate** - workflow orchestration
- **Vercel** - proxy layer handling HMAC SHA256 authentication
- **Vault GRC** - HSW management platform
- **SharePoint** - offboarding audit log
- **Email** - confirmation to HSW team

## Built By

Carlo Tuhakaraina identified the problem, designed the solution, and did all the testing. Claude (Anthropic) wrote the code.

---

*Part of the [CNZ Automations](../) portfolio.*
