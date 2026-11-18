# Campus Stores — Live Chain of Custody

A simple administrator-first prototype for equipment lending and shift handovers.

## Run

No build tools or dependencies are required. Download/clone this folder and run:

```bash
python3 -m http.server 8000
```

Open http://localhost:8000. You can also open `index.html` directly in a modern browser.

## What is included

- **Overview:** immediate shift-start summary of items still out, current holders, and recent changes.
- **Loans:** searchable loan register with original borrower, current holder, expected return, and item trace.
- **Inventory:** physical assets identified by unique asset ID.
- **Activity:** chronological audit trail; exportable as CSV.
- **User research:** interviewer-entry form, aggregated difficulty/frequency summaries, response review, and CSV export.
- **Information:** plain-language guide, terminology, and before/after administrator workflow.

## Demo workflow

The fictional starting data includes Film Society request RQ-214 and Debate Union request RQ-209. Open AD-12 and use **Transfer custody** to change the current holder and expected return. Its original borrower and previous events remain in the trace. Use **Record return** only after the item is physically back at the desk.

Use **Restart sample shift** to restore the initial scenario. Research responses are separate from the sample loan data. The optional research examples are fictional and labelled as illustrative; never present them as actual user research.

## Important limitations

This is a front-end prototype. Data is saved in the current browser's localStorage; it does not synchronize across staff devices. A production system needs authentication, a shared database/API, real-time synchronization, role-based permissions, server timestamps, and an immutable audit log. Custody transfer in this prototype is entered by the administrator and is not yet confirmed by the receiving person.

## Product decision

The chosen direction is **Live Chain of Custody**: replace free-text notes as the primary source of operational truth with structured events and a current-custodian field. Notes remain available as context attached to the relevant asset trace.
