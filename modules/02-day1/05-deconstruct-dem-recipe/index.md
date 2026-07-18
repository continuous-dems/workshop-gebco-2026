---
title: "5 - Deconstruct the Fiji YAML Recipe"
---

After checking the Fiji test DEM outputs, we will open the YAML recipe that defines the DEM workflow and break it down section by section.

The goal is to understand how CUDEM and `globato` use a single recipe file to organize the full DEM-building process.

The Fiji recipe is not just a list of settings. It defines the project area, the target reference frame, the source datasets, source-level processing hooks, workflow-wide processing steps, gridding/interpolation settings, and final outputs.

By walking through this file, participants will see how a reproducible coastal DEM workflow is built from many different source datasets.

---

## What Is a YAML Recipe?

A YAML recipe is a human-readable workflow file. It tells CUDEM what region to work in, what data sources to use, how to prepare each source, and how to combine everything into a final DEM.

To export the Fiji workflow as a YAML recipe, run:

```bash
globato cudem build -R 178.25/178.65/-18.30/-17.95 -E 3s -O fiji_test -P epsg:4326+3855 -M ms_binary_cudem:algos=interp_gmt -X 0:5 global-bathy-topo --export
```

The `--export` option writes the generated YAML recipe to disk instead of running the full DEM build.

Expected recipe output:

```text
fiji_test_recipe.yaml
```

---

## Main Parts of the Fiji Recipe

The Fiji YAML can be understood in five major sections:

```text
project
region
modules
global_hooks
execution
```

Each section has a specific role in the workflow.

---

## Project Name

```yaml
project:
  name: fiji_test
```

The project name defines the workflow name. It is also used to organize output names and related files.

For this example, the project is named:

```text
fiji_test
```

This keeps the outputs tied to the Fiji test region.

---

## Region and Reference Frame

```yaml
region: 178.25/178.65/-18.3/-17.95
region_srs: epsg:4326+3855
```

The `region` defines the bounding box for the DEM in geographic decimal degrees:

```text
west/east/south/north
```

For this example:

```text
178.25/178.65/-18.3/-17.95
```

The `region_srs` defines the target horizontal and vertical reference frame:

```text
epsg:4326+3855
```

This tells the workflow that the target output uses WGS84 geographic coordinates with an EGM08 vertical reference.

---

# Source Modules: The DEM Ingredients

The `modules` section defines the input data sources used to build the DEM. Each module represents a different source or class of data.

The Fiji recipe uses several source modules:

```text
copernicus
copernicus_marine
mbdb
sysu
margrav
csb
```

These are the source-data ingredients of the DEM recipe.

---

## Copernicus Topography

The recipe includes Copernicus topographic data:

```yaml
- module: copernicus
  args:
    weight: 1.5
    datatype: 3
```

It also includes another Copernicus entry:

```yaml
- module: copernicus
  args:
    weight: 0.75
    datatype: 1
```

These modules provide land/topographic elevation data. The recipe assigns different weights to different Copernicus data types.

A higher weight means that source has more influence when datasets overlap.

The topographic sources use a `range_z` hook:

```yaml
- name: range_z
  args:
    min_z: 0.01
```

This keeps positive elevation values and helps separate land/topography from bathymetry.

---

## Copernicus Marine Bathymetry

The Fiji recipe also uses Copernicus Marine data:

```yaml
- module: copernicus_marine
  args:
    weight: 1.2
```

This source provides satellite-derived bathymetric data.

Important hooks include:

```yaml
- name: set-srs
  args:
    srs: EPSG:4326+9003

- name: dynamic-weight
  args:
    source_field: confidence
    method: inverse
    offset: 1.2

- name: range_z
  args:
    max_z: -0.01
```

The `set-srs` hook tells the workflow what reference frame the source data are in. The `dynamic-weight` hook adjusts the source weight using a confidence field. The `range_z` hook keeps negative elevation values, so this source contributes to the bathymetric part of the DEM.

This is an important concept: the recipe can treat sources differently based on their type, confidence, elevation range, and reference frame.

---

## MBDB Multibeam Data

The recipe includes NCEI multibeam data through the `mbdb` module:

```yaml
- module: mbdb
  args:
    weight: 1
    want_inf: true
```

Multibeam data can be valuable because it is often high-resolution and directly measured. However, it can also include noise, outliers, or survey artifacts.

The Fiji recipe filters multibeam files and applies an `rq` filtering hook:

```yaml
- name: filename_filter
  args:
    match: ".fbt"

- name: rq
  args:
    reference: gmrt/gebco
    threshold: 10
    mode: percent
    builder: grid
    res: 0.000277777
```

The `rq` hook compares the input data against a reference surface, such as GMRT or GEBCO, and filters values that differ beyond the specified threshold. This improves source quality before gridding.

---

## Supporting Bathymetric Sources

The Fiji recipe also uses lower-weight supporting bathymetric sources:

```yaml
- module: sysu
  args:
    weight: 0.1

- module: margrav
  args:
    weight: 0.1

- module: csb
  args:
    weight: 0.25
```

These sources help fill areas where higher-confidence data are sparse or unavailable. They are assigned lower weights so they can support the surface without dominating higher-priority sources.

The general strategy is:

```text
Use the best available data where it exists, but allow supporting sources to fill gaps.
```

---

# Per-Source Hooks

Each source module can include hooks that prepare the data before it is used in the DEM.

Common hooks in the Fiji recipe include:

```text
stream_reproject
set-srs
set-datatype
range_z
spatial_crop
rq
```

These hooks do the practical work of normalizing and filtering the data.

| Hook | Purpose |
|---|---|
| `stream_reproject` | Reprojects source data into the target reference frame |
| `set-srs` | Assigns the source horizontal/vertical reference |
| `set-datatype` | Tells the workflow how to interpret the source |
| `range_z` | Keeps only values within a specified elevation/depth range |
| `spatial_crop` | Crops data to the target region |
| `rq` | Filters values against a reference surface |

Different datasets need different preparation steps before they can be combined into one DEM.

---

# Global Hooks

The `global_hooks` section applies workflow-wide processing steps. These steps operate across the full collection of sources and generate intermediate and final outputs.

Important global hooks in the Fiji recipe include:

```text
audit
enrich
transfer_log
provenance
source_masks
multi_stack
ms_binary_cudem
format_cog
```

These hooks help make the workflow reproducible and inspectable.

---

## Provenance and Source Masks

The Fiji recipe creates provenance and source mask outputs:

```yaml
- name: provenance
  args:
    res: 3s
    output: fiji_test_provenance.tif

- name: source_masks
  args:
    res: 1s
    output: fiji_test_sources.vrt
```

These outputs help answer important QA questions:

```text
Which sources contributed to this DEM?
Where did each source contribute?
Which areas were filled by lower-priority data?
Where might the DEM need closer inspection?
```

---

## Stacked Source Output

The Fiji recipe also creates a stacked source raster before the final interpolation step:

```yaml
- name: multi_stack
  args:
    res: 3s
    crs: epsg:4326+3855
    mode: mixed
    nodata: -9999
    weight_threshold:
      interpolate: 1.0
      fill: 0.5
    output: fiji_test_stack.tif
```

The stack file is an important intermediate product. It represents the source data after they have been discovered, transformed, filtered, weighted, and combined onto a common grid.

In this workflow, the stack is written to:

```text
fiji_test_stack.tif
```

This file shows what the workflow has assembled before the final DEM surface is generated.

The stacked output helps answer questions such as:

```text
What data were available before interpolation?
Where do source datasets overlap?
Where are there gaps in the source coverage?
Which areas are strongly constrained by data?
Which areas will require more interpolation or filling?
Are there obvious source conflicts before the final DEM is created?
```

The stack file is not necessarily the final DEM. It is an intermediate compilation of the available source data. The final interpolation step uses this stacked surface, along with the workflow settings, to produce the final DEM output.

For the Fiji workflow, the relationship is:

```text
source modules
→ hooks and filters
→ weighted source stack
→ interpolation / DEM compilation
→ final DEM
```

The stack file is one of the most useful outputs for troubleshooting. If the final DEM looks wrong, inspect the stack first. It can help determine whether the issue comes from the input data, the source weighting, the source coverage, or the final interpolation step.

---

# Main Takeaway

The YAML recipe is the reproducible record of the DEM workflow.

It shows:

```text
where the workflow runs
which sources are used
how each source is prepared
how sources are weighted
how intermediate products are created
how the final DEM is generated
```

This is the core idea behind CUDEM workflows: the DEM is not just an output raster. It is the result of an inspectable, repeatable recipe.
