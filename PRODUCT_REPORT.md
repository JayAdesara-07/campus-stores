# Campus Stores — Product Understanding & Proposed Improvement

## Executive summary

Campus Stores supports a campus equipment desk that issues physical equipment to student groups and records returns. The assignment scenario exposes a shift-handover problem: a kit can be partially returned, and an individual item can move from the original borrower to another person before it reaches the desk. If this change is communicated only through free-text notes, the incoming administrator must reconstruct the current situation manually.

We propose **Live Chain of Custody**: a simple dashboard of outstanding physical items, a clearly identified current custodian, and a chronological trace of checkout, transfers, notes, and return. The original borrower remains visible for accountability. Notes remain supported but become supplementary context rather than the primary operational record.

## 1. Understanding the administrator's job

The administrator is not primarily trying to manage a database. At the beginning of a shift, they need to answer a small set of operational questions quickly and confidently:

1. Which physical items are still outside the equipment room?
2. What is the exact asset ID of each item?
3. Who has each item now—not merely who first checked it out?
4. When is it expected back?
5. What happened since it left the desk?
6. Which items have actually been returned and can be made available again?

The desired outcome is a reliable shift handover with minimal reconstruction, fewer interruptions to the previous shift, and less risk of losing track of equipment.

## 2. Scenario and evidence from the supplied workflow

The candidate scenario follows Film Society request RQ-214:

- At Friday 3:45 PM, Kabir Sen collects a projector, speaker, and HDMI adapter.
- At Friday 7:30 PM, the projector and speaker are returned, but the adapter remains outside the desk.
- The adapter is now with Leela Das, who expects to return it Saturday at 9:00 AM.
- The next shift must identify the exact adapter and close the correct outstanding loan when it arrives.

This scenario demonstrates why tracking only a request or equipment category is insufficient. Each physical unit needs its own identity and lifecycle. It also demonstrates why the original borrower and current custodian are different concepts.

## 3. What works well and should be retained

- Individual physical assets have unique IDs, so two similar adapters are not treated as the same object.
- Inventory availability and repair status help prevent an unavailable unit from being issued.
- Separate loan records make partial returns possible.
- A chronological record is valuable for accountability.

The proposed change preserves these strengths rather than replacing the whole product.

## 4. Problems identified

We identified several related opportunities:

1. **Fragmented handover information:** important facts can be buried in multiple free-text notes.
2. **Stale borrower information:** the loan may continue to show the original borrower even after custody changes.
3. **Unclear current state:** an incoming administrator has to compare notes and loan records to find who currently has an item.
4. **Due-time changes:** the expected return can change after issue and should be visible on the active loan.
5. **Finding the exact unit:** similar equipment must be distinguished by physical asset ID.
6. **Cross-shift accountability:** the system should preserve who held an item and when each transfer occurred.

## 5. Why we focused on one direction

We chose **Live Chain of Custody** as the first improvement because it addresses the central shift-handover outcome and connects several of the issues above without requiring a complete redesign.

The scope is intentionally focused:

- A shift overview lists all items still out.
- Each row shows asset ID, item name, original borrower, current holder, and expected return.
- An administrator can record a custody transfer and update the expected return.
- Every transfer is appended to the item's history; earlier events are not overwritten.
- A physical return closes the active loan.
- Optional notes can be attached to the relevant asset trace.

We are not attempting to build a full procurement, reservation, messaging, or notification platform in this iteration.

## 6. Proposed workflow

### Friday pickup

The administrator issues each approved physical unit as an individual loan. The person collecting the kit is recorded as the original borrower and current holder.

### Partial return and transfer

The administrator records the projector and speaker as returned. For the adapter, they record a custody transfer from Kabir to Leela and revise the expected return time. The active loan now shows Leela as current holder while preserving Kabir as original borrower.

### Incoming shift

The incoming administrator opens the overview and immediately sees the outstanding adapter, its exact asset ID, current holder, and expected return. Opening its trace reveals the original issue and subsequent transfer. When the adapter is physically received, the administrator records its return.

## 7. Prototype design principles

- **Outcome first:** show what is still out and who has it before showing historical detail.
- **Plain language:** use labels such as “Still out”, “Current holder”, and “Expected back”.
- **Progressive detail:** show a concise dashboard first; open the full trace only when needed.
- **Preserve history:** changing the current holder must append an event, not erase the previous holder.
- **Asset-level accuracy:** use the unique physical asset ID in every trace.
- **Notes as context:** free text supplements structured status and events.
- **Administrator control:** only record a return after physical receipt.

## 8. Research plan and form

The prototype includes an interviewer-entry research form. The administrator does not need to operate the software to provide feedback; the interviewer records the response. Each entry captures an anonymous participant code, role, experience, evidence type, hardest handover task, frequency, severity (1–5), current workaround, preferred support, and an optional example.

The research page aggregates the most difficult tasks and frequency from saved responses. Responses can be exported to CSV. No names or contact details should be collected. The optional sample responses are fictional and explicitly labelled; they must not be reported as real research.

Suggested research questions:

- Walk me through the last shift handover you completed.
- Which handover task is most difficult: finding the current holder, identifying the exact asset, interpreting notes, updating custody, tracking due times, or confirming returns?
- How often does this happen?
- What do you do today when this happens?
- What is the consequence when the information is missing or incorrect?
- Which single change would save you the most time or prevent the most errors?

## 9. How we would validate the improvement

Conduct short interviews and observe real shift changes. Give an incoming administrator a realistic partially returned kit scenario and ask them to identify the outstanding asset, current holder, expected return, and latest custody event without asking the outgoing employee.

Measure:

- Time to identify all outstanding items.
- Percentage of items for which the current holder is identified correctly.
- Percentage of returns matched to the correct physical asset ID.
- Number of clarification calls/messages to the previous shift.
- Handover errors or unresolved discrepancies.
- Administrator confidence in the handover information.

Compare the existing note-based workflow with the prototype using similar tasks. Do not claim improvement until observed data supports it.

## 10. Before and after: administrator outcome

**Without the proposed workflow:** the incoming administrator reads notes, cross-checks the loan register, searches for asset IDs, infers who has an item, and may need to contact the previous shift.

**With Live Chain of Custody:** the administrator sees the outstanding-item list, current holder, due time, and latest event in one place, then opens a trace for details. The goal is less reconstruction and a more reliable handover.

## 11. Technical scope and limitations

This deliverable is a browser-based front-end prototype. It persists data in localStorage for the current browser only. It demonstrates immediate local UI updates, not multi-device real-time synchronization. A production implementation would require a shared backend and database, authenticated staff identities, role-based permissions, server-side timestamps, concurrency handling, and an append-only audit log. A later iteration could add receiver acknowledgement for custody transfers.

## Conclusion

The product opportunity is not simply to add another notes field or make the dashboard more decorative. It is to make the operational state of every outstanding physical item obvious and trustworthy at shift change. Live Chain of Custody is a focused first step toward that outcome.
