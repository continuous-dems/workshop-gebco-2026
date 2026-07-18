---
title: "6 - Workflow Modules and Custom Region DEM"
---

# CUDEM Workshop Day 2

## Workflow Modules and Custom Region DEM

Day 2 builds on the Fiji test DEM and YAML walkthrough from Day 1. Now that participants have seen how a complete DEM recipe is structured, the goal is to step through the main workflow tools more directly and then run a custom region using `globato`.

By the end of Day 2, participants should understand how `fetchez`, `transformez`, `globato`, and IVERT fit together in a reproducible coastal DEM workflow. Participants should be able to discover source data, understand vertical transformation needs, run or modify a recipe, inspect generated DEM outputs, and consider basic validation and quality-control questions.

## Learning Goals

By the end of this section, participants should be able to:

- Reactivate the `cudem` environment
- Confirm that the main CUDEM tools are still working
- Understand the role of `fetchez` in data discovery and source-data access
- Understand the role of `transformez` in vertical harmonization
- Understand how `globato` connects the CUDEM workflow
- Run or inspect a custom DEM recipe with `globato`
- Review DEM outputs and identify basic quality-control issues
- Understand how a Fiji-style workflow can be adapted to another coastal region

---

## Reactivate the CUDEM Environment

Open a Miniforge Prompt or terminal and reactivate the workshop environment:

```bash
conda activate cudem
```

Confirm that the main tools are available:

```bash
fetchez --help
transformez --help
globato --help
ivert --help
```

If these commands return help text, the environment is ready for the Day 2 exercises.

---

# Module Overview: Data Discovery with `fetchez`

`fetchez` is the CUDEM data discovery and access tool. It helps users query, filter, cache, stream, and download source datasets through a standardized command-line interface.

In a coastal DEM workflow, `fetchez` helps answer questions such as:

- What source data are available in this region?
- Which datasets overlap the area of interest?
- Can the results be filtered by source type, year, provider, or data class?
- Can only the required data be downloaded?
- How should a source and its processing settings be represented in YAML?

Run the basic help command:

```bash
fetchez --help
```

List available source modules:

```bash
fetchez modules list
```

Inspect the module commands:

```bash
fetchez modules --help
```

Search available modules by keyword:

```bash
fetchez modules search topography
fetchez modules search bathymetry
```

Inspect a specific source module:

```bash
fetchez modules info mbdb
```

The main concept is that data discovery should be simple and programmatically accessible. Instead of manually downloading files from many different websites, `fetchez` provides a common interface for accessing more than 75 source types as part of a larger workflow or as a standalone data-acquisition tool.

The `fetchez` exercise also introduces the YAML structure used to represent source modules, module arguments, hooks, weights, and cache settings. These same components later appear inside the workflows coordinated by `globato`.

> Continue to the **Data Discovery with `fetchez`** exercise.

---

# Module Overview: Vertical Harmonization with `transformez`

`transformez` supports vertical-reference transformation workflows. In coastal DEM development, vertical consistency is critical because topographic and bathymetric datasets often use different vertical references.

A coastal DEM may combine data referenced to:

- An ellipsoid
- An EGM geoid model
- NAVD88
- Mean Lower Low Water
- Mean High Water
- Local tidal datums
- Chart datums

`fetchez` can prepare source data through hooks such as cropping, filtering, datatype assignment, and horizontal reprojection. `transformez` focuses on the vertical component by helping relate source elevations and depths to a common vertical reference.

Run the basic help command:

```bash
transformez --help
```

List available transformation resources:

```bash
transformez list
```

Inspect vertical shift-grid generation:

```bash
transformez grid --help
```

Inspect raster transformation options:

```bash
transformez raster --help
```

A general vertical shift-grid pattern is:

```bash
transformez grid -R WEST/EAST/SOUTH/NORTH -E RESOLUTION -I INPUT_DATUM -O OUTPUT_DATUM -o shift_grid.tif
```

A general raster transformation pattern is:

```bash
transformez raster INPUT_DEM.tif -I INPUT_DATUM -O OUTPUT_DATUM -o OUTPUT_DEM.tif
```

The main concept is that datasets cannot be compiled reliably into one coastal DEM until their elevation and depth values are referenced consistently.

> Continue to the **Vertical Harmonization with `transformez`** exercise.

---

# Module Overview: DEM Workflows with `globato`

`globato` connects the CUDEM workflow. It combines `fetchez` for data discovery and access, `transformez` for vertical datum and reference-frame transformation, and CUDEM tools for source filtering, stacking, interpolation, and output generation.

The result is a reproducible, recipe-based workflow that transforms diverse source datasets into a commonly referenced, composite DEM.

Run the basic help command:

```bash
globato --help
```

Inspect the CUDEM workflow commands:

```bash
globato cudem --help
```

Inspect the build command:

```bash
globato cudem build --help
```

Inspect the recipe-run command:

```bash
globato cudem run --help
```

The two main command patterns are:

```text
globato cudem build    Generate a workflow from command-line options
globato cudem run      Execute a saved YAML recipe
```

`globato` can also use module bundles that collect multiple source modules and their default settings under a single name.

List available bundles:

```bash
globato modules bundles list
```

Inspect the Fiji source bundle:

```bash
globato modules bundles dump global-bathy-topo
```

The main concept is that `globato` turns the DEM-building process into a workflow that can be exported, inspected, modified, shared, rerun, and version-controlled.

> Continue to the **DEM Workflows with `globato`** exercise.

---

# Custom Region DEM Exercise

After reviewing the main CUDEM modules, choose a small coastal region and apply the same build pattern used for the Fiji example.

General command:

```bash
globato cudem build -R WEST/EAST/SOUTH/NORTH -E 3s -O custom_test -P epsg:4326+3855 -X 0:5 global-bathy-topo --export
```

Replace:

```text
WEST/EAST/SOUTH/NORTH
```

with the selected coastal bounding box.

The `--export` option writes the generated YAML workflow to a recipe file instead of running the full DEM build.

Expected recipe output:

```text
custom_test_recipe.yaml
```

Run the exported recipe:

```bash
globato cudem run custom_test_recipe.yaml
```

Expected DEM output:

```text
custom_test_final.tif
```

Create a shaded relief image:

```bash
globato perspecto hillshade custom_test_final.tif custom_test_final_hs.tif
```

---

# Basic Output Review

Review the generated DEM and consider the following questions:

- Does the output cover the expected region?
- Are land and water in the correct locations?
- Are land elevations generally positive?
- Are underwater elevations generally negative?
- Are there obvious gaps in the source coverage?
- Are there edge artifacts?
- Are there unexpected interpolation patterns?
- Do neighboring source datasets appear consistent?
- Are there areas that should be checked against local or independent data?

This is not a complete validation procedure. The purpose is to begin thinking critically about the relationship between source data, workflow settings, and DEM quality.

---

# Wrap-Up and Next Steps

By the end of Day 2, participants should have:

- Reactivated the `cudem` environment
- Reviewed the Fiji test DEM
- Seen how `fetchez`, `transformez`, `globato`, and IVERT support the CUDEM workflow
- Explored source modules and YAML formatting
- Reviewed the role of vertical harmonization
- Generated or inspected a custom DEM recipe
- Reviewed DEM outputs for basic quality-control issues
- Understood how the Fiji recipe can be adapted to other coastal regions

## Suggested Next Steps

- Explore additional source modules and module bundles
- Inspect additional example recipes
- Try a new coastal region
- Compare different output resolutions
- Add local or project-specific source data
- Begin adapting the workflow to your own project area
- Use IVERT or other independent data to assess the resulting DEM
- Continue using Zulip for questions, support, and sharing workflow ideas

[Continuous DEMs Zulip](https://cudem.zulipchat.com)
