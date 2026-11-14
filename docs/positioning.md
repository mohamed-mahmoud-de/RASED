# RASED Positioning: Competitors, Uniqueness, ML, Problem & Solution

Research: 2026-11-14. Context: after the face-to-face meeting with Captain-2, the team agreed that RASED focuses on **import/export shipments** (D-010, [decision-log.md](decision-log.md)). How a shipment and its bill of lading move: [bill-of-lading-flow.md](bill-of-lading-flow.md). Data sources: [shipment-data-sources.md](shipment-data-sources.md).

> Competitor features below come from their **public** web pages and vendor-written comparison articles, not from trial accounts. Accuracy figures are the vendors' own claims. "Nobody does X" means our search found nobody, not proof.

## 1. One-line positioning

> **Nowlun shows where your container is. RASED forecasts when your ship will really berth at Alexandria, why, and whether your papers will be ready in time, so you stop paying for delays you could have seen coming.**

Tracking is free everywhere (carrier websites, Nowlun), so tracking is RASED's **input layer, not its product**. The product is the forecast and the warning.

## 2. Competitors (links checked 2026-11-14)

| Competitor | Link | Overlap with RASED |
|---|---|---|
| **Nowlun** (Alexandria) | [nowlun.com](https://nowlun.com/en) | **Closest competitor.** See §3 |
| project44 | [project44.com](https://www.project44.com/) | Container tracking + predicted ETA, enterprise |
| FourKites | [fourkites.ai](https://www.fourkites.ai/) | Ocean tracking + predicted ETA, enterprise (vendor claims ≥95% final-destination ETA accuracy) |
| GoComet | [gocomet.com](https://www.gocomet.com/) | Tracking, demurrage/detention alerts, port-congestion dashboard |
| Portcast | [portcast.io](https://www.portcast.io/) | Predicts delays 48–72 h before the carrier reports them; "last free day" alerts. Egypt coverage not confirmed |
| Vizion | [vizionapi.com](https://www.vizionapi.com/) | Container-tracking API, last free day |
| Terminal49 | [terminal49.com](https://terminal49.com/) | Importer container tracking (US ports); free plan = 10 containers |
| ShipsGo | [shipsgo.com](https://shipsgo.com/) | Low-cost container tracking |
| SeaRates (now DP World) | [searates.com](https://www.searates.com/) | Tracking by B/L, container, booking or vessel; API |
| Kpler | [kpler.com](https://www.kpler.com/) | **Berth-time and terminal-wait prediction**; analyst tool, expensive |
| Lloyd's List Intelligence | [lloydslistintelligence.com](https://www.lloydslistintelligence.com/) | **ETA, berth time (claims 1–2 h once destination known) and port waiting time** |
| Windward | [windward.ai](https://windward.ai/) | Predicted vessel ETA, delay alerts |
| Sinay | [sinay.ai](https://sinay.ai/en/) | Predicted arrival at the anchorage |
| MarineTraffic / ShipInfo | [marinetraffic.com](https://www.marinetraffic.com/), [shipinfo.net](https://shipinfo.net/) | Free ship maps (what Captain-1 uses) |
| **The real competitor today** | — | WhatsApp, phone calls to the agent, Excel, carrier websites |

## 3. Nowlun in detail

**What it is:** "the first digital freight forwarder in the region" (its FAQ). Alexandria-based, offices in Egypt and Saudi Arabia, raised about $2.3M (seed, led by Ingressive Capital per WAYA). It earns from **selling freight** (FCL rates from 35+ lines, bulk, break bulk, air, inland trucking, customs support). Free tools bring customers in. Has an Arabic site and iOS/Android apps.

| Nowlun tool | What it does (public pages, 2026-11-14) |
|---|---|
| Tracking | Container, B/L or booking number, or **upload the B/L (PDF/JPEG/PNG) and it reads the numbers**. **Free, unlimited.** Location, status, documents; **WhatsApp + email** alerts. Tracks shipments not booked with Nowlun too |
| Port Pulse | 24 Middle East ports and chokepoints (incl. Alexandria, Suez Canal) labelled *normal / disrupted / suspended* **now**, with a "data as of" timestamp. No waiting-ship counts, no waiting times, **no forecast**. Data source not stated |
| ACID extension | Helps fill in ACID data for Nafeza |
| Laytime calculator | Demurrage/despatch for **chartered bulk ships** (charter-party terms), not container late fees |
| Logistics assistant | Mentioned in funding news as **in development** (AI) |

### Where RASED differs

| The importer's question | Nowlun | RASED |
|---|---|---|
| Where is my container now? | ✅ | ✅ (input layer) |
| **When will my ship really berth at Alexandria?** | ❌ carrier date only | ✅ anchorage-wait forecast → realistic berth date, as a range |
| **Why is it late?** | ❌ | ✅ "14 ships waiting + storm Thursday" (SHAP) |
| Is the port busy? | Label for *now* | Number of waiting ships and hours, **2–3 days ahead** |
| **Will my papers be ready before the ship?** | ❌ (ACID only) | ✅ original B/L, delivery order, customs vs. forecast berth date |
| **What will the delay cost?** | ❌ (bulk laytime only) | ✅ container free days → EGP |
| How accurate are you? | ❌ | ✅ public scoreboard: RASED vs. carrier ETA |

**Not unique any more (Nowlun has them):** B/L photo reading, WhatsApp alerts, Arabic UI, free tracking. Chatbot: Nowlun is building one, so it is not a lasting edge.

**Nowlun validates the market** (investors paid for this problem in Alexandria) and **could be a customer**: RASED's berth forecast as an API for forwarders.

## 4. RASED's uniqueness (strongest first)

1. **Prediction, not tracking:** anchorage-wait and realistic berth date at Alexandria, from data Nowlun doesn't show (Port Authority board, alexcont, GFW anchorage history, weather). This is also the ML + MLOps core of the thesis.
2. **Papers vs. ship:** joins Egypt's clearance chain (ACID → original B/L → delivery order → Nafeza declaration) to the forecast berth date and warns 2–3 days ahead.
3. **Neutral, not a forwarder:** RASED sells no freight. A forwarder (Captain-1: 200–300 shipments each) won't load its clients into a competitor forwarder's platform. *Validate in interviews.*
4. **Cost of delay in EGP** from each line's free days and Egyptian demurrage/detention tariffs (Emirates Line and Maersk publish Egypt tariffs).
5. **Explained, honest forecasts:** a range (P10/P50/P90), the reason, and a public accuracy record.
6. **Users Nowlun doesn't serve:** customs brokers, truckers, marine-services crews (tugs, pilots) like Captain-2.

**Proposal wording change:** drop "tracking" as a selling point; lead with forecast + reason, papers-vs.-ship, cost of delay, accuracy scoreboard.

## 5. The ML in RASED

| # | Model | Predicts | Method | Data | Must beat |
|---|---|---|---|---|---|
| 1 | **Anchorage wait / berth time** (core) | Hours before berthing, P10/P50/P90 | XGBoost/LightGBM quantile regression; later a survival model per ship | GFW anchorage episodes (DBSCAN zones), Port Authority board, alexcont, Open-Meteo | Persistence, seasonal-naive, "no wait" |
| 2 | **Arrival-date correction** | Real arrival vs. B/L ETA | searoute distance + speed history + gradient boosting | GFW port visits, ShipInfo | **The carrier's ETA on the B/L** (key baseline) |
| 3 | **Congestion outlook** | Ships waiting at Alexandria 1–3 days ahead | Time-series model, same features | GFW, board | Persistence |
| 4 | **The "why"** | Main causes per forecast | SHAP | Model 1 | — |
| 5 | **B/L reader** | Fields from PDF/photo | OCR + LLM, **no training**; evaluated on real redacted B/Ls | User uploads | Manual typing |
| 6 | **Chatbot** | Answers (Arabic/English) | LLM calling RASED's own API, grounded (§7) | RASED data only | — |
| 7 | **Papers-risk score** | Chance a container stays past its free days | Rules first; ML once real outcomes exist | Checklist + forecasts | — |

**MLOps:** Feast feature store (same `features/` code for batch and streaming); MLflow registry with `@champion`/`@challenger`; Evidently drift monitoring; drift → retrain, regression → rollback; every prediction logged and scored when the real berth date matures.

**Horizon:** importers and Captain-2 want **2–3 days ahead**, not 6–12 h (update O-002). **Synthetic data:** demos only, never training or evaluation (D-005).

## 6. "Enter by ship" (the 30-container idea)

**Idea (team, 2026-11-14):** the importer types the ship number and sees all its containers on that ship instead of typing 30 B/Ls.

**Reality:** no public source maps a ship to one importer's containers. The cargo manifest is confidential between the shipping line, its agent and customs. So a ship number alone cannot find "your 30 containers."

**What works:**
1. **One B/L often covers many containers.** 30 containers may be 1–3 B/Ls; a master B/L returns all its containers (Tradlinx and carrier APIs).
2. **Bulk add:** Excel list of B/L numbers, or the agent's arrival notice (usually lists every B/L on that ship).
3. **"Track by ship" view:** type the ship, RASED groups every shipment the user already has on it: one forecast, one alert, all containers on one screen.
4. **Later:** the importer's own carrier account (Maersk, CMA CGM, Hapag-Lloyd DCSA Track & Trace; CMA CGM's also searches by vessel) or a forwarder integration.

## 7. Chatbot

**Rule:** it answers only from RASED's data, never from memory. The LLM calls RASED functions (`get_shipment`, `get_forecast`, `get_papers_status`, the Egypt procedure guide) and cites them; if it doesn't know, it says so.

Example questions: "Which of my shipments arrive this week?", "Why is the MSC Anna late?", "What does it cost if the D/O arrives Sunday?", "What's a telex release?"

**Multimedia (SIM department):** Arabic voice questions (speech-to-text) and a 1-minute daily Arabic audio briefing (text-to-speech).

## 8. More features to add

**Build (cheap, and only we have the data):**
1. Accuracy scoreboard: "last 30 days, RASED vs. carrier ETA."
2. Carrier and route reliability into Alexandria, from GFW history.
3. Anchorage replay on the map: time-lapse of the queue (multimedia; Ever Given demo).
4. Egyptian calendar: Eid, holidays, Fridays in clearance timing.
5. What-if: "if my originals arrive Sunday, what will it cost?"
6. Share links: one shipment's live status for the broker, trucker or end customer.

**Skip / later:** WhatsApp alerts (paid Business API, Nowlun has it, Captain-2 asked for email); port comparison (rated low by Captain-2); live Port Said (no data); own AIS receivers (no-hardware rule).

## 9. Problem statement

> Egyptian importers and freight forwarders follow their sea shipments by hand: phone calls to the shipping agent, WhatsApp, Excel and carrier websites. The only date they have is the ETA printed on the bill of lading, which is the carrier's promise, not the day the container can actually leave the port. At Alexandria, ships often wait at anchorage before berthing because of berth availability, weather and several ships arriving together. Even after berthing, the container is released only when a chain of papers is complete: the ACID registration, the original B/L, the delivery order and the Nafeza customs declaration. The port knows the queue and the importer knows its papers, but nobody connects the two. When the papers fall behind the ship, containers sit in the terminal and the importer pays storage, demurrage and detention, and delivers late to its own customers. Global visibility platforms exist, but they are built and priced for large shippers, and we found none that models Alexandria's anchorage queue or Egypt's clearance chain.

## 10. Solution statement

> RASED is a web platform for Egyptian importers and freight forwarders. The user adds shipments by uploading a bill of lading, typing its number, or importing a list grouped by vessel. RASED identifies the ship, follows the ocean leg, and uses machine learning on satellite AIS history, the Alexandria Port Authority board and weather data to forecast the anchorage wait and a realistic berth date, as a range with the reasons behind it. It compares that date with the importer's paperwork, warns two to three days ahead when papers will be late, and estimates the cost of the delay in Egyptian pounds. Users get email alerts, a live map, and an Arabic assistant that answers questions from their own shipment data. Behind it, a production MLOps pipeline (feature store, model registry, drift monitoring, retraining and rollback) checks every forecast against the real berth time and keeps the model accurate.

## 11. Text diagram

```
IMPORTER / FREIGHT FORWARDER
   │  adds a shipment:  B/L upload (PDF/photo) · B/L number · Excel list · ship name
   ▼
RASED · INPUT ──────────► reads the B/L: vessel, voyage, port of loading → Alexandria,
   │                         ETA, containers, ACID
   ▼
RASED · IDENTIFY SHIP ──► vessel name → IMO number        (ShipInfo, GFW)
   ▼
RASED · OCEAN LEG ──────► last port + position → distance → arrival estimate
   │                         (GFW, ShipInfo, searoute)       [ML 2: arrival correction]
   ▼
RASED · PORT END ◄─────── live queue: Port Authority board, alexcont
   │  (ML core)  ◄─────── history: GFW anchorage episodes · weather: Open-Meteo
   │   anchorage wait: earliest / likely / latest + why     [ML 1 + SHAP]
   ▼
REALISTIC BERTH DATE
   ├──► PAPERS CHECK ◄──── importer ticks: ACID ✓  originals ✓  D/O ☐  customs ☐
   │       "ship berths ~Thursday, delivery order still missing → risk"
   ├──► COST OF DELAY ◄─── carrier free days + Egypt demurrage/detention tariffs (EGP)
   ▼
OUTPUT ─► email alerts · live map + anchorage replay · Arabic chatbot (text/voice)
   │       · share link for broker / trucker / customer · accuracy scoreboard
   ▼
FEEDBACK LOOP ─► real berth / gate-out dates (Port Authority board, user)
                 → score every forecast → drift? retrain : keep → worse? roll back   (MLOps)
```

## 12. Defense answer

- **"Nowlun already does this."** → Nowlun is a forwarder that tracks where the container is, for free, to sell freight. RASED is neutral and predicts when the ship will really berth, why, and whether the papers will be ready, with a public accuracy record. Nowlun could be a customer of RASED's forecast API.

## 13. To do

- [ ] Make a free Nowlun account and track one real container: what does the logged-in dashboard / app show? Any ETA prediction?
- [ ] Interview questions: "Do you use Nowlun or a similar tool?", "What does it not tell you?", "Would you put your clients' shipments into a forwarder's platform?", "Would you tick your paperwork steps by hand?"
- [ ] Prove the core claim: forecast beats the B/L's ETA on real Alexandria arrivals.
- [ ] Update the proposal's competitor analysis (Nowlun missing) and problem/solution statements.

## Sources

- Nowlun: [home](https://nowlun.com/en), [tracking](https://nowlun.com/en/track), [Port Pulse](https://nowlun.com/en/port-pulse/map), [FAQ](https://nowlun.com/en/faqs), [MENAbytes: seed](https://www.menabytes.com/nowlun-seed/), [WAYA: $600K raise](https://waya.media/egypts-nowlun-raises-usd-600k-to-streamline-mena-freight-logistics/)
- Comparisons (vendor-written): [GoFreight](https://gofreight.com/blog/freight-tracking-software-comparison), [SeaVantage](https://www.seavantage.com/blog/best-container-tracking-software-in-2026-8-platforms-compared), [BuyCo](https://buyco.co/blog/visibility/top-6-ocean-freight-visibility-platforms/), [CEOPickz D&D tools](https://ceopickz.com/list/freight-forwarding/best-demurrage-detention-software/)
- Prediction products: [Portcast terminal data](https://www.portcast.io/port-terminal-visibility), [Lloyd's List Intelligence on ETAs](https://www.lloydslistintelligence.com/resources/blog/inaccurate-vessel-eta-marine-maritime-business), [Kpler Containers Intelligence](https://www.kpler.com/product/maritime/container-intelligence), [Windward vessel ETA](https://windward.ai/solutions/vessel-eta/), [Sinay ETA](https://sinay.ai/?p=24038)
- Carrier APIs: [CMA CGM Track & Trace](https://apis.io/apis/cma-cgm/cma-cgm-track-and-trace-api/), [Tradlinx API](https://tradlinx.com/docs/welcome), [SeaRates tracking](https://docs.searates.com/reference/tracking), [MSC integrations](https://www.msc.com/solutions/digital-solutions/direct-integrations)
- Egypt tariffs: [Emirates Line Egypt advisories](https://www.emiratesline.com/advisories-tag/egypt/), [Maersk Egypt](https://www.maersk.com/local-information/europe/egypt/export)
