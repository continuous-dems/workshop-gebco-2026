---
title: "8 - Vertical Transformation with transformez"
---

`transformez` supports vertical datum and reference-frame transformation workflows. In coastal DEM development, this is important because topographic and bathymetric datasets often come from different sources and may use different vertical references.

A coastal DEM may combine data referenced to:

- An ellipsoid
- An EGM geoid model
- NAVD88
- Mean Lower Low Water
- Mean High Water
- Local tidal datums
- Chart datums

`transformez` can generate vertical shift grids or apply vertical datum transformations to existing raster datasets. This allows source data to be moved into a common vertical reference before they are used in a DEM workflow.

During this workshop, the main goal is to understand where `transformez` fits into the CUDEM workflow. We will not cover every vertical datum case in detail.

---

## Basic Help Command

Run:

```bash
transformez --help
```

The main command groups are:

| Command | Purpose |
|---|---|
| `grid` | Generate a standalone vertical shift grid |
| `raster` | Apply a vertical datum shift to an existing DEM |
| `list` | List supported vertical datums and EPSG options |
| `htdp` | Manage and run NGS HTDP |
| `vdatum` | Manage and run NOAA VDatum |

---

## List Supported Datums

Use `list` to see supported vertical datums and EPSG options:

```bash
transformez list
```

This is useful when you need to confirm the correct input or output datum name before running a transformation.

---

## Generate a Vertical Shift Grid

Use `transformez grid` when you want to create a standalone vertical shift grid for a region.

Inspect the available options:

```bash
transformez grid --help
```

General command pattern:

```bash
transformez grid -R WEST/EAST/SOUTH/NORTH -E RESOLUTION -I INPUT_DATUM -O OUTPUT_DATUM -o shift_grid.tif
```

Example:

```bash
transformez grid -R 178.25/178.65/-18.30/-17.95 -E 3s -I mllw -O 3855 -o fiji_shift_grid.tif
```

### Command Breakdown

| Command option | Purpose |
|---|---|
| `transformez grid` | Create a vertical shift grid |
| `-R WEST/EAST/SOUTH/NORTH` | Set the target region |
| `-E 3s` | Set the output resolution |
| `-I INPUT_DATUM` | Set the source or input vertical datum |
| `-O OUTPUT_DATUM` | Set the target or output vertical datum |
| `-o shift_grid.tif` | Set the output shift-grid filename |

Preview the shift grid:

```bash
transformez grid -R 178.25/178.65/-18.30/-17.95 -E 3s -I mllw -O 3855 --preview
```

The shift grid represents the vertical correction required to move values from the input datum to the output datum across the selected region.

---

## Apply a Vertical Shift to a Raster

Use `transformez raster` when you already have a DEM or raster and want to apply a vertical datum shift to it.

Inspect the available options:

```bash
transformez raster --help
```

General command pattern:

```bash
transformez raster INPUT_DEM.tif -I INPUT_DATUM -O OUTPUT_DATUM -o output_dem.tif
```

Example:

```bash
transformez raster fiji_test.tif -I mllw -O 3855 -o fiji_test_transformed.tif
```

### Command Breakdown

| Command option | Purpose |
|---|---|
| `transformez raster` | Apply a vertical datum shift to an existing DEM |
| `INPUT_DEM.tif` | Set the input raster |
| `-I INPUT_DATUM` | Set the source or input vertical datum |
| `-O OUTPUT_DATUM` | Set the target or output vertical datum |
| `-o output_dem.tif` | Set the output transformed raster |

Check the transformed raster:

```bash
gdalinfo fiji_test_transformed.tif
```

Useful checks include:

- Coordinate reference system
- Pixel size
- Raster extent
- NoData value
- Minimum and maximum elevation values
- Whether the output file opens successfully

---

## How `transformez` Connects to the Fiji Recipe

In the Fiji workflow, reference information appears in the YAML recipe through settings such as:

```text
region_srs
set-srs
stream_reproject
```

These settings help the workflow understand:

- The reference frame assigned to each source dataset
- The target horizontal reference of the workflow
- The target vertical reference of the final DEM
- Which sources may require transformation before they can be combined

Source-level preparation, including horizontal reprojection, can be handled through `fetchez` hooks such as `stream_reproject`.

`transformez` focuses on the vertical relationship between the source and target references. It can create the vertical shift grids or transformed rasters needed to harmonize elevations and depths before DEM compilation.

The key idea is that vertical transformation should be part of the reproducible workflow rather than an undocumented manual step performed after the DEM is created.

---

## What to Look For

When using `transformez`, check:

- Did I define the correct input datum?
- Did I define the correct output datum?
- Does the selected region cover the source dataset?
- Is the output resolution appropriate?
- Did the shift grid or transformed raster write successfully?
- Can GDAL open the output?
- Are the sign and magnitude of the vertical shift reasonable?
- Does the selected vertical reference make sense for the intended DEM application?

A technically successful command does not necessarily mean the correct datums were selected. The input and output reference assumptions should always be verified.

---

## Main Takeaway

`transformez` makes vertical datum handling explicit and reproducible within CUDEM workflows.

This is critical because vertical-reference mistakes can introduce significant offsets when combining topography, bathymetry, tidal datums, chart datums, and other elevation sources into a composite DEM.

The basic workflow is:

```text
identify the source vertical datum
→ identify the target vertical datum
→ generate or apply the vertical shift
→ inspect the transformed output
→ use the harmonized data in the DEM workflow
```

## Documentation

[Transformez documentation](https://transformez.readthedocs.io/en/latest/index.html)
