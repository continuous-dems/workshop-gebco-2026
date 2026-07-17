# CUDEM/GEBCO-Nippon Scholars Workshop 2026

All workshop materials are available on this website.

**What:** Generating Coastal DEMs: From Data Discovery to Validation   
**When:** Monday, July 20, 2026 and Tuesday, July 21, 2026 from 8:30 AM - 3:00 PM
**Where:** NOAA/NCEI Boulder, CO (Virtual information needed to join will be shared by email)

## Workshop abstract

This workshop introduces CUDEM workflows for building reproducible coastal digital elevation models from diverse topographic and bathymetric source datasets. Participants will learn how CUDEM tools can be used to discover source data, prepare and harmonize datasets, build recipe-based workflows, generate composite coastal DEMs, and inspect outputs.

The workshop focuses on the CUDEM toolchain, including `fetchez` for data discovery and source-data access, `transformez` for vertical datum harmonization, and `globato` for connecting source modules, hooks, transformations, stacking, interpolation, and output generation into reproducible YAML-based workflows.

A Fiji/Suva-area test region will be used as the primary example. Participants will run example commands, inspect generated YAML recipes, review DEM outputs, and consider how the workflow can be adapted to other coastal regions.

## Workshop goals

By the end of the workshop, participants should be able to:

- Set up and verify a working CUDEM software environment
- Use `fetchez` to discover and access source datasets
- Understand how CUDEM source modules, hooks, and YAML recipe blocks are structured
- Use `transformez` to understand vertical datum harmonization
- Use `globato` to generate, export, run, and modify CUDEM recipes
- Inspect DEM outputs using command-line tools
- Adapt a workflow to a new coastal region

## Workshop structure

### Day 1: Onboarding and Fiji test DEM

Day 1 focuses on software setup, environment checks, and running the Fiji/Suva-area test workflow. We will review the generated YAML recipe and discuss how source modules, hooks, source weights, global hooks, stacking, and interpolation are represented in a CUDEM recipe.

### Day 2: Workflow modules and custom region exercise

Day 2 focuses on the individual workflow components. We will review `fetchez`, `transformez`, and `globato`, then apply the same workflow concepts to a custom coastal region. The day will also include output inspection, data filtering, and discussion of workflow adaptation.

## Main workflow sequence

```text
fetchez      → discover, access, describe, and prepare source data
transformez  → harmonize vertical references
globato      → synthesize sources, hooks, transformations, stacking, interpolation, and outputs
