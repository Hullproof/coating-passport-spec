# Changelog

All notable changes to the CoatingPassport schema are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1] — 2026-06-02

Backward-compatible additions (MINOR bump per governance). Persisted `1.0` passports remain valid without change; `schema_version` now accepts `"1.0"` or `"1.1"`, and newly generated passports declare `"1.1"`.

### Added
- Optional `CoatingPassport.structural_monitoring` — structural-health-monitoring (SHM) block carrying hull-girder stress, used-fatigue-life, slamming, ice and sloshing loads as a first-class telemetry modality alongside `performance`. Each `metric` carries `confidence`, `data_quality_score`, and a measurement window (audit-ready lineage); plus `sensing_principle`, `class_notation` (DNV HMON / ABS SMART(SHM) / BV MON-HULL / …), and `ingested_via`. Source-agnostic (e.g. SENSFIB, BMT, Strainstall); ingest path of record is DNV Veracity OVD.
- Asset taxonomy now also reflects production types added since 1.0: aquaculture asset types (`aquaculture_net_pen`, `aquaculture_mooring`, `aquaculture_feed_barge`, `aquaculture_ras_tank`, `aquaculture_closed_pen`, `aquaculture_offshore_pen`) and finding categories `net_damage` and `biofilm`.

### Notes
- All changes are additive; no migration required. A `1.0` consumer validating a `1.1` passport sees only unknown optional fields.
- Related derived export (not part of the schema): `passportToEmissionsEvidence()` maps a passport to CountEmissions EU / EN ISO 14083:2023 supporting evidence.

## [1.0] — 2026-05-27

Initial public release of the CoatingPassport schema. Snapshot of the version live in production Hullproof inspections.

### Added
- Core CoatingPassport object with `passport_id`, `tenant_id`, `schema_version`, `asset`, `inspection`, `findings`, `compliance`, `history`, `interoperability`, `generated_at`.
- `AssetType` enum covering 13 asset categories from `vessel_hull` through `naval_hull`.
- `Material` enum covering 11 material categories.
- `FindingCategory` enum covering 18 categories — 14 visual (corrosion, coating_breakdown, biofouling, etc.) plus 4 non-visual (acoustic_anomaly, vibration_anomaly, telemetry_anomaly, chemical_marker).
- `PrimaryModality` enum covering 8 sensor modalities (video, image, acoustic, vibration, telemetry, chemical, thermal, radar).
- Optional `Finding.modality_metadata` — open record for standard-specific keys (`iso_12944_class`, `iso_8501_rust_grade`, `imo_biofouling_rating`, etc.).
- Optional `CoatingPassport.performance` — ISO 19030 Performance Value with reference period, evaluation period, correction log, confidence band, compliance gate.
- Interoperability hooks for Cognite CDF, OSDU, DNV Veracity, knowledge graph.

### Notes
- The schema is generated from the zod source in [hullproof-dashboard/src/lib/passport/types.ts](https://github.com/jasonhawk-create/hullproof-dashboard/blob/main/src/lib/passport/types.ts). To regenerate: `npm run export:passport-schema` in the dashboard repo.
