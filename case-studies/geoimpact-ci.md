# GeoImpact CI — case study

**Know the spatial blast radius before you merge.**

GeoImpact CI is a deterministic geospatial regression tool I built to answer a specific pre-merge question:

> If a spatial dataset changes, which declared downstream spatial relationships change with it?

The current stable release is **v1.2.0** under the MIT License. It is deliberately narrow: a reproducible research-software demonstrator for spatial regression testing, not a universal GIS validation platform.

## The problem

Traditional geospatial validation can tell you whether a dataset is valid, complete enough, or structurally consistent. That is not the same as understanding the consequences of a change.

GeoImpact compares a **BASE** and **CANDIDATE** polygon dataset using stable IDs, classifies primary features as unchanged, modified, added or removed, measures deterministic change-footprint and boundary-displacement evidence, and evaluates fixed dependent Point features against the before/after states.

The core distinction is:

- validation asks whether a dataset is acceptable;
- GeoImpact asks what a dataset **change** could affect.

## Current v1.2.0 contract

GeoImpact CI v1.2.0 supports:

- Polygon / MultiPolygon BASE and CANDIDATE GeoJSON;
- required stable feature IDs;
- first-class `unchanged`, `modified`, `added` and `removed` primary states;
- EPSG:25830 analysis;
- deterministic geometry-change evidence;
- fixed dependent Point features;
- exact `WITHIN` relationships;
- `TOUCHES` reported separately as boundary ambiguity;
- STRtree candidate filtering followed by exact GEOS predicates;
- PASS/BLOCK policy outcomes and operational ERROR separation;
- JSON, Markdown and GeoJSON evidence artifacts;
- CLI and GitHub Actions use;
- report contract **V5**.

V5 adds deterministic provenance for the exact raw bytes of the config, BASE, CANDIDATE and dependency inputs using SHA-256 plus byte size. It also records the qualified GeoImpact CI, Shapely, GEOS, PyProj and PROJ versions while excluding local paths, usernames, timestamps, hostnames and working directories.

The release intentionally does not claim arbitrary CRS or predicate support, raster/network analysis, PostGIS integration, web UI/API coverage, or automatic legal, causal or administrative interpretation.

## Release evolution

The project was developed in intentionally bounded steps rather than by continuously expanding scope:

- **v1.0.0** established the first stable spatial-regression contract, deterministic artifacts, positive/negative research cases and cross-platform qualification.
- **v1.1.0** hardened correctness by making added and removed primary IDs first-class, completing change-footprint semantics and tightening fail-closed input handling.
- **v1.2.0** advances the report contract to V5 with deterministic input and engine provenance while preserving the underlying relationship evidence semantics.

This versioning matters because report-contract changes are explicit rather than silently changing the meaning of prior evidence.

## Evidence ladder

### 1. Synthetic contract

The controlled fixture establishes product behavior, deterministic serialization and canonical artifact identities. PASS, BLOCK and operational ERROR remain separate CLI outcomes.

For v1.2.0, the same synthetic relationship GeoJSON evidence remains byte-for-byte stable while provenance-bearing report JSON/Markdown identities change intentionally because the engine version is part of Report V5.

### 2. Madrid positive benchmark

A bounded real-world benchmark compares official INE census-section geometry changes against a fixed Ayuntamiento de Madrid portal inventory.

Observed in the frozen benchmark:

- **2,112 relationship changes**;
- **26 old/new transition pairs**;
- **1,768 split-like patterns**;
- **344 merge-like patterns**;
- **0 gained assignments**;
- **0 lost assignments**;
- **0 boundary ambiguities**.

V4/V5 primary-change evidence also records **19 added**, **7 removed** and **30 modified** primary IDs.

These are geometry and stable-ID transition patterns. The project does not infer administrative intent, resident movement or legal effect from them.

### 3. Sierra de Baza negative control

An independently selected protected-area case compares two official Sierra de Baza boundary states against a fixed REDIAM public-use equipment inventory.

Observed:

- **52 relationships**;
- **52 unchanged**;
- **0 regressions**;
- **0 boundary ambiguities**;
- maximum boundary displacement: **368.82509547712834 m**.

This case matters because the geometry changes, but the declared dependent relationships do not. GeoImpact therefore does not manufacture an impact simply because spatial geometry changed.

## Reproducibility

The v1.2.0 release is qualified on:

- Linux / Python 3.11;
- Windows / Python 3.14;
- **113 tests** in each hosted environment;
- PASS / BLOCK / ERROR CLI contracts;
- installed wheel and source-distribution qualification;
- the same synthetic, Madrid and Sierra scientific relationship evidence.

Report V5 adds a provenance layer around that evidence. Hashes identify the exact input bytes used for a run and the qualified spatial-engine versions. They do **not** authenticate or embed the original files, and inputs are expected to remain stable during one analysis run.

The numerical contract explicitly separates computational reproducibility from source accuracy. Derived change footprints use a `1e-6 m` precision model, and maximum boundary displacement is measured on temporary geometry copies using its own `1e-6 m` measurement grid.

## Performance qualification

The relationship engine originally scanned every dependent feature against every primary polygon.

Gate 7 replaced that exhaustive candidate scan with STRtree filtering while preserving exact `within` and `touches` predicates as the authoritative evidence.

On the controlled large synthetic benchmark:

- 300 primary polygons;
- 1,500 dependent points;
- naive: **15.420192 s**;
- indexed: **0.134966 s**;
- observed speedup: **114.25x**;
- candidate reduction: **99.667%**.

This is a benchmark-specific result, not a general runtime guarantee.

## What I wanted to demonstrate

The value of this project is not only the geometry code. I wanted to demonstrate a complete evidence-oriented software workflow:

- define and freeze a narrow product contract;
- separate policy outcomes from operational failures;
- model spatial additions/removals explicitly instead of hiding them;
- version the report contract when semantics or provenance change;
- freeze and hash evidence;
- test cross-platform determinism;
- use positive and negative real-world cases;
- distinguish source-data terms from software licensing;
- package and install the actual release artifact;
- stop adding features when there is no external need for them.

## My role

Independent design and implementation across:

**Python · Shapely · PyProj · GeoJSON · STRtree · deterministic serialization · pytest · GitHub Actions · research provenance · release engineering**

## Status

**GeoImpact CI v1.2.0 — released**

- [Repository](https://github.com/soroushkarahrodi79-oss/geoimpact-ci)
- [v1.2.0 release](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/releases/tag/v1.2.0)
- [README at v1.2.0](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.2.0/README.md)
- [Report V5 migration record](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.2.0/docs/REPORT_V5_MIGRATION.md)
- [Madrid benchmark record](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.2.0/docs/GATE_6_REAL_WORLD_BENCHMARK.md)
- [Sierra negative-control record](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.2.0/docs/GATE_8_REAL_WORLD_GENERALIZATION.md)
