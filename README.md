CNZ Automations
Royal New Zealand Coastguard — HSW Team
This repository documents a suite of automations built to reduce manual administrative burden on the HSW team at Royal New Zealand Coastguard. The organisation runs 62 units across New Zealand with a mix of paid staff and volunteers. The HSW team is three people covering the whole country. These automations exist because hiring more people wasn't an option, so we built systems to handle the work that didn't need a human.
All three automations run inside a Microsoft 365 environment and connect to Vault GRC, an external HSW management platform, via authenticated API calls handled through a Vercel proxy layer.

Automations
AutomationStatusWhat It DoesVault Daily Triage✅ LivePulls overnight HSW incidents from Vault GRC, runs AI analysis, and delivers a risk-ranked summary to the HSW team each morning via Microsoft TeamsWorker Offboarding✅ LiveWhen HR submits a form, automatically locates the worker in Vault GRC by Employee Number and archives them, no manual steps requiredWorker Onboarding✅ LiveWhen HR submits a form, automatically creates a new worker profile in Vault GRC with the correct employee type and shift classification

Tech Stack

Microsoft Power Automate — workflow orchestration
Microsoft Forms — trigger data capture from HR
Vercel — proxy layer that handles HMAC SHA256 API authentication
Vault GRC — the external HSW management platform these automations connect to
Claude API (Anthropic) — AI analysis on daily incident data
SharePoint — audit log for every action taken
Microsoft Teams — notification delivery to the HSW team


How It Was Built
These automations were built as a human-AI collaboration. Carlo identified the operational problems, designed the solutions, made every technical decision, and did all the testing. Claude (Anthropic) wrote the code.
The honest version of that is documented in the individual README files for each automation, including the problems that were hit along the way and how they were solved.

Part of the broader CNZ HSW digital transformation. See carlo-tuhakaraina for more context.
