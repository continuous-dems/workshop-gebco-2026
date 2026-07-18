# Schedule

## Day 1: Onboarding, Test DEM Generation, and Workflow Deconstruction

Day 1 focuses on getting everyone set up and comfortable with the basic CUDEM workflow. We will begin by framing coastal DEM development as a reproducible open-source process that connects data discovery, transformation, DEM generation, and validation.

The first major goal is environment setup. We will get everyone into a consistent `cudem` conda environment and run basic checks to confirm that the core tools are working.

After setup, we will run a small Fiji/Suva-area test DEM. This gives everyone a concrete output early in the workshop and provides a shared example that we can inspect, troubleshoot, and use throughout the rest of the course.

The afternoon will focus on checking the DEM outputs and deconstructing the YAML workflow used to generate them. We will walk through the source modules, processing hooks, stacking steps, gridding/interpolation settings, and vertical reference assumptions so participants can see how a single recipe organizes the full DEM-building process.

By the end of Day 1, participants should have a working environment, a generated test DEM, and a basic understanding of how CUDEM moves from source data and configuration files toward consistent DEM outputs.

### Day 1 Agenda

| Time | Topic | Description |
|---|---|---|
| 9:00–9:20 | Introductions and Workshop Overview | Introduce the workshop goal: building reproducible open-source coastal DEM workflows from data discovery through validation. Frame `fetchez`, `transformez`, `globato`, and `ivert` as parts of one CUDEM workflow. |
| 9:20–10:30 | Environment Setup | Set up a consistent `cudem` conda environment. Reproducibility depends on everyone working from the same software stack. |
| 10:30–11:00 | Installation Tests and Troubleshooting | Confirm that the main tools are installed and responding: `fetchez`, `transformez`, `globato`, and `ivert`. Troubleshoot installation issues before running the test DEM. |
| 11:00–11:30 | Run the Fiji Test DEM Region | Run a small Fiji/Suva-area test region before lunch to verify the environment and generate a shared workshop output. |
| 11:30–1:00 | Lunch | Lunch offsite. |
| 1:00–2:00 | Check DEM Outputs | Review the generated DEM outputs, including file outputs, metadata, raster properties, and quick visual inspection products. |
| 2:00–2:50 | Deconstruct the YAML Recipe | Walk through the YAML recipe used to generate the test DEM, including project settings, region, source modules, hooks, weights, stacking outputs, gridding/interpolation steps, and final outputs. |
| 2:50–3:00 | Day 1 Wrap-Up | Review what worked, what broke, and what questions should carry into Day 2. |

---

## Day 2: Workflow Modules, Data Filtering, and Custom Region Exercise

Day 2 builds on the Fiji test DEM and YAML walkthrough from Day 1. Now that participants have seen how a complete DEM recipe is structured, we will step through the main workflow tools more directly and then apply the same concepts to a custom coastal region.

The goal is to understand how the CUDEM workflow components fit together. `fetchez` is used for data discovery, source-data access, and source-level preprocessing. `transformez` focuses on vertical datum harmonization. `globato` connects these pieces into a reproducible recipe-based workflow for building composite coastal DEMs.

By the end of Day 2, participants should understand how to inspect source modules, interpret YAML recipe structure, think through vertical harmonization, run or modify a `globato` recipe, and inspect the resulting DEM outputs.

### Day 2 Agenda

| Time | Topic | Description |
|---|---|---|
| 9:00–9:15 | Day 1 Review and Day 2 Goals | Review the Fiji test DEM workflow from Day 1 and introduce the Day 2 focus: understanding the tools behind the recipe and adapting the workflow to another region. |
| 9:15–10:00 | Data Discovery with `fetchez` | Explore source-data modules, module arguments, simple data download patterns, hooks, streams, and recipe formatting. The goal is to understand how source datasets are represented before they are synthesized by the full CUDEM workflow. |
| 10:00–10:45 | Vertical Harmonization with `transformez` | Review the role of vertical datums and reference frames in coastal DEM development. Introduce how `transformez` supports vertical harmonization and shift-grid workflows. |
| 10:45–11:00 | Break / Troubleshooting | Short break and time for troubleshooting. |
| 11:00–11:30 | Workflow Synthesis with `globato` | Review how `globato` connects source modules, hooks, transformations, stacking, interpolation, and outputs into a reproducible CUDEM workflow. Introduce module bundles such as `global-bathy-topo`. |
| 11:30–1:00 | Lunch | Lunch offsite. |
| 1:00–1:40 | Data Filtering and Source QC | Review source filtering concepts using point-cloud visualization, `rq` filtering, source masks, provenance outputs, and stacked intermediate products. Discuss how these outputs help diagnose source coverage and DEM quality. |
| 1:40–2:30 | Custom Region Exercise | Choose a small coastal region and run a custom `globato` workflow. Export or inspect the YAML recipe, run the workflow, and generate basic output products. |
| 2:30–2:50 | Output Review and Discussion | Inspect DEM metadata, hillshade outputs, source masks, provenance, and stacked rasters. Discuss what looks reasonable, what needs closer inspection, and how the workflow could be improved with local data. |
| 2:50–3:00 | Workshop Wrap-Up | Review the full CUDEM workflow sequence, summarize key takeaways, and discuss next steps for adapting the workflow to participant regions. |
