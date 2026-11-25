# Meeting Notes

Newest first. Copy the template for every team meeting, supervisor meeting, and customer interview. Move any decisions into `decision-log.md`.

## Template

```
### YYYY-MM-DD — <Team sync | Supervisor | Customer interview> — <topic>
Attendees:
Agenda:
Notes:
Decisions:            (→ add to decision-log.md with an ID)
Action items:         - [ ] <owner> — <task> — due <date>
Blockers / risks:
Next meeting:
```

### Customer interview add-on

```
Company / role:       (agent, forwarder, terminal, carrier, bunker supplier, chandler…)
Current process:      How do they learn about congestion today? What tools?
Pain & cost:          Last time waiting cost them money: how much, how often?
Decision they'd change with a forecast:
Lead time needed:     (hours/days)
Delivery preference:  dashboard / email / WhatsApp / API / Excel
Would they pay?       budget owner, rough range, pilot interest
Would they share ground truth? (actual anchorage/berth timestamps)
```

## Log

### ~2026-11-01 (exact date TBC) — Customer interview — Captain-2, face to face
Attendees: Captain-2 (marine services, oil-terminal company; name kept off GitHub), team (recorded from the team lead's chat summary; details to be added)
Notes:
- Captain-2 demonstrated how the shipping flow works ("how the system of the ocean works").
- The team demonstrated RASED as the **import/export system** agreed earlier.
- He showed that there are **different types of bill of lading** (researched in [bill-of-lading-flow.md](bill-of-lading-flow.md) §4).
Decisions: RASED focuses on import/export shipments (→ D-010).
Action items:
- [ ] Team — talk to import companies, using the questions in [bill-of-lading-flow.md](bill-of-lading-flow.md) §7 and [positioning.md](positioning.md) §13
- [ ] Team — ask for 3–5 redacted B/Ls
- [ ] Team — make a free Nowlun account and test one real container
Next meeting: TBC

### 2026-10-20 — Supervisor — proposal feedback
Attendees: Prof. Mohamed Kholief, team (recorded by Mohamed Mahmoud from memory; more may be added)
Notes:
- The idea is good but **not unique enough**: ports and maritime operators already have systems that show current congestion.
- Team answer: our core is the **6–12 h forecast**; competitors publish averages and current status. The supervisor agreed, but wants the project to be **very unique and sellable**.
- Go talk to **port authorities, solution companies and people working in the field**; collect unique features from them.
- **Synthetic / simulated data is acceptable** to the supervisor: he wants to see every feature working end to end.
- **Integrate multimedia**: the department is Software Industry and **Multimedia** (SIM).
- **Rename to RASED** (راصد). "Rasid" reads as رصيد (phone credit).
- Deliver a **full website**, not a Streamlit dashboard.
- Final documentation follows the department template "Senior Project1 Documentation" (4 chapters: Introduction, Requirements Analysis, Design, Implementation Aspects). **Chapters 1–2 are due next week (~2026-10-27).**
Decisions:            (→ to log in decision-log.md) name RASED; full website (closes O-005/P-003 dashboard part); synthetic data allowed for feature demos (revises D-005)
Action items:
- [ ] Team — interviews/questionnaire with port, agency and solution-company people (also feeds template §2.a–c) — launch by 2026-10-22
- [ ] Team — Chapters 1–2 of the template — due ~2026-10-27
- [ ] Team — shortlist unique features + multimedia features
- [ ] Team lead — rename Rasid → RASED in docs, brand assets, repo
Blockers / risks: one week for chapter 2 includes collecting survey responses.
Next meeting: TBC
