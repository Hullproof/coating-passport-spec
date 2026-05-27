# Governance

How changes to the CoatingPassport schema are proposed, reviewed, and accepted.

## Principles

1. **Backward compatibility first.** Additive changes (new optional fields, new enum values) ship freely. Breaking changes require a major version bump and a deprecation path.
2. **Open process.** Schema changes are public from the moment they are proposed. No private «we already decided» merges.
3. **Audit-grade trust.** This schema underpins tribunal-grade evidence (charter-party disputes, P&I claims, classification surveys). Backward compatibility and traceability are not negotiable.
4. **Hullproof retains canonical authority.** This is the open spec for the data product Hullproof produces. Hullproof is the eventual decision-maker on the canonical schema.

## How to propose a change

Open a pull request against this repository with:

- A clear description of the change and the motivation.
- The full schema diff (regenerated via `npm run export:passport-schema` in the [Hullproof dashboard repo](https://github.com/jasonhawk-create/hullproof-dashboard)).
- At least one updated or new example passport in `examples/` that exercises the change.
- A `CHANGELOG.md` entry under `[Unreleased]`.
- If the change is breaking, a migration note covering how existing passports remain valid or are translated.

## Review process

| Step | Owner | Timeline |
|---|---|---|
| PR opened | Anyone | — |
| Initial triage (in scope? backward compatible?) | Hullproof maintainer | Within 7 days |
| Open comment period | Public | 7 days minimum for non-trivial changes |
| Maintainer review and merge or rejection | Hullproof maintainer | After comment period |
| Schema-version bump and release tag | Hullproof maintainer | On merge of breaking change |

«Non-trivial» means: any change to required fields, any new enum value, any change to the `compliance` or `performance` blocks, any change that breaks downstream exporters (CDF, OSDU, Veracity).

«Trivial» means: typo fixes in descriptions, documentation-only updates, governance-doc edits.

## Versioning

Follows Semantic Versioning:

- **MAJOR** version bump for incompatible schema changes.
- **MINOR** version bump for backward-compatible additions.
- **PATCH** version bump for backward-compatible fixes (typos, description clarifications).

The `schema_version` field embedded in every CoatingPassport currently uses MAJOR.MINOR only (e.g. `1.0`). Patch-level changes do not affect the embedded field.

## Out of scope

- Changes that would require Hullproof to break customer integrations without a migration path — declined unless the technical or legal benefit is overwhelming.
- Proposals that fork the schema into a competing standard — Hullproof retains canonical authority; please contribute upstream instead.
- Vendor-specific extensions — use `modality_metadata` or `interoperability.external_system_ids` for vendor data rather than expanding the canonical schema.

## Contact

- Issues: [GitHub issues](https://github.com/Hullproof/coating-passport-spec/issues)
- Maintainer: Jarle Haugereid, founder, Hullproof
- Standards-collaboration channel: `standards@hullproof.com`
