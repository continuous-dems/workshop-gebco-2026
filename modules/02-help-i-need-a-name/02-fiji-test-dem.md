# Run the Fiji Test DEM

Once the `cudem` environment is active and the core tools are available, we will run a small Fiji/Suva-area test DEM.

This test region gives everyone a concrete output early in the workshop. We will use the same output later to inspect DEM files, review source-data behavior, and deconstruct the YAML workflow.

## Fiji Test Region

The Fiji test region is:

```text
178.25/178.65/-18.30/-17.95
```

The bounding-box format is:

```text
WEST/EAST/SOUTH/NORTH
```

For this example:

```text
west longitude   178.25
east longitude   178.65
south latitude   -18.30
north latitude   -17.95
```

## Run the Test DEM

Make sure the `cudem` environment is active:

```bash
conda activate cudem
```

Then run:

```bash
globato cudem build -R 178.25/178.65/-18.30/-17.95 -E 3s -O fiji_test -P epsg:4326+3855 -M ms_binary_cudem:algos=interp_gmt -X 0:5 global-bathy-topo
```

This command builds a small Fiji coastal DEM using the `global-bathy-topo` source bundle.

## Command Breakdown

| Command option | Purpose |
|---|---|
| `globato cudem build` | Build and run a DEM workflow |
| `-R 178.25/178.65/-18.30/-17.95` | Set the Fiji test region |
| `-E 3s` | Set the output resolution to 3 arc-seconds |
| `-O fiji_test` | Set the output basename |
| `-P epsg:4326+3855` | Set the target horizontal and vertical reference |
| `-M ms_binary_cudem:algos=interp_gmt` | Set the DEM compilation and interpolation method |
| `-X 0:5` | Extend the processing region before gridding |
| `global-bathy-topo` | Use the global bathymetry-topography source bundle |

The `-X 0:5` option extends the processing region by 5 percent before gridding. This adds a small buffer around the requested output area and can help reduce edge effects near the final raster boundary.

## Expected Output

Expected output DEM:

```text
fiji_test_final.tif
```

Additional intermediate products may also be created, including:

```text
source masks
provenance rasters
stack files
logs
temporary processing outputs
```

## Runtime Note

DEM outputs may vary slightly across systems because of differences in package versions and interpolation libraries.

For this workshop, the goal is to understand the workflow structure and confirm that each participant can run or inspect the Fiji example. Reference outputs will be available if a local run fails or produces a different-looking result.

---

# Check the Fiji Test DEM Outputs

After the Fiji test DEM finishes, we will confirm that the workflow completed and inspect the main outputs.

The goals of this section are to:

- Confirm that the DEM was generated
- Identify missing or failed outputs
- Troubleshoot data-access or credential issues
- Check basic raster metadata
- Perform a quick visual review before opening the YAML recipe

## Expected Output Files

Expected output DEM:

```text
fiji_test_final.tif
```

Expected hillshade:

```text
fiji_test_final_hs.tif
```

## Check the DEM Metadata

Run:

```bash
gdalinfo fiji_test_final.tif
```

Useful metadata to review include:

```text
Driver
Raster size
Coordinate system
Origin
Pixel size
Corner coordinates
Band information
NoData value
Minimum and maximum values
```

## Create a Hillshade

Create a quick visual inspection product with:

```bash
globato perspecto hillshade fiji_test_final.tif fiji_test_final_hs.tif
```

This creates a georeferenced hillshade that can be reviewed without opening an external GIS application.

## Basic Visual Checks

Ask the following questions:

```text
Does the DEM cover the expected Fiji region?
Do land and water appear in the correct locations?
Are elevations generally positive on land?
Are elevations generally negative underwater?
Are there obvious missing-data areas?
Are there visible artifacts near the coastline?
Are there unusual edge effects?
Does the result broadly resemble the instructor example?
```

This is not a full validation exercise. The goal is to confirm that the workflow ran and produced a reasonable output.

## Troubleshooting Questions

If the output is missing or incomplete, check:

```text
Was the cudem environment active?
Did the build command finish successfully?
Did a remote source request fail?
Were the Copernicus Marine credentials available?
Was the output written to another directory?
Did the command use the correct region and output name?
```

Once the output is confirmed, the next step is to inspect the YAML recipe that defines the workflow.
