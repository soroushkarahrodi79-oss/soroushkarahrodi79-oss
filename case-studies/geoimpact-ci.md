# GeoImpact CI — case study

**Know the spatial blast radius before you merge.**

GeoImpact CI is a deterministic geospatial regression tool I built to answer a specific pre-merge question:

> If a spatial dataset changes, which declared downstream spatial relationships change with it?

The project is released as **v1.0.0** under the MIT License and is deliberately narrow: it is a reproducible research-software demonstrator, not a universal GIS validation platform.

## The problem

Traditional geospatial validation can tell you whether a dataset is valid, complete enough, or structurally consistent. That is not the same as understanding the consequences of a change.

GeoImpact compares a **BASE** and **CANDIDATE** polygon dataset using stable IDs, identifies changed geometry, measures a deterministic spatial footprint and boundary displacement, and then evaluates fixed dependent point features against the before/after states.

The core distinction is:

- validation asks whether a dataset is acceptable;
- GeoImpact asks what a dataset **change** could affect.

## v1 contract

GeoImpact CI v1.0.0 supports:

- polygon BASE/CANDIDATE GeoJSON;
- stable feature IDs;
- EPSG:25830 analysis;
- deterministic geometry-change evidence;
- dependent point features;
- exact `WITHIN` relationships;
- `TOUCHES` reported separately as boundary ambiguity;
- STRtree candidate filtering followed by exact GEOS predicates;
- PASS/BLOCK policy outcomes;
- JSON, Markdown and GeoJSON evidence artifacts;
- CLI and GitHub Actions use;
- report contract V3.

The release intentionally does not claim arbitrary CRS or predicate support, raster/network analysis, PostGIS integration, web UI/API coverage, or automatic legal or causal interpretation.

## Evidence ladder

### 1. Synthetic contract

The controlled fixture establishes the product behavior and canonical artifact hashes. PASS, BLOCK and operational ERROR are separate CLI outcomes.

### 2. Madrid positive benchmark

A bounded real-world benchmark compares official INE census-section geometry changes against a fixed Ayuntamiento de Madrid portal inventory.

Observed in the frozen benchmark:

- **2,112 assignment changes**;
- **26 old/new transition pairs**;
- **1,768 split-like patterns**;
- **344 merge-like patterns**;
- **0 gained assignments**;
- **0 lost assignments**;
- **0 boundary ambiguities**.

These are geometry/ID transition patterns. The project does not infer administrative intent, resident movement or legal effect from them.

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

The v1 release is qualified on:

- Linux / Python 3.11;
- Windows / Python 3.14;
- Shapely 2.1.2 / GEOS 3.13.1;
- pinned PyProj and PyYAML dependencies.

The same canonical synthetic, Madrid and Sierra evidence contracts are checked in CI on both operating systems.

The numerical contract explicitly separates computational reproducibility from source accuracy. Derived change footprints use a 1e-6 metre precision model, and maximum boundary displacement is measured on temporary geometry copies using its own 1e-6 metre measurement grid.

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

- define a narrow product contract;
- separate policy outcomes from operational failures;
- freeze and hash evidence;
- test cross-platform determinism;
- use positive and negative real-world cases;
- distinguish source-data terms from software licensing;
- package and install the actual release artifact;
- stop adding features once the v1 contract is complete.

## My role

Independent design and implementation across:

**Python · Shapely · PyProj · GeoJSON · STRtree · deterministic serialization · pytest · GitHub Actions · research provenance · release engineering**

## Status

**GeoImpact CI v1.0.0 — released**

- [Repository](https://github.com/soroushkarahrodi79-oss/geoimpact-ci)
- [v1.0.0 release](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/releases/tag/v1.0.0)
- [README](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.0.0/README.md)
- [Madrid benchmark record](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.0.0/docs/GATE_6_REAL_WORLD_BENCHMARK.md)
- [Sierra negative-control record](https://github.com/soroushkarahrodi79-oss/geoimpact-ci/blob/v1.0.0/docs/GATE_8_REAL_WORLD_GENERALIZATION.md)
