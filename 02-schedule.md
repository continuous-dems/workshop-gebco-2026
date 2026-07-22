# Schedule

## Day 1: Onboarding, Test DEM Generation, and Workflow Deconstruction

Day 1 focuses on getting everyone set up and comfortable with the basic CUDEM workflow. We will begin by framing coastal DEM development as a reproducible open-source process that connects data discovery, transformation, DEM generation, and validation.

The first major goal is environment setup. We will get everyone into a consistent `cudem` conda environment and run basic checks to confirm that the core tools are working.

After setup, we will run a small test DEM. This gives everyone a concrete output early in the workshop and provides a shared example that we can inspect, troubleshoot, and use throughout the rest of the course.

The afternoon will focus on checking the DEM outputs and deconstructing the YAML workflow used to generate them. We will walk through the source modules, processing hooks, stacking steps, gridding/interpolation settings, and vertical reference assumptions so participants can see how a single recipe organizes the full DEM-building process.

By the end of Day 1, participants should have a working environment, a generated test DEM, and a basic understanding of the CUDEM framework.

### Day 1 Agenda

| Time | Topic | Description |
|---|---|---|
| 9:00–9:20 AM | Introductions and Workshop Overview ([slides](https://github.com/continuous-dems/workshop-gebco-2026/releases/download/slide-decks-v1/CUDEM_Overview_GEBCO_Scholars_2026.07.20.pptx)) | `fetchez`, `transformez`, `globato`, and `ivert` as parts of one CUDEM workflow. |
| 9:20–10:30 AM | Environment Setup | Set up a consistent `cudem` conda environment |
| 10:30–11:00 AM | Installation Tests and Troubleshooting | Confirm that the main tools are installed and responding: `fetchez`, `transformez`, `globato`, and `ivert` |
| 11:00–11:30 AM | Run the Fiji Test DEM | Run the test region to verify the environment |
| 11:30 AM–1:00 PM | Lunch | offsite |
| 1:00–2:00 PM | Check DEM Outputs | Review the generated DEM outputs |
| 2:00–2:50 PM | Deconstruct the YAML Recipe | Walk through the YAML recipe used to generate the test DEM |
| 2:50–3:00 PM | Day 1 Wrap-Up | Review what worked, what broke, and what questions should carry into Day 2. |

---
## Day 2: Validation, Workflow Modules, and Custom Region Exercise

Day 2 begins with the final stage of the CUDEM workflow: validating the Fiji test DEM generated on Day 1 with `ivert`. We will run the Fiji validation workflow and examine the resulting statistics and plots. You will be able to re-run IVERT at any time when we explore deeper into the other data-discovery and DEM-creation modules.

After the validating the test DEM, we will go through individual module exercises to explore the code environment in more depth.

The afternoon will conclude with a custom DEM exercise. Participants will apply the same concepts to another coastal area, build a YAML recipe, run the workflow, and review the resulting outputs.

By the end of Day 2, participants should understand how to validate a DEM with `ivert`, interpret validation results, work with the main CUDEM workflow tools, and adapt a recipe-based workflow to a new region.

### Day 2 Agenda

| Time | Topic | Description |
|---|---|---|
| 9:00 AM–9:30 AM | Introduction to `ivert` ([slides](https://github.com/continuous-dems/workshop-gebco-2026/releases/download/slide-decks-v1/IVERT_Intro_GEBCO_Scholars_2026.07.21.pptx)) | Review the purpose of independent DEM validation, required inputs, and expected outputs. |
| 9:30 AM–10:20 AM | Fiji `ivert` Validation | Run the Fiji test DEM through the `ivert` validation workflow. Filter results. Learn how to set up custom validations in ivert. |
| 10:20 AM–10:30 AM | Take a Break | Stretch legs, use the facilities. |
| 10:30–11:00 AM | Data Discovery with `fetchez` | Review source modules, arguments, hooks, streams, and source-level preprocessing. |
| 11:00–11:30 PM | Vertical Harmonization with `transformez` | Review vertical datums, reference frames, harmonization, and shift-grid workflows. |
| 11:30 AM–1:00 PM | Lunch | offsite |
| 1:00–2:00 PM | Workflow Synthesis with `globato` | Review how source modules, transformations, stacking, interpolation, and outputs are connected. |
| 2:00-2:10 PM | Take a Break | Stretch legs, use the facilities. |
| 2:10–2:50 PM | Custom Region Exercise | Inspect or modify a YAML recipe, run a workflow for another coastal region, and review the outputs. |
| 2:50–3:00 PM | Workshop Wrap-Up | Review the complete CUDEM workflow and discuss next steps. |
