# Employee Request Management System — assessment submission

Automation / Systems Analyst, Stage 2. Three parts as specified in the brief.

---

## What is in this folder

```
01-workflow-design.html          Part 1 — full workflow design document (open in any browser)
02-prototype-reqdesk.html        Part 2 — working prototype (open in any browser)
03-part3-summary-skeleton.md     Part 3 — decision skeleton to write the summary from
lib/mermaid.min.js               diagram renderer, bundled so Part 1 works offline
exports/
  workflow-design.pdf            Part 1 as a PDF, diagrams included
  fig-01-master-lifecycle.png    end-to-end lifecycle swimlane
  fig-02-intake-normalisation.png
  fig-03-routing-decision-tree.png
  fig-04-escalation-ladder.png
  fig-05-state-machine.png
```

Both HTML files are self-contained. Double-click to open — no server, no install, no account.
Keep `lib/` next to `01-workflow-design.html` or the diagrams will not draw.

---

## Part 1 — Workflow Design

`01-workflow-design.html`, or `exports/workflow-design.pdf`, or the five PNGs.

Covers the five required specifications and then some:

| Section | Contents |
|---|---|
| 01 Master lifecycle | End-to-end swimlane across intake → classify → route → monitor → resolve |
| 02 Intake | Channel adapters, the normaliser, identity resolution, duplicate merge, captured fields |
| 03 Taxonomy | 5 departments, request types, the actual signal keywords, how classification decides |
| 04 Routing | Decision tree plus six assignment rules in precedence order |
| 05 Priority & SLA | Impact × urgency matrix, priority overrides, per-priority SLA targets |
| 06 Escalation | Four-rung ladder, four trigger types, and why escalation is not reassignment |
| 07 State machine | Internal states mapped to the three the requester sees, plus enforced transition rules |
| 08 Resolution | Six-step verified closure pathway |
| 09 Scenario catalogue | 22 cases the system must handle, with the expected behaviour and end state |
| 10 Failure modes | How the system itself breaks, and what happens then |
| 11 Metrics | What gets measured, tied back to the four inefficiencies in the brief |

---

## Part 2 — Prototype

`02-prototype-reqdesk.html`. Five tabs: Submit a request, My requests, Agent console, Analytics,
How it works.

**Two-minute tour**

1. On **Submit a request**, type `my salary was credited short this month` into the subject.
   The classification panel scores all four departments live and auto-routes to Payroll.
2. Replace it with `system not letting me apply leave`. Signals split between HR and IT,
   confidence falls into the middle band, and the verdict becomes route-and-flag.
3. Replace it with `need help with the thing from last week`. Nothing matches, confidence is
   0.00, and it is held in triage. The system refuses to guess.
4. Set *Who is affected* to `My site or the company` and *What is blocked* to
   `I am blocked entirely`. Priority jumps to P1 and the resolve target collapses to 4 hours.
5. Submit it, then press **+24h** twice in the header and open **Agent console**. The escalation
   column climbs L0 → L1 → L2 on its own and rows past SLA gain a red stripe.
6. Open any ticket. Resolve it, finalize it, then reopen it — the ticket ID stays the same and
   the reopen counter increments. Every step is written to the audit trail at the bottom.

**Reset demo** in the header restores the twelve seeded tickets at any time.

The **How it works** tab states exactly what is genuinely implemented and what is simulated.
Read it before judging the prototype — the boundary is deliberate, not accidental.

---

## Part 3 — Solution Summary

The brief asks for a *non-AI* summary. `03-part3-summary-skeleton.md` is therefore a structured
list of the assumptions, trade-offs and decisions actually embedded in the build — raw material
to write the two-page summary from, not the summary itself.

---

## Design positions worth asking me about

- The channel is not the entry point; the **normaliser** is. Adding a channel is an adapter, not a redesign.
- **Keyword lexicon first, LLM second.** A word list an HR lead can edit beats three points of model accuracy in year one.
- **Three confidence bands**, not one threshold — the middle band lets the system act while staying checkable.
- **Priority is computed** from impact × urgency. Nobody self-declares urgency, so nothing is universally urgent.
- **Exactly one accountable human at all times.** When it cannot be an agent, it is the group lead.
- The SLA clock **pauses for the requester but not for a vendor**. A vendor's delay is still our delay, and a paused clock turns SLA reporting into fiction.
- **Escalation adds watchers**, and only reassigns at L2 after two hours untouched. Yanking work at the first nudge produces SLA gaming, not speed.
- A reopen inside 14 days **keeps the same ticket ID**, which is what makes reopen rate a real quality metric.
- Automation is applied to **routing and monitoring, not to deciding**. Wherever an outcome affects someone's pay, leave or standing, a human is accountable.
