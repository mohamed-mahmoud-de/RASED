# RASED (راصد) — Smart Port & Maritime Congestion Predictor

RASED tells Egyptian importers and freight forwarders **when their ship will really berth at Alexandria**, and whether their papers will be ready in time.

An importer adds a shipment (bill of lading photo, B/L number, Excel list or ship name). RASED identifies the ship, follows it at sea, and uses machine learning on satellite ship-tracking (AIS) history, the Alexandria Port Authority ship board and weather data to forecast the anchorage wait and a realistic berth date, as a range with the reasons behind it. It compares that date with the importer's paperwork, warns two to three days ahead when papers will be late, and estimates the cost of the delay in Egyptian pounds. Behind it runs a production data engineering + MLOps pipeline (feature store, model registry, drift monitoring, retraining and rollback).

Graduation project, **Alexandria University — Faculty of Science — Software Industry and Multimedia (SIM) Department**. Supervisor: **Prof. Mohamed Kholief**.

**Team:**
- Mohamed Mahmoud — Team Lead, MLOps Engineer & Data Engineer
- Reem Adel — Data Engineer
- Alhassan Mohamed — Data Engineer
- Youssef Nasser — Data Engineer
- Youssef Elkomy — Data Engineer

## 📄 Documents

**All project documents are in [`docs/`](docs/README.md)**, with a suggested reading order.

- **Proposal v3** (import/export focus, 30 pages): [`docs/proposal/proposal.pdf`](docs/proposal/proposal.pdf)
- **Current direction and competitors:** [`docs/positioning.md`](docs/positioning.md)
- **New to shipping terms?** Start with [`docs/maritime-101.md`](docs/maritime-101.md)
- **Senior Project 1 documentation plan:** [`docs/course/senior-project-1-requirements.md`](docs/course/senior-project-1-requirements.md)

## Status (2026-11-25)

- **Direction (D-010):** import/export shipments into Alexandria; forecast horizon of days, not hours.
- **Proposal v3** submitted 2026-11-14, built from a clickable English/Arabic prototype.
- **Method (D-012):** Scrum, one-week sprints, tracked in Jira.
- **Documentation schedule (team targets):** Chapters 1–2 on 29 Nov, Chapter 3 (design) on 6 Dec, Chapter 4 + complete document on 13 Dec 2026.
- This repository contains **documents only**, for supervisor review (D-009). Source code will be added after the documents are approved.

---
*Satellite AIS data powered by [Global Fishing Watch](https://globalfishingwatch.org) (CC BY-NC 4.0, non-commercial use).*
