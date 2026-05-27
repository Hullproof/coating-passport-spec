# Changelog

All notable changes to the CoatingPassport schema are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
