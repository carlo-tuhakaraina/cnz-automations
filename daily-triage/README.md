# Vault Daily Triage Automation
### CNZ Automations - Royal New Zealand Coastguard

## The Problem

The HSW team needed to review overnight incidents logged in Vault GRC every morning. This meant logging into Vault, finding new incidents, reading through them, and deciding which ones needed immediate attention. Doing this manually every day across a three-person team covering all of New Zealand was time consuming and inconsistent.

## What It Does

Every morning the automation pulls all new incidents from Vault GRC since the last run, runs AI analysis on each one, and delivers a risk-ranked summary to the HSW team via Microsoft Teams. The team starts the day with a clear picture of what came in overnight and what needs attention, without logging into Vault.

## How It Works

1. Power Automate triggers on a recurrence schedule each morning
2. The flow sends a request to the Vercel proxy with the last processed record ID as a watermark
3. The Vercel proxy pulls new incidents from Vault GRC using the Vault API
4. The incidents are passed to the Claude API for analysis using the CNZ Trident Triage framework and the CNZ 5x5 risk matrix
5. Claude returns a structured risk-ranked summary for each incident
6. The watermark is updated in SharePoint so the next run only pulls new incidents
7. The summary is delivered to the HSW team via Microsoft Teams

## Technical Challenges Solved

**Vault GRC uses HMAC SHA256 authentication**
Power Automate cannot generate HMAC SHA256 signatures natively. This was solved by routing all Vault API calls through a Vercel proxy (Node.js) that handles the authentication before passing the request to Vault.

**Vault returns incidents in oldest-first order**
To ensure only new incidents were processed each run, the API limit was increased to 1000 records and results were filtered by record ID against a stored watermark in SharePoint.

**Storing the watermark between runs**
SharePoint was used to store the last processed record ID. Each run reads the watermark, processes only newer records, and updates the watermark before finishing.

## AI Triage Approach

The Claude API analyses each incident using two CNZ frameworks. The Trident Triage framework (Prong 1 regulatory lens) guides the risk assessment and the CNZ 5x5 risk matrix determines the risk level. The output covers risk level, notifiability under the Health and Safety at Work Act 2015 and the Maritime Transport Act 1994, safety learning, welfare check flags, and systemic flags.

The AI output is a decision support tool. All final decisions remain with the HSW team.(human in the loop)

## Tech Stack

- **Microsoft Power Automate** - workflow orchestration and scheduling
- **Vercel** - proxy layer handling HMAC SHA256 authentication
- **Vault GRC** - HSW management platform
- **Claude API (Anthropic)** - AI incident analysis
- **SharePoint** - watermark storage between runs
- **Microsoft Teams** - daily summary delivery to HSW team

## Built By

Carlo Tuhakaraina identified the problem, designed the solution, and did all the testing. Claude (Anthropic) wrote the code.

---

*Part of the [CNZ Automations](../) portfolio.*
