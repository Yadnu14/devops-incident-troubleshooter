---
name: devops-incident-troubleshooter
description: >
  Diagnose Linux and Docker incidents by gathering evidence, identifying likely
  root causes, recommending safe fixes, and verifying the result. Use this skill
  when the user reports "Docker container keeps restarting", "port already in
  use", "Linux service won't start", "permission denied", "disk is full", or
  "application cannot connect", including when the user describes the problem
  without naming it as troubleshooting.
license: MIT
metadata:
  llmskillhub:
    version: 0.1.0
    categories: [devops]
    keywords: [docker, linux, troubleshooting, devops, incident-response]
    capabilities:
      network: false
      filesystem: read
      shell: false
      secrets: []
---

# DevOps Incident Troubleshooter

Use this skill to diagnose common Linux and Docker incidents through an
evidence-first, low-risk troubleshooting workflow.

The goal is not to guess the fix. The goal is to collect the smallest useful
set of evidence, rank plausible causes, make the least risky appropriate
change, and verify the result.

## When to use this skill

Use this skill when a user reports or asks for help with:

- Docker containers repeatedly restarting, crashing, or exiting
- Docker port binding conflicts or "address already in use" errors
- Linux services that fail to start or stop unexpectedly
- Permission denied errors
- Disk space or inode exhaustion
- Applications that cannot connect to another service
- Configuration or environment-variable problems
- Basic Linux or Docker runtime failures where command-line evidence can
  identify the cause

Do not use this skill as a general-purpose DevOps assistant. If the problem
requires Kubernetes, Terraform, cloud-provider infrastructure, CI/CD systems,
advanced security investigation, or application-specific debugging beyond the
available evidence, state that the skill's scope does not cover the problem
and provide only relevant general guidance.

## Core troubleshooting principles

### 1. Evidence before action

Never begin by prescribing a fix merely because an error message resembles a
familiar problem.

First identify:

- What is failing?
- When did it start?
- What changed?
- What is the expected behavior?
- What evidence is already available?
- What environment is involved?

Prefer direct evidence from the system over assumptions.

### 2. Use the smallest useful diagnostic step

Ask for or run the minimum diagnostic command that can distinguish between
plausible causes.

Prefer read-only diagnostics such as:

- `docker ps`
- `docker ps -a`
- `docker logs`
- `docker inspect`
- `docker port`
- `docker images`
- `docker network inspect`
- `systemctl status`
- `journalctl`
- `ss -ltnp`
- `df -h`
- `df -i`
- `free -h`
- `ps`
- `id`
- `ls -l`

Do not run commands simply because they are commonly used. Explain what
evidence each command is expected to provide.

### 3. Separate diagnosis from remediation

Do not mix evidence gathering with corrective actions.

Use this order:

1. Understand the incident.
2. Gather evidence.
3. Generate plausible hypotheses.
4. Rank hypotheses using the evidence.
5. Select the least risky appropriate remediation.
6. Apply the remediation only when justified.
7. Verify the expected behavior.
8. Recommend prevention or monitoring improvements.

### 4. Classify actions by risk

Treat actions as three categories:

**Read-only diagnostics**

Commands that inspect state without intentionally modifying it.

Examples:
`docker ps`, `docker logs`, `docker inspect`, `systemctl status`,
`journalctl`, `ss`, `df`, `free`, `ps`, `id`, and `ls`.

**Low-risk remediation**

Changes that are targeted and normally reversible.

Examples may include restarting a failed service after identifying the cause,
correcting a known configuration value, or changing a clearly incorrect file
permission when ownership and intended access are known.

**Potentially destructive actions**

Actions such as:

- deleting containers or images
- removing Docker volumes
- deleting files
- recursively changing ownership or permissions
- flushing firewall rules
- killing processes without understanding their role
- removing packages
- deleting logs or system data

Do not recommend destructive actions as the first response. Explain the risk,
identify what could be lost or disrupted, and obtain appropriate confirmation
when the action is consequential.

## Troubleshooting workflow

### Step 1: Establish the incident

Summarize the failure in one sentence.

Identify:

- affected service, container, host, or application
- observed symptom
- expected behavior
- approximate time of failure
- recent changes
- relevant error messages

If critical information is missing, ask only for the information needed to
choose the next diagnostic step.

### Step 2: Gather evidence

Start with the narrowest relevant diagnostic command.

For Docker incidents, inspect container state, exit status, logs, configuration,
ports, and relevant networks.

For Linux service incidents, inspect service status and recent journal entries.

For resource incidents, inspect disk space, inodes, memory, and running
processes.

Record important observations rather than dumping large amounts of unrelated
output.

### Step 3: Form and rank hypotheses

Create a short list of plausible causes.

For each hypothesis, state:

- supporting evidence
- contradicting evidence
- the next diagnostic that would confirm or reject it

Do not present speculation as a confirmed root cause.

### Step 4: Test the highest-value hypothesis

Choose the diagnostic that provides the most useful distinction between the
remaining hypotheses while causing the least disruption.

Prefer one diagnostic step at a time.

After each result, update the diagnosis instead of continuing with a
prewritten command sequence.

### Step 5: Recommend remediation

Only recommend a fix when there is enough evidence to justify it.

For each remediation, explain:

- what it changes
- why it addresses the suspected cause
- whether it is reversible
- any important side effects

If the evidence is insufficient, say so and continue diagnosis rather than
inventing certainty.

### Step 6: Verify

Verification is mandatory.

Define a concrete success condition before or while applying the fix.

Examples:

- container remains running
- expected port is listening
- service reports `active (running)`
- application can establish the required connection
- disk usage returns below the relevant threshold
- the original error no longer appears in logs

If verification fails, return to evidence gathering and reassess the
hypotheses.

### Step 7: Prevent recurrence

After the immediate incident is resolved, provide one or more practical
prevention measures appropriate to the confirmed cause.

Examples include:

- health checks
- resource limits
- log rotation
- monitoring and alerts
- configuration validation
- startup dependency handling
- deployment checks
- documented recovery procedures

Do not recommend unrelated infrastructure changes.

## Incident-specific guidance

### Docker container restarting or crashing

First determine:

- container state and exit code
- recent container logs
- configured command or entrypoint
- relevant environment/configuration
- mounted files or volumes
- resource or dependency failures

Useful diagnostics:

```bash
docker ps -a
docker logs --tail 100 <container>
docker inspect <container>