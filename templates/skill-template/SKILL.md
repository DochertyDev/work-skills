---
name: skill-template
description: "Replace with a concise capability and the specific user requests that should activate this skill."
metadata:
  category: software-development
  source: original
---

# Skill title

Authoring template only: copy this file into `.agents/skills/your-skill-name/SKILL.md`, match `name` to the new directory, and replace every instruction to the author before publishing. Select a catalog category and record actual provenance; `original` applies only to work authored here.

## Purpose and scope

Describe the outcome, intended inputs, and boundaries. Make the frontmatter description specific enough to select this skill without reading its body.

## Prerequisites

List required inputs, tools, access, and dependencies, or remove this section if none apply. Explain what to do when a prerequisite is missing.

## Workflow

1. Establish the relevant context and inputs.
2. Describe the concrete actions and decision points needed for the task.
3. Validate the result and handle incomplete or failed work.

Replace these steps with the actual procedure; avoid generic instructions that do not improve decisions.

## Expected outputs

Specify the deliverable, its location or format, and the concise completion information the user needs.

## Quality checks

Define observable success criteria and practical checks. Include one realistic activating request and one nearby request that should not activate the skill during author review.

## Constraints and safety

Document relevant privacy, licensing, and authorization boundaries. Preserve existing work and user scope; the skill itself does not grant permission for external or destructive actions.

## Optional supporting files

Remove this section if unused. Add `references/` for task-specific detail, `scripts/` for tested helpers, or `assets/` for output resources only when needed. Link each file relative to this directory and explain when to read or use it. Do not create empty directories or link nonexistent files.
