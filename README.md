# Action Tracker — Enterprise Task Agent for Microsoft 365 Copilot

**Agents League Hackathon 2026 — Enterprise Agents track**
Built with Microsoft Copilot Studio · Grounded with **Work IQ**

---

## What it does

Action Tracker is a Microsoft 365 Copilot agent that turns work commitments into tracked
Microsoft Planner tasks — and keeps them moving. It solves a universal enterprise problem:
**decisions and commitments made in meetings and emails get forgotten and never followed through.**

The agent supports two ways to create a task, both feeding one shared, safety-gated loop:

1. **From real work context (Work IQ):** the agent reads the user's actual emails, meetings,
   and chats through Work IQ, surfaces commitments and follow-ups, and proposes them as tasks.
2. **From a direct request:** the user types a plain-language instruction
   (e.g. "Assign Manish to finish the Q3 report by Friday").

In both cases the agent: resolves the person against the directory, asks the owner to
**confirm before anything is created or sent**, creates the task in Planner with the correct
assignee and due date, and Planner notifies the assignee. The owner can also list tasks,
update them, delete them, and receive an automated **daily digest** of status and overdue items.

## Why this is an Enterprise Agent

- **Real business scenario:** task accountability across a team — applicable in any organization.
- **Microsoft 365 integration:** built natively on Microsoft 365 Copilot via Copilot Studio,
  using Microsoft Planner and Microsoft 365 user directory data.
- **Microsoft IQ layer (required):** uses **Work IQ** as the grounding layer — building context
  from the user's emails, meetings, chats, and documents — so the agent extracts *real*
  commitments instead of inventing them. This is load-bearing, not decorative: the meeting/email
  extraction feature cannot function without it.
- **Security & Responsible AI:** the agent **never writes, assigns, or sends anything without
  explicit owner confirmation**. It never guesses people, IDs, or task data — it only uses values
  returned by tools or the directory. Name ambiguity is resolved by asking, never guessing.

## Architecture

```
                  Work IQ                          Direct typed request
       (emails / meetings / chats              "Assign X to do Y by Z"
            -> commitments)
                   |                                      |
                   +------------------+-------------------+
                                      |
                                      v
                     Resolve person (directory)
                       - 1 match  -> continue
                       - many     -> ask which one
                       - none     -> ask for clarification
                                      |
                                      v
                     Owner confirms  (safety gate)
                                      |
                                      v
                     Create / Update / Delete in
                     Microsoft Planner + notify assignee
                                      |
                                      v
                     Daily digest (scheduled flow)
                     status - due dates - overdue
```

## Components & tools

- **Platform:** Microsoft Copilot Studio (generative orchestration enabled)
- **IQ layer:** Work IQ (emails, meetings, chats, people, relationships)
- **Task store:** Microsoft Planner (Create, List, Update, Delete tasks)
- **Identity:** Microsoft 365 directory + Office 365 Users (Get user profile) for assignee resolution
- **Automation:** Power Automate scheduled flow for the daily digest
- **Channel:** published to Microsoft 365 Copilot / Teams

## Key features

- Two-way task creation (Work IQ context + direct request)
- Robust person resolution (single / multiple / no match handled correctly)
- Confirm-before-write on every create, update, and delete
- Full task lifecycle: create, list/status, update (priority, due date, status), delete
- Assignee notification via Planner
- Automated daily task digest

## Repository contents

| Path | What it is |
|------|------------|
| `README.md` | This file — project overview |
| `instructions.md` | The agent's full instruction set (its reasoning logic) |
| `/screenshots` | Proof of the agent working (Planner tasks, disambiguation, Work IQ, digest) |
| `/exports` | Exported Copilot Studio agent solution + Power Automate flow (for reproducibility) |

## Demo

Demo video: **https://vimeo.com/1201315651?share=copy&fl=sv&fe=ci**

Example flow shown in the demo:
1. Agent surfaces a commitment from a real email via Work IQ and proposes a task.
2. Owner confirms → task is created in Planner and the assignee is notified.
3. Owner types a new task directly; the agent resolves the person and confirms.
4. Owner asks for status → agent lists tasks and highlights overdue ones.

## How to run / reproduce

1. In Microsoft Copilot Studio, import the agent solution from `/exports` (or create a new agent
   and enable **generative orchestration**).
2. Enable **Work IQ** in the agent's Tools.
3. Add the Planner actions (Create, List, Update, Delete a task) and Office 365 Users (Get user profile).
4. Apply the agent instructions from `instructions.md`.
5. Publish to Microsoft 365 Copilot / Teams.
6. (Optional) Import the Power Automate daily-digest flow from `/exports`.

## Responsible AI & security

- No action that creates, assigns, sends, or deletes is taken without explicit user confirmation.
- The agent does not fabricate people, emails, IDs, or task data — only tool/directory values are used.
- No secrets, credentials, API keys, or connection strings are stored in this repository.
- **All data used in this project is synthetic / demonstration data. No real customer data or PII is used.**

---

*Built for the Agents League Hackathon 2026, Enterprise Agents track. Uses synthetic data only.*
