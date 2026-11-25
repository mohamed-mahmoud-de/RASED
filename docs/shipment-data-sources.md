# Shipment & Bill-of-Lading Data Sources (for O-008 b)

Research and tests: 2026-10-30. Context: [Captain-1 interview](field-research/2026-10-30-captain-1-voice-notes.md) and open question **O-008** in [decision-log.md](decision-log.md). Port and AIS sources are covered in [data-sources.md](data-sources.md).

**Legend:**
- ✅ **tested by us**: opened, downloaded or called it ourselves on 2026-10-30
- 🔒 **needs an account**: the team must register; we don't create accounts for tests
- ☑️ **provider documentation**: not tested
- ❌ **tested, not usable**

## Bottom line

1. **A bill of lading (B/L) has the same core fields everywhere, but not one layout.** Every B/L has:
   - B/L number, shipper, consignee, notify party;
   - vessel name + voyage number, port of loading (POL), port of discharge (POD);
   - containers and seals, cargo, packages, weight;
   - place and date of issue.

   Each carrier prints its own form. The main types are the **master B/L** (shipping line), the **house B/L** (forwarder, often on the FIATA FBL form) and the **charter-party B/L** (bulk cargo, e.g. BIMCO CONGENBILL). Three open electronic standards define the fields: **DCSA** (container lines), **FIATA eFBL** (forwarders) and **BIMCO eBL** (bulk).
2. **The B/L gives the vessel *name*, not its IMO number.** DCSA's electronic B/L does carry `plannedDepartureDate` and `plannedArrivalDate`; paper forms often don't. **Tested fix for name → IMO:** the ShipInfo free search returns IMO + MMSI for a ship name.
3. **Every B/L into Egypt carries a 19-digit ACID number.** The ACI pre-registration on Nafeza has been mandatory since Oct 2021, so imports are registered before the ship sails. We can't access it.
4. **No free, real, document-level B/L dataset exists.** We checked Kaggle, Hugging Face, data.world and GitHub: everything document-shaped is synthetic or a template. Real B/L *records* exist only from US customs (ImportYeti free view, Trademo free 10-day sample).
5. **The full free chain works.** We tested every link:
   - ship name → IMO (ShipInfo);
   - IMO → real voyage history (GFW port visits);
   - IMO → current position, ≥14 h old (ShipInfo);
   - Alexandria expected/berthed ships (APA board; alexcont for container ships);
   - remaining sea distance → ETA (searoute);
   - port codes (UN/LOCODE).

   The only missing link is live container events from carriers, which need team accounts.

## Test results (2026-10-30)

| Source | Result | Verdict |
|---|---|---|
| **DCSA eBL 3.0.2 OpenAPI** (SwaggerHub `dcsaorg/DCSA_EBL/3.0.2`) | Downloaded. Apache-2.0, 110 schemas, 443 inline examples. `TransportDocument` has 19 required fields; `Transports` has `plannedArrivalDate`, `plannedDepartureDate`, `portOfLoading`/`portOfDischarge` (UN/LOCODE), `vesselVoyages[vesselName, carrierExportVoyageNumber]`. No IMO field | ✅ **Use as our internal B/L schema** |
| **DCSA reference test data** (`DCSA-Edocumentation`, `V8_1__test_data_ebl.sql`) | Downloaded (48 KB): 2 transport documents, 3 shipping instructions, 8 bookings; ports USMIA, NLRTM, SGSIN, DEHAM; vessels by IMO | ✅ Tiny; good for unit tests only |
| **FIATA eFBL** (GitHub) | Schemas downloaded; example 1/3 validates, 2 and 3 fail on one extra field. Schema has `mainCarriageTransportMovement`, `voyageNumber`, `loadingBaseportLocation`, `unloadingBaseportLocation`. Example values are random fake text. No licence file | ✅ House-B/L field reference. Ask FIATA before redistributing |
| **BIMCO eBL standard** (20 fields v1.0; v2.0 reported with 36) | bimco.org returns HTTP 403 to automated download | ❓ A team member downloads it by hand |
| **ImportYeti** (US customs B/Ls) | Web works free. An Alexandria exporter shows real B/L numbers (CMA CGM `CMDU…`, MSC `MEDU…`), dates (latest 23 Oct 2026), weights, containers. Vessel/port columns hidden on free view; CSV and API paid | ✅ Real B/L examples to show the supervisor. Don't scrape |
| **Trademo US Imports** (AWS Data Exchange) | Needs an AWS account and accepting the vendor EULA | 🔒 Team decision: one AWS account, then the free 10-day sample (Sep 2022) |
| **OEC Bill of Lading bulk download** (`oec.world/en/resources/bulk-download/bill-of-lading`) | Same US customs (CBP) source as ImportYeti/Trademo, cleaned by OEC. Full files need the **Premium plan: $1,999/month** (50% off for academic emails). Free: 3 sample CSVs, 5–10 records each. Imports sample has vessel name + code (IMO), voyage, master and house B/L numbers, loading/unloading port, **estimated vs actual arrival date** (OEC adds `lag_days`). US ports only, nothing for Egypt | ✅ Samples = good real B/L field reference. ❌ Full data is paid (rule: no paid data) |
| **Kaggle "Container Tracking Data Set"** (muzammalnawaz) | Metadata read: 250 KB xlsx, China → US containers with a predicted vs actual delivery date, 2022, **licence Unknown**; download needs a Kaggle login | ☑️ Interesting ETA-error reference; licence blocks redistribution |
| **Kaggle / databasesample "shipping" sets** | All synthetic (stated by authors), no vessel/B/L realism | ❌ |
| **Hugging Face ClarusC64 B/L set** | 10 synthetic rows | ❌ |
| **ShipInfo.net preview API** (`/v1/bootstrap/vessels/search`, `/last_position`) | **No key needed**: headers `X-Agent-Name` + `X-Agent-Vendor`; 60 req/day and 6/min per endpoint; positions delayed ≥14 h; attribution "Data by ShipInfo.net". Name search → IMO + MMSI + type ✅. 6 ships the APA board expects this week: all found; 4 with positions 14–19 h old, 1 at 77 h, 1 at 663 h. SEA GLORY was due 28 Oct but was ~300 nm away at 6 kn (late; exactly our use case) | ✅ **Name → IMO matching + coarse position.** Too limited for 300 shipments/day; ShipInfo sells a paid tier |
| **GFW vessel search + port-visit events** (our token) | Works. SEA GLORY: 36 port visits in 2026 (Istanbul, Beirut, El Arish, …); EVER GIVEN: 37 (Felixstowe, Antwerp, Rotterdam, Tanjung Pelepas 25–26 Oct). Lag ~4 days. **Gotchas:** (1) Cloudflare blocks Python's default User-Agent (send a project UA); (2) a ship can have several GFW identities, so union all identities with the same IMO | ✅ **Backbone of the demo recipe** (real voyages, real departures) |
| **APA Alexandria board** (`collector/apa_scraper.py`) | Ran once: expected 100, in port 91, departed 19 rows. 77 expected rows have a plausible IMO; many ETAs are stale (2021–2023 records). **No snapshots between 2026-10-06 and today: the 24/7 poller is not running on this machine** | ✅ Live. ⚠️ Fix O-004 |
| **alexcont.com berthing situation** (Alexandria container terminal) | Public page, no robots.txt. 159 table rows: vessel, line, agent, ETA / ATA / ATB / ATW / ETC, plus expected vessels for the next 26 days. Last update 28/10 08:00 (not daily) | ✅ **New free live source for container-line ETAs at Alexandria.** Poll politely |
| **Maersk API** (`api.maersk.com`) | 403 without a key. Portal needs registration; whether tracking/schedules are free for non-customers is unclear (third-party sources disagree) | 🔒 Team registers and checks |
| **Hapag-Lloyd API** (`api.hlag.com`, DCSA T&T 2.2) | 401 without a key; portal blocks bots (403) | 🔒 Team registers and checks |
| **CMA CGM API** (`apis.cma-cgm.net`) | 401 without a key; portal page is a JS app | 🔒 Team registers and checks |
| **Terminal49** | 401 without a key. Free plan (their help centre): **10 active containers**, free API = create tracking requests only (no GET of events) | 🔒 Too small; demo only |
| **ShipsGo / SafeCube / IQAX Big Schedules** | Trials only (5 credits / 14 days / trial form) | 🔒 Demo only |
| **searoute** (Python, Apache-2.0) | Installed and ran: Shanghai → Alexandria 7,446 nm (via Suez, ~22 days at 14 kn); Rotterdam → Alexandria 3,177 nm; Izmail → Alexandria 927 nm | ✅ Remaining distance → baseline ETA |
| **UN/LOCODE** | unece.org blocks automated download (403); the DataHub GitHub mirror works (updated 2026-10-16): 127 Egyptian locations, 41 ports. EGALY, EGEDK, EGPSD, EGPSE, EGDAM, EGSOK, EGSUZ, EGADA present (EGPSD and EGEDK have no coordinates) | ✅ Port code normalisation |
| **AIS port pages** (VesselFinder, MyShipTracking, VesselTracker expected arrivals) | Not tested: scraping is against VesselFinder's terms; the others are AIS-derived like ShipInfo | Skip |
| **SynthDoG** (synthetic document images) | Not tested: only needed if we ever train a B/L PDF reader (see below) | Later |

## Sources by role

| Role in Rasid | Free source (tested) | Upgrade path |
|---|---|---|
| B/L schema | DCSA eBL 3.0 (+ FIATA eFBL for house B/Ls, BIMCO for bulk) | — |
| Real B/L examples | ImportYeti free view; redacted B/Ls from Captain-1 and forwarders | Trademo 10-day sample (AWS account) |
| Ship name → IMO | ShipInfo search; GFW vessel search | — |
| Voyage history (demo + training) | GFW port visits (~4-day lag) | — |
| Current position | ShipInfo (≥14 h delay, 60/day) | ShipInfo paid tier / Datalastic |
| Container events (gate-out, loaded, discharged) | None free at scale | Carrier APIs (Maersk, Hapag-Lloyd, CMA CGM) once registered |
| Alexandria arrival / berth (ground truth) | APA board (all ships) + alexcont (container ships) | APA written permission (supervisor letter) |
| Distance / baseline ETA | searoute | — |
| Port codes | UN/LOCODE (DataHub mirror) | — |

## Synthetic data: where it's allowed

The project rule ([CLAUDE.md](../CLAUDE.md), D-005) is that there is **no synthetic data in training or evaluating the forecast model**. That still holds, and the forecast doesn't need synthetic data: GFW and the APA board are real.

Synthetic B/Ls are fine as **user input**: demo shipments, end-to-end tests, load tests. They tell the app which real ship to follow; they don't teach the model anything.

If we later train a model to *read* B/L PDFs, training on synthetic documents is a new decision (log it). Simpler: use an off-the-shelf extractor (LLM or OCR) with no training, and evaluate it on real redacted B/Ls.

## Demo dataset recipe (all links tested)

1. **Pick real voyages.** From the APA board and alexcont, list ships that recently arrived at Alexandria. Get each IMO (board column, or ShipInfo name search).
2. **Get their real past ports.** From GFW port visits (union all identities per IMO), take the previous port and departure time.
3. **Generate a DCSA-shaped B/L for each:**
   - **real:** vessel name, voyage, POL/POD as UN/LOCODE, ETD = real departure;
   - **promised ETA:** departure + searoute distance ÷ 14 kn;
   - **invented:** shipper, consignee, cargo, container numbers (valid ISO 6346 check digit).
4. **Replay as if live.** Alerts fire on real port-visit events; the wait forecast runs on real anchorage data.
5. **Score against reality:** actual arrival and berth time (APA board, alexcont, GFW).

## To do

- [ ] **Restart the 24/7 APA poller (O-004).** No snapshots since 2026-10-06; that history is lost for good.
- [ ] Team registers on the Maersk, Hapag-Lloyd and CMA CGM portals: are tracking and schedule APIs free for non-customers, and what are the limits?
- [ ] Decide on one team AWS account for the Trademo free sample (EULA).
- [ ] Download the BIMCO eBL field list by hand.
- [ ] Add alexcont to the collectors (daily, polite) and to [data-sources.md](data-sources.md).
- [ ] Ask Captain-1 and forwarders for 5–10 redacted real B/Ls.
- [ ] Add the GFW gotchas to `collector/fetch_gfw.py`: project User-Agent; union identities per IMO.
