---
title: "4 - Check Fiji DEM Outputs"
---

After running the Fiji test DEM, we will check the generated outputs. This step confirms that the environment is stable, that the test command completed successfully, and that participants can find and inspect their output files.

The main goals are to:

- Confirm who successfully ran the Fiji test DEM
- Identify failed runs or missing outputs
- Troubleshoot installation, credential, or data-access issues
- Make sure the `cudem` environment is stable before moving into the YAML walkthrough
- Begin thinking about basic DEM quality-control questions

---

## Expected Output Files

Expected output DEM:

```text
fiji_test_final.tif
```

Expected output shaded relief image:

```text
fiji_test_final_hs.tif
```

---

## Check the DEM Metadata

Run:

```bash
gdalinfo fiji_test_final.tif
```

Useful things to check in the `gdalinfo` output include:

```text
Driver
Size
Coordinate System
Origin
Pixel Size
Corner Coordinates
Band information
NoData Value
```

This confirms that the output is a valid raster and gives basic information about the DEM extent, resolution, coordinate reference system, and data values.

---

## Create a Hillshade

Create a quick hillshade using `globato perspecto`:

```bash
globato perspecto hillshade fiji_test_final.tif fiji_test_final_hs.tif
```

This creates a georeferenced hillshade that can be used for quick visual inspection.

---

## Basic Visual Checks

This is not a full validation step. The goal is to confirm that the workflow ran and produced a reasonable output.

Things to check:

```text
Does the DEM cover the expected Fiji test region?
Do land and water areas appear in the correct places?
Are elevations generally positive on land?
Are underwater elevations generally negative?
Are there obvious missing-data areas?
Does the output look broadly similar to the instructor example?
Are there obvious artifacts near the coastline or raster edges?
```

If the final DEM looks wrong, do not assume the entire workflow failed. Later sections will show how to inspect intermediate products such as source masks, provenance files, and stacked source rasters.

---

## Troubleshooting Questions

If the output is missing or incomplete, check:

```text
Was the cudem environment active?
Did the command complete or exit with an error?
Did the workflow fail while accessing a remote data source?
Did Copernicus Marine credentials work?
Was the output written to a different folder?
Did the command use the expected region and output name?
```

The goal of this section is to get everyone to a stable point before opening and deconstructing the YAML recipe.
