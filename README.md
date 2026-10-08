# DevOps Incident Troubleshooter

An AI-agent skill for diagnosing common Linux and Docker incidents using an evidence-first, low-risk troubleshooting workflow.

## Problem

DevOps incidents are often approached through trial and error.

A user may know that a Docker container is restarting, a Linux service has failed, a port is unavailable, or an application cannot connect to another service, but not know:

- What evidence to collect first
- Which possible causes are most likely
- Which diagnostic commands are safe
- When a proposed fix could be destructive
- How to verify that the problem is actually resolved

## Solution

**DevOps Incident Troubleshooter** teaches an AI agent to approach incidents systematically.

The skill follows this workflow:

**Understand → Gather Evidence → Rank Hypotheses → Diagnose → Remediate → Verify → Prevent**

It prioritizes evidence before action and separates diagnosis from remediation.

## Supported Incidents

The skill focuses on common Linux and Docker operational problems:

- Docker containers crashing or restarting
- Docker port conflicts
- Linux services that will not start
- Permission denied errors
- Disk space or inode exhaustion
- Application connectivity failures
- Configuration and environment problems

## How It Works

### 1. Establish the incident

Identify what is failing, what changed, and what the user expected to happen.

### 2. Gather evidence

Start with the smallest useful diagnostic step instead of immediately changing the system.

Examples include:

- Container status and logs
- Docker inspection data
- Listening ports
- Linux service status
- Service logs
- File and directory permissions
- Disk and inode usage
- Docker network information

### 3. Form and rank hypotheses

Use the collected evidence to identify possible root causes and prioritize the most likely or highest-value hypothesis.

### 4. Test the hypothesis

Run targeted diagnostics to confirm or eliminate the suspected cause.

### 5. Recommend remediation

Separate safe diagnostic actions from remediation.

The skill distinguishes between:

- **Read-only diagnostics**
- **Low-risk remediation**
- **Potentially destructive actions**

Potentially destructive actions should not be recommended casually.

### 6. Verify the result

A fix is not considered complete just because a command succeeded.

The skill requires checking that the original problem is actually resolved.

### 7. Prevent recurrence

Where appropriate, identify configuration, monitoring, resource, or operational improvements that can reduce the chance of the incident happening again.

## Response Format

The skill produces a structured troubleshooting response containing:

- **Diagnosis**
- **Evidence**
- **Root Cause**
- **Fix**
- **Verification**
- **Prevention**
- **Confidence**

## Example Trigger

A user might say:

> My Docker container keeps restarting. I don't know why.

The skill should guide the agent toward collecting evidence such as container status and logs before recommending changes.

Another example:

> My Linux service won't start after a configuration change.

The skill should investigate service status and relevant logs, identify the likely cause, recommend an appropriate fix, and verify the service afterward.

## Design Principles

### Evidence before action

Do not guess when useful evidence can be collected first.

### Smallest useful diagnostic step

Avoid running a long list of unrelated commands. Start with the diagnostic that provides the most useful information.

### Safe remediation

Do not treat destructive commands as ordinary troubleshooting steps.

### Verification matters

A proposed fix is incomplete until its effect has been checked.

### Explain the reasoning

The skill is designed to make troubleshooting understandable rather than simply producing a command to copy and paste.

## Installation

Install the skill through LLM SkillHub:

```bash
llmsh install ykgdg2026/devops-incident-troubleshooter