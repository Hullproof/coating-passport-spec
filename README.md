# CoatingPassport — open spec

> Canonical JSON Schema for the Hullproof CoatingPassport — a machine-readable, audit-ready asset condition data product.

The CoatingPassport is the central data product of the [Hullproof](https://hullproof.com) Physical Asset Condition Intelligence platform. It captures inspection findings (visual, acoustic, vibration, telemetry, chemical, thermal, radar — multi-modal) with explicit confidence scores, lineage to source data, and audit-ready compliance metadata.

This repository publishes the schema as an open standard so external integrators — classification societies, P&I clubs, charterers, port authorities, GIS vendors, and competing inspection-tool vendors — can produce, consume, and verify CoatingPassports without proprietary tooling.

## Files

| File | What it is |
|---|---|
| [`coating-passport.schema.json`](./coating-passport.schema.json) | The canonical JSON Schema (Draft 2020-12). Generated from the zod source in the [Hullproof dashboard repo](https://github.com/jasonhawk-create/hullproof-dashboard). |
| [`examples/`](./examples/) | Worked examples — a clean offshore-jacket inspection, a fouled vessel hull with ISO 19030 PV. |
| [`CHANGELOG.md`](./CHANGELOG.md) | Schema version history. Current: `1.0`. |
| [`governance.md`](./governance.md) | How changes to the spec are proposed, reviewed, and accepted. |
| [`LICENSE`](./LICENSE) | Apache License 2.0. |

## What's in a CoatingPassport

Every passport carries:

- **Asset identity** — `asset_id`, `asset_type` (vessel_hull, ballast_tank, cargo_hold, offshore_platform_jacket, wind_monopile, wind_turbine_blade, bridge_section, power_pylon, pipeline, industrial_tank, subsea_structure, harbor_quay, naval_hull), material.
- **Inspection metadata** — when, by whom, from what footage source (rov, drone, handheld, fixed_camera, satellite, crawler, auv).
- **Findings** — atomic condition observations. Each carries `category`, `severity`, `confidence` (0–1), `n_frames_supporting`, `image_quality_score`, location on the asset, source frame URIs, AI model version, optional human-review metadata, and **`modality_metadata`** for standard-specific fields (e.g. `iso_12944_class`, `iso_8501_rust_grade`, `imo_biofouling_rating`).
- **Compliance** — EU AI Act classification, model card URI, data lineage URI, review workflow status, applicable standards.
- **History** — append-only version chain.
- **Interoperability hooks** — Cognite CDF compatibility, OSDU mapping, DNV Veracity export URI, knowledge graph URI, external system ids.
- **Optional ISO 19030 PerformanceValue** — hull and propeller speed-loss measurement with confidence band, reference period (sea trial, post dry-dock, design point, rolling baseline), evaluation period, correction log, method, and compliance gate.

## Why open

Three reasons:

1. **Standard-setting position.** Cognite, DNV, OSDU all publish their schemas openly. CoatingPassport joining that company normalises it as the asset-condition data product reference.
2. **Trust through inspection.** Charter-party tribunals, P&I claims, and classification surveys all benefit when the data structure is verifiable rather than proprietary. The closed core (VLM inference, biofouling-attribution engine, customer data) stays in the Hullproof product; the open schema is the contract layer.
3. **Integration ergonomics.** Third-party developers building against Hullproof want a citable schema. PyPI clients, OpenAPI generators, and JSON Schema validators all consume this directly.

## Using the schema

### JSON Schema validation (any language)

```bash
# Python
pip install jsonschema
python -c "
import json, jsonschema
schema = json.load(open('coating-passport.schema.json'))
passport = json.load(open('examples/offshore-jacket-clean.json'))
jsonschema.validate(passport, schema)
print('valid')
"

# Node.js
npm install ajv
node -e "
const Ajv = require('ajv').default;
const ajv = new Ajv({ strict: false });
const schema = require('./coating-passport.schema.json');
const passport = require('./examples/offshore-jacket-clean.json');
const validate = ajv.compile(schema);
console.log(validate(passport) ? 'valid' : validate.errors);
"
```

### Python SDK

Use [`hullproof`](https://github.com/Hullproof/hullproof-python) — the official Python client — for typed access to live passports and ISO 19030 PV computation.

### TypeScript / zod source

The schema is generated from [`src/lib/passport/types.ts`](https://github.com/jasonhawk-create/hullproof-dashboard/blob/main/src/lib/passport/types.ts) in the Hullproof dashboard repo. zod consumers can import the typed schema directly.

## Versioning

`schema_version` is a literal field in every passport. Current version: `1.0`.

Breaking changes bump the major version (`2.0`); additive changes (new optional fields, new enum values) bump the minor (`1.1`). See [`CHANGELOG.md`](./CHANGELOG.md).

## Related schemas and standards

- **[ISO 19030](https://www.iso.org/standard/63774.html)** — Hull and propeller performance measurement. The `performance` block of a passport implements an alternative method per ISO 19030-3 Annex A.
- **[ISO 12944](https://www.iso.org/standard/64832.html)** — Corrosion protection of steel by paint systems. Referenced in `modality_metadata.iso_12944_class`.
- **[ISO 8501-1](https://www.iso.org/standard/55833.html)** — Surface preparation visual standards. Referenced in `modality_metadata.iso_8501_rust_grade`.
- **[IMO MEPC.378(80)](https://wwwcdn.imo.org/localresources/en/OurWork/Environment/Documents/MEPC.378(80).pdf)** — Biofouling guidelines. Referenced via finding category `biofouling` and `imo_biofouling_rating`.
- **[OSDU](https://osduforum.org/)** — Open Subsurface Data Universe. Hullproof exports passports to OSDU work-product format.
- **[Cognite Data Fusion](https://docs.cognite.com/)** — CDF Asset + Event model. Hullproof exports passports to CDF.
- **[DNV Veracity](https://www.veracity.com/)** — DNV's industrial data exchange. Hullproof exports passports to Veracity format.

## Governance

See [`governance.md`](./governance.md). RFC-style PRs welcome. Schema changes go through a 7-day comment period before merge.

## License

[Apache License 2.0](./LICENSE). Schema content and examples included.

## Contact

- Issues and proposals: [GitHub issues](https://github.com/Hullproof/coating-passport-spec/issues)
- Commercial integration: `support@hullproof.com`
- Standards collaboration: `standards@hullproof.com`
